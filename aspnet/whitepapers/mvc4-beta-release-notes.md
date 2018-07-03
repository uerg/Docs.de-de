---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument beschreibt die Version von ASP.NET MVC 4 Beta für Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: ''
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 62bd641b1c5c926ec33ed3948854365aea99e979
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390607"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Dieses Dokument beschreibt die Version von ASP.NET MVC 4 Beta für Visual Studio 2010.
> 
> > [!NOTE]
> > Dies ist nicht die aktuellste Version. Die Anmerkungen zu dieser Version von ASP.NET MVC 4 RC stehen [hier](mvc4-release-notes.md).


- [Installationshinweise](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Softwareanforderungen](#_Toc303253805)
- [Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4](#_Toc303253806)
- [Neue Features in ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [ASP.NET-Single-Page-Anwendung](#_Toc317096198)
    - [Verbesserungen bei der Standard-Projektvorlagen](#_Toc303253808)
    - [Mobile-Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, der Ansichtumschaltung und Browser überschreiben](#_Toc303253811)
    - [Rezepte für die Codegenerierung in Visual Studio](#_Toc303253812)
    - [Task-Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Bekannte Probleme und aktueller Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET MVC 4 Beta für Visual Studio 2010 installiert werden können, aus der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) mithilfe des Webplattform-Installers.

Sie müssen alle zuvor installierten Preview-Versionen von ASP.NET MVC 4 vor der Installation von ASP.NET MVC 4 Beta deinstallieren.

Diese Version ist nicht kompatibel mit .NET Framework 4.5 Developer Preview. Sie müssen .NET 4.5 Developer Preview deinstallieren, vor der Installation von ASP.NET MVC 4 Beta.

ASP.NET MVC 4 installiert werden kann, und führe die Seite-an-Seite mit ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und Weitere Informationen zu ASP.NET MVC finden Sie auf der MVC 4-Seite der ASP.NET-Website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Unterstützung

Dies ist eine Vorschauversion und wird offiziell nicht unterstützt. Falls Sie Fragen zum Arbeiten mit dieser Version haben, stellen Sie diese zum ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), wobei Mitglieder der ASP.NET-Community informelle Unterstützung bieten. oftmals können sind.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2.0 und Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4

ASP.NET MVC 4 kann auf demselben Computer ausführen, wodurch Sie die Flexibilität bei der Entscheidung, wann zum Aktualisieren einer ASP.NET MVC 3-Anwendung zu ASP.NET MVC 4, parallel zu ASP.NET MVC 3 installiert werden.

Die einfachste Möglichkeit, ein upgrade ist zum Erstellen eines neuen ASP.NET MVC 4-Projekts und kopieren alle Ansichten, Controller, Code und Inhalt von Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt, und klicken Sie dann zum Aktualisieren der Assemblys verweist, in das neue Projekt mit das alte Projekt übereinstimmen. Wenn Sie Änderungen an der Datei "Web.config" in das MVC 3-Projekt vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web.config" in der MVC 4-Projekts zusammenführen.

Um eine vorhandene ASP.NET MVC 3-Anwendung auf Version 4 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Ersetzen Sie in allen Dateien "Web.config" im Projekt (es ist eine im Stammverzeichnis des Projekts im Ordner "Views" und im Ordner "Views" für die einzelnen Bereiche in Ihrem Projekt vorhanden), jede Instanz von den folgenden Text ein:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Aktualisieren Sie in der Stammdatei "Web.config" der *WebPages:Version* Element in "2.0.0.0", und fügen Sie einen neuen *PreserveLoginUrl* Schlüssel, der den Wert "True" hat:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Löschen Sie im Projektmappen-Explorer den Verweis auf *System.Web.Mvc* (die verweist auf die Version 3-DLL). Fügen Sie dann einen Verweis auf *System.Web.Mvc* (v4.0.0.0). Achten Sie insbesondere die folgenden Änderungen vor, um die Verweise der Assembly zu aktualisieren. Nachstehend sind die Details:

    1. Löschen Sie im Projektmappen-Explorer die Verweise auf die folgenden Assemblys: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Fügen Sie einen Verweise auf die folgenden Assemblys hinzu: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *ProjectName*csproj.
5. Suchen Sie die *ProjectTypeGuids* Element, und Ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} mit {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Die Änderungen zu speichern, schließen Sie die Projektdatei (.csproj), die Sie bearbeitet haben, mit der rechten Maustaste in des Projekts und wählen Sie dann das Projekt erneut laden.
7. Wenn das Projekt Drittanbieter Bibliotheken, die mit früheren Versionen von ASP.NET MVC kompiliert werden verweist, öffnen Sie die Stammdatei "Web.config", und fügen Sie die folgenden drei *BindingRedirect* Elemente unter dem  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Neue Features in ASP.NET MVC 4 Beta

In diesem Abschnitt wird beschrieben, Funktionen, die eingeführt wurden, in der ASP.NET MVC 4 Beta-Version.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 umfasst jetzt die ASP.NET Web-API, ein neues Framework zum Erstellen von HTTP-Diensten, die eine Vielzahl von Clients einschließlich Browsern und mobilen Geräten erreichen kann. ASP.NET Web-API ist auch eine ideale Plattform zum Erstellen von RESTful-Dienste.

ASP.NET Web-API unterstützt die folgenden Funktionen:

- **Moderne HTTP-Programmiermodell:** direkten Zugriff auf und Bearbeiten von HTTP-Anforderungen und Antworten in Ihre Web-APIs, die mit einer neuen, stark typisierte HTTP-Objektmodell. Das gleiche Programmiermodell sowie HTTP-Pipeline symmetrisch auf dem Client über den neuen Typ von "HttpClient" verfügbar ist.
- **Vollständige Unterstützung für Routen**: Web-APIs unterstützen jetzt den vollständigen Satz von Route-Funktionen, die immer einen Teil der Web-Stack, einschließlich Routenparametern und Einschränkungen wurden. Darüber hinaus bietet Zuordnung zu den Aktionen vollständige Unterstützung für Konventionen, damit Sie nicht mehr benötigen, anzuwendende Attribute wie z. B. [HttpPost] auf Ihre Klassen und Methoden.
- **Inhaltsaushandlung**: Client und Server zusammenarbeiten, um das richtige Format für von einer API zurückgegebenen Daten zu bestimmen. Wir bieten standardmäßig Unterstützung für XML, JSON und Formular-URL-codierten Format, und Sie können diese Unterstützung durch Hinzufügen Ihrer eigenen Formatierer zu erweitern oder sogar ersetzen die Standardstrategie für die Aushandlung von Inhalten.
- **Modellbindung und Validierung:** Modellbinder bieten eine einfache Möglichkeit zum Extrahieren von Daten aus verschiedenen Teilen einer HTTP-Anforderung und konvertiert die Nachrichtenteile in .NET-Objekte auf die durch die Web-API-Aktionen verwendet werden kann.
- **Filter:** Web-APIs unterstützt jetzt die Filter, einschließlich der bekannten Filter für Faktoren wie dem [Authorize]-Attribut. Sie erstellen und schließen Sie Ihren eigenen Filter für Aktionen, Autorisierung und Behandlung von Ausnahmen.
- **Der abfragekomposition:** durch eine einfache Rückkehr "IQueryable"&lt;T&gt;, Ihre Web-API unterstützt die Abfrage über den OData URL-Konventionen.
- **Verbesserte Prüfbarkeit von HTTP-Details:** statt HTTP-Details im statischen Kontextobjekte festlegen, können jetzt Web-API-Aktionen mit Instanzen von HttpRequestMessage und HttpResponseMessage arbeiten. Generische Versionen dieser Objekte, die auch zum Arbeiten mit benutzerdefinierten Typen zusätzlich zu den HTTP-Typen können vorhanden sein.
- **Verbesserte Inversion of Control (IoC) über DependencyResolver:** Web-API verwendet jetzt dienstlocatormuster von MVC Abhängigkeitskonfliktlöser implementiert, um Instanzen für viele unterschiedliche Funktionen zu erhalten.
- **Codebasierte Konfiguration:** Web-API-Konfiguration erfolgt ausschließlich über Code, lassen Ihre Konfiguration Dateien zu bereinigen.
- **Self-Hosting:** Web-APIs kann in Ihrem eigenen Prozess neben IIS gehostet werden, während Sie immer noch das volle Potenzial von Routen und anderen Funktionen von Web-API.

Weitere Informationen zu ASP.NET Web-API finden Sie unter [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET-Single-Page-Anwendung

ASP.NET MVC 4 umfasst jetzt eine frühe Vorschau der Oberfläche zum Erstellen von einseitigen Anwendungen mit signifikanten clientseitigen Interaktionen mit JavaScript und Web-APIs. Diese Unterstützung umfasst Folgendes:

- Ein Satz von JavaScript-Bibliotheken für umfangreichere lokalen Interaktionen mit zwischengespeicherten Daten
- Zusätzliche Web-API-Komponenten für die Arbeitseinheit und DAL-Unterstützung
- Eine MVC-Projektvorlage mit Gerüst zum schnellen Einstieg

Für Weitere Informationen zu den einseitigen Anwendung in ASP.NET MVC 4-Unterstützung finden Sie auf [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen bei der Standard-Projektvorlagen

Die Vorlage, die zum Erstellen der neuen ASP.NET MVC 4-Projekte wurde aktualisiert, um einen moderner aussehenden-Website zu erstellen:

![](mvc4-beta-release-notes/_static/image1.png)

Zusätzlich zu den Verbesserungen kosmetischer Natur hat es in der neuen Vorlage verbessert. Die Vorlage verwendet eine Technik namens adaptives Rendering in aktivierten Desktopbrowsern und mobile Browser ohne Anpassung gut aussehen.

![](mvc4-beta-release-notes/_static/image2.png)

Um adaptive Rendering in Aktion zu sehen, können einen mobilen-Emulator verwenden oder Ändern der Größe des desktop-Browser-Fensters, um kleinere es einfach ausprobieren. Wenn das Browserfenster klein genug erhält, wird das Layout der Seite ändern.

Eine weitere Verbesserung von die standardmäßige Projektvorlage ist die Verwendung von JavaScript, um eine umfangreichere Benutzeroberfläche bereitzustellen. Die Login and Register Links, die in der Vorlage verwendet werden sind Beispiele für die jQuery-UI-Dialogfeld zu verwenden, um eine umfassende Anmeldebildschirm darzustellen:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile-Projektvorlage

Wenn Sie ein neues Projekt beginnen und einen Standort speziell für mobile Geräte und Tablets erstellen möchten, können Sie die neue Projektvorlage für die Mobile-Anwendung. Dieser Wert basiert auf jQuery Mobile, ein Open Source-Bibliothek für touchbedienung optimierten Benutzeroberfläche zu erstellen:

![](mvc4-beta-release-notes/_static/image4.png)

Diese Vorlage enthält die gleiche Anwendungsstruktur wie Internet Application-Vorlage (und der controllercode ist nahezu identisch), aber es ist ein Format mit jQuery Mobile gut aussehen und Verhalten sich auch auf mobilen Geräten touchbasierte. Weitere Informationen finden Sie Informationen zum Strukturieren und mobilen Benutzeroberfläche formatieren, finden Sie unter den [mobilen jQuery-Projektwebsite](http://jquerymobile.com/).

Wenn bereits eine Desktop-orientierten-Website, für Mobilgeräte optimierten Ansichten auf hinzugefügt werden soll, oder wenn Sie einen einzelnen Standort erstellen möchten, die unterschiedlich formatierte Ansichten für Desktop- und mobile Browser dient, können Sie den neuen Anzeigemodi. (Siehe nächsten Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Die neuen Anzeigemodi kann es sich um eine Anwendung, die Ansichten, je nach Browser auswählen, die die Anforderung stammt. Wenn ein desktop-Browser auf der Startseite anfordert, kann die Anwendung z. B. die Views\Home\Index.cshtml-Vorlage verwenden. Wenn auf der Startseite von ein mobiler Browser angefordert wird, kann die Anwendung die Views\Home\Index.mobile.cshtml Vorlage zurück.

Layouts und Teilansichten können auch für bestimmte Browsertypen überschrieben werden. Zum Beispiel:

- Wenn Ihr Ordner "Views\Shared" enthält die \_Layout.cshtml und \_Layout.mobile.cshtml Vorlagen, wird die Anwendung standardmäßig \_Layout.mobile.cshtml während der Anfragen von mobilen Browsern und \_Layout.cshtml, während andere Anforderungen.
- Wenn ein Ordner enthält \_MyPartial.cshtml und \_MyPartial.mobile.cshtml, die Anweisung @Html.Partial("\_MyPartial") rendert \_MyPartial.mobile.cshtml während der Anforderungen von Mobile Browser und \_MyPartial.cshtml, während andere Anforderungen.

Wenn Sie spezifischere Ansichten, Layouts und Teilansichten für andere Geräte erstellen möchten, können Sie ein neues registrieren *DefaultDisplayMode* Instanz zum Angeben der um zu suchenden, wenn eine Anforderung bestimmte Bedingungen erfüllt. Beispielsweise können Sie den folgenden Code hinzu Hinzufügen der *Anwendung\_starten* -Methode in der Datei "Global.asax", um die Zeichenfolge "iPhone" als einen Anzeigemodus zu registrieren, die gilt, wenn Sie der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Nachdem dieser Code ausgeführt, wenn ein Apple iPhone-Browser eine Anforderung sendet, verwendet Ihre Anwendung die Views\Shared\\_Layout.iPhone.cshtml Layout (falls vorhanden).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, der Ansichtumschaltung und Browser überschreiben

jQuery Mobile ist ein open-Source-Bibliothek zum Erstellen von Touch-optimierte Web-UI. Wenn Sie jQuery Mobile mit einer ASP.NET MVC 4-Anwendung verwenden möchten, können Sie herunterladen und installieren ein NuGet-Paket, das hilft Ihnen beim Einstieg. Um die Installation über die Visual Studio-Paket-Manager-Konsole, geben Sie den folgenden Befehl aus:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Dadurch werden installiert, jQuery Mobile und einige Hilfsprogrammdateien, einschließlich der folgenden:

- Views/Shared/\_Layout.Mobile.cshtml, die ein jQuery Mobile-basierten Layout ist.
- Eine ansichtumschaltung Komponente besteht aus denViews/Shared/\_ViewSwitcher.cshtml Teilansicht und ViewSwitcherController.cs-Controller.

Nachdem Sie das Paket installiert haben, führen Sie die Anwendung, die mit einem mobilen Browser (oder äquivalent, wie die Firefox [Benutzer-Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) -Add-On). Sehen Sie, dass Ihre Seiten unterschiedlich angezeigt, da jQuery Mobile Layout behandelt und formatieren. Um diesen Vorteil nutzen zu können, können Sie Folgendes tun:

- Außerkraftsetzungen von mobiltaugliche Ansicht zu erstellen, wie unter [Anzeigemodi](#_Toc303253810) zuvor (z. B. Views\Home\Index.mobile.cshtml um Views\Home\Index.cshtml für mobile Browser überschreiben, erstellen).
- Lesen der [jQuery Mobile-Dokumentation](http://jquerymobile.com/) Weitere Informationen zum Hinzufügen von Touch-optimierte Benutzeroberflächenelemente in mobile Ansichten.

Eine Konvention für Mobilgeräte optimierte Web Pages ist auf einen Link hinzufügen, dessen Text ist etwas wie Desktopansicht oder vollständige standortmodus, in dem Benutzer, die in einer Desktopversion von der Seite wechseln kann. Das jQuery.Mobile.MVC-Paket enthält eine ansichtumschaltung Komponente für diesen Zweck. Hiermit wird in der standardmäßigen Views\Shared\\_Layout.Mobile.cshtml anzeigen, und es sieht wie folgt aus, wenn die Seite gerendert wird:

![](mvc4-beta-release-notes/_static/image5.png)

Wenn Besucher auf den Link klicken, können sie auf die desktop-Version der gleichen Seite gewechselt.

Da Ihre Desktoplayout eine ansichtumschaltung nicht standardmäßig enthalten sind, müssen Besucher keine Möglichkeit, um mobile-Modus zu erhalten. Um dies zu aktivieren, fügen Sie den folgenden Verweis  *\_ViewSwitcher* auf Ihrem desktop Layout an, nur innerhalb der *&lt;Text&gt;* Element:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Ansichtumschaltung verwendet ein neues Feature namens Browser überschreiben. Diese Funktion kann Ihre Anwendung, die Anforderungen zu behandeln, als ob sie bald wurden von einem anderen Browser (Benutzer-Agent) als die sind tatsächlich aus. Die folgende Tabelle enthält die Methoden, die Browser überschreiben bereitstellt.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Überschreibt der Anforderung tatsächlichen Benutzer-Agent-Wert mit dem angegebenen Benutzer-Agent an. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Gibt Außerkraftsetzungswert in der Anforderung. Benutzer-Agent oder der tatsächlichen Benutzer-Agent-Zeichenfolge zurück, wenn keine Außerkraftsetzung angegeben wurde. |
| `HttpContext.GetOverriddenBrowser()` | Gibt eine *HttpBrowserCapabilitiesBase* Instanz, die der Benutzer-Agent eingerichtet, für die Anforderung entspricht (tatsächlichen oder überschrieben). Sie können diesen Wert verwenden, um Eigenschaften zu erhalten, wie z. B. *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Entfernt alle außer Kraft gesetzten Benutzer-Agent für die aktuelle Anforderung. |

Außerkraftsetzen der Browser ist ein zentrales Feature von ASP.NET MVC 4 und ist verfügbar, auch wenn Sie nicht das jQuery.Mobile.MVC-Paket installieren. Es wirkt sich jedoch nur anzeigen, Layout und Speicherortformate der Teilansicht Auswahl – es hat keine Auswirkungen auf jede andere ASP.NET-Funktion, von denen abhängt der *Request.Browser* Objekt.

Standardmäßig werden die Benutzer-Agent-Außerkraftsetzung über ein Cookie gespeichert. Wenn Sie die Außerkraftsetzung (z. B. in einer Datenbank) an einem anderen Ort speichern möchten, können Sie den Standardanbieter ersetzen (*BrowserOverrideStores.Current*). Dokumentation für diesen Anbieter werden zur Verfügung, die eine spätere Version von ASP.NET MVC begleitet.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Rezepte für die Codegenerierung in Visual Studio

Das neue Feature für die Rezepte ermöglicht Visual Studio zum Generieren von lösungsspezifischem Code auf Grundlage der Pakete, die Sie mithilfe von NuGet installieren können. Das Framework Rezepte erleichtert es Entwicklern, codegenerierung Plug-Ins zu schreiben, die Sie ebenfalls verwenden können, ersetzen Sie den integrierten Code-Generatoren für Bereich hinzufügen "," Controller hinzufügen, und "Ansicht hinzufügen. Da Rezepte als NuGet-Pakete bereitgestellt werden, können sie ganz einfach in die quellcodeverwaltung eingecheckt und für alle Entwickler auf das Projekt automatisch freigegeben. Sie sind auch auf einer Basis pro Lösung verfügbar.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Task-Unterstützung für asynchrone Controller

Sie können jetzt schreiben asynchroner Aktionsmethoden als einzelne Methoden, die ein Objekt des Typs zurückgeben *Aufgabe* oder *Aufgabe&lt;ActionResult&gt;*.

Z. B. bei Verwendung von Visual c# 5 (oder mithilfe der [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), Sie können angeben, erstellen eine asynchronen Aktionsmethode, die wie folgt aussieht:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

In der vorherigen Aktionsmethode, die Aufrufe an *newsService.GetHeadlinesAsync* und *sportsService.GetScoresAsync* asynchron aufgerufen werden und einen Thread aus dem Threadpool nicht blockieren.

Asynchrone Aktionsmethoden, die zurückgeben *Aufgabe* Instanzen unterstützen auch Timeouts. Um Ihrer Aktionsmethode abbrechbar zu machen, fügen Sie einen Parameter vom Typ *CancellationToken* der Action-Methodensignatur. Das folgende Beispiel zeigt eine asynchrone Aktionsmethode, bei dem ein Timeout von 2500 Millisekunden und zeigt, die, eine *"timedOut"* an den Client anzeigen, wenn ein Timeout auftritt.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta unterstützt die September 2011 1.5-Version von Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

- **Nach der Installation von ASP.NET MVC 4 Beta, kann der CSHTML/VBHTML-Editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML-Editor einen längeren Zeitraum, nach dem Eingeben der Ausschnitt oder JavaScript Cshtml oder Vbhtml-Dateien anhalten.** Dies ist nur in ASP.NET MVC 4-Anwendungen, die erst kürzlich erstellt wurde und noch nicht kompiliert wurden.

    Die problemumgehung ist zum Kompilieren des Projekts, um die Assemblys im Ordner "Bin" zu erhalten. Beachten Sie, wenn Sie das Projekt bereinigen, das Assemblys aus dem Ordner "Bin" entfernt, die das Editor-Problem Ergebnisart zurückgegeben wird.

    Dies wird in der nächsten Version behoben.
- **C#-Projektvorlagen für Visual Studio 11 Beta enthalten eine fehlerhaften Verbindungszeichenfolge in "Global.asax.cs".** Die standardverbindung, die in der Anwendung angegeben\_Start-Methode für Projekte in Visual Studio 11 Beta erstellt eine LocalDB-Verbindungszeichenfolge enthält die ohne Escapezeichen einen umgekehrten Schrägstrich enthält (\) Zeichen. Dies führt zu einem Verbindungsfehler bei Zugriffsversuche auf ein Entity Framework-DbContext, die eine SqlException generiert.

    Um dieses Problem zu beheben, mit Escapezeichen versehen den umgekehrte Schrägstrich in der App\_Start-Methode von "Global.asax.cs" so, dass er wie folgt lautet:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **ASP.NET MVC 4-Anwendungen, die auf .NET 4.5 ausgerichtet, löst eine FileLoadException beim Versuch, die die Ausführung unter .NET 4.0 System.Net.Http.dll-Assembly zugreifen.** ASP.NET MVC 4-Anwendungen, die unter .NET 4.5 erstellt wurden, enthalten eine Bindung umleiten, die in einem FileLoadException führen, die angibt, die "konnte nicht geladen werden Datei oder Assembly 'System.Net.Http' oder eine ihrer Abhängigkeiten." Wenn die Anwendung auf einem System mit installiertem .NET 4.0 ausgeführt wird. Um dieses Problem zu beheben, entfernen Sie die folgende bindungsumleitung aus "Web.config" ein:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Die Assembly Binding-Element in der geänderten Datei "Web.config" sollte wie folgt aussehen:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Elementvorlage "Controller hinzufügen" in Visual Basic-Projekten generiert einen falschen Namespace beim Aufrufen</strong><strong>aus innerhalb eines Bereichs.</strong> Wenn Sie einen Controller in einen Bereich in einem ASP.NET MVC-Projekt, die Visual Basic verwendet hinzufügen, fügt die Item-Vorlage den falschen Namespace in den Controller. Das Ergebnis ist ein "Datei nicht gefunden"-Fehler auf, wenn Sie auf eine beliebige Aktion im Controller navigieren.  
  
  Der generierte Namespace lässt alles nach den Stamm-Namespace. Beispielsweise ist von der generierten Namespace *RootNamespace* , sollte jedoch *RootNamespace.Areas.AreaName.Controllers* .
- **Wichtige Änderungen in der Razor-Ansichtsengine.** Im Rahmen einer Neuerstellung der Razor-Parser, wurden die folgenden Typen von entfernt *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Die folgenden Methoden wurden ebenfalls entfernt werden: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Wenn WebMatrix.WebData.dll in das Verzeichnis "/ bin" von einer ASP.NET MVC 4-apps enthalten ist, hat er die URL für die Formularauthentifizierung.** Hinzufügen der Assembly WebMatrix.WebData.dll zu Ihrer Anwendung (z. B. durch Verwendung des Dialogfelds bereitstellbare Abhängigkeiten hinzufügen, wählen Sie "ASP.NET Web Pages mit Razor-Syntax") überschreibt die Anmeldung Umleitung Authentifizierung/Konto/anmelden statt / Konto/anmelden, wie in der Standardeinstellung die ASP.NET MVC-Kontocontroller erwartet wird. Um zu verhindern, dass dieses Verhalten, und verwenden Sie die URL, die bereits im Abschnitt "Authentifizierung" von "Web.config" angegeben wird, können ein AppSetting namens PreserveLoginUrl hinzufügen und auf "true" festlegen: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Der NuGet-Paket-Manager kann beim Installieren von ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 zu installieren.** Zum Ausführen von Visual Studio 2010 und Visual Web Developer 2010 parallel zu ASP.NET MVC 4 müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Deinstallieren von ASP.NET MVC 4 schlägt fehl, wenn die Voraussetzungen bereits deinstalliert wurden.** Um ordnungsgemäß zu ASP.NET MVC deinstallieren müssen 4you ASP.NET MVC 4 deinstallieren, vor dem Deinstallieren von Visual Studio.
- **Ausführen eines Standard-Web-API-Projekts zeigt die Anweisungen, die nicht ordnungsgemäß Weiterleiten der Benutzern das Hinzufügen der Routen, die mit der RegisterApis-Methode, die nicht vorhanden ist.** Routen sollten in der RegisterRoutes-Methode, die mithilfe der ASP.NET Routingtabelle hinzugefügt werden.
- **Installation von ASP.NET MVC 4 Beta, wird die ASP.NET MVC 3 RTM-Anwendungen funktionsuntüchtig.** ASP.NET MVC 3-Anwendungen, die erstellt wurden, mit der RTM-Version Release (nicht mit der ASP.NET MVC 3 Tools Update-Version), müssen Sie die folgenden Änderungen für die Arbeit Seite-an-Seite mit ASP.NET MVC 4 Beta. Beim Erstellen des Projekts ohne diese Updates Ergebnisse Kompilierungsfehler vorzunehmen. 

    **Erforderliche updates**

  1. Fügen Sie in der Stammdatei "Web.config" einen neuen *&lt;"appSettings"&gt;* Eintrag mit dem Schlüssel *WebPages:Version* und den Wert *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *ProjectName*csproj.
  3. Suchen Sie die folgenden Assemblyverweise aus: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Ersetzen sie durch Folgendes:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Speichern Sie die Änderungen zu, schließen Sie die clientprojektdatei (csproj) Datei, die Sie zuvor bearbeitet werden, mit der rechten Maustaste in des Projekts und wählen Sie erneut laden.
