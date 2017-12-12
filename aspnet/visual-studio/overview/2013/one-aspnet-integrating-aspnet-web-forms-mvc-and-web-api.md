---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Praktische Übungseinheiten: One ASP.NET\": Integrieren von ASP.NET Web Forms, MVC und Web-API | Microsoft Docs"
author: rick-anderson
description: ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie MVC, Web-API und andere. Mit der Erweiterung ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Praktische Übungseinheiten: One ASP.NET": Integrieren von ASP.NET Web Forms, MVC und Web-API
====================
durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie MVC, Web-API und andere. Mit der Erweiterung aufgetreten ASP.NET seit seiner Erstellung und die ausdrückliche müssen diese Technologien integriert haben, stattgefunden haben aktuelle Aufwand arbeiten für **One ASP.NET"**.
> 
> Visual Studio 2013 führt ein neues einheitliche Projektsystem können Sie eine Anwendung erstellen und verwenden die ASP.NET-Technologien in einem Projekt ein. Dieses Feature entfällt die Notwendigkeit, eine Technologie zu Beginn eines Projekts und mit dem sie auswählen, und stattdessen empfiehlt die Verwendung mehrerer ASP.NET Frameworks in einem Projekt.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen einer Website auf der Grundlage der **One ASP.NET"** Projekttyp
- Verwenden von unterschiedlichen **ASP.NET** Frameworks wie **MVC** und **Web-API** im selben Projekt
- Identifizieren Sie die Hauptkomponenten einer **ASP.NET** Anwendung
- Nutzen Sie die **ASP.NET Gerüstbau** Framework zum automatischen Erstellen von Controller und Ansichten zum Ausführen von CRUD-Vorgängen basierend auf Ihren Modellklassen
- Verfügbar machen Sie den gleichen Satz von Informationen in den Computer und lesbare Formate, die mit dem richtigen Tool für jeden Auftrag

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie Windows-Explorer und Navigieren in des Labors **Quelle** Ordner.
2. Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese Umgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden die Codeausschnitte

