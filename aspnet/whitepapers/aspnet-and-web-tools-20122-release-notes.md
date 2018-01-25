---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET und Tools 2012.2 Versionshinweise | Microsoft Docs
author: rick-anderson
description: "Versionshinweise für ASP.NET und Web Tools 2012.2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Anmerkungen zur Version von ASP.NET und Webtools 2012.2
====================
> Dieses Dokument beschreibt die Version von ASP.NET und Web Tools 2012.2. Es ist ein Update für Visual Studio-Webtools und ASP.NET.


- [Hinweise zur installationsnachbereitung](#_Installation)
- [Dokumentation](#_Documentation)
- [Unterstützung](#_Support)
- [Softwareanforderungen](#_Software_Requirements)
- [Neue Funktionen in ASP.NET und Webtools 2012.2](#_New_Features_in)

    - [Tools](#_Tooling)
    - [Webpublishing](#_Web_Publishing)
    - [ASP.NET MVC-Vorlagen](#_Templates)
    - [ASP.NET-Web-API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Bekannte Probleme und aktueller Änderungen](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

ASP.NET und 2012.2 von Web Tools für Visual Studio 2012, können mithilfe von installiert werden [Webplattform-Installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Dies ist ein Update für Visual Studio 2012 oder Visual Studio Express 2012 für das Web, die erforderlich ist. Wenn Sie nicht Visual Studio installiert haben, wird Visual Studio Express 2012 für das Web installiert.

Sie können ASP.NET und Web Tools 2012.2 auch manuell installieren. Sie benötigen Visual Studio 2012 oder Visual Studio Express 2012 für das Web installiert. Gehen Sie dann folgendermaßen vor: 

1. Herunterladen [ASP.NET und Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) Installer aus dem Download Center.
2. Aufforderung zur Eingabe klicken Sie bei der Ausführung. Sie können auch die Datei zur späteren Ausführung speichern.
3. Überprüfen Sie die Version von Visual Studio, die Sie aktualisiert werden. Hierzu können Sie durch Starten der Visual Studio, die Sie aktualisieren möchten. Klicken Sie dann auf das Menüelement Hilfe.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Wenn Sie das Menüelement finden Sie unter &quot;zu Microsoft Visual Studio 2012 für das Web&quot; dann herunterladen [Web Developer Tools 2012.2 - Visual Studio Express 2012 für das Web](https://go.microsoft.com/fwlink/?LinkID=282228). Andernfalls herunterladen [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Aufforderung zur Eingabe klicken Sie bei der Ausführung. Sie können auch die Datei zur späteren Ausführung speichern.

> [!NOTE]
> ASP.NET und Web Tools 2012.2 Version umfasst SQL Server Data Tools nicht. SQL Server und Windows Azure SQL-Datenbanken enthält einen umfassenderen Satz an-Datenbanktools einschließlich Offlineentwicklung Projekt gesichert, Schemavergleich und verbesserte Funktionen zur Bereitstellung. Weitere Informationen zum Installieren von SQL Server Data Tools finden [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET und Web Tools 2012.2 sind ASP.NET-Website (https://www.asp.net) verfügbar.

<a id="_Support"></a>
## <a name="support"></a>Unterstützung

ASP.NET Web Tools 2012.2 offiziell freigegeben und unterstützt. Sie können Ihren normalen Support-Kanal verwenden. Sie können auch Fragen zu den Foren für ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), in denen Mitglieder der ASP.NET-Community informelle Unterstützung leisten häufig können sind.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET und Web Tools 2012.2 erfordert Visual Studio 2012 oder Visual Studio Express 2012 für das Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Neue Funktionen in ASP.NET und Webtools 2012.2

Dieser Abschnitt beschreibt die Funktionen, die in der ASP.NET und Web Tools 2012.2 Version eingeführt wurden.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tools

- Seitenprüfung 

    - Unterstützt JavaScript Auswahl Zuordnung ermöglicht Seitenprüfung als Elemente zugeordnet werden, die dynamisch auf der Seite wieder auf den entsprechenden JavaScript-Code hinzugefügt wurden.
    - Die Fähigkeit, CSS-Updates in Echtzeit angezeigt.
    - Weitere Informationen finden Sie unter [automatische CSS-Synchronisierung und JavaScript Auswahl zuordnen, in der Seitenprüfung](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Syntaxhervorhebung für CoffeeScript, Mustache Lenkern und JsRender zu unterstützen.
    - Der HTML-Editor stellt Intellisense Knockout Bindungen.
    - WENIGER bearbeiten und die Compiler Unterstützung zum Erstellen von dynamischen CSS, die mit weniger.
    - JSON als .NET Klasse einfügen. Mit diesem Befehl fügen Sie spezielle JSON in c# oder VB.NET-Codedatei ein, und Visual Studio einfügen, werden .NET Klassen abgeleitet aus dem JSON automatisch generiert.
- Mobile-Emulator-Unterstützung bietet Erweiterbarkeit Hooks Drittanbieter-Emulatoren als eine VSIX installiert werden können. Die installierten Emulatoren werden, damit Entwickler ihren Websites auf einer Vielzahl von mobilen Geräten in der Vorschau anzeigen können in der Dropdownliste F5 angezeigt. Weitere Informationen zu dieser Funktion in Scott Hanselmans Eigenschaftskriterien [der neuen BrowserStack-Integration in Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Webpublishing

- Website-Projekte weisen nun die publishing dieselbe benutzererfahrung als Web Application-Projekte, einschließlich der Veröffentlichung auf Windows Azure-Websites.
- Selektive veröffentlichen &#8211; für eine oder mehrere Dateien können Sie (nach der Veröffentlichung mit einem Web Deploy-Endpunkt) die folgenden Aktionen ausführen: 

    - Veröffentlichen Sie ausgewählte Dateien.
    - Finden Sie unter den Unterschied zwischen einer lokalen Datei und einer Remotedatei.
    - Aktualisieren Sie die lokale Datei mit der remote-Datei oder aktualisieren Sie die remote-Datei mit der lokalen Datei zu.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC-Vorlagen

- Die neue Facebook-Anwendungsvorlage erleichtert das Schreiben von Facebook Canvas Anwendungen einfach. In wenigen einfachen Schritten können Sie eine Facebook-Anwendung erstellen, die Ruft Daten von einem angemeldeten Benutzer und ihre Freunde integriert. Die Vorlage enthält eine neue Bibliothek, die abnimmt sind bei der Erstellung einer Facebook-app, einschließlich der Authentifizierung, Berechtigungen, die Zugriff auf Facebook-Daten und vieles mehr kümmern. Weitere Informationen zur Verwendung der Vorlage der Facebook-Anwendung finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Eine neue Vorlage für die einzelnen Seite Anwendung MVC ermöglicht Entwicklern das Erstellen von interaktiven clientseitigen webapps mit HTML 5, CSS 3 und den beliebten Knockout- und jQuery JavaScript-Bibliotheken über ASP.NET Web-API. Die Vorlage enthält eine "Todo" List-Anwendung, die gängige Praktiken zum Erstellen einer JavaScript HTML5-Anwendung, die eine RESTful-Server-API verwendet veranschaulicht. Weitere Informationen finden Sie unter [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Sie können jetzt eine VSIX erstellen, die neue Vorlagen zum Dialogfeld "neues ASP.NET MVC-Projekt" hinzufügt. Erfahren Sie, wie hier: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes Paket &#8211; MVC-Projektvorlagen wurden aktualisiert, um das neue "FixedDisplayModes" NuGet-Paket enthalten, das eine problemumgehung für einen Programmfehler in MVC 4 enthält. Weitere Informationen über die im Paket enthaltenen Korrektur finden Sie in diesem Blogbeitrag ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) vom MVC-Team.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET Web-API verfügt über einige neue Funktionen erweitert:

- ASP.NET Web API OData
- ASP.NET Web API Tracing
- ASP.NET Web API Help Page

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData bietet Ihnen die Flexibilität zum Erstellen von OData-Endpunkte mit umfangreiche Geschäftslogik für beliebige Datenquellen erforderlich. Mit ASP.NET Web API OData steuern Sie die Menge des OData-Semantik, die Sie verfügbar machen möchten. ASP.NET Web API OData ist in der ASP.NET MVC 4-Projektvorlagen enthalten und ist auch über NuGet verfügbar ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData unterstützt derzeit die folgenden Funktionen:

- Aktivieren Sie die Semantik der OData-Abfrage durch Anwenden des Attributs [Queryable].
- Überprüfen Sie der OData-Abfragen und einfach schränken Sie den Satz von unterstützten Abfrageoptionen, Operatoren und Funktionen.
- Parameter binden ODataQueryOptions direkt zur eine abstract Syntax Tree-Darstellung der Abfrage abzurufen, die überprüft und einem IQueryable oder IEnumerable angewendet werden können.
- Aktivieren Sie Service-driven Paging und linkgenerierung der nächsten Seite, indem Sie Grenzwerte für Ergebnis [Queryable]-Attribut.
- Fordern Sie eine Inline-Anzahl der Gesamtzahl der übereinstimmenden Ressourcen mithilfe von $inlinecount an.
- Nullweitergabe zu steuern.
- Alle/alle Operatoren in $filter.
- Abzuleiten Sie ein Entity Data Model gemäß der Konvention, oder passen Sie explizit ein Modell ähnlich zu Entity Framework Code First.
- Entitätenmengen durch Ableiten von EntitySetController verfügbar gemacht.
- Einfache, anpassbare Konventionen für das Verfügbarmachen von Navigationseigenschaften, Bearbeiten von Links und Implementieren von OData-Aktionen.
- Vereinfacht das routing mit MapODataRoute-Erweiterungsmethode.
- Unterstützung für die versionsverwaltung durch das Verfügbarmachen von mehreren EDM-Modellen.
- Und verfügbar zu machen dienstdokument $metadata, damit Sie Clients (.NET für Windows Phone, Windows Store, usw.) generieren, können für Ihre Web-API.
- Unterstützung für die OData-Atom, JSON und JSON verbose-Formate.
- Erstellen, aktualisieren, teilweise aktualisieren (PATCH) und Löschen von Entitäten.
- Abfragen und Bearbeiten von Beziehungen zwischen Entitäten.
- Erstellen Sie beziehungslinks, die bis zu Ihrer Routen von Netzwerkdaten.
- Komplexe Typen
- Entitätsvererbung-Typ.
- Eigenschaften der Sammlung.
- Enumerationen.
- OData-Aktionen.
- Auf der gleichen Grundlage wie WCF Data Services, nämlich ODataLib basiert ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Weitere Informationen zu ASP.NET Web API OData finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API Tracing

ASP.NET Web API Tracing integriert .NET Tracing Ablaufverfolgungsdaten aus der Web-APIs. Es ist jetzt in der Web-API-Projektvorlage standardmäßig aktiviert. Protokollierungsdaten für Ihre Web-APIs wird gesendet, um das Fenster "Ausgabe" und mit IntelliTrace verfügbar gemacht. ASP.NET Web API Tracing können Sie Ablaufverfolgungsinformationen zu Ihrer Web-API beim Hosten in Windows Azure durch die Integration mit [Windows Azure-Diagnose](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Sie können außerdem installieren und Aktivieren von ASP.NET Web API Tracing in jeder Anwendung, die mithilfe des ASP.NET Web API Tracing NuGet-Pakets ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Weitere Informationen zum Konfigurieren und Verwenden von ASP.NET Web API Tracing finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API Help Page

Die ASP.NET Web API-Hilfeseite ist jetzt standardmäßig in der Web-API-Projektvorlage enthalten. Die ASP.NET Web API-Hilfeseite generiert automatisch die Dokumentation für die Web-APIs, die die HTTP-Endpunkte, die unterstützten HTTP-Methoden, Parameter und Beispiel-Anforderung und Antwort Nutzlasten für Nachrichten einschließlich. Dokumentation automatisch von den Kommentaren im Code per Pull abgerufen wird. Sie können auch die ASP.NET Web API-Hilfeseite für jede Anwendung, die mithilfe des ASP.NET Web API Hilfe Page NuGet-Pakets hinzufügen ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Weitere Informationen zum Einrichten und anpassen den ASP.NET Web API-Hilfeseite finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR vereinfacht echtzeitweb-Funktionen hinzufügen, um Ihre ASP.NET-Anwendung mithilfe von WebSockets, falls verfügbar und automatisch Fallback auf andere Techniken, wenn dies nicht der Fall.

Weitere Informationen zur Verwendung von ASP.NET SignalR finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs erleichtert für Web Forms-Entwickler zum Generieren von URLs cleaner (ohne die ASPX-Erweiterung) gesucht. Er erfordert nur wenig zu keine Konfiguration und kann mit vorhandenen ASP.NET v4. 0-Anwendungen verwendet werden. Die Funktion FriendlyURLs erleichtert auch die für Entwickler mobiler Unterstützung für ihre Anwendungen hinzufügen, durch die Unterstützung der Wechsel zwischen den Ansichten "desktop" und mobile.

Weitere Informationen zum Installieren und Verwenden von Friendly URLs von ASP.NET finden Sie unter [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und Änderungen, die in der Version von ASP.NET und Web Tools 2012.2 sind.

### <a name="installation-issues"></a>Probleme bei der Installation

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Außerhalb der Reihenfolge Installationen von Visual Studio 2012

Installieren eine zusätzliche SKU von Visual Studio 2012, nach der Installation von ASP.NET und Web Tools 2012.2 ein Reparaturvorgangs benötigen. Betrachten Sie die folgende Schritte aus:

1. Visual Studio 2012 Express für Web installieren
2. Installieren Sie ASP.NET und Webtools 2012.2
3. Installieren Sie Visual Studio 2012 Professional, Premium oder Ultimate

Schritt 2 würde nur Updates für Express für Web installieren. Um sicherzustellen, dass die zusätzliche SKU, die in Schritt 3 installiert das Update enthält, müssen Sie ASP.NET und Web Tools 2012.2 zum Installieren der Updates für die letzten SKU installiert zu reparieren. Dies gilt auch, wenn die SKUs in Schritt 1 und 3 rückgängig gemacht werden.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Microsoft ASP.NET und Web Tools 2012.2 zu installieren, wenn Visual Studio geöffnet ist.

Wenn VS während der Installation von Microsoft ASP.NET und Web Tools 2012.2 geöffnet ist, kann Visual Studio in einem fehlerhaften Zustand enden. Es wird empfohlen, dass Benutzer über alle Instanzen von Visual Studio vor dem Fortsetzen der Installation schließen.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Abbrechen von ASP.NET und Web Tools 2012.2 Setup während der installation

Abbrechen von ASP.NET und Web Tools 2012.2 wird während der Installation von Setup Visual Studio in einem fehlerhaften Zustand beibehalten. Auf dieses Problems führen Sie folgende Schritte aus: 

- Wechseln Sie zu Programme.
- Deinstallieren Sie Microsoft ASP.NET und Web Tools 2012.2, falls vorhanden.
- Installieren Sie Microsoft ASP.NET und Webtools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Nach der Deinstallation von ASP.NET und Web Tools 2012.2 der ASP.NET MVC 4 fehlen und Razor v2-Websitevorlagen

Deinstallieren von ASP.NET und Web Tools 2012.2 kann auch alle ASP.NET MVC 4 und Razor v2 Websitevorlagen auf Visual Studio 2012 deinstalliert werden.

Die problemumgehung besteht darin, die Visual Studio 2012-Installation zur Neuinstallation des ASP.NET MVC 4 und Razor v2 Websitevorlagen zu reparieren.

### <a name="tooling-issues"></a>Tools-Probleme

#### <a name="nuget-error-reported-during-project-creation"></a>NuGet Fehler während der projekterstellung

Nach der Installation von ASP.NET und Web Tools 2012.2 können Sie die folgende Fehlermeldung angezeigt, beim Erstellen einer MVC 4-Projekts

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

Der ASP.NET Web Tools 2012.2 geliefert wird NuGet 2.1 und wird die Erweiterung in Visual Studio 2012 aktualisiert. In einigen Fällen kann das VSIX-Installationsprogramm ordnungsgemäß VSIX-Projekt aktualisieren. Die folgenden Schritte können Sie dieses Problem zu beheben:

1. Starten Sie Visual Studio 2012 als Administrator
2. Wechseln Sie zu Extras -&gt;Erweiterungen und Updates und Deinstallieren von NuGet.
3. Schließen Sie Visual Studio.
4. Navigieren Sie zum Installationsordner ASP.NET und Web Tools 2012.2:

    1. Für Visual Studio 2012: **Programmieren Programme\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Für Visual Studio 2012 Express für Web: **Programme\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 für das Web**
5. Klicken Sie mit der Doppelklicken auf die NuGet.Tools.vsix NuGet neu installieren.

### <a name="web-api-issues"></a>Web-API-Probleme

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analysieren von Problemen in $filter und DateTime-Literale

Der OData-URI-Parser kann teilweise Datetime-Literale ordnungsgemäß zu analysieren. Beispielsweise $filter = Start-Eq "DateTime" ' 2012-12-31T12:00 "nicht ordnungsgemäß analysiert. Eine problemumgehung ist die Verwendung der vollständigen literal $filter = Start-Eq "DateTime" ' 2012-12-31T12:00:00 ".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData unterstützt keine Groß-/Kleinschreibung Eigenschaftsnamen.

OData unterstützt keine Groß-/Kleinschreibung Eigenschaftennamen in OData-Abfragen und OData-Pfad. Finden Sie unter "Arbeitsaufgaben":

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Wenn Benutzer mit abweichender Groß-/Kleinschreibung auf Javascript clientseitige und serverseitige vorhanden sind, werden sie möglicherweise dieses Problem auftreten. Dieses Problem ist ein Merkmal OData-Protokoll. Meldet jedoch viele Benutzer dieses Problem. Dieses Problem, zu umgehen müssen die Benutzer inbegriffen in URL zu korrigieren.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Standardmäßige OData-Konventionen routing unterstützt keine POST/PUT für Navigationseigenschaft.

Standardmäßige OData-Konventionen routing unterstützt keine POST/PUT für Navigationseigenschaft. Finden Sie unter Workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Wir werden diese häufig verwendete Konvention in Standardkonventionen fehlt.

Dieses Problem, zu umgehen müssen Benutzer neue routing-Konvention für eine Unterstützung zu erweitern.

### <a name="facebook-template-issues"></a>Probleme der Facebook-Vorlage

#### <a name="facebook-application-template-only-works-using-net-45"></a>Facebook-Anwendungsvorlage funktioniert nur mit .NET 4.5

Wählen Sie in der Dropdownliste "Framework" im Dialogfeld "Neues Projekt" auf der Facebook-Anwendungsvorlage in ASP.NET MVC 4 finden Sie unter .NET 4.5.

#### <a name="real-time-update-controller"></a>Echtzeit-Update-Controller

Die Facebook-Anwendungsvorlage kann Benutzer problemlos erstellen Sie eine Web-API-Controller um echtzeitupdates von Facebook zu behandeln. Wenn der Entwicklungscomputer hinter NAT ist, funktioniert möglicherweise Ihr Controller nicht ohne weitere Konfiguration des Netzwerks. Details finden Sie hier: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Abfrage-Zeichenfolgenwerte mit Facebook OAuth-Parameter in Konflikt stehen

Die folgenden Felder in Konflikt mit Facebook OAuth-Dialogfeld Aufruf URL zurück. Fügen Sie keine eigene Abfragezeichenfolgen-Werte mit den folgenden Namen: Code "," Fehler "," Fehler\_Beschreibung "," Fehler\_Grund.

#### <a name="using-page-inspector-with-facebook-template"></a>Verwenden die Seitenprüfung mit Facebook-Vorlage

Sie können nicht die Page Inspector-Funktion in Visual Studio 2012 verwenden, beim Debuggen einer Facebook-Anwendung. Der Seitenprüfung unterstützt keine Iframes augenblicklich nicht.

### <a name="single-page-application-template-issues"></a>Einseitige Anwendungsprobleme-Vorlage

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Mit JQuery 1.9/Knockout 2.2.1 Update, beim Ausführen von MVC SPA Standardprojekt, neue TODO-Element bearbeiten geben Focus-Ereignis ordnungsgemäß behandelt wird.

Mit JQuery 1.9/Knockout 2.2.1 aktualisieren, bei der Ausführung von MVC SPA Standardprojekt neue TODO-Element bearbeiten Geben Sie nicht mehr den Fokus an das neue Feld "TODO-Element bearbeiten" nach der Eingabe des neuen TODO-Elements der TODO-Liste.

Um Abhilfe Verweis [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html), und stellen Sie den folgenden Beispielcode ähnliche Fix:

Datei todo.model.js  
 todolist(data)-Funktion, fügen Sie folgenden:  
 **self.isSelected = ko.observable(false);**

todoList.prototype.addTodo-Funktion, fügen Sie den folgenden blacked Text hinzu:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Datei index.cshtml, fügen Sie den folgenden blacked Text hinzu:  
 &lt;bilden Sie Data-Bind =&quot;übermitteln: AddTodo&quot;&gt;  
 &lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;  
 &lt;/form&gt;
