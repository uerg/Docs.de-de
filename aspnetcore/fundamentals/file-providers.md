---
title: Dateianbieter in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core Dateisystemzugriff durch die Verwendung von Dateianbietern abstrahiert.
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276238"
---
# <a name="file-providers-in-aspnet-core"></a>Dateianbieter in ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrahiert Dateisystemzugriff durch die Verwendung von Dateianbietern.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Dateianbieterabstraktionen

Dateianbieter sind eine Abstraktion über Dateisysteme. Die Hauptschnittstelle ist `IFileProvider`. `IFileProvider` macht Methoden zum Abrufen von Informationen (`IFileInfo`), Verzeichnisinformationen (`IDirectoryContents`) und zum Einrichten von Benachrichtigungen (mithilfe eines `IChangeToken`) verfügbar.

`IFileInfo` stellt Methoden und Eigenschaften einzelner Dateien oder Verzeichnisse zur Verfügung. Sie verfügt über zwei boolesche Eigenschaften, `Exists` und `IsDirectory` sowie über Eigenschaften, die `Name`, `Length` (in Bytes) und das `LastModified`-Datum der Datei beschreiben. Sie können die Datei mithilfe der `CreateReadStream`-Methode lesen.

## <a name="file-provider-implementations"></a>Dateianbieterimplementierungen

Es sind drei Implementierungen von `IFileProvider` verfügbar: physische, eingebettete und zusammengesetzte. Der physische Anbieter wird verwendet, um auf die tatsächlichen Systemdateien zuzugreifen. Der eingebettete Anbieter wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind. Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnisse von einem oder mehreren anderen Anbietern bereitzustellen.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

Der `PhysicalFileProvider` ermöglicht den Zugriff auf das physische Dateisystem. Er dient als Wrapper für den `System.IO.File`-Typ (für den physischen Anbieter). Alle Pfade werden einem Verzeichnis und seinen untergeordneten Elementen zugeordnet. Diese Bereichsdefinition schränkt den Zugriff auf ein bestimmtes Verzeichnis und die untergeordneten Elemente ein, wodurch der Zugriff auf Dateisysteme außerhalb dieses Bereichs verhindert wird. Beim Instanziieren dieses Anbieters müssen Sie einen Verzeichnispfad zur Verfügung stellen, der als Basispfad für alle Anforderungen an diesen Anbieter dient (und den Zugriff außerhalb dieses Pfads beschränkt). In einer ASP.NET Core-Anwendung können Sie einen `PhysicalFileProvider`-Anbieter direkt instanziieren oder einen `IFileProvider` in einem Controller oder Dienstkonstruktor über [Dependeny Injection](dependency-injection.md) anfordern. Der letzte Ansatz ergibt in der Regel eine flexiblere und testbare Lösung.

Das folgende Beispiel zeigt, wie Sie einen `PhysicalFileProvider` erstellen.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Sie können die Verzeichnisinhalte durchlaufen oder die Informationen für eine bestimmte Datei durch die Bereitstellung eines Unterpfads abrufen.

Zur Anbieteranforderung von einem Controller müssen Sie ihn im Konstruktor des Controllers angeben und einem lokalen Feld zuweisen. Verwenden Sie die lokale Instanz aus Ihren Aktionsmethoden:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Erstellen Sie dann den Anbieter in der `Startup`-Anwendungsklasse:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

Durchlaufen Sie in der *Index.cshtml*-Ansicht die bereitgestellten `IDirectoryContents`:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Das Ergebnis:

