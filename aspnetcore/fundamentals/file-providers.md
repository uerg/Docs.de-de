---
title: Datei-Anbieter in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core Dateisystemzugriff durch die Verwendung von Datei Anbieter abstrahiert.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a>Datei-Anbieter in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrahiert Dateisystemzugriff durch die Verwendung von Datei-Anbieter.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Datei Anbieter Abstraktionen

Anbieter der Datei sind eine Abstraktion über Dateisysteme. Ist die Hauptschnittstelle `IFileProvider`. `IFileProvider`Macht Methoden zum Abrufen von Informationen (`IFileInfo`), Verzeichnisinformationen (`IDirectoryContents`), und zum Einrichten von Benachrichtigungen (mithilfe einer `IChangeToken`).

`IFileInfo`Stellt Methoden und Eigenschaften für einzelne Dateien oder Verzeichnisse. Sie verfügt über zwei boolesche Eigenschaften, `Exists` und `IsDirectory`, sowie Eigenschaften, die der Datei beschreibt `Name`, `Length` (in Bytes), und `LastModified` Datum. Erfahren Sie aus der Datei mithilfe der `CreateReadStream` Methode.

## <a name="file-provider-implementations"></a>Datei-anbieterimplementierungen

Drei Implementierungen des `IFileProvider` sind verfügbar: physische, eingebettet und zusammengesetzte. Der physische Anbieter wird verwendet, auf die tatsächlichen Systemdateien zugreifen. Der eingebettete Anbieter wird verwendet, um den Zugriff auf Dateien, die in Assemblys eingebettet. Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnissen von anderen Anbietern bereitzustellen.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

Die `PhysicalFileProvider` ermöglicht den Zugriff auf dem physischen Dateisystem. Es dient als Wrapper für die `System.IO.File` Typ (für den physischen Anbieter), alle Pfade zu einem Verzeichnis und seinen untergeordneten Elementen Bereichsauswahl. Diese Bereichsdefinition schränkt den Zugriff auf einem bestimmten Verzeichnis und seinen untergeordneten Elementen, die im Dateisystem außerhalb dieser Grenze den Zugriff zu verhindern. Dieser Anbieter zu instanziieren, müssen Sie es mit einem Verzeichnispfad bereit dient als der Basispfad für alle Anforderungen an diesen Anbieter vorgenommen wurden (und die schränkt den Zugriff außerhalb dieser Pfad). In einer app ASP.NET Core instanziieren Sie eine `PhysicalFileProvider` Anbieter direkt, oder Sie können anfordern, ein `IFileProvider` in einem Controller oder des Diensts-Konstruktor über [Abhängigkeitsinjektion](dependency-injection.md). Der zweite Ansatz ergibt in der Regel eine Lösung flexible und getestet werden können.

