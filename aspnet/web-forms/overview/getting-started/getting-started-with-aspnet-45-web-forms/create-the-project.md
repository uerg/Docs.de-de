---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Erstellen des Projekts | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 094733dcbe31486385dda2f8b44ba77a17486c82
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="create-the-project"></a>Erstellen des Projekts
====================
by [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm erstellen, überprüfen und führen Sie das Standardprojekt in Visual Studio, die Sie mit Funktionen von ASP.NET vertraut machen können. Darüber hinaus überprüfen Sie die Visual Studio-Umgebung.

## <a name="what-youll-learn"></a>Lernen Sie:

- Vorgehensweise: Erstellen Sie ein neues Web Forms-Projekt.
- Der Dateistruktur des Web Forms-Projekts.
- Wie Sie das Projekt in Visual Studio ausführen.
- Die verschiedenen Funktionen von der Standardeinstellung Web Forms-Anwendung.
- Einige Grundlagen zur Verwendung von Visual Studio-Umgebung.

## <a name="creating-the-project"></a>Erstellen des Projekts

1. Öffnen Sie Visual Studio.
2. Wählen Sie **neues Projekt** aus der **Datei** Menü in Visual Studio. 

    ![Erstellen des Projekts – neues Projekt im Menü-Element](create-the-project/_static/image1.png)
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.  
 Diese Reihe von Lernprogrammen verwendeten .NET Framework 4.5.2.
5. Namen für das Projekt *WingtipToys* , und wählen Sie die **OK** Schaltfläche. 

    ![Erstellen des Projekts - Dialogfeld "Neues Projekt"](create-the-project/_static/image2.png)

    > [!NOTE]
    > Der Name des Projekts in diesem Lernprogramm Reihe ist **WingtipToys**. Es wird empfohlen, dass Sie diese Option verwenden *genaue* Projektname, sodass der Code in der gesamten Reihe von Lernprogrammen Funktionen wie erwartet bereitgestellt.
6. Wählen Sie als Nächstes die **Web Forms** Vorlage, und wählen Sie die **Projekt erstellen** Schaltfläche.  

    ![Erstellen des Projekts - neue Projektvorlage](create-the-project/_static/image3.png)

Das Projekt dauert eine gewisse Zeit zu erstellen. Wenn Sie wieder bereit ist, öffnen Sie die **"default.aspx"** Seite.

![Erstellen des Projekts - neue Projektvorlage](create-the-project/_static/image4.png)

Sie können zwischen wechseln **Entwurf** anzeigen und **Quelle** Ansicht durch Auswahl einer Option am unteren Rand des Fensters Center. **Entwurf** zeigt ASP.NET Web Pages, Masterseiten, Inhaltsseiten, HTML-Seiten und Benutzersteuerelemente mithilfe einer fast-WYSIWYG-Ansicht. **Quelle** zeigt HTML-Markup für die Webseite, die Sie bearbeiten können.

> [!TIP] 
> 
> **Grundlegendes zu den ASP.NET-Frameworks**
> 
> ASP.NET Web Forms können Sie Builds dynamische Websites mithilfe eines vertrauten Drag & Drop, ereignisgesteuerte-Modells. Eine Entwurfsoberfläche und Hunderte von Steuerelementen und Komponenten ermöglichen Ihnen das schnelle einfach komplexe und umfangreiche GUI-gesteuerte Websites mit Datenzugriff erstellen. Der Wingtip Toys-Speicher basiert auf ASP.NET Web Forms, aber viele Konzepte, die Sie in diesem Lernprogramm Reihe Kennenlernen gelten für alle von ASP.NET.
> 
> ASP.NET bietet vier primäre Entwicklungsframeworks:
> 
> - [ASP.NET Web Forms](../../../index.md)  
>  Das Web Forms-Framework wendet sich an Entwickler, die lieber deklarative und Steuerelement basierende Programmierung, z. B. Microsoft Windows Forms (WinForms) und WPF/XAML/Silverlight. Er bietet ein Modell WYSIWYG-Designer-driven Development, daher ist es gängige für Entwickler, die Suche nach einer Umgebung rapid Application Development, (RAD), für die Webentwicklung. Wenn Sie mit dem Webprogrammierung vertraut sind und mit der herkömmlichen Microsoft RAD Client Entwicklungstools (z. B. Visual Basic und Visual c#) vertraut sind, können Sie schnell eine Webanwendung erstellen, ohne Sie Erfahrung mit HTML und JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC wendet sich an Entwickler, die Muster und Prinzipien wie eine testgesteuerte Entwicklung, die Abgrenzung von Problemen, die Umkehrung des Steuerelements (IoC) und abhängigkeiteneinschleusung (DI) interessiert sind. Dieses Framework vertraut zu machen, die Geschäftslogikschicht einer Web-Anwendung in der Darstellungsschicht trennen.
> - [ASP.NET-Webseiten 2](../../../../web-pages/index.md)  
>  ASP.NET Web Pages wendet sich an Entwickler, die eine einfache Web Development Story, nach dem Vorbild der PHP möchten. Im Modell Webseiten HTML-Seiten erstellen, und klicken Sie dann auf der Seite serverbasierten Code hinzufügen, um dynamisch zu steuern, wie dieses Markup gerendert wird. Web Pages ist speziell für eine einfache Framework werden, und es ist der einfachste Einstiegspunkt in ASP.NET für Personen, die wissen, HTML, aber möglicherweise keine umfassenden Programmierfunktionen - z. B., Studenten oder Tüftler, die. Es ist auch eine gute Möglichkeit für Entwickler von Websites, die wissen, PHP oder ähnliche Frameworks mithilfe von ASP.NET zu starten.
> - [ASP.NET Single-Page-Anwendung](../../../../single-page-application/index.md)  
>  ASP.NET Single-Page-Anwendung (SPA) können Sie Anwendungen erstellen, die erhebliche clientseitige Interaktion mit HTML 5, CSS 3 und JavaScript enthalten. ASP.NET und Web-Toolsupdate 2012.2 geliefert wird eine neue Vorlage zum Erstellen von einseitige Anwendungen mit knockout.js und ASP.NET Web-API. Zusätzlich zu den neuen SPA-Vorlage sind auch neue von der Community erstellte SPA Vorlagen zum Download zur Verfügung.
> 
> Zusätzlich zu den vier wichtigsten Entwicklungsframeworks bietet ASP.NET auch zusätzliche Technologien, die wichtig zu beachten und mit vertraut sein, aber nicht in diesem Lernprogramm Reihe behandelt:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -ein Framework zur Erstellung von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen.
> - [ASP.NET SignalR](../../../../signalr/index.md) -eine Bibliothek, die beim Entwickeln von Echtzeit-Webfunktionen erleichtert.


### <a name="reviewing-the-project"></a>Überprüfen das Projekt

In Visual Studio die **Projektmappen-Explorer** -Fenster können Sie die Dateien für das Projekt zu verwalten. Werfen wir einen Blick auf die Ordner, die zur Anwendung hinzugefügt wurden **Projektmappen-Explorer**. Die Web Application-Vorlage fügt eine grundlegende Ordnerstruktur:

![Erstellen des Projekts - Projektmappen-Explorer](create-the-project/_static/image5.png)

Visual Studio erstellt, einige ursprünglichen Ordner und Dateien für das Projekt. Die erste Dateien, die Sie später in diesem Lernprogramm arbeiten, sind die folgenden:

| **Datei** | **Purpose** |
| --- | --- |
| *Default.aspx* | In der Regel die erste Seite angezeigt, wenn die Anwendung in einem Browser ausgeführt wird. |
| *Site.Master* | Eine Seite, die Ihnen ermöglicht, ein konsistentes Layout und die Verwendung-Standardverhalten für Seiten in Ihrer Anwendung zu erstellen. |
| *Global.asax* | Eine optionale Datei, die Code für die Reaktion auf Ereignisse, die sich auf Anwendungsebene und auf Sitzungsebene ausgelöst von ASP.NET oder HTTP-Module enthält. |
| *Web.config* | Die Konfigurationsdaten für eine Anwendung. |

### <a name="running-the-default-web-application"></a>Die Standard-Web-Anwendung ausführen

Die standardwebanwendung bietet eine komfortable basierend auf integrierte Funktionen und Support. Ohne Änderungen auf das Standardprojekt für Web Forms ist die Anwendung auf Ihrem lokalen Webbrowser ausgeführt.

1. Drücken Sie die ***F5*** gedrückt, während in Visual Studio.   
 Die Anwendung erstellen und in Ihrem Webbrowser anzuzeigen.  

    ![Erstellen des Projekts - Standardseite](create-the-project/_static/image6.png)
2. Sobald Sie abgeschlossene Review die laufende Anwendung haben, schließen Sie das Browserfenster.

Es gibt drei wichtigsten Seiten in dieser Standard-Web-Anwendung: *"default.aspx"* (privat), *About.aspx*, und *Contact.aspx*. Jede dieser Seiten kann von der oberen Navigationsleiste erreicht werden. Es gibt auch zwei zusätzlichen Seiten, die im Ordner "Konto" enthalten, die Register.aspx Seite und die Seite "Login.aspx". Diese beiden Seiten können Sie die Mitgliedschaftsfunktionen von ASP.NET zum Erstellen, speichern und überprüfen die Anmeldeinformationen des Benutzers verwenden.

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Forms-Hintergrund

ASP.NET Web Forms sind die Seiten, die auf Microsoft ASP.NET Technologie basieren, in denen Code, der auf dem Server ausgeführt dynamisch wird Webseitenausgabe an den Browser oder Client-Gerät generiert. Eine ASP.NET Web Forms-Seite gerendert wird automatisch das korrekte Browser kompatiblen HTML für Funktionen wie Formatvorlagen, Layout und So weiter. WebForms sind kompatibel mit einer beliebigen Sprache, die von der .NET common Language Runtime, wie z. B. Microsoft Visual Basic und Visual c# unterstützt. Web Forms darüber hinaus basieren auf der [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), bietet Vorteile, z. B. einer verwalteten Umgebung, typsicherheit und Vererbung.

Wenn eine ASP.NET Web Forms-Seite ausgeführt wird, durchläuft die Seite einen Lebenszyklus, in dem sie eine Reihe von Verarbeitungsschritten ausführt. Diese Schritte umfassen Initialisierung, instanziieren Steuerelemente, wiederherstellen und Zustand beibehalten, das Ausführen von Ereignishandlercode und das Rendern. Wie Sie die Leistungsfähigkeit von ASP.NET Web Forms kennen gelernt, ist für die Ihnen beim Verständnis der [Lebenszyklus von ASP.NET-Seiten](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) , damit Sie Code in der entsprechenden Lebenszyklus Phase für den Effekt schreiben können, Sie möchten.

Bei ein Webserver eine Anforderung für eine Seite empfängt, sie sucht nach der Seite, verarbeitet, wird an den Browser gesendet und dann verwirft alle Informationen zur Seite. Wenn der Benutzer erneut die gleiche Seite anfordert, wiederholt der Server die gesamte Sequenz, die eine erneute Verarbeitung der Seite von Grund auf neu. Anders ausgedrückt, ein Server ist kein Arbeitsspeicher von Seiten verarbeitet Seiten verfügt zustandslos sind. Das ASP.NET-Seitenframework behandelt automatisch die Aufgabe der Verwaltung des Zustands von der Seite und seine Steuerelemente, und sie versorgt Sie mit expliziten weisen den Status der anwendungsspezifische Informationen beibehalten.

> [!TIP] 
> 
> **Web Application-Funktionen in die Vorlage für das Web Forms-Anwendung**
> 
> Der ASP.NET Web Forms-Anwendung-Vorlage stellt einen umfangreichen Satz von integrierten Funktionen bereit. Er bietet nicht nur mit einem *Home.aspx* Seite ein *About.aspx* Seite eine *Contact.aspx* Seite, sondern umfasst auch Mitgliedschaftsfunktionen, die Benutzer registriert und speichert Ihre Anmeldeinformationen, damit Ihre Website anmelden können. Diese Übersicht bietet weitere Informationen zu einigen der Features, die in der Vorlage für ASP.NET Web Forms-Anwendung und deren Verwendung in der Anwendung des Wingtip Toys enthalten sind.
> 
> **Mitgliedschaft**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identität speichert Ihrer Benutzer-Anmeldeinformationen in einer Datenbank, die von der Anwendung erstellt. Wenn Ihre Benutzer anmelden, überprüft die Anwendung ihre Anmeldeinformationen durch Lesen der Datenbank. Des Projekts *Konto* Ordner enthält die Dateien, die die verschiedenen Teilen von Mitgliedschaft zu implementieren: registrieren, anmelden, das Ändern des Kennworts und Autorisieren des Zugriffs. Darüber hinaus unterstützt die ASP.NET Web Forms OAuth- und OpenID. Diese Verbesserungen Authentifizierung Benutzern gestatten, in Ihre vorhandenen Anmeldeinformationen, derartige Konten wie Facebook, Twitter, Windows Live und Google-Website anzumelden.
> 
> ![Erstellen des Projekts - Projektmappen-Explorer (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Standardmäßig erstellt die Vorlage eine Mitgliedschaftsdatenbank mithilfe eines standarddatenbanknamens in einer Instanz von SQL Server Express LocalDB, dem Development-Datenbankserver, der mit Visual Studio Express 2013 für Web enthalten ist.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) ist eine vereinfachte Version von SQL Server, die viele Programmierbarkeitsfunktionen einer SQL Server-Datenbank verfügt. SQL Server Express LocalDB im Benutzermodus ausgeführt und verfügt über eine schnelle, Konfigurationsfreie Installation, die eine kurze Liste der erforderlichen Installationskomponenten verfügt. In Microsoft SQL Server, eine beliebige Datenbank oder Transact-SQL-Code kann von SQL Server Express LocalDB in SQL Server und SQL Azure ohne Upgradeschritte verschoben werden. Daher kann SQL Server Express LocalDB für Anwendungen für alle Editionen von SQL Server als entwicklerumgebung verwendet werden. SQL Server Express LocalDB aktiviert Funktionen z. B. gespeicherte Prozeduren, benutzerdefinierte Funktionen und Aggregate, .NET Framework-Integration, räumliche Typen und andere, die in SQL Server Compact nicht verfügbar sind.
> 
> **Masterseiten**
> 
> Ein [ASP.NET-Masterseite](https://msdn.microsoft.com/library/wtxbf3hh.aspx) ein einheitliches Aussehen und Verhalten für alle Seiten in der Anwendung definiert. Mit dem Inhalt einer einzelnen Inhaltsseite auf die letzte Seite zu erzeugen, die dem Benutzer angezeigt wird, werden das Layout der Masterseite zusammengeführt. In der Anwendung des Wingtip Toys, ändern Sie die *Site.master* Masterseite, sodass alle Seiten in der Website des Wingtip Toys dieselbe unterschiedliche Logo und Navigation Leiste freigeben.
> 
> **HTML5**
> 
> Die Vorlage für ASP.NET Web Forms-Anwendung unterstützt [HTML5](http://www.w3schools.com/html/html5_intro.asp), dies ist die neueste Version der HTML-Markup Language. HTML5 unterstützt neue Elemente und Funktionen, die zum Erstellen von Websites zu vereinfachen.
> 
> **Modernizr**
> 
> Für Browser, die nicht mit HTML5 unterstützen, können Sie [Modernizr](http://www.modernizr.com/). Modernizr ist eine Open Source-JavaScript-Bibliothek, die erkennt, ob ein Browser HTML5-Funktionen unterstützt, und aktivieren Sie diese, wenn dies nicht der Fall. In der Vorlage ASP.NET Web Forms-Anwendung wird von Modernizr als ein NuGet-Paket installiert.
> 
> **Bootstrap**
> 
> Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Designumgebung-Framework von Twitter erstellt. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts dynamisch an die anderen Browser Fenstergrößen angepasst werden können. Der Bootstrap-Designumgebung-Funktion können auch einfach eine Änderung in der Anwendung aussehen und Verhalten wirksam. Standardmäßig enthält der ASP.NET Web Application-Vorlage in Visual Studio 2013 Bootstrap als NuGet-Paket.
> 
> **NuGet-Pakete**
> 
> Die Vorlage für ASP.NET Web Forms-Anwendung enthält eine Reihe von [NuGet](http://www.nuget.org/) Pakete. Diese Pakete stellen als Komponente aufbereitete Funktionen in Form von open Source-Bibliotheken und Tools bereit. Es gibt eine Vielzahl von Paketen zum Erstellen und Testen Ihre Anwendungen. Visual Studio erleichtert das Hinzufügen, entfernen und Aktualisieren von NuGet-Pakete. Entwickler können erstellen und Pakete sowie zu NuGet hinzufügen.
> 
> ![Projekt - NuGet-Dialogfeld "erstellen"](create-the-project/_static/image8.png)
> 
> Bei der Installation eines Pakets NuGet kopiert Dateien in der Projektmappe, und Sie werden automatisch macht alle Änderungen, die erforderlich sind, z. B. Hinzufügen von verweisen, oder ändern die Konfiguration der Webanwendung zugeordnet. Wenn Sie die Bibliothek entfernen möchten, wird NuGet werden Dateien entfernt, und kehrt alle Änderungen in Ihrem Projekt vorgenommen werden, damit, dass keine übersichtliche verbleibt. NuGet ist verfügbar, von der **Tools** Menü in Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) ist eine schnelle und präzise JavaScript-Bibliothek, die das HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Aktivitäten für die schnelle Webentwicklung vereinfacht. Die jQuery JavaScript-Bibliothek ist in der Vorlage für ASP.NET Web Forms-Anwendung als ein NuGet-Paket enthalten.
> 
> **Unaufdringlichen Überprüfung**
> 
> Integrierte Validierungssteuerelemente wurden konfiguriert, um unaufdringliches JavaScript für die clientseitige Validierungslogik verwenden. Dies erheblich reduziert die Menge an JavaScript Inline im Markup Seite gerendert, und verringert die Gesamtgröße der Seite. Unaufdringlichen Überprüfung Global hinzugefügt wird, der ASP.NET Web Forms-Anwendung Vorlage basierend auf der Einstellung in der &lt;"appSettings"&gt; Element von der *"Web.config"* Datei im Stammverzeichnis der Anwendung.
> 
> **Entity Framework Code First**
> 
> Neben den Funktionen in der Vorlage für ASP.NET Web Forms-Anwendung, die Anwendung des Wingtip Toys verwendet [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), dies ist ein NuGet-Bibliothek, die Code-orientierte Entwicklung ermöglicht, bei der Arbeit mit Daten. Einfach ausgedrückt, wird den datenbankbezogene Teil Ihrer Anwendung basierend auf den von Ihnen geschriebenen Code erstellt. Mit dem Entity Framework, abrufen und Bearbeiten von Daten als stark typisierte Objekte. Damit Sie in die Details wie die Daten zugegriffen wird, anstatt die Anwendung die Geschäftslogik konzentrieren können.
> 
> Weitere Informationen zu den installierten Bibliotheken und Pakete, die mit der ASP.NET Web Forms-Vorlage enthalten finden Sie in der Liste der installierten NuGet-Pakete. Zu diesem Zweck erstellen Sie In Visual Studio ein neues Web Forms-Projekt, die Option **Tools**  - &gt; **Bibliothekspaket-Manager**  - &gt; **verwalten NuGet-Pakete für Projektmappe**, und wählen Sie **installierte Pakete** in der **NuGet-Pakete verwalten** (Dialogfeld).


### <a name="touring-visual-studio"></a>Visual Studio Touring

Die wichtigsten Fenster in Visual Studio enthalten die **Projektmappen-Explorer**, **Server-Explorer** (**Datenbank-Explorer** in Express), wird die **Eigenschaften Fenster**, **Toolbox**, **Symbolleiste**, und die **Dokumentfenster**.

![Projekt - NuGet-Dialogfeld "erstellen"](create-the-project/_static/image9.png)

Weitere Informationen zu Visual Studio finden Sie unter [Visual Visual Web Developer-Handbuch](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie erstellt haben, führen Sie die Standard-Web Forms-Anwendung und überprüft. Sie haben haben gelernt, einige Grundlagen zur Verwendung von Visual Studio-Umgebung und überprüft die verschiedenen Funktionen von der Standardeinstellung Web Forms-Anwendung. In den folgenden Lernprogrammen erstellen Sie die Datenzugriffsebene.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Auswahl des richtigen Programmiermodell](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Webanwendungsprojekten Websiteprojekte](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web Forms-Seiten (Übersicht)](https://msdn.microsoft.com/library/428509ah.aspx)

>[!div class="step-by-step"]
[Zurück](introduction-and-overview.md)
[Weiter](create_the_data_access_layer.md)
