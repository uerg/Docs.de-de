---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET und Web Tools 2013.2 für Visual Studio 2013 Release Notes | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 2a22c5b686cb8e02054f421f78a8fc910af7ce28
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833382"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET und Webtools 2013.2 für Visual Studio 2013 – Versionsanmerkungen
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Installationshinweise

ASP.NET and Web Tools für Visual Studio 2013.2 werden in der main-Installationsprogramm gebündelt und heruntergeladen werden kann, als Teil des [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET und Webtools für Visual Studio 2013.2 stehen über die [ASP.NET-Website](https://www.asp.net/).

## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET and Web Tools für Visual Studio 2013.2 erfordert Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Neue Features in ASP.NET and Webtools für Visual Studio 2013.2

Die folgenden Abschnitte beschreiben die Funktionen, die in der Version eingeführt wurden.

- [Vorlagen für ein ASP.NET-Projekt](#oneaspnet)
- [Beim Starten von Webanwendungen unter IIS Express SSL-Unterstützung](#ssl)
- [Visual Studio Web-Editor-Erweiterungen](#vswebeditor)
- [Browserverknüpfung](#browserlink)
- [Unterstützung für Azure App Service-Web-Apps in Visual Studio](#waws)
- [Erstellen Sie remote-Azure-Ressourcen, wenn Sie ein neues Webprojekt erstellen](#AzureResources)
- [Webveröffentlichung mit Erweiterungen](#webpublish)
- [ASP.NET-Gerüstbau](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET-Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web-API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entitätsframework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN-Komponenten](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Vorlagen für ein ASP.NET-Projekt

- Updates für ASP.NET Projektvorlagen zur Unterstützung von kontobestätigung und Kennwortzurücksetzung.
- Aktualisieren von Vorlagen für ASP.NET Web-API zur Unterstützung der Authentifizierung, die auf lokale Organisationskonten.
- Die ASP.NET SPA-Vorlage enthält nun die Authentifizierung, die Ansichten "MVC" und "Server Side basiert. Die Vorlage weist einen WebAPI-Controller, die nur durch authentifizierte Benutzer zugegriffen werden kann.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Beim Starten von Webanwendungen unter IIS Express SSL-Unterstützung

Um die sicherheitswarnung, die beim Durchsuchen und Debuggen von HTTPS auf "localhost" zu vermeiden, haben wir ein Dialogfeld, um zuzulassen, dass Internet Explorer und Chrome, vertrauen die selbstsigniertes IIS express SSL-Zertifikat hinzugefügt.

Beispielsweise kann eine Web-Projekt-Eigenschaft festgelegt werden, Verwendung von SSL. Klicken Sie auf F4, um das Dialogfeld "Eigenschaften" zu öffnen. Änderung **SSL aktiviert** auf "true". Kopieren Sie die SSL-URL ein.

![SSL aktiviert-Eigenschaft](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Richten Sie die Web-Projekt Seite Web Registerkarte für Eigenschaften für die HTTPS basierend auf URL (die SSL-URL werden `https://localhost:44300/` , sofern Sie zuvor SSL-Websites erstellt haben.)

![Legen Sie die Projekt-URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Drücken Sie STRG+F5, um die Anwendung auszuführen. Führen Sie die Anweisungen, um das selbstsignierte Zertifikat zu vertrauen, das IIS Express generiert hat.

![SSL-Warnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lesen der **Sicherheitswarnung** Dialogfeld, und klicken Sie dann auf **Ja** ggf. zum Installieren des Zertifikats, das "localhost" darstellt.

![Sicherheitswarnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Die Website in Internet Explorer oder Chrome angezeigt wird ohne die zertifikatwarnung im Browser.

![HTTPS-Seite ohne Warnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox verwendet einen eigenen Zertifikatspeicher, damit sie eine Warnung angezeigt wird.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web-Editor-Erweiterungen

- **Neues JSON-Projektelement und Editor**: Wir haben eine JSON-Projektelement und -Editor in Visual Studio hinzugefügt. Aktuelle JSON-Editor-Funktionen zählen unter anderem die farbliche Kennzeichnung, syntaxüberprüfung, Vervollständigung von Klammern, Gliederung, Tools optionseinstellung.

    ![JSON-Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Unterstützt nun die IntelliSense [JSON-Schema](http://json-schema.org/) v3 und v4. Es ist ein Schema-Kombinationsfeld vorhandenen Schemas auswählen, bearbeiten Sie den Pfad für die lokale Schema, oder einfach ziehen und Ablegen einer JSON-Projektdatei, um den relativen Pfad zu erhalten.

    ![JSON-Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON-Schema-editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Neuen Sass (SCSS) Editor**: wir hinzugefügt, in VS2013 RTM, und wir verfügen jetzt über ein Projektelement Sass und Editor. Sass-Editor-Features sind vergleichbar mit dem LESS-Editor und umfassen farbige Markierung, Variablen und Mixins IntelliSense, kommentieren/auskommentierung aufheben, QuickInfo, Formatierung, syntaxüberprüfung, Gliederung, Goto-Definition, Farbauswahl, tools usw. Festlegen der Option.

    ![Neues Element hinzufügen: SCSS-Stylesheet](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Formatvorlagen-editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Neue URL-Auswahl in HTML, Razor, CSS, LESS und Sass-Dokumente:** Lieferumfang von Visual Studio 2013 sind keine URL-Auswahl außerhalb von Web Forms-Seiten. Die neue URL-Auswahl für HTML, Razor, CSS, LESS und Sass Editoren wird ein Dialogfeld – kostenlos, fluent Typisierung-Auswahl, die versteht '..' und Filterdatei werden entsprechend Img-Tags und Links aufgeführt.

    ![URL-Auswahl für ImageTag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL-Auswahl für Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL für CSS-Auswahl](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Updates für LESS-Editor durch Hinzufügen weiterer features**
- **Upgrade von Knockout Intellisense**: Wir haben eine nicht standardmäßige KnockOut-Syntax für das Visual Studio IntelliSense, hinzugefügt "Ko-Vs-Editor" ViewModel ":" Syntax. Sie können verwendet werden, zum Binden an mehrere Modelle von anzeigen auf einer Seite, die mithilfe von Kommentaren in der Form:

    ![Knockout-Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Wir haben auch Unterstützung für geschachtelte ViewModel-IntelliSense, hinzugefügt, damit Sie tief geschachtelte Objekte auf das "ViewModel" anzeigen können.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Die IntelilSense angezeigt, ist die vollständige IntelliSense-Funktionalität des JavaScript-Objekts.

    ![IntelliSense zeigt vollständige JavaScript-Objekt](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Neue URL-Auswahl in HTML, Razor, CSS, LESS und Sass-Dokumente**: Lieferumfang von Visual Studio 2013 sind keine URL-Auswahl außerhalb von Web Forms-Seiten. Die neue URL-Auswahl für HTML, Razor, CSS, LESS und Sass Editoren wird ein Dialogfeld – kostenlos, fluent Typisierung-Auswahl, die versteht '..' und Filterdatei werden entsprechend Img-Tags und Links aufgeführt.

    ![URL-Auswahl für ImageTag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL-Auswahl für Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL für CSS-Auswahl](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browserverknüpfung

- Browserverknüpfung HTTPS-Verbindungen unterstützt und werden aufgelistet, die im Dashboard mit anderen Verbindungen, solange das Zertifikat vom Browser als vertrauenswürdig eingestuft wird.
- Statische HTML-Quelle-Zuordnung
- SPA-Unterstützung für das Zuordnen von Daten
- Zuordnen von Daten automatisch aktualisieren

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Unterstützung für Azure App Service-Web-Apps in Visual Studio

- **Melden Sie sich Unterstützung für Azure.**
- **Remotedebugging-Funktionen und Remote-Ansicht für Web-apps**: Wir unterstützen jetzt [Remotedebugging-Funktionen für Web-apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) und remote-Ansicht des Web-app-Inhaltsdateien im Server-Explorer.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Erstellen Sie remote-Azure-Ressourcen, wenn Sie ein neues Webprojekt erstellen

Wir haben eine Azure hinzugefügt ["Remoteressourcen erstellen"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) auf das Dialogfeld "Neues Web-Anwendung" das Kontrollkästchen. Indem Sie ihn auswählen, werden Sie die Erfahrung mit der Erstellung einer neuen Webanwendung, die zum Einrichten der Azure-publishing-Website für Tests und erstellen in wenigen einfachen Schritten Veröffentlichungsprofil zu integrieren.

![Neues Projekt mit Azure-Ressourcen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Veröffentlichen in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Webveröffentlichung mit Erweiterungen

- Verbesserung der benutzerfreundlichkeit für die Veröffentlichung.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET-Gerüstbau

- **Unterstützung von Enumerationen:** Wenn Ihr Modell wird mithilfe von Enumerationen, die MVC-Gerüstbauer Dropdownliste für Enumeration generiert. Dabei wird die Enum-Hilfsprogramme in MVC verwendet.
- **Unterstützung für das Bootstrapping**: aktualisiert die EditorFor-Vorlagen in MVC-Gerüstbau aus, damit diese die Bootstrap-Klassen verwenden.
- **Paket Unterstützung**: MVC und Web-API-Scaffolder fügen 5.1-Pakete für MVC und Web-API

Die folgenden Screenshots veranschaulichen die Gerüstbau-Modelle.

- Model-Code:

     ![Model-code](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilieren Sie das Model-Code, mit der rechten Maustaste und wählen Sie **hinzufügen**, **neues Gerüstelement**.

     ![Neues Gerüstelement hinzufügen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wählen Sie **MVC5-Controller mit Ansichten unter Verwendung von Entity Framework**:

     ![Hinzufügen von neuen MVC5-Controller mit Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Controller hinzufügen** mithilfe des Modells:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Überprüfen Sie den generierten Code, z. B. Views/WeekdayModels/Edit.cshtml enthält `@Html.EnumDropDownListFor`: ![anzeigen, die EnumDropDownListFor enthält.](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Führen Sie die Seite finden Sie unter der Enumeration "ComboBox" generiert, beachten, dass wenn ein Wert als null sein kann, eine leere Zeichenfolge für das Kombinationsfeld ausgewählt werden kann. Z. B. die **erstellen** Seite zeigt Folgendes:

    ![Kombinationsfeld in dem die leere Zeichenfolge](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, die im April 2014 RTM-Version veröffentlicht wird. Hier sind die wichtigsten Punkte aus den Anmerkungen zur Version, aber überprüfen Sie die [vollständigen Anmerkungen](http://docs.nuget.org/docs/release-notes/nuget-2.8) für Weitere Informationen zu diesen Änderungen.

- **Verweisen auf Windows Phone 8.1-Anwendungen**: NuGet 2.8.1 unterstützt jetzt die als Ziel Windows Phone 8.1-Anwendungen mithilfe der 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' und "WPA81" Target frameworkMoniker.
- **Patch-Auflösung für Abhängigkeiten**: beim Auflösen von paketabhängigkeiten, NuGet wurde in der Vergangenheit implementiert, eine Strategie für die niedrigste Haupt- und Nebenversionsnummern Paketversion, die Abhängigkeiten für das Paket erfüllt, auswählen. Im Gegensatz zu den Haupt- und Nebenversionsnummern-Version jedoch wurde die Patchversion immer auf die höchste Version behoben. Obwohl das Verhalten arglose war, erstellt es einen Mangel an Determinismus zum Installieren von Paketen mit Abhängigkeiten.
- **Optionen "dependencyversion" Switch**: Obwohl NuGet 2.8 ändert die *Standard* Verhalten zum Auflösen von Abhängigkeiten werden außerdem hinzugefügt, eine genauere Steuerung der Abhängigkeit Lösungsprozess über den Switch - Optionen "dependencyversion" in der Paket-Manager-Konsole. Der Schalter aktiviert Auflösen von Abhängigkeiten, um die niedrigste mögliche Version (Standardverhalten), die höchste mögliche Version oder die höchste Nebenversion oder Patch-Version. Diese Option funktioniert nur für Install-Package, in der Powershell-Befehl.
- **Optionen "dependencyversion"-Attribut**: Zusätzlich zu den oben beschriebenen Switch - Optionen "dependencyversion" verfügt über NuGet auch die Möglichkeit, legen Sie ein neues Attribut in der Datei "NuGet.config", was der Standardwert ist, definieren, wenn die - Optionen "dependencyversion" wechseln zulässig Bei einem Aufruf von Install-Package ist nicht angegeben werden. Dieser Wert wird auch durch das Dialogfeld "NuGet-Paket-Manager" für beliebige Vorgänge der Install-Package berücksichtigt werden. Um diesen Wert festzulegen, fügen Sie das Attribut unter zu Ihrer Datei "NuGet.config" hinzu:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Vorschau der NuGet-Vorgänge mit - Whatif**: Einige NuGet-Pakete können die Tiefe von Abhängigkeitsdiagrammen haben und daher es hilfreich sein, während einer Installation, deinstallieren oder aktualisieren, lesen zunächst, was passiert, kann. NuGet 2.8 fügt die Standardversion von PowerShell-Schalter was geschieht, wenn auf die Befehle "Install-Package-Paket deinstallieren und Update-Package" So aktivieren Sie die Visualisierung des gesamten Abschluss von Paketen, die auf die der Befehl angewendet wird.
- **Ein Downgrade Paket**: Es ist nicht ungewöhnlich, mit eine Vorabversion von einem Paket zu installieren, um neue Funktionen zu untersuchen und dann entscheiden, ob ein Rollback auf die letzte stabile Version. Vor NuGet 2.8 war dies ein mehrstufiger Prozess deinstalliert werden, die Vorabversion Paket und seine Abhängigkeiten, und anschließend die frühere Version installieren. Mit NuGet 2.8 führt allerdings-Paket für das Update jetzt die gesamte paketabschluss (z.B. der Paket Abhängigkeit-Struktur) auf die vorherige Version Rollback.
- **Entwicklungsabhängigkeiten**: viele verschiedene Arten von Funktionen, die als NuGet-Pakete – einschließlich Tools, die verwendet werden, für die Optimierung des Entwicklungsprozesses übermittelt werden können. Diese Komponenten können sie dazu entwickeln ein neues Paket sein sollte, dass eine Abhängigkeit für das neue Paket, wenn sie später veröffentlicht nicht berücksichtigt. NuGet 2.8 kann es sich um ein Paket aus, um sich in der NuSpec-Datei als ein DevelopmentDependency zu identifizieren. Bei der Installation werden diese Metadaten auch die Datei "Packages.config" des Projekts hinzugefügt werden in dem das Paket installiert wurde. Wenn die Datei "Packages.config" später während nuget.exe-Pack für NuGet-Abhängigkeiten analysiert wird, schließt er diese Abhängigkeiten als entwicklungsabhängigkeiten gekennzeichnet.
- **Einzelne Datei "Packages.config"-Dateien für verschiedene Plattformen**: beim Entwickeln von Anwendungen für mehrere Zielplattformen ist es üblich, verschiedene Projektdateien für jede von der jeweiligen Buildumgebungen verwenden. Es ist auch üblich, die verschiedene NuGet-Paketen in verschiedenen Projektdateien verwenden, wie Pakete verschiedene Ebenen der Unterstützung für verschiedene Plattformen haben. NuGet 2.8 bietet verbesserte Unterstützung für dieses Szenario durch verschiedene "Packages.config"-Dateien für die verschiedenen plattformspezifischen-Projektdateien erstellen.
- **Fallback auf den lokalen Cache**: Obwohl NuGet-Pakete werden in der Regel wie z. B. aus einem Remotekatalog verbraucht die [NuGet-Katalog](http://www.nuget.org) mithilfe einer Netzwerkverbindungs viele Szenarios, in dem der Client ist nicht verbunden, werden. Ohne eine Netzwerkverbindung konnte der NuGet-Client nicht für die erfolgreiche Installation von Paketen – selbst wenn diese Pakete bereits auf dem Clientcomputer im lokalen NuGet-Cache enthalten waren. NuGet 2.8 hinzugefügt der Paket-Manager-Konsole automatisch nach fallback Cacheservern.

    Die Cache-fallback-Funktion erfordert keine bestimmten Befehlsargumente. Darüber hinaus Cache fallback funktioniert derzeit nur in der Konsole des Paket-Manager: das Verhalten funktioniert derzeit nicht im Dialogfeld "Paket-Manager".
- **Fehlerbehebungen**: eines der wichtigsten Fehlerbehebungen vorgenommen wurde leistungsverbesserungen in der Update-Package-Befehl zu installieren.

    Zusätzlich zu diesen Features und die oben genannten Leistung Fehlerbehebung enthält diese Version von NuGet auch viele andere Fehlerkorrekturen. Gab es 181 insgesamt in den Release behobenen Problemen. Eine vollständige Liste der Arbeit Elemente eine feste in NuGet 2.8, bitte Ansicht der [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) für diese Version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

- Die Web Forms-Vorlagen zeigen jetzt Vorgehensweise Kontobestätigung und Kennwortzurücksetzung für ASP.NET Identity.
- Das Entity Data Source-Steuerelement und der dynamischen-Datenanbieter für Entity Framework 6. Weitere Informationen finden Sie im folgende MSDN-Blog: [Dynamic Data-Anbieter und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Attribut-Routing-Verbesserungen](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Bootstrap-Unterstützung für Editor-Vorlagen](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Unterstützung von Enumerationen in Ansichten](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unobstrusive-Unterstützung für MinLength / MaxLength-Attribute](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Die "this"-Kontext unterstützen in unaufdringlichen Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web-API 2.1.2

- [Globale Fehlerbehandlung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Attribut-routing-Verbesserungen](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Verbesserungen der Hilfe auf der Eigenschaftenseite](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute-Unterstützung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON medientypformatierer](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Bessere Unterstützung für Async-Filter](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Für den Client Library Formatierung Analysieren der Abfrage](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entitätsframework 6.1

Entitätsframework wurde auf Version 6.1 für Laufzeit und der Tools aktualisiert. Entity Framework (EF) 6.1 ist eine Aktualisierung auf Entity Framework 6 und umfasst eine Reihe von Fehlerbehebungen und neue Features. Ausführliche Informationen zu EF6.1, einschließlich Links zur Dokumentation für die neuen Funktionen finden Sie unter [Entity Framework-Versionsverlaufs](https://msdn.microsoft.com/data/jj574253). Die neuen Funktionen in dieser Version umfassen:

- **Konsolidierung Tools** bietet eine einheitliche Möglichkeit zum Erstellen eines neuen EF-Modells. Diese Funktion erweitert den ADO.NET Entity Data Model-Assistenten zum Erstellen von Code First-Modelle auch reverse Engineering aus einer vorhandenen Datenbank zu unterstützen. Diese Features waren zuvor in der Beta-Qualität in EF Power Tools verfügbar.
- **Behandlung von Fehler bei Commit** bietet die neuen [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) die mit der der neu eingeführten Möglichkeit Transaktionsvorgänge abfangen. Die **CommitFailureHandler** ermöglicht eine automatische Wiederherstellung bei Verbindungsfehlern, während ein Commit der Transaktion durchgeführt.
- **IndexAttribute** Indizes angegeben werden, indem Sie ein Attribut auf eine Eigenschaft (oder Eigenschaften) in Ihrem Code First-Modell platzieren können. Code wird zunächst klicken Sie dann einen entsprechenden Index in der Datenbank erstellt.
- **Die öffentliche API-Zuordnung** ermöglicht den Zugriff auf die Informationen, die EF hat, auf wie die Eigenschaften und Typen für Spalten und Tabellen in der Datenbank zugeordnet werden. In früheren Versionen war diese API interne.
- **Möglichkeit zum Konfigurieren von Interceptors über die Datei App/Web.config**(interceptoren, um ohne Erneutes Kompilieren der Anwendung hinzugefügt werden können).
- **DatabaseLogger** ist ein neuer Interceptor, die es einfach macht, die alle Datenbankvorgänge in einer Datei zu protokollieren. In Kombination mit der vorherigen Funktion können Sie ganz einfach zur Protokollierung von Datenbankvorgängen für eine bereitgestellte Anwendung, ohne neu kompilieren zu wechseln.
- **Änderungserkennung für Migrationen Modell** wurde verbessert, damit erstellte Migrationen sind eine präzisere; Leistung des Änderungsprozesses Erkennung wurde ebenfalls deutlich verbessert.
- **Leistungsverbesserungen** einschließlich reduzierten Datenbankvorgänge während der Initialisierung generieren (Modellerstellung) Optimierungen für Vergleiche von null auf Gleichheit in LINQ-Abfragen schneller anzeigen, in anderen Szenarien und effizienter die Materialisierung der nachverfolgten Entitäten mit mehreren Zuordnungen.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Zweistufige Authentifizierung**: ASP.NET Identity unterstützt jetzt zweistufige Authentifizierung. Zwei-Faktor-Authentifizierung bietet eine zusätzliche Sicherheitsebene zu Benutzerkonten in dem Fall, in dem Ihr Kennwort kompromittiert wird. Es gibt auch Schutz für brute-Force-Angriffen der zweistufigen Codes.
- **Kontosperrung:** bietet eine Möglichkeit, den Benutzer zu sperren, wenn der Benutzer das Kennwort oder einer zweistufigen Codes falsch eingibt. Die Anzahl der ungültigen und den Zeitraum kann für die Benutzer gesperrt werden, konfiguriert werden. Entwickler kann optional Sperrung von Konten für bestimmte Benutzerkonten deaktivieren sie Sie benötigen sollte.
- **Kontobestätigung:** der ASP.NET Identity-System unterstützt jetzt die Kontobestätigung. Dies ist ein gängiges Szenario in den meisten Websites noch heute, wenn Sie ein neues Konto auf der Website registrieren, Sie erforderlich sind, Ihre e-Mail-Adresse zu bestätigen, bevor Sie alle Elemente in der Website ausführen können. E-Mail-Bestätigung ist hilfreich, da es dadurch verhindert, dass gefälschte Konten erstellt werden. Dies ist äußerst nützlich, wenn Sie e-Mail-Adresse als eine Methode für die Kommunikation mit der Benutzer Ihrer Website wie z. B. Forum-Websites, Banking, e-Commerce- oder soziale Websites verwenden.
- **Zurücksetzen des Kennworts:** Zurücksetzen des Kennworts ist ein Feature, kann Benutzer ihre Kennwörter zurücksetzen, wenn sie ihr Kennwort vergessen haben.
- **Der Sicherheitsstempel (Abmeldung überall):** unterstützt eine Möglichkeit zum erneuten Generieren der Sicherheitstoken des Benutzers in Fällen, wenn der Benutzer ihre Kennwörter oder jede andere Sicherheit ändert-bezogenen Informationen wie z. B. die zugeordnete Anmeldung (z. B. Facebook, Google, entfernen Microsoft-Konto und So weiter). Dies ist erforderlich, um sicherzustellen, dass alle Token generiert, mit dem alten Kennwort ungültig sind. Im Beispielprojekt Wenn Sie das Kennwort des Benutzers ändern klicken Sie dann ein neues Token für den Benutzer generiert und werden alle vorherigen Token für ungültig erklärt. Diese Funktion bietet eine zusätzliche Sicherheitsebene für Ihre Anwendung seit, wenn Sie Ihr Kennwort ändern, Sie werden abgemeldet von überall her (alle anderen Browser), in dem Sie sich diese Anwendung angemeldet haben.
- **Ändern Sie den Typ des Primärschlüssels für Benutzer und Rollen erweiterbar sein**: In ASP.NET Identity 1.0, die der Typ des primären Schlüssels für die Tabelle-Benutzer und Rollen wurde Zeichenfolgen. Dies bedeutet, wenn die ASP.NET Identity-System in SQL Server mithilfe von Entity Framework persistent gespeichert wurde, haben wir Nvarchar verwendet. Es gab viele Diskussionen rund um diese Standardimplementierung auf Stack Overflow und basierend auf dem eingehenden Feedback. Wir haben eine erweiterungsmöglichkeit bereitgestellt, in dem Sie angeben können, was den Primärschlüssel der Tabelle Benutzer und Rollen werden soll. Dieser erweiterungsmöglichkeit ist besonders nützlich, wenn Sie Ihre Anwendung migrieren und die Anwendung war das Speichern von Benutzer-IDs GUIDs oder Ints sind.
- **"IQueryable" für Benutzer und Rollen unterstützen**: Unterstützung für die "IQueryable" UsersStore und RolesStore, können Sie einfach die Liste von Benutzern und Rollen abrufen.
- **Unterstützt die Delete-Vorgang, durch die UserManager**
- **Indizierung für Benutzernamen**: In ASP.NET Identity Entity Framework-Implementierung, wir haben hinzugefügt einen eindeutigen Index auf den Benutzernamen mithilfe der neuen IndexAttribute in EF 6.1.0. Dadurch wird sichergestellt, dass Benutzernamen immer eindeutig sind, und es keine Racebedingung gab, in dem Sie mit dem doppelten Benutzernamen letztlich.
- **Verbesserte Kennwort-Validierungssteuerelement:** die Kennwort-Validierungssteuerelement, das in ASP.NET Identity 1.0 geliefert wurde, war ein ziemlich einfache Kennwort-Validierungssteuerelement, das nur die minimale Länge überprüft wurde. Es gibt ein neues Kennwort-Validierungssteuerelement, das Ihnen mehr Kontrolle über die Komplexität des Kennworts bietet. Bitte beachten Sie, dass selbst wenn Sie alle Einstellungen in dieses Kennwort aktivieren, zweistufige Authentifizierung für die Benutzerkonten aktivieren empfohlen.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Benutzer-Manager**: Sie können Factoryimplementierung verwenden, um eine Instanz UserManager aus dem OWIN-Kontext abzurufen. Dieses Muster ist ähnlich wie wir zum Abrufen von AuthenticationManager von OWIN-Kontext für die Anmeldung und Abmeldung verwenden. Dies ist eine empfohlene Methode zum Abrufen einer Instanz UserManager pro Anforderung für die Anwendung.
    - **DbContextFactory**: ASP.NET Identity verwendet Entity Framework für das Identitätssystem in SQL Server beibehalten. Zu diesem Zweck, dass das Identitätssystem verfügt über einen Verweis auf die ApplicationDbContext aus. Die DbContextFactory Middleware gibt eine Instanz der ApplicationDbContext pro Anforderung, mit denen Sie in Ihrer Anwendung zurück.
- **ASP.NET Identity-Beispiele-NuGet-Paket**: das Beispiele für NuGet-Paket kann das Installieren und Ausführen von Beispielen für ASP.NET Identity und führen die bewährten Methoden erleichtern. Dies ist ein Beispiel für ASP.NET MVC-Anwendung. Ändern Sie den Code, um Ihre Anwendung zu entsprechen, bevor Sie diese in der Produktion bereitstellen. Das Beispiel sollte in eine leere ASP.NET-Anwendung zur installiert werden. Weitere Informationen zum Paket finden Sie unter der folgenden Blogbeitrag: [Ankündigung RTM von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN-Komponenten

Es gab viele Fehler, die in dieser Version behoben wurden. Informieren Sie sich die [der 2.1.0 Anmerkungen zu dieser Version Version](https://katanaproject.codeplex.com/releases/view/113281) Ausführlichere Informationen.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Es gab viele Fehler, die in dieser Version behoben wurden. Informieren Sie sich die [der 2.0.2 Anmerkungen zu dieser Version Version](https://github.com/SignalR/SignalR/releases/tag/2.0.2) Ausführlichere Informationen.
