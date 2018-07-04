---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Praxisnahe Übung: One ASP.NET: Integrieren von ASP.NET Web Forms, MVC und Web-API | Microsoft-Dokumentation'
author: rick-anderson
description: ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie z. B. MVC, Web-API und andere. Aufgrund der steigenden ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382946"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Praxisnahe Übung: One ASP.NET: Integrieren von ASP.NET Web Forms, MVC und Web-API
====================
durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie z. B. MVC, Web-API und andere. Aufgrund der steigenden ASP.NET gesehen hat, seit ihrer Erstellung und die ausgedrückt müssen diese Technologien integriert, es gab aktuellen bemühungen in funktionierenden für **One ASP.NET**.
> 
> Visual Studio 2013 bietet es sich um ein neues einheitliches Projektsystem können Sie eine Anwendung erstellen und verwenden die ASP.NET-Technologien in einem Projekt. Diese Funktion entfällt die Notwendigkeit, eine Technologie zu Beginn eines Projekts und mit der sie auswählen, und stattdessen empfiehlt die Verwendung von mehreren ASP.NET Frameworks in einem Projekt.
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen einer Website auf der Grundlage der **One ASP.NET** Projekttyp
- Andere **ASP.NET** Frameworks wie **MVC** und **Web-API-** im selben Projekt
- Identifizieren Sie die wichtigsten Komponenten von einer **ASP.NET** Anwendung
- Profitieren Sie von der **ASP.NET-Gerüstbau** Framework zum automatischen Erstellen von Controllern und Ansichten zum Durchführen von CRUD-Vorgänge basierend auf Ihren Modellklassen
- Bieten Sie die gleichen Informationen in Computer und lesbaren Formaten, die mit dem richtigen Tool für jeden Auftrag

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen von Windows-Explorer und navigieren Sie zu des Labs die **Quelle** Ordner.
2. Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** zum Starten des Setup-Prozesses, die die Umgebung konfigurieren, und installieren die Visual Studio-Codeausschnitte für diese Übung.
3. Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese laborumgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden von Codeausschnitten

