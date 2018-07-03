---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Erstellen von ASP.NET Web-in Visual Studio 2013 Projekte | Microsoft-Dokumentation
author: tdykstra
description: In diesem Thema wird erläutert, dass die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3 hier einige der neuen Features für Web Development c werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363467"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Erstellen von ASP.NET-Webprojekten in Visual Studio 2013
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> In diesem Thema wird erläutert, die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3
> 
> Hier sind einige der neuen Features für die Webentwicklung im Vergleich zu früheren Versionen von Visual Studio:
> 
> - Eine einfache Benutzeroberfläche zum Erstellen von Projekten dieses Angebot [Unterstützung für mehrere ASP.NET Frameworks](#add) (Web Forms, MVC und Web-API).
> - [ASP.NET Identity](#indauth), ein neues ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET-Framework-Versionen und funktioniert mit Web-Software als IIS-hosting funktioniert.
> - Die Verwendung von [Bootstrap](#bootstrap) um reaktionsschnelle Entwurfs- und Designfunktionen bereitzustellen.
> - Neue Features für Web Forms, mit der nur für MVC, angeboten werden, z. B. [automatische Erstellung von Testprojekten](#testproj) und [Intranet-Websitevorlage](#winauth).
> 
> Weitere Informationen zum Erstellen von Webprojekten für Azure Cloud Services oder Azure Mobile Services zu erhalten, finden Sie unter [erste Schritte mit Azure Cloud Services und ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) und [beim Erstellen einer Leaderboard-App mit Azure Mobile Services .NET Back-End-](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Dieser Artikel gilt für [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) mit [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installiert.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Von Webanwendungsprojekten und Websiteprojekten

ASP.NET bietet Ihnen die Wahl zwischen zwei Arten von Webprojekten: *web-Anwendungsprojekte* und *Websiteprojekte*. Webanwendungsprojekte für die neue Entwicklung empfohlen, und dieser Artikel gilt nur für Webanwendungsprojekte. Weitere Informationen finden Sie unter [von Webanwendungsprojekten und Websiteprojekten in Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) auf der MSDN-Website.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Übersicht über die Erstellung des Webprojekts-Anwendung

Die folgenden Schritte zeigen, wie Sie ein Webprojekt zu erstellen:

1. Klicken Sie auf **neues Projekt** in die **starten** Seite oder in der **Datei** Menü.
2. In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich.

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image1.png)

    Können Sie **Cloud** im linken Bereich zum Erstellen einer [Azure-Clouddienst](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), oder [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). In diesem Thema nicht in der diese Vorlagen enthalten.
3. Klicken Sie im rechten Bereich auf die **Application Insights zu Projekt hinzufügen** das Kontrollkästchen, wenn Sie die nutzungsüberwachung der Integrität und Ihrer Anwendung möchten. Weitere Informationen finden Sie unter [Leistungsüberwachung in Webanwendungen](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Geben Sie Projekt **Namen**, **Speicherort**, andere Optionen, und klicken Sie dann auf **OK**.

    Die **neues ASP.NET-Projekt** Dialogfeld wird angezeigt.

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image2.png)
5. Klicken Sie auf eine Vorlage.

    ![Wählen Sie eine Vorlage](creating-web-projects-in-visual-studio/_static/image3.png)
6. Wenn Sie Unterstützung für zusätzliche Frameworks, die nicht enthalten, in der Vorlage hinzufügen möchten, klicken Sie auf das entsprechende Kontrollkästchen. (Im Beispiel gezeigt, können Sie MVC und/oder Web-API zu einem Web Forms-Projekt hinzufügen.)

    ![Fügen Sie Frameworks hinzu](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Wenn Sie ein Komponententestprojekt hinzufügen möchten, klicken Sie auf **Komponententests hinzufügen,**.

    ![Hinzufügen von Komponententests](creating-web-projects-in-visual-studio/_static/image5.png)
8. Wenn Sie möchten eine andere Authentifizierungsmethode als was standardmäßig die Vorlage enthält, klicken Sie auf **Authentifizierung ändern**.

    ![Authentifizierungsschaltfläche "konfigurieren"](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Dialogfeld "Authentifizierung" Konfigurieren](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Erstellen Sie eine Web-app oder den virtuellen Computer in Azure

Visual Studio enthält Funktionen, die Arbeit mit Azure-Dienste zum Hosten von Webanwendungen erleichtern. Beispielsweise können Sie alle der folgenden direkt aus Visual Studio-IDE ausführen:

- Erstellen und Verwalten von Web-apps oder virtuelle Computer, die Ihre Anwendung über das Internet verfügbar zu machen.
- Anzeigen von Protokollen, die von der Anwendung erstellt wird, wie sie in der Cloud ausgeführt wird.
- Im Debugmodus Remote führen Sie aus, während die Anwendung in der Cloud ausgeführt wird.
- Viiew und anderen Azure-Diensten wie SQL-Datenbanken verwalten.

Sie können [erstellen Sie ein Azure-Konto](https://www.windowsazure.com/pricing/free-trial/) , umfasst grundlegende Dienste wie Web-apps kostenlos, und wenn Sie ein MSDN-Abonnent sind können Sie [Vorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , die von einer Gutschrift für zusätzliche Azure bieten Dienste. 

In der Standardeinstellung die **neues ASP.NET-Projekt** Dialogfeld können Sie eine Web-app oder virtuellen Computer für ein neues Webprojekt erstellen. Wenn Sie eine neue Web-app oder den virtuellen Computer erstellen möchten, deaktivieren Sie die **in der Cloud hosten** Kontrollkästchen.

![Remoteressourcen erstellen](creating-web-projects-in-visual-studio/_static/image8.png)

Die Beschriftung für das Kontrollkästchen möglicherweise **in der Cloud hosten** oder **Remoteressourcen erstellen**, und der Effekt ist in beiden Fällen identisch. Wenn Sie das Kontrollkästchen aktiviert lassen, erstellt Visual Studio eine Web-app in Azure App Service in der Standardeinstellung an. Können Sie im Dropdown-Menü zum Ändern einer **VM** Falls gewünscht. Wenn Sie nicht bereits in Azure angemeldet sind, werden Sie für Azure-Anmeldeinformationen aufgefordert. Nachdem Sie sich angemeldet haben, kann ein Dialogfeld, das Sie die Ressourcen zu konfigurieren, die für Ihr Projekt Visual Studio erstellt. Die folgende Abbildung zeigt das Dialogfeld für eine Web-app; Es werden unterschiedliche Optionen angezeigt, wenn Sie einen virtuellen Computer erstellen möchten.

![Konfigurieren von Azure-App-Einstellungen](creating-web-projects-in-visual-studio/_static/image9.png)

Weitere Informationen dazu, wie Sie diesen Prozess verwenden, zum Erstellen von Azure-Ressourcen finden Sie unter [erste Schritte mit Azure und ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) und [Erstellen eines virtuellen Computers für eine Website mit Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Im weiteren Verlauf dieses Artikels bietet weitere Informationen zu den verfügbaren Vorlagen sowie die Optionen an. Der Artikel beschreibt auch die Bootstrap-Stil, das Layout und die Design-Framework, die in den Vorlagen verwendet.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013-Web-Projektvorlagen

Visual Studio verwendet Vorlagen, um Webprojekte zu erstellen. Eine Projektvorlage kann Dateien und Ordner im neuen Projekt erstellen, Installieren von NuGet-Pakete und stellen entsprechenden Beispielcode für eine einfache, funktionierende Anwendung. Vorlagen, implementieren die aktuellsten Webstandards und sollen veranschaulichen, dass die bewährte Methoden zur Verwendung auf ASP.NET-Technologien sowie bieten Ihnen einen Sprung zum Erstellen Ihrer eigenen Anwendung starten.

Visual Studio 2013 bietet die folgenden Optionen für die Vorlagen für Webprojekte für Projekte, die auf .NET 4.5 oder höher von .NET Framework ausgerichtet:

- [Leere Vorlage](#empty)
- [Web Forms-Vorlage](#wf)
- [MVC-Vorlage](#mvc)
- [Web-API-Vorlage](#webapi)
- [Einzelvorlage-Page-Anwendung](#spa)
- [Azure Mobile Service-Vorlage](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012-Vorlagen](#vs2012)

Sie können auch installieren, die bietet Visual Studio-Erweiterung eine [Facebook-Vorlage](#facebook).

Weitere Informationen zum Erstellen von Projekten, die auf .NET 4 abzielen, finden Sie unter [Visual Studio 2012-Vorlagen](#vs2012) weiter unten in diesem Thema.

Informationen zum Erstellen von ASP.NET-Anwendungen für mobile Clients finden Sie unter [Mobile-Unterstützung in ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Leere Vorlage

Die leere Vorlage bietet bare minimale-Ordner und Dateien für eine ASP.NET Web-app wie z. B. eine Projektdatei (*csproj* oder. *VBPROJ*) und ein *"Web.config"* Datei. Sie können Unterstützung für Web Forms, MVC und/oder Web-API hinzufügen, indem Sie mithilfe der Kontrollkästchen unter der **fügen Sie Ordner und kernreferenzen für:** Bezeichnung.

Für die leere Vorlage stehen keine Authentifizierungsoptionen. Authentifizierung-Funktionalität ist in der Beispielanwendungen implementiert, und die leere Vorlage erstellt keine Beispiel-App.

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms-Vorlage

Die Web Forms-Framework bietet, dass die folgenden Funktionen, mit denen Sie schnell Websites erstellen, die vielfältige Benutzeroberfläche und die Daten sind auf Features zugreifen:

- Einen WYSIWYG-Designer in Visual Studio.
- Serversteuerelemente, die Rendern von HTML und, dass Sie durch Festlegen von Eigenschaften und Stile anpassen können.
- Eine umfangreiche Auswahl an Steuerelemente für den Datenzugriff und die Daten angezeigt werden soll.
- Ein Ereignismodell, das Ereignisse verfügbar macht, die Sie programmieren können, wie Sie eine Client-Anwendung, z. B. WPF Programmieren würde.
- Automatische Beibehaltung des Status (Daten) zwischen HTTP-Anforderungen.

Erstellen einer Web Forms-Anwendung ist im Allgemeinen weniger Aufwand verbunden als das Erstellen der gleichen Anwendung mithilfe von ASP.NET MVC-Framework erforderlich. Web Forms ist jedoch nicht nur für eine schnelle Anwendungsentwicklung. Es gibt viele komplexe kommerzielle Anwendungen und baut auf den Web Forms-Frameworks.

Da Web Forms-Seite und die Steuerelemente auf der Seite automatisch ein Großteil des Markups zu, die an den Browser gesendet wird generieren, müssen Sie nicht die Art des eine präzisere Kontrolle über den HTML-Code, die ASP.NET MVC bietet. Mit dem deklarative Modell zum Konfigurieren von Seiten und Steuerelemente minimiert die Menge an Code schreiben, aber Blendet einen Teil des Verhaltens von HTML und HTTP. Beispielsweise ist es nicht immer möglich, geben Sie genau welches Markup wird von einem Steuerelement generiert werden kann.

Die Web Forms-Framework eignet sich nicht als ohne weiteres als ASP.NET MVC Mustern basierende Entwicklungsverfahren wie z. B. [testgesteuerte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), [Trennung der Belange](http://en.wikipedia.org/wiki/Separation_of_concerns), [Umkehrung des Steuerelement](http://en.wikipedia.org/wiki/Inversion_of_control), und [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Wenn Sie schreiben, dass der Code auf diese Weise aufgeteilt werden sollen, können Sie folgende Schritte ausführen; Es ist einfach nicht als automatische, wie in ASP.NET MVC-Framework. Die [MVP für ASP.NET Web Forms](http://webformsmvp.com/) Projekt zeigt einen Ansatz, der ermöglicht die Trennung von Bereichen und Prüfbarkeit und gleichzeitig die schnelle Entwicklung, die Web Forms erstellt wurde, um zu übermitteln. Microsoft SharePoint basiert auf Web Forms-MVP.

Die Web Forms-Vorlage erstellt eine Web Forms-beispielanwendung, die verwendet [Bootstrap](#bootstrap) um reaktionsschnelle Entwurfs- und Designfunktionen Funktionen zu bieten. Die folgende Abbildung zeigt die Startseite.

![Web Forms-app-Startseite](creating-web-projects-in-visual-studio/_static/image10.png)

Weitere Informationen zu Web Forms, finden Sie unter [ASP.NET Web Forms](https://asp.net/web-forms). Informationen, was die Web Forms-Vorlage für Sie übernimmt, finden Sie unter [erstellen mit Visual Studio 2013 eine einfache Web Forms-Anwendung](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC-Vorlage

ASP.NET MVC wurde entworfen, um Mustern basierende Entwicklungsverfahren wie z. B. erleichtern [testgesteuerte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), [Trennung der Belange](http://en.wikipedia.org/wiki/Separation_of_concerns), [Inversion des Steuerelements](http://en.wikipedia.org/wiki/Inversion_of_control), und [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Das Framework ermöglicht es den Geschäftslogikebene einer Webanwendung auf der Darstellungsschicht zu trennen. Indem Sie die Anwendung in Modellen (M), Ansichten (V) und Controllern (C) unterteilen, können ASP.NET MVC zum Verwalten von Komplexität in größeren Anwendungen erleichtern.

Mit ASP.NET MVC arbeiten Sie direkt mit HTML und HTTP als in Web Forms. Beispielsweise Web Forms Zustand zwischen HTTP-Anforderungen automatisch beibehalten, jedoch müssen Sie explizit in MVC code. Der Vorteil des MVC-Modells ist, dass Sie die vollständige Kontrolle über genau wie die Anwendung ausführt und das Verhalten in der webumgebung nutzen. Der Nachteil ist, dass Sie mehr Code schreiben müssen.

MVC wurde auf Erweiterbarkeit ausgelegt, werden die Power-Entwicklern die Möglichkeit, das Framework für ihre anwendungsanforderungen anzupassen. Darüber hinaus ist der Quellcode für die ASP.NET MVC unter ein OSI-Lizenz verfügbar.

Die MVC-Vorlage erstellt eine MVC 5-beispielanwendung, die verwendet [Bootstrap](#bootstrap) um reaktionsschnelle Entwurfs- und Designfunktionen Funktionen zu bieten. Die folgende Abbildung zeigt die Startseite.

![MVC-beispielanwendung](creating-web-projects-in-visual-studio/_static/image11.png)

Weitere Informationen zu MVC finden Sie unter [ASP.NET MVC](https://asp.net/mvc). Informationen dazu, wie Sie die MVC 4-Vorlage auswählen, finden Sie unter [Vorlagen für Visual Studio 2012](#vs2012) weiter unten in diesem Artikel.

<a id="webapi"></a>
### <a name="web-api-template"></a>Web-API-Vorlage

Die Web-API-Vorlage erstellt eine Beispiel-Webdiensts, der basierend auf Web-API, einschließlich der API-Hilfeseiten basierend auf MVC.

ASP.NET Web-API ist ein Framework, das erleichtert die Erstellung von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten zu erreichen. ASP.NET Web-API ist eine ideale Plattform zum Erstellen von RESTful-Dienste in .NET Framework.

Die Web-API-Vorlage erstellt eine Beispiel-Webdiensts. Die folgenden Abbildungen zeigen die Beispiel-Hilfeseiten.

![Web-API-Hilfeseite](creating-web-projects-in-visual-studio/_static/image12.png)

![Web-API-Hilfeseite für die API zum Abrufen des](creating-web-projects-in-visual-studio/_static/image13.png)

Weitere Informationen zu Web-API, finden Sie unter [ASP.NET Web-API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Vorlage der einseitigen Anwendung

Die Single-Page-Anwendung (SPA)-Vorlage erstellt eine beispielanwendung, die JavaScript, HTML 5, nutzt und [KnockoutJS](http://knockoutjs.com/) auf dem Client und ASP.NET Web-API auf dem Server.

Die Option nur zur Authentifizierung für die SPA-Vorlage ist [einzelne Benutzerkonten](#indauth).

Die folgende Abbildung zeigt den Ausgangszustand der beispielanwendung, die die SPA-Vorlage erstellt.

![Beispiel-einzelseitenanwendung](creating-web-projects-in-visual-studio/_static/image14.png)

Weitere Informationen zum Erstellen einer Anwendung mithilfe der SPA-Vorlage finden Sie unter [Web-API – externe Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md).

Weitere Informationen über den einseitigen ASP.NET-Anwendungen und zusätzliche SPA-Vorlagen, die JavaScript-Frameworks als KnockoutJS verwenden können, finden Sie unter den folgenden Ressourcen:

- [ASP.NET einzelne Seite, Anwendung](../../../single-page-application/index.md).
- [Grundlegendes zu Sicherheitsfunktionen in der SPA-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Single Page Applications: Erstellen Sie moderner und reaktionsfähiger Web-Apps mit ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook-Vorlage

Sie können Installieren einer [Visual Studio-Erweiterung, die eine Facebook-Vorlage bietet](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Diese Vorlage erstellt eine beispielanwendung, die entwickelt wurde, in der Facebook-Website ausgeführt wird. Es basiert auf ASP.NET MVC und Web-API für Echtzeit-Update-Funktionalität verwendet.

Es sind keine Authentifizierungsoptionen für die Facebook-Vorlage zur Verfügung, da Facebook-Anwendungen in die Facebook-Website ausgeführt werden und auf Facebooks-Authentifizierung basieren.

Weitere Informationen zu ASP.NET Facebook-Anwendungen, finden Sie unter [aktualisieren die Facebook-API von MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012-Vorlagen

Das Dialogfeld für Visual Studio 2013 Web die projekterstellung bietet keine Zugriff auf einige Vorlagen, die in Visual Studio 2012 verfügbar waren. Wenn Sie eine dieser Vorlagen verwenden möchten, können Sie den Knoten "Visual Studio 2012" im linken Bereich des Dialogfelds Visual Studio-Projekt klicken.

![Visual Studio 2012-Vorlagen](creating-web-projects-in-visual-studio/_static/image15.png)

Die **Visual Studio 2012** Knoten können Sie auswählen, dass die folgenden Webvorlagen, die Sie besitzen Zugriff auf in der Standardliste mit Vorlagen für Visual Studio 2013:

- ASP.NET MVC 4-Webanwendung
- ASP.NET Dynamic Data Entities-Webanwendung
- ASP.NET AJAX-Serversteuerelements
- ASP.NET AJAX-Extender-Server-Steuerelement
- ASP.NET-Serversteuerelement

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Starten Sie in den Visual Studio 2013-Web-Projektvorlagen

Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework erstellt die Lösung durch Twitter. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts an anderen Browser Fenstergrößen dynamisch anpassen können. Beispielsweise sieht in einem Browserfenster für die Breite auf der Startseite, die von der Web Forms-Vorlage erstellt die folgende Abbildung:

![Web Forms-app-Startseite](creating-web-projects-in-visual-studio/_static/image16.png)

Legen das Fenster schmaler, und verschieben Sie die Spalten mit horizontal angeordneten in vertikaler Ausrichtung an:

![Anordnung von Bootstrap vertikalen Spalte](creating-web-projects-in-visual-studio/_static/image17.png)

Das Fenster ein wenig mehr eingrenzen und horizontale Hauptmenü ändert sich, in ein Symbol, das Sie klicken können, um in einem vertikal ausgerichteten Menü zu erweitern:

![Symbol "Menü" Bootstrap "](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap vertikale Menü](creating-web-projects-in-visual-studio/_static/image19.png)

Sie können auch die Bootstrap Design-Funktion verwenden, auf einfache Weise eine Änderung in der Anwendung aussehen und Verhalten wirksam. Beispielsweise können Sie die folgenden Schritte aus, um das Design ändern tun.

1. Wechseln Sie in Ihrem Browser zu [ http://Bootswatch.com ](http://Bootswatch.com), wählen Sie ein Design aus, und klicken Sie dann auf **herunterladen**. (Dieser Download enthält *bootstrap.min.css* , wenn Sie den CSS-Code untersuchen möchten, erhalten Sie standardmäßig *Datei "Bootstrap.CSS"* anstelle der minimierte Version.)
2. Kopieren Sie den Inhalt der heruntergeladenen CSS-Datei.
3. Erstellen Sie in Visual Studio ein neues **Stylesheet** Datei mit dem Namen *Bootstrap-theme.css* in die *Content* Ordner, und fügen Sie das heruntergeladene CSS-code hinein.
4. Open *App\_Start/Bundle.config* , und ändern Sie *Datei "Bootstrap.CSS"* zu *Bootstrap-theme.css*.

Führen Sie das Projekt erneut aus, und die Anwendung hat ein neues Aussehen. Die folgende Abbildung zeigt die Auswirkungen des Amelia Designs:

![Bootstrap Amelia-Design](creating-web-projects-in-visual-studio/_static/image20.png)

Viele Bootstrap-Designs sind verfügbar, free und Premium-Versionen. Bootstrap bietet außerdem eine Vielzahl von UI-Komponenten, wie z. B. [Dropdownlisten](http://twitter.github.io/bootstrap/components.html#dropdowns), [Schaltfläche Gruppen](http://twitter.github.io/bootstrap/components.html#buttonGroups), und [Symbole](http://twitter.github.io/bootstrap/base-css.html#images). Weitere Informationen zu Bootstrap finden Sie unter [der Bootstrap-Website](http://twitter.github.io/bootstrap/).

Wenn Sie die Web Forms-Designer in Visual Studio verwenden, beachten Sie, dass der Designer nicht CSS3, nicht unterstützt, sodass sie nicht alle Effekte von Bootstrap Designs oder durch dynamische layoutänderungen genau angezeigt. Die Web Forms-Seiten werden jedoch ordnungsgemäß angezeigt, wenn Sie mit einem Browser angezeigt.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Hinzufügen von Unterstützung für zusätzliche Frameworks

Wenn Sie eine Vorlage auswählen, wird das Kontrollkästchen für die Frameworks verwendet, die von der Vorlage verwendeten automatisch ausgewählt. Angenommen, Sie wählen die **Web Forms** Vorlage die **Web Forms** Kontrollkästchen ausgewählt ist, und Sie können nicht deaktiviert werden.

![Wählen Sie eine Vorlage](creating-web-projects-in-visual-studio/_static/image21.png)

![Fügen Sie Frameworks hinzu](creating-web-projects-in-visual-studio/_static/image22.png)

Sie können das Kontrollkästchen für ein Framework auswählen, die in der Vorlage enthalten ist, um Unterstützung für das Framework hinzuzufügen, wenn das Projekt erstellt wird. Um beispielsweise die Verwendung von Web Forms ermöglichen *aspx* wählen Sie die Seiten, wenn Sie die MVC-Vorlage ausgewählt haben die **Web Forms** Kontrollkästchen. Oder um MVC aktivieren, wenn Sie die Web Forms-Vorlage verwenden, klicken Sie auf die **MVC** Kontrollkästchen. Durch die ein Framework hinzufügen, können zur Entwurfszeit als auch zur Laufzeit unterstützen. Z. B. Wenn Sie MVC-Unterstützung zu einem Web Forms-Projekt hinzufügen, werden Sie zum Erstellen des Gerüsts für Controller und Ansichten können.

Wenn Sie Kombinieren von Web Forms und MVC in einem Projekt, und aktivieren [Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Forms möglicherweise unerwartetes routing Probleme, bei mehreren mögliche Zielen hat eine URL. Die Routen, die definiert sind, werden zuerst Vorrang. Angenommen, Sie haben eine `Home` Controller und eine *Home.aspx* Seite die `http://contoso.com/home` URL wechseln zur *Home.aspx* Aufrufen der `EnableFriendlyUrls` Methode vor dem Aufruf der `MapRoute`-Methode in der *RouteConfig.cs*, oder die gleiche URL geht in der Standardansicht für Ihre `Home` Controller Aufrufen `MapRoute` vor `EnableFriendlyUrls`.

Ein Framework hinzufügen, werden keine Funktionen der Beispiel-Anwendung hinzugefügt. Z. B. Wenn Sie hinzufügen, Web Forms unterstützt, wenn Sie nicht die MVC-Vorlage ausgewählt haben *"default.aspx"* Homepage-Datei wird erstellt. Nur die Ordner, Dateien und Verweise erforderlich, um das Framework unterstützen, werden hinzugefügt. Aus diesem Grund ändert Frameworks hinzufügen Authentifizierungsoptionen, nicht die von Beispielanwendungen, die von den Vorlagen erstellten Code implementiert werden. Beispielsweise, wenn Sie die leere Vorlage auswählen und hinzufügen Web Forms oder MVC unterstützen, die **Konfigurieren der Authentifizierung** Schaltfläche wird immer noch deaktiviert.

In den folgenden Abschnitten werden kurz die Auswirkung der einzelnen Kontrollkästchen beschrieben.

### <a name="add-web-forms-support"></a>Hinzufügen von Web Forms-Unterstützung

Erstellt leere *App\_Daten* und *Modelle* Ordner und eine *"Global.asax"* Datei. Diese wurden bereits erstellt durch alle Vorlagen außer der leeren Vorlage, deshalb ist Web Forms des Kontrollkästchens keinen Unterschied bei den anderen Vorlagen.

Die Web Forms-Vorlage aktiviert Friendly URLs standardmäßig, aber wenn Sie hinzufügen, dass die Web Forms-Unterstützung mit anderen Vorlagen durch Auswählen der Web Forms-Kontrollkästchen, die, das Friendly URLs nicht automatisch aktiviert werden.

### <a name="add-mvc-support"></a>Hinzufügen von MVC-Unterstützung

MVC und Razor WebPages NuGet-Pakete werden installiert, erstellt die leere *App\_Daten*, *Controller*, *Modelle*, und *Ansichten*Ordnern erstellt *App\_starten* Ordner mit *RouteConfig.cs* Datei und erstellt *"Global.asax"* Datei.

### <a name="add-web-api-support"></a>Hinzufügen von Web-API-Unterstützung

Web-API "newtonsoft.JSON" NuGet-Pakete und installiert wird, erstellt leere *App\_Daten*, *Controller*, und *Modelle* Ordnern erstellt  *App\_starten* Ordner mit *WebApiConfig.cs* Datei und erstellt *"Global.asax"* Datei.

<a id="auth"></a>
## <a name="authentication-methods"></a>Authentifizierungsmethoden

Visual Studio 2013 bietet mehrere Authentifizierungsoptionen für die Web Forms, MVC und Web-API-Vorlagen:

- [Keine Authentifizierung](#noauth)
- [Einzelne Benutzerkonten](#indauth) (ASP.NET Identity, ehemals ASP.NET-Mitgliedschaft)
- [Organisationskonten](#orgauth) (Windows Server Active Directory oder Azure Active Directory)
- [Windows-Authentifizierung](#winauth) (Intranet)

![Dialogfeld "Authentifizierung" Konfigurieren](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Keine Authentifizierung

Bei Auswahl von **keine Authentifizierung**, die beispielanwendung enthält keine Webseiten für die Anmeldung, nicht-Benutzeroberfläche, der angibt, wer angemeldet ist keine Entität für eine Mitgliedschaftsdatenbank und keine Verbindungszeichenfolge für eine Mitgliedschaftsdatenbank Klassen.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Einzelne Benutzerkonten

Bei Auswahl von **einzelne Benutzerkonten**, wird die beispielanwendung zur Verwendung von ASP.NET Identity (ehemals ASP.NET-Mitgliedschaft) konfiguriert werden, für die Benutzerauthentifizierung. ASP.NET Identity kann einen Benutzer ein Konto registrieren, durch das Erstellen eines Benutzernamens und Kennworts auf der Website oder durch Anmelden mit Anbietern von sozialen Netzwerken wie Facebook, Google, Microsoft Account oder Twitter. Der Standardspeicher für die Daten für Benutzerprofile in ASP.NET Identity ist eine SQL Server LocalDB-Datenbank, die Sie in SQL Server oder Azure SQL-Datenbank für den Produktionsstandort bereitstellen können.

Diese Features in Visual Studio 2013 sind Visual Studio 2012 identisch, aber der zugrunde liegenden Code für das Mitgliedschaftssystem von ASP.NET wurde neu erstellt. Die folgenden: Vorteile der neuen Codebasis

- Das neue Mitgliedschaftssystem basiert auf [OWIN](http://owin.org/) anstatt das Modul für die ASP.NET-Formularauthentifizierung. Dies bedeutet, dass Sie den gleichen Authentifizierungsmechanismus verwenden können, ob Sie Web Forms oder MVC in IIS verwenden, oder Sie, Web-API oder SignalR Selbsthosting sind.
- Die neue Mitgliedschaftsdatenbank von Entity Framework Code First verwaltet wird, und alle Tabellen werden dargestellt, von Entitätsklassen, die Sie ändern können. Dies bedeutet, dass Sie ganz einfach anpassen können, das Datenbankschema und die Profilbezogene Web-UI Ihren eigenen Anforderungen anpassen, und Sie können Ihre Updates mit Code First-Migrationen ganz einfach bereitstellen.

Das neue Mitgliedschaftssystem wird automatisch in die neuen Vorlagen implementiert, und sie können manuell in jedes Projekt, das auf .NET 4.5 abzielt implementiert oder höher werden.

ASP.NET Identity ist eine gute Wahl, wenn Sie eine Internet-Website handelt es sich hauptsächlich für externe Kunden erstellen. Wenn Ihre Organisation Active Directory verwendet oder Office 365 und Sie möchten, ein Projekt zu erstellen, die es ermöglicht Single-Sign-on für Mitarbeitern und Geschäftspartnern, die **Organisationskonten** Option ist möglicherweise eine bessere Wahl.

Weitere Informationen zur Option "einzelne Benutzerkonten" finden Sie unter den folgenden Ressourcen:

- [www.ASP.NET/Identity](../../../identity/index.md). Dokumentation zu ASP.NET Identity auf der ASP.NET-Website.
- [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Außerdem gezeigt, wie Benutzerprofildaten anpassen.
- [Web-API – externe Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md)
- [Hinzufügen externer Anmeldungen zu Ihrer ASP.NET-Anwendung in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Organisations-Konten

Bei Auswahl von **Organisationskonten**, die beispielanwendung wird so konfiguriert werden, dass Sie Windows Identity Foundation (WIF) verwenden, für die Authentifizierung basierend auf Benutzerkonten in Azure Active Directory (Azure AD, einschließlich Office 365) oder Windows Server Active Directory. Weitere Informationen finden Sie unter [Organisations-Konto-Authentifizierungsoptionen](#orgauthoptions) weiter unten in diesem Thema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows-Authentifizierung

Bei Auswahl von **Windows-Authentifizierung**, die beispielanwendung wird so konfiguriert werden, dass um das Windows-Authentifizierung-IIS-Modul für die Authentifizierung zu verwenden. Die Domäne und Benutzer-ID des Active Directory oder dem lokalen Computer-Konto, das Windows angemeldet ist, aber nicht enthalten benutzerregistrierung oder Benutzeroberfläche für Anmeldung wird von der Anwendung angezeigt werden. Diese Option ist für Intranet-Websites vorgesehen.

Alternativ können Sie erstellen eine Intranetwebsite, die AD-Authentifizierung wird, durch Auswählen verwendet der [On-Premises-Option "unter" Organisationskonten](#orgauthonprem). Die On-Premises-Option verwendet die Windows Identity Foundation (WIF) anstelle der Windows-Authentifizierungsmodul. Einige zusätzliche Schritte sind erforderlich, damit Sie die Option On-Premises einrichten, aber WIF Funktionen aktiviert, die nicht mit dem Modul für Windows-Authentifizierung zur Verfügung stehen. Mit WIF können Sie z. B. Anwendungszugriff in Active Directory und Abfragen von Verzeichnisdaten konfigurieren.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Organisations-Konto-Authentifizierungsoptionen

Die **Konfigurieren der Authentifizierung** Dialogfeld bietet Ihnen mehrere Optionen für Azure Active Directory (Azure AD, einschließlich Office 365) oder Windows Server Active Directory (AD)-Kontoauthentifizierung:

- [Cloud - einzelne Organisation](#orgauthsingle) (Azure AD oder AD mithilfe der Directory-Integration mit Azure AD)
- [Cloud – mit mehreren Unternehmen](#orgauthmulti) (Azure AD oder AD mithilfe der Directory-Integration mit Azure AD)
- [Lokale](#orgauthonprem) (AD)

Wenn Sie eine der Azure AD Optionen ausprobieren möchten, jedoch kein Konto verfügen, [klicken Sie hier, um sich für ein Azure AD-Konto anzumelden](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Wenn Sie eine der Azure AD-Optionen auswählen, Ihr Projekt erfordert eine Datenbank und Konto eines globalen Administrators für Ihren Azure AD-Mandanten anmelden. Geben Sie den Namen und das Kennwort für ein organisationskonto (z. B. admin@contoso.onmicrosoft.com), die Administratorberechtigungen für Azure AD-Mandanten verfügt.
> 
> **Geben Sie Anmeldeinformationen nicht für ein Microsoft-Konto (z. B. contoso@hotmail.com) in das Dialogfeld-Anmeldung.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - einzelne Organisationsauthentifizierung

![Authentifizierung für einzelne Organisation](creating-web-projects-in-visual-studio/_static/image24.png)

Wählen Sie diese Option aus, wenn Sie möchten zum Aktivieren der Authentifizierung für Benutzerkonten, die in einem Azure AD definiert sind [Mandanten](https://technet.microsoft.com/library/jj573650.aspx). Beispielsweise die Website ist "contoso.com", und es wird zur Verfügung gestellt, die Mitarbeiter des Unternehmens Contoso, die im Mandanten "contoso.onmicrosoft.com" sind. Sie wird nicht zum Konfigurieren von Azure AD, damit Benutzer von anderen Mandanten auf die Anwendung zugreifen können.

#### <a name="domain"></a>Domäne

Geben Sie die Azure AD-Domäne, die Sie einrichten die Anwendung, z. B. möchten: `contoso.onmicrosoft.com`. Wenn Sie haben eine [benutzerdefinierte Domäne](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), z. B. `contoso.com` anstelle von `contoso.onmicrosoft.com`, können Sie, die hier eingeben.

#### <a name="access-level"></a>Zugriffsebene

Wenn die Anwendung muss zum Abfragen oder Aktualisieren von Verzeichnisinformationen mit der Graph-API wählen **einmaliges Anmelden, Verzeichnisdaten lesen** oder **einmaliges Anmelden, lesen und Schreiben von Verzeichnisdaten**. Wählen Sie andernfalls **Single Sign-On**. Weitere Informationen finden Sie unter [Anwendung Zugriffsebenen](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) und [mithilfe der Graph-API zum Abfragen von Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Standardmäßig erstellt die Vorlage Anwendungs-ID-URI für Sie durch den Namen des Projekts mit der Azure AD-Domäne anfügen. Wenn der Projektname ist z. B. `Example` und die Domäne `contoso.onmicrosoft.com`, die Anwendung, die ID-URI wird `https://contoso.onmicrosoft.com/Example`. Wenn Sie den Anwendungs-ID-URI manuell angeben möchten, erweitern Sie die **Weitere Optionen** aus, und geben Sie den Anwendungs-ID-URI in das Textfeld ein. Die Anwendung, die ID-URI muss mit beginnen `https://`.

Standardmäßig verfügt eine Anwendung, die bereits in Azure AD bereitgestellt wird dieselbe Anwendung-ID-URI der, die Verwendung von Visual Studio für das Projekt wird das Projekt sein der vorhandenen Anwendung anstatt eine neue Bereitstellung verbunden. Wenn eine neue Anwendung in diesem Fall bereitgestellt werden soll, deaktivieren Sie die **Eintrag zu überschreiben, wenn eine mit der gleichen ID bereits vorhanden ist** Kontrollkästchen.

Wenn die **überschreiben** ist das Kontrollkästchen deaktiviert, und Visual Studio sucht nach einer vorhandenen Anwendung mit der gleichen Anwendung-ID-URI, durch Anfügen einer Nummer an den URI an, es war im Begriff, verwenden Sie, einen neuen URI erstellt. Nehmen wir beispielsweise an, die der Namen des Projekts ist `Example`, Sie das Textfeld leer lassen, deaktivieren Sie die **überschreiben** das Kontrollkästchen, und Azure AD-Mandanten hat bereits eine Anwendung mit dem URI `https://contoso.onmicrosoft.com/Example`. In diesem Fall wird eine neue Anwendung bereitgestellt werden, mit einer Anwendung, die ID-URI wie `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Die Anwendung in Azure AD-Bereitstellung

Zum Bereitstellen der Anwendung in Azure AD, oder verbinden das Projekt zu einer vorhandenen Anwendung, benötigt Visual Studio die Anmeldeinformationen eines globalen Administrators für die Domäne an. Beim Klicken auf **OK** in die **Konfigurieren der Authentifizierung** im Dialogfeld werden Sie aufgefordert, für den Benutzernamen und das Kennwort eines globalen Administrators für die Domäne, die Sie angegeben haben. Später, wenn Sie auf **Erstellen eines Projekts** in die **neues ASP.NET-Projekt** Dialogfeld Visual Studio stellt die Anwendung in Azure AD. Beachten Sie, dass Visual Studio bettet den geheimen Clientschlüssel als Teil dieses Prozesses in der Datei "Web.config", das ein Jahr nach der Erstellung abläuft.

Informationen zum Erstellen von Anwendungen, mit denen **Cloud - einzelne Organisation** -Authentifizierung finden Sie unter den folgenden Ressourcen:

- [Azure-Authentifizierung](../2012/windows-azure-authentication.md)
- [Hinzufügen von Anmeldung bei Ihrer Webanwendung mithilfe von Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Entwickeln von ASP.NET-Apps mit Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Sichern von ASP.NET Web-API mit Azure AD und Microsoft-OWIN-Komponenten](https://msdn.microsoft.com/magazine/dn463788.aspx)

In den Tutorials wurde noch nicht für Visual Studio 2013 aktualisiert. Einige der welche in den Tutorials, die Sie manuell weiterleiten erfolgt automatisch in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud – mit mehreren Unternehmen-Authentifizierung

![Mehrere Unternehmen-Authentifizierung](creating-web-projects-in-visual-studio/_static/image25.png)

Wählen Sie diese Option aus, wenn Sie möchten zum Aktivieren der Authentifizierung für Benutzerkonten, die in mehreren Azure AD definiert sind [Mandanten](https://technet.microsoft.com/library/jj573650.aspx). Beispielsweise die Website ist "contoso.com", und es wird zur Verfügung gestellt, die Mitarbeiter des Unternehmens Contoso, die im Mandanten "contoso.onmicrosoft.com" sind, und Mitarbeiter des Unternehmens Fabrikam, die im Mandanten "Fabrikam.onmicrosoft.com" sind.

Die Einstellungen, die Sie eingeben und die Anwendung, die Bereitstellung von Schritt ähneln [einzelne organisationsauthentifizierung](#orgauthsingle).

Informationen zum Erstellen von Anwendungen, mit denen **Cloud – mit mehreren Unternehmen** -Authentifizierung finden Sie unter den folgenden Ressourcen:

- [Einfache Web-App-Integration mit Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) im Active Directory-Team-Blog.
- [Entwickeln von mehrinstanzenfähigen Webanwendungen mit Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) Tutorial. Das Tutorial noch nicht für Visual Studio 2013 noch aktualisiert wurde; Einige der wie im Tutorial leitet Sie manuell erfolgt automatisch in Visual Studio 2013.
- [Sie müssen mit Ihren eigenen mehrere Organisationen ASP.NET App anmelden, bevor Sie sich anmelden können](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog von Vittorio Bertocci, die erklärt, wie Sie eine allgemeine Problem, die Benutzer zu lösen auftreten, wenn es sich bei Erstellen eines Projekts, multiorganisationsanwendung-Authentifizierung verwendet.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Lokale Organisationsauthentifizierung

![Lokale organisationsauthentifizierung](creating-web-projects-in-visual-studio/_static/image26.png)

Wählen Sie diese Option aus, wenn Sie zum Aktivieren der Authentifizierung für Benutzerkonten, die in die Windows Server Active Directory (AD) definiert sind, und nicht Azure AD verwenden möchten. Sie können diese Option verwenden, um eine Intranetwebsite oder eine Internetwebsite zu erstellen. Verwenden Sie für eine IIS-Website Active Directory Federation Services (ADFS), um Zugriff auf AD zu ermöglichen. Weitere Informationen finden Sie unter [verwenden Sie die On-Premises Organisation Authentication Option (ADFS) mit ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Für eine Intranetwebsite als Alternative können Sie [Windows-Authentifizierung](#winauth) statt dieser Option. Für die Option Windows-Authentifizierung müssen Sie keine Metadatendokument-URL bereitstellen. Allerdings wird Windows-Authentifizierung die Möglichkeit zum Steuern des Zugriffs in Active Directory-Anwendung oder zum Abfragen von Verzeichnisdaten Ihnen nicht.

#### <a name="on-premises-authority"></a>Lokale Autorisierungsstelle

Geben Sie eine URL, die auf das Dokument zeigt. Das Metadatendokument enthält die Koordinaten der Autorisierungsstelle. Die Anwendung wird diese Koordinaten verwenden, um das Web SSO-Ablauf zu steuern.

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Geben Sie einen eindeutigen URI, die Active Directory verwenden können, diese Anwendung zu identifizieren oder leer lassen, um Visual Studio erstellen können.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

Dieses Dokument hat einige grundlegende Hilfe für das Erstellen eines neuen Webprojekts von ASP.NET in Visual Studio 2013 bereitgestellt. Weitere Informationen zur Verwendung von für die Webentwicklung für Visual Studio finden Sie unter [ https://www.asp.net/visual-studio/ ](../../index.md).
