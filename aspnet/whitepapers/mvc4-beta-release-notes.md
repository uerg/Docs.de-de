---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Dieses Dokument beschreibt die Version von ASP.NET MVC 4 Beta für Visual Studio 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 4af2df61ab4507b1f100d6bb75777da1168c5a75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Dieses Dokument beschreibt die Version von ASP.NET MVC 4 Beta für Visual Studio 2010.
> 
> > [!NOTE]
> > Dies ist nicht die aktuellste Version. Die Anmerkungen zu dieser Version von ASP.NET MVC 4-RC stehen [hier](mvc4-release-notes.md).


- [Hinweise zur installationsnachbereitung](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Softwareanforderungen](#_Toc303253805)
- [Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4](#_Toc303253806)
- [Neue Funktionen in ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [ASP.NET Single-Page-Anwendung](#_Toc317096198)
    - [Verbesserungen an Standardeinstellung-Projektvorlagen](#_Toc303253808)
    - [Mobile-Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, Ansichtumschaltung und Browser überschreiben](#_Toc303253811)
    - [Know-how für die Codegenerierung in Visual Studio](#_Toc303253812)
    - [Task-Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Bekannte Probleme und aktueller Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

ASP.NET MVC 4 Beta für Visual Studio 2010 installiert werden können, aus der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) mithilfe von Web Platform Installer.

Sie müssen alle zuvor installierten Previews von ASP.NET MVC 4 vor der Installation von ASP.NET MVC 4 Beta deinstallieren.

Diese Version ist nicht mit der .NET Framework 4.5 Developer Preview kompatibel. Sie müssen die .NET 4.5 Developer Preview vor der Installation von ASP.NET MVC 4 Beta deinstallieren.

ASP.NET MVC 4 installiert werden kann und Seite-an-Seite ausführen können mit ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter folgender URL verfügbar:

[https://go.Microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und Weitere Informationen zu ASP.NET MVC sind auf die MVC 4-Seite der ASP.NET-Website verfügbar ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Unterstützung

Dies wird als Vorschauversion verfügbar und wird offiziell nicht unterstützt. Falls Sie Fragen zum Arbeiten mit dieser Version haben, stellen Sie diese zum ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in denen Mitglieder der ASP.NET-Community informelle Unterstützung leisten häufig können sind.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2.0 und Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4

ASP.NET MVC 4 kann parallel zu ASP.NET MVC 3 auf dem gleichen Computer installiert werden die Flexibilität bei der Wahl der Zeitpunkt des Upgrades von einer ASP.NET MVC 3-Anwendung zu ASP.NET MVC 4.

Die einfachste Methode zum Aktualisieren ist, erstellen Sie ein neues ASP.NET MVC 4-Projekt und eine Kopie aller Ansichten, Controller, Code und Inhalt Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt, und klicken Sie dann auf Aktualisieren Sie die Assembly verweist, in das neue Projekt mit das alte Projekt übereinstimmen. Wenn Sie Änderungen an der Datei "Web.config" in der MVC 3-Projekts vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web.config" in der MVC 4-Projekt zusammenführen.

Um eine vorhandene ASP.NET MVC 3-Anwendung auf Version 4 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Ersetzen Sie in alle Web.config-Dateien im Projekt (es ist eine im Stammverzeichnis des Projekts, in den Ordner Views, und im Ordner "Ansichten" für die einzelnen Bereiche im Projekt vorhanden), jede Instanz von den folgenden Text ein:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. In der Stammdatei "Web.config" Aktualisieren der *WebPages:Version* Element "2.0.0.0" und fügen Sie einen neuen *PreserveLoginUrl* Schlüssel, der den Wert "True" aufweist:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Löschen Sie im Projektmappen-Explorer den Verweis auf *System.Web.Mvc* (der verweist auf die Version 3-DLL). Einen Verweis auf Grundlage des domänenwissens *System.Web.Mvc* (v4.0.0.0). Stellen Sie insbesondere die folgenden Änderungen an der Assemblyverweise zu aktualisieren. Nachstehend sind die Details:

    1. Löschen Sie im Projektmappen-Explorer die Verweise auf die folgenden Assemblys: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v. 1.0.0.0)
        - *System.Web.Razor*(v. 1.0.0.0)
        - *System.Web.WebPages.Deployment*(v. 1.0.0.0)
        - *System.Web.WebPages.Razor*(v. 1.0.0.0)
    2. Fügen Sie ein Verweise auf die folgenden Assemblys hinzu: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *Projektname*csproj.
5. Suchen Sie die *ProjectTypeGuids* -Element, und Ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} durch {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Die Änderungen zu speichern, schließen Sie die Projektdatei (csproj), die Sie in Bearbeitung hatten, mit der rechten Maustaste des Projekts und wählen Sie dann Projekt erneut laden.
7. Wenn das Projekt alle Bibliotheken von Drittanbietern, die mit früheren Versionen von ASP.NET MVC kompiliert werden verweist, öffnen Sie die Stammdatei "Web.config", und fügen Sie die folgenden drei *BindingRedirect* Elemente unter dem  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Neue Funktionen in ASP.NET MVC 4 Beta

Dieser Abschnitt beschreibt die Funktionen, die eingeführt wurden in der ASP.NET MVC 4 Beta-Version.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 enthält jetzt die ASP.NET Web-API, ein neues Framework zum Erstellen von HTTP-Diensten, die eine Breite Palette von Clients einschließlich Browsern und mobilen Geräten erreichen kann. ASP.NET-Web-API ist auch eine ideale Plattform zum Erstellen von RESTful-Dienste.

ASP.NET Web-API unterstützt die folgenden Funktionen:

- **Moderne HTTP-Programmiermodell:** direkt zugreifen und diese bearbeiten, die HTTP-Anforderungen und Antworten in Ihrer Web-APIs, die mit einer neuen, stark typisierte HTTP-Objektmodell. Dasselbe Programmiermodell und HTTP-Pipeline symmetrisch auf dem Client über den neuen Typ für "HttpClient" verfügbar ist.
- **Vollständige Unterstützung für Routen**: Web-APIs unterstützen jetzt den vollständigen Satz von Route-Funktionen, die Teil der Web-Stapel, einschließlich Routenparameter und Einschränkungen immer wurden. Darüber hinaus weist Zuordnung zum Aktionen vollständige Unterstützung für Konventionen, damit Sie nicht mehr benötigen, Attribute, z. B. [HttpPost] angewendet auf Klassen und Methoden.
- **Inhalts-Aushandlung**: Client und Server zusammenarbeiten, um zu bestimmen, das richtige Format für Daten, die von einer API zurückgegeben wird. Wir geben standardunterstützung für XML-, JSON- und Formate Formular URL-codierte, und Sie können diese Unterstützung erweitern, indem Sie eine eigene Formatierungsprogramme hinzufügen oder sogar bei der Standardstrategie inhaltsaushandlung ersetzt.
- **Modell Bindung und Validierung:** Modellbinder bieten eine einfache Möglichkeit zum Extrahieren von Daten aus verschiedenen Teilen einer HTTP-Anforderung, und konvertieren die Nachrichtenteile in .NET-Objekte der durch die Web-API-Aktionen verwendet werden kann.
- **Filter:** Web-APIs unterstützt jetzt die Filter, einschließlich der bekannten Filter für Faktoren wie der [Authorize]-Attribut. Sie können erstellen und schließen Sie Ihren eigenen Filter für Aktionen, Autorisierung und Behandlung von Ausnahmen.
- **Der abfragekomposition:** durch eine einfache Rückkehr IQueryable&lt;T&gt;, Ihre Web-API unterstützt die Abfrage über den OData URL-Konventionen.
- **Verbessert die Prüfbarkeit von HTTP-Details:** statt HTTP-Details im statischen Kontextobjekte festlegen, können jetzt Web-API-Aktionen mit Instanzen von HTTP-Anforderungsnachricht und HttpResponseMessage arbeiten. Generische Versionen dieser Objekte, die auch zum ermöglichen Ihnen die Zusammenarbeit mit Ihrer benutzerdefinierten Typen zusätzlich zu den HTTP-vorhanden sein.
- **Verbesserte Inversion of Control (IoC) über DependencyResolver:** Web-API verwendet jetzt das von MVCs-Abhängigkeitskonfliktlöser implementiert Locator-Muster zum Abrufen von Instanzen für viele unterschiedliche Funktionen.
- **Code-basierte Konfiguration:** Web-API-Konfiguration erfolgt ausschließlich über Code, verlassen der Config Dateien zu bereinigen.
- **Selbsthosting:** Web-APIs können in Ihren eigenen Prozess zusätzlich zu den IIS gehostet werden, wobei dennoch die volle Leistung von Routen und anderen Features der Web-API.

Weitere Informationen zu ASP.NET Web-API finden Sie hier [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET Single-Page-Anwendung

ASP.NET MVC 4 enthält nun eine frühe Vorschau der die Umgebung zum Erstellen von einseitige Anwendungen mit erheblichen clientseitige Interaktion mit JavaScript und Web-APIs. Diese Unterstützung umfasst Folgendes:

- Ein Satz von JavaScript-Bibliotheken für umfangreichere lokale Interaktionen mit zwischengespeicherten Daten
- Zusätzliche Web-API-Komponenten für Komponententests von Arbeit "und" DAL-Unterstützung
- Eine MVC-Projektvorlage mit Gerüst zum schnellen Einstieg

Weitere Informationen zu den einzelnen Page-Anwendung zu unterstützen, in ASP.NET MVC 4 finden Sie auf [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen an Standardeinstellung-Projektvorlagen

Die Vorlage, die zum Erstellen von neuen ASP.NET MVC 4-Projekte wurde aktualisiert, um eine Website mehr Modern-Suche zu erstellen:

![](mvc4-beta-release-notes/_static/image1.png)

Zusätzlich zu den kosmetische Verbesserungen wurde es Funktionalität in der neuen Vorlage verbessert. Die Vorlage verwendet ein Verfahren namens adaptives Rendering in Desktopbrowsern und mobilen Browsern ohne Anpassung gut aussehen.

![](mvc4-beta-release-notes/_static/image2.png)

Um adaptive Rendering in Aktion zu sehen, können Sie mobile-Emulator verwenden, oder versuchen Sie es nur mit dem Ändern der Größe der desktop Browserfenster, um kleiner sein. Das Browserfenster klein genug abruft, ändert das Layout der Seite.

Eine andere Erweiterung aus der Standardprojektvorlage ist die Verwendung von JavaScript aus, um eine umfangreichere Benutzeroberfläche bereitstellen. Die Anmeldung und Registrierung Links, die in der Vorlage verwendet werden einige Beispiele für wie dem jQuery UI-Dialogfeld zu verwenden, um eine umfassende Anmeldebildschirm darzustellen:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile-Projektvorlage

Wenn Sie ein neues Projekt beginnen und einen Standort speziell für mobile Geräte und Browser Tablet erstellen möchten, können Sie die neue Projektvorlage von Mobile-Anwendung. Dieser Wert basiert auf jQuery Mobile, ein Open Source-Bibliothek für touchbedienung optimierten Benutzeroberfläche zu erstellen:

![](mvc4-beta-release-notes/_static/image4.png)

Diese Vorlage enthält die gleiche Anwendungsstruktur wie der Internet Application-Vorlage (und der controllercode ist im Grunde identisch), es ist jedoch formatiert, jQuery Mobile gut aussehen und Verhalten sich auch auf Touch-basierte mobile Geräte verwenden. Weitere Informationen zur Struktur hinzu und Formatieren von mobile-Benutzeroberfläche finden Sie unter der [jQuery Mobile Projekt Website](http://jquerymobile.com/).

Falls Sie eine Desktop-orientierten Site noch Ansichten für Mobilgeräte optimierte hinzugefügt werden soll, oder einen einzelnen Standort erstellt werden soll, die unterschiedlich formatierte Ansichten für desktop und mobile Browser dient, können Sie die neue Anzeigemodi-Funktion verwenden. (Siehe nächsten Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Das neue Feature zur Anzeigemodi ermöglicht eine Anwendung, die je nach Browser Ansichten auswählen, die die Anforderung stammt. Z. B. kann ein Desktopbrowser auf der Startseite anfordert, die Anwendung die Views\Home\Index.cshtml-Vorlage verwenden. Ein mobilen Browser auf der Startseite "angefordert, kann die Anwendung die Vorlage Views\Home\Index.mobile.cshtml zurückgeben.

Layouts und Replikatsgruppenelemente können auch für bestimmte Browsertypen überschrieben werden. Zum Beispiel:

- Wenn Ordner Views\Shared sowohl enthält die \_Layout.cshtml und \_Layout.mobile.cshtml Vorlagen, standardmäßig die Anwendung \_Layout.mobile.cshtml während der Anfragen von mobilen Browsern und \_Layout.cshtml, während andere Anforderungen.
- Enthält sowohl ein Ordner \_MyPartial.cshtml und \_MyPartial.mobile.cshtml, die Anweisung @Html.Partial("\_MyPartial") gibt die \_MyPartial.mobile.cshtml während Anforderungen von Mobile Browser, und \_MyPartial.cshtml, während andere Anforderungen.

Wenn Sie spezifischere Ansichten, Layouts oder Teilansichten für andere Geräte erstellen möchten, können Sie ein neues registrieren *DefaultDisplayMode* Instanz angeben, den Namen, wenn bestimmte Bedingungen erfüllt, die eine Anforderung für gesucht werden soll. Sie können z. B. den folgenden Code hinzu Hinzufügen der *Anwendung\_starten* Methode in der Datei "Global.asax" die Zeichenfolge "iPhone" als einen Anzeigemodus zu registrieren, die gilt, wenn es sich bei der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Nachdem dieser Code ausgeführt, wenn ein Apple iPhone-Browser eine Anforderung sendet, verwendet die Anwendung die Views\Shared\\_Layout.iPhone.cshtml-Layout (falls vorhanden).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, Ansichtumschaltung und Browser überschreiben

jQuery Mobile ist eine open Source-Bibliothek zum Erstellen von berührungsoptimiertes Web-Benutzeroberfläche. Wenn Sie jQuery Mobile mit einer ASP.NET MVC 4-Anwendung verwenden möchten, können Sie herunterladen und Installieren von NuGet-Paket, das Ihnen den Einstieg hilft. Geben Sie zum Installieren von Visual Studio-Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Dadurch werden installiert, jQuery Mobile und einige Helper-Dateien, einschließlich der folgenden:

- Ansichten/freigegeben/\_Layout.Mobile.cshtml, also ein jQuery Mobile-basierten Layout.
- Eine Komponente besteht aus den Ansichten/freigegeben ansichtumschaltung /\_ViewSwitcher.cshtml partielle Ansicht und Controller ViewSwitcherController.cs.

Nachdem Sie das Paket installiert haben, führen Sie die Anwendung, die mit einem mobilen Browser (oder äquivalent, wie die Firefox [Benutzer-Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) -Add-On). Sehen Sie, dass Ihre Seiten ganz anders aussehen, da jQuery Mobile Layout behandelt und Formate. Um dieses nutzen möchten, können Sie Folgendes tun:

- Außerkraftsetzungen von Mobile-spezifische Ansicht erstellen, wie beschrieben unter [Anzeigemodi](#_Toc303253810) zuvor (z. B. Views\Home\Index.mobile.cshtml um Views\Home\Index.cshtml für mobile Browser überschreiben erstellen).
- Lesen der [jQuery Mobile-Dokumentation](http://jquerymobile.com/) Informationen zum Hinzufügen von UI-Elemente berührungsoptimiertes in mobilen Ansichten enthält.

Eine Konvention für Mobilgeräte optimierte Webseiten besteht darin, einen Link hinzufügen, dessen Text ist etwas wie Desktopansicht oder vollständige standortmodus, in dem Benutzer, die in einer desktop-Version von der Seite wechseln kann. Das jQuery.Mobile.MVC-Paket enthält eine Beispiel ansichtumschaltung Komponente zu diesem Zweck. Es dient in der standardmäßigen Views\Shared\\_Layout.Mobile.cshtml-Ansicht und sieht wie folgt, wenn die Seite gerendert wird:

![](mvc4-beta-release-notes/_static/image5.png)

Wenn Besucher auf den Link klicken, werden sie in der Desktopversion von der gleichen Seite gewechselt.

Da das Layout Ihren desktop eine ansichtumschaltung standardmäßig nicht eingeschlossen werden, keine Besucher eine Möglichkeit, mobile Modus zu erhalten. Um dies zu ermöglichen, fügen Sie den folgenden Verweis auf  *\_ViewSwitcher* auf Ihrem desktop Layout nur innerhalb der  *&lt;Text&gt;*  Element:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Ansichtumschaltung verwendet ein neues Feature namens Browser überschreiben. Mit diesem Feature können Sie Ihre Anwendung Anforderungen zu behandeln, als ob sie stammen von einem anderen Browser (Benutzer-Agent) demjenigen, sie sind tatsächlich aus. Die folgende Tabelle enthält die Methoden, die Browser überschreiben bereitstellt.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Überschreibt die Anforderung tatsächlichen Benutzer-Agent-Wert, der mit dem angegebenen Benutzer-Agent. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Außerkraftsetzungswert für die Anforderung Benutzer-Agent oder der tatsächlichen Benutzer-Agent-Zeichenfolge zurückgegeben werden, wenn keine Außerkraftsetzung angegeben wurde. |
| `HttpContext.GetOverriddenBrowser()` | Gibt eine *HttpBrowserCapabilitiesBase* -Instanz, die der Benutzer-Agent, die derzeit für die Anforderung festgelegt entspricht (tatsächlichen oder überschrieben). Sie können diesen Wert verwenden, beim Abrufen von Eigenschaften wie z. B. *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Entfernt alle außer Kraft gesetzten Benutzer-Agent für die aktuelle Anforderung. |

Überschreiben der Browser ist eine grundlegende Funktion von ASP.NET MVC 4 und ist verfügbar, auch wenn Sie das Paket jQuery.Mobile.MVC nicht installieren. Er wirkt sich jedoch nur anzeigen, Layout und Speicherortformate der Teilansicht Auswahl – er wirkt sich keiner anderen ASP.NET-Funktion, von denen abhängt der *Request.Browser* Objekt.

Standardmäßig wird die Benutzer-Agent-Außerkraftsetzung mit einem Cookie gespeichert. Wenn Sie die Außerkraftsetzung an anderer Stelle (z. B. in einer Datenbank) speichern möchten, können Sie den Standardanbieter ersetzen (*BrowserOverrideStores.Current*). Dokumentation für diesen Anbieter zu eine spätere Version von ASP.NET MVC steht zur Verfügung.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Know-how für die Codegenerierung in Visual Studio

Das neue Feature für die Rezepte kann Visual Studio generieren lösungsspezifische Code basierend auf Pakete, die Sie installieren können, mithilfe von NuGet. Das Framework Rezepte ganz einfach für Entwickler Codegenerierungs-Plug-Ins, schreiben, die Sie ebenfalls verwenden können, ersetzen Sie den integrierten Code-Generatoren für einen Bereich hinzufügen, die Controller hinzufügen und die Ansicht hinzufügen. Da Rezepte als NuGet-Pakete bereitgestellt werden, können sie problemlos in die quellcodeverwaltung eingecheckt und alle Entwickler, die für das Projekt automatisch freigegeben. Sie sind auch auf der Basis eines pro-Lösung verfügbar.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Task-Unterstützung für asynchrone Controller

Sie können jetzt schreiben asynchrone Aktionsmethoden als einzelne Methoden, die ein Objekt des Typs zurückgeben *Aufgabe* oder *Aufgabe&lt;ActionResult&gt;*.

Z. B. bei Verwendung von Visual C#-5 (oder über die [Async CTP](https://msdn.microsoft.com/en-us/vstudio/async.aspx)), können Sie eine asynchrone Aktionsmethode, die wie folgt aussieht:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

In der vorherigen Aktionsmethode, die Aufrufe von *newsService.GetHeadlinesAsync* und *sportsService.GetScoresAsync* asynchron aufgerufen werden und werden nicht durch einen Thread aus dem Threadpool blockiert.

Asynchrone Aktionsmethoden, die zurückgeben *Aufgabe* Instanzen können auch Timeouts unterstützen. Um die Aktionsmethode abzubrechende gestalten, fügen Sie einen Parameter vom Typ *CancellationToken* auf die Methodensignatur für die Aktion. Das folgende Beispiel zeigt eine asynchrone Aktionsmethode mit dem ein Timeout von 2500 Millisekunden und zeigt, die eine *"timedOut"* an den Client anzeigen, wenn ein Timeout auftritt.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta unterstützt die September 2011 1.5-Version des Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

- **Nach der Installation von ASP.NET MVC 4 Beta, kann die CSHTML/VBHTML-Editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML-Editor für eine lange Zeit nach dem Eingeben der Ausschnitt oder JavaScript in Cshtml oder Vbhtml-Dateien anhalten.** Dieser Fehler tritt nur in ASP.NET MVC 4-Anwendungen, die gerade erstellt wurden und noch nicht kompiliert wurde.

    Die problemumgehung besteht darin, kompilieren Sie das Projekt, um die Assemblys im Ordner "Bin" zu erhalten. Beachten Sie, wenn Sie das Projekt bereinigen, das Assemblys aus dem Ordner "bin" entfernt, wird das Problem Editor wieder stammen.

    Dies wird in der nächsten Version behoben werden.
- **C#-Projektvorlagen für Visual Studio 11 Beta enthalten eine fehlerhaften Verbindungszeichenfolge in Global.asax.cs.** Die standardmäßige Verbindung, die in der Anwendung angegebenen\_Start-Methode für Projekte in Visual Studio 11 Beta erstellt eine LocalDB-Verbindungszeichenfolge enthalten, die einen ohne Escapezeichen umgekehrter Schrägstrich enthält (\) Zeichen. Dies führt zu einem Verbindungsfehler bei Zugriffsversuchen auf ein Entity Framework ' DbContext ', die eine SqlException generiert.

    Um dieses Problem zu beheben, mit Escapezeichen versehen den umgekehrte Schrägstrich in der App\_Start-Methode der Global.asax.cs, damit er wie folgt aussieht:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **ASP.NET MVC 4-Anwendungen, die auf .NET 4.5 abzielen, löst eine FileLoadException beim Versuch, die Ausführung unter .NET 4.0 System.Net.Http.dll-Assembly zugreifen.** ASP.NET MVC 4-Anwendungen, die unter .NET 4.5 erstellt wurden, enthalten eine Bindung Umleitung, die eine FileLoadException das führt, der angibt, "konnte nicht geladen werden-Datei oder Assembly 'System.Net.Http' oder eine ihrer Abhängigkeiten." Wenn die Anwendung auf einem System mit installiertem .NET 4.0 ausgeführt wird. Um dieses Problem zu beheben, entfernen Sie die folgende bindungsumleitung aus Datei "Web.config" ein:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Die Assembly Binding-Element in der geänderten Datei "Web.config" sollte wie folgt aussehen:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Die Elementvorlage "Controller hinzufügen" in Visual Basic-Projekten generiert einen falschen Namespace beim Aufrufen *** von innerhalb eines Bereichs.** Wenn Sie einen Bereich in einer ASP.NET MVC-Projekt einen Controller, die Visual Basic verwendet hinzufügen, fügt die Elementvorlage falschen Namespace in den Controller. Das Ergebnis ist eine "Datei nicht gefunden"-Fehler auf, wenn Sie auf eine beliebige Aktion im Controller navigieren.  
  
 Der generierte Namespace wird alles, was nach der Stammnamespace ausgelassen. Der generierten Namespace ist z. B. *RootNamespace* , sollte jedoch *RootNamespace.Areas.AreaName.Controllers* .
- **Wichtige Änderungen in der Razor-Ansichtsmodul.** Im Rahmen des eine neue Version von Razor-Parser, wurden die folgenden Typen von entfernt *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Die folgenden Methoden wurden ebenfalls entfernt: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Wenn WebMatrix.WebData.dll im Verzeichnis "/ bin" von einer ASP.NET MVC 4-apps in enthalten ist, hat er die URL für die Formularauthentifizierung.** Der Anwendung (z. B. durch "ASP.NET Web Pages mit Razor-Syntax" auswählen, wenn Sie das Dialogfeld "bereitstellbare Abhängigkeiten hinzufügen" verwenden) die WebMatrix.WebData.dll-Assembly hinzufügen, wird die Authentifizierung Anmeldung Umleitung für die Anmeldung/Account/überschrieben statt / Konto/Anmeldenamens, wie erwartet das ASP.NET MVC-Controllers ein Konto in der Standardeinstellung verwendet wird. Um zu verhindern, dass dieses Verhalten, und verwenden Sie die URL, die bereits im Abschnitt "Authentifizierung" der Datei "Web.config" angegeben, können Sie Hinzufügen einer AppSetting PreserveLoginUrl aufgerufen und auf "true" festgelegt haben: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Die NuGet-Paket-Manager nicht installiert werden, beim Versuch, die ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 installieren.** Zum Ausführen von Visual Studio 2010 und Visual Web Developer 2010 parallel zu ASP.NET MVC 4 müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Deinstallieren von ASP.NET MVC 4 fehlschlägt, wenn die erforderlichen Komponenten bereits deinstalliert wurden.** Um ASP.NET MVC ordnungsgemäß deinstalliert muss 4you ASP.NET MVC 4 Deinstallieren Sie vor dem Deinstallieren von Visual Studio.
- **Ausführen eines Standard-Web-API-Projekts zeigt Anweisungen, die falsch weisen Sie den Benutzer zum Hinzufügen von Routen, die mit der RegisterApis-Methode, die nicht vorhanden ist.** Routen sollten in der RegisterRoutes-Methode, die mithilfe von ASP.NET Routentabelle hinzugefügt werden.
- **Installieren ASP.NET MVC 4 Beta wird ASP.NET MVC 3 RTM Anwendungen unterbrochen.** ASP.NET MVC 3-Anwendungen, die erstellt wurden, mit der RTM-Version (nicht mit der ASP.NET MVC 3 Tools Update-Version) die folgenden Änderungen Seite-an-Seite mit ASP.NET MVC 4 Beta Ausführung erforderlich. Beim Erstellen des Projekts ohne diese Updates Ergebnisse in Kompilierungsfehler auftreten. 

    **Erforderliche updates**

    1. In der Stammdatei "Web.config", fügen Sie einen neuen  *&lt;"appSettings"&gt;*  Eintrag mit dem Schlüssel *WebPages:Version* und dem Wert *1.0.0.0*.

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *Projektname*csproj.
    3. Suchen Sie die folgenden Assemblyverweise aus: 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        Ersetzen Sie sie wie folgt:

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. Speichern Sie die Änderungen zu, schließen Sie die Datei clientprojektdatei (csproj), die Sie wurden bearbeiten, und klicken Sie dann mit der rechten Maustaste des Projekts und wählen Sie erneut laden.
