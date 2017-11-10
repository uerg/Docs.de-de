---
title: Arbeiten mit statischen Dateien in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Arbeiten mit statischen Dateien in ASP.NET Core.
keywords: ASP.NET Core, statische Dateien, statische Assets, HTML, CSS und JavaScript
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40c9a799c6ac8a2ce712df4b8fbf3c142ef3fd82
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a>Arbeiten mit statischen Dateien in ASP.NET Core

<a name="fundamentals-static-files"></a>

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Statische Dateien, z. B. HTML, CSS, Bild und JavaScript, sind Ressourcen, die eine ASP.NET Core app direkt an Clients fungieren kann.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>Bereitstellen statischer Dateien

Statische Dateien befinden sich in der Regel der `web root` (*\<Content-Stamm > / "Wwwroot"*) Ordner. Finden Sie unter [Content Stamm](xref:fundamentals/index#content-root) und [Stammweb](xref:fundamentals/index#web-root) für Weitere Informationen. In der Regel legen Sie Inhalt Stammverzeichnis der aktuellen Verzeichnis zu sein, damit Ihres Projekts `web root` wird in der Entwicklung gefunden werden.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Statische Dateien gespeichert werden können, in einem beliebigen Ordner unter dem `web root` und mit einem relativen Pfad mit dem Stamm zugegriffen. Beispielsweise, wenn Sie einen Standard-Webanwendungsprojekt mithilfe von Visual Studio erstellen, sind mehrere Ordner erstellt, die innerhalb der *"Wwwroot"* Ordner - *Css*, *Bilder*, und *Js*. Der URI ein Bild im Zugriff auf die *Bilder* Unterordner:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Damit kann für statische Dateien verarbeitet werden, müssen Sie konfigurieren die [Middleware](middleware.md) um statische Dateien an die Pipeline hinzuzufügen. Die Middleware für statische Dateien konfiguriert werden kann, durch Hinzufügen einer Abhängigkeit auf der *Microsoft.AspNetCore.StaticFiles* Paket in Ihr Projekt aufrufen und anschließend die `UseStaticFiles` Erweiterungsmethode über `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`Stellt die Dateien im `web root` (*"Wwwroot"* standardmäßig) servable. Später zeige ich wie andere Verzeichnisinhalt mit servable `UseStaticFiles`.

Sie müssen das NuGet-Paket "Microsoft.AspNetCore.StaticFiles" einschließen.

> [!NOTE]
> `web root`wird standardmäßig auf die *"Wwwroot"* Directory, aber Sie können festlegen, die `web root` Verzeichnis mit `UseWebRoot`.

Angenommen, Sie eine Projekthierarchie haben, in dem Sie dienen möchten auf statischen Dateien sich außerhalb befinden, der `web root`. Zum Beispiel:

* wwwroot
  * CSS
  * Bilder
  * ...
* MyStaticFiles
  * Test.PNG

Für eine dienstanforderung zum Zugreifen auf *test.png*, konfigurieren Sie die Middleware für statische Dateien wie folgt:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Eine Anforderung zum `http://<app>/StaticFiles/test.png` dient der *test.png* Datei.

`StaticFileOptions()`Antwort-Header kann festgelegt werden. Der folgende Code z. B. richtet aus Bereitstellung statischer Dateien der *"Wwwroot"* Ordner und legt die `Cache-Control` Header zum sie 10 Minuten (600 Sekunden) öffentlich zwischengespeichert werden:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Antwort-Header, die mit der Cache-Control-Header wurde hinzugefügt](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statische Autorisierung

Das Modul für statische Dateien bietet **keine** autorisierungsüberprüfungen vorgenommen. Alle Dateien von bedient, u. a. die *"Wwwroot"* öffentlich verfügbar sind. Um Dateien anhand der Autorisierung zu dienen:

* Speichern Sie sie außerhalb von *"Wwwroot"* und einem beliebigen Verzeichnis zugegriffen werden kann, um die Middleware für statische Dateien **und**

* Dienen sie über eine Controlleraktion Zurückgeben einer `FileResult` Autorisierung wird angewendet

## <a name="enabling-directory-browsing"></a>Aktivieren die Verzeichnissuche

Verzeichnissuche ermöglicht dem Benutzer für Ihre Web-app, um eine Liste von Verzeichnissen und Dateien in einem angegebenen Verzeichnis anzuzeigen. Verzeichnissuche ist aus Sicherheitsgründen standardmäßig deaktiviert (siehe [Überlegungen](#considerations)). Um die Verzeichnissuche zu aktivieren, rufen die `UseDirectoryBrowser` Erweiterungsmethode über `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

Und fügen Sie die erforderlichen Dienste durch Aufrufen von `AddDirectoryBrowser` Erweiterungsmethode über `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Der obige Code ermöglicht, Verzeichnissuche, der die *"Wwwroot" / Bilder* Ordner mit die URL http://\<app > / MyImages, mit Links zu einzelnen Dateien und Ordner:

![Verzeichnissuche](static-files/_static/dir-browse.png)

Finden Sie unter [Überlegungen](#considerations) auf die Sicherheitsrisiken beim Durchsuchen aktivieren.

Beachten Sie die beiden `app.UseStaticFiles` aufrufen. Die erste Vorlage ist erforderlich, um CSS, Bilder und JavaScript in dienen der *"Wwwroot"* Ordners und der zweite Aufruf für die Verzeichnissuche, der die *"Wwwroot"-Images* Ordner mit die URL http://\<app > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Ein Standarddokument bedient

Festlegen einer Standardstartseite kann Besucher der Website eines volumenprogramms starten, wenn Ihre Site besuchen. Rufen Sie in der Reihenfolge für Ihre Web-app, ohne dass der Benutzer den URI vollqualifiziert eine Standardseite dient, die `UseDefaultFiles` Erweiterungsmethode über `Startup.Configure` wie folgt.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`muss aufgerufen werden, bevor `UseStaticFiles` Standarddatei dienen. `UseDefaultFiles`ist ein erneute URL-Writer, der tatsächlich die Datei dienen. Sie müssen die Middleware für statische Dateien aktivieren (`UseStaticFiles`), die die Datei dient.

Mit `UseDefaultFiles`, Anforderungen an einen Ordner gesucht werden:

* default.htm
* "default.HTML"
* Index.htm
* index.html

Die erste Datei, die aus der Liste gefunden wird, als ob die Anforderung den voll gekennzeichneten URI war (obwohl die Browser-URL angezeigt, die angeforderten URI weiterhin) bedient.

Der folgende Code zeigt, wie so ändern Sie den Standardnamen für die Datei in *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`kombiniert die Funktionalität der `UseStaticFiles`, `UseDefaultFiles`, und `UseDirectoryBrowser`.

Der folgende Code ermöglicht, statische Dateien und die Standarddatei, die bereitgestellt werden, aber keine Verzeichnissuche zulässt:

```csharp
app.UseFileServer();
   ```

Der folgende Code ermöglicht, statische Dateien "," Standarddateien "und" Verzeichnis durchsuchen:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Finden Sie unter [Überlegungen](#considerations) auf die Sicherheitsrisiken beim Durchsuchen aktivieren. Wie bei `UseStaticFiles`, `UseDefaultFiles`, und `UseDirectoryBrowser`, wenn Sie möchten, dass Sie Dateien dienen, die außerhalb der `web root`, instanziieren und konfigurieren eine `FileServerOptions` -Objekt, das Sie als Parameter an übergeben `UseFileServer`. Betrachten Sie z. B. die folgenden Verzeichnishierarchie in der Web-app:

* wwwroot

  * CSS

  * Bilder

  * ...

* MyStaticFiles

  * Test.PNG

  * "default.HTML"

Verwenden die oben genannten hierarchiebeispiel, möglicherweise möchten statische Dateien, Standarddateien und Durchsuchen ermöglichen die `MyStaticFiles` Verzeichnis. Im folgenden Codeausschnitt erfolgt, die mit einem einzigen Aufruf `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Wenn `enableDirectoryBrowsing` festgelegt ist, um `true` aufrufen, müssen Sie `AddDirectoryBrowser` Erweiterungsmethode über `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Verwenden die Dateihierarchie und oben angegeben Codes:

| URI            |                             Antwort  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Wenn keine Dateien standardmäßig in sind die *MyStaticFiles* Directory, http://\<app > / StaticFiles returns die Verzeichnisliste mit klickbaren Links:

![Liste der statischen Dateien](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`und `UseDirectoryBrowser` dauert die Url http://\<app > / StaticFiles ohne nachstehenden Schrägstrich und Ursache eine clientseitige Umleitung zu http://\<app > /StaticFiles/ (das Hinzufügen des nachstehenden Schrägstrich). Ohne die nachgestellten Schrägstrich relativen URLs in den Dokumenten würden falsch sein.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

Die `FileExtensionContentTypeProvider` Klasse enthält eine Auflistung, die MIME-Inhaltstypen Dateierweiterungen zugeordnet. Im folgenden Beispiel wird mehrere Dateierweiterungen bekannten MIME-Typen registriert sind, ersetzt das ".rtf" und "mp4" wird entfernt.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Finden Sie unter [MIME-Inhaltstypen](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Nicht standardmäßige Inhaltstypen

Die ASP.NET-Middleware für statische Dateien erkennt fast 400 bekannten Dateiinhaltstypen. Wenn der Benutzer eine Datei mit einer unbekannten Dateityp anfordert, gibt die Middleware für statische Dateien Antworten HTTP 404 (nicht gefunden) zurück. Wenn Verzeichnissuche aktiviert ist, wird ein Link zur Datei angezeigt werden, aber die URI einen HTTP 404-Fehler zurück.

Der folgende Code ermöglicht bedient unbekannte Typen und die unbekannte Datei als Bild rendert.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Mit dem Code wird eine Anforderung für eine Datei mit einem unbekannten Inhaltstyp als Bild zurückgegeben.

>[!WARNING]
> Aktivieren der `ServeUnknownFileTypes` stellt ein Sicherheitsrisiko, und verwenden, wird abgeraten.  `FileExtensionContentTypeProvider`(oben erläuterten) bietet eine sicherere Alternative zum Bereitstellen von Dateien mit standarderweiterungen.

### <a name="considerations"></a>Weitere Überlegungen

>[!WARNING]
> `UseDirectoryBrowser`und `UseStaticFiles` kann Offenlegung von geheimen Schlüsseln. Es wird empfohlen, die Sie **nicht** Enable Verzeichnissuche in der Produktion. Achten Sie darauf über die Verzeichnisse mit aktivieren `UseStaticFiles` oder `UseDirectoryBrowser` wie das gesamte Verzeichnis und alle Unterverzeichnisse zugegriffen werden können. Es wird empfohlen, öffentliche Inhalte wie z. B. im Verzeichnis eigenen beibehalten  *\<Inhalts-Stamm > / "Wwwroot"*, Weg von Sichten, Konfigurationsdateien usw.

* Die URLs für Inhalte, die verfügbar gemacht, mit `UseDirectoryBrowser` und `UseStaticFiles` unterliegen die Groß-/Kleinschreibung und zeichenbeschränkungen von ihren zugrunde liegenden Dateisystem. Z. B. Windows wird Groß-/Kleinschreibung nicht beachtet, aber Mac und Linux befinden sich nicht.

* ASP.NET Core-Anwendungen, die in IIS gehostete verwenden ASP.NET Core-Modul, um alle Anforderungen an die Anwendung dies umfasst auch Anforderungen für statische Dateien weitergeleitet. IIS Handler für statische Dateien wird nicht verwendet werden, weil er nicht abrufen, eine Möglichkeit zum Verarbeiten von Anforderungen, bevor sie das ASP.NET Core-Modul verarbeitet werden.

* So entfernen Sie die IIS-Handler für statische Dateien (auf dem Server oder Website-Ebene):

     * Navigieren Sie zu der **Module** Funktion

     * Wählen Sie **StaticFileModule** in der Liste

     * Tippen Sie auf **entfernen** in der **Aktionen** Randleiste

>[!WARNING]
> Wenn die IIS-Handler für statische Dateien aktiviert ist **und** der ASP.NET Core-Modul (ANCM) ist nicht richtig konfiguriert (z. B. wenn *"Web.config"* wurde nicht bereitgestellt), werden statische Dateien verarbeitet werden.

* Codedateien (z. B. c# und Razor) sollte außerhalb der app-Projekt `web root` (*"Wwwroot"* standardmäßig). Dies erstellt eine saubere Trennung zwischen den Inhalt Ihrer app Client Side und serverseitigen Source Code, wodurch verhindert wird, dass serverseitigen Code zugreifen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Middleware](middleware.md)

* [Einführung in ASP.NET Core](../index.md)