In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie in Visual Studio 2013 zu vermeiden, dass sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung wird eine beginnend Projektmappe befindet sich im beiliegen der **beginnen** Ordner der Übung, die Ihnen ermöglicht, jede Übung unabhängig von den anderen folgen. Denken Sie daran, dass die Codeausschnitte, die während einer Übung hinzugefügt werden in diese Lösungen starten fehlen, werden und funktionieren möglicherweise nicht, bis die Übung abgeschlossen ist. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Erstellen eines neuen Web Forms-Anwendungsprojekts](#Exercise1)
2. [Erstellen einen MVC-Controller, die mithilfe von Gerüstbau](#Exercise2)
3. [Erstellen eine Web-API-Controller, die mithilfe von Gerüstbau](#Exercise3)

Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt. In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Übung 1: Erstellen eines neuen Web Forms-Anwendungsprojekts

In dieser Übung erstellen Sie eine neue Web Forms-Website in Visual Studio 2013 verwenden die **One ASP.NET"** einheitliche Projekt, dem Sie Web Forms, MVC und Web-API-Komponenten in der gleichen Anwendung leicht zu integrieren können. Anschließend untersuchen Sie die generierte Lösung und die Teile zu identifizieren, und abschließend sehen Sie die Website in Aktion.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Aufgabe 1 – erstellen einen neuen Standort mithilfe einer ASP.NET-Erfahrung

In dieser Aufgabe Sie starten eine neue Website erstellen, in Visual Studio auf der Grundlage der **One ASP.NET"** Projekttyp. **Eine ASP.NET** vereinheitlicht alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, mischen und zuordnen diese wie gewünscht. Klicken Sie dann, erkennen Sie die verschiedenen Komponenten von Web Forms, MVC und Web-API, die live paralleler Ausführung innerhalb der Anwendung.

1. Open **Visual Studio Express 2013 für Web** , und wählen Sie **Datei | Neues Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **ASP.NET-Webanwendung** unter der **Visual C# | Web** Registerkarte, und stellen Sie sicher, dass **.NET Framework 4.5** ausgewählt ist. Nennen Sie das Projekt *MyHybridSite*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Neues ASP.NET Web Application-Projekt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Erstellen eines neuen Projekts für die ASP.NET-Webanwendung*
3. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage, und wählen die **MVC** und **Web-API** Optionen. Stellen Sie außerdem sicher, dass die **Authentifizierung** Option wird festgelegt, um **einzelne Benutzerkonten**. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Erstellen ein neues Projekt mit der Web Forms-Projektvorlage, einschließlich von Web-API und MVC-Komponenten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Erstellen ein neues Projekt mit der Web Forms-Projektvorlage, einschließlich von Web-API und MVC-Komponenten*
4. Sie können jetzt die Struktur der generierten Lösung durchsuchen.

    ![Untersuchen die generierte Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Untersuchen die generierte Lösung*

    1. **Konto:** dieser Ordner enthält das Web Form-Seiten zum Registrieren, melden Sie sich an und Verwalten von Benutzerkonten für die Anwendung. In diesem Ordner hinzugefügt, wenn die **einzelne Benutzerkonten** Authentifizierungsoption während der Konfiguration der Web Forms-Projektvorlage ausgewählt ist.
    2. **Modelle:** dieser Ordner enthält die Klassen, die Ihre Anwendungsdaten darstellen.
    3. **Controller** und **Ansichten**: Diese Ordner sind erforderlich, damit die **ASP.NET-MVC** und **ASP.NET Web API** Komponenten. Untersuchen Sie die Technologien MVC und Web-API in der nächsten Übung.
    4. Die **"default.aspx"**, **Contact.aspx** und **About.aspx** Dateien sind vordefinierte Web Form-Seiten, die Sie als Ausgangspunkt verwenden können, erstellen Sie die Seiten, die spezifisch für Ihre die Anwendung. Die Programmierlogik der Dateien befindet sich in einer separaten Datei genannt der &quot;Code-Behind-&quot; gemäß der ein &quot;. aspx.cs&quot; oder &quot;. aspx.vb&quot; Erweiterung (je nach den verwendete Sprache). Die Code-Behind-Logik auf dem Server ausgeführt wird und die HTML-Ausgabe für die Seite dynamisch erzeugt.
    5. Die **Site.Master** und **Site.Mobile.Master** Seiten definieren das Aussehen und Verhalten und das Standardverhalten aller Seiten in der Anwendung.
5. Doppelklicken Sie auf die **"default.aspx"** Datei, um den Inhalt der Seite zu untersuchen.

    ![Untersuchen die Seite "default.aspx"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Untersuchen die Seite "default.aspx"*

    > [!NOTE]
    > Die **Seite** Direktive am Anfang der Datei definiert die Attribute eines Web Forms-Seite. Z. B. die **MasterPageFile** -Attribut gibt den Pfad zum Master Seite – in diesem Fall die *Site.Master* Seiten- und **Inherits** Attribut definiert die Code-Behind-Klasse für die Seite zu erben. Diese Klasse befindet sich in der Datei, die bestimmt, indem die **CodeBehind** Attribut.
    > 
    > Die **Asp: Inhalt** Steuerelement enthält den tatsächlichen Inhalt der Seite (Text, Markup und Steuerelementen) und zugeordnet wird eine **Asp: ContentPlaceHolder** Steuerelement auf der Masterseite. In diesem Fall wird der Inhalt der Seite gerendert werden, in der *MainContent* Steuerelement definiert, der *Site.Master* Seite.
6. Erweitern Sie die **App\_starten** Ordner, und beachten Sie, dass die **WebApiConfig.cs** Datei. Visual Studio enthalten diese Datei in der generierten Lösung, da Sie beim Konfigurieren von Ihrem Projekts mit der Vorlage eine ASP.NET Web-API enthalten.
7. Öffnen der **WebApiConfig.cs** Datei. In der *"webapiconfig"* Klasse finden Sie die Konfiguration zugeordnet Web-API, die HTTP zugeordnet wird leitet **Web-API-Controller**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Öffnen der **RouteConfig.cs** Datei. Innerhalb der *RegisterRoutes* Methode finden Sie die Konfiguration zugeordnet MVC, das die HTTP-Routen an ordnet **MVC-Controller**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Projektmappe

Sie wird in dieser Aufgabe führen Sie die Projektmappe generierte, untersuchen Sie die app und einige Funktionen, wie URLs und die integrierte Authentifizierung.

1. Um die Projektmappe auszuführen, drücken Sie die **F5** oder klicken Sie auf die **starten** Schaltfläche auf der Symbolleiste befinden. Die Startseite der Anwendung sollte im Browser geöffnet werden.

    ![Ausführen der Projektmappe](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Stellen Sie sicher, dass die Web Forms-Seiten aufgerufen werden. Zu diesem Zweck fügen **/contact.aspx** an die URL in die Adressleiste ein und drücken Sie **EINGABETASTE**.

    ![Friendly URLs](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Friendly URLs*

    > [!NOTE]
    > Wie Sie sehen können, ändert sich die URL in **/wenden Sie sich an**. Beginnend mit **ASP.NET 4**, URL-routing-Funktionen wurden hinzugefügt, um Web Forms, damit Sie schreiben können, wie URLs  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  statt  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Weitere Informationen finden Sie unter [URL-Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Untersuchen Sie nun die Authentifizierungsablauf in die Anwendung integriert. Klicken Sie hierzu auf **registrieren** in der oberen rechten Ecke der Seite.

    ![Registrieren eines neuen Benutzers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrieren eines neuen Benutzers*
4. In der **registrieren** geben eine **Benutzername** und **Kennwort**, und klicken Sie dann auf **registrieren**.

    ![Seite "registrieren"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Seite "registrieren"*
5. Die Anwendung wird das neue Konto registriert, und der Benutzer authentifiziert ist.

    ![Benutzerauthentifizierung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Benutzerauthentifizierung*
6. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Übung 2: Erstellen eines MVC-Controllers mithilfe von Gerüstbau

In dieser Übung gelangen Sie sicherungsframework von ASP.NET Gerüstbau bereitgestellt, die von Visual Studio und erstellen einen ASP.NET MVC 5-Controller mit Aktionen und Razor-Ansichten zum CRUD-Vorgänge ausgeführt werden, ohne eine einzige Codezeile schreiben zu müssen. Der Prozess des Gerüstbaus wird Entity Framework Code First verwenden, um den Datenkontext und das Datenbankschema in der SQL-Datenbank zu generieren.

**Über Entity Framework Code First**

Entity Framework (EF) ist eine objektrelationale Mapper (ORM), die Ihnen zum Erstellen von datenzugriffsanwendungen mittels Programmierung mit einem konzeptionellen Anwendungsmodell anstelle des Programmierens direkt mit einem Speicherschema ermöglicht.

Der Entity Framework Code First Modellierung-Workflow ermöglicht es Ihnen, Ihren eigenen Domänenklassen zu verwenden, um das Modell darzustellen, dem EF beim Ausführen von Abfragen nutzt, Nachverfolgen von Änderungen und Update-Funktionen. Mithilfe des Code First-Entwicklung-Workflows, müssen nicht auf Ihre Anwendung zu beginnen, indem Sie das Erstellen einer Datenbank oder das Angeben eines Schemas. Stattdessen können Sie .NET Standardklassen, die definieren, die am besten geeigneten domänenmodellobjekten für Ihre Anwendung schreiben, und Entity Framework wird die Datenbank für Sie erstellt.

> [!NOTE]
> Weitere Informationen finden Sie Informationen zu Entity Framework [hier](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Aufgabe 1 – erstellen ein neues Modell

Definieren Sie jetzt eine **Person** -Klasse, die das Modell durch den Prozess des Gerüstbaus zum Erstellen von MVC-Controller und Ansichten verwendet werden. Beginnen Sie mit dem Erstellen einer **Person** Modellklasse, und die CRUD-Vorgänge im Controller automatisch erstellt werden mithilfe von Gerüstbau Funktionen.

1. Open **Visual Studio Express 2013 für Web** und die **MyHybridSite.sln** Projektmappe befindet sich in der **Quell-/Ex2-MvcScaffolding/Anfang** Ordner. Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Modelle** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Klasse...** .

    ![Modellklasse von Person hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Modellklasse von Person hinzufügen*
3. In der **neues Element hinzufügen** (Dialogfeld), benennen Sie die Datei *Person.cs* , und klicken Sie auf **hinzufügen**.

    ![Erstellen der Person-Modell-Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Erstellen der Person-Modell-Klasse*
4. Ersetzen Sie den Inhalt der **Person.cs** Datei durch den folgenden Code. Drücken Sie **STRG + S** um die Änderungen zu speichern.

    (Codeausschnitt - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. In **Projektmappen-Explorer**, mit der rechten Maustaste die **MyHybridSite** Projekt, und wählen Sie **erstellen**, oder drücken Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Aufgabe 2: Erstellen eines MVC-Controllers

Nachdem die **Person** Modell erstellt wurde, verwenden Sie ASP.NET-MVC-Gerüstbau mit Entity Framework zum Erstellen der CRUD-Controlleraktionen und Ansichten für **Person**.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Neues Gerüst...** .

    ![Erstellen eines neuen scaffolded Domänencontrollers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Erstellen eines neuen Migrationsgerüst-Domänencontrollers*
2. In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** , und klicken Sie dann auf **hinzufügen.**

    ![MVC 5-Controller mit Ansichten und Entity Framework auswählen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *MVC 5-Controller mit Ansichten und Entity Framework auswählen*
3. Legen Sie *MvcPersonController* als der **Controllernamen**, wählen die **asynchrone Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  als die **Modellschemas**.

    ![Einen MVC-Controller mit Gerüst hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Einen MVC-Controller mit Gerüst hinzufügen*
4. Klicken Sie unter **Datenkontextklasse**, klicken Sie auf **neuen Datenkontext...** .

    ![Erstellen einen neuen Datenkontext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Erstellen einen neuen Datenkontext*
5. In der **neuen Datenkontext** Dialogfeld Feld "Name" den neuen Datenkontext *PersonContext* , und klicken Sie auf **hinzufügen**.

    ![Erstellen neue PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Erstellen den neuen PersonContext-Typ*
6. Klicken Sie auf **hinzufügen** zum Erstellen der neuen Controllers für **Person** mit Gerüstbau. Visual Studio generiert die Controlleraktionen, den Datenkontext Person und Razor-Ansichten.

    ![Nach dem Erstellen der MVC-Controller mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Nach dem Erstellen der MVC-Controller mit Gerüstbau*
7. Öffnen der **MvcPersonController.cs** in der Datei die **Controller** Ordner. Beachten Sie, dass die CRUD-Aktionsmethoden automatisch generiert wurden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Durch Auswahl der **asynchrone Controlleraktionen verwenden** Kontrollkästchen aus das Gerüst Optionen in den vorherigen Schritten, generiert Visual Studio asynchrone Aktionsmethoden für alle Aktionen, die Zugriff auf den Datenkontext Person einschließen. Es wird empfohlen, die Verwendung asynchroner Aktionsmethoden für lang andauernde, nicht CPU-gebundene Anforderungen, um zu vermeiden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3: Ausführen der Projektmappe

In dieser Aufgabe führen Sie der Projektmappe erneut aus, um zu überprüfen, ob die Ansichten für **Person** sind wie erwartet funktioniert. Fügen Sie eine neue Person ein, um sicherzustellen, dass es erfolgreich in der Datenbank gespeichert ist.

1. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Navigieren Sie zu **/MvcPerson**. Die scaffolded an, die die Liste der Personen zeigt, sollte angezeigt werden.
3. Klicken Sie auf **neu erstellen** eine neue Person hinzufügen.

    ![Navigieren zu der scaffolded MVC-Ansichten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navigieren zu der scaffolded MVC-Ansichten*
4. In der **erstellen** anzuzeigen, geben Sie einen **Namen** und ein **Alter** für die Person, und klicken Sie auf **erstellen**.

    ![Eine neue Person hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Eine neue Person hinzufügen*
5. Die neue Person wird zur Liste hinzugefügt. Klicken Sie in der Liste der Elemente auf **Details** der Person-Detailansicht angezeigt. Klicken Sie auf die **Details** anzuzeigen, klicken Sie auf **zurück zur Listenansicht** um zurück zur Listenansicht zu wechseln.

    ![Person Details anzuzeigen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Person Details anzuzeigen*
6. Klicken Sie auf die **löschen** Link zu der Person zu löschen. In der **löschen** anzuzeigen, klicken Sie auf **löschen** zum Bestätigen des Vorgangs.

    ![Durch das Löschen einer person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Durch das Löschen einer person*
7. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Übung 3: Erstellen einer Web-API-Controller, die mithilfe von Gerüstbau

Die Web-API-Framework ist Teil des Stapels ASP.NET und entwickelt, um die implementierende HTTP-Dienste, in der Regel senden und Empfangen von Daten über eine REST-API JSON oder XML-Format zu vereinfachen.

In dieser Übung werden Sie ASP.NET Gerüstbau erneut verwenden, um eine Web-API-Controller zu generieren. Verwenden Sie die gleiche **Person** und **PersonContext** Klassen aus der vorherigen Übung, um die gleiche Person Daten im JSON-Format bereitzustellen. Es wird angezeigt, wie Sie die gleichen Ressourcen auf unterschiedliche Weise in der gleichen ASP.NET-Anwendung verfügbar gemacht werden können.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Aufgabe 1 – erstellen eine Web-API-Controller

In dieser Aufgabe erstellen Sie ein neues **Web-API-Controller** , wird die Person-Daten in einem Computer die Verwendbarkeit Format wie JSON offenlegen.

1. Wenn nicht bereits geöffnet haben, öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **MyHybridSite.sln** Projektmappe befindet sich in der **Quell-/Ex3-WebAPI/Anfang** Ordner. Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.

    > [!NOTE]
    > Wenn Sie mit der Begin-Lösung von Übung 3 starten, drücken Sie **STRG + UMSCHALT + B** auf die Projektmappe zu erstellen.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Neues Gerüst...** .

    ![Erstellen eines neuen scaffolded Domänencontrollers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Erstellen eines neuen scaffolded Domänencontrollers*
3. In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Web-API** im linken Bereich, klicken Sie dann **Web API 2-Controller mit Aktionen unter Verwendung von Entity Framework** im mittleren Bereich, und klicken Sie dann auf  **Fügen Sie hinzu.**

    ![Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "auswählen von Web-API 2-Controller mit Aktionen und Entity Framework")

    *Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework*
4. Legen Sie *ApiPersonController* als der **Controllernamen**, wählen die **asynchrone Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  und **PersonContext (MyHybridSite.Models)** als die **Modell** und **Datenkontext** bzw. Klassen. Klicken Sie anschließend auf **Hinzufügen**.

    ![Ein Web-API-Controller mit Gerüst hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "einen Web-API-Controller mit Gerüst hinzufügen")

    *Einen Web-API-Controller mit Gerüst hinzufügen*
5. Visual Studio generiert die **ApiPersonController** Klasse mit vier CRUD-Aktionen, um mit Ihren Daten zu arbeiten.

    ![Nach dem Erstellen des Web-API-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "nach dem Erstellen des Web-API-Controllers mit Gerüstbau")

    *Nach dem Erstellen des Web-API-Controllers mit Gerüstbau*
6. Öffnen der **ApiPersonController.cs** Datei, und überprüfen Sie die *"GetPeople"* Aktionsmethode. Diese Methode fragt das DB-Feld der **PersonContext** Typ, um die Personen Daten abzurufen.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Betrachten Sie nun den Kommentar oberhalb der Methodendefinition. Er bietet den URI, der diese Aktion verfügbar macht, die Sie in der nächsten Aufgabe verwendet werden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Standardmäßig Web-API ist so konfiguriert, dass Abfragen zum Abfangen der */API* Pfad zum Vermeiden von Konflikten mit MVC-Controller. Wenn Sie diese Einstellung ändern möchten, finden Sie in [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Projektmappe

In dieser Aufgabe verwenden Sie Internet Explorer **F12 Entwicklertools** , die vollständige Antwort aus dem Web-API-Controller zu überprüfen. Es wird angezeigt, wie Sie den Netzwerkverkehr, um weitere Einblicke in Ihre Anwendungsdaten zu erhalten erfassen können.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** ausgewählt ist, der **starten** Schaltfläche befindet sich auf der Symbolleiste von Visual Studio.
> 
> ![Internet Explorer-option](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Die **F12 Entwicklertools** haben ein umfangreichen Satzes von Funktionen, die in diese praktische Übung-on-Übung nicht behandelt wird. Wenn Sie mehr darüber erfahren möchten, lesen Sie [mit den F12 Entwicklertools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Drücken Sie **F5** um die Projektmappe auszuführen.

    > [!NOTE]
    > Um diesen Task ordnungsgemäß ausführen zu können, muss Ihre Anwendung Daten verfügen. Wenn die Datenbank leer ist, können Sie zurückkehren in Aufgabe 3 in Übung 2 und führen Sie die Schritte zum Erstellen einer neuen Person, die unter Verwendung der MVC-Ansichten.
2. Drücken Sie im Browser **F12** So öffnen die **Entwicklertools** Bereich. Drücken Sie **STRG** + **4** oder klicken Sie auf die **Netzwerk** Symbol, und klicken Sie dann auf der grüne Pfeil-Schaltfläche, um das Erfassen des Netzwerkverkehrs zu beginnen.

    ![Initiieren von Web-API-netzwerkerfassung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "netzwerkerfassung initiieren, Web-API")

    *Web-API-netzwerkerfassung initiieren*
3. Append **-api/ApiPerson** an die URL in die Adressleiste des Browsers angezeigt. Sie werden jetzt die Details der Antwort vom Überprüfen der **ApiPersonController**.

    ![Abrufen von Daten der Person über Web-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Abrufen von Daten der Person über Web-API")

    *Abrufen von Daten der Person über Web-API*

    > [!NOTE]
    > Sobald der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei herzustellen. Lassen Sie das Dialogfeld geöffnet, um des Inhalts der Antwort durch den Entwickler Toolfenster beobachten können.
4. Sie werden nun den Text der Antwort überprüfen. Klicken Sie hierzu auf die **Details** Registerkarte, und klicken Sie dann auf **Antworttext**. Sehen Sie sich, dass die heruntergeladenen Daten eine Liste von Objekten mit den Eigenschaften **Id**, **Namen** und **Alter** entsprechen den **Person** -Klasse.

    ![Anzeigen von Web-API-Antworttext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Anzeigen von Web-API-Antworttext")

    *Anzeigen von Web-API-Antworttext*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Aufgabe 3: Hinzufügen von Web-API-Hilfeseiten

Wenn Sie eine Web-API erstellen, ist es hilfreich, eine Hilfeseite erstellen, damit andere Entwickler wissen, wie Ihre API aufgerufen werden. Sie erstellen und die Dokumentationsseiten manuell aktualisieren, aber es ist besser, automatisch, um zu vermeiden, dass Wartungsarbeiten Vorgehensweise generiert. In dieser Aufgabe verwenden Sie ein NuGet-Paket zum automatischen Generieren von Web-API-Hilfeseiten der Projektmappe.

1. Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**.
2. In der **Package Manager Console** Fenster, führen Sie den folgenden Befehl:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Die **Microsoft.AspNet.WebApi.HelpPage** Paket installiert die erforderlichen Assemblys und fügt der MVC-Ansichten für die Hilfeseiten unter der **Bereiche/HelpPage** Ordner.

    ![HelpPage Bereich](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage-Bereich")

    *HelpPage Bereich*
3. Standardmäßig wird die Hilfe über Seiten verfügen Platzhalter-Zeichenfolgen für die Dokumentation. XML-Dokumentationskommentare können Sie um die Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen die **HelpPageConfig.cs** -Datei die **Bereiche/HelpPage/App\_starten** Ordner und die auskommentierung der folgenden Zeile:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. In **Projektmappen-Explorer**, mit der rechten Maustaste des Projekts **MyHybridSite**Option **Eigenschaften** , und klicken Sie auf die **erstellen** Registerkarte.

    ![Registerkarte "erstellen"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Abschnitt erstellen")

    *Registerkarte "erstellen"*
5. Klicken Sie unter **Ausgabe**Option **XML-Dokumentationsdatei**. Geben Sie im Bearbeitungsfeld **App\_Data/XmlDocument.xml**.

    ![Ausgabe in der Registerkarte "Build" im Abschnitt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Ausgabe Abschnitt auf der Registerkarte "Build"")

    *Ausgabeabschnitt auf der Registerkarte "Build"*
6. Drücken Sie **STRG** + **S** um die Änderungen zu speichern.
7. Öffnen der **ApiPersonController.cs** -Datei von der **Controller** Ordner.
8. Geben Sie eine neue Zeile zwischen den *"GetPeople"* Methodensignatur und die */ / api/ApiPerson abrufen* kommentieren, und geben Sie dann drei Schrägstriche.

    > [!NOTE]
    > Visual Studio fügt automatisch die XML-Elemente, die die Dokumentation der Methode zu definieren.
9. Hinzufügen einer summary-Text und der Rückgabewert für die *"GetPeople"* Methode. Es sollte wie folgt aussehen.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Drücken Sie **F5** um die Projektmappe auszuführen.
11. Append **/help** an die URL in der Adressleiste, um die Hilfeseite zu suchen.

    ![ASP.NET Web-API-Hilfeseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web-API-Hilfeseite")

    *ASP.NET Web API Help Page*

    > [!NOTE]
    > Der Hauptinhalt der Seite ist eine Tabelle von APIs, die vom Netzwerkcontroller gruppiert. Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle. Wenn Sie hinzufügen oder Aktualisieren von ein API-Controller, wird die Tabelle automatisch das nächste Mal aktualisiert werden, die die Anwendung zu erstellen.
    > 
    > Die **API** Spalte listet die HTTP-Methode und des relativen URI. Die **Beschreibung** Spalte enthält Informationen, die aus der Dokumentation der Methode extrahiert wurde.
12. Beachten Sie, dass die Beschreibung an, die Sie hinzugefügt haben, über die Methodendefinition in der Description-Spalte angezeigt wird.

    ![API-methodenbeschreibung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Beschreibung der API-Methode")

    *Beschreibung der API-Methode*
13. Klicken Sie auf die API-Methoden zum Navigieren zu einer Seite mit ausführlicheren Informationen, einschließlich beispielantworttexte.

    ![Detail-Informationsseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detailinformationen zur Seite "")

    *Seite "Informationen"*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Dieser praktischen Übungseinheit abschließen, indem Sie haben gelernt, wie Sie:

- Erstellen einer neuen Webanwendung verwenden eine ASP.NET-Umgebung in Visual Studio 2013
- Mehrere Technologien von ASP.NET in ein einzelnes Projekt integrieren
- Generieren Sie aus Ihren Modellklassen mithilfe von Gerüstbau für ASP.NET MVC-Controller und Ansichten
- Generieren von Web-API-Controller, die Features wie z. B. für die asynchrone Programmierung und Datenzugriff über Entity Framework verwenden
- Web-API-Hilfeseiten automatisch zu generieren, für die Controller
