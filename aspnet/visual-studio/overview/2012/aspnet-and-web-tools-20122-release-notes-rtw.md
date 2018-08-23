---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET und Web Tools 2012.2 – Anmerkungen zu dieser | Microsoft-Dokumentation
author: rick-anderson
description: Versionshinweise zu ASP.NET und Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: 0566a362b36f6cfb73b6479cd490e82c63455459
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838361"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET und Webtools 2012.2 – Anmerkungen dieser Version
====================
> Dieses Dokument beschreibt die Version von ASP.NET und Web Tools 2012.2. Es ist ein Update für Visual Studio Web-Tools und ASP.NET.


- [Installationshinweise](#_Installation)
- [Dokumentation](#_Documentation)
- [Unterstützung](#_Support)
- [Softwareanforderungen](#_Software_Requirements)
- [Neue Features in ASP.NET und Webtools 2012.2](#_New_Features_in)

    - [Tools](#_Tooling)
    - [Veröffentlichen im Web](#_Web_Publishing)
    - [ASP.NET MVC-Vorlagen](#_Templates)
    - [ASP.NET-Web-API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Bekannte Probleme und aktueller Änderungen](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET und Web Tools 2012.2 für Visual Studio 2012, können mithilfe von installiert werden [Webplattform-Installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Dies ist ein Update für Visual Studio 2012 oder Visual Studio Express 2012 für Web, die erforderlich ist. Wenn Sie nicht Visual Studio installiert haben, wird Visual Studio Express 2012 für das Web installiert.

Sie können auch ASP.NET und Web Tools 2012.2 manuell installieren. Sie benötigen Visual Studio 2012 oder Visual Studio Express 2012 für das Web installiert. Klicken Sie dann gehen Sie folgendermaßen vor: 

1. Herunterladen [ASP.NET und Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) Installer aus dem Download Center.
2. Klicken Sie nach Aufforderung auf bei der Ausführung. Sie können auch die Datei zur späteren Ausführung speichern.
3. Überprüfen Sie die Version von Visual Studio, die Sie ein update aus. Hierzu können Sie starten die Visual Studio, die Sie aktualisieren möchten. Klicken Sie dann auf das Menüelement Hilfe.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Wenn Sie sehen, dass das Menüelement &quot;zu Microsoft Visual Studio 2012 für Web&quot; dann herunterladen [Web Developer Tools 2012.2 – Visual Studio Express 2012 für Web](https://go.microsoft.com/fwlink/?LinkID=282228). Andernfalls laden [Web Developer Tools 2012.2 – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Klicken Sie nach Aufforderung auf bei der Ausführung. Sie können auch die Datei zur späteren Ausführung speichern.

> [!NOTE]
> ASP.NET und Web Tools 2012.2-Release umfasst keine SQL Server Data Tools. SQL Server und Windows Azure SQL-Datenbank bietet eine vielseitigere Palette an Tools, einschließlich der Projekt-gesicherten Offlineentwicklung, Schemavergleich und verbesserten Datenbank-Bereitstellungsfunktionen Datenbank. Weitere Informationen zum Installieren von SQL Server Data Tools finden Sie [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET und Web Tools 2012.2 ASP.NET-Website verfügbar sind ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Unterstützung

ASP.NET und Web Tools 2012.2 ist offiziell veröffentlicht und unterstützt. Sie können Ihre normale Support-Kanal verwenden. Sie können auch Fragen zu den Foren für ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), wobei Mitglieder der ASP.NET-Community informelle Unterstützung bieten. oftmals können sind.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET und Web Tools 2012.2 erfordert Visual Studio 2012 oder Visual Studio Express 2012 für das Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Neue Features in ASP.NET und Webtools 2012.2

Dieser Abschnitt beschreibt die Funktionen, die in der Version von ASP.NET und Web Tools 2012.2 eingeführt wurden.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tools

- Seitenprüfung 

    - Unterstützung von JavaScript-auswahlzuordnung ermöglicht der Seitenprüfung als Elemente zugeordnet werden, die die Seite wieder an die entsprechenden JavaScript-Code dynamisch hinzugefügt wurden.
    - Die Möglichkeit, die CSS-Aktualisierungen in Echtzeit finden Sie unter.
    - Weitere Informationen finden Sie [automatische CSS-Synchronisierung und JavaScript Auswahl zuordnen, in der Seitenprüfung](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Syntaxhervorhebung für CoffeeScript, Handlebars, Mustache und JsRender zu unterstützen.
    - Der HTML-Editor stellt Intellisense für die Knockout-Bindungen bereit.
    - Unterstützen zum Erstellen von dynamischen CSS mit weniger weniger bearbeiten und den Compiler.
    - Fügen Sie die JSON als .NET-Klasse. Mithilfe des folgenden Befehls spezielle fügen Sie zum Einfügen von JSON in die C#- oder VB.NET-Codedatei, und Visual Studio generiert automatisch .NET Klassen abgeleitet aus den JSON-Code.
- Unterstützung für Mobile-Emulator fügt erweiterungsmöglichkeiten, Drittanbieter-Emulatoren als einer VSIX-Datei installiert werden können. Die Emulatoren installierten werden, damit Entwickler ihre Websites auf einer Vielzahl von mobilen Geräten (Vorschau) können in der Dropdownliste "F5" angezeigt. Erfahren Sie mehr über dieses Feature in Scott Hanselmans Blogeintrag auf [der neuen BrowserStack-Integration mit Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Veröffentlichen im Web

- Website-Projekte weisen nun die gleichen Funktionen für die Veröffentlichung als ein Webanwendungsprojekt einschließlich der Veröffentlichung in Windows Azure-Websites.
- Selektive Veröffentlichung &#8211; für eine oder mehrere Dateien können Sie (nach der Veröffentlichung ein Web Deploy-Endpunkt) die folgenden Aktionen ausführen: 

    - Veröffentlichen Sie ausgewählte Dateien.
    - Finden Sie unter den Unterschied zwischen einer lokalen Datei und einer entfernten Datei.
    - Aktualisieren Sie die lokale Datei mit der remote-Datei, oder Aktualisieren der remote-Datei mit der lokalen Datei.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC-Vorlagen

- Die neue Vorlage für die Facebook-Anwendung ist das Schreiben von Facebook Canvas-Anwendungen einfach. In wenigen einfachen Schritten können Sie eine Facebook-Anwendung erstellen, die Ruft Daten aus einem angemeldeten Benutzer ab, und seine Freunde integriert. Die Vorlage enthält eine neue Bibliothek, die abnimmt, sind beim Erstellen einer Facebook-app, einschließlich Authentifizierung, Berechtigungen, die Zugriff auf Facebook-Daten und vieles mehr kümmern. Weitere Informationen zur Verwendung der Facebook-Application-Vorlage finden Sie unter [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Eine neue Vorlage für die einzelnen Seite Anwendung MVC ermöglicht Entwicklern das Erstellen von interaktiven clientseitigen Web-apps, die mit HTML 5, CSS 3 und den beliebten Knockout- und jQuery JavaScript-Bibliotheken über ASP.NET Web-API. Die Vorlage enthält eine "Todolist"-Anwendung, die zeigt, gängige Praktiken zum Erstellen einer JavaScript HTML5-Anwendung, die eine RESTful-Server-API verwendet. Weitere Informationen finden Sie unter [ https://www.asp.net/single-page-application ](../../../single-page-application/index.md).
- Sie können jetzt eine VSIX erstellen, die neue Vorlagen zum Dialogfeld "neue ASP.NET MVC-Projekt" hinzufügt. Erfahren Sie, wie hier: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes Paket &#8211; MVC-Projektvorlagen wurden aktualisiert, um das neue "FixedDisplayModes" NuGet-Paket zu enthalten, die eine problemumgehung für einen Fehler in MVC 4 enthält. Weitere Informationen zu den Fix im Paket enthaltenen, finden Sie in diesem Blogbeitrag ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) aus dem MVC-Team.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET Web-API verfügt über mehrere neue Features erweitert:

- OData der ASP.NET Web-API
- ASP.NET Web-API-Ablaufverfolgung
- ASP.NET Web-API-Hilfeseite

#### <a name="aspnet-web-api-odata"></a>OData der ASP.NET Web-API

ASP.NET Web API OData bietet Ihnen die Flexibilität zum Erstellen von OData-Endpunkte mit umfassenden Geschäftslogik für beliebige Datenquellen erforderlich. Mit ASP.NET Web API OData steuern Sie die Menge der OData-Semantik, die Sie verfügbar machen möchten. ASP.NET Web API OData ist in der ASP.NET MVC 4-Projektvorlagen enthalten und ist auch über NuGet verfügbar ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData unterstützt derzeit die folgenden Funktionen:

- Aktivieren Sie die Semantik der OData-Abfrage, indem dem [Queryable]-Attribut anwenden.
- Ganz einfach überprüfen Sie der OData-Abfragen aus, und schränken Sie den Satz von unterstützten Abfrageoptionen, Operatoren und Funktionen.
- Parameter binden ODataQueryOptions direkt zur eine abstrakte Syntax-Struktur-Darstellung der Abfrage abzurufen, die überprüft und einem "IQueryable" oder "IEnumerable" angewendet werden können.
- Aktivieren Sie dienstorientiertes Paging und linkgenerierung der nächsten Seite, indem Sie die Ergebnis-Grenzen für den [Queryable]-Attribut festlegen.
- Fordern Sie eine Inline-Anzahl der Gesamtzahl der übereinstimmenden Ressourcen, die mithilfe von $inlinecount.
- Null-Weitergabe zu steuern.
- Jede/alle Operatoren in $filter.
- Ableiten eines Entity Data Models gemäß der Konvention, oder passen Sie explizit ein Modell ähnlich wie auf Entity Framework Code First.
- Machen Sie Entitätenmengen durch das Ableiten vom entitysetcontroller erbt.
- Einfache, anpassbare Konventionen für das Verfügbarmachen von Navigationseigenschaften, Bearbeiten von Links aus, und Implementieren von OData-Aktionen.
- Vereinfacht das routing mithilfe der MapODataRoute-Erweiterungsmethode.
- Unterstützung für die versionsverwaltung durch das Verfügbarmachen von mehreren EDM-Modelle.
- Verfügbar machen Sie-dienstdokument und Atom $metadata damit Sie Clients (.NET, Windows Phone, Windows Store, usw.) generieren, können für Ihre Web-API.
- Unterstützung für die OData-Atom, JSON und JSON verbose-Formate.
- Erstellen, aktualisieren, teilweise zu aktualisieren (PATCH) und Löschen von Entitäten.
- Abfragen und Bearbeiten von Beziehungen zwischen Entitäten.
- Erstellen Sie Relationship-Links, die bis zu die Routen zu verknüpfen.
- Komplexe Typen
- Vererbung im Entity Typ.
- Auflistungseigenschaften.
- Enumerationen.
- OData-Aktionen.
- Baut auf derselben Grundlage wie WCF Data Services, d. h. ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Weitere Informationen zu ASP.NET Web API OData finden Sie unter [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web-API-Ablaufverfolgung

ASP.NET Web API Tracing integriert .NET Ablaufverfolgung Ablaufverfolgungsdaten aus Ihrer Web-APIs. Es ist jetzt in der Web-API-Projektvorlage standardmäßig aktiviert. Die Ablaufverfolgung für Ihre Web-APIs wird im Ausgabefenster gesendet und wird über IntelliTrace verfügbar gemacht. ASP.NET Web API Tracing können Sie Ablaufverfolgungsinformationen zu Ihrer Web-API, wenn Sie in Windows Azure gehostet, durch die Integration in [Windows Azure-Diagnose](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Sie können auch installieren und Aktivieren von ASP.NET Web API Tracing in jeder Anwendung, die mithilfe des ASP.NET Web API-Ablaufverfolgung NuGet-Pakets ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Weitere Informationen zum Konfigurieren und Verwenden von ASP.NET Web-API-Ablaufverfolgung finden Sie unter [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web-API-Hilfeseite

Die ASP.NET Web API-Hilfeseite ist jetzt standardmäßig in der Web-API-Projektvorlage enthalten. Die ASP.NET Web API-Hilfeseite generiert automatisch die Dokumentation für Web-APIs, z.B. die HTTP-Endpunkte, unterstützten HTTP-Methoden, Parameter und Beispiel Anforderungs- und antwortnutzlasten der Nachricht. Dokumentation automatisch von Kommentaren in Ihrem Code per Pull abgerufen wird. Sie können auch die ASP.NET Web API-Hilfeseite an eine beliebige Anwendung, die mithilfe des ASP.NET Web API Help Page NuGet-Pakets hinzufügen ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Weitere Informationen zum Einrichten und Anpassen von der ASP.NET Web API-Hilfeseite finden Sie unter [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR vereinfacht Hinzufügen von Echtzeit-Webfunktionen-Funktionen zu Ihrer ASP.NET-Anwendung mithilfe von WebSockets, falls verfügbar und automatisch Fallback auf andere Verfahren, wenn dies nicht der Fall.

Weitere Informationen zur Verwendung von ASP.NET SignalR finden Sie unter [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs erleichtert für Web Forms-Entwickler zum Generieren von URLs cleaner (ohne die ASPX-Erweiterung) zu suchen. Es ist wenig keine Konfiguration erforderlich und kann mit vorhandenen ASP.NET v4. 0-Anwendungen verwendet werden. Das Feature FriendlyURLs erleichtert auch die Entwickler ihre Anwendungen durch die Unterstützung von Wechseln zwischen Desktop- und mobile Ansichten mobile-Unterstützung hinzugefügt.

Weitere Informationen zur Installation und Verwendung von Friendly URLs von ASP.NET finden Sie unter [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und Änderungen, die in der Version von ASP.NET und Web Tools 2012.2 sind.

### <a name="installation-issues"></a>Probleme bei der Installation

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Außerhalb der Reihenfolge Installationen von Visual Studio 2012

Installieren eine zusätzliche SKU von Visual Studio 2012, nach der Installation von ASP.NET und Web Tools 2012.2 ein Reparaturvorgangs benötigen. Betrachten Sie die folgende Sequenz:

1. Installieren von Visual Studio 2012 Express für Web
2. Installieren von ASP.NET und Webtools 2012.2
3. Installieren Sie Visual Studio 2012 Professional, Premium oder Ultimate

Schritt 2 würde nur Installieren von Updates für Express für Web. Um sicherzustellen, dass die zusätzliche SKU, die in Schritt 3 installiert das Update enthält, müssen Sie reparieren Sie die ASP.NET und Web Tools 2012.2, zum Installieren der Updates für die letzten-SKU installiert. Dies gilt auch, wenn die SKUs in Schritt 1 und 3 rückgängig gemacht werden.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Microsoft ASP.NET und Web Tools 2012.2 zu installieren, wenn Visual Studio geöffnet ist.

Wenn Visual Studio während der Installation von Microsoft ASP.NET und Web Tools 2012.2 geöffnet ist, kann Visual Studio in einem fehlerhaften Zustand kommen. Es empfiehlt sich, dass Benutzer über alle Instanzen von Visual Studio, bevor Sie mit der Installation schließen.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Abbrechen von ASP.NET und Web Tools 2012.2-Setup in der Mitte der installation

Abbrechen von ASP.NET und Web Tools 2012.2 wird von Setup in der Mitte Installation Visual Studio in einem fehlerhaften Zustand beibehalten. Um dieses Problems führen Sie diese Schritte beheben: 

- Wechseln Sie zu Programme.
- Deinstallieren Sie Microsoft ASP.NET und Web Tools 2012.2, falls vorhanden.
- Installieren von Microsoft ASP.NET und Webtools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Nach der Deinstallation von ASP.NET- und Web Tools 2012.2 der ASP.NET MVC 4 fehlen Vorlagen und Razor v2-Website-Vorlagen

Deinstallieren von ASP.NET und Web Tools 2012.2 werden alle ASP.NET MVC 4 und Razor v2-Website-Vorlagen auch in Visual Studio 2012 deinstallieren.

Die problemumgehung besteht darin, die Visual Studio 2012-Installation zur Neuinstallation des ASP.NET MVC 4 und Razor v2-Websitevorlagen zu reparieren.

### <a name="tooling-issues"></a>Probleme mit Tools

#### <a name="nuget-error-reported-during-project-creation"></a>NuGet-Fehler gemeldet wird, während der projekterstellung

Nach der Installation von ASP.NET und Web Tools 2012.2 können Sie die folgende Fehlermeldung angezeigt, beim Erstellen eines MVC 4-Projekts

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

Die ASP.NET und Web Tools 2012.2 ausgeliefert wird NuGet 2.1 und aktualisiert die Erweiterung in Visual Studio 2012. In einigen Fällen kann das VSIX-Installationsprogramm nicht ordnungsgemäß aktualisiert, die VSIX-Datei. Die folgenden Schritte können Sie dieses Problem zu beheben:

1. Starten Sie Visual Studio 2012 als Administrator
2. Wechseln Sie zum Tools -&gt;Erweiterungen und Updates und NuGet zuerst deinstallieren.
3. Schließen Sie Visual Studio.
4. Navigieren Sie zu dem ASP.NET- und Web Tools 2012.2-Installationsordner:

    1. Für Visual Studio 2012: **Programmieren c:\Programme\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Für Visual Studio 2012 Express für Web: **Programme\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 für Web**
5. Klicken Sie mit der Doppelklicken auf die NuGet.Tools.vsix NuGet erneut installieren.

### <a name="web-api-issues"></a>Web-API-Probleme

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analysieren von Problemen in $filter und DateTime-literalen

Der OData-URI-Parser kann nicht ordnungsgemäß analysiert werden teilweise Datetime-Literalen. Z. B. $filter = Start-Eq "DateTime" "2012-12-31T12:00' kann nicht ordnungsgemäß analysiert werden. Eine problemumgehung ist, verwenden Sie die vollständige Zeichenliteral $filter = Start-Eq "DateTime" "2012-12-31T12:00:00".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData unterstützt keine Groß-/Kleinschreibung Eigenschaftennamen.

OData unterstützt keine Groß-/Kleinschreibung Eigenschaftennamen in OData-Abfragen und Odata-Pfad. Finden Sie unter Arbeitselemente:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Wenn Benutzer unterschiedlicher Groß-/Kleinschreibung auf Javascript clientseitige und serverseitige haben, werden sie wahrscheinlich dieses Problem auftritt. Dieses Problem ist mit Absicht in Odata-Protokoll. Meldet jedoch viele Benutzer dieses Problem. Dieses Problem, zu umgehen müssen Benutzer ihre Groß-/Kleinschreibung in der URL zu korrigieren.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Standardmäßige OData-Konventionen routing unterstützt keine POST/PUT für Navigationseigenschaft.

Standardmäßige OData-Konventionen routing unterstützt keine POST/PUT für Navigationseigenschaft. Finden Sie unter Workitem [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Wir werden diese häufig verwendete Konvention Standardkonventionen fehlt.

Dieses Problem, zu umgehen müssen Benutzer neue routing-Konvention, um Unterstützung zu erweitern.

### <a name="facebook-template-issues"></a>Facebook-Vorlagenprobleme

#### <a name="facebook-application-template-only-works-using-net-45"></a>Facebook-Anwendungsvorlage funktioniert nur mit .NET 4.5

Sie müssen .NET 4.5 auswählen, in der Dropdownliste "Framework" in das Dialogfeld "Neues Projekt" auf die Vorlage der Facebook-Anwendung in ASP.NET MVC 4 finden Sie unter.

#### <a name="real-time-update-controller"></a>Real-Time-Update-Controller

Die Facebook-Anwendungsvorlage kann Benutzer ganz einfach erstellen Sie eine Web-API-Controller, um Aktualisierungen in Echtzeit über Facebook zu behandeln. Wenn es sich bei Ihrem Entwicklungscomputer hinter einer NAT befindet, funktioniert möglicherweise Ihres Controllers nicht ohne weitere Konfiguration des Netzwerks. Einzelheiten finden Sie hier: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Abfrage-Zeichenfolgenwerte mit Facebook OAuth-Parameter in Konflikt stehen

Die folgenden Felder in Konflikt mit Facebook OAuth Dialogfeld Aufruf URL sichern. Fügen Sie keine eigene Abfragezeichenfolgen-Werte mit den folgenden Namen: Code "," Fehler "," Fehler\_Beschreibung "," Fehler\_Grund.

#### <a name="using-page-inspector-with-facebook-template"></a>Verwenden der Seitenprüfung mit Facebook-Vorlage

Sie können nicht das Feature der Seitenprüfung in Visual Studio 2012 verwenden, während des Debuggens Ihrer Facebook-Anwendung. Die Seitenprüfung unterstützt derzeit keine Iframes.

### <a name="single-page-application-template-issues"></a>Einseitige Anwendung Vorlagenprobleme

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Fokusereignis wird mit JQuery 2.2.1 1.9/Knockout-Update, bei der Ausführung von Standard-MVC-SPA-Projekt neu Bearbeiten der Todo-Element geben Sie nicht ordnungsgemäß verarbeitet.

Mit JQuery 1.9/Knockout 2.2.1 aktualisiert werden, beim Standard-MVC-SPA-Projekt ausführen, neue Todo-Elements bearbeiten geben nicht mehr den Fokus wieder für das Bearbeiten neue Todo-Element nach dem das neue Todo-Element der Todo-Liste eingeben.

Problemumgehung Verweis [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html), und nehmen Sie ähnliche Korrektur an der folgende Code:

Datei todo.model.js  
 todolist(data) funktioniert, fügen Sie folgenden:  
 **self.isSelected = ko.observable(false);**

todoList.prototype.addTodo funktioniert, fügen Sie den folgenden blacked Text hinzu:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Datei "Index.cshtml", fügen Sie den folgenden blacked Text hinzu:  
 &lt;Form-Data-Bind =&quot;übermitteln: AddTodo&quot;&gt;  
 &lt;Geben Sie die Klasse =&quot;AddTodo&quot; Typ =&quot;Text&quot; Data-Bind =&quot;Wert: NewTodoTitle, Platzhalter: "Typ hier hinzufügen", BlurOnEnter: "true" **"HasFocus": IsSelected**, Ereignis: {blur: AddTodo}&quot; /&gt;  
 &lt;/form&gt;