Das folgende Beispiel zeigt, wie Sie erstellen eine `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Sie können der Verzeichnisinhalt durchlaufen oder Abrufen von Informationen für eine bestimmte Datei durch die Bereitstellung ein Unterpfad.

Um einen Anbieter von einem Controller anzufordern, geben Sie es im Konstruktor des Controllers aus, und weisen sie Sie ein lokales Feld. Verwenden Sie die lokale Instanz von Ihrem Aktionsmethoden:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Erstellen Sie dann den Anbieter in der app `Startup` Klasse:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

In der *Index.cshtml* anzeigen, durchlaufen die `IDirectoryContents` bereitgestellt:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Das Ergebnis:

![Datei Anbieter Beispiel Anwendung Angebot physischen Dateien und Ordner](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

Die `EmbeddedFileProvider` wird verwendet, um den Zugriff auf Dateien, die in Assemblys eingebettet. In .NET Core, betten Sie Dateien in eine Assembly mit dem `<EmbeddedResource>` Element in der *csproj* Datei:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Sie können [Globmodus Muster](#globbing-patterns) beim Festlegen der Dateien in der Assembly eingebettet werden sollen. Diese Muster können verwendet werden, eine oder mehrere Dateien übereinstimmen.

> [!NOTE]
> Es ist unwahrscheinlich, dass Sie jemals tatsächlich jede JS-Datei in Ihr Projekt in seiner Assembly einbetten möchten. Im obigen Beispiel ist nur zu Demonstrationszwecken.

Beim Erstellen einer `EmbeddedFileProvider`, übergeben Sie die Assembly, die sie an ihren Konstruktor gelesen wird.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Der obige Codeausschnitt veranschaulicht, wie ein `EmbeddedFileProvider` mit Zugriff auf den gerade ausgeführten Assembly.

Aktualisieren die Beispielapp für die Verwendung einer `EmbeddedFileProvider` ergibt sich die folgende Ausgabe:

![Datei-Anbieter-beispielanwendung, die eingebettete Dateien auflisten](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Eingebettete Ressourcen nicht Verzeichnissen verfügbar machen. Stattdessen wird der Pfad auf die Ressource (über dessen Namespace) eingebettet, in einer Filename mithilfe `.` Trennzeichen.

> [!TIP]
> Die `EmbeddedFileProvider` Konstruktor akzeptiert einen optionalen `baseNamespace` Parameter. Durch diese Angabe wird die Aufrufe ausgeweitet `GetDirectoryContents` auf diese Ressourcen unter dem angegebenen Namespace.

### <a name="compositefileprovider"></a>CompositeFileProvider

Die `CompositeFileProvider` kombiniert `IFileProvider` Instanzen, die eine einzelne Schnittstelle zum Arbeiten mit Dateien, die von mehreren Anbietern verfügbar macht. Beim Erstellen der `CompositeFileProvider`, Sie übergeben eine oder mehrere `IFileProvider` Instanzen an ihren Konstruktor:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Aktualisieren die Beispielapp für die Verwendung einer `CompositeFileProvider` , dass schließt sowohl die physischen und eingebettete Datenanbieter zuvor konfigurierten, ergibt sich die folgende Ausgabe:

![Datei-Anbieter-beispielanwendung, die sowohl für physische Dateien und eingebettete Dateien und Ordner auflisten](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Überwachen von Änderungen

Die `IFileProvider` `Watch` Methode bietet eine Möglichkeit, eine oder mehrere Dateien oder Verzeichnisse auf Änderungen überwacht. Diese Methode akzeptiert eine Pfadzeichenfolge, dem können [Globmodus Muster](#globbing-patterns) an mehrere Dateien und gibt ein `IChangeToken`. Dieses Token stellt eine `HasChanged` -Eigenschaft, die überprüft werden kann, und eine `RegisterChangeCallback` Methode, die aufgerufen wird, wenn Änderungen an der angegebenen Pfadzeichenfolge erkannt werden. Beachten Sie, dass jede Änderungstoken zugeordneten Rückrufs nur als Antwort auf eine einzelne Änderung aufruft. Um die Konstante Überwachung zu aktivieren, können Sie eine `TaskCompletionSource` wie unten dargestellt oder Neuerstellen `IChangeToken` Instanzen als Reaktion auf Änderungen.

In diesem Artikel Beispiel konfiguriert eine Konsolenanwendung wird eine Meldung angezeigt, bei einer Änderung eine Textdatei:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Das Ergebnis nach dem Speichern der Datei mehrmals:

![Befehlsfenster nach Dotnet ausführen Ausführen, zeigt die Anwendung, die Überwachung der quotes.txt-Datei für die Änderungen, die die Datei verfügt über fünf Mal geändert.](file-providers/_static/watch-console.png)

> [!NOTE]
> Einige Dateisysteme, z. B. Docker-Containern und Netzwerkfreigaben, möglicherweise nicht zuverlässig Benachrichtigungen gesendet werden. Legen Sie die `DOTNET_USE_POLLINGFILEWATCHER` Umgebungsvariablen `1` oder `true` im Dateisystem nach Änderungen alle 4 Sekunden abruft.

## <a name="globbing-patterns"></a>Globmodus-Muster

Pfade des Dateisystems verwenden Platzhaltermustern aufgerufen *Globmodus Muster*. Diese einfache Muster können verwendet werden, um Gruppen von Dateien anzugeben. Die zwei Platzhalterzeichen sind `*` und `**`.

**`*`**

   Entspricht allem am aktuellen Ordnerebene oder einen Dateinamen oder eine Dateierweiterung. Übereinstimmungen werden durch beendet `/` und `.` Zeichen im Dateipfad.

<strong><code>**</code></strong>

   Entspricht allem über mehrere Verzeichnisebenen. Kann verwendet werden, um rekursiv viele Dateien innerhalb einer Verzeichnishierarchie entsprechen.

### <a name="globbing-pattern-examples"></a>Beispiele für Globmodus-Muster

**`directory/file.txt`**

   Entspricht eine bestimmte Datei in einem bestimmten Verzeichnis an.

**<code>directory/*.txt</code>**

   Entspricht allen Dateien mit `.txt` Erweiterung in einem bestimmten Verzeichnis.

**`directory/*/bower.json`**

   Entspricht allen `bower.json` Dateien in Verzeichnissen genau nächsttieferen Ebene aus der `directory` Verzeichnis.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Entspricht allen Dateien mit `.txt` Erweiterung finden Sie eine beliebige Stelle unter der `directory` Verzeichnis.

## <a name="file-provider-usage-in-aspnet-core"></a>Anbieter Dateiverwendung in ASP.NET Core

Einige Teile der ASP.NET Core nutzen Datei Anbieter. `IHostingEnvironment`macht die app Inhalts-Stamm und Web-Stammverzeichnis als `IFileProvider` Typen. Die Middleware für statische Dateien verwendet Datei Anbieter, um statische Dateien zu suchen. Razor nutzt intensiv `IFileProvider` in Suchen von Ansichten. Der Dotnet veröffentlichen Funktionalität verwendet Datei Anbieter und Globmodus-Muster, um anzugeben, welche Dateien veröffentlicht werden sollen.

## <a name="recommendations-for-use-in-apps"></a>Empfehlungen für apps

Wenn Ihre app ASP.NET Core Dateisystemzugriff erforderlich ist, können Sie anfordern, dass eine Instanz von `IFileProvider` über Abhängigkeitsinjektion, und verwenden Sie die Methoden für den Zugriff, wie in diesem Beispiel gezeigt. Dadurch können Sie zum Konfigurieren des Anbieters einmal, wenn die Anwendung wird gestartet, und die Anzahl der von Implementierungstypen reduziert, die Ihre app instanziiert wird.
