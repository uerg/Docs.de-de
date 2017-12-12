---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Erstellen von ASP.NET Web-in Visual Studio 2013 Projekte | Microsoft Docs
author: tdykstra
description: "In diesem Thema wird erläutert, dass die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3 hier sind einige der neuen Features für Web Development c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 96960ef56b1206374458dbbba4befffaa83c1624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Erstellen von ASP.NET-Webprojekten in Visual Studio 2013
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

> In diesem Thema wird erläutert, die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3
> 
> Hier sind einige der neuen Features für die Webentwicklung im Vergleich zu früheren Versionen von Visual Studio:
> 
> - Eine einfache Benutzeroberfläche zum Erstellen von Projekten des betreffenden Angebots [für mehrere Frameworks, die ASP.NET unterstützen](#add) (Web Forms, MVC und Web-API).
> - [ASP.NET Identity](#indauth), eine neue ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET Frameworks und funktioniert mit Webhosting-Software als IIS arbeitet.
> - Die Verwendung von [Bootstrap](#bootstrap) reaktionsfähige Design und Designumgebung-Funktionen bereitzustellen.
> - Neue Features für Web Forms, mit der nur für MVC, angeboten werden, z. B. [automatische Test projekterstellung](#testproj) und ein [Intranet-Websitevorlage](#winauth).
> 
> Informationen zum Erstellen von Webprojekten für Azure-Cloud-Diensten oder Azure Mobile Services finden Sie unter [erste Schritte mit Azure-Cloud-Dienste und ASP.NET](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-get-started/) und [eine Leaderboard-App mit Azure Mobile Services .NET erstellen Back-End-](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Dieser Artikel gilt für [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) mit [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installiert.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Webanwendungsprojekten Websiteprojekte

ASP.NET bietet Ihnen die Wahl zwischen zwei Arten von Webprojekten: *Webanwendungsprojekten* und *Websiteprojekte*. Webanwendungsprojekte für Neuentwicklungen empfohlen, und dieser Artikel gilt nur für Webanwendungsprojekte. Weitere Informationen finden Sie unter [-Webanwendungsprojekten und Websiteprojekten in Visual Studio](https://msdn.microsoft.com/en-us/library/dd547590(v=vs.120).aspx) auf der MSDN-Website.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Übersicht über die Erstellung des Webprojekts Anwendung

Die folgenden Schritte zeigen, wie Sie ein Webprojekt zu erstellen:

1. Klicken Sie auf **neues Projekt** in der **starten** Seite oder in der **Datei** Menü.
2. In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich.

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image1.png)

    Sie könne **Cloud** im linken Bereich zum Erstellen einer [Azure-Cloud-Dienst](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/en-us/library/windows/apps/dn629482.aspx), oder [Azure WebJob](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-webjobs). In diesem Thema nicht diese Vorlagen nicht behandelt.
3. Klicken Sie im rechten Bereich auf die **Projekt Application Insights hinzufügen** das Kontrollkästchen, wenn Sie System- und Verwendungsdaten für Ihre Anwendung überwachen möchten. Weitere Informationen finden Sie unter [Überwachen der Leistung in Webanwendungen](https://azure.microsoft.com/en-us/documentation/articles/app-insights-web-monitor-performance/).
4. Geben Sie Projekt **Namen**, **Speicherort**, andere Optionen, und klicken Sie dann auf **OK**.

    Die **neues ASP.NET-Projekt** Dialogfeld wird angezeigt.

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image2.png)
5. Klicken Sie auf eine Vorlage.

    ![Wählen Sie eine Vorlage](creating-web-projects-in-visual-studio/_static/image3.png)
6. Wenn Sie Unterstützung für zusätzliche Frameworks, die nicht in der Vorlage enthaltenen hinzufügen möchten, klicken Sie auf das entsprechende Kontrollkästchen. (Im Beispiel gezeigt, konnte Sie MVC und/oder Web-API zu einem Web Forms-Projekt hinzufügen.)

    ![Fügen Sie Frameworks hinzu](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Wenn Sie ein Komponententestprojekt hinzufügen möchten, klicken Sie auf **Komponententests hinzufügen**.

    ![Hinzufügen von Komponententests](creating-web-projects-in-visual-studio/_static/image5.png)
8. Wenn Sie möchten eine andere Authentifizierungsmethode als die standardmäßig die Vorlage bietet, klicken Sie auf **Authentifizierung ändern**.

    ![Authentifizierungsschaltfläche "konfigurieren"](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Authentifizierungsdialogfeld "konfigurieren"](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Erstellen Sie eine Web-app oder eine virtuelle Maschine in Azure

Visual Studio enthält Funktionen, die mit Azure Services für das Hosten von Webanwendungen verwendet werden zu erleichtern. Beispielsweise können Sie die folgenden rechts von der Visual Studio-IDE vornehmen:

- Erstellen und Verwalten von Web-apps oder virtuelle Maschinen, die Ihre Anwendung über das Internet verfügbar zu machen.
- Zeigen Sie Protokolle, die von der Anwendung erstellt wird, wie sie in der Cloud ausgeführt wird.
- Im Debugmodus befinden, Remote führen Sie aus, während die Anwendung in der Cloud ausgeführt wird.
- Viiew und andere Azure-Dienste wie SQL-Datenbanken verwalten.

Sie können [erstellen Sie ein Azure-Konto](https://www.windowsazure.com/en-us/pricing/free-trial/) , kostenlos umfasst grundlegende Dienste wie Web-apps, und wenn Sie ein MSDN-Abonnent sind können Sie [Aktivieren von Vorteilen](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , die Ihnen dabei helfen monatlicher Gutschriften gegen weitere Azure Dienste. 

Standardmäßig die **neues ASP.NET-Projekt** Dialogfeld können Sie eine Web-app oder virtuellen Computer für ein neues Webprojekt erstellen. Wenn Sie eine neue Web-app oder eine virtuelle Maschine erstellen möchten, deaktivieren Sie die **Host in der Cloud** Kontrollkästchen.

![Remoteressourcen](creating-web-projects-in-visual-studio/_static/image8.png)

Die Beschriftung für das Kontrollkästchen möglicherweise **Host in der Cloud** oder **Remoteressourcen**, und in jedem Fall der Effekt ist derselbe. Wenn Sie das Kontrollkästchen aktiviert lassen, erstellt Visual Studio eine Web-app in Azure App Service standardmäßig an. Können Sie das Dropdown-Feld ändern, um eine **VM** Falls gewünscht. Wenn Sie nicht bereits in Azure angemeldet sind, werden Sie für Azure-Anmeldeinformationen aufgefordert. Nachdem Sie sich angemeldet haben, können ein Dialogfeld so konfigurieren Sie die Ressourcen, die Visual Studio für das Projekt erstellen. Die folgende Abbildung zeigt das Dialogfeld für eine Web-app; Wenn Sie zum Erstellen eines virtuellen Computers auswählen, werden unterschiedliche Optionen angezeigt.

![Konfigurieren von Azure-App-Einstellungen](creating-web-projects-in-visual-studio/_static/image9.png)

Weitere Informationen zur Verwendung dieser Prozess zum Erstellen von Azure-Ressourcen finden Sie unter [erste Schritte mit Azure und ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) und [erstellen eine virtuelle Maschine für eine Website mit Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Der Rest dieses Artikels bietet weitere Informationen zu den verfügbaren Vorlagen sowie die Optionen an. Der Artikel führt außerdem Bootstrap, das Layout und die Designumgebung Framework, die in den Vorlagen verwendet.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web-Projektvorlagen

Visual Studio verwendet Vorlagen, Webprojekte zu erstellen. Eine Projektvorlage kann Dateien und Ordner im neuen Projekt erstellen, Installieren von NuGet-Pakete und stellen entsprechenden Beispielcode für eine rudimentäre funktionierende Anwendung. Vorlagen implementieren die aktuellsten Webstandards und dienen zum Veranschaulichen der bewährte Methoden zur Verwendung für ASP.NET Technologien als auch bieten Ihnen einen Sprung zum Erstellen Ihrer eigenen Anwendung starten.

Visual Studio 2013 bietet die folgenden Optionen für die Vorlagen für Projekte, die auf .NET 4.5 oder höher von .NET Framework abzielen:

- [Leere Vorlage](#empty)
- [Web Forms-Projektvorlage](#wf)
- [MVC-Vorlage](#mvc)
- [Web-API-Vorlage](#webapi)
- [Vorlage für die einzelnen Page-Anwendung](#spa)
- [Azure Mobile Service-Vorlage](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012-Vorlagen](#vs2012)

Sie können auch eine Visual Studio-Erweiterung, bereitstellt Installieren einer [Facebook-Vorlage](#facebook).

Informationen zum Erstellen von Projekten, die auf .NET 4 abzielen, finden Sie unter [Visual Studio 2012-Vorlagen](#vs2012) weiter unten in diesem Thema.

Informationen über das Erstellen von ASP.NET-Anwendungen für mobile Clients finden Sie unter [Mobile-Unterstützung in ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Leere Vorlage

Die leere Vorlage enthält die bare minimale Ordner und Dateien für eine ASP.NET Web-app, wie z. B. eine Projektdatei (*csproj* oder. *VBPROJ*) und ein *"Web.config"* Datei. Sie können Unterstützung für Web Forms, MVC und/oder Web-API hinzufügen, indem Sie mithilfe der Kontrollkästchen unter der **Hinzufügen von Ordnern und Verweise für core:** Bezeichnung.

Keine Authentifizierungsoptionen sind für die leere Vorlage verfügbar. Authentifizierungsfunktionen in Beispielanwendungen implementiert ist, und die leere Vorlage erstellt eine beispielanwendung nicht.

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms-Projektvorlage

Web Forms Framework bietet, dass die folgenden Funktionen, mit denen Sie schnell Websites erstellen, die vielfältige Benutzeroberfläche und die Daten werden auf Funktionen zugreifen:

- Eine WYSIWYG-Designer in Visual Studio.
- Serversteuerelemente, die Rendern von HTML und Sie können durch Festlegen von Eigenschaften und Stile anpassen.
- Eine umfangreiche Auswahl der Steuerelemente für den Datenzugriff und die Darstellung der Daten.
- Ein Ereignismodell, das Ereignisse verfügbar macht, die Sie programmieren können, wie Sie eine Clientanwendung z. B. WPF Programmieren würde.
- Automatische Beibehaltung des Status (Daten) zwischen HTTP-Anforderungen.

Erstellen einer Web Forms-Anwendung erfordert im Allgemeinen weniger Aufwand verbunden als dieselbe Anwendung mithilfe von ASP.NET MVC-Framework erstellen. Web Forms ist jedoch nicht nur für eine schnelle Anwendungsentwicklung. Es gibt viele komplexe kommerzielle Anwendungen und baut auf den Web Forms-Frameworks.

Da Web Forms-Seite und die Steuerelemente auf der Seite automatisch ein Großteil der Markup, die an den Browser gesendet wird generiert, haben Sie die Art der eine präzisere Kontrolle über den HTML-Code, die ASP.NET MVC bietet. Die deklarative Modell für die Konfiguration von Seiten und Steuerelemente minimiert die Menge an Code schreiben müssen, aber Blendet einige des Verhaltens von HTML und HTTP. Beispielsweise ist es nicht immer möglich, geben Sie genau Markup von einem Steuerelement generiert werden kann.

Das Web Forms-Framework nicht verleiten als ASP.NET MVC leicht auf Mustern basierende Entwicklungsverfahren wie z. B. [testorientierte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), [Trennung von Anliegen](http://en.wikipedia.org/wiki/Separation_of_concerns), [steuerungsumkehrcontainer) Steuerelement](http://en.wikipedia.org/wiki/Inversion_of_control), und [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Wenn schreiben, dass der Code auf diese Weise berücksichtigt werden sollen, können Sie aus; Es ist einfach nicht automatische unverändert in ASP.NET MVC-Framework. Die [ASP.NET Web Forms MVP](http://webformsmvp.com/) Projekt zeigt einen Ansatz, die die Trennung von Anliegen und die Prüfbarkeit und gleichzeitig die schnelle Entwicklung, die Web Forms erstellt wurde, zum Übermitteln von erleichtert. Microsoft SharePoint basiert auf Web Forms-MVP.

Die Web Forms-Projektvorlage erstellt eine Web Forms-beispielanwendung, die verwendet [Bootstrap](#bootstrap) um reaktionsfähiges Design und Designs Funktionen zu bieten. Die folgende Abbildung zeigt die Startseite.

![Web Forms-app-Startseite](creating-web-projects-in-visual-studio/_static/image10.png)

Weitere Informationen zu Web Forms, finden Sie unter [ASP.NET Web Forms](https://asp.net/web-forms). Informationen über die Funktionsweise der Web Forms-Projektvorlage für Sie finden Sie unter [Erstellen einer grundlegenden Web Forms-Anwendung mit Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC-Vorlage

ASP.NET MVC wurde entworfen, um Mustern basierende Entwicklungsmethoden erleichtern, z. B. [testorientierte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), [Trennung von Anliegen](http://en.wikipedia.org/wiki/Separation_of_concerns), [Umkehrung des Steuerelements](http://en.wikipedia.org/wiki/Inversion_of_control), und [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Das Framework vertraut zu machen, die Geschäftslogikschicht einer Web-Anwendung in der Darstellungsschicht trennen. Indem Sie die Anwendung in Modellen (M), Sichten (V) und Domänencontroller (C) unterteilen, kann ASP.NET MVC einfacher zu verwalten, die Komplexität in größeren Anwendungen erleichtern.

Mit ASP.NET MVC arbeiten Sie direkt mit HTML und HTTP als in Web Forms. Beispielsweise Webformulare Zustand zwischen HTTP-Anforderungen automatisch beibehalten, jedoch müssen Sie explizit in MVC code. Der Vorteil, dass das MVC-Modell ist, dass Sie die vollständige Kontrolle über genau wie Ihre Anwendung auf diese Weise ist und das Verhalten in der webumgebung nutzen können. Der Nachteil ist, dass Sie mehr Code schreiben müssen.

MVC wurde entworfen, erweiterbar, Bereitstellen von Power-Entwickler die Möglichkeit zum Anpassen von des Framework für ihre Anwendung erfordert. Darüber hinaus ist der ASP.NET MVC-Quellcode unter ein OSI-Lizenz verfügbar sind.

Das MVC-Projektvorlage erstellt eine MVC 5-beispielanwendung, die verwendet [Bootstrap](#bootstrap) um reaktionsfähiges Design und Designs Funktionen zu bieten. Die folgende Abbildung zeigt die Startseite.

![MVC-beispielanwendung](creating-web-projects-in-visual-studio/_static/image11.png)

Weitere Informationen zu MVC finden Sie unter [ASP.NET-MVC](https://asp.net/mvc). Weitere Informationen dazu, wie die MVC 4-Vorlage auswählen, finden Sie unter [Visual Studio 2012 Vorlagen](#vs2012) weiter unten in diesem Artikel.

<a id="webapi"></a>
### <a name="web-api-template"></a>Web-API-Vorlage

Die Web-API-Projektvorlage erstellt eine Beispielwebdienst basierend auf Web-API, z. B. API-Hilfeseiten auf MVC basiert.

ASP.NET Web-API ist ein Framework, das erleichtert das Erstellen von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen. ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Dienste auf .NET Framework.

Die Web-API-Projektvorlage erstellt einen Beispiel-Webdienst. Die folgenden Abbildungen zeigen die Beispiel-Hilfeseiten.

![Web-API-Hilfeseite](creating-web-projects-in-visual-studio/_static/image12.png)

![Web-API-Hilfeseite für die GET-API](creating-web-projects-in-visual-studio/_static/image13.png)

Weitere Informationen zur Web-API finden Sie unter [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Einseitige-Anwendungsvorlage

Die Vorlage für die einzelnen Seite Anwendung (SPA) erstellt eine beispielanwendung, die JavaScript HTML 5, nutzt und [KnockoutJS](http://knockoutjs.com/) auf dem Client und ASP.NET Web-API auf dem Server.

Die Option nur zur Authentifizierung für die SPA-Vorlage ist [einzelne Benutzerkonten](#indauth).

Die folgende Abbildung zeigt den Ausgangszustand der beispielanwendung, die die SPA-Vorlage erstellt.

![SPA-beispielanwendung](creating-web-projects-in-visual-studio/_static/image14.png)

Informationen zum Erstellen einer Anwendung mithilfe der SPA-Vorlage finden Sie unter [Web-API - externen Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md).

Weitere Informationen zu ASP.NET Single Page Applications sowie zu weiteren SPA-Vorlagen, die JavaScript-Frameworks als KnockoutJS verwenden können, finden Sie unter den folgenden Ressourcen:

- [ASP.NET einzelne Seite Anwendung](../../../single-page-application/index.md).
- [Grundlegendes zu Sicherheitsfunktionen in der SPA-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Single-Page-Anwendungen: Erstellen Sie moderne, reaktionsfähiger Web-Apps mit ASP.NET](https://msdn.microsoft.com/en-us/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook-Vorlage

Sie können eine [Visual Studio-Erweiterung, die eine Facebook-Vorlage stellt](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Diese Vorlage erstellt eine beispielanwendung, die entwickelt wird, um in der Facebook-Website ausgeführt wird. Es basiert auf ASP.NET MVC und Web-API für Echtzeit-Update-Funktionalität verwendet.

Keine Authentifizierungsoptionen sind verfügbar für die Facebook-Vorlage, da Facebook-Anwendungen in der Facebook-Website ausgeführt und auf Facebooks-Authentifizierung basieren.

Weitere Informationen zu ASP.NET Facebook-Anwendung finden Sie unter [Aktualisieren der Facebook-API für MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012-Vorlagen

Das Visual Studio 2013-Projekt-Erstellung Webdialogfeld stellt nicht den Zugriff auf einige Vorlagen bereit, die in Visual Studio 2012 zur Verfügung standen. Wenn Sie eine dieser Vorlagen verwenden möchten, können Sie den Visual Studio 2012-Knoten im linken Bereich des Dialogfelds Neues Projekt in Visual Studio klicken.

![Visual Studio 2012-Vorlagen](creating-web-projects-in-visual-studio/_static/image15.png)

Die **Visual Studio 2012** Knoten können Sie auswählen, dass die folgenden Webvorlagen, die Sie besitzen Zugriff auf in die Standardliste der Vorlagen für Visual Studio 2013:

- ASP.NET MVC 4-Webanwendung
- ASP.NET Dynamic Data Entities-Webanwendung
- ASP.NET AJAX-Steuerelement
- ASP.NET AJAX-Steuerelement Serverextender
- ASP.NET-Serversteuerelement

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrapping in Visual Studio 2013-Web-Projektvorlagen

Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Designumgebung-Framework von Twitter erstellt. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts dynamisch an die anderen Browser Fenstergrößen angepasst werden können. In einem wide Browserfenster sieht der Startseite ", die von der Web Forms-Vorlage erstellten wie in der folgenden Abbildung:

![Web Forms-app-Startseite](creating-web-projects-in-visual-studio/_static/image16.png)

Legen das Fenster schmaler, und verschieben Sie horizontal angeordneten Spalten in vertikaler Ausrichtung an:

![Bootstrap vertikalen Spalte Anordnung](creating-web-projects-in-visual-studio/_static/image17.png)

Schränken Sie das Fenster etwas mehr, und aktiviert die horizontale Hauptmenü in ein Symbol, das Sie klicken können, um die Erweiterung in einem Menü vertikal ausgerichtet:

![Bootstrap Menüsymbol](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap vertikale Menü](creating-web-projects-in-visual-studio/_static/image19.png)

Der Bootstrap-Designumgebung-Funktion können auch einfach eine Änderung in der Anwendung aussehen und Verhalten wirksam. Beispielsweise können Sie die folgenden Schritte aus, um das Design ändern tun.

1. Wechseln Sie in Ihrem Browser zur [http://Bootswatch.com](http://Bootswatch.com), wählen Sie ein Design aus, und klicken Sie dann auf **herunterladen**. (Dadurch werden *bootstrap.min.css* standardmäßig; abgerufen werden sollen, wenn Sie den CSS-Code zu untersuchen möchten, *bootstrap.css* anstelle der verkleinerte Version.)
2. Kopieren Sie den Inhalt der heruntergeladenen CSS-Datei.
3. Erstellen Sie in Visual Studio ein neues **Stylesheet** Datei mit dem Namen *Bootstrap-theme.css* in der *Content* Ordner, und fügen Sie das heruntergeladene CSS-code hinein.
4. Open *App\_Start/Bundle.config* , und ändern Sie *bootstrap.css* auf *Bootstrap-theme.css*.

Führen Sie das Projekt erneut aus, und die Anwendung hat ein neues Aussehen. Die folgende Abbildung zeigt die Auswirkung des Designs Amelia:

![Das Design Amelia "Bootstrap"](creating-web-projects-in-visual-studio/_static/image20.png)

Viele Bootstrap Designs sind verfügbar, kostenlosen und Premium-Versionen. Bootstrap bietet auch eine Vielzahl von Benutzeroberflächenkomponenten, z. B. [Dropdownlisten](http://twitter.github.io/bootstrap/components.html#dropdowns), [Schaltfläche Gruppen](http://twitter.github.io/bootstrap/components.html#buttonGroups), und [Symbole](http://twitter.github.io/bootstrap/base-css.html#images). Weitere Informationen zu Bootstrap, finden Sie unter [Bootstrap Standort](http://twitter.github.io/bootstrap/).

Wenn Sie die Web Forms-Designer in Visual Studio verwenden, beachten Sie, dass der Designer CSS3, nicht unterstützt, damit sie nicht alle die Auswirkungen eines Bootstrap Designs oder dynamisches layoutänderungen genau angezeigt. Web Forms-Seiten werden jedoch richtig angezeigt, wenn Sie mit einem Browser angezeigt.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Hinzufügen der Unterstützung für zusätzliche Frameworks

Wenn Sie eine Vorlage auswählen, wird das Kontrollkästchen für die von der Vorlage verwendeten Framework(s) automatisch ausgewählt. Angenommen, Sie wählen die **Web Forms** Vorlage, die **Web Forms** Kontrollkästchen ist aktiviert und kann nicht deaktiviert werden.

![Wählen Sie eine Vorlage](creating-web-projects-in-visual-studio/_static/image21.png)

![Fügen Sie Frameworks hinzu](creating-web-projects-in-visual-studio/_static/image22.png)

Sie können das Kontrollkästchen für ein Framework auswählen, die in der Vorlage enthalten ist, um Unterstützung für das Zielframework hinzuzufügen, wenn das Projekt erstellt wird. Beispielsweise, um die Verwendung von Web Forms *aspx* Seiten bei der Auswahl der MVC-Vorlage, wählen Sie die **Web Forms** Kontrollkästchen. Oder um MVC aktivieren, wenn Sie das Web Forms-Projektvorlage verwenden, klicken Sie auf die **MVC** Kontrollkästchen. Hinzufügen einer Framework ermöglicht sowohl zur Entwurfszeit als auch zur Laufzeit-Unterstützung. Z. B. Wenn Sie eine Web Forms-Projekt MVC-Unterstützung hinzugefügt haben, werden Sie das Gerüst für Controller und Ansichten erstellen können.

Wenn Sie Web Forms und MVC, in einem Projekt kombinieren, und aktivieren [Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Forms, möglicherweise unerwartete Multicastrouting hat eine URL mehrere mögliche Ziele. Die Routen, die definiert sind, werden zuerst Vorrang. Angenommen, Sie haben eine `Home` Controller und einer *Home.aspx* Seite der `http://contoso.com/home` URL wird zur *Home.aspx* beim Aufrufen der `EnableFriendlyUrls` Methode vor dem Aufrufen der `MapRoute`Methode im *RouteConfig.cs*, oder die gleiche URL wird zur Standardansicht für Ihre `Home` Controller beim Aufrufen `MapRoute` vor `EnableFriendlyUrls`.

Hinzufügen von einem Framework werden alle in der Anwendungsfunktion Beispiel nicht hinzugefügt werden. Z. B. Wenn Sie hinzufügen, Web Forms unterstützt, wenn Sie nicht die MVC-Vorlage ausgewählt haben *"default.aspx"* Startseite-Datei wird erstellt. Nur die Ordner, Dateien und zur Unterstützung von Framework erforderlichen Verweise hinzugefügt werden. Aus diesem Grund ändern nicht hinzufügen Frameworks Authentifizierungsoptionen, von Code in Beispielanwendungen erstellt, indem die Vorlagen implementiert werden. Beispielsweise, wenn Sie die leere Vorlage auswählen und hinzufügen Web Forms- oder MVC unterstützen, die **Konfigurieren der Authentifizierung** Schaltfläche wird immer noch deaktiviert.

Die Auswirkung der einzelnen Kontrollkästchen werden die in den folgenden Abschnitten kurz beschrieben.

### <a name="add-web-forms-support"></a>Web Forms-Unterstützung hinzufügen

Erstellt leere *App\_Daten* und *Modelle* Ordner und eine *"Global.asax"* Datei. Diese werden bereits erstellt durch alle Vorlagen außer der leeren Vorlage, damit durch Aktivieren des Kontrollkästchens Web Forms keinen Unterschied für andere Vorlagen macht.

Die Web Forms-Projektvorlage aktiviert Friendly URLs in der Standardeinstellung jedoch beim Hinzufügen von Web Forms-Unterstützung für andere Vorlagen durch Aktivieren des Kontrollkästchens für Web Forms Friendly URLs nicht automatisch aktiviert werden.

### <a name="add-mvc-support"></a>Hinzufügen von MVC-Unterstützung

MVC Razor und WebPages NuGet-Pakete installiert ist, erstellt leere *App\_Daten*, *Controller*, *Modelle*, und *Ansichten*Ordner erstellt *App\_starten* Ordner mit *RouteConfig.cs* Datei und erstellt *"Global.asax"* Datei.

### <a name="add-web-api-support"></a>Hinzufügen von Web-API-Unterstützung

WebApi "und" Newtonsoft.Json NuGet-Pakete installiert ist, erstellt leere *App\_Daten*, *Controller*, und *Modelle* Ordner erstellt  *App\_starten* Ordner mit *WebApiConfig.cs* Datei und erstellt *"Global.asax"* Datei.

<a id="auth"></a>
## <a name="authentication-methods"></a>Authentifizierungsmethoden

Visual Studio 2013 bietet mehrere Authentifizierungsoptionen für die Web-API, Web Forms und MVC-Vorlagen:

- [Keine Authentifizierung](#noauth)
- [Einzelne Benutzerkonten](#indauth) (ASP.NET Identity, ehemals ASP.NET-Mitgliedschaft)
- [Organisationskonten](#orgauth) (Windows Server Active Directory oder Azure Active Directory)
- [Windows-Authentifizierung](#winauth) (Intranet)

![Authentifizierungsdialogfeld "konfigurieren"](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Keine Authentifizierung

Bei Auswahl des **keine Authentifizierung**, die beispielanwendung enthält keine Web-Seiten für die Anmeldung, keine UI, der angibt, die angemeldet ist, keine Entität für eine Mitgliedschaftsdatenbank und keine Verbindungszeichenfolge für eine Mitgliedschaftsdatenbank Klassen.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Einzelne Benutzerkonten

Bei Auswahl des **einzelne Benutzerkonten**, die beispielanwendung wird zur Verwendung von ASP.NET Identity (ehemals ASP.NET-Mitgliedschaft) für die Benutzerauthentifizierung konfiguriert werden. ASP.NET Identity ermöglicht einem Benutzer durch das Erstellen eines Benutzernamens und Kennworts am Standort oder durch Anmelden mit sozialen Anbietern wie Facebook, Google, Microsoft Account oder Twitter ein Konto zu registrieren. Der Standard-Datenspeicher für Benutzerprofile in ASP.NET Identity ist eine SQL Server-LocalDB-Datenbank, die in SQL Server oder Azure SQL-Datenbank für den Produktionsstandort bereitgestellt werden kann.

Diese Funktionen sind in Visual Studio 2013 in Visual Studio 2012 identisch, aber der zugrunde liegende Code für die ASP.NET-Mitgliedschaftssystem wurde umgeschrieben wurde. Die neue Codebasis hat folgende Vorteile:

- Das neue Mitgliedschaftssystem basiert auf [OWIN](http://owin.org/) anstelle der ASP.NET-Formularauthentifizierung-Modul. Dies bedeutet, dass Sie die gleichen Authentifizierungsmechanismus verwenden können, ob Sie Web Forms- oder MVC in IIS verwenden, oder Sie Web-API oder SignalR Selbsthosting sind.
- Die neue Mitgliedschaftsdatenbank wird vom Entity Framework Code First verwaltet, und alle Tabellen werden dargestellt von Entitätsklassen, die Sie ändern können. Dies bedeutet, dass können Sie einfach das Datenbankschema und Profilbezogene Webbenutzeroberfläche an Ihre eigenen Bedürfnisse anpassen, und können problemlos zum Bereitstellen der Updates mithilfe von Code First-Migrationen.

Das neue Mitgliedschaftssystem wird automatisch in die neuen Vorlagen implementiert, und können manuell in jedes Projekt, das als Ziel .NET 4.5 verwendet implementiert oder höher sein.

ASP.NET Identity ist eine gute Wahl, wenn Sie eine Internet-Website erstellen, die ist hauptsächlich für externe Kunden. Wenn Ihre Organisation Active Directory verwendet oder Office 365 und Sie möchten, erstellen Sie ein Projekt, das aktiviert Single-Sign-on für Mitarbeiter und Geschäftspartner, die **Organisationskonten** Option möglicherweise die bessere Wahl sein.

Weitere Informationen zu den einzelnen Benutzerkonten finden Sie unter den folgenden Ressourcen:

- [www.ASP.NET/Identity](../../../identity/index.md). Dokumentation zu ASP.NET Identity auf der ASP.NET-Website.
- [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google "oauth2" und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Zeigt außerdem das Benutzerprofildaten anpassen.
- [Web-API - externen Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md)
- [Hinzufügen externer Anmeldungen an die ASP.NET-Anwendung in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Organisations-Konten

Bei Auswahl des **Organisationskonten**, wird die beispielanwendung zum Verwenden von Windows Identity Foundation (WIF) für die Authentifizierung basierend auf Benutzerkonten in Azure Active Directory (Azure AD, einschließlich Office 365) konfiguriert oder Windows Server Active Directory. Weitere Informationen finden Sie unter [Unternehmenskonto Authentifizierungsoptionen](#orgauthoptions) weiter unten in diesem Thema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows-Authentifizierung

Bei Auswahl des **Windows-Authentifizierung**, wird die beispielanwendung konfiguriert werden, um das Windows-Authentifizierung, IIS-Modul für die Authentifizierung zu verwenden. Die Domäne und Benutzer-ID des Active Directory oder dem lokalen Computer-Konto, das Windows angemeldet ist jedoch nicht einschließen benutzerregistrierung oder UI anmelden, wird die Anwendung angezeigt. Diese Option ist für Intranetwebsites vorgesehen.

Alternativ können Sie die Erstellen einer Intranetsite, die AD-Authentifizierung durch Auswahl verwendet die [On-Premises-Option "unter" Organisationskonten](#orgauthonprem). Die On-Premises-Option verwendet die Windows Identity Foundation (WIF) anstelle des Moduls für Windows-Authentifizierung. Einige zusätzliche Schritte sind erforderlich, um die lokale Option einzurichten, aber WIF Funktionen aktiviert, die nicht mit dem Modul der Windows-Authentifizierung zur Verfügung stehen. Mit WIF können Sie z. B. Anwendungszugriff in Active Directory und zum Abfragen von Verzeichnisdaten konfigurieren.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Organisations-Konto-Authentifizierungsoptionen.

Die **Konfigurieren der Authentifizierung** Dialogfeld bietet mehrere Optionen zum Azure Active Directory (Azure AD, einschließlich Office 365) oder Windows Server Active Directory (AD)-Authentifizierung:

- [Cloud - einzelne Organisation](#orgauthsingle) (Azure AD oder Azure AD Directory-Integration mit AD)
- [Cloud - Organisation mit mehreren](#orgauthmulti) (Azure AD oder Azure AD Directory-Integration mit AD)
- [Lokaler](#orgauthonprem) (AD)

Wenn Sie eine der Azure AD-Optionen ausprobieren möchten, aber kein Konto noch [klicken Sie hier, um eine Azure AD-Konto registrieren](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Wenn Sie eine der Azure AD-Optionen auswählen, das Projekt erfordert eine Datenbank, und müssen Sie sich ein globales Administratorkonto für Azure AD-Mandanten anmelden. Geben Sie den Namen und das Kennwort für ein Organisations-Konto (z. B. admin@contoso.onmicrosoft.com), die über Administratorberechtigungen für Ihren Azure AD-Mandanten verfügt.
> 
> **Geben Sie Anmeldeinformationen nicht für ein Microsoft-Konto (z. B. contoso@hotmail.com) in das Dialogfeld Anmelden.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - Authentifizierung für einzelne Organisation

![Einzelne Organisationsauthentifizierung](creating-web-projects-in-visual-studio/_static/image24.png)

Wählen Sie diese Option aus, wenn Authentifizierung für Benutzerkonten zu aktivieren, die in einem Azure AD definiert werden sollen [Mandanten](https://technet.microsoft.com/en-us/library/jj573650.aspx). Beispielsweise die Website "contoso.com" lautet, und es wird zur Verfügung gestellt werden Mitarbeiter des Unternehmens Contoso, die im Mandanten contoso.onmicrosoft.com sind. Sie wird nicht so konfigurieren Sie Azure AD, um Benutzern zu ermöglichen, von anderen Mandanten auf die Anwendung zugreifen können.

#### <a name="domain"></a>Domäne

Geben Sie die Azure AD-Domäne, die Sie einrichten die Anwendung, z. B. möchten: `contoso.onmicrosoft.com`. Haben eine [benutzerdefinierte Domäne](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), wie z. B. `contoso.com` anstelle von `contoso.onmicrosoft.com`, können Sie, die hier eingeben.

#### <a name="access-level"></a>Zugriffsebene

Wenn Abfragen oder Aktualisieren von Verzeichnisinformationen mithilfe der Graph-API, wählen in der Anwendung benötigt **einmaliges Anmelden, Verzeichnisdaten lesen** oder **einmaliges Anmelden, lesen und Schreiben von Verzeichnisdaten**. Wählen Sie andernfalls **Single Sign-On**. Weitere Informationen finden Sie unter [Anwendung Zugriffsebenen](https://msdn.microsoft.com/en-us/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) und [mithilfe der Graph-API auf Azure AD-Abfragen](https://msdn.microsoft.com/en-US/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Standardmäßig erstellt die Vorlage eine Anwendung-ID-URI für Sie durch das Anfügen des Projektnamens an die Azure AD-Domäne. Wenn der Projektname ist beispielsweise `Example` und die Domäne `contoso.onmicrosoft.com`, der Anwendung, die ID-URI wird `https://contoso.onmicrosoft.com/Example`. Wenn Sie den Anwendungs-ID-URI manuell angeben möchten, erweitern Sie die **Weitere Optionen** Textabschnitt und den Anwendungs-ID-URI in das Textfeld eingeben. Die Anwendung, die ID-URI muss mit beginnen `https://`.

Standardmäßig verfügt eine Anwendung, die bereits in Azure AD bereitgestellt wird die gleiche Anwendung-ID-URI wie die, die Verwendung von Visual Studio für das Projekt wird das Projekt mit der vorhandenen Anwendung anstatt eine neue Bereitstellung verbunden sein. Wenn Sie eine neue Anwendung an, die in diesem Fall bereitgestellt werden soll, deaktivieren Sie die **dem Anwendungseintrag überschreiben, wenn eine mit der gleichen ID bereits vorhanden ist** Kontrollkästchen.

Wenn die **überschreiben** Kontrollkästchen ist deaktiviert, und Visual Studio sucht nach einer vorhandenen Anwendung mit der gleichen Anwendung-ID-URI, erstellt er einen neuen URI durch Anfügen einer Nummer an den URI an, es war im Begriff, verwenden. Nehmen wir beispielsweise an, der Projektname ist `Example`, Sie das Textfeld leer lassen, Sie deaktivieren die **überschreiben** Kontrollkästchen und Azure AD-Mandanten hat bereits eine Anwendung mit dem URI `https://contoso.onmicrosoft.com/Example`. In diesem Fall wird eine neue Anwendung bereitgestellt werden, mit einer Anwendung, die ID-URI wie `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Bereitstellung der Anwendung in Azure AD

Um die Anwendung in Azure AD bereitstellen, oder verbinden das Projekt für eine vorhandene Anwendung, benötigt Visual Studio die Anmeldeinformationen eines globalen Administrators für die Domäne an. Beim Klicken auf **OK** in der **Konfigurieren der Authentifizierung** (Dialogfeld), werden Sie aufgefordert, den Benutzernamen und das Kennwort eines globalen Administrators für die Domäne, die Sie angegeben haben. Später, wenn Sie auf **Projekt erstellen** in der **neues ASP.NET-Projekt** Dialogfeld Visual Studio wird die Anwendung in Azure AD. Beachten Sie, dass Visual Studio bettet als Teil dieses Prozesses für den geheimen Client-Werte in der Datei "Web.config", die ein Jahr nach der Erstellung ablaufen.

Informationen zum Erstellen von Anwendungen, mit denen **Cloud – einzelne Organisation** -Authentifizierung finden Sie unter den folgenden Ressourcen:

- [Azure-Authentifizierung](../2012/windows-azure-authentication.md)
- [Hinzufügen von einmaligem Anmelden für Ihre Webanwendung mithilfe von Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Entwickeln von ASP.NET-Apps mit Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Sichern von ASP.NET Web-API bei Azure AD und Microsoft owin-Komponenten](https://msdn.microsoft.com/en-us/magazine/dn463788.aspx)

Die Lernprogramme wurden noch nicht für Visual Studio 2013 aktualisiert; Einige der welche die Lernprogramme, die Sie manuell weiterleiten erfolgt automatisch in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - Multi-Organisationsauthentifizierung

![Mehrere organisationsauthentifizierung](creating-web-projects-in-visual-studio/_static/image25.png)

Wählen Sie diese Option aus, wenn Sie Authentifizierung für Benutzerkonten, die in mehreren Azure AD definiert sind, aktivieren möchten [Mandanten](https://technet.microsoft.com/en-us/library/jj573650.aspx). Beispielsweise die Website "contoso.com" lautet, und es wird zur Verfügung gestellt werden Mitarbeiter des Unternehmens Contoso, die im Mandanten contoso.onmicrosoft.com sind und die Mitarbeiter des Unternehmens Fabrikam, die im Mandanten fabrikam.onmicrosoft.com sind.

Die Einstellungen, die Sie eingeben und die Anwendung Bereitstellungsschritt ähneln [einzelne organisationsauthentifizierung](#orgauthsingle).

Informationen zum Erstellen von Anwendungen, mit denen **Cloud – Multi-Organisation** -Authentifizierung finden Sie unter den folgenden Ressourcen:

- [Einfache Web-App-Integration mit Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) im Active Directory-Team-Blog.
- [Entwickeln von Webanwendungen mit Azure AD mit mehreren Mandanten](https://msdn.microsoft.com/en-us/library/windowsazure/dn151789.aspx) Lernprogramm. Das Lernprogramm wurde nicht für Visual Studio 2013 noch aktualisiert wurde; Einige der was das Lernprogramm leitet Sie manuell erfolgt automatisch in Visual Studio 2013.
- [Sie müssen mit Ihrer eigenen mehrere Organisationen ASP.NET App anmelden, bevor Sie sich anmelden können](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog von Vittorio Bertocci, die erläutert, wie eine allgemeine Problem Personen gelöst auftreten, wenn ein Projekt erstellen, mit mehreren organisationsauthentifizierung verwendet.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Lokale Organisations-Authentifizierung

![Lokale Organisations-Authentifizierung](creating-web-projects-in-visual-studio/_static/image26.png)

Wählen Sie diese Option, wenn Sie zum Aktivieren der Authentifizierung für Benutzerkonten, die in die Windows Server Active Directory (AD) definiert sind, und Sie keine Azure AD verwenden möchten. Sie können diese Option zum Erstellen einer Intranetsite oder einer Internetsite verwenden. Verwenden Sie für eine IIS-Site Active Directory Federation Services (ADFS) Zugriff auf AD bereitstellen. Weitere Informationen finden Sie unter [verwenden Sie die On-Premises Organisationseinheit Authentifizierung Option (ADFS) mit ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Für einen Wechsel zu Intranetsite, als Alternative können Sie auswählen [Windows-Authentifizierung](#winauth) statt dieser Option. Für die Option Windows-Authentifizierung müssen Sie eine Metadatendokument-URL angeben. Allerdings wird Windows-Authentifizierung nicht die Möglichkeit zum Steuern des Zugriffs in Active Directory-Anwendung oder auf Verzeichnisdaten Abfrage liefert.

#### <a name="on-premises-authority"></a>Lokale Autorisierungsstelle

Geben Sie eine URL, die für das Metadatendokument verweist. Das Metadatendokument enthält die Koordinaten der Autorisierungsstelle. Die Anwendung verwendet diese Koordinaten zum Steuern des Web-anmelden-Fluss.

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Geben Sie einen eindeutigen URI ein, mit denen AD kann diese Anwendung zu identifizieren, oder übernehmen Sie leer, wenn Sie Visual Studio erstellen können.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

Dieses Dokument hat einige grundlegende Hilfe für das Erstellen eines neuen ASP.NET Web-Projekts in Visual Studio 2013 bereitgestellt. Weitere Informationen zur Verwendung von für die Webentwicklung für Visual Studio finden Sie unter [https://www.asp.net/visual-studio/](../../index.md).