In diesem Dokument Lab werden Sie aufgefordert, zum Einfügen von Codeblöcken. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitten, die Sie in Visual Studio 2013, um zu vermeiden, müssen sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung umfasst eine ab Lösung befindet sich in der **beginnen** Ordner der Übung, mit dem Sie jede Übung unabhängig von den anderen verfolgen kann. Bedenken Sie bitte, dass die Codeausschnitte, die während der Übung hinzugefügt werden fehlen aus diesen Lösungen ab und funktioniert möglicherweise nicht, bis Sie in dieser Übung abgeschlossen haben. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der Visual Studio-Projektmappe mit dem Code, die aus der Schritte in der entsprechenden Übung enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Erstellen eines neuen Web Forms-Projekts](#Exercise1)
2. [Erstellen eines MVC-Controllers mithilfe von Gerüstbau](#Exercise2)
3. [Erstellen einer Web-API-Controller mithilfe von Gerüstbau](#Exercise3)

Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient einem bestimmten Entwicklungsstil und bestimmt, Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Dialogfeld "Optionen". In dieser Übung wird beschrieben, die erforderlichen Aktionen zum Ausführen der jeweiligen Aufgabe in Visual Studio bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wenn Sie eine Sammlung mit anderen Einstellungen für Ihre Entwicklungsumgebung auswählen, unter Umständen gibt es bestehen Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Übung 1: Erstellen eines neuen Web Forms-Projekts

In dieser Übung erstellen Sie eine neue Web Forms-Website in Visual Studio 2013 verwenden die **One ASP.NET** einheitliche Projekt, sodass Sie die einfache Integration von Web Forms, MVC und Web-API-Komponenten in der gleichen Anwendung. Sie klicken Sie dann die Teile zu identifizieren und untersuchen Sie die so erzeugte Lösung, und schließlich sehen Sie die Website in Aktion.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Aufgabe 1 – Erstellen eines neuen Standorts mit der eine Erfahrung mit ASP.NET

In dieser Aufgabe Sie starten eine neue Website erstellen, in Visual Studio auf der Grundlage der **One ASP.NET** Projekttyp. **Eine ASP.NET** vereinigt alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit zum kombinieren, und passen sie nach Bedarf. Klicken Sie dann, erkennen Sie die verschiedenen Komponenten der Web Forms, MVC und Web-API, die live nebeneinander in Ihrer Anwendung.

1. Open **Visual Studio Express 2013 für Web** , und wählen Sie **Datei | Neues Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **ASP.NET-Webanwendung** unter der **Visual c# | Web** Registerkarte, und stellen Sie sicher, dass **.NET Framework 4.5** ausgewählt ist. Nennen Sie das Projekt *MyHybridSite*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Neues ASP.NET Web Application-Projekt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Erstellen eines neuen Projekts für die ASP.NET-Webanwendung*
3. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage, und wählen die **MVC** und **Web-API-** Optionen. Stellen Sie außerdem sicher, dass die **Authentifizierung** Option wird festgelegt, um **einzelne Benutzerkonten**. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Erstellen eines neuen Projekts mit der Web Forms-Vorlage, einschließlich Web-API und MVC-Komponenten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Erstellen eines neuen Projekts mit der Web Forms-Vorlage, einschließlich Web-API und MVC-Komponenten*
4. Sie können jetzt die Struktur die so erzeugte Lösung erkunden.

    ![Untersuchen der Lösung generierten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Untersuchen der Lösung generierten*

    1. **Konto:** dieser Ordner enthält die Web Form-Seiten, um zu registrieren, melden Sie sich bei und Verwalten von Benutzerkonten für der Anwendung. In diesem Ordner hinzugefügt, wenn die **einzelne Benutzerkonten** Authentifizierungsoption während der Konfiguration der Web Forms-Projektvorlage ausgewählt ist.
    2. **Modelle:** dieser Ordner enthält die Klassen, die Ihre Anwendungsdaten darstellen.
    3. **Controller** und **Ansichten**: Diese Ordner sind erforderlich, damit die **ASP.NET MVC** und **ASP.NET Web-API** Komponenten. Untersuchen Sie die MVC und Web-API-Technologien in den nächsten Übungen.
    4. Die **"default.aspx"**, **Contact.aspx** und **About.aspx** Dateien sind vorab definierte Web Form-Seiten, die Sie als Ausgangspunkt verwenden können, erstellen Sie die Seiten, die spezifisch für Ihre die Anwendung. Die Programmierlogik dieser Dateien befinden sich in einer separaten Datei genannt die &quot;CodeBehind&quot; gemäß der ein &quot;. "aspx.vb"&quot; oder &quot;. aspx.cs&quot; Erweiterung (je die verwendete Sprache). Die Code-Behind-Logik auf dem Server ausgeführt wird und HTML-Ausgabe für Ihre Seite dynamisch erzeugt.
    5. Die **Site.Master** und **Site.Mobile.Master** Seiten definieren das Aussehen und Verhalten und das Standardverhalten aller Seiten in der Anwendung.
5. Doppelklicken Sie auf die **"default.aspx"** Datei, um den Inhalt der Seite zu untersuchen.

    ![Untersuchen die Seite "default.aspx"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Untersuchen die Seite "default.aspx"*

    > [!NOTE]
    > Die **Seite** Direktive am Anfang der Datei definiert die Attribute der Web Forms-Seite. Z. B. die **MasterPageFile** -Attribut gibt den Pfad zum Master Seite – in diesem Fall die *Site.Master* Seiten- und **Inherits** -Attribut definiert die Code-Behind-Klasse für die Seite erbt. Diese Klasse befindet sich in der Datei, die bestimmt, indem die **CodeBehind** Attribut.
    > 
    > Die **Asp: Content** Steuerelement enthält den eigentlichen Inhalt der Seite (Text, Markup und Steuerelemente) und wird ein **Asp: ContentPlaceHolder** Steuerelement auf der Masterseite. In diesem Fall wird der Inhalt der Seite gerendert werden, in der *MainContent* -Steuerelements in definiert die *Site.Master* Seite.
6. Erweitern Sie die **App\_starten** Ordner, und beachten Sie, dass die **WebApiConfig.cs** Datei. Visual Studio enthalten die Datei in der generierten Projektmappe, da Sie beim Konfigurieren von Ihrem Projekts mit der Vorlage für eine ASP.NET Web-API enthalten.
7. Öffnen der **WebApiConfig.cs** Datei. In der *WebApiConfig* Klasse finden Sie die Konfiguration zugeordnet Web-API, die HTTP zugeordnet wird leitet **Web-API-Controllern**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Öffnen der **RouteConfig.cs** Datei. In der *RegisterRoutes* Methode finden Sie die Konfiguration zugeordnet MVC, das die HTTP-Route, ordnet **MVC-Controller**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Lösung

In dieser Aufgabe Sie führen die so erzeugte Lösung, Erkunden Sie die app und einige der Features, wie URL-Umschreibung und die integrierte Authentifizierung.

1. Um die Projektmappe auszuführen, drücken Sie die **F5** oder klicken Sie auf die **starten** Schaltfläche befindet sich auf der Symbolleiste. Startseite der Anwendung sollte im Browser geöffnet.

    ![Ausführen der Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Stellen Sie sicher, dass die Web Forms-Seiten aufgerufen werden. Fügen Sie zu diesem Zweck **/contact.aspx** an die URL in die Adressleiste ein und drücken Sie **EINGABETASTE**.

    ![Friendly URLs](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Friendly URLs*

    > [!NOTE]
    > Wie Sie sehen können, ändert die URL in **/wenden Sie sich an**. Beginnend mit **ASP.NET 4**, URL-routing-Funktionen wurden hinzugefügt, um die Web Forms, sodass Sie schreiben können wie URLs *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* anstelle von  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Weitere Informationen finden Sie unter [URL-Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Sie werden nun der Ablauf der Authentifizierung in die Anwendung integriert untersuchen. Zu diesem Zweck klicken Sie auf **registrieren** in der oberen rechten Ecke der Seite.

    ![Registrieren eines neuen Benutzers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrieren eines neuen Benutzers*
4. In der **registrieren** geben eine **Benutzernamen** und **Kennwort**, und klicken Sie dann auf **registrieren**.

    ![Seite "registrieren"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Seite "registrieren"*
5. Die Anwendung registriert das neue Konto ein, und der Benutzer wird authentifiziert.

    ![Benutzerauthentifizierung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Benutzerauthentifizierung*
6. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Übung 2: Erstellen eines MVC-Controllers mithilfe von Gerüstbau

In dieser Übung werden Sie nutzen von ASP.NET-Gerüstbau Framework bereitgestellt, die von Visual Studio zum Erstellen eines ASP.NET MVC 5-Controllers mit Aktionen und Razor-Ansichten für CRUD-Vorgänge ausführen, ohne eine einzige Codezeile schreiben zu müssen. Der Gerüstbau-Prozess wird Entity Framework Code First verwenden, um den Datenkontext und das Datenbankschema in der SQL-Datenbank zu generieren.

**Über Entitätsframework Code First**

Entity Framework (EF) ist ein Objektrelationaler Mapper (ORM), der Ihnen die Erstellung von Anwendungen für den Datenzugriff durch das Programmieren mit einem konzeptionellen Anwendungsmodell anstelle des Programmierens direkt mit einem Speicherschema ermöglicht.

Der Entity Framework Code First Modellierung Workflow können Sie Ihre eigenen Domänenklassen verwenden, um das Modell darstellen, dem EF auf stützt sich bei der Ausführung von Abfragen, Nachverfolgen von Änderungen und Aktualisieren von Funktionen. Verwenden den Code First Entwicklungsworkflow, müssen nicht Sie Ihre Anwendung durch Erstellen einer Datenbank oder das Angeben eines Schemas zu beginnen. Stattdessen können Sie den standardmäßige .NET-Klassen, die definieren, der am besten geeigneten domänenmodellobjekte für Ihre Anwendung schreiben, und Entity Framework erstellt die Datenbank für Sie.

> [!NOTE]
> Weitere Informationen finden Sie Informationen zu Entity Framework [hier](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Aufgabe 1 – Erstellen eines neuen Modells

Definieren Sie nun eine **Person** -Klasse, die das Modell, das durch den Gerüstbau-Prozess zum Erstellen von MVC-Controller und Ansichten verwendet werden. Beginnen Sie mit dem Erstellen einer **Person** Model-Klasse, und die CRUD-Vorgänge im Controller werden automatisch erstellt mithilfe von Gerüstbau-Funktionen.

1. Open **Visual Studio Express 2013 für Web** und **MyHybridSite.sln** Lösung befindet sich in der **Quelle/Ex2-MvcScaffolding/Anfang** Ordner. Alternativ können Sie die Lösung weiter nutzen, den Sie in der vorherigen Übung haben.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Modelle** Ordner die **MyHybridSite** Projekt, und wählen **hinzufügen | Klasse...** .

    ![Die Person-Klasse hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Die Person-Klasse hinzufügen*
3. In der **neues Element hinzufügen** Dialogfeld benennen Sie die Datei *Person.cs* , und klicken Sie auf **hinzufügen**.

    ![Erstellen die Person-Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Erstellen die Person-Klasse*
4. Ersetzen Sie den Inhalt der **Person.cs** -Datei mit den folgenden Code. Drücken Sie **STRG + S** um die Änderungen zu speichern.

    (Codeausschnitt - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. In **Projektmappen-Explorer**, mit der rechten Maustaste die **MyHybridSite** Projekt, und wählen **erstellen**, oder drücken Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Aufgabe 2: Erstellen eines MVC-Controllers

Nachdem die **Person** Modell erstellt wurde, verwenden Sie ASP.NET-MVC-Gerüstbau mit Entity Framework zum Erstellen des CRUD-Controller-Aktionen und Ansichten für **Person**.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner die **MyHybridSite** Projekt, und wählen **hinzufügen | Neues Gerüstelement...** .

    ![Erstellen eines neuen erstellten Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Erstellen eines neuen Gerüst-Controllers*
2. In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** , und klicken Sie dann auf **hinzufügen.**

    ![MVC 5-Controller mit Ansichten und Entity Framework auswählen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *MVC 5-Controller mit Ansichten und Entity Framework auswählen*
3. Legen Sie *MvcPersonController* als die **Controllername**, wählen die **Async-Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  als die **Modellklasse**.

    ![Einen MVC-Controller mit Gerüst hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Einen MVC-Controller mit Gerüst hinzufügen*
4. Klicken Sie unter **Datenkontextklasse**, klicken Sie auf **neuer Datenkontext...** .

    ![Erstellen einen neuen Datenkontext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Erstellen einen neuen Datenkontext*
5. In der **neuer Datenkontext** Dialogfeld den neuen Datenkontext *PersonContext* , und klicken Sie auf **hinzufügen**.

    ![Erstellen die neue PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Erstellen den neuen PersonContext-Typ*
6. Klicken Sie auf **hinzufügen** zum Erstellen des neuen Controllers für **Person** mit Gerüstbau. Visual Studio generiert die Controlleraktionen, die Person den Datenkontext und der Razor-Ansichten.

    ![Nach dem Erstellen von MVC-Controller mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Nach dem Erstellen von MVC-Controller mit Gerüstbau*
7. Öffnen der **MvcPersonController.cs** Datei die **Controller** Ordner. Beachten Sie, dass die CRUD-Aktionsmethoden automatisch generiert wurden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Durch Auswahl der **Async-Controlleraktionen verwenden** Kontrollkästchen aus dem integrierten Gerüstbau "Optionen" in den vorherigen Schritten, Visual Studio generiert asynchrone Aktionsmethoden für alle Aktionen, die Zugriff auf den Kontext der Person-Daten betreffen. Es wird empfohlen, dass Sie asynchrone Aktionsmethoden für lang andauernde verwenden, nicht CPU-gebundene Anforderungen, um zu vermeiden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3: Ausführen der Lösung

In dieser Aufgabe führen Sie der Lösung erneut aus, um zu überprüfen, ob die Ansichten für **Person** wie erwartet funktionieren. Fügen Sie eine neue Person aus, um sicherzustellen, dass sie erfolgreich in der Datenbank gespeichert ist.

1. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Navigieren Sie zu **/MvcPerson**. Die erstellte Ansicht, die die Liste der Personen enthält, sollte angezeigt werden.
3. Klicken Sie auf **neu erstellen** eine neue Person hinzufügen.

    ![Navigieren in den erstellten MVC-Ansichten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navigieren in den erstellten MVC-Ansichten*
4. In der **erstellen** anzuzeigen, geben Sie einen **Namen** und **Alter** für die Person, und klicken Sie auf **erstellen**.

    ![Eine neue Person hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Eine neue Person hinzufügen*
5. Die Liste wird die neue Person hinzugefügt. Klicken Sie in der Liste der Elemente, auf **Details** der Person-Detailansicht angezeigt. Klicken Sie auf die **Details** anzuzeigen, klicken Sie auf **zurück zur Liste** um zurück zur Listenansicht zu wechseln.

    ![Person Details anzeigen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Person Details anzeigen*
6. Klicken Sie auf die **löschen** Link zu die Person zu löschen. In der **löschen** anzuzeigen, klicken Sie auf **löschen** zum Bestätigen des Vorgangs.

    ![Löschen eine person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Löschen eine person*
7. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Übung 3: Erstellen einer Web-API-Controller mithilfe von Gerüstbau

Die Web-API-Framework ist Teil der ASP.NET-Stapel und um Implementieren von HTTP-Dienste in der Regel senden und Empfangen von JSON oder XML-formatierte Daten über eine RESTful-API zu vereinfachen, soll.

In dieser Übung werden Sie ASP.NET-Gerüstbau erneut verwenden, um einen Web-API-Controller zu generieren. Verwenden Sie das gleiche **Person** und **PersonContext** Klassen aus der vorherigen Übung die gleiche Person-Daten im JSON-Format bereitstellen. Es wird angezeigt, wie Sie die gleichen Ressourcen auf unterschiedliche Weise in der gleichen ASP.NET-Anwendung verfügbar machen können.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Aufgabe 1 – Erstellen einer Web-API-Controller

In dieser Aufgabe erstellen Sie ein neues **Web API Controller** macht, die die Person-Daten in einem Computer-anwendbares Format wie JSON verfügbar.

1. Wenn nicht bereits geöffnet ist, öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **MyHybridSite.sln** Lösung befindet sich in der **Quelle/Ex3-Web-API/Anfang** Ordner. Alternativ können Sie die Lösung weiter nutzen, den Sie in der vorherigen Übung haben.

    > [!NOTE]
    > Wenn Sie mit der Begin-Lösung aus Übung 3 starten, drücken Sie die **STRG + UMSCHALT + B** zum Erstellen der Projektmappe.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner die **MyHybridSite** Projekt, und wählen **hinzufügen | Neues Gerüstelement...** .

    ![Erstellen eines neuen erstellten Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Erstellen eines neuen erstellten Controllers*
3. In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Web-API-** im linken Bereich, klicken Sie dann **Web API 2-Controller mit Aktionen unter Verwendung von Entity Framework** im mittleren Bereich, und klicken Sie dann auf  **Fügen Sie hinzu.**

    ![Auswählen der Web-API 2-Controller mit Aktionen und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "auswählen von Web-API 2-Controller mit Aktionen und Entity Framework")

    *Auswählen der Web-API 2-Controller mit Aktionen und Entity Framework*
4. Legen Sie *ApiPersonController* als die **Controllername**, wählen die **Async-Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  und **PersonContext (MyHybridSite.Models)** als die **Modell** und **Datenkontext** bzw. Klassen. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen einer Web-API-Controller mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "einen Web-API-Controller mit Gerüst hinzufügen")

    *Einen Web-API-Controller mit Gerüst hinzufügen*
5. Visual Studio generiert die **ApiPersonController** Klasse mit die vier CRUD-Aktionen, die mit Ihren Daten arbeiten.

    ![Nach dem Erstellen des Web-API-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "nach dem Erstellen des Web-API-Controllers mit Gerüstbau")

    *Nach dem Erstellen des Web-API-Controllers mit Gerüstbau*
6. Öffnen der **ApiPersonController.cs** Datei, und überprüfen Sie die *"GetPeople"* Aktionsmethode. Diese Methode fragt die Db-Feld der **PersonContext** Typ, um die Benutzer Daten abzurufen.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Sehen Sie im Kommentar über der Definition der Methode an. Es enthält den URI, der diese Aktion verfügbar macht, die Sie in der nächsten Aufgabe verwendet werden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Standardmäßig Web-API ist so konfiguriert, dass die Abfragen zum Abfangen der *"/ API"* Pfad, um Konflikte mit MVC-Controller zu vermeiden. Wenn Sie diese Einstellung ändern müssen, finden Sie unter [Routing in ASP.NET Web-API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Lösung

In dieser Aufgabe verwenden Sie den Internet Explorer **F12-Entwicklertools** , überprüfen die vollständige Antwort aus dem Web-API-Controller. Es wird angezeigt, wie Sie Netzwerkdatenverkehr rufen Sie einen besseren Einblick in Ihre Anwendungsdaten erfassen können.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** ausgewählt ist, der **starten** Schaltfläche befindet sich auf der Symbolleiste von Visual Studio.
> 
> ![Internet Explorer-option](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Die **F12-Entwicklertools** haben Sie eine Breite Palette von Funktionen, die in dieser praktischen Übung nicht behandelt wird. Wenn Sie mehr darüber erfahren möchten, lesen Sie [mit den F12-Entwicklertools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Drücken Sie **F5** um die Projektmappe auszuführen.

    > [!NOTE]
    > Um diese Aufgabe genau befolgen zu können, muss Ihre Anwendung über Daten verfügen. Wenn Ihre Datenbank leer ist, können Sie zurück zur Aufgabe 3 in Übung 2, und führen Sie die Schritte zum Erstellen einer neuen Person, die mithilfe der MVC-Ansichten.
2. Drücken Sie im Browser **F12** zum Öffnen der **Entwicklertools** Bereich. Drücken Sie **STRG** + **4** oder klicken Sie auf die **Netzwerk** Symbol, und klicken Sie dann auf der grüne Pfeil-Schaltfläche, beginnt die Erfassung von Netzwerkdatenverkehr.

    ![Initiieren die netzwerkerfassung für Web-API-](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "netzwerkerfassung initiieren, Web-API")

    *Initiieren die netzwerkerfassung für Web-API*
3. Fügen Sie **-api/ApiPerson** an die URL in die Adressleiste des Browsers. Sie werden nun überprüfen Sie die Details der Antwort von der **ApiPersonController**.

    ![Abrufen von Personendaten über Web-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Abrufen von Personendaten über Web-API")

    *Abrufen von Personendaten über Web-API*

    > [!NOTE]
    > Sobald der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei herzustellen. Lassen Sie das Dialogfeld geöffnet, um den Inhalt der Antwort über das Entwickler-Fenster überwachen können.
4. Sie werden jetzt der Text der Antwort überprüfen. Zu diesem Zweck klicken Sie auf die **Details** Registerkarte, und klicken Sie dann auf **Antworttext**. Sehen Sie sich, dass die heruntergeladenen Daten eine Liste von Objekten mit den Eigenschaften **Id**, **Namen** und **Alter** , entsprechen die **Person** -Klasse.

    ![Anzeigen von Web-API-Antworttext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Anzeigen von Web-API-Antwort-Text")

    *Anzeigen von Web-API-Antworttext*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Aufgabe 3: Hinzufügen von Web-API-Hilfeseiten

Wenn Sie eine Web-API erstellen, ist es hilfreich sein, eine Seite erstellen, damit andere Entwickler zum Aufrufen Ihrer API veranschaulicht werden. Sie erstellen und die Dokumentationsseiten manuell aktualisieren, jedoch ist es besser, die automatisch, um zu vermeiden, dass Sie Wartungsarbeiten generieren. In dieser Aufgabe verwenden Sie ein Nuget-Paket zum automatischen Generieren von Web-API-Hilfeseiten mit der Lösung.

1. Von der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **-Paket-Manager-Konsole**.
2. In der **-Paket-Manager-Konsole** Fenster den folgenden Befehl ausführen:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Die **Microsoft.AspNet.WebApi.HelpPage** Paket installiert die erforderlichen Assemblys und fügt Sie MVC-Ansichten für die Hilfeseiten unter der **Bereiche/HelpPage** Ordner.

    ![HelpPage Bereich](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage-Bereich")

    *HelpPage-Bereich*
3. Standardmäßig werden in der Hilfe Seiten sind Platzhalter-Zeichenfolgen für die Dokumentation. Sie können XML-Dokumentationskommentare verwenden, um die Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen die **HelpPageConfig.cs** Datei befindet sich in der **Bereiche/HelpPage/App\_starten** Ordner und kommentieren Sie die folgende Zeile:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. In **Projektmappen-Explorer**, mit der rechten Maustaste in des Projekts **MyHybridSite**Option **Eigenschaften** , und klicken Sie auf die **erstellen** Registerkarte.

    ![Registerkarte "erstellen"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Abschnitt erstellen")

    *Registerkarte "erstellen"*
5. Klicken Sie unter **Ausgabe**Option **XML-Dokumentationsdatei**. Geben Sie in das Bearbeitungsfeld **App\_Data/XmlDocument.xml**.

    ![Ausgabe in der Registerkarte "Build" im Abschnitt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Ausgabe Abschnitt auf der Registerkarte Build ")

    *Ausgabeabschnitt in der Registerkarte "Build"*
6. Drücken Sie **STRG** + **S** um die Änderungen zu speichern.
7. Öffnen der **ApiPersonController.cs** -Datei aus dem **Controller** Ordner.
8. Geben Sie eine neue Zeile zwischen den *"GetPeople"* Methodensignatur und */ / GET-api/ApiPerson* kommentieren, und geben Sie dann drei Schrägstriche.

    > [!NOTE]
    > Visual Studio fügt automatisch die XML-Elemente, die die Dokumentation der Methode zu definieren.
9. Fügen Sie eine Zusammenfassung Text und der Rückgabewert für die *"GetPeople"* Methode. Es sollte wie folgt aussehen.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Drücken Sie **F5** um die Projektmappe auszuführen.
11. Fügen Sie **/help** an die URL in der Adressleiste, um die Hilfeseite zu suchen.

    ![ASP.NET Web-API-Hilfeseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web-API-Hilfeseite")

    *ASP.NET Web-API-Hilfeseite*

    > [!NOTE]
    > Der Hauptinhalt der Seite ist eine Tabelle mit APIs, die vom Netzwerkcontroller gruppiert. Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle. Wenn Sie hinzufügen oder aktualisieren einen API-Controller, wird in der Tabelle automatisch das nächste Mal aktualisiert werden, die, das Sie die Anwendung zu erstellen.
    > 
    > Die **API** Spalte enthält den HTTP-Methode und des relativen URI. Die **Beschreibung** Spalte enthält Informationen, die in der Dokumentation der Methode extrahiert wurde.
12. Beachten Sie, dass die Beschreibung, die Sie über die Definition der Methode hinzugefügt, die in der Spalte "Beschreibung" angezeigt wird.

    ![Beschreibung der API-](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Beschreibung der API-Methode")

    *Beschreibung der API-Methode*
13. Klicken Sie auf die API-Methoden, auf eine Seite mit ausführlicheren Informationen, einschließlich von beispielantworttexte zu navigieren.

    ![Seite mit Detailinformationen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detailinformationen zur Seite")

    *Seite "Informationen"*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Nach Abschluss dieser praktischen Übungseinheit Sie haben gelernt, wie Sie:

- Erstellen einer neuen Webanwendung, die mit der eine Erfahrung mit ASP.NET in Visual Studio 2013
- Integrieren Sie mehrere ASP.NET-Technologien in einem einzelnen Projekt
- Generieren Sie aus Ihren Modellklassen, die mithilfe von Gerüstbau für ASP.NET MVC-Controllern und Ansichten
- Generieren von Web-API-Controller, die Features wie z. B. für die asynchrone Programmierung und den Datenzugriff über Entity Framework verwenden
- Web-API-Hilfeseiten für Ihre Controller automatisch zu generieren