![Die Beispielanwendung des Dateianbieters mit physischen Dateien und Ordnern](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

Der `EmbeddedFileProvider` wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind. Betten Sie Ihre Dateien in .NET Core mit dem `<EmbeddedResource>`-Element in der *CSPROJ*-Datei in eine Assembly ein:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Sie können [Globmuster](#globbing-patterns) verwenden, wenn Sie Dateien für die Einbettung in der Assembly angeben. Diese Muster können verwendet werden, um eine oder mehrere Dateien zuzuordnen.

> [!NOTE]
> Es ist unwahrscheinlich, dass Sie jemals tatsächlich jede JS-Datei in Ihrem Projekt in die Assembly einbetten möchten. Das obige Beispiel dient nur zu Demonstrationszwecken.

Beim Erstellen eines `EmbeddedFileProvider` übergeben Sie die Assembly, die für ihren Konstruktor gelesen wird.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Der obige Codeausschnitt veranschaulicht die Erstellung eines `EmbeddedFileProvider` mit Zugriff auf die gerade ausgeführte Assembly.

Das Aktualisieren der Beispielanwendung für die Verwendung eines `EmbeddedFileProvider` ergibt die folgende Ausgabe:

![Die Beispielanwendung des Dateianbieters mit eingebetteten Dateien](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Eingebettete Ressourcen machen keine Verzeichnisse verfügbar. Stattdessen wird der Ressourcenpfad (über dessen Namespace) mithilfe von `.`-Trennzeichen in ihren Dateinamen eingebettet.

> [!TIP]
> Der `EmbeddedFileProvider`-Konstruktor akzeptiert einen optionalen `baseNamespace`-Parameter. Durch diese Angabe werden Aufrufe von `GetDirectoryContents` auf diese Ressourcen unter dem angegebenen Namespace festgelegt.

### <a name="compositefileprovider"></a>CompositeFileProvider

Der `CompositeFileProvider` kombiniert `IFileProvider`-Instanzen, die eine einzelne Schnittstelle zum Arbeiten mit Dateien von mehreren Anbietern verfügbar machen. Beim Erstellen des `CompositeFileProvider` übergeben Sie eine oder mehrere `IFileProvider`-Instanzen an seinen Konstruktor:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Aktualisieren die Beispielanwendung für die Verwendung eines `CompositeFileProvider`. Hier sind zuvor konfigurierte physische und eingebettete Anbieter enthalten. Daraus ergibt sich die folgende Ausgabe:

![Die Beispielanwendung des Dateianbieters mit physischen Dateien und Ordnern sowie eingebetteten Dateien](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Erkennen von Änderungen

Die `IFileProvider` `Watch`-Methode bietet eine Möglichkeit, eine oder mehrere Dateien oder Verzeichnisse auf Änderungen zu überwachen. Diese Methode akzeptiert eine Pfadzeichenfolge, die [Globmuster](#globbing-patterns) verwenden kann, um mehrere Dateien anzugeben und ein `IChangeToken` zurückzugeben. Dieses Token stellt eine `HasChanged`-Eigenschaft zur Verfügung, die überprüft werden kann, und eine `RegisterChangeCallback`-Methode, die aufgerufen wird, wenn Änderungen der angegebenen Pfadzeichenfolge erkannt werden. Beachten Sie, dass jedes Änderungstoken nur seine zugeordneten Rückrufe als Antwort auf eine einzelne Änderung aufruft. Zur Aktivierung der konstanten Überwachung können Sie wie unten dargestellt eine `TaskCompletionSource` verwenden oder `IChangeToken`-Instanzen als Reaktion auf Änderungen neu erstellen.

Im Beispiel dieses Artikels wird eine Konsolenanwendung konfiguriert, damit sie bei einer Änderung einer Textdatei eine Meldung anzeigt:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Das Ergebnis nach mehrmaligem Speichern der Datei:

![Befehlsfenster nach der Ausführung von .NET zeigt die Anwendung, die nach Änderungen in der quotes.txt-Datei Ausschau hält, und dass die Datei fünf Mal geändert wurde.](file-providers/_static/watch-console.png)

> [!NOTE]
> Einige Dateisysteme, z.B. Docker-Container und Netzwerkfreigaben, senden Änderungsmeldungen möglicherweise nicht zuverlässig . Legen Sie die `DOTNET_USE_POLLINGFILEWATCHER`-Umgebungsvariable auf `1` oder `true` fest, um das Dateisystem alle 4 Sekunden nach Änderungen zu untersuchen.

## <a name="globbing-patterns"></a>Globmuster

Dateisystempfade verwenden Platzhaltermuster namens *Globmuster*. Diese einfachen Muster können verwendet werden, um Dateigruppen anzugeben. Die zwei Platzhalterzeichen sind `*` und `**`.

**`*`**

   Überprüft alles auf der aktuellen Ordnerebene, jeden Dateinamen und jede Dateierweiterung. Übereinstimmungen werden durch `/`- und `.`-Zeichen im Dateipfad beendet.

<strong><code>**</code></strong>

   Überprüft alles auf mehrere Verzeichnisebenen. Kann verwendet werden, um rekursiv eine Übereinstimmung für viele Dateien innerhalb einer Verzeichnishierarchie zu finden.

### <a name="globbing-pattern-examples"></a>Beispiele für Globmuster

**`directory/file.txt`**

   Entspricht einer bestimmten Datei in einem bestimmten Verzeichnis.

**<code>directory/*.txt</code>**

   Entspricht allen Dateien mit `.txt`-Erweiterung in einem bestimmten Verzeichnis.

**`directory/*/bower.json`**

   Entspricht allen `bower.json`-Dateien in Verzeichnissen auf der nächsttieferen Ebene des `directory`-Verzeichnisses.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Entspricht allen Dateien mit `.txt`-Erweiterung, die an einer beliebigen Stelle unter dem `directory`-Verzeichnis gefunden werden.

## <a name="file-provider-usage-in-aspnet-core"></a>Verwenden des Dateianbieters in ASP.NET Core

Einige Komponenten von ASP.NET Core nutzen Dateianbieter. `IHostingEnvironment` stellt das Inhaltsstammverzeichnis und das Webstammverzeichnis der Anwendung als `IFileProvider`-Typen zur Verfügung. Die Middleware der statischen Dateien verwendet Dateianbieter, um statische Dateien zu suchen. Razor nutzt `IFileProvider` intensiv für die Ansichtssuche. Die Veröffentlichungsfunktion von .NET verwendet Dateianbieter und Globmuster, um anzugeben, welche Dateien veröffentlicht werden sollen.

## <a name="recommendations-for-use-in-apps"></a>Verwendungsempfehlungen in Anwendungen

Wenn Ihre ASP.NET Core-Anwendung Dateisystemzugriff erfordert, können Sie über Dependeny Injection eine `IFileProvider`-Instanz anfordern und anschließend die Methoden für den Zugriff verwenden, wie in diesem Beispiel gezeigt. Somit können Sie den Anbieter beim Start der Anwendung einmal konfigurieren und die Anzahl der Implementierungstypen, die von Ihrer Anwendung instanziiert werden, reduzieren.
