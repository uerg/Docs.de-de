---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Erstellen Sie das Projekt | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 754f085e3e43f7efa155f410d02a0d29d3349612
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912292"
---
<a name="create-the-project"></a>Erstellen des Projekts
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial werden Sie zu erstellen, überprüfen Sie, und führen Sie das Standardprojekt in Visual Studio, die Sie mit Funktionen von ASP.NET vertraut machen können. Darüber hinaus überprüfen Sie die Visual Studio-Umgebung.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Vorgehensweise: erstellen ein neues Web Forms-Projekt.
- Die Dateistruktur des Web Forms-Projekts.
- So führen Sie das Projekt in Visual Studio.
- Die verschiedenen Features von der Standard-Web Forms-Anwendung.
- Einige Grundlagen zur Verwendung von Visual Studio-Umgebung.

## <a name="creating-the-project"></a>Erstellen des Projekts

1. Öffnen Sie Visual Studio.
2. Wählen Sie **neues Projekt** aus der **Datei** Menü in Visual Studio. 

    ![Erstellen Sie das Projekt - neues Projekt im Menü-Element](create-the-project/_static/image1.png)
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.  
 Dieser tutorialreihe verwendet .NET Framework 4.5.2.
5. Benennen Sie Ihr Projekt *WingtipToys* , und wählen Sie die **OK** Schaltfläche. 

    ![Erstellen Sie das Projekt - Dialogfeld "Neues Projekt"](create-the-project/_static/image2.png)

    > [!NOTE]
    > Der Name des Projekts in dieser tutorialreihe ist **WingtipToys**. Es wird empfohlen, dass Sie diese Option verwenden *genaue* Projektname, damit der Code in der tutorialreihe Funktionen wie erwartet bereitgestellt.

6. Klicken Sie auf die **Authentifizierung ändern** Schaltfläche. Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf die **OK** Schaltfläche.

7. Wählen Sie die **Web Forms** Vorlage, und klicken Sie auf die **OK** Schaltfläche.

    ![Erstellen Sie das Projekt - Vorlage für neue Projekte](create-the-project/_static/image3.png)

Das Projekt dauert ein wenig Zeit, zu erstellen. Wenn er bereit ist, öffnen Sie die **"default.aspx"** Seite.

![Erstellen Sie das Projekt - Vorlage für neue Projekte](create-the-project/_static/image4.png)

Sie können zwischen wechseln **Entwurf** anzeigen und **Quelle** Ansicht eine Option am unteren Rand des Fensters Center. **Entwurf** zeigt ASP.NET Web Pages, Masterseiten, Inhaltsseiten, HTML-Seiten und Benutzersteuerelemente eine fast-WYSIWYG-Ansicht verwenden. **Quelle** zeigt das HTML-Markup für Ihre Webseite, die Sie bearbeiten können.

