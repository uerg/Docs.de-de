---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET und Webtools für Visual Studio 2013-Versionshinweise | Microsoft Docs
author: microsoft
description: Dieses Dokument beschreibt die Version von ASP.NET und Webtools für Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877800"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET und Webtools für Visual Studio 2013-Versionshinweise
====================
durch [Microsoft](https://github.com/microsoft)

> Dieses Dokument beschreibt die Version von ASP.NET und Webtools für Visual Studio 2013.


## <a name="contents"></a>Inhalt

- [Hinweise zur installationsnachbereitung](#TOC1)
- [Dokumentation](#TOC2)
- [Softwareanforderungen](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Funktionen in ASP.NET und Webtools für Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Neue Web Project-Benutzeroberfläche](#newproj)
- [ASP.NET Gerüstbau](#scaffold)
- [Browserverknüpfung](#browser-link)
- [Web-Editor von Visual Studio-Erweiterungen](#web-editor)
- [Azure App Service-Web-Apps-Support in Visual Studio](#waws)
- [Web veröffentlichen Erweiterungen](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Microsoft owin-Komponenten](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET App Suspend](#TOC15)
- [Bekannte Probleme und aktueller Änderungen](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

ASP.NET und Webtools für Visual Studio 2013 können heruntergeladen werden und werden in der main-Installationsprogramm gebündelt [hier](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET und Webtools für Visual Studio 2013 stehen über die [ASP.NET-Website](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET und Webtools erfordert Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Funktionen in ASP.NET und Webtools für Visual Studio 2013

Die folgenden Abschnitte beschreiben die Funktionen, die in der Version eingeführt wurden.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Mit der Version von Visual Studio 2013 haben wir einen Schritt in Richtung zusammen, die Verwendung von ASP.NET-Technologien, sodass Sie problemlos mischen und Zuordnen der gewünschten ausgeführt. Beispielsweise können Sie Starten eines Projekts mit MVC und bequem später Web Forms-Seiten zum Projekt hinzufügen oder Web-APIs in einer Web Forms-Projekt zu erstellen. Eine ASP.NET ist leichter Sie als Entwickler Schritte auszuführen, die Ihnen in ASP.NET gefallen. Unabhängig davon, welche Technologie Sie wählen, haben Sie mehr Gewissheit, die Sie auf die vertrauenswürdige zugrunde liegende Framework von einer ASP.NET erstellen.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Neue Web Project-Benutzeroberfläche

Wir haben die Funktion zum Erstellen neuer Webprojekte in Visual Studio 2013 erweitert. In der **neuen ASP.NET-Webprojekts** Dialogfeld können Sie den Projekttyp werden soll, konfigurieren eine beliebige Kombination aus Technologien (Web Forms, MVC, Web-API), konfigurieren Sie Authentifizierungsoptionen und fügen Sie ein Komponententestprojekt hinzu auswählen.

![Neues ASP.NET-Projekt](release-notes/_static/image1.png)

Das Dialogfeld "neue" können Sie die standardmäßigen Authentifizierungsoptionen für viele der Vorlagen ändern. Z. B. beim Erstellen einer ASP.NET Web Forms-Projekt können Sie eine der folgenden Optionen auswählen:

- Keine Authentifizierung
- Einzelne Benutzerkonten (ASP.NET-Mitgliedschaft oder soziale Anbieter anzumelden)
- Organisations-Konten (Active Directory in einer internetanwendung)
- Windows-Authentifizierung (Active Directory in einem Intranetanwendung)

![-Authentifizierungsoptionen.](release-notes/_static/image2.png)

Weitere Informationen zu den neuen Prozess zum Erstellen von Webprojekten, finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Weitere Informationen zu den neuen Authentifizierungsoptionen finden Sie unter [ASP.NET Identity](#TOC8) weiter unten in diesem Dokument.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Gerüstbau

ASP.NET Gerüstbau ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen. Es vereinfacht den Standardcode zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit Visual Studio 2013 können Sie jetzt Gerüst für eine beliebige ASP.NET-Projekt, einschließlich Web Forms verwenden. Visual Studio 2013 unterstützt zurzeit nicht Generieren von Seiten für eine Web Forms-Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie das Projekt MVC-Abhängigkeiten hinzufügen. Unterstützung für das Generieren von Seiten für Web Forms wird in einem zukünftigen Update hinzugefügt werden.

Mithilfe von Gerüstbau stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einer ASP.NET Web Forms-Projekt beginnen und dann mithilfe von Gerüstbau einer Web-API-Controller hinzufügen, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

Um Gerüstbau für MVC eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für MVC Gerüstbau; Minimal "und" vollständig ". Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Wenn Sie die Option "vollständig", wählen Sie die minimale Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für asynchrone Controller Gerüstbau verwendet die neue asynchrone Funktionen von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browserlink – SignalR-Kanal zwischen dem Browser und Visual Studio

Die neue [Browserlink](using-browser-link.md) Funktion können Sie Verbinden von mehreren Browsern mit Visual Studio und alle durch Klicken auf eine Schaltfläche auf der Symbolleiste zu aktualisieren. Verbinden von mehreren Browsern mit Ihrer Entwicklungswebsite, einschließlich mobile Emulatoren aus, können und klicken Sie auf aktualisieren, aktualisieren Sie alle zur selben Zeit aller Browser. Browserlink stellt außerdem eine API, um Entwicklern ermöglichen, Schreiben von Browserlink-Erweiterungen zur Verfügung.

![](release-notes/_static/image3.png)

Da der Entwickler die Browserlink-API nutzen, wird es möglich ist, erstellen sehr komplexen Szenarios, die über mehrere zwischen Visual Studio und jeden beliebigen Browser, der verbunden ist. Web Essentials nutzt die-API, um die optimale Integration zwischen Visual Studio und den Browser Entwicklertools, remote steuern von mobile-Emulatoren und vieles mehr erstellen.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Web-Editor von Visual Studio-Erweiterungen

Visual Studio 2013 umfasst einen neue HTML-Editor für den Razor-Dateien und HTML-Dateien in Webanwendungen. Neue HTML-Editor bietet eine einheitliche einzelschema basierend auf HTML5. Es bietet automatische geschweifte Klammer Abschluss, jQuery UI und AngularJS-Attribut IntelliSense,-Attribut IntelliSense Gruppierung, ID und Klassennamen Intellisense und weitere Verbesserungen, z. B. eine bessere Leistung, Formatierung und SmartTags.

Das folgende Bildschirmfoto zeigt die Bootstrap-Attribut IntelliSense in der HTML-Editor verwenden.

![IntelliSense in HTML-editor](release-notes/_static/image4.png)

Visual Studio 2013 gehören auch mit beiden CoffeeScript und weniger Editoren integriert. Das LESS-Editor mit allen beeindruckenden Funktionen aus den CSS-Editor stammt, und verfügt über bestimmte Intellisense für Variablen und Mixins über die weniger Dokumente in der @import Kette.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Azure App Service-Web-Apps-Support in Visual Studio

In Visual Studio 2013 mit dem Azure SDK für .NET 2.2 enthält, können Sie **Server-Explorer** direkt mit der remote-Web-apps interagieren. Sie können sich Ihr Azure-Konto anmelden. neue Web-apps erstellen, Konfigurieren von apps, echtzeitprotokolle und vieles mehr anzeigen. Verarbeitet, die kurz nach SDK 2.2 veröffentlicht wird, im Debugmodus befinden, Remote in Azure ausführen können. Die meisten neuen Funktionen für Azure App Service-Web-Apps funktionieren auch in Visual Studio 2012, wenn Sie die aktuelle Version des Azure SDK für .NET installieren.

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Erstellen einer ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web veröffentlichen Erweiterungen

Visual Studio 2013 enthält neue und verbesserte Web Publish-Funktionen. Hier sind einige davon:

- Einfache [automatisieren "Web.config" dateiverschlüsselung](https://go.microsoft.com/fwlink/?LinkId=325529). (Dieser Link und die folgenden beiden zeigen Sie auf Dokumentation auf MSDN, die möglicherweise nicht in den Tag 10/17 spät erst verfügbar.)
- Einfache [automatisieren eine Anwendung offline, während der Bereitstellung dauert](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfigurieren von Web Deploy, um [verwenden Sie die Datei Prüfsumme statt zuletzt geändert am](https://go.microsoft.com/fwlink/?LinkId=325531) um zu bestimmen, welche Dateien an den Server kopiert werden sollen.
- Veröffentlichen Sie schnell einzelnen ausgewählte Dateien (einschließlich der Datei "Web.config") auf, wenn Sie, die das FTP verwenden oder Dateisystem veröffentlichen Methoden als auch mit Web Deploy.

Weitere Informationen zur Bereitstellung von ASP.NET finden Sie unter [der ASP.NET-Website](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 enthält einen umfangreichen Satz von neuen Features, die beschrieben werden ausführlich auf [NuGet 2.7-Anmerkungen zu dieser Version](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Diese Version von NuGet entfernt auch die Notwendigkeit, bieten explizite Zustimmung für NuGet Paket-Wiederherstellungsfunktion Pakete herunterladen. Zustimmung (und die zugehörigen Kontrollkästchen im Dialogfeld "NuGet Einstellungen") werden nun durch die Installation von NuGet gewährt. Paketwiederherstellung funktioniert nun einfach standardmäßig.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

### <a name="one-aspnet"></a>One ASP.NET

Die Web Forms-Projektvorlagen nahtlose Integration in die neue Umgebung für eine ASP.NET. Sie können hinzufügen, MVC und Web-API unterstützen, dem Web Forms-Projekt, und Sie können Authentifizierung mit der ein ASP.NET Projekt Fertigstellen des Assistenten konfigurieren. Weitere Informationen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die Web Forms-Projektvorlagen unterstützen das neue ASP.NET Identity Framework. Darüber hinaus unterstützen die Vorlagen jetzt die Erstellung eines Projekts Intranet Web Forms. Weitere Informationen finden Sie unter [Authentifizierungsmethoden](creating-web-projects-in-visual-studio.md#auth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Verwenden Sie die Web Forms-Vorlagen [Bootstrap](http://twitter.github.io/bootstrap/) , ein schlankes und reaktionsschnelle Aussehen und Verhalten bereitzustellen, die Sie leicht anpassen können. Weitere Informationen finden Sie unter [Bootstrap in Visual Studio 2013 Webprojektvorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Die Web-MVC-Projektvorlagen nahtlose Integration in die neue Umgebung für eine ASP.NET. Sie können das MVC-Projekt anpassen und konfigurieren Sie die Authentifizierung mit dem Assistenten für eine ASP.NET erstellen. Ein einführendes Lernprogramm zu ASP.NET MVC 5 finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die MVC-Projektvorlagen wurden aktualisiert, um ASP.NET Identity für die Authentifizierung und identitätsverwaltung verwenden. Ein Lernprogramm mit Facebook und Google-Authentifizierung und die neue Mitgliedschafts-API finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) und [erstellen Sie eine ASP.NET MVC-Anwendung mit Auth und SQL-Datenbank und in Azure App Service bereitstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Die MVC-Projektvorlage wurde aktualisiert, um verwenden [Bootstrap](http://getbootstrap.com/) , ein schlankes und reaktionsschnelle Aussehen und Verhalten bereitzustellen, die Sie leicht anpassen können. Weitere Informationen finden Sie unter [Bootstrap in Visual Studio 2013 Webprojektvorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Authentifizierungsfilter

Authentifizierungsfilter sind eine neue Art von Filter in ASP.NET MVC, die vor dem Autorisierungsfilter in der ASP.NET MVC-Pipeline ausgeführt werden und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben Sie einen entsprechenden Prinzipal. Authentifizierungsfilter können auch authentifizierungsaufforderungen als Antwort auf nicht autorisierte Anfragen hinzufügen.

### <a name="filter-overrides"></a>Filtern von Außerkraftsetzungen

Sie können jetzt Überschreiben der Filter an eine bestimmte Aktion-Methode oder der Controller anwenden, indem Sie einen Außerkraftsetzung Filter angeben. Außerkraftsetzung Filter geben Sie einen Satz von Typen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie so konfigurieren Sie Filter, die global angewendet, aber dann Ausschließen bestimmter globale Filter nicht auf bestimmte Aktionen oder Controllern angewendet.

### <a name="attribute-routing"></a>Attributrouting

ASP.NET MVC unterstützt jetzt die routing-Attribut, Dank einen Beitrag von Tim McCall der Autor [ http://attributerouting.net ](http://attributerouting.net). Mit routing-Attribut können Sie Ihre Routen angeben, durch Hinzufügen einer Anmerkung zu Ihrer Aktionen und Controller.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET-Web-API 2

### <a name="attribute-routing"></a>Attributrouting

ASP.NET Web-API unterstützt jetzt die routing-Attribut, Dank einen Beitrag von Tim McCall der Autor [ http://attributerouting.net ](http://attributerouting.net). Mit routing-Attribut können Sie Ihre Web-API-Routen Hinzufügen einer Anmerkung zu Ihrer Aktionen und Controller wie folgt angeben:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Routing-Attribut bietet Ihnen mehr Kontrolle über die URIs in Ihre Web-API. Beispielsweise können Sie problemlos mit einem einzigen API-Controller-Ressourcenhierarchie definieren:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Auch routing-Attribut enthält eine zweckmäßigen Syntax zum Angeben Optionaler Parameter, Standardwerte und routeneinschränkungen:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Weitere Informationen zu routing-Attribut, finden Sie unter [Routing-Attribut in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Die Web-API und einseitigen Anwendung Projektvorlagen unterstützen nun mithilfe von OAuth 2.0 Autorisierung. OAuth 2.0 ist ein Framework für die Autorisierung des Clientzugriff auf geschützte Ressourcen. Dies funktioniert für eine Vielzahl von Clients einschließlich Browsern und mobilen Geräten.

Unterstützung für OAuth 2.0 basiert auf neuen Sicherheit Middleware durch den Microsoft owin-Komponenten für trägerauthentifizierung bereitgestellt, und Implementieren der Autorisierung-Serverrolle. Alternativ können die Clients mit einem organisationsbezogenen autorisierungsserver, z. B. Azure Active Directory oder AD FS in Windows Server 2012 R2 autorisiert werden.

### <a name="odata-improvements"></a>OData-Verbesserungen

**Unterstützung für $select $ $batch und $value erweitern**

ASP.NET Web API OData bietet jetzt vollständige Unterstützung für $select, $expand, und $value. Sie können auch $batch für Anforderung Batchverarbeitung und Verarbeitung von Changesets.

Der $select und $expand-Optionen können Sie ändern die Form der Daten, die von einem OData-Endpunkt zurückgegeben werden. Weitere Informationen finden Sie unter [Introducing $select und $expand-Unterstützung in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Verbesserte Erweiterbarkeit**

Die OData-Formatierer sind jetzt erweiterbar. Sie können Atom-Eintrag-Metadaten hinzufügen, unterstützen benannte Stream und Media-Link-Einträge instanzanmerkungen hinzufügen und Anpassen der zum Generieren von Links.

**Typ-kleiner-Unterstützung**

Sie können nun die OData-Dienste erstellen, ohne CLR-Typen für die Entitätstypen zu definieren. Stattdessen können die OData-Controller dauern oder Instanzen zurückgeben, **IEdmObject**, was sind die OData-Formatierer Serialisierung/Deserialisierung.

**Wiederverwenden eines vorhandenen Modells**

Wenn Sie bereits vorhandene Entity Data Model (EDM) verfügen, können Sie jetzt es direkt wiederverwenden anstatt ein neues Konto erstellen. Bei Verwendung von Entity Framework können Sie z. B. EDM verwenden, in dem EF für Sie erstellt.

### <a name="request-batching"></a>Anfordern der Batchverarbeitung

Anforderung Batchverarbeitung werden mehrere Vorgänge in einer HTTP POST-Anforderung, um den Netzwerkverkehr zu reduzieren, und geben Sie einen glatter, weniger ' geschwätzige ' Benutzeroberfläche kombiniert. ASP.NET Web-API unterstützt jetzt mehrere Strategien für die Batchverarbeitung der Anforderung:

- Verwenden Sie die $batch Endpunkt von einem OData-Dienst.
- Verpacken Sie mehrere Anforderungen in einer einzelnen MIME-multipart-Anforderung.
- Verwenden Sie ein benutzerdefiniertes Format für die Batchverarbeitungs.

Um die Anforderung, die Batchverarbeitung zu aktivieren, fügen Sie einfach eine Route mit einem Batchverarbeitung Handler Ihrer Web-API-Konfiguration:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Sie können auch steuern, ob Anforderungen oder sequenziell oder in einer beliebigen Reihenfolge ausgeführt.

### <a name="portable-aspnet-web-api-client"></a>Portable ASP.NET Web-API-Client

Der ASP.NET Web API-Client können jetzt erstellen portable Klassenbibliotheken, die Arbeit über Ihre Windows Store und Windows Phone 8-Anwendungen. Sie können auch portable Formatierungsprogramme erstellen, die für Client und Server freigegeben werden können.

### <a name="improved-testability"></a>Verbesserte Prüfbarkeit

Web-API 2 macht es viel einfacher, Einheit testen Sie die API-Controller. Nur instanziieren Sie Ihrer API-Controller mit die Anforderungsnachricht und die Konfiguration, und rufen Sie anschließend die Aktionsmethode, die Sie testen möchten. Es ist auch einfach zum Simulieren der **UrlHelper** Klasse, für Aktionsmethoden, die linkgenerierung ausführen.

### <a name="ihttpactionresult"></a>IHttpActionResult

Sie können jetzt IHttpActionResult, um das Ergebnis Ihrer Web-API-Aktionsmethoden zu kapseln implementieren. Ein von einer Web-API-Aktion-Methode zurückgegebenen IHttpActionResult wird ausgeführt, von der Laufzeit ASP.NET Web-API, um den resultierenden Antwortnachricht zu erzeugen. Ein IHttpActionResult zurückgegeben werden kann, in einer beliebigen Web-API-Aktion zur Vereinfachung der Einheit Testen Ihrer Web-API-Implementierung. Der Einfachheit halber eine Reihe von IHttpActionResult Implementierungen bereitgestellt werden, einschließlich der Ergebnisse für bestimmte Rückgabestatuscodes ausgegeben formatiert Content und Aushandeln mit Inhalt-Antworten.

### <a name="httprequestcontext"></a>HttpRequestContext

Die neue **HttpRequestContext** verfolgt alle Status, die an die Anforderung gebunden ist, aber ist nicht in der Anforderung sofort verfügbar. Beispielsweise können Sie der **HttpRequestContext** zum Abrufen von Routendaten, den Prinzipal, der die Anforderung das Clientzertifikat zugeordnet der **UrlHelper** und Stamm des virtuellen Pfads. Lässt sich leicht erstellen eine **HttpRequestContext** für Komponententests Testzwecke verwenden.

Weil der Prinzipal für die Anforderung, mit der Anforderung weitergegeben wird anstelle des **Thread.CurrentPrincipal**, der Prinzipal ist jetzt verfügbar, während der gesamten Lebensdauer der Anforderung, während sie in der Web-API-Pipeline ist.

### <a name="cors"></a>CORS

Unser Dank gilt eine andere hervorragende Beitrag Brock Abläufe unterstützt ASP.NET jetzt vollständig Cross Origin anfordern Sharing (CORS).

Browsersicherheit wird verhindert, dass eine Webseite, die AJAX-Anforderungen in eine andere Domäne. [CORS](http://www.w3.org/TR/cors/) ist ein W3C-Standard, der einem Server ermöglicht, die gleichen-Origin-Richtlinie weniger streng gehandhabt. Verwenden CORS, können ein Servers explizit einige Cross-Origin-Anforderungen beim Ablehnen von anderen Benutzern.

Web-API 2 unterstützt jetzt CORS, einschließlich der automatischen Bearbeitung von preflight-Anforderungen. Weitere Informationen finden Sie unter [Cross-Origin-Anforderungen aktivieren, in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Authentifizierungsfilter

Authentifizierungsfilter sind eine neue Art von Filter in ASP.NET Web-API, die vor dem Autorisierungsfilter in der ASP.NET Web API-Pipeline ausgeführt werden und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben Sie einen entsprechenden Prinzipal. Authentifizierungsfilter können auch authentifizierungsaufforderungen als Antwort auf nicht autorisierte Anfragen hinzufügen.

### <a name="filter-overrides"></a>Filtern von Außerkraftsetzungen

Sie können jetzt überschreiben an eine bestimmte Aktion-Methode oder der Controller, welche Filter anwenden, indem Sie einen Außerkraftsetzung Filter angeben. Außerkraftsetzung Filter geben Sie einen Satz von Typen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie globale Filter hinzufügen, aber schließen Sie einige Geräte werden über bestimmte Aktionen oder Controllern.

### <a name="owin-integration"></a>OWIN-Integration

ASP.NET Web API nun vollständig unterstützt OWIN und kann auf alle OWIN-fähigen Hosts ausgeführt werden. Ebenfalls enthalten sind ein **HostAuthenticationFilter** , Integration in die OWIN-Authentifizierungssystem bereitstellt.

OWIN-Integration können Sie Web-API in Ihrem eigenen Prozess zusammen mit anderen OWIN-Middleware, z. B. SignalR selbst hosten. Weitere Informationen finden Sie unter [verwenden OWIN Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

In den folgenden Abschnitten werden die Funktionen von SignalR 2.0 beschrieben.

- [Basiert auf OWIN](#builtonowin)
- [MapHubs und MapConnection sind jetzt MapSignalR](#MapSignalR)
- [Cross-Domain-Unterstützung](#crossdomain)
- [iOS und Android-Unterstützung über MonoTouch und MonoDroid](#mobile)
- [Portable .NET Client](#portable)
- [Neues Selbsthosting-Paket](#selfhost)
- [Abwärtskompatible serverunterstützung](#backwardcompat)
- [Server-Unterstützung für .NET 4.0 entfernt](#remove40)
- [Senden einer Nachricht an eine Liste von Clients und Gruppen](#messagelist)
- [Senden einer Nachricht an einen bestimmten Benutzer](#sendtouser)
- [Eine bessere Unterstützung der Fehler behandeln](#errorhandling)
- [Einfacher Einheit von Hubs testen](#unittesting)
- [JavaScript-Fehlerbehandlung](#javascripterror)

Ein Beispiel dafür, wie ein vorhandenes 1.x-Projekt auf SignalR 2.0 aktualisieren, finden Sie unter [Aktualisieren einer SignalR 1.x Projekt](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basiert auf OWIN

SignalR 2.0 basiert vollständig auf [OWIN (Open Weboberfläche für .NET)](http://owin.org/). Diese Änderung wird von der Setup-Prozess für SignalR-weitaus Web gehostet und selbst gehostete SignalR-Anwendungen konsistent, aber Sie hat auch eine Reihe von API-Änderungen erforderlich.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs und MapConnection sind jetzt MapSignalR

Aus Kompatibilitätsgründen mit OWIN-Standards wurden diese Methoden um umbenannt `MapSignalR`. `MapSignalR` wird aufgerufen, ohne Parameter alle Hubs zugeordnet werden kann (als `MapHubs` ist in Version 1.x); um einzelne zuordnen **"persistentconnection"** Objekte, geben Sie die Verbindung als der Typparameter und die URL-Erweiterung für die Verbindung als der Erstes Argument.

Die `MapSignalR` Methode wird aufgerufen, in einer Owin-Start-Klasse. Visual Studio 2013 enthält eine neue Vorlage für eine Owin-Start-Klasse. Diese Vorlage verwenden möchten, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf das Projekt
2. Wählen Sie **hinzufügen**, **neues Element...**
3. Wählen Sie **Owin Startklasse**. Benennen Sie die neue Klasse **Startup.cs**.

In einer **-Webanwendung** der owin-Start-Klasse, enthält die `MapSignalR` Methode wird dann an Owins-Startprozess mit einem Eintrag im Knoten "Application Settings" die Datei "Web.config" hinzugefügt, wie unten dargestellt.

In einem **selbst gehosteten Anwendung**, die Startklasse wird übergeben, wie der Typparameter der `WebApp.Start` Methode.

**Zuordnen von Hubs und Verbindungen in SignalR 1.x (aus der globalen Anwendungsdatei in einer Webanwendung):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Zuordnen von Hubs und Verbindungen in SignalR 2.0 (über eine Owin-Start-Klassendatei):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In einer **selbst gehosteten Anwendung**, die Startklasse wird übergeben, wie der Typparameter für die `WebApp.Start` Methode, wie unten dargestellt.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Cross-Domain-Unterstützung

Im SignalR wurden von einem einzelnen EnableCrossDomain Flag 1.x, domänenübergreifende Anforderungen gesteuert. Dieses Flag gesteuert, JSONP und CORS-Anforderungen. Für größere Flexibilität, die alle CORS-Unterstützung die Serverkomponente von SignalR entzogen wurde (JavaScript Lients weiterhin verwenden CORS normalerweise, wenn es erkannt wird, dass der Browser unterstützt), und neue OWIN-Middleware zur Unterstützung dieser Szenarien verfügbar gemacht wurde.

In SignalR 2.0 Wenn JSONP auf dem Client (zur Unterstützung von älteren Browsern domänenübergreifende Anforderungen) erforderlich ist, muss diese explizit aktiviert werden, indem die Einstellung `EnableJSONP` auf die `HubConfiguration` -Objekt `true`, wie unten dargestellt. JSONP ist standardmäßig deaktiviert, da es weniger sicher als CORS ist.

Um die neue CORS-Middleware in SignalR 2.0 hinzuzufügen, fügen die `Microsoft.Owin.Cors` Bibliothek zum Projekt, und rufen `UseCors` vor Ihrer SignalR-Middleware, wie im Abschnitt weiter unten dargestellt.

**Hinzufügen von Microsoft.Owin.Cors zu Ihrem Projekt**: Führen Sie den folgenden Befehl in der Paket-Manager-Konsole, um diese Bibliothek zu installieren:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Mit diesem Befehl wird die 2.0.0 hinzugefügt Version des Pakets zu Ihrem Projekt.

**UseCors aufrufen**

Die folgenden Codeausschnitte veranschaulichen, wie Sie die domänenübergreifende Verbindungen in SignalR implementieren 1.x und 2.0.

**Implementieren von domänenübergreifende Anforderungen in SignalR 1.x (aus der globalen Anwendungsdatei)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementieren von domänenübergreifende Anforderungen in SignalR 2.0 (aus einer C#-Code-Datei)**

Der folgende Code veranschaulicht, wie in einem Projekt auf SignalR 2.0 CORS oder JSONP aktivieren. Dieses Codebeispiel verwendet `Map` und `RunSignalR` anstelle von `MapSignalR`, sodass nur für die SignalR-Anforderungen, die CORS-Unterstützung erfordern, die CORS-Middleware ausgeführt wird (und nicht für den gesamten Datenverkehr im angegebenen Pfad `MapSignalR`.) `Map` kann auch verwendet werden, für alle anderen Middleware, die für eine bestimmte URL-Präfix, nicht für die gesamte Anwendung ausgeführt werden muss.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS und Android-Unterstützung über MonoTouch und MonoDroid

Unterstützung für IOS- und Android mit MonoTouch und MonoDroid Komponenten von Clients wurde der [Xamarin Bibliothek](https://xamarin.com/). Informationen zu ihrer Verwendung finden Sie unter [mithilfe von Xamarin-Komponenten](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Diese Komponenten in stehen die [Xamarin Store](https://store.xamarin.com/) Wenn die SignalR RTW-Version verfügbar ist.

<a id="portable"></a> ### Portabler .NET client

Ermöglichen, eine bessere plattformübergreifende Entwicklung, die Silverlight, WinRT und Windows Phone-Clients mit einem einzelnen portable .NET Client, der die folgenden Plattformen unterstützt ersetzt wurden:

- NET 4.5
- Silverlight 5
- WinRT (.NET für Windows Store-Apps)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Neues Selbsthosting-Paket

Es ist jetzt ein NuGet-Paket zu erleichtern den Einstieg in das Selbsthosting SignalR (SignalR-Anwendungen, die in einem Prozess gehostet werden oder andere Anwendung, statt auf einem Webserver gehostet werden). So aktualisieren Sie die mit SignalR erstellten Projekts Selbsthosting 1.x, entfernen Sie das Paket Microsoft.AspNet.SignalR.Owin, und fügen Sie das Paket Microsoft.AspNet.SignalR.SelfHost. Weitere Informationen zu den ersten Schritten mit dem Paket Selbsthosting finden Sie unter [Lernprogramm: SignalR Selbsthosting](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Abwärtskompatible serverunterstützung

In früheren Versionen von SignalR, die Versionen von SignalR-Pakets auf dem Client verwendet und dem Server erforderlich, um identisch sein. Um dick-Clientanwendungen zu unterstützen, die schwer zu aktualisieren, SignalR 2.0 unterstützt jetzt eines älteren Clients mit einer neueren Server-Version. **Hinweis: SignalR 2.0 unterstützt keine Server, die mit neueren Clients mit älteren Versionen erstellt.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Server-Unterstützung für .NET 4.0 entfernt

SignalR 2.0 wurde Unterstützung für die Interoperabilität mit .NET 4.0 Server gelöscht. .NET 4.5 muss mit SignalR-2.0-Servern verwendet werden. Es ist immer noch ein .NET 4.0-Client für SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Senden einer Nachricht an eine Liste von Clients und Gruppen

In SignalR 2.0 ist es möglich, eine Nachricht mit einer Liste von Client und Gruppen-IDs zu senden. Die folgenden Codeausschnitte veranschaulichen, wie Sie dies tun.

**Senden einer Nachricht an eine Liste von Clients und Gruppen mithilfe von "persistentconnection"**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Senden einer Nachricht an eine Liste von Clients und Gruppen, die unter Verwendung von Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Senden einer Nachricht an einen bestimmten Benutzer

Diese Funktion ermöglicht Benutzern die Angabe, was die Benutzer-ID wird basierend auf einer IRequest über eine neue Schnittstelle IUserIdProvider:

**Die IUserIdProvider-Schnittstelle**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Standardmäßig wird eine Implementierung, die als der Benutzername des Benutzers IPrincipal.Identity.Name verwendet, werden.

In den Hubs werden Sie zum Senden von Nachrichten an diese Benutzer über eine neue API:

**Verwenden die Clients.User-API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Eine bessere Unterstützung der Fehler behandeln

Benutzer können jetzt auslösen **HubException** aus jeder hubaufruf. Der Konstruktor, der die **HubException** dauert ein zeichenfolgenmeldung und ein Objekt zusätzliche Fehlerdaten. SignalR wird die Ausnahme automatisch zu serialisieren und senden sie an den Client, in dem sie dienen zum Ablehnen/Aufrufs der hubmethode Fehler.

Die **Anzeigen von detaillierten Hub Ausnahmen** Einstellung hat keinen Einfluss auf **HubException** zurück an den Client gesendet werden, oder nicht; es wird immer gesendet.

**Serverseitiger Code veranschaulicht eine HubException an den Client senden**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript-Clientcode Demonstrieren der Reaktion auf eine vom Server gesendete HubException**

[!code-html[Main](release-notes/samples/sample16.html)]

**Demonstrieren der Reaktion auf eine vom Server gesendete HubException .NET Client-code**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Einfacher Einheit von Hubs testen

SignalR 2.0 enthält eine Schnittstelle mit dem Namen `IHubCallerConnectionContext` auf Hubs, die für die Erstellung von simulierten Seite Aufrufe erleichtert. Die folgenden Codeausschnitte veranschaulichen die Verwendung dieser Schnittstelle mit gängigen langwierigeren [xUnit.net](https://github.com/xunit/xunit) und [Moq](https://code.google.com/p/moq/).

**Komponententests für SignalR mit xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Komponententests für SignalR mit moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript-Fehlerbehandlung

In SignalR 2.0 zurück alle Rückrufe von JavaScript-Fehlerbehandlung JavaScript-Fehlerobjekte anstelle von unformatierten Zeichenfolgen. Dies ermöglicht umfangreichere Informationen in Ihrem Fehlerhandler SignalR. Sie erhalten die innere Ausnahme aus der `source` Eigenschaft des Fehlers.

**JavaScript-Clientcode, der zur Ausnahmebehandlung Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Neue ASP.NET-Mitgliedschaftssystem

ASP.NET Identity ist das neue Mitgliedschaftssystem für ASP.NET-Anwendungen. ASP.NET Identity erleichtert die Anwendungsdaten benutzerspezifische Profildaten integriert. ASP.NET Identity ermöglicht Ihnen die persistenzspeicherungsmodell für Benutzerprofile in Ihrer Anwendung auszuwählen. Sie können die Daten in einer SQL Server-Datenbank oder einen anderen Datenspeicher, einschließlich NoSQL-Datenspeicher wie z. B. Azure-Speichertabellen speichern. Weitere Informationen finden Sie unter [einzelne Benutzerkonten](creating-web-projects-in-visual-studio.md#indauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Anspruchbasierte Authentifizierung

ASP.NET unterstützt jetzt die anspruchsbasierte Authentifizierung verwendet werden, wo die Identität des Benutzers einen Satz von Ansprüchen von einem vertrauenswürdigen Aussteller dargestellt wird. Benutzer können authentifiziert, mit einem Benutzernamen und Kennwort in einer Anwendungsdatenbank beibehalten oder Identitätsanbieter sozialer Netzwerke mit sein (z. B.: Microsoft Accounts, Facebook, Google, Twitter), oder mithilfe von Unternehmenskonten über Azure Active Directory oder Active Directory-Verbunddienste (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integration mit Azure Active Directory und Windows Server Active Directory

Sie können jetzt ASP.NET-Projekte erstellen, die Azure Active Directory oder Windows Server Active Directory (AD) für die Authentifizierung zu verwenden. Weitere Informationen finden Sie unter [Organisationskonten](creating-web-projects-in-visual-studio.md#orgauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="owin-integration"></a>OWIN-Integration

ASP.NET-Authentifizierung basiert jetzt auf OWIN-Middleware, die auf eine beliebige OWIN-basierten Host verwendet werden kann. Weitere Informationen zu OWIN, finden Sie in der folgenden [Microsoft owin-Komponenten](#TOC7) Abschnitt.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft owin-Komponenten

[Öffnen Sie die Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, Web Applications Host Unabhängigkeit vornehmen. Sie können z. B. eine OWIN-basierte Webanwendung in IIS hosten oder Selbsthosting in einem benutzerdefinierten Prozess.

Änderungen, die in den Microsoft OWIN-Komponenten (auch bekannt als das Katana-Projekt) eingeführt umfassen neue Server und Host-Komponenten, neue Helper-Bibliotheken und Middleware und neue authentifizierungsmiddleware.

Weitere Informationen zu OWIN und Katana, finden Sie unter [Neuheiten bei OWIN und Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Hinweis: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Anwendungen können in IIS im klassischen Modus ausgeführt, sie müssen im integrierten Modus ausgeführt werden.**

**Hinweis: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Anwendungen müssen voller Vertrauenswürdigkeit ausgeführt werden.**

### <a name="new-servers-and-hosts"></a>Neue Server und Hosts

Mit dieser Version wurden neue Komponenten hinzugefügt, um selbst gehosteten Szenarien zu ermöglichen. Zu diesen Komponenten gehören die folgenden NuGet-Pakete:

- **Microsoft.Owin.Host.HttpListener**. Stellt eine OWIN-Servers, der verwendet **HttpListener** zum Empfangen von HTTP-Anforderungen sowie in der OWIN-Pipeline angewiesen.
- **"Microsoft.owin.Hosting"** enthält eine Bibliothek für Entwickler, die eine OWIN-Pipeline in einem benutzerdefinierten Prozess, z. B. eine Konsolenanwendung oder eine Windows-Dienst selbst hosten möchten.
- **OwinHost**. Stellt eine eigenständige ausführbare Datei, die umschließt `Microsoft.Owin.Hosting` und ermöglicht Ihnen die eine OWIN-Pipeline Selbsthosting ohne eine benutzerdefinierte Anwendung schreiben zu müssen.

Darüber hinaus die `Microsoft.Owin.Host.SystemWeb` Paket ermöglicht jetzt die Middleware Hinweise zum Übermitteln der **SystemWeb** -Server, der angibt, dass die Middleware während einer Phase der Pipeline von ASP.NET aufgerufen werden muss. Diese Funktion ist besonders hilfreich bei der Authentifizierung-Middleware, die früh in der ASP.NET-Pipeline ausgeführt werden sollen.

### <a name="helper-libraries-and-middleware"></a>Hilfsprogramm-Bibliotheken und Middleware

Obwohl Sie schreiben können, OWIN-Komponenten, die nur die Definitionen-Funktion und den Typ aus der OWIN-Spezifikation, mit der neuen `Microsoft.Owin` Paket stellt eine benutzerfreundlichere Anzahl Abstraktionen bereit. Dieses Paket kombiniert mehrere frühere Pakete (z. B. `Owin.Extensions`, `Owin.Types`) in einer einzelnen gut strukturierte-Objektmodell, das dann problemlos von anderen OWIN-Komponenten verwendet werden kann. In der Tat die meisten Microsoft OWIN-Komponenten jetzt verwenden Sie dieses Paket.

> [!NOTE]
> [OWIN](http://www.owin.org) Anwendungen können in IIS im klassischen Modus ausgeführt, sie müssen im integrierten Modus ausgeführt werden.

> [!NOTE]
> [OWIN](http://www.owin.org) Anwendungen müssen voller Vertrauenswürdigkeit ausgeführt werden.

Diese Version enthält auch das Paket "Microsoft.owin.Diagnostics", einschließlich Middleware zum Überprüfen einer ausgeführten OWIN-Anwendung sowie Fehlerseite Middleware zum Untersuchen von Fehlern helfen.

### <a name="authentication-components"></a>Authentication-Komponenten

Die folgenden Authentifizierungskomponenten sind verfügbar.

- **Microsoft.Owin.Security.ActiveDirectory**. Aktiviert die Authentifizierung mithilfe einer lokalen oder cloudbasierten Verzeichnisdienst.
- **Microsoft.Owin.Security.Cookies** aktiviert die Authentifizierung mithilfe von Cookies. Dieses Paket wurde zuvor mit dem Namen `Microsoft.Owin.Security.Forms`.
- **"Microsoft.owin.Security.Facebook"** aktiviert die Authentifizierung mithilfe Facebooks-OAuth-basierten Dienst.
- **"Microsoft.owin.Security.Google"** aktiviert die Authentifizierung mithilfe Googles OpenID-basierten Dienst.
- **"Microsoft.owin.Security.jwt"** aktiviert die Authentifizierung mithilfe von JWT-Token.
- **Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.
- **Microsoft.Owin.Security.OAuth**. Stellt ein OAuth-autorisierungsservers sowie die Middleware zum Authentifizieren von Bearer-Token.
- **"Microsoft.owin.Security.Twitter"** aktiviert die Authentifizierung mithilfe von Twitter OAuth-basierten Dienst.

Diese Version umfasst auch die `Microsoft.Owin.Cors` Paket, das Middleware für die Verarbeitung von Cross-Origin-HTTP-Anforderungen enthält.

> [!NOTE]
> Unterstützung für die Signierung von JWT wurde in der endgültigen Version von Visual Studio 2013 entfernt.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Eine Liste der neuen Funktionen und andere Änderungen im Entity Framework 6, finden Sie unter [Entity Framework-Versionsverlaufs](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 enthält die folgenden neuen Features:

- Unterstützung für die Bearbeitung der Registerkarte ". Preivously, die **Dokument formatieren** Befehl automatisch den Einzug und automatische Formatierung in Visual Studio nicht korrekt funktioniert haben bei Verwendung der **Tabulatoren beibehalten** Option. Diese Änderung behebt Visual Studio, die Formatierung für die Razor-Code für die Registerkarte "formatieren.
- Unterstützung für URL Rewrite Regeln beim Generieren von Links.
- Entfernen von Sicherheitsattribut transparent.
  > [!NOTE]
  > Dadurch ist eine wichtige Änderung, und Razor-3 nicht kompatibel mit MVC 4 und früheren Versionen, während der Razor-2 nicht kompatibel mit MVC5 oder Assemblys, die für MVC5 kompiliert wird.

Razor 3 Probleme in Visual Studio 2013 von Vorabversionen verwendbaren [hier](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET App Suspend

ASP.NET App Suspend ist ein Feature revolutionäre in .NET Framework 4.5.1, die die benutzerfreundlichkeit und wirtschaftlichen Gesamtmodell zum Hosten der großen Anzahl von ASP.NET-Websites auf einem einzelnen Computer radikal ändert. Weitere Informationen finden Sie unter [ASP.NET App Suspend – reaktionsfähig freigegebene .NET-Webhosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

In diesem Abschnitt werden bekannte Probleme und aktueller Änderungen im ASP.NET und Webtools für Visual Studio 2013 beschrieben.

### <a name="nuget"></a>NuGet

- [Neue paketwiederherstellung auf Mono funktioniert nicht, wenn SLN-Datei mit](https://nuget.codeplex.com/workitem/3596) – wird in einen bevorstehenden nuget.exe Download behoben und [NuGet.CommandLine Paket](http://www.nuget.org/packages/NuGet.CommandLine/) aktualisieren.
- [Neue paketwiederherstellung funktioniert nicht mit Wix-Projekten](https://nuget.codeplex.com/workitem/3598) – wird in einen bevorstehenden nuget.exe Download behoben und [NuGet.CommandLine Paket](http://www.nuget.org/packages/NuGet.CommandLine/) aktualisieren.
- [Automatische paketwiederherstellung funktioniert nicht für Projekte unter einem Projektmappenordner](https://nuget.codeplex.com/workitem/3625) – wird in NuGet 2.8 behoben.

### <a name="aspnet-web-api"></a>ASP.NET-Web-API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` Gibt keinen zurück `IQueryable<T>` immer, wie wir Unterstützung für hinzugefügt `$select` und `$expand`.

    Unsere früheren Beispiele für `ODataQueryOptions<T>` immer die Typumwandlung der Rückgabewert von `ApplyTo` auf `IQueryable<T>`. Dies zuvor bearbeitet werden, da die Abfrageoptionen wir unterstützt zuvor (`$filter`, `$orderby`, `$skip`, `$top`) ändern Sie die Form der Abfrage nicht. Nun, dass wir unterstützen `$select` und `$expand` der Rückgabewert `ApplyTo` nicht `IQueryable<T>` immer.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Wenn Sie den Beispielcode aus zuvor verwenden, es werden weiterhin ausgeführt, wenn der Client keine gesendet werden `$select` und `$expand`. Jedoch, wenn Sie unterstützen möchten `$select` und `$expand` müssen Sie diesen Code auf diese zu ändern.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url oder RequestContext.Url ist null, während eine Batchanforderung**

    In einem Szenario mit Batchverarbeitung **UrlHelper** ist null, wenn der Zugriff von **Request.Url** oder **RequestContext.Url**.

    Dieses Problem wird derzeit hier nachverfolgt: [BatchRequestContext.Url ist null für die Batchverarbeitung Anforderung](http://aspnetwebstack.codeplex.com/workitem/1301).

    Die problemumgehung für dieses Problem ist, erstellen Sie eine neue Instanz der **UrlHelper**, wie im folgenden Beispiel:

    **Erstellen eine neue Instanz der UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Verwendung von MVC5 und OrgAuth, wenn Sie über Ansichten verfügen die AntiForgerToken Überprüfung sind, möglicherweise stoßen Sie auf der folgende Fehler beim Übermitteln von Daten an die Ansicht:

    **Fehler**:

    *Serverfehler in '/' Anwendung.*

    <em>Ein Anspruch des Typs "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'oder'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' wurde nicht auf die angegebene" ClaimsIdentity "vorhanden. Überprüfen Sie, der konfigurierten Anspruchsanbieter beider diese Ansprüche in den "ClaimsIdentity"-Instanzen bereitstellt, die sie generiert, um Unterstützung für token fälschungssicherheitssystem auf einen mit anspruchsbasierter Authentifizierung zu aktivieren. Wenn die konfigurierten Anspruchsanbieter einen anderen Anspruchstyp stattdessen als eindeutiger Bezeichner verwendet, können sie mithilfe der statischen Eigenschaft AntiForgeryConfig.UniqueClaimTypeIdentifier konfiguriert werden.</em>

    **Problemumgehung**:

    Fügen Sie die folgende Zeile in der Datei Global.asax sie diesen Fehler beheben:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Dies wird für die der nächsten Version behoben werden.
2. Nach dem Upgrade einer MVC4 app MVC5 – erstellen Sie die Projektmappe, und starten Sie ihn. Sie sollten die folgende Fehlermeldung angezeigt:

    [A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection. Typ A, stammen aus "System.Web.WebPages.Razor, Version = 2.0.0.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35' im Kontext 'Default' an Position" C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4. 0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll ". Typ B stammt aus der "System.Web.WebPages.Razor, Version = 3.0.0.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35' im Kontext 'Default' an Position ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll ".

    Um den oben aufgeführten Fehler zu beheben, öffnen Sie *alle* die Dateien "Web.config" (einschließlich derjenigen, die im Ordner "Sichten") im Projekt und führen Sie Folgendes:

   1. Aktualisieren Sie alle Vorkommen von "5.0.0.0" Version "4.0.0.0" von "System.Web.Mvc".
   2. Aktualisieren Sie alle Vorkommen von "System.Web.Helpers", Version "2.0.0.0" &quot;System.Web.WebPages&quot; und &quot;System.Web.WebPages.Razor&quot; auf "3.0.0.0"

      Nachdem Sie die oben genannten Änderungen vorgenommen haben, sollten die Assemblybindungen z. B. wie folgt aussehen:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Wird die clientseitige Validierung mit jQuery unaufdringlichen Validierung verwendet, ist der validierungsmeldung manchmal falsch für ein HTML-Eingabeelement mit Typ = 'Number'. Die Validierungsfehler für einen erforderlichen Wert ("das Altersfeld ist erforderlich") wird angezeigt, wenn eine ungültige Anzahl statt der Nachricht richtig eingegeben werden, dass eine gültige Zahl erforderlich ist.

    Dieses Problem wird häufig für ein Modell mit einer Ganzzahleigenschaft für das Erstellen und Bearbeiten von Sichten mit Gerüstbau Code gefunden.

    Um dieses Problem zu umgehen, ändern Sie die Editor-Hilfsprogramm aus:

    `@Html.EditorFor(person => person.Age)`

    Nach:

    `@Html.TextBoxFor(person => person.Age)`
4. Teilweise Vertrauenswürdigkeit wird von ASP.NET MVC 5 nicht mehr unterstützt. Verknüpfen mit den Binärdateien für MVC oder WebAPI Projekte sollten Entfernen der [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) Attribut und der [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) Attribut. Entfernen dieser Attribute wird Compilerfehler wie z. B. die folgenden beseitigt.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Beachten Sie, wie ein Nebeneffekt der Sie diese 4.0 und 5.0-Assemblys nicht in derselben Anwendung verwenden können. Sie müssen alle von ihnen auf 5.0 aktualisiert.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA-Vorlage mit Facebook-Autorisierung möglicherweise instabil in Internet Explorer während der Website in der Intranetzone gehostet wird

Die SPA-Vorlage enthält externe mit Facebook anmelden. Wenn das Projekt mit der Vorlage erstellte lokal ausgeführt wird, kann die anmelden IE abstürzen führen.

Projektmappe:

1. Host der Website in der Internetzone; oder

2. Testen Sie das Szenario in einem Browser als Internet Explorer.

### <a name="web-forms-scaffolding"></a>Web Forms-Gerüstbau

Web Forms Gerüstbau VS2013 entzogen wurde, und wird in einem zukünftigen Update von Visual Studio verfügbar sein. Allerdings weiterhin können Gerüstbau innerhalb einer Web Forms-Projekt Sie durch Hinzufügen von MVC-Abhängigkeiten und Gerüstbau für MVC generiert. Das Projekt enthält eine Kombination von Web Forms und MVC.

Um das Web Forms-Projekt MVC hinzugefügt haben, fügen Sie ein neues Gerüst, und wählen Sie **MVC 5-Abhängigkeiten**. Wählen Sie die minimale oder vollständige abhängig davon, ob Sie alle Inhaltsdateien, z. B. Skripts erforderlich. Anschließend fügen Sie ein Gerüst für MVC, die Ansichten und einem Controller in Ihrem Projekt erstellt werden.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC und Web-API Gerüstbau - HTTP 404, wurde nicht gefunden.

Wenn beim Hinzufügen eines gerüstelements zu einem Projekt ein Fehler aufgetreten ist, ist es möglich, die Ihr Projekt in einem inkonsistenten Zustand gelassen werden. Einige Änderungen vorgenommen werden Gerüstbau wird ein Rollback, aber andere Änderungen, z. B. die installierten NuGet-Pakete werden nicht zurückgesetzt werden. Falls die routing konfigurationsänderungen zurückgesetzt werden, erhalten Benutzer einen HTTP 404-Fehler beim Navigieren zu Elemente Gerüstbau.

Problemumgehung:

- Klicken Sie zum Beheben dieses Fehlers für MVC eine neue Elements mit Gerüst hinzufügen, und wählen Sie MVC 5-Abhängigkeiten (minimale oder vollständige). Dabei werden alle erforderlichen Änderungen zu Ihrem Projekt hinzufügen.
- So beheben Sie diesen Fehler für die Web-API:

  1. Fügen Sie der Klasse "webapiconfig" zu Ihrem Projekt hinzu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Konfigurieren Sie in der Anwendung WebApiConfig.Register\_Start-Methode in "Global.asax" wie folgt:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
