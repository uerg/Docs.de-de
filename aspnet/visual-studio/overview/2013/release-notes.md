---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Webtools für Visual Studio 2013 Release Notes | Microsoft-Dokumentation
author: microsoft
description: Dieses Dokument beschreibt die Version von ASP.NET und Webtools für Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 44ab88b61a96235da27ff41d6b649bfd7fce3e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836193"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET and Webtools für Visual Studio 2013 – Versionsanmerkungen
====================
durch [Microsoft](https://github.com/microsoft)

> Dieses Dokument beschreibt die Version von ASP.NET und Webtools für Visual Studio 2013.


## <a name="contents"></a>Inhalt

- [Installationshinweise](#TOC1)
- [Dokumentation](#TOC2)
- [Softwareanforderungen](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Features in ASP.NET und Webtools für Visual Studio 2013

- [Eine ASP.NET](#TOC6)
- [Neue Project-Weboberfläche](#newproj)
- [ASP.NET-Gerüstbau](#scaffold)
- [Browserverknüpfung](#browser-link)
- [Visual Studio Web-Editor-Erweiterungen](#web-editor)
- [Azure App Service-Web-Apps-Unterstützung in Visual Studio](#waws)
- [Webveröffentlichung mit Erweiterungen](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET-Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web-API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Microsoft OWIN-Komponenten](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor-3](#TOC14)
- [ASP.NET-App Suspend](#TOC15)
- [Bekannte Probleme und aktueller Änderungen](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET and Web Tools für Visual Studio 2013 werden in der main-Installationsprogramm gebündelt und können heruntergeladen werden [hier](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET und Webtools für Visual Studio 2013 stehen über die [ASP.NET-Website](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET and Web Tools erfordert Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Features in ASP.NET und Webtools für Visual Studio 2013

Die folgenden Abschnitte beschreiben die Funktionen, die in der Version eingeführt wurden.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Eine ASP.NET

Mit der Veröffentlichung von Visual Studio 2013 haben wir einen Schritt in Richtung vereinheitlichen die Verwendung von ASP.NET-Technologien, damit Sie leicht kombinieren und identisch mit der gewünschten ergriffen. Beispielsweise können Sie starten Sie ein Projekt mithilfe von MVC und einfach Web Forms-Seiten für das Projekt später hinzufügen, oder Erstellen des Gerüsts für Web-APIs in einem Web Forms-Projekt. Eine ASP.NET geht es einfacher für Sie als Entwickler, die Dinge zu tun, die Sie in ASP.NET gibt, lieben. Unabhängig davon, welche Technologie Sie wählen, können Sie die Gewissheit verfügen, die Sie in der vertrauenswürdigen zugrunde liegende Framework von One ASP.NET erstellen.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Neue Project-Weboberfläche

Wir haben die Erfahrung mit der Erstellung neuer Webprojekte in Visual Studio 2013 verbessert. In der **neuen ASP.NET-Webprojekts** Dialogfeld können Sie den Projekttyp, Sie möchten, konfigurieren eine beliebige Kombination aus Technologien (Web Forms, MVC, Web-API), konfigurieren Sie Authentifizierungsoptionen und hinzufügen ein Komponententestprojekts, auswählen.

![Neues ASP.NET-Projekt](release-notes/_static/image1.png)

Der neue Dialog können Sie die standardmäßigen Authentifizierungsoptionen für viele der Vorlagen zu ändern. Z. B. beim Erstellen einer ASP.NET Web Forms-Projekts können Sie eine der folgenden Optionen auswählen:

- Keine Authentifizierung
- Einzelne Benutzerkonten (ASP.NET-Mitgliedschaft oder Anbieter sozialer-Anmeldung)
- Organisations-Konten (Active Directory in einer internetanwendung)
- Windows-Authentifizierung (Active Directory in einer Intranetanwendung)

![Authentifizierungsoptionen](release-notes/_static/image2.png)

Weitere Informationen zu den neuen Prozess für das Erstellen von Webprojekten, finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Weitere Informationen zu den neuen Authentifizierungsoptionen finden Sie unter [ASP.NET Identity](#TOC8) weiter unten in diesem Dokument.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET-Gerüstbau

ASP.NET-Gerüstbau ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET. Es erleichtert die Codebausteine zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit Visual Studio 2013 können Sie jetzt Gerüstbau für ASP.NET Projekte, einschließlich Web Forms. Visual Studio 2013 unterstützt derzeit keine Generieren von Seiten für eine Web Forms-Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, durch das Hinzufügen von MVC-Abhängigkeiten auf das Projekt. Unterstützung für das Generieren von Seiten für Web Forms wird in einem späteren Update hinzugefügt werden.

Wenn Gerüstbau verwenden zu können, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einem ASP.NET Web Forms-Projekt beginnen, und klicken Sie dann mithilfe von Gerüstbau einer Web-API-Controller hinzu, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

MVC-Gerüstbau um eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für den Gerüstbau für MVC. Minimaler und vollständiger. Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Bei Auswahl der Option "vollständig", die mindestens erforderliche Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für den Gerüstbau für Async-Controller verwendet, die neuen asynchronen Features von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browserlink-SignalR-Kanal zwischen Browser und Visual Studio

Die neue [Browserlink](using-browser-link.md) -Funktion können Sie das Verbinden von mehreren Browsern mit Visual Studio, und aktualisieren sie alle durch Klicken auf eine Schaltfläche auf der Symbolleiste. Sie können Verbinden von mehreren Browsern mit Ihrer Entwicklungswebsite, einschließlich der mobilen Emulatoren aus, und klicken Sie auf aktualisieren, aktualisieren Sie alle zur gleichen Zeit alle Browser. Browserverknüpfung macht auch eine API zum ermöglichen Entwicklern das Schreiben von Browser Link-Erweiterungen von verfügbar.

![](release-notes/_static/image3.png)

Durch die ermöglicht es Entwicklern, die die Browserlink-API nutzen, wird es möglich, Erstellen von sehr komplexen Szenarien, die über mehrere zwischen Visual Studio und einem beliebigen Browser, der verbunden ist. Web Essentials nutzt die Vorteile der API um eine integrierte Erfahrung zwischen Visual Studio und im Browser auf die Entwicklertools, remote steuern des mobilen Emulatoren und vieles mehr zu erstellen.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web-Editor-Erweiterungen

Visual Studio 2013 enthält einen neuen HTML-Editor für den Razor-Dateien und HTML-Dateien in Webanwendungen. Der neue HTML-Editor bietet ein einzelnes einheitliches Schema auf Grundlage von HTML5. Es bietet automatische Vervollständigung von Klammern, jQuery-Benutzeroberfläche und AngularJS-Attribut von IntelliSense, Attribut IntelliSense Gruppierung, ID und Klassennamen Intellisense und andere Verbesserungen, die z. B. eine bessere Leistung, Formatierung und SmartTags.

Der folgende Screenshot zeigt mithilfe von Bootstrap Attribut IntelliSense in der HTML-Editor.

![IntelliSense in HTML-editor](release-notes/_static/image4.png)

Visual Studio 2013 enthält auch mit beiden CoffeeScript und LESS-Editor integriert. LESS-Editor enthält alle interessanten Features von CSS-Editor, und verfügt über bestimmte Intellisense für Variablen und Mixins in alle weniger Dokumente in der @import Kette.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Azure App Service-Web-Apps-Unterstützung in Visual Studio

In Visual Studio 2013 mit dem Azure SDK für .NET 2.2 können Sie **Server-Explorer** direkt mit die remote-Web-apps interagieren. Sie melden Sie sich bei Ihrem Azure-Konto, Erstellen neuer Web-apps, apps konfigurieren, echtzeitprotokolle und vieles mehr anzeigen. Feedback nach SDK 2.2 veröffentlicht wird, Sie werden in den Debugmodus Remote in Azure ausführen können. Die meisten neuen Features von Azure App Service-Web-Apps funktionieren auch in Visual Studio 2012, wenn Sie die aktuelle Version des Azure SDK für .NET installieren.

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Erstellen einer ASP.NET-Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Problembehandlung in einer Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Webveröffentlichung mit Erweiterungen

Visual Studio 2013 enthält neue und verbesserte Web Publish-Funktionen. Hier sind einige davon:

- Ganz einfach [Automatisieren von "Web.config" dateiverschlüsselung](https://go.microsoft.com/fwlink/?LinkId=325529). (Dieser Link und die folgenden beiden Punkt in der Dokumentation auf MSDN, die möglicherweise nicht verfügbar bis spät in der Tag auf 10/17.)
- Ganz einfach [automatisieren eine Anwendung offline schalten, während der Bereitstellung dauert](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfigurieren von Web Deploy, um [Dateiprüfsumme verwenden, anstatt das zuletzt Änderungsdatum](https://go.microsoft.com/fwlink/?LinkId=325531) zu bestimmen, welche Dateien an den Server kopiert werden sollen.
- Veröffentlichen Sie schnell können einzelnen ausgewählte Dateien (einschließlich der Datei "Web.config"), bei der Verwendung von FTP oder Dateisystem veröffentlichen Methoden als auch mit Web Deploy.

Weitere Informationen zur Bereitstellung von ASP.NET finden Sie unter [der ASP.NET-Website](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 umfasst einen umfangreichen Satz von neuen Features, die beschrieben werden, im Detail [Anmerkungen zu NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Diese Version von NuGet wird auch die Notwendigkeit, bieten explizite Zustimmung für das Feature für die Wiederherstellung von NuGet Paket zum Herunterladen von Paketen entfernt. Zustimmung (und die zugehörigen Kontrollkästchen im NuGet Dialogfeld "Einstellungen") werden jetzt durch Installieren von NuGet gewährt. Wiederherstellen von Paketen funktioniert nun einfach in der Standardeinstellung.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

### <a name="one-aspnet"></a>Eine ASP.NET

Die Web Forms-Projektvorlagen nahtlose Integration in die neue Oberfläche für die One ASP.NET. Sie können hinzufügen, MVC und Web-API unterstützen, dem Web Forms-Projekt, und Sie können die Authentifizierung mithilfe von One ASP.NET projekterstellungs-Assistenten konfigurieren. Weitere Informationen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die Web Forms-Projektvorlagen unterstützen das neue ASP.NET Identity-Framework. Darüber hinaus unterstützen die Vorlagen jetzt Erstellung eines Web Forms-Intranet-Projekts. Weitere Informationen finden Sie unter [Authentifizierungsmethoden](creating-web-projects-in-visual-studio.md#auth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap-Stil

Verwenden Sie die Web Forms-Vorlagen [Bootstrap](http://twitter.github.io/bootstrap/) , ein schlankes und reaktionsfähige Aussehen und Verhalten bereitzustellen, die Sie problemlos anpassen können. Weitere Informationen finden Sie unter [Bootstrap in den Visual Studio 2013-Web-Projektvorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Eine ASP.NET

Die Web-MVC-Projektvorlagen nahtlose Integration in die neue Oberfläche für die One ASP.NET. Sie können Ihre MVC-Projekt anpassen und Konfigurieren von Authentifizierung unter Verwendung von One ASP.NET projekterstellungs-Assistenten. Ein einführendes Lernprogramm zu ASP.NET MVC 5 finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die MVC-Projektvorlagen wurden aktualisiert, um ASP.NET Identity für Authentifizierung und identitätsverwaltung verwenden. Ein Tutorial mit Facebook und Google-Authentifizierung und der neuen Mitgliedschafts-API finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) und [Erstellen einer ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank und in Azure App Service bereitstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap-Stil

MVC-Projektvorlage wurde aktualisiert, um verwenden [Bootstrap](http://getbootstrap.com/) , ein schlankes und reaktionsfähige Aussehen und Verhalten bereitzustellen, die Sie problemlos anpassen können. Weitere Informationen finden Sie unter [Bootstrap in den Visual Studio 2013-Web-Projektvorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Authentifizierungsfilter

Authentifizierungsfilter sind eine neue Art von Filter in ASP.NET MVC, die vor dem Autorisierungsfilter werden also in der ASP.NET MVC-Pipeline ausgeführt und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben einen entsprechenden Prinzipal. Authentifizierungsfilter können authentifizierungsanforderungen auch als Reaktion auf nicht autorisierte Anforderungen hinzufügen.

### <a name="filter-overrides"></a>Überschreibungsfilter

Sie können jetzt überschreiben, die Filter auf eine bestimmte Aktionsmethode bzw. einen Controller angewendet werden, durch Angabe eines Filters außer Kraft setzen. Überschreibungsfilter Geben Sie einen Satz von Filtertypen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie so konfigurieren Sie Filter, die global angewendet, aber dann bestimmte globale Filter ausschließen, nicht auf bestimmte Aktionen oder Controller angewendet.

### <a name="attribute-routing"></a>Attributrouting

ASP.NET MVC unterstützt jetzt das attributrouting, Dank der einen Beitrag von Tim McCall der Autor des [ http://attributerouting.net ](http://attributerouting.net). Das attributrouting können Sie Ihre Routen angeben, durch das Hinzufügen von Anmerkungen zu Ihren Aktionen und Controllern.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET-Web-API 2

### <a name="attribute-routing"></a>Attributrouting

ASP.NET Web-API unterstützt jetzt das attributrouting, Dank der einen Beitrag von Tim McCall der Autor des [ http://attributerouting.net ](http://attributerouting.net). Das attributrouting können Sie Ihre Web-API-Routen angeben, durch das Hinzufügen von Anmerkungen zu Ihren Aktionen und Controllern wie folgt:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Attribut-routing bietet Ihnen mehr Kontrolle über die URIs in Ihrer Web-API. Beispielsweise können Sie ganz einfach über einen einzelnen API-Controller Ressourcenhierarchie definieren:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Attributrouting auch bietet eine praktische Syntax zum Angeben Optionaler Parameter, Standardwerte und Einschränkungen:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Weitere Informationen zu Attribut-routing, finden Sie unter [Attributrouting in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Die Web-API und Single Page Application-Projektvorlagen unterstützen nun mithilfe von OAuth 2.0 Autorisierung. OAuth 2.0 ist ein Framework für die Autorisierung des Clientzugriff auf geschützte Ressourcen. Dies funktioniert für eine Vielzahl von Clients einschließlich Browsern und mobilen Geräten.

Unterstützung für OAuth 2.0 basiert auf neuen sicherheitsmiddleware von den Microsoft-OWIN-Komponenten für Bearer-Authentifizierung bereitgestellt werden, und implementieren die Serverrolle für die Autorisierung. Alternativ können die Clients mit einem organisationsbezogenen autorisierungsserver, z. B. Azure Active Directory oder AD FS in Windows Server 2012 R2 autorisiert werden.

### <a name="odata-improvements"></a>OData-Verbesserungen

**Unterstützung für $select, $ $batch und $value erweitern**

ASP.NET Web API OData bietet jetzt vollständige Unterstützung für $select, $expand, und $value. Sie können auch für die Anforderung, Batchverarbeitung und Verarbeitung von Changesets $batch verwenden.

Der $select und $expand-Optionen können Sie ändern die Form der Daten, die von einem OData-Endpunkt zurückgegeben werden. Weitere Informationen finden Sie unter [Einführung in $select und $expand-Unterstützung in Web-API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Verbesserte Erweiterbarkeit**

Die OData-Formatierer können jetzt erweitert werden. Sie können Atom-Eintrag-Metadaten hinzufügen, benannte Streams und Medien-Link-Einträge unterstützt, instanzanmerkungen hinzufügen und anpassen, wie Links generiert werden.

**Typ-kleiner-Unterstützung**

Sie können nun die OData-Dienste erstellen, ohne zu CLR-Typen für die Entitätstypen definieren müssen. Stattdessen können die OData-Controller nutzen oder Zurückgeben der Instanzen von **IEdmObject**, werden die OData-Formatierer serialisieren/deserialisieren.

**Wiederverwenden eines vorhandenen Modells**

Wenn Sie bereits über ein vorhandenes Entitätsdatenmodell (EDM) verfügen, können Sie jetzt es direkt wiederverwenden anstatt eine neue Ressourcengruppe zu erstellen. Z. B. Wenn Sie Entity Framework verwenden, können das EDM Sie, die EF für Sie erstellt.

### <a name="request-batching"></a>Anfordern der Batchverarbeitung

Anforderung Batchverarbeitung kombiniert mehrere Vorgänge in eine einzelne HTTP POST-Anforderung, um den Netzwerkverkehr verringern, und geben Sie eine glattere, weniger ' geschwätzige '-Benutzeroberfläche. ASP.NET Web-API unterstützt nun verschiedene Strategien für die Batchverarbeitung der Anforderung:

- Verwenden Sie den $batch-Endpunkt von einem OData-Dienst.
- Verpacken Sie mehrere Anforderungen in einer einzelnen MIME-multipart-Anforderung.
- Verwenden Sie ein benutzerdefiniertes Format für die Batchverarbeitungs.

Um Anforderung, die Batchverarbeitung zu aktivieren, müssen fügen Sie eine Route mit einem Batchverarbeitung Handler einfach Ihrer Web-API-Konfiguration hinzu:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Sie können auch steuern, ob Anforderungen oder ausgeführt werden, sequenziell oder in einer beliebigen Reihenfolge.

### <a name="portable-aspnet-web-api-client"></a>Portable ASP.NET Web-API-Client

Sie können jetzt die ASP.NET Web-API-Client verwenden, zum Erstellen von portablen Klassenbibliotheken, die funktionieren in Ihrer Windows Store und Windows Phone 8-Anwendungen. Sie können auch portable Formatierer erstellen, die zwischen Client und Server gemeinsam genutzt werden können.

### <a name="improved-testability"></a>Verbesserte Prüfbarkeit

Web-API 2 macht es viel einfacher, Komponententests testen Sie die API-Controller. Instanziieren Sie einfach Ihren API-Controller mit Ihrem Request-Nachricht und die Konfiguration, und rufen Sie dann auf die Aktionsmethode, die Sie testen möchten. Es ist auch einfach zum Simulieren der **UrlHelper** -Klasse, für die Aktionsmethoden, die linkgenerierung ausführen.

### <a name="ihttpactionresult"></a>IHttpActionResult

Sie können jetzt IHttpActionResult kapselt das Ergebnis der Ihre Web-API-Aktionsmethoden implementieren. Ein von einer Web-API-Aktion-Methode zurückgegebenen IHttpActionResult wird von der ASP.NET Web-API-Laufzeit zum Erzeugen der resultierenden Response-Nachricht ausgeführt. Ein IHttpActionResult zurückgegeben werden kann, in einer beliebigen Web-API-Aktion zur Vereinfachung der Einheit Testen Ihrer Web-API-Implementierung. Der Einfachheit halber, die eine Anzahl von IHttpActionResult Implementierungen, z. B. die Ergebnisse bereitgestellt werden für bestimmte Rückgabestatuscodes formatiert oder dem Inhalt ausgehandelte Antworten.

### <a name="httprequestcontext"></a>HttpRequestContext

Die neue **HttpRequestContext** verfolgt alle Status, die der Anforderung verknüpft ist, aber ist nicht sofort verfügbar ist, aus der Anforderung. Beispielsweise können Sie die **HttpRequestContext** zum Abrufen von Routendaten, die dem Prinzipal zugeordnete Anforderung das Clientzertifikat, das **UrlHelper** und der virtuelle pfadstamm. Sie können ganz einfach erstellen, eine **HttpRequestContext** für Komponententests Testzwecke.

Weil der Prinzipal für die Anforderung, mit der Anforderung übergeben wird nicht die **Thread.CurrentPrincipal**, der Prinzipal ist jetzt verfügbar, während der gesamten Lebensdauer der Anforderung, während es in der Web-API-Pipeline ist.

### <a name="cors"></a>CORS

Dank einer anderen hervorragenden Beitrag von Brock Allen unterstützt ASP.NET jetzt vollständig Request Freigeben von CORS (Cross Origin).

Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen in eine andere Domäne. [CORS](http://www.w3.org/TR/cors/) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht. Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.

Web-API 2 unterstützt jetzt CORS, einschließlich der automatische Behandlung von preflight-Anforderungen. Weitere Informationen finden Sie unter [Aktivieren von Cross-Origin Requests in ASP.NET Web-API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Authentifizierungsfilter

Authentifizierungsfilter sind eine neue Art von Filter in ASP.NET Web-API, die vor dem Autorisierungsfilter werden also in der ASP.NET Web-API-Pipeline ausgeführt und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben einen entsprechenden Prinzipal. Authentifizierungsfilter können authentifizierungsanforderungen auch als Reaktion auf nicht autorisierte Anforderungen hinzufügen.

### <a name="filter-overrides"></a>Überschreibungsfilter

Sie können jetzt überschreiben, der Filter auf eine bestimmte Aktion-Methode oder den Controller angewendet werden, durch Angabe eines Filters außer Kraft setzen. Überschreibungsfilter Geben Sie einen Satz von Filtertypen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie globale Filter hinzufügen, aber dann Ausschließen einiger aus bestimmte Aktionen oder Controllern.

### <a name="owin-integration"></a>OWIN-Integration

ASP.NET Web-API jetzt vollständig unterstützt OWIN und kann jede OWIN-fähigen Hosts ausgeführt werden. Enthält auch eine **auch namensbasiert** , Integration in die OWIN-Authentifizierungssystem bereitstellt.

Mit der OWIN-Integration können Sie Web-API in Ihrem eigenen Prozess zusammen mit anderen OWIN-Middleware, wie SignalR selbst hosten. Weitere Informationen finden Sie unter [verwenden OWIN zum selfhosten von ASP.NET-Web-API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

In den folgenden Abschnitten werden die Funktionen von SignalR 2.0 beschrieben.

- [Basiert auf OWIN](#builtonowin)
- [MapHubs und MapConnection sind jetzt MapSignalR](#MapSignalR)
- [Cross-Domain-Unterstützung](#crossdomain)
- [iOS und Android-Unterstützung über MonoTouch und MonoDroid](#mobile)
- [Portable .NET Client](#portable)
- [Neues Self-Hosting-Paket](#selfhost)
- [Abwärtskompatibilität-serverunterstützung](#backwardcompat)
- [Server-Unterstützung für .NET 4.0 entfernt](#remove40)
- [Senden einer Nachricht an eine Liste der Clients und Gruppen](#messagelist)
- [Senden einer Nachricht an einen bestimmten Benutzer](#sendtouser)
- [Eine bessere Unterstützung von Fehler behandeln](#errorhandling)
- [Einfacher Komponententest Testen von Hubs](#unittesting)
- [JavaScript-Fehlerbehandlung](#javascripterror)

Ein Beispiel für ein vorhandenes 1.x-Projekt zu SignalR 2.0 aktualisieren, finden Sie unter [Aktualisieren einer SignalR 1.x-Projekt](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basiert auf OWIN

SignalR 2.0 basiert vollständig auf [OWIN (Open Web Interface for .NET)](http://owin.org/). Diese Änderung wird den Setupvorgang für SignalR-weitaus webgehostete und selbst gehostete SignalR-Anwendungen konsistent, aber es ist auch eine Anzahl von API-Änderungen erforderlich.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs und MapConnection sind jetzt MapSignalR

Kompatibilität mit OWIN-Standards, wurden diese Methoden in umbenannt `MapSignalR`. `MapSignalR` wird aufgerufen, ohne Parameter alle Hubs zugeordnet werden (als `MapHubs` ist in Version 1.x), um einzelne zuzuordnen **PersistentConnection** Objekte, geben Sie die Verbindung als der Type-Parameter, und die URL-Erweiterung für die Verbindung als der Erstes Argument.

Die `MapSignalR` Methode wird aufgerufen, in einer Owin-Startup-Klasse. Visual Studio 2013 enthält eine neue Vorlage für eine Owin-Startklasse; Um diese Vorlage verwenden möchten, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf das Projekt
2. Wählen Sie **hinzufügen**, **neues Element...**
3. Wählen Sie **Owin-Startklasse**. Nennen Sie die neue Klasse **"Startup.cs"**.

In einem **Webanwendung,** die Owin Startup-Klasse, enthält die `MapSignalR` Methode wird dann an Owins-Startvorgangs mit einem Eintrag in der Anwendungsknoten der Einstellungen der Datei "Web.config" hinzugefügt, wie unten dargestellt.

In einem **für selbst gehostete Anwendung**, die Startup-Klasse übergeben wird, wie der Typparameter der `WebApp.Start` Methode.

**Zuordnen von Hubs und Verbindungen in SignalR 1.x (aus der globalen Datei in einer Webanwendung):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Zuordnen von Hubs und Verbindungen in SignalR 2.0 (aus einer Datei für den Owin-Startup-Klasse):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In einem **für selbst gehostete Anwendung**, die Startup-Klasse übergeben wird, wie der Typparameter für die `WebApp.Start` Methode, wie unten dargestellt.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Cross-Domain-Unterstützung

In SignalR 1.x, domänenübergreifende Anforderungen wurden gesteuert, indem einem einzigen EnableCrossDomain-Flag. Dieses Flag gesteuert, sowohl JSONP und CORS-Anforderungen. Zur Erhöhung der Flexibilität-alle CORS-Unterstützung wurde aus dem die Server-Komponente von SignalR entfernt (JavaScript Lients weiterhin verwenden CORS normalerweise, wenn es erkannt wird, dass der Browser unterstützt), und neue OWIN-Middleware zur Unterstützung dieser Szenarios verfügbar gemacht wurde.

In SignalR 2.0 Wenn JSONP auf dem Client (zur Unterstützung von domänenübergreifende Anforderungen in älteren Browsern) erforderlich ist, muss explizit aktiviert werden, durch Festlegen von `EnableJSONP` auf die `HubConfiguration` -Objekt `true`, wie unten dargestellt. JSONP ist standardmäßig deaktiviert, da er als weniger sicher als CORS ist.

Um die neuen CORS-Middleware in SignalR 2.0 hinzuzufügen, fügen Sie der `Microsoft.Owin.Cors` Bibliothek, um das Projekt, und rufen `UseCors` vor Ihrer SignalR-Middleware, wie im folgenden Abschnitt gezeigt.

**Hinzufügen der Microsoft.Owin.Cors zu Ihrem Projekt**: um diese Bibliothek zu installieren, führen Sie den folgenden Befehl in der Paket-Manager-Konsole:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Dieser Befehl fügt die 2.0.0 Version des Pakets zu Ihrem Projekt.

**Aufrufen von "usecors"**

Die folgenden Codeausschnitte veranschaulichen, wie implementieren Sie die domänenübergreifende Verbindungen in SignalR 1.x und 2.0.

**Implementieren von domänenübergreifende Anforderungen in SignalR 1.x (aus der globalen Datei)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementieren von domänenübergreifende Anforderungen in SignalR 2.0 (aus einer C#-Code-Datei)**

Der folgende Code veranschaulicht, wie CORS oder JSONP in einem SignalR 2.0-Projekt zu aktivieren. Dieses Codebeispiel verwendet `Map` und `RunSignalR` anstelle von `MapSignalR`, sodass nur für die SignalR-Anforderungen, die CORS-Unterstützung erfordern, die CORS-Middleware ausgeführt wird (und nicht für den gesamten Datenverkehr an den im angegebenen Pfad `MapSignalR`.) `Map` kann auch verwendet werden, für alle anderen Middleware, die für eine bestimmte URL-Präfix, nicht aber für die gesamte Anwendung ausgeführt werden muss.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS und Android-Unterstützung über MonoTouch und MonoDroid

Unterstützung für IOS- und Android-Clients mithilfe des MonoTouch und MonoDroid-Komponenten von wurde die [Xamarin-Bibliothek](https://xamarin.com/). Weitere Informationen dazu, wie deren Verwendung finden Sie unter [mithilfe von Xamarin-Komponenten](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Diese Komponenten werden in der [Xamarin Store](https://store.xamarin.com/) Wenn die SignalR-RTW-Version verfügbar ist.

<a id="portable"></a> ### Portable .NET client

Vereinfachen die bessere plattformübergreifende Entwicklung, die Silverlight, WinRT und Windows Phone-Clients mit einem einzelnen portable .NET Client, der die folgenden Plattformen unterstützt ersetzt wurden:

- NET 4.5
- Silverlight 5
- WinRT (.NET für Windows Store-Apps)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Neues Self-Hosting-Paket

Es ist jetzt ein NuGet-Paket zu vereinfachen den Einstieg in die Selfhosten von SignalR (SignalR-Anwendungen, die in einem Prozess gehostet werden oder andere Anwendungen, statt auf einem Webserver gehostet werden). Aktualisieren Sie eine Self-Hosting-Projekt erstellt, die mit SignalR 1.x, entfernen Sie das Paket Microsoft.AspNet.SignalR.Owin, und fügen Sie das Paket Microsoft.AspNet.SignalR.SelfHost. Weitere Informationen zu den ersten Schritten mit dem Self-Hosting-Paket finden Sie unter [Tutorial: Selfhosten von SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Abwärtskompatibilität-serverunterstützung

In früheren Versionen von SignalR, die Versionen der SignalR-Paket auf dem Client verwendet und dem Server, die erforderlich sind, identisch sein. Um Thick-Clientanwendungen zu unterstützen, die schwer zu aktualisieren, unterstützt bei SignalR 2.0 jetzt eine neuere Serverversion mit einem älteren Client verwenden. **Hinweis: SignalR 2.0 unterstützt keine Server, die mit älteren Versionen mit neuere Clients erstellt.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Server-Unterstützung für .NET 4.0 entfernt

SignalR 2.0 wurde Unterstützung für die Interoperabilität mit .NET 4.0 Server gelöscht. .NET 4.5 muss mit SignalR 2.0-Servern verwendet werden. Es ist immer noch ein .NET 4.0-Client für SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Senden einer Nachricht an eine Liste der Clients und Gruppen

In SignalR 2.0 ist es möglich, eine Nachricht mit einer Liste von Client und der Gruppe IDs senden. Die folgenden Codeausschnitte veranschaulichen die Vorgehensweise.

**Senden einer Nachricht an eine Liste der Clients und Gruppen mit PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Senden einer Nachricht an eine Liste der Clients und Gruppen, die mithilfe von Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Senden einer Nachricht an einen bestimmten Benutzer

Dieses Feature ermöglicht Benutzern, um anzugeben, was die Benutzer-ID basierend auf einer IRequest über eine neue Schnittstelle IUserIdProvider:

**Die IUserIdProvider-Schnittstelle**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Standardmäßig wird eine Implementierung, die den Benutzernamen des Benutzers IPrincipal.Identity.Name verwendet werden.

In den Hubs wird zum Senden von Nachrichten an diese Benutzer über eine neue API werden:

**Verwenden die Clients.User-API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Eine bessere Unterstützung von Fehler behandeln

Benutzer können jetzt auslösen **HubException** aus einem hubaufruf. Der Konstruktor, der die **HubException** sind eine zeichenfolgenmeldung und ein Objekt zusätzliche Fehlerdaten. SignalR wird die Ausnahme automatisch serialisiert und sendet es an den Client, in dem er dient zum Ablehnen oder den Aufruf der hubmethode nicht.

Die **ausführliche Hub-Ausnahmen zeigen** Einstellung hat keinen Einfluss auf **HubException** zurück an den Client gesendet werden, oder nicht; sie wird immer gesendet.

**Serverseitiger Code veranschaulicht eine HubException an den Client gesendet.**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript-Client-Codebeispiel zur Erläuterung, reagieren auf eine vom Server gesendeten HubException**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET Client-Code veranschaulicht, reagieren auf eine vom Server gesendeten HubException**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Einfacher Komponententest Testen von Hubs

SignalR 2.0 umfasst eine Schnittstelle namens `IHubCallerConnectionContext` Hubs, der zum Erstellen von mock Seite Aufrufe einfacher macht. Die folgenden Codeausschnitte veranschaulichen das Verwenden dieser Schnittstelle mit beliebten testumgebungen [xUnit.net](https://github.com/xunit/xunit) und [Moq](https://code.google.com/p/moq/).

**Komponententests in SignalR mit xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Komponententests in SignalR mit moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript-Fehlerbehandlung

In SignalR 2.0 zurück alle Rückrufe von JavaScript-Fehlerbehandlung JavaScript-Fehler-Objekte anstelle von unformatierten Zeichenfolgen. Dadurch wird ein SignalR umfangreichere Informationen in Ihrem Fehlerhandler. Sie erhalten die innere Ausnahme aus der `source` Eigenschaft des Fehlers.

**JavaScript-Clientcode, der die Start.Fail-Ausnahme behandelt.**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Neue ASP.NET-Mitgliedschaftssystem

ASP.NET Identity ist das neue Mitgliedschaftssystem für ASP.NET-Anwendungen. ASP.NET Identity vereinfacht benutzerspezifische Profildaten in Anwendungsdaten zu integrieren. ASP.NET Identity ermöglicht auch das Persistenzmodell für Benutzerprofile in Ihrer Anwendung auswählen. Sie können die Daten in einer SQL Server-Datenbank oder einem anderen Datenspeicher, einschließlich NoSQL-Datenspeicher wie Azure Storage-Tabellen speichern. Weitere Informationen finden Sie unter [einzelne Benutzerkonten](creating-web-projects-in-visual-studio.md#indauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Anspruchbasierte Authentifizierung

ASP.NET unterstützt nun die anspruchsbasierte Authentifizierung verwendet werden, wo die Identität des Benutzers als einen Satz von Ansprüchen von einem vertrauenswürdigen Aussteller dargestellt wird. Benutzer können sein, authentifizierten eines Benutzernamens und Kennworts in einer Datenbank gespeichert oder über soziale Netzwerke als Identitätsanbieter (z. B.: Microsoft Accounts, Facebook, Google, Twitter), oder verwenden die Organisations-Konten über Azure Active Directory oder Active Directory-Verbunddienste (AD FS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Azure Active Directory und Windows Server Active Directory-Integration

Sie können jetzt ASP.NET-Projekte erstellen, die Azure Active Directory oder Windows Server Active Directory (AD) für die Authentifizierung zu verwenden. Weitere Informationen finden Sie unter [Organisationskonten](creating-web-projects-in-visual-studio.md#orgauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="owin-integration"></a>OWIN-Integration

Authentifizierung in ASP.NET basiert jetzt auf OWIN-Middleware, die auf einem OWIN-basierten Host verwendet werden können. Weitere Informationen zu OWIN, finden Sie unter den folgenden [Microsoft OWIN-Komponenten](#TOC7) Abschnitt.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN-Komponenten

[Öffnen von Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, sodass Web Applications Hosts gegenüber agnostisch. Sie können z. B. eine OWIN-basierten Webanwendung in IIS hosten oder selbst in einem benutzerdefinierten Prozess hosten.

Änderungen, die in den Microsoft-OWIN-Komponenten (auch bekannt als das Katana-Projekt) umfassen neue Server und Host-Komponenten, neue Bibliotheken und -Middleware und neue Authentifizierungs-Middleware.

Weitere Informationen zu OWIN und Katana, finden Sie unter [Neuigkeiten in der OWIN und Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Hinweis: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Anwendungen können nicht im klassischen Modus von IIS ausgeführt werden, müssen im integrierten Modus ausgeführt werden.**

**Hinweis: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Anwendungen mit voller Vertrauenswürdigkeit ausgeführt werden müssen.**

### <a name="new-servers-and-hosts"></a>Neue Server und -Hosts

In dieser Version wurden neue Komponenten hinzugefügt, um Self-Hosting-Szenarios zu ermöglichen. Zu diesen Komponenten gehören die folgenden NuGet-Pakete:

- **Microsoft.Owin.Host.HttpListener**. Stellt einen OWIN-Server, der verwendet **HttpListener** zum Abhören von HTTP-Anforderungen und leiten Sie ihn in die OWIN-Pipeline.
- **Microsoft.Owin.Hosting** stellt eine Bibliothek für Entwickler, die eine OWIN-Pipeline in einem benutzerdefinierten Prozess, z. B. eine Konsolenanwendung oder einen Windows-Dienst selbst hosten möchten.
- **OwinHost**. Stellt eine eigenständige ausführbare Datei, die umschließt `Microsoft.Owin.Hosting` und ermöglicht Ihnen die eine OWIN-Pipeline selbst hosten, ohne eine benutzerdefinierte hostanwendung schreiben zu müssen.

Darüber hinaus die `Microsoft.Owin.Host.SystemWeb` Paket ermöglicht jetzt die Middleware Hinweise zum Angeben der **SystemWeb** Server, der angibt, dass die Middleware in einer bestimmten Pipeline-Phase von ASP.NET aufgerufen werden soll. Diese Funktion ist besonders nützlich für die authentifizierungsmiddleware, die einem frühen Zeitpunkt in der ASP.NET-Pipeline ausgeführt werden sollen.

### <a name="helper-libraries-and-middleware"></a>Bibliotheken und Middleware

Obwohl Sie nur die Funktions- und Typnamen Definitionen von OWIN-Spezifikation, mit der OWIN-Komponenten schreiben, können die neue `Microsoft.Owin` Paket stellt einen benutzerfreundlicheren Satz an Abstraktionen bereit. Dieses Paket kombiniert mehrere frühere Pakete (z. B. `Owin.Extensions`, `Owin.Types`) in einer einzelnen, klar strukturierten Objektmodell, das dann leicht von anderen OWIN-Komponenten verwendet werden kann. Die meisten Microsoft-OWIN-Komponenten verwenden, jetzt dieses Paket.

> [!NOTE]
> [OWIN](http://www.owin.org) Anwendungen können nicht im klassischen Modus von IIS ausgeführt werden, müssen im integrierten Modus ausgeführt werden.

> [!NOTE]
> [OWIN](http://www.owin.org) Anwendungen mit voller Vertrauenswürdigkeit ausgeführt werden müssen.

Diese Version enthält außerdem das Microsoft.Owin.Diagnostics-Paket, das zum Überprüfen einer ausgeführten Anwendung für die OWIN-Middleware sowie Fehlerseite-Middleware, um den Fehler untersuchen enthält.

### <a name="authentication-components"></a>Authentication-Komponenten

Die folgenden Authentifizierungskomponenten sind verfügbar.

- **Microsoft.Owin.Security.ActiveDirectory**. Ermöglicht die Authentifizierung mit einer lokalen oder cloudbasierten Directory Services.
- **Microsoft.Owin.Security.Cookies** aktiviert die Authentifizierung mithilfe von Cookies. Dieses Paket wurde zuvor mit dem Namen `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** aktiviert die Authentifizierung mit Facebook OAuth-Dienst basiert.
- **Microsoft.Owin.Security.Google** aktiviert die Authentifizierung mithilfe Googles OpenID-basierten Dienst.
- **Microsoft.Owin.Security.Jwt** aktiviert die Authentifizierung mit JWT-Token.
- **Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.
- **Microsoft.Owin.Security.OAuth**. Stellt einen OAuth-autorisierungsserver sowie die Middleware zum Authentifizieren von Bearer-Tokens bereit.
- **Microsoft.Owin.Security.Twitter** aktiviert die Authentifizierung über Twitter OAuth-Dienst basiert.

Diese Version umfasst auch die `Microsoft.Owin.Cors` Paket, das Middleware für die Verarbeitung von Cross-Origin-HTTP-Anforderungen enthält.

> [!NOTE]
> Unterstützung für die Signierung von JWT wurde in der endgültigen Version von Visual Studio 2013 entfernt.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Eine Liste der neuen Features und andere Änderungen in Entity Framework 6, finden Sie unter [Entity Framework-Versionsverlaufs](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor-3

ASP.NET Razor 3 enthält die folgenden neuen Features:

- Unterstützung für die Registerkarte zu bearbeiten. Preivously, die **Dokument formatieren** -Befehl, automatische Einzug und automatische Formatierung in Visual Studio funktionierte nicht ordnungsgemäß bei Verwendung der **Tabulatoren beibehalten** Option. Diese Änderung behebt Visual Studio, die Formatierung für Razor-Code für die Registerkarte, die Formatierung.
- Unterstützung für URL-Rewrite-Regeln beim Generieren von Links.
- Entfernen des transparenten Sicherheitsattribut.
  > [!NOTE]
  > Dies ist eine wichtige Änderung und Razor-3 nicht kompatibel mit MVC 4 und früheren Versionen bei heruntergefahrenem Razor 2 nicht kompatibel mit MVC5 oder Assemblys, die für MVC5 kompiliert ist.

Razor 3 behobene Problemen in Visual Studio 2013 älterer Versionen finden Sie [hier](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET-App Suspend

ASP.NET App Suspend ist eine revolutionäre-Funktion in .NET Framework 4.5.1, die Benutzeroberfläche und wirtschaftlichen Gesamtmodell spielt für das Hosten von große Anzahl von ASP.NET-Websites unter einem einzelnen Computer radikal ändert. Weitere Informationen finden Sie unter [ASP.NET App Suspend – reaktionsfähiges .NET Web shsp](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET und Webtools für Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Wiederherstellen von neuen Paketen unter Mono funktioniert nicht, beim Verwenden der SLN-Datei](https://nuget.codeplex.com/workitem/3596) – wird in einer bevorstehenden nuget.exe-Download behoben und [NuGet.CommandLine Paket](http://www.nuget.org/packages/NuGet.CommandLine/) aktualisieren.
- [Neue paketwiederherstellung funktioniert nicht mit Wix-Projekten](https://nuget.codeplex.com/workitem/3598) – wird in einer bevorstehenden nuget.exe-Download behoben und [NuGet.CommandLine Paket](http://www.nuget.org/packages/NuGet.CommandLine/) aktualisieren.
- [Automatische paketwiederherstellung funktioniert nicht für Projekte unter einem Projektmappenordner](https://nuget.codeplex.com/workitem/3625) – wird in NuGet 2.8 behoben werden.

### <a name="aspnet-web-api"></a>ASP.NET-Web-API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` Gibt keinen zurück `IQueryable<T>` immer, wie wir Unterstützung für hinzugefügt `$select` und `$expand`.

    Für unseren früheren Beispielen `ODataQueryOptions<T>` immer die Typumwandlung der Rückgabewert von `ApplyTo` zu `IQueryable<T>`. Es funktioniert hat zuvor, da die Abfrage, die Optionen für uns unterstützt zuvor (`$filter`, `$orderby`, `$skip`, `$top`) ändern Sie die Form der Abfrage nicht. Nun, da wir unterstützen `$select` und `$expand` den Rückgabewert von `ApplyTo` nicht `IQueryable<T>` immer.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Wenn Sie den Beispielcode von vorhin verwenden, es werden weiterhin ausgeführt, wenn der Client keine sendet `$select` und `$expand`. Aber wenn Sie unterstützen möchten `$select` und `$expand` müssen Sie diesen Code so zu ändern.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url oder RequestContext.Url ist null, während eine Batchanforderung**

    In einem Szenario mit Batchverarbeitung **UrlHelper** ist null, wenn der Zugriff von **Request.Url** oder **RequestContext.Url**.

    Dieses Problem ist hier zurzeit nachverfolgt: [BatchRequestContext.Url ist null für die Batchverarbeitung Anforderung](http://aspnetwebstack.codeplex.com/workitem/1301).

    Die problemumgehung für dieses Problem besteht darin erstellen Sie eine neue Instanz der **UrlHelper**, wie im folgenden Beispiel:

    **Erstellen eine neue Instanz der UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Wenn MVC5- und OrgAuth, verwenden Sie Ansichten verfügen, die AntiForgerToken Validierungen, möglicherweise stoßen Sie auf den folgenden Fehler beim Senden von Daten in der Ansicht:

    **Fehler**:

    *Serverfehler in Anwendung '/'.*

    <em>Ein Anspruch des Typs "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'oder'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" war nicht vorhanden ist, auf die angegebene "ClaimsIdentity". Um Unterstützung für fälschungssicherheitstoken token mit Ansprüchen basierende Authentifizierung zu aktivieren, stellen Sie sicher, dass konfigurierten Anspruchsanbieter-Vertrauensstellung beide dieser Ansprüche auf der "ClaimsIdentity"-Instanzen bereitstellt, die sie generiert. Wenn die konfigurierten Anspruchsanbieter-Vertrauensstellung stattdessen einen anderen Anspruchstyp als eindeutiger Bezeichner verwendet, kann es durch Festlegen der statischen Eigenschaft AntiForgeryConfig.UniqueClaimTypeIdentifier konfiguriert werden.</em>

    **Problemumgehung**:

    Fügen Sie die folgende Zeile in "Global.asax", um das Problem zu beheben:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Dies wird für die nächste Version behoben.
2. Nach dem Upgrade einer MVC4 app MVC5 – erstellen Sie die Projektmappe, und starten Sie ihn aus. Sie sollten die folgende Fehlermeldung angezeigt:

    [A] System.Web.WebPages.Razor.Configuration.HostSection kann nicht umgewandelt werden, um [B]System.Web.WebPages.Razor.Configuration.HostSection. Typ A stammt von ' System.Web.WebPages.Razor, Version = 2.0.0.0, Kultur = Neutral, PublicKeyToken = 31bf3856ad364e35' im Kontext 'Default' am Speicherort ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4. 0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll ". Typ B stammt "System.Web.WebPages.Razor, Version = 3.0.0.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35' im Kontext 'Default' am Speicherort ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll ".

    Um die oben angegebene Fehler zu beheben, öffnen Sie *alle* der Web.config-Dateien (einschließlich derjenigen, die im Ordner "Views") in Ihr Projekt, und gehen Sie wie folgt:

   1. Aktualisieren Sie alle Vorkommen von Version "4.0.0.0" von "System.Web.Mvc", "5.0.0.0".
   2. Aktualisieren Sie alle Vorkommen von "System.Web.Helpers," Version "2.0.0.0" &quot;System.Web.WebPages&quot; und &quot;System.Web.WebPages.Razor&quot; auf "3.0.0.0"

      Nachdem Sie die oben genannten Änderungen vornehmen, sollten die Assemblybindungen beispielsweise wie folgt aussehen:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Wenn Sie die clientseitige Validierung mit jQuery Unobtrusive Validation verwenden zu können, ist der validierungsmeldung manchmal für eine HTML-Eingabeelement mit Typ = "Number". Der Validierungsfehler für einen erforderlichen Wert ("das Altersfeld ist erforderlich") wird angezeigt, wenn eine ungültige Anzahl die richtige Nachricht eingegeben werden, dass eine gültige Zahl erforderlich ist.

    Dieses Problem wird häufig mit eingerüsteten Code für ein Modell mit einer Zahl mit Vorzeichen in den Bearbeitungs- und änderungsansichten gefunden.

    Um dieses Problem zu umgehen, ändern Sie das Hilfsprogramm "Editor" aus:

    `@Html.EditorFor(person => person.Age)`

    Nach:

    `@Html.TextBoxFor(person => person.Age)`
4. Teilweise Vertrauenswürdigkeit wird von ASP.NET MVC 5 nicht mehr unterstützt. Projekte, die Verknüpfung mit den Binärdateien MVC- oder Web-API sollte Entfernen der [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) Attribut und die [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) Attribut. Entfernen diese Attribute werden Compilerfehler wie den folgenden entfernen.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Beachten Sie, wie ein Nebeneffekt, dass Sie diese 4.0 und 5.0-Assemblys nicht in derselben Anwendung verwenden können. Sie müssen alle auf 5.0 aktualisiert.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA-Vorlage mit Facebook-Autorisierung verursachen Instabilität in Internet Explorer, während die Website, in der Intranetzone gehostet wird

Die SPA-Vorlage enthält externe melden Sie sich mit Facebook. Wenn das Projekt erstellt haben, mit der Vorlage lokal ausgeführt wird, kann die Anmeldung bei Internet Explorer zum Absturz führen.

Projektmappe:

1. Hosten Sie die Website in der Zone des Internets. oder

2. Testen Sie das Szenario, in einem anderen Browser als Internet Explorer.

### <a name="web-forms-scaffolding"></a>Web Forms-Gerüst

Web Forms-Gerüsts aus VS2013 entfernt wurde, und es werden in einem zukünftigen Update von Visual Studio zur Verfügung stehen. Allerdings weiterhin können Gerüstbau in Web Forms-Projekten Sie durch Hinzufügen von MVC-Abhängigkeiten, und Generieren von Gerüstbau für MVC. Das Projekt enthält eine Kombination von Web Forms und MVC.

Um MVC das Web Forms-Projekt hinzuzufügen, fügen Sie ein neues gerüstelement hinzu, und wählen Sie **MVC 5-Abhängigkeiten**. Wählen Sie minimale oder vollständige, je nachdem, ob Sie alle Inhaltsdateien, z. B. Skripts benötigen. Anschließend fügen Sie eines Elements mit Gerüst für MVC, das die Ansichten und ein Controller im Projekt erstellt wird.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC und Web-API-Gerüstbau - HTTP 404-Antwort, wurde nicht gefunden-Fehler

Wenn beim Hinzufügen eines Elements mit Gerüst zu einem Projekt ein Fehler aufgetreten ist, ist es möglich, die Ihr Projekt in einem inkonsistenten Zustand gelassen werden. Einige der Gerüstbau werden die Änderungen werden rückgängig gemacht werden, aber andere Änderungen, z. B. die installierten NuGet-Pakete werden nicht zurückgesetzt werden. Wenn die Weiterleitung Änderungen an der Konfiguration ein Rollback ausgeführt werden, erhalten Benutzer einen HTTP 404-Fehler bei der Navigation zu Elementen erstellt haben.

Problemumgehung:

- Klicken Sie zum Beheben dieses Fehlers für MVC, fügen Sie ein neues gerüstelement hinzu, und wählen Sie MVC 5-Abhängigkeiten (minimale oder vollständige). Dabei werden alle erforderlichen Änderungen zu Ihrem Projekt hinzufügen.
- So beheben Sie diesen Fehler für Web-API:

  1. Fügen Sie der Klasse "webapiconfig" zu Ihrem Projekt hinzu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Konfigurieren Sie in der Anwendung WebApiConfig.Register\_Start-Methode in "Global.asax" wie folgt:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