> [!TIP] 
> 
> **Grundlegendes zu den ASP.NET Frameworks**
> 
> ASP.NET Web Forms können Sie erstellen dynamischer Websites mit einem vertrauten Drag & Drop, ereignisgesteuertes Modell. Eine Entwurfsoberfläche und Hunderte von Steuerelementen und Komponenten können Sie schnell komplexe und umfangreiche GUI-gesteuerte Websites mit Datenzugriff erstellen. Die Wingtip Toys-Store basiert auf ASP.NET Web Forms, aber viele der Konzepte, die Sie in diesem Tutorial erfahren Sie, gelten für alle von ASP.NET.
> 
> ASP.NET bietet vier primäre Webentwicklungs-Frameworks:
> 
> - [ASP.NET-Web Forms](../../../index.md)  
>  Die Web Forms-Framework richtet sich an Entwickler, die deklarative und Steuerelementen basierenden Programmierung, z. B. Microsoft Windows Forms (WinForms) "und" WPF/XAML/Silverlight bevorzugen. Es bietet ein Modell WYSIWYG-Designer-driven Development, daher ist die beliebt sind, mit der Entwickler, die für eine Umgebung rapid Application Development (RAD), für die Webentwicklung. Wenn Sie noch keine Erfahrungen Webprogrammierung und mit der herkömmlichen Microsoft RAD-Entwicklung Clienttools (z. B. für Visual Basic und Visual c#) vertraut sind, können Sie schnell eine Webanwendung erstellen, ohne Benutzeroberfläche in HTML und JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC richtet sich an Entwickler, die Muster und Prinzipien wie die testgesteuerte Entwicklung, Trennung von Belangen, IoC Inversion of Control (IoC) und Abhängigkeitsinjektion (Dependency Injection) interessiert sind. Dieses Framework fördert die Trennung des Geschäftslogikebene einer Webanwendung auf der Darstellungsschicht.
> - [ASP.NET-Webseiten 2](../../../../web-pages/index.md)  
>  ASP.NET Web Pages richtet sich an Entwickler, die eine einfache Web-Entwicklung Story, wie PHP wünschen. Im Webseiten-Modell HTML-Seiten erstellen und die Seite anschließend serverbasierten Code hinzugefügt werden, um dynamisch zu steuern, wie dieses Markup gerendert wird. Webseiten wurde speziell für ein schlankes Framework werden, und es ist der einfachste Einstiegspunkt in ASP.NET für Personen, die wissen, HTML, aber möglicherweise keine umfassende Programmierfunktionen – z. B., Schüler/Studenten oder Hobbyprogrammierer. Es ist auch eine gute Möglichkeit für Webentwickler, die wissen, PHP oder ähnliche Frameworks, um mithilfe von ASP.NET zu starten.
> - [ASP.NET-Single-Page-Anwendung](../../../../single-page-application/index.md)  
>  ASP.NET Single Page Application (SPA) können Sie die Anwendungen zu erstellen, die signifikante clientseitigen Interaktionen mit HTML 5, CSS 3 und JavaScript enthalten. Die ASP.NET und Web Tools 2012.2-Update umfasst eine neue Vorlage zum Erstellen von einseitigen Anwendungen mithilfe von knockout.js und ASP.NET Web-API. Zusätzlich zu der neuen SPA-Vorlage werden neue Community erstellte SPA-Vorlagen auch zum Download zur Verfügung.
> 
> Zusätzlich zu den vier wichtigsten Entwicklungsframeworks bietet ASP.NET auch zusätzliche Technologien, die wichtig zu beachten und mit vertraut sein, aber nicht in diesem Lernprogramm behandelt:
> 
> - [ASP.NET Web-API](../../../../web-api/index.md) -ein Framework zum Erstellen von HTTP-Dienste, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten zu erreichen.
> - [ASP.NET SignalR](../../../../signalr/index.md) – eine Bibliothek, die Entwicklung von Echtzeit-Webfunktionen erleichtert.


### <a name="reviewing-the-project"></a>Überprüfen das Projekt

In Visual Studio die **Projektmappen-Explorer** Fenster können Sie die Dateien für das Projekt zu verwalten. Werfen wir einen Blick auf die Ordner, die zur Anwendung hinzugefügt wurden **Projektmappen-Explorer**. Die Web Application-Vorlage fügt eine grundlegende Ordnerstruktur:

![Erstellen Sie das Projekt - Projektmappen-Explorer](create-the-project/_static/image5.png)

Visual Studio erstellt einige anfängliche Ordner und Dateien für Ihr Projekt. Die erste Dateien, die Sie später in diesem Tutorial mit arbeiten, sind die folgenden:

| **Datei** | **Zweck** |
| --- | --- |
| *"Default.aspx"* | In der Regel die erste Seite angezeigt, wenn die Anwendung in einem Browser ausgeführt wird. |
| *Site.Master* | Eine Seite, die Ihnen ermöglicht, ein konsistentes Layout und die Verwendung-Standardverhalten für Seiten in Ihrer Anwendung zu erstellen. |
| *"Global.asax"* | Eine optionale Datei, die Code für die Reaktion auf Ereignisse, die sich auf Anwendungsebene und auf Sitzungsebene ausgelöst von ASP.NET oder HTTP-Modulen enthält. |
| *Web.config* | Die Konfigurationsdaten für eine Anwendung. |

### <a name="running-the-default-web-application"></a>Ausführen der Standardwebanwendung

Die standardwebanwendung bietet eine komfortable basierend auf integrierte Funktionen und unterstützt. Die Anwendung kann ohne Änderungen an das Standard-Web Forms-Projekt auf Ihren lokalen Webbrowser ausgeführt werden.

1. Drücken Sie die ***F5*** gedrückt, während in Visual Studio.   
 Die Anwendung wird erstellt und in Ihrem Webbrowser angezeigt.  

    ![Erstellen des Projekts - Standardseite](create-the-project/_static/image6.png)
2. Wenn Sie vollständige Überprüfung die ausgeführte Anwendung haben, schließen Sie das Browserfenster.

Es gibt drei Hauptseiten in dieser Standard-Webanwendung: *"default.aspx"* (Stamm), *About.aspx*, und *Contact.aspx*. Jede dieser Seiten aus der oberen Navigationsleiste erreichbar ist. Es gibt auch zwei zusätzlichen Seiten, die in den Konto-Ordner enthalten, der Register.aspx-Seite und die Seite "Login.aspx". Diese beiden Seiten können Sie die Mitgliedschaftsfunktionen von ASP.NET zum Erstellen, speichern und überprüfen die Anmeldeinformationen des Benutzers verwenden.

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Forms-Hintergrund

ASP.NET Web Forms sind Seiten, die auf Microsoft ASP.NET-Technologie basieren, Code, auf dem Server dynamisch ausgeführt wird, in denen Ausgabe einer Webseite an den Browser oder Client-Gerät erstellt wird. Eine ASP.NET Web Forms-Seite rendert automatisch die richtige Browser kompatibel HTML für Features wie z. B. Stile, Layout und So weiter. Web Forms sind kompatibel mit jeder von der .NET common Language Runtime, z. B. Microsoft Visual Basic und Microsoft Visual c# unterstützt. Darüber hinaus Web Forms auf Grundlage der [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), die bietet Vorteile wie z. B. einer verwalteten Umgebung, typsicherheit und Vererbung.

Wenn eine ASP.NET Web Forms-Seite ausgeführt wird, durchläuft die Seite einen Lebenszyklus, in dem sie eine Reihe von Verarbeitungsschritten ausführt. Diese Schritte beinhalten Initialisierung Instanziieren von Steuerelementen, wiederherstellen und aufrechterhalten des Zustands, Ausführen von Code im Ereignishandler und das Rendern. Wie Sie mit der Leistungsfähigkeit der ASP.NET Web Forms machen vertraut, es ist wichtig, dass Sie verstehen die [Lebenszyklus von ASP.NET-Seiten](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) , damit Sie Code in der entsprechenden Lebenszyklus-Phase für den Effekt schreiben können, Sie möchten.

Wenn ein Webserver eine Anforderung für eine Seite erhält, sucht nach der Seite, verarbeitet, sendet es an den Browser und klicken Sie dann verwirft alle Informationen zur Seite. Wenn der Benutzer erneut die gleiche Seite anfordert, wird der Server die gesamte Sequenz, die eine erneute Verarbeitung der Seite von Grund auf neu wiederholt. Anders ausgedrückt, ein Server keinen Speicher von Seiten, dass sie verarbeitet Seiten hat hat sind zustandslos. Das ASP.NET-Seitenframework behandelt automatisch die Aufgabe der Verwaltung des Ihrer Seite und ihre Steuerelemente, und bietet Ihnen eine explizite Möglichkeiten, den Status einer anwendungsspezifischen Informationen beibehalten.

> [!TIP] 
> 
> **Web Application-Funktionen in der Vorlage die Web Forms-Anwendung**
> 
> Die Vorlage für ASP.NET Web Forms-Anwendung bietet einen umfangreichen Satz von integrierten Funktionen. Es stellt nicht nur mit einem *Home.aspx* Seite eine *About.aspx* Seite eine *Contact.aspx* Seite, enthält jedoch auch Mitgliedschaftsfunktion, die Benutzer registriert, und speichert die Anmeldeinformationen, damit sie zu Ihrer Website anmelden können. Diese Übersicht bietet weitere Informationen zu einigen der Features in der Vorlage für ASP.NET Web Forms-Anwendung und deren Verwendung in der Anwendung Wingtip Toys enthalten sind.
> 
> **Mitgliedschaft**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity speichert die Benutzeranmeldeinformationen in einer Datenbank, die von der Anwendung erstellt. Wenn Ihre Benutzer anmelden, überprüft die Anwendung ihrer Anmeldeinformationen durch Lesen aus der Datenbank an. Ihres Projekts *Konto* Ordner enthält die Dateien, die die verschiedenen Teile der Mitgliedschaft zu implementieren: Registrierung, Anmeldung, das Ändern des Kennworts und Autorisieren des Zugriffs. Darüber hinaus unterstützt ASP.NET Web Forms, OAuth und OpenID. Diese Verbesserungen der Authentifizierung können Benutzer bei einer Website mithilfe von vorhandenen Anmeldeinformationen aus solchen Facebook, Twitter, Windows Live und Google-Konten anmelden.
> 
> ![Erstellen Sie das Projekt - Projektmappen-Explorer (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Standardmäßig erstellt die Vorlage eine Mitgliedschaftsdatenbank mithilfe eines standarddatenbanknamens auf einer Instanz von SQL Server Express LocalDB, den Datenbankserver für Entwicklung, die in Visual Studio Express 2013 für Web enthalten ist.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) ist eine Basisversion von SQL Server, die über viele Programmierbarkeitsfunktionen der SQL Server-Datenbank verfügt. SQL Server Express LocalDB im Benutzermodus ausgeführt und verfügt über eine schnelle, Konfigurationsfreie Installation, die eine kurze Liste an Voraussetzungen. In Microsoft SQL Server, eine beliebige Datenbank oder Transact-SQL-Code kann aus SQL Server Express LocalDB zu SQL Server und SQL Azure ohne Upgradeschritte verschoben werden. Daher kann SQL Server Express LocalDB als entwicklerumgebung für Anwendungen für alle Editionen von SQL Server verwendet werden. SQL Server Express LocalDB ermöglicht Funktionen wie z. B. gespeicherte Prozeduren, benutzerdefinierte Funktionen und Aggregate, .NET Framework-Integration, räumliche Typen und anderen Benutzern, die in SQL Server Compact nicht verfügbar sind.
> 
> **Masterseiten**
> 
> Ein [ASP.NET-Masterseite](https://msdn.microsoft.com/library/wtxbf3hh.aspx) ein einheitliches Erscheinungsbild und Verhalten für alle Seiten in Ihrer Anwendung definiert. Mit dem Inhalt einer einzelnen Inhaltsseite auf die letzte Seite zu erzeugen, die dem Benutzer angezeigt wird, werden das Layout der Masterseite zusammengeführt. In der Anwendung Wingtip Toys, die Sie ändern die *Site.master* Masterseite, sodass alle Seiten in der Wingtip Toys-Website dieselbe unterschiedliche Logo und Navigation Leiste freigeben.
> 
> **HTML5**
> 
> Die Vorlage für ASP.NET Web Forms-Anwendung unterstützt [HTML5](http://www.w3schools.com/html/html5_intro.asp), d.h., dass die neueste Version von die HTML-Markup Language. HTML5 unterstützt neue Elemente und Funktionen, die zum Erstellen von Websites zu vereinfachen.
> 
> **Modernizr**
> 
> Für Browser, die kein HTML5 unterstützen, können Sie [Modernizr](http://www.modernizr.com/). Modernizr ist eine Open Source-JavaScript-Bibliothek, die erkennt, ob ein Browser HTML5-Features unterstützt, und aktivieren, wenn dies nicht der Fall. In der Vorlage ASP.NET Web Forms-Anwendung ist Modernizr als NuGet-Paket installiert.
> 
> **Bootstrap**
> 
> Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework erstellt die Lösung durch Twitter. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts an anderen Browser Fenstergrößen dynamisch anpassen können. Sie können auch die Bootstrap Design-Funktion verwenden, auf einfache Weise eine Änderung in der Anwendung aussehen und Verhalten wirksam. Standardmäßig enthält die ASP.NET Web Application-Vorlage in Visual Studio 2013 Bootstrap als NuGet-Paket.
> 
> **NuGet-Pakete**
> 
> Die Vorlage für ASP.NET Web Forms-Anwendung umfasst eine Reihe von [NuGet](http://www.nuget.org/) Pakete. Diese Pakete stellen in Komponenten gegliederten Funktionen in Form von open Source-Bibliotheken und Tools bereit. Es gibt eine Vielzahl von Paketen zu erstellen und Testen Ihrer Anwendungen. Visual Studio erleichtert das Hinzufügen, entfernen und Aktualisieren von NuGet-Pakete. Entwickler können erstellen und Pakete auch in NuGet hinzufügen.
> 
> ![Erstellen Sie das Projekt - NuGet-Dialogfeld](create-the-project/_static/image8.png)
> 
> Bei der Installation eines Pakets, NuGet kopiert Dateien in Ihrer Lösung und wird automatisch auf alle Änderungen, die erforderlich sind, z. B. Hinzufügen von verweisen, und Ändern der Konfigurations Ihrer Web-Anwendung zugeordnet. Wenn Sie die Bibliothek entfernen möchten, wird NuGet entfernt Dateien und kehrt alle Änderungen, die sie in Ihrem Projekt vorgenommen, sodass keine Überfrachtung gelassen wird. NuGet steht über den **Tools** Menü in Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) ist eine schnelle und präzise JavaScript-Bibliothek, die HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Aktivitäten für eine schnelle Webentwicklung vereinfacht. JavaScript-Bibliothek jQuery ist als NuGet-Paket in der Vorlage für ASP.NET Web Forms-Anwendung enthalten.
> 
> **Unaufdringliche Validierung**
> 
> Integrierten Validator-Steuerelemente wurden konfiguriert, um für die clientseitige Validierungslogik unaufdringliches JavaScript zu verwenden. Dies erheblich reduziert die Menge an JavaScript Inline im Markup Seite gerendert, und verringert die Gesamtgröße der Seite. Unaufdringliche Validierung ist global hinzugefügt, der ASP.NET Web Forms-Anwendung-Vorlage basierend auf der Einstellung in der &lt;"appSettings"&gt; Element der *"Web.config"* Datei im Stammverzeichnis der Anwendung.
> 
> **Entitätsframework Code First**
> 
> Neben den Features in der Vorlage für ASP.NET Web Forms-Anwendung, die Wingtip Toys-Anwendung verwendet [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), dies ist eine NuGet-Bibliothek, die Code-orientierte Entwicklung ermöglicht, bei der Arbeit mit Daten. Kurz gesagt, wird die Datenbank betreffende Teil Ihrer Anwendung basierend auf den Code, den Sie schreiben. Mithilfe von Entity Framework, abrufen und Bearbeiten von Daten als stark typisierte Objekte. Mit diesem können Sie auf die Geschäftslogik konzentrieren, in der Anwendung statt auf die Details wie die Daten zugegriffen wird.
> 
> Weitere Informationen zu den installierten Bibliotheken und Pakete, die in der ASP.NET Web Forms-Vorlage enthalten finden Sie in der Liste der installierten NuGet-Pakete. Dazu erstellen Sie In Visual Studio ein neues Web Forms-Projekt auswählen **Tools** > **NuGet Paket-Manager** > **NuGet-Pakete verwaltenLösung**, und wählen Sie **installierte Pakete** in der **NuGet-Pakete verwalten** Dialogfeld.

### <a name="touring-visual-studio"></a>Visual Studio Touring

Die wichtigsten Fenster in Visual Studio enthalten die **Projektmappen-Explorer**, **Server-Explorer** (**Datenbank-Explorer** in Express), wird die **Eigenschaften Fenster**, **Toolbox**, **Symbolleiste**, und die **Dokumentfenster**.

![Erstellen Sie das Projekt - NuGet-Dialogfeld](create-the-project/_static/image9.png)

Weitere Informationen zu Visual Studio, finden Sie unter [visuelle Anleitung zu Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erstellt haben, führen Sie die Standard-Web Forms-Anwendung und überprüft. Sie haben die anderen Funktionen von der Standard-Web Forms-Anwendung überprüft und haben einige Grundlagen zur Verwendung von Visual Studio-Umgebung. In den folgenden Tutorials erstellen Sie die Datenzugriffsebene.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Auswählen des richtigen Programmiermodells](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Von Webanwendungsprojekten und Websiteprojekten](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web Forms-Seiten-Übersicht](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Zurück](introduction-and-overview.md)
> [Weiter](create_the_data_access_layer.md)
