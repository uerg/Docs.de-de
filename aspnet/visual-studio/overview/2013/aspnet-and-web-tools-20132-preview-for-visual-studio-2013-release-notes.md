---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: "ASP.NET und Tools für Visual Studio 2013-Versionshinweise 2013.2 | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET und Webtools 2013.2 für Versionshinweisen zu Visual Studio 2013
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

ASP.NET und Webtools für Visual Studio 2013.2 kann im Rahmen des heruntergeladen werden und werden in die main-Installationsprogramm gebündelt [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentation

Lernprogramme und andere Informationen zu ASP.NET und Webtools für Visual Studio 2013.2 stehen über die [ASP.NET-Website](https://www.asp.net/).

## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET und Webtools für Visual Studio 2013.2 erfordert Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Neue Funktionen in ASP.NET und Webtools für Visual Studio 2013.2

Die folgenden Abschnitte beschreiben die Funktionen, die in der Version eingeführt wurden.

- [Vorlagen für ein ASP.NET-Projekt](#oneaspnet)
- [Unterstützung von SSL beim Starten von Webanwendungen unter IIS Express](#ssl)
- [Web-Editor von Visual Studio-Erweiterungen](#vswebeditor)
- [Browserverknüpfung](#browserlink)
- [Unterstützung für Azure App Service-Web-Apps in Visual Studio](#waws)
- [Erstellen Sie remote-Azure-Ressourcen, wenn ein neues Webprojekt erstellen](#AzureResources)
- [Web veröffentlichen Erweiterungen](#webpublish)
- [ASP.NET Gerüstbau](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN-Komponenten](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Vorlagen für ein ASP.NET-Projekt

- Updates für ASP.NET Projektvorlagen um kontobestätigung und Kennwortzurücksetzung zu unterstützen.
- ASP.NET Web API-Vorlage Update zur Unterstützung der Authentifizierung auf lokalen Organisationskonten.
- Die ASP.NET SPA-Vorlage enthält jetzt eine Authentifizierung, die Ansichten "MVC" und "Server Side basiert. Die Vorlage hat einen WebAPI-Controller, die nur durch authentifizierte Benutzer zugegriffen werden kann.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Unterstützung von SSL beim Starten von Webanwendungen unter IIS Express

Um die sicherheitswarnung, die beim Durchsuchen und Debuggen von HTTPS auf "localhost" zu vermeiden, haben wir ein Dialogfeld, um zuzulassen, dass Internet Explorer und Chrome vertrauen selbstsigniertes IIS express SSL-Zertifikat hinzugefügt.

Z. B. Festlegen einer Projekteigenschaft Web kann zur Verwendung von SSL. Klicken Sie auf F4, um das Dialogfeld "Eigenschaften" anzuzeigen. Änderung **SSL aktiviert** auf "true". Kopieren Sie die SSL-URL ein.

![SSL aktiviert-Eigenschaft](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Set Web Registerkarte "Project Eigenschaft Seite" Web "für die HTTPS-basierte URL (die SSL-URL werden `https://localhost:44300/` , wenn Sie SSL-Websites zuvor erstellt haben.)

![Festlegen der Projekt-URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Drücken Sie STRG+F5, um die Anwendung auszuführen. Führen Sie die Anweisungen, um das selbstsignierte Zertifikat vertrauen, das IIS Express generiert hat.

![SSL-Warnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lesen der **Sicherheitswarnung** Dialogfeld und klicken Sie dann auf **Ja** gegebenenfalls zum Installieren des Zertifikats, das "localhost" darstellt.

![Sicherheitshinweis](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Die Website wird in Internet Explorer oder Chrome angezeigt werden ohne zertifikatwarnung im Browser.

![Seite "HTTPS" ohne Warnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox verwendet einen eigenen Zertifikatspeicher an, damit sie eine Warnung angezeigt wird.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Web-Editor von Visual Studio-Erweiterungen

- **Neue JSON-Projektelement und -Editor**: Wir haben eine JSON-Projektelement und -Editor in Visual Studio hinzugefügt. Aktuelle JSON-Editor-Funktionen enthalten die farbliche Kennzeichnung, syntaxüberprüfung, geschweifte Klammer für Abschluss, Gliederung, optionseinstellung Tools und mehr.

    ![JSON-Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense unterstützt jetzt auch [JSON-Schema](http://json-schema.org/) v3 und v4. Es ist ein Schema Kombinationsfeld vorhandenen Schemas auswählen, bearbeiten die lokalen Schemapfad oder einfach Drag & drop eine JSON-Projektdatei, um den relativen Pfad abzurufen.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON-Schema-editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Editor für neues Sass (SCSS)**: weniger VS2013 RTM hinzugefügt, und wir verfügen jetzt über ein Projektelement Sass und -Editor. Sass-Editor-Funktionen sind vergleichbar mit dem LESS-Editor, und schließen farbliche Kennzeichnung, Variablen und Mixins IntelliSense, Kommentar/kommentieren, QuickInfo, Formatierung, syntaxüberprüfung, Gliederung, Gehe zu Definition, Farbauswahl, tools usw. Festlegen der Option.

    ![Neues Element hinzufügen: SCSS-Stylesheet](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Formatvorlagen-editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Neue URL-Auswahl in HTML, Razor, CSS, LESS und Sass Dokumente:** Lieferumfang von Visual Studio 2013 sind keine URL-Auswahl außerhalb der Web Forms-Seiten. Die neue URL-Auswahl für HTML, Razor, CSS, LESS und Sass Editoren wird ein Dialogfeld freizugeben, fluent Eingabe-Auswahl, die versteht '..' und Filterdatei führt entsprechend Img-Tags und Links.

    ![URL-Auswahl für Bildtag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL-Auswahl für Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL für CSS-Auswahl](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Updates für LESS-Editor, indem Sie weitere Funktionen hinzufügen**
- **Knockout Intellisense Upgrade**: Wir haben eine nicht standardmäßige KnockOut-Syntax für VS-IntelliSense hinzugefügt "Ko-Vs-Editor-ViewModel:" Syntax. Es kann zum Binden an mehrere Ansichtsmodelle auf einer Seite verwenden von Kommentaren in das Formular verwendet werden:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Wir bietet nun auch Unterstützung für geschachtelte ViewModel IntelliSense, damit Sie einen Drilldown in der Schachtelungshierarchie auf Objekte auf das ViewModel ausführen können.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Die angezeigten IntelilSense ist vollständiger IntelliSense-Unterstützung des JavaScript-Objekts.

    ![IntelliSense zeigt vollständige JavaScript-Objekt](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Neue URL-Auswahl in HTML, Razor, CSS, LESS und Sass Dokumente**: VS 2013 Lieferumfang keine URL-Auswahl außerhalb der Web Forms-Seiten. Die neue URL-Auswahl für HTML, Razor, CSS, LESS und Sass Editoren wird ein Dialogfeld freizugeben, fluent Eingabe-Auswahl, die versteht '..' und Filterdatei führt entsprechend Img-Tags und Links.

    ![URL-Auswahl für Bildtag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL-Auswahl für Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL für CSS-Auswahl](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browserlink

- Browserlink HTTPS-Verbindungen unterstützt und werden aufgelistet, die im Dashboard mit anderen Verbindungen als Browser das Zertifikat vertrauenswürdig ist.
- Statische HTML-Quelle-Zuordnung
- SPA-Unterstützung für das Zuordnen von Daten
- Zuordnen von Daten automatisch aktualisiert

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Unterstützung für Azure App Service-Web-Apps in Visual Studio

- **Unterstützung für Azure anmelden.**
- **Remotedebuggen und Remotesicht für Web-apps**: unterstützen wir jetzt [des Remotedebuggens für webapps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) und remote Web app Inhaltsdateien im Server-Explorer-Ansicht.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Erstellen Sie remote-Azure-Ressourcen, wenn ein neues Webprojekt erstellen

Wir haben eine Azure hinzugefügt ["Remoteressourcen erstellen"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) auf das Dialogfeld "Neues Web-Anwendung" das Kontrollkästchen. Indem Sie ihn auswählen, werden Sie der Umgebung zum Erstellen einer neuen Webanwendung, die publishing Azure-Website zum Testen und erstellen in wenigen einfachen Schritten Veröffentlichungsprofil einrichten integrieren können.

![Neues Projekt mit Azure-Ressourcen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Veröffentlichen in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web veröffentlichen Erweiterungen

- Verbesserung der benutzerfreundlichkeit für die Veröffentlichung.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Gerüstbau

- **Enum-Unterstützung:** Wenn Ihr Modell verwendeten Enumerationen, die MVC-Scaffolder generiert Dropdownliste für die Enumeration. Dabei wird die Enum-Hilfsprogramme in MVC verwendet.
- **Unterstützung für das Bootstrapping**: EditorFor Vorlagen in MVC-Gerüstbau aktualisiert, damit diese die Bootstrap-Klassen verwenden.
- **Paket-Unterstützung**: MVC und Web-API Gerüstbauer wird 5.1 Pakete für MVC und Web-API hinzufügen

Die folgenden Screenshots zeigen Gerüstbau Modelle.

- Model-Code:

     ![Model-code](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilieren Sie die Model-Code, mit der rechten Maustaste und wählen Sie **hinzufügen**, **neues Gerüstelement**.

     ![Neue Elements mit Gerüst hinzufügen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wählen Sie **MVC5-Controller mit Ansichten unter Verwendung von Entity Framework**:

     ![Hinzufügen von neuen MVC5-Controller mit Ansichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Hinzufügen eines Controllers** mithilfe des Modells:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Überprüfen Sie den generierten Code, z. B. Views/WeekdayModels/Edit.cshtml enthält `@Html.EnumDropDownListFor`: ![mit EnumDropDownListFor anzeigen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Führen Sie die Seite finden Sie unter der Enum-Combobox generiert, beachten, dass wenn ein Wert null ist zulässig, eine leere Zeichenfolge für das Kombinationsfeld ausgewählt werden kann. Z. B. die **erstellen** Seite zeigt Folgendes:

    ![Kombinationsfeld, sodass die leere Zeichenfolge](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, die im April 2014 RTM veröffentlicht wird. Hier werden die wichtigsten Punkte aus den Anmerkungen zur Version, aber überprüfen Sie die [vollständiges Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.8) für Weitere Informationen zu diesen Änderungen.

- **Windows Phone 8.1-Clientanwendungen als Ziel**: NuGet 2.8.1 jetzt unterstützt zielt auf Windows Phone 8.1-Clientanwendungen verwenden die Ziel-Framework-Moniker "WindowsPhoneApp", "WPA", "WindowsPhoneApp81" und "WPA81".
- **Patch für die Auflösung für Abhängigkeiten**: beim Auflösen von paketabhängigkeiten NuGet wurde in der Vergangenheit implementiert, eine Strategie für die niedrigste Haupt- und Nebenversionsnummern Paketversion, die Abhängigkeiten für das Paket erfüllt, auszuwählen. Im Gegensatz zu den Haupt- und Nebenversionsnummern-Version wurde die Patchversion jedoch immer die höchste Version behoben. Obwohl das Verhalten Exploit wurde, erstellt es einen mangelnde Determinismus zum Installieren von Paketen mit Abhängigkeiten.
- **DependencyVersion Switch**: Obwohl NuGet 2.8 ändert die *Standard* Verhalten zum Auflösen von Abhängigkeiten, werden außerdem hinzugefügt, präzisen Steuern Auflösungsvorgang über den Switch - DependencyVersion in Abhängigkeit der Paket-Manager-Konsole. Der Switch ermöglicht die niedrigste mögliche Version (Standardverhalten), die höchste mögliche Version oder die höchste Nebenversion oder Patchversion Auflösen von Abhängigkeiten. Diese Option funktioniert nur bei Installationspakets in der Powershell-Befehl.
- **DependencyVersion Attribut**: Zusätzlich zu den aufgeführten DependencyVersion - Switch verfügt über NuGet ebenfalls für die Fähigkeit zum Festlegen eines neuen Attributs in der Datei "nuget.config" definieren was der Standardwert ist, wenn die - DependencyVersion wechseln zulässig in einem Aufruf der Install-Package ist nicht angegeben werden. Dieser Wert wird auch über das Dialogfeld "NuGet-Paket-Manager" für alle Paketvorgängen installieren eingehalten werden. Um diesen Wert festzulegen, fügen Sie das Attribut unter mit Ihrer Datei "NuGet.config" hinzu:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Vorschau von NuGet-Vorgänge mit - Whatif**: Einige NuGet-Pakete können umfassende Abhängigkeitsdiagramme haben und daher es hilfreich sein, während der Installation, Deinstallation oder Aktualisierungsvorgang zunächst sehen, was passiert. NuGet 2.8 fügt die standard-PowerShell-was geschieht, wenn wechseln Sie zu dem Paket installieren, deinstallieren Sie Paket und Update-Paket-Befehle zum Aktivieren, Visualisierung die gesamte Schließung von Paketen, die auf die der Befehl angewendet wird.
- **Downgrade Paket**: Es ist nicht ungewöhnlich, installieren Sie eine Vorabversion von einem Paket, um neue Funktionen zu untersuchen und dann entscheiden, ob ein Rollback auf die letzte stabile Version. Bevor Sie NuGet 2.8 enthalten war dies mehreren Schritten der Vorabversion Paket und seine Abhängigkeiten deinstallieren und installieren Sie dann die frühere Version. Mit NuGet 2.8 enthalten wird jedoch das Updatepaket jetzt das gesamte Paket schließen (z. B. das Paket Abhängigkeitsstruktur) auf die vorherige Version zurückgesetzt.
- **Entwicklung Abhängigkeiten**: viele verschiedene Arten von Funktionen als NuGet-Pakete - einschließlich Tools, die verwendet werden, für die Optimierung des Entwicklungsprozesses übermittelt werden können. Diese Komponenten, während sie bei der Entwicklung eines neuen Pakets instrumentellen stehen sollte nicht verwendet werden, dass eine Abhängigkeit für das neue Paket, wenn sie später veröffentlicht. NuGet 2.8 kann ein Paket in der .nuspec-Datei als ein DevelopmentDependency selbstidentifizierung. Wenn installiert ist, werden diese Metadaten auch in der Datei "Packages.config" des Projekts hinzugefügt werden in dem das Paket installiert wurde. Wenn die Datei "Packages.config" später während nuget.exe Pack für NuGet Abhängigkeiten analysiert wird, schließt er diese Abhängigkeiten als Entwicklung Abhängigkeiten gekennzeichnet.
- **Einzelne "Packages.config" Dateien für unterschiedliche Plattformen**: bei der Entwicklung von Anwendungen für mehrere Zielplattformen ist es üblich, andere Projektdateien für jede von der jeweiligen Buildumgebungen verwenden. Es ist auch, die verschiedene NuGet-Pakete in andere Projektdateien, nutzen gemeinsam, wie Pakete verschiedene Ebenen der Unterstützung für unterschiedliche Plattformen haben. NuGet 2.8 bietet verbesserte Unterstützung für dieses Szenario durch verschiedene "Packages.config" Dateien für verschiedene plattformspezifischen-Projektdateien erstellen.
- **Fallback auf den lokalen Cache**: Obwohl NuGet-Pakete werden in der Regel wie z. B. von einem remote-Katalog verwendet die [NuGet Gallery](http://www.nuget.org) mithilfe einer Netzwerkverbindung, es gibt viele Szenarien, in denen der Client ist nicht verbunden. Ohne eine Netzwerkverbindung konnte der NuGet-Client nicht für die erfolgreiche Installation von Paketen – selbst wenn diese Pakete bereits auf dem Clientcomputer in den lokalen NuGet-Cache enthalten waren. NuGet 2.8 enthalten hinzugefügt die Paket-Manager-Konsole fallback automatische Cache.

    Die Cache-fallback-Funktion erfordert keine Befehlsargumente bestimmte. Darüber hinaus fallback Cache funktioniert zurzeit nur in der Paket-Manager-Konsole – das Verhalten funktioniert zurzeit nicht im Dialogfeld "Paket-Manager".
- **Fehlerkorrekturen**: einer der wichtigsten Fehlerbehebungen vorgenommen wurde Leistungssteigerung in der Update-Paket-Befehl zu installieren.

    Zusätzlich zu diesen Funktionen und den zuvor erwähnten Leistung Fix enthält diese Version von NuGet auch zahlreiche Programmfehlerbehebungen. Es gab 181 insgesamt Probleme, die in der Version behoben. Eine vollständige Liste der Arbeit Elemente korrigiert in NuGet 2.8 enthalten, bitte Ansicht der [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) für diese Version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

- Die Web Forms-Vorlagen zeigen nun wie Kontobestätigung und Kennwortzurücksetzung für ASP.NET Identity ausgeführt werden.
- Die Entität-Datenquellen-Steuerelement, und der dynamischen Data-Anbieter für Entity Framework 6. Für Weitere Informationen Sie im folgenden MSDN-Blogbeitrag finden: [Dynamic Data-Anbieter und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [-Attribut Routing Verbesserungen](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Bootstrap-Unterstützung für Editor-Vorlagen](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Enum-Unterstützung in den Ansichten](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unobstrusive-Unterstützung für MinLength / MaxLength-Attribute](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Unterstützung von 'this' Kontext in der unaufdringlichen Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Verschiedene [Fehlerkorrekturen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Globale Fehlerbehandlung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [-Attribut routing Erweiterungen](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Hilfe zur Seite Verbesserungen](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute-Unterstützung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON medientypformatierer](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Bessere Unterstützung für asynchrone Filter](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Für den Client Formatierung Bibliothek Analysieren der Abfrage](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Verschiedene [Fehlerkorrekturen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- Verschiedene [Fehlerkorrekturen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework wurde aktualisiert auf Version 6.1 für Common Language Runtime und Tools erforderlich sind. Entity Framework (EF) 6.1 ist ein kleineres Update auf Entity Framework 6 und umfasst eine Reihe von Fehlerkorrekturen und neue Funktionen. Ausführliche Informationen zu EF6.1, einschließlich Links zur Dokumentation für die neuen Funktionen finden Sie unter [Entity Framework-Versionsverlaufs](https://msdn.microsoft.com/data/jj574253). Die neuen Funktionen in dieser Version enthalten:

- **Konsolidierung Tooling** bietet eine konsistente Möglichkeit zum Erstellen eines neuen EF-Modells. Diese Funktion erweitert den ADO.NET Entity Data Model-Assistenten zum Erstellen von Code First-Modelle, einschließlich reverse Engineering aus einer vorhandenen Datenbank zu unterstützen. Diese Funktionen wurden zuvor in der Beta-Qualität in der EF Power Tools verfügbar.
- **Handhabung von commitfehlern Transaktion** bietet die neuen [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) welche nutzt die neu eingeführte Möglichkeit Transaktionsvorgänge abzufangen. Die **CommitFailureHandler** können Sie eine automatische Wiederherstellung bei Verbindungsfehlern, während eine Transaktion ein Commit ausgeführt.
- **' Indexattribute '** Indizes angegeben werden, indem Sie ein Attribut auf eine Eigenschaft (oder Eigenschaften) in Ihrem Code First-Modell platzieren können. Code wird einen entsprechenden Index zunächst klicken Sie dann in der Datenbank erstellt.
- **Die öffentliche API-Zuordnung** ermöglicht den Zugriff auf die Informationen EF hat, auf wie die Eigenschaften und Typen für Spalten und Tabellen in der Datenbank zugeordnet sind. In früheren Versionen war diese API interne.
- **Möglichkeit, die über die Datei App/Web.config Interceptors konfigurieren**(die Schaffung Interceptors ohne erneute Kompilierung der Anwendung hinzugefügt werden).
- **DatabaseLogger** ist ein neue Interceptor, den alle Datenbankoperationen in einer Datei protokollieren erleichtert. In Kombination mit der vorherigen Funktion können Sie diese problemlos in Protokollierung der Datenbankvorgänge für eine bereitgestellte Anwendung, ohne dass Sie neu kompilieren müssen.
- **Änderungserkennung für Migrationen Modell** wurde verbessert, sodass Gerüstbau Migrationen genauer sind; Leistung den Erkennungsprozess Änderung auch erheblich verbessert.
- **Verbesserte Leistung beim** einschließlich reduzierte Datenbankvorgänge während der Initialisierung, Generierung (Modellerstellung) Optimierungen für null Gleichheitsvergleich in LINQ-Abfragen schneller anzeigen, in Weitere Szenarien, und eine effizientere die Materialisierung der überwachten Entitäten mit mehreren Zuordnungen.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Zweistufige Authentifizierung**: ASP.NET Identity unterstützt jetzt zweistufige Authentifizierung. Zweistufige Authentifizierung bietet eine zusätzliche Sicherheitsebene für Ihre Benutzerkonten in die Groß-/Kleinschreibung, in dem Ihr Kennwort kompromittiert wird. Es gibt auch Schutz für Brute-Force-Angriffen der zweistufigen Codes.
- **Kontosperrung:** bietet eine Möglichkeit, den Benutzer zu sperren, wenn der Benutzer das Kennwort oder einer zweistufigen Codes falsch eingibt. Die Anzahl der ungültigen Versuche und den Zeitraum kann für die Benutzer gesperrt sind, konfiguriert werden. Ein Entwickler kann optional Kontosperrung für bestimmte Benutzerkonten deaktivieren sollten sie zu müssen.
- **Kontobestätigung:** die ASP.NET Identity-System unterstützt jetzt die Kontobestätigung. Dies ist ein gängiges Szenario in den meisten Websites heute, wenn Sie ein neues Konto auf der Website registrieren, Sie erfordert, um Ihre e-Mail-Adresse zu bestätigen, bevor Sie alle Elemente in der Website ausführen können. E-Mail-Bestätigung ist hilfreich, da er dadurch verhindert, dass gefälschte Konten erstellt werden. Dies ist äußerst nützlich, wenn Sie als Methode zur Kommunikation mit den Benutzern der Website z. B. Standorte, Bankgeschäfte, Ecommerce oder soziale Websites Forum-e-Mail verwenden.
- **Das Zurücksetzen des Kennworts:** Zurücksetzen des Kennworts ist ein Feature, in denen die Benutzer ihre Kennwörter zurücksetzen, wenn sie ihr Kennwort vergessen haben.
- **Sicherheitsstempel (Abmeldung everywhere):** unterstützt eine Möglichkeit, das Sicherheitstoken für den Benutzer in Fällen generieren, wenn der Benutzer ihr Kennwort oder jede andere Sicherheit ändert-bezogenen Informationen wie z. B. die zugeordnete Anmeldung (z. B. Facebook, Google, entfernen Microsoft-Konto und So weiter). Dies ist erforderlich, um sicherzustellen, dass alle Token, die mit dem alten Kennwort generiert ungültig werden. Im Beispielprojekt Wenn Sie das Kennwort des Benutzers ändern klicken Sie dann ein neues Token für den Benutzer generiert wird und alle vorherigen Token werden für ungültig erklärt. Diese Funktion bietet eine zusätzliche Sicherheitsebene für Ihre Anwendung seit, wenn Sie Ihr Kennwort ändern, Sie werden abgemeldet aus (alle anderen Browser), in dem Sie sich an der Anwendung angemeldet haben.
- **Machen Sie den Typ des Primärschlüssels ist erweiterbar, für Benutzer und Rollen werden**: In ASP.NET Identity 1.0, die der Typ des Primärschlüssels ist für Benutzer und Rollen wurde Zeichenfolgen. Dies bedeutet, wenn das System ASP.NET Identity in SQL Server mithilfe von Entity Framework persistent gespeichert wurde, haben wir Nvarchar verwendet. Es wurden viele Diskussionen diese Standardimplementierung Stapelüberlauf und basierend auf der eingehenden Feedback. Wir haben erweiterungshook bereitgestellt, in dem Sie angeben können, was der Primärschlüssel der Tabelle Benutzer und Rollen werden soll. Diese Erweiterbarkeit Hook ist besonders nützlich, wenn Sie Ihre Anwendung migrieren und die Anwendung wurde das Speichern von Benutzer-IDs GUIDs oder Ints sind.
- **IQueryable für Benutzer und Rollen unterstützen**: Unterstützung für IQueryable auf UsersStore und RolesStore, können Sie einfach die Liste von Benutzern und Rollen abrufen.
- **Löschvorgang durch die UserManager unterstützen**
- **Indizierung von Benutzernamen**: In ASP.NET Identity Entity Framework-Implementierung, wir haben hinzugefügt einen eindeutigen Index auf den Benutzernamen mithilfe der neuen ' Indexattribute ' in EF 6.1.0. Dadurch wird sichergestellt, dass immer Usernames eindeutig sind und es wurde keine Racebedingung, in dem Sie mit dem doppelten Benutzernamen annehmen konnte.
- **Verbesserte Kennwort-Validierungssteuerelement:** die Kennwort-Validierungssteuerelement, die in ASP.NET Identity 1.0 geliefert wurde, wurde ein relativ einfach Kennwort-Validierungssteuerelement, die nur die minimale Länge überprüft wurde. Es ist ein neues Kennwort-Validierungssteuerelement, die Ihnen mehr Kontrolle über die Komplexität des Kennworts bietet. Beachten Sie, dass selbst wenn Sie alle Einstellungen in dieses Kennwort aktivieren, wir Sie zweistufige Authentifizierung für die Benutzerkonten aktivieren empfehlen.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Benutzer-Manager**: können Sie die Factoryimplementierung zum Abrufen einer Instanz von UserManager aus dem OWIN-Kontext. Dieses Muster ist ähnlich wie wir zum Abrufen von AuthenticationManager von owin-Kontext für das anmelden und Abmelden verwenden. Dies ist eine empfohlene Vorgehensweise zum Erstellen einer Instanz von UserManager pro Anforderung für die Anwendung.
    - **DbContextFactory**: ASP.NET Identity Entity Framework verwendet, für das Identitätssystem in SQL Server beibehalten. Verfügt über einen Verweis auf die ApplicationDbContext, hierfür das Identitätssystem. Die DbContextFactory Middleware gibt eine Instanz von der ApplicationDbContext pro Anforderung, die Sie in Ihrer Anwendung verwenden können.
- **ASP.NET Identity Beispiele NuGet-Paket**: das Beispiele NuGet-Paket kann das Installieren und Ausführen der Beispiele für ASP.NET Identity, und befolgen die bewährten erleichtern. Dies ist ein Beispiel für ASP.NET MVC-Anwendung. Ändern Sie den Code entsprechend Ihrer Anwendung anpassen, bevor Sie dies in der Produktion bereitstellen. Das Beispiel sollte in einer leeren ASP.NET-Anwendung installiert werden. Weitere Informationen zu dem Paket finden Sie im folgenden Blogbeitrag: [Ankündigung RTM von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN-Komponenten

Es wurden viele Probleme, die in dieser Version behoben wurden. Finden Sie unter der [Versionshinweise für die 2.1.0 Release](https://katanaproject.codeplex.com/releases/view/113281) detailliertere Informationen.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Es wurden viele Probleme, die in dieser Version behoben wurden. Finden Sie unter der [Versionshinweise für die 2.0.2 Release](https://github.com/SignalR/SignalR/releases/tag/2.0.2) detailliertere Informationen.
