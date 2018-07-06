---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 – Grundlagen | Microsoft-Dokumentation
author: rick-anderson
description: Diese praktische Übungseinheit wird MVC (Model View Controller) Music Store, eine lernprogrammanwendung basiert auf eingeführt und erläutert Schritt für Schritt, wie ASP.NET MV verwenden...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 2b3f8916bdca1df0dd2855f02ae46f5e5d13311a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819970"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 – Grundlagen

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

Diese praktische Übungseinheit wird MVC (Model View Controller) Music Store, eine lernprogrammanwendung basiert auf eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio verwenden. Erfahren Sie in den Laboren die Einfachheit halber noch Power diese Technologien zusammen verwendet. Sie werden mit einer einfachen Anwendung gestartet und werden es erstellt, bis Sie über eine voll funktionsfähige ASP.NET MVC 4-Webanwendung verfügen.

Dieses Lab funktioniert mit ASP.NET MVC 4.

Wenn Sie die ASP.NET MVC 3-Version der Anwendung Tutorial erkunden möchten, finden Sie in [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).

Diese praktische Übungseinheit wird davon ausgegangen, dass der Entwickler Erfahrungen in der Entwicklung von webtechnologien wie HTML und JavaScript.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [Grundlagen von ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Die Music Store-Anwendung

Die Music Store-Anwendung, die in dieser Übung erstellt wird, umfasst drei Hauptkomponenten: Warenkorb, Auschecken und Verwaltung. Besucher werden Alben nach Genre durchsuchen, Alben zu Einkaufswagen hinzufügen, überprüfen ihre Auswahl und schließlich fortfahren, lesen Sie die Anmeldung und vervollständigen Sie die Bestellung. Darüber hinaus werden Store-Administratoren verfügbaren Alben sowie ihre Haupteigenschaften verwalten können.

![Music Store-Bildschirme](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store Bildschirme")

*Music Store-Bildschirme*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Music Store-Anwendung erstellt werden mit **Model View Controller (MVC)**, ein Architekturmuster, das trennt eine Anwendung in drei Hauptkomponenten:

- **Modelle**: Modellobjekte sind die Teile der Anwendung, die die Domänenlogik implementieren. Model-Objekte wird häufig auch abrufen, und der Modellzustand in einer Datenbank speichern.
- **Ansichten:** Ansichten sind die Komponenten, die der Anwendung die Benutzeroberfläche (UI) an. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre der Bearbeitungsansicht von Alben, die Textfelder und eine Dropdown-Liste basierend auf den aktuellen Status eines Objekts Album anzeigt.
- **Domänencontroller:** Controller sind Komponenten, die Benutzerinteraktionen verarbeiten, bearbeiten das Modell und letztlich eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs auswählen. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.

Das MVC-Muster unterstützt Sie beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und UI-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen zu trennen. Diese Trennung ermöglicht Ihnen das bewältigen von Komplexität beim Erstellen einer Anwendung, da Sie sich auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können. Darüber hinaus erleichtert das MVC-Muster zum Testen von Anwendungen, da Sie auch die Verwendung der testgesteuerten Entwicklung (TDD) zum Erstellen von Anwendungen.

Die **ASP.NET MVC** Framework stellt eine Alternative für das ASP.NET Web Forms-Muster zum Erstellen von Webanwendungen mit ASP.NET MVC-basierten. Die **ASP.NET MVC** Framework ist eine einfache und leicht zu testendes Präsentationsframework, (wie bei Web Forms-basierte Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und mitgliedschaftsbasierte integriert Authentifizierung, um die Leistungsfähigkeit von .NET Core Framework zu erhalten. Dies ist nützlich, wenn Sie bereits mit ASP.NET Web Forms vertraut sind, da alle Bibliotheken, mit denen Sie auch in ASP.NET MVC 4 verfügbar sind.

Darüber hinaus wird die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung auch parallele Entwicklung. Z. B. ein Entwickler kann die auf die Ansicht, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen Sie eine ASP.NET MVC-Anwendung von Grund auf neu, die basierend auf dem Lernprogramm für die Music Store-Anwendung
- Hinzufügen von Controllern zum Behandeln von URLs zur Homepage der Website und seine wichtigsten Funktionen durchsuchen
- Hinzufügen einer Ansicht, um den Inhalt, der angezeigt wird, zusammen mit den Stil anzupassen
- Hinzufügen von Modellklassen um enthalten und Verwalten von Daten und Domänenlogik
- Verwenden Sie View Model-Muster, um Informationen aus Controlleraktionen an die Ansichtsvorlagen zu übergeben.
- Erkunden der neuen ASP.NET MVC 4-Vorlage von Internetanwendungen

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:

1. [Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt Music Store](#Exercise1)
2. [Übung 2: Erstellen eines Controllers](#Exercise2)
3. [Übung 3: Übergeben von Parametern an einen Controller](#Exercise3)
4. [Übung 4: Erstellen einer Ansicht](#Exercise4)
5. [Schritt 5: Erstellen von Anzeigemodellen](#Exercise5)
6. [Übung 6: Verwenden von Parametern in der Ansicht](#Exercise6)
7. [Übung 7: Eine Tour durch neue ASP.NET MVC 4-Vorlage](#Exercise7)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt Music Store

In dieser Übung lernen Sie, wie Sie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für Web als auch der Ordner "main"-Organisation zu erstellen. Darüber hinaus erfahren Sie, wie zum Hinzufügen eines neuen Controllers, und stellen sie eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Aufgabe 1: Erstellen das ASP.NET MVC-Webanwendungsprojekt

1. In dieser Aufgabe erstellen Sie eine leere ASP.NET MVC-Anwendungsprojekt mit der Visual Studio für MVC-Vorlage. Starten Sie **Visual Studio Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. In der **neues Projekt** aktivieren Sie im Dialogfeld die **ASP.NET MVC 4-Webanwendung** Projekttyp, befindet sich im **Visual c#** **Web** Vorlage Liste.
4. Ändern der **Namen** zu *MvcMusicStore*.
5. Legen Sie den Speicherort der Lösung in einem neuen **beginnen** im Ordner "in dieser Übung die Quelle", z. B. **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. Klicken Sie auf **OK**.

    ![Dialogfeld Neues Projekt erstellen](aspnet-mvc-4-fundamentals/_static/image2.png "Dialogfeld Neues Projekt erstellen")

    *Dialogfeld Neues Projekt erstellen*
6. In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **grundlegende** Vorlage und stellen Sie sicher, dass die **Ansichts-Engine** ausgewählt **Razor**. Klicken Sie auf **OK**.

    ![Dialogfeld Neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image3.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")

    *Neues ASP.NET MVC 4-Projekt (Dialogfeld)*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Aufgabe 2: untersuchen die Lösungsstruktur

ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, die Ihnen das Erstellen von Webanwendungen unterstützen das MVC-Muster hilft. Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung, mit der erforderlichen Ordnern, Elementvorlagen und Konfigurationsdatei Einträge.

In dieser Aufgabe untersuchen Sie die Projektmappenstruktur, die Elemente zu verstehen, die beteiligt sind und deren Beziehungen. Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig verwendet eine &quot;Konvention geht vor Konfiguration&quot; Ansatz und legt einige Annahmen basiert, für die Benennung von Ordner Konventionen.

1. Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die im Projektmappen-Explorer auf der rechten Seite erstellt wurde:

    ![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")

    *ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*

   1. **Controller**. Dieser Ordner enthält die Controllerklassen. In einer MVC-basierten Anwendung dienen Controller Verarbeiten von End-Benutzerinteraktionen, das Modell bearbeiten und auswählen letztlich eine Ansicht für die Benutzeroberfläche zu rendern.

       > [!NOTE]
       > Das MVC-Framework erfordert die Namen aller Controller endet nicht mit &quot;Controller&quot;– z. B. HomeController, LoginController oder ProductController.
   2. **Modelle**. Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen. Dies schließt in der Regel Code, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert. In der Regel werden die eigentlichen Modellobjekte in separaten Klassenbibliotheken. Wenn Sie eine neue Anwendung erstellen, können Sie jedoch umfassen Klassen und Sie dann im Entwicklungszyklus in separate Klassenbibliotheken zu einem späteren Zeitpunkt verschieben.
   3. **Ansichten**. Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich. Ansichten verwenden aspx, ASCX, .cshtml und Master-Dateien zusätzlich zu anderen Dateien, die zum Rendern von Ansichten verknüpft werden. Ordner "Views" enthält einen Ordner für jeden Controller. der Ordner den Namen mit dem Controller-Namenspräfix. Wenn Sie einen Controller mit dem Namen haben z. B. **HomeController**, den Ordner "Views" enthält einen Ordner mit der Bezeichnung Home. In der Standardeinstellung beim Laden von ASP.NET MVC-Framework einer Ansicht, gesucht, der eine ASPX-Datei mit dem angeforderten Namen im Ordner "Views\controllerName" (**Ansichten [Namedescontrollers] [Action] aspx**) oder (**Ansichten [Namedescontrollers] [Aktion] .cshtml**) für Razor-Ansichten.

      > [!NOTE]
      > Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Web-Anwendung die **"Global.asax"** standardmäßig hinzu, um globale URL-routing einzurichten, und es verwendet die **"Web.config"** Datei zum Konfigurieren der Anwendung.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Aufgabe 3: hinzufügen einen HomeController

In ASP.NET-Anwendungen, die das MVC-Framework nicht verwenden, wird in eine Benutzerinteraktion nach Seiten und Auslösen und Behandeln von Ereignissen aus diesen Seiten angeordnet werden. Im Gegensatz dazu ist Benutzerinteraktion in ASP.NET-MVC-Anwendungen in Controllern und ihre Aktionsmethoden organisiert.

ASP.NET MVC-Framework ordnet URLs auf der anderen Seite zu Klassen, die als Domänencontroller bezeichnet werden. Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen Sie die zugehörige Anwendungslogik und die Antwort zurück an den Client senden zu bestimmen (HTML-Seite anzeigen, eine Datei herunterzuladen, Umleitung an eine andere URL usw.). Im Fall von HTML anzeigen, ruft eine Controller-Klasse in der Regel eine separate Ansichtskomponente, um das HTML-Markup für die Anforderung zu generieren. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.

In dieser Aufgabe fügen Sie eine Controller-Klasse, die URLs, die auf der Startseite der Music Store-Website behandelt.

1. Mit der rechten Maustaste **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann **Controller** Befehl:

    ![Fügen Sie einen Controllerbefehl](aspnet-mvc-4-fundamentals/_static/image5.png "fügen einen Controllerbefehl hinzu")

    *Controllerbefehl "hinzufügen"*
2. Die **Controller hinzufügen** Dialogfeld wird angezeigt. Nennen Sie den Controller *HomeController* , und drücken Sie **hinzufügen**.

    ![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image6.png "Controller-Dialogfeld \"hinzufügen\"")

    *Controller-Dialogfeld "hinzufügen"*
3. Die Datei **"HomeController.cs"** wird erstellt, der **Controller** Ordner. Damit die **HomeController** geben eine Zeichenfolge zurück, auf die Index-Aktion, ersetzen Sie die **Index** Methode durch den folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex1 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe werden Sie die Anwendung in einem Webbrowser testen.

1. Drücken Sie **F5** zum Ausführen der Anwendung. Das Projekt kompiliert wird, und die lokale IIS-Webserver gestartet wird. Die lokale IIS-Webserver wird einen Webbrowser zur URL des Webservers automatisch geöffnet.

    ![In einem Webbrowser ausgeführte Anwendung](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung, die in einem Webbrowser ausgeführt wird")

    *Anwendung, die in einem Webbrowser ausgeführt wird*

    > [!NOTE]
    > Lokale IIS-Webservers wird die Website auf eine zufällige freie Portnummer ausgeführt werden. In der obigen Abbildung die Website ausgeführt wird, am `http://localhost:50103/`, sodass sie Port 50103 verwendet wird. Ihre Portnummer kann variieren.
2. Schließen Sie den Browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Übung 2: Erstellen eines Controllers

In dieser Übung lernen Sie, wie beim Aktualisieren des Controllers zur einfachen Funktionalität die Music Store-Anwendung zu implementieren. Dieser Controller definieren Aktionsmethoden anwenden, um die folgenden spezifischen Anforderungen zu verarbeiten:

- Eine Angebotsseite des Genres Musik in die Music Store
- Seite "Durchsuchen", die alle die Alben für eine bestimmte "Genre" aufgeführt werden
- Eine Seite, die Informationen zu einem bestimmten Musikalbum anzeigt

Für den Bereich in dieser Übung gibt diese Aktionen jetzt einfach eine Zeichenfolge zurück.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse

In dieser Aufgabe fügen Sie einen neuen Controller hinzu.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web 2012**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex02-CreatingAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Fügen Sie dem neuen Controller hinzu. Dazu, mit der Maustaste der **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und klicken Sie dann die **Controller** Befehl. Ändern der **Controllername** zu *StoreController*, und klicken Sie auf **hinzufügen**.

    ![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image8.png "Controller-Dialogfeld \"hinzufügen\"")

    *Controller-Dialogfeld "hinzufügen"*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Aufgabe 2: Ändern der StoreController Aktionen

In dieser Aufgabe ändern Sie die Controller-Methoden, die aufgerufen werden **Aktionen**. Aktionen sind verantwortlich für die Verarbeitung von URL-Anforderungen, und bestimmen den Inhalt, der zurück an den Browser oder die Benutzer, die die URL aufgerufen gesendet werden soll.

1. Die **StoreController** Klasse verfügt bereits über ein **Index** Methode. Sie verwenden es später in diesem Kurs auf um die Seite zu implementieren, die alle Genres im Music Store auflistet. Ersetzen Sie vorerst einfach die **Index** Methode durch den folgenden Code, der eine Zeichenfolge zurückgibt, &quot;Grüße aus Store.Index()&quot;:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Hinzufügen **Durchsuchen** und **Details** Methoden. Zu diesem Zweck fügen Sie den folgenden Code der **StoreController**:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe werden Sie die Anwendung in einem Webbrowser testen.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt startet der **Startseite** Seite. Ändern Sie die URL, um die Implementierung der einzelnen Aktionen zu überprüfen.

    1. **/ Store**. Sehen Sie  **&quot;Grüße aus Store.Index()&quot;**.
    2. **/ Store/durchsuchen**. Sehen Sie  **&quot;Grüße aus Store.Browse()&quot;**.
    3. **/ Store/Details**. Sehen Sie  **&quot;Grüße aus Store.Details()&quot;**.

        ![Durchsuchen von StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse durchsuchen")

        */Store/Browse durchsuchen*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Übung 3: Übergeben von Parametern an einen Controller

Bis jetzt haben Sie durch den Controller, Konstante Zeichenfolgen zurück, wurde. In dieser Übung lernen Sie, wie zum Übergeben von Parametern an einen Controller mithilfe der URL und Abfragezeichenfolge, und nehmen Sie dann die methodenaktionen, die mit dem Text an den Browser zu reagieren.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Aufgabe 1: Hinzufügen von "Genre"-Parameter, um StoreController

In dieser Aufgabe verwenden Sie die **Querystring** Senden von Parametern, die **Durchsuchen** Aktionsmethode in der **StoreController**.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex03-PassingParametersToAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Open **StoreController** Klasse. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
4. Ändern der **Durchsuchen** Methode, die einen Zeichenfolgenparameter zum Anfordern von für eine spezifische Genre hinzufügen. ASP.NET MVC automatisch alle Abfragezeichenfolge übergeben oder FormPost-Parameter, die mit dem Namen **"Genre"** beim Aufrufen dieser Aktion-Methode. Ersetzen Sie hierzu die **Durchsuchen** Methode durch den folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Sie verwenden die **HttpUtility.HtmlEncode durchführen** Utility-Methode, um Benutzer daran gehindert Einfügen von Javascript in der Ansicht mit einem Link wie   **/Store/durchsuchen? Genre =&lt;Skript&gt;Window.Location = "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.
> 
> Weitere Informationen finden Sie unter [diesem Msdn-Artikel](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe Sie die Anwendung in einem Webbrowser testen und Verwenden der **"Genre"** Parameter.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt startet der **Startseite** Seite. Ändern Sie die URL zum   */Store/durchsuchen? Genre = Disco* um sicherzustellen, dass die Aktion den genreparameter empfängt.

    ![Durchsuchen von StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "durchsuchen StoreBrowseGenre = Disco")

    *Durchsuchen Sie /Store/Browse? Genre = Disco*
3. Schließen Sie den Browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Aufgabe 3: Hinzufügen eines Id-Parameters, die in der URL eingebettet

In dieser Aufgabe verwenden Sie die **URL** übergeben eine **Id** Parameter, um die **Details** Action-Methode der der **StoreController**. Routing-Konvention wird das Segment der URL nach dem Namen der Aktionsmethode behandelt, als Parameter mit dem Namen ASP.NETs-Standard **Id**. Wenn Ihrer Aktionsmethode Parameter Id verfügt, wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben. In der URL **Store/Details/5**, **Id** wird als interpretiert **5**.

1. Ändern der **Details** -Methode der der **StoreController**, Hinzufügen einer **Int** Parameter namens **Id**. Ersetzen Sie hierzu die **Details** Methode durch den folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe Sie die Anwendung in einem Webbrowser testen und Verwenden der **Id** Parameter.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt startet der **Startseite** Seite. Ändern Sie die URL zum */Store/Details/5* um sicherzustellen, dass die Aktion mit den Id-Parameter empfängt.

    ![Durchsuchen von StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 durchsuchen")

    */Store/Details/5 durchsuchen*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Übung 4: Erstellen einer Ansicht

Bisher haben Sie Zeichenfolgen aus Controlleraktionen zurückgegeben wurde. Obwohl dies ist eine gute Möglichkeit für das Verständnis der Funktionsweise von Controllern, ist es nicht, wie Ihre echten Webanwendungen erstellt werden. Ansichten sind Komponenten, die einen besseren Ansatz zum Generieren von HTML zurück an den Browser mit der Verwendung von Vorlagendateien bereitstellen.

In dieser Übung lernen Sie das Hinzufügen eine Masterseite "Layout" zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten der Website und schließlich eine ansichtsvorlage zu HomeController HTML zurückgeben zu verbessern.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Aufgabe 1: Ändern der Datei \_layout.cshtml

Die Datei **~/Views/Shared/\_layout.cshtml** können Sie eine Vorlage für gemeinsamer HTML-Code für die Verwendung in der gesamten Website einrichten. In dieser Aufgabe fügen Sie im Bereich auf der Startseite "und" Store eine Layout-Masterseite mit ein common-Header mit Links hinzu.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex04-CreatingAView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Die Datei  <strong>\_layout.cshtml</strong> enthält die HTML-Containerlayout für alle Seiten auf der Website. Es enthält die <strong>&lt;html&gt;</strong> -Element für die HTML-Antwort als auch die <strong>&lt;Head&gt;</strong> und <strong>&lt;Text&gt;</strong> Elemente. <strong>@RenderBody()</strong> im HTML-Code identifizieren Text Regionen diese Ansicht, die Vorlagen werden in der Lage, sich mit dynamischen Inhalten zu füllen.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Fügen Sie einen allgemeinen Header mit Links in den Bereich "Startseite" und "Store" auf allen Seiten der Website hinzu. Zu diesem Zweck fügen Sie den folgenden Code unter &lt;Text&gt; Anweisung.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Enthalten Sie ein DIV-Element um das Body-Bereich jeder Seite zu rendern. Ersetzen Sie dies  <strong>@RenderBody()</strong> durch den folgenden Code der Higlighted: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Wussten Sie schon? Visual Studio 2012 stellt Ausschnitte, die häufig verwendeten Code hinzufügen, in HTML, Codedateien und mehr erleichtern. Versuchen Sie es, indem Sie eingeben **&lt;Div&gt;** und **Registerkarte** zweimal, um eine vollständige einfügen **Div** Tag.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Aufgabe 2: Hinzufügen von CSS-Stylesheet

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zum Anzeigen von grundlegenden Formen und validierungsmeldungen enthält nur. Sie werden zusätzliche CSS- und -Images (die möglicherweise von einem Designer bereitgestellt werden) verwenden, um das Aussehen und Verhalten der Website zu verbessern.

In dieser Aufgabe fügen Sie eine CSS-Stylesheet an, um die Formatvorlagen der Site zu definieren.

1. Die CSS-Datei und die Bilder befinden sich der **Source\Assets\Content** Ordner dieser Anleitung. Um diese an die Anwendung hinzuzufügen, ziehen Sie in ihrer Inhalte mit einer **Windows Explorer** Einblick in die **Projektmappen-Explorer** in Visual Studio Express für Web, wie unten dargestellt:

    ![Style-Inhalte ziehen](aspnet-mvc-4-fundamentals/_static/image12.png "Style-Inhalte ziehen")

    *Ziehen die Style-Inhalt*
2. Eine Warnmeldung angezeigt, in der Sie gefragt, ersetzen Sie dies zu bestätigen **"Site.CSS"** Datei- und einige vorhandene Images. Überprüfen Sie **anwenden, um alle Elemente** , und klicken Sie auf **Ja**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Aufgabe 3: Hinzufügen einer Vorlage anzeigen

In dieser Aufgabe fügen Sie eine Vorlage anzeigen, um die HTML-Antwort zu generieren, die die Masterseite Layout verwendet werden, und CSS in dieser Übung hinzugefügt.

1. Um eine ansichtsvorlage beim Durchsuchen der Homepage der Website zu verwenden, müssen Sie zuerst an, dass anstelle einer Zeichenfolge, die **HomeController Index** Methode gibt zurück, eine **Ansicht**. Open **HomeController** Klasse, und ändern die **Index** -Methode zur Rückgabe einer **ActionResult**, und zurückliefern **View()**.

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex4 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Nun müssen Sie eine geeignete ansichtsvorlage hinzufügen. Zu diesem Zweck **mit der rechten Maustaste** innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird die **Ansicht hinzufügen** Dialogfeld.

    ![Hinzufügen einer Ansicht aus, in die Indexmethode](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Ansicht aus, in der Index-Methode")

    *Hinzufügen einer Ansicht aus, in der Index-Methode*
3. Die **Ansicht hinzufügen** Dialogfeld wird angezeigt, um eine ansichtsvorlagendatei zu generieren. Standardmäßig füllt dieses Dialogfeld den Namen der Vorlage anzeigen, damit diese die Aktionsmethode entspricht, die dazu verwendet werden. Da Sie verwendet die **Ansicht hinzufügen** Kontextmenü innerhalb der **Index** Action-Methode im HomeController, die **Ansicht hinzufügen** Dialogfeld hat den Index als den Standardnamen für die Ansicht. Klicken Sie auf **Hinzufügen**.

    ![Ansicht-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image14.png "anzeigen-Dialogfeld \"hinzufügen\"")

    *Ansicht-Dialogfeld "hinzufügen"*
4. Visual Studio generiert eine **"Index.cshtml"** ansichtsvorlage innerhalb der **Views\Home** Ordner und anschließend geöffnet.

    ![Startseite der Indexansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Startseite Indexansicht erstellt")

    *Home Indexansicht erstellt*

    > [!NOTE]
    > Name und Speicherort der der **"Index.cshtml"** Datei relevant ist und die standardmäßige ASP.NET-MVC-Namenskonventionen entspricht.
    > 
    > Der Ordner \Views\**Startseite** den Controllernamen entspricht (**Startseite** Controller). Namen der Ansicht (**Index**), entspricht der Aktionsmethode des Controllers der Ansicht anzeigen lassen.
    > 
    > Auf diese Weise wird ASP.NET MVC vermieden, müssen den Namen oder Speicherort, der eine ansichtsvorlage explizit angeben, wenn diese Benennungskonvention zu verwenden, um eine Ansicht zurückgegeben.
5. Die generierte ansichtsvorlage basiert auf der  **\_layout.cshtml** Vorlage, die zuvor definierten. Aktualisieren Sie die ViewBag.Title-Eigenschaft, um **Startseite**, und ändern Sie den Hauptinhalt **Dies ist die Startseite**, wie im folgenden Code gezeigt:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wählen Sie **MvcMusicStore** -Projekt in der Projektmappen-Explorer, und drücken Sie **F5** zum Ausführen der Anwendung.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Aufgabe 4: Überprüfung

Um sicherzustellen, dass Sie alle Schritte in der vorherigen Übung ordnungsgemäß ausgeführt haben, gehen Sie wie folgt aus:

Mit der Anwendung, die in einem Browser geöffnet wird sollten Sie Folgendes beachten:

1. Des HomeController Index-Aktionsmethode gefunden und angezeigt, die **\Views\Home\Index.cshtml** Vorlage anzeigen, auch wenn der Code aufgerufen **View() zurückgeben**, da die ansichtsvorlage gefolgt der standardmäßige Benennungskonvention zu verwenden.
2. Auf der Startseite zeigt die Willkommensnachricht in definiert die **\Views\Home\Index.cshtml** Vorlage anzeigen.
3. Auf der Startseite wird mithilfe der  **\_layout.cshtml** Vorlage, und daher die Willkommensnachricht in der Standardwebsite HTML-Layout enthalten ist.

    ![Indexansicht mit dem definierten LayoutPage und Stil Home](aspnet-mvc-4-fundamentals/_static/image16.png "Startseite Ansicht \"Index\" mit dem definierten LayoutPage und Stil")

    *Home Ansicht "Index" mit dem definierten LayoutPage und Stil*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Schritt 5: Erstellen von Anzeigemodellen

Bis jetzt vorgenommenen Ihre Ansichten, die hartcodierte HTML anzeigen, aber zum Erstellen dynamischer Webanwendungen die ansichtsvorlage erhalten Informationen über den Controller. Ein gängiges Verfahren, die für diesen Zweck verwendet werden wird die **"ViewModel"** Muster, das einen Domänencontroller, um alle Informationen, die zum Generieren der entsprechenden HTML-Antwort benötigt Verpacken kann.

In dieser Übung erfahren Sie, wie erstellen eine ViewModel-Klasse, und fügen Sie die erforderlichen Eigenschaften: die Anzahl von Genres in den Speicher und eine Liste mit diesen Genres. Aktualisieren Sie auch die StoreController zur Verwendung von "ViewModel" erstellt, und schließlich erstellen Sie eine neue Vorlage anzeigen, die die genannten Eigenschaften auf der Seite angezeigt werden.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Aufgabe 1: erstellen eine ViewModel-Klasse

In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, die das Store "Genre" Auflistung Szenario implementiert wird.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex05-CreatingAViewModel\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Erstellen Sie eine **ViewModels** Ordner für das "ViewModel". Dazu, mit der Maustaste der obersten Ebene **MvcMusicStore** -Projekt, wählen **hinzufügen** und dann **neuer Ordner**.

    ![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "einen neuen Ordner hinzufügen")

    *Hinzufügen eines neuen Ordners*
4. Nennen Sie den Ordner *ViewModels*.

    ![Ordner "ViewModels" im Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "Ordner \"ViewModels\" im Projektmappen-Explorer")

    *Ordner "ViewModels" im Projektmappen-Explorer*
5. Erstellen Sie eine **"ViewModel"** Klasse. Zu diesem Zweck mit der Maustaste auf die **ViewModels** Ordner vor kurzem erstellt haben, wählen Sie **hinzufügen** und dann **neues Element**. Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *StoreIndexViewModel.cs*, klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "eine neue Klasse hinzufügen")

    *Hinzufügen einer neuen Klasse*

    ![Erstellen von StoreIndexViewModel Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "erstellen StoreIndexViewModel-Klasse")

    *Erstellen von StoreIndexViewModel-Klasse*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Aufgabe 2: Hinzufügen von Eigenschaften, die ViewModel-Klasse

Es gibt zwei Parameter an die ansichtsvorlage aus der StoreController übergeben werden, um die erwartete HTML-Antwort zu generieren: die Anzahl von Genres in den Speicher und eine Liste mit diesen Genres.

In dieser Aufgabe fügen Sie dieser 2 Eigenschaften der **StoreIndexViewModel** Klasse: **NumberOfGenres** (eine ganze Zahl) und **Genres** (eine Liste von Zeichenfolgen).

1. Hinzufügen **NumberOfGenres** und **Genres** Eigenschaften, die die **StoreIndexViewModel** Klasse. Zu diesem Zweck fügen Sie die folgenden 2 Zeilen an die Klassendefinition hinzu:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreIndexViewModel Eigenschaften*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> Die **{get; festlegen;}**  Notation nutzt # Funktion für automatisch implementierte Eigenschaften. Es bietet die Vorteile einer Eigenschaft, ohne uns um ein dahinter liegendes Feld zu deklarieren.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Aufgabe 3: Aktualisieren StoreController der StoreIndexViewModel verwenden

Die **StoreIndexViewModel** -Klasse kapselt die erforderlichen Informationen zum Übergeben von **StoreController**des **Index** Methode, um eine Vorlage anzeigen, um eine Antwort zu generieren. .

In dieser Aufgabe aktualisieren Sie die **StoreController** verwenden die **StoreIndexViewModel**.

1. Open **StoreController** Klasse.

    ![Öffnen StoreController Klasse](aspnet-mvc-4-fundamentals/_static/image21.png "öffnen StoreController-Klasse")

    *Öffnende StoreController-Klasse*
2. Zum Verwenden der **StoreIndexViewModel** -Klasse aus der **StoreController**, den folgenden Namespace hinzufügen, am oberen Rand der **StoreController** Code:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreIndexViewModel Verwenden von ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Ändern der **StoreController**des **Index** Action-Methode so, dass die It erstellt und füllt eine **StoreIndexViewModel** Objekt aus, und leitet diese dann an eine ansichtsvorlage deaktivieren Generieren einer HTML-Antwort mit.

    > [!NOTE]
    > In Übungseinheit &quot;ASP.NET MVC-Modelle und Datenzugriff&quot; schreiben Sie Code, der die Liste der Store Genres aus einer Datenbank abruft. In den folgenden Code, erstellen Sie eine **Liste** Dummydaten Genres, das Auffüllen der **StoreIndexViewModel**.
    > 
    > Nach dem Erstellen und Einrichten der **StoreIndexViewModel** Objekt, übergeben sie als Argument an die **Ansicht** Methode. Dies gibt an, dass die ansichtsvorlage zum Generieren einer HTML-Antwort, damit dieses Objekt verwendet.
4. Ersetzen Sie die **Index** Methode durch den folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreController Indexmethode*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Wenn Sie nicht mit c# vertraut sind, Sie können davon ausgehen, dass die Verwendung **Var** bedeutet, dass die **"ViewModel"** Variable ist spät gebunden. Das ist nicht falsch – c#-Compiler verwendet den Typrückschluss basierend auf was Sie auf die Variable zuweisen ermitteln, ob **"ViewModel"** ist vom Typ **StoreIndexViewModel**. Darüber hinaus durch das Kompilieren des lokalen **"ViewModel"** -Variable als ein **StoreIndexViewModel** Geben Sie Get kompilierzeitüberprüfung und Visual Studio Code-Editor-Unterstützung.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Aufgabe 4: Erstellen einer Vorlage anzeigen, die StoreIndexViewModel verwendet

In dieser Aufgabe erstellen Sie eine Vorlage anzeigen, die ein StoreIndexViewModel-Objekt, das vom Controller übergebenen verwendet, um eine Liste der Genres anzuzeigen.

1. Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **anzeigen-Dialogfeld hinzufügen** kennt die **StoreIndexViewModel** Klasse. Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.

    ![Beim Erstellen des Projekts](aspnet-mvc-4-fundamentals/_static/image22.png "beim Erstellen des Projekts")

    *Beim Erstellen des Projekts*
2. Erstellen Sie eine neue ansichtsvorlage für die ein. Klicken Sie dazu auf, auf die **Index** Methode, und wählen **Ansicht hinzufügen**.

    ![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "Hinzufügen einer Ansicht")

    *Hinzufügen einer Ansicht*
3. Da die **anzeigen-Dialogfeld hinzufügen** aufgerufen wurde, aus der **StoreController**, fügen sie die ansichtsvorlage standardmäßig in eine **\Views\Store\Index.cshtml** Datei. Überprüfen Sie die **erstellen Sie eine stark typisierte-anzeigen** Kontrollkästchen und wählen Sie dann **StoreIndexViewModel** als die **Modellklasse**. Stellen Sie außerdem sicher, dass die Ansichts-Engine ausgewählt ist **Razor**. Klicken Sie auf **Hinzufügen**.

    ![Ansicht-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image24.png "anzeigen-Dialogfeld \"hinzufügen\"")

    *Ansicht-Dialogfeld "hinzufügen"*

    Die **\Views\Store\Index.cshtml** ansichtsvorlagendatei wird erstellt und geöffnet. Anhand der Informationen bereitgestellt, um die **Ansicht hinzufügen** Dialogfeld im letzten Schritt die Ansicht, die Vorlage werden erwarten, dass eine **StoreIndexViewModel** -Instanz wie die Daten zu verwenden, um eine HTML-Antwort zu generieren. Sie werden feststellen, dass die Vorlage übernimmt eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C# geschrieben.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Aufgabe 5: Aktualisieren der Vorlage anzeigen

In dieser Aufgabe aktualisieren Sie die Vorlage anzeigen, die in der vorhergehenden Aufgabe zum Abrufen der Anzahl von Genres und deren Namen auf der Seite erstellt.

> [!NOTE]
> Verwenden Sie @-Syntax (bezeichnet als &quot;Codeausdrücke&quot;) zum Ausführen von Code in der Vorlage anzeigen.

1. In der **"Index.cshtml"** -Datei, in der **Store** Ordner, ersetzen Sie den Code durch Folgendes:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Schleife über der Liste "Genre" in **StoreIndexViewModel** , und erstellen Sie eine HTML **&lt;Ul&gt;** auflisten mithilfe einer **Foreach** Schleife.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie **/Store**. Sehen Sie die Liste der Genres übergeben die **StoreIndexViewModel** -Objekt aus der **StoreController** der Vorlage anzeigen.

    ![Ansicht zum Anzeigen einer Liste der Genres](aspnet-mvc-4-fundamentals/_static/image26.png "Ansicht zum Anzeigen einer Liste der Genres")

    *Ansicht zum Anzeigen einer Liste der genres*
4. Schließen Sie den Browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Übung 6: Verwenden von Parametern in der Ansicht

In Übung 3 haben Sie gelernt, wie Parameter an den Controller übergeben werden. In dieser Übung lernen Sie, wie Sie diese Parameter in die ansichtsvorlage zu verwenden. Zu diesem Zweck werden Sie zuerst auf ViewModel-Klassen vorgestellt, mit denen Sie Ihre Daten und Domänenlogik zu verwalten. Darüber hinaus lernen Sie, wie Sie Links zu Seiten in ASP.NET MVC-Anwendung zu erstellen, ohne sich Gedanken machen Dinge wie URL-Pfade, die Codierung.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Aufgabe 1: Hinzufügen von Modellklassen

Im Gegensatz zu ViewModels, die erstellt werden, um Informationen vom Controller an die Ansicht zu übergeben, werden ViewModel-Klassen erstellt, um enthalten und Verwalten von Daten und Domänenlogik. In dieser Aufgabe fügen Sie zwei Modellklassen zur Darstellung dieser Konzepte: **"Genre"** und **Album**.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex06-UsingParametersInView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Hinzufügen einer **"Genre"** Modellklasse. Dazu, mit der Maustaste der **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *Genre.cs*, klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")

    *Hinzufügen eines neuen Elements*

    ![Hinzufügen der Modellklasse für "Genre"](aspnet-mvc-4-fundamentals/_static/image28.png "\"Genre\" Modellklasse hinzufügen")

    *Hinzufügen von "Genre" Model-Klasse*
4. Hinzufügen einer **Namen** Eigenschaft der Klasse "Genre". Zu diesem Zweck fügen Sie den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 "Genre"*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Befolgen die gleiche Vorgehensweise wie zuvor Hinzufügen einer **Album** Klasse. Dazu, mit der Maustaste der **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *Album.cs*, klicken Sie dann auf **hinzufügen**.
6. Fügen Sie zwei Eigenschaften zur Klasse Album: **"Genre"** und **Titel**. Zu diesem Zweck fügen Sie den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 Album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Aufgabe 2: Hinzufügen einer StoreBrowseViewModel

Ein **StoreBrowseViewModel** wird in dieser Aufgabe verwendet werden, um Alben anzuzeigen, die eine ausgewählte Genre entsprechen. In dieser Aufgabe Sie diese Klasse erstellen und dann fügen Sie zwei Eigenschaften zum Behandeln der **"Genre"** und die zugehörige **Album**der Liste.

1. Hinzufügen einer **StoreBrowseViewModel** Klasse. Dazu, mit der Maustaste der **ViewModels** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *StoreBrowseViewModel.cs*, klicken Sie dann auf **hinzufügen**.
2. Hinzufügen eines Verweises auf die Modelle in **StoreBrowseViewModel** Klasse. Zu diesem Zweck fügen Sie die folgenden Namespace verwenden:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Fügen Sie zwei Eigenschaften, **StoreBrowseViewModel** Klasse: **"Genre"** und **Alben**. Zu diesem Zweck fügen Sie den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Was ist **Liste&lt;Album&gt;**  ?: Diese Definition ist die Verwendung der **Liste&lt;T&gt;**  Typ, in denen **T** schränkt die Typ der Elemente dieser **Liste** gehören, in diesem Fall **Album** (oder eines seiner Nachfolger).
> 
> Diese Möglichkeit zum Entwerfen von Klassen und Methoden, die die Spezifikation des einen oder mehrere Typen verzögern, bis die Klasse oder Methode deklariert und instanziiert, indem der Client-Code ein Feature von c#-Sprache ist namens **Generika**.
> 
> **Liste&lt;T&gt;**  ist die generische Entsprechung, der die **ArrayList** geben und steht in der **System.Collections.Generic** Namespace. Einer der Vorteile der Verwendung von **Generika** , da der Typ angegeben ist, müssen Sie keine typüberprüfung für Vorgänge wie das Umwandeln der Elemente in kümmern **Album** wie mit einer **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Aufgabe 3: verwenden das neue "ViewModel" in der StoreController

In dieser Aufgabe ändern Sie die **StoreController**des **Durchsuchen** und **Details** Aktionsmethoden anwenden, um die Verwendung der neuen **StoreBrowseViewModel** .

1. Hinzufügen eines Verweises auf den Ordner "Models" im **StoreController** Klasse. Zu diesem Zweck, erweitern Sie die **Controller** Ordner in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** Klasse. Fügen Sie dann den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Ersetzen Sie die **Durchsuchen** Aktionsmethode verwendet die **StoreViewBrowseController** Klasse. Erstellen Sie ein Genre und zwei neue Alben Objekte mit unechten Daten (in der nächsten praktische Übungseinheit verwenden Sie echte Daten von einer Datenbank). Ersetzen Sie hierzu die **Durchsuchen** Methode durch den folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Ersetzen Sie die **Details** Aktionsmethode verwendet die **StoreViewBrowseController** Klasse. Erstellen Sie ein neues **Album** zu zurückzugebenden Objekts der **Ansicht**. Ersetzen Sie hierzu die **Details** Methode durch den folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Aufgabe 4: Hinzufügen eines durchsuchen-Vorlage anzeigen

In dieser Aufgabe fügen Sie eine **Durchsuchen** zum Anzeigen der Alben für eine spezifische Genre gefunden.

1. Vor dem Erstellen der neuen Vorlage anzeigen, sollten Sie das Projekt erstellen, damit die **Ansicht hinzufügen** Dialogfeld kennt die **"ViewModel"** zu verwendende Klasse an. Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.
2. Hinzufügen einer **Durchsuchen** anzeigen. Zu diesem Zweck mit der Maustaste in den **Durchsuchen** Action-Methode der der **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.
3. In der **Ansicht hinzufügen** Dialogfeld Vergewissern Sie sich, dass der Ansichtsname ist **Durchsuchen**. Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **StoreBrowseViewModel** aus der **Modellklasse** Dropdownliste. Lassen Sie die anderen Felder auch die Standardwerte. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen einer Ansicht durchsuchen](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer Ansicht durchsuchen")

    *Hinzufügen einer Ansicht durchsuchen*
4. Ändern der **Browse.cshtml** zur Anzeige von Informationen für die "Genre", den Zugriff auf die **StoreBrowseViewModel** -Objekt, das die ansichtsvorlage übergeben wird. Zu diesem Zweck ersetzen Sie den Inhalt durch Folgendes: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **Durchsuchen** Methode ruft Alben aus der **Durchsuchen** Methodenaktion.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum   **/Store/durchsuchen? Genre = Disco** zu überprüfen, ob die Aktion zwei Alben zurückgibt.

    ![Durchsuchen Store Disco Alben](aspnet-mvc-4-fundamentals/_static/image30.png "Disco Alben Store durchsuchen")

    *Disco Alben Store durchsuchen*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Aufgabe 6: Anzeigen von Informationen über ein bestimmtes Album

In dieser Aufgabe implementieren Sie die **Store/Details** können Sie Informationen über ein bestimmtes Album anzeigen. In dieser praktischen Übungseinheit alles, was Sie über das Album zeigt ist bereits Bestandteil der **Ansicht** Vorlage. In diesem Fall statt einer **StoreDetailsViewModel** -Klasse, verwenden Sie die aktuelle **StoreBrowseViewModel** Vorlage, die das Album an sie übergibt.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Fügen Sie einen neuen **Details** anzeigen für die **StoreController**des **Details** Aktionsmethode. Dazu, mit der Maustaste der **Details** -Methode in der die **StoreController** Klasse, und klicken Sie auf **Ansicht hinzufügen**.
2. In der **Ansicht hinzufügen** Dialogfeld, überprüfen Sie, ob die **Sichtname** ist **Details**. Überprüfen Sie die **eine stark typisierte Ansicht erstellen,** Kontrollkästchen, und wählen Sie **Album** aus der **Modellklasse** Dropdown-Liste. Lassen Sie die anderen Felder auch die Standardwerte. Klicken Sie anschließend auf **Hinzufügen**. Wird erstellt, und öffnen Sie eine **\Views\Store\Details.cshtml** Datei.

    ![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "Hinzufügen einer Detailansicht")

    *Hinzufügen einer Detailansicht*
3. Ändern der **Details.cshtml** Datei zur Anzeige von Informationen des Albums, den Zugriff auf die **Album** -Objekt, das die ansichtsvorlage übergeben wird. Zu diesem Zweck ersetzen Sie den Inhalt durch Folgendes: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **Details** Ansicht abruft Albuminformationen aus der **Details Aktion** Methode.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt startet der **Startseite** Seite. Ändern Sie die URL zum **/Store/Details/5** des Albums Informationen überprüfen.

    ![Durchsuchen Alben Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Alben Detail durchsuchen")

    *Durchsuchen des Albums-Details*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Aufgabe 8: Hinzufügen von Verknüpfungen zwischen Seiten

In dieser Aufgabe fügen Sie einen Link in der Ansicht der Store über einen Link in die Namen aller "Genre" in den entsprechenden **/Store/durchsuchen** URL. Auf diese Weise beim Klicken auf ein Genre, z. B. **Disco**, navigiert er zur **/Store/durchsuchen? Genre = Disco** URL.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Update der **Index** einen Link zum Hinzufügen der **Durchsuchen** Seite. Klicken Sie hierzu in der **Projektmappen-Explorer** erweitern Sie die **Ansichten** Ordner die **Store** Ordner und doppelklicken Sie auf die **"Index.cshtml"** Seite.
2. Die Ansicht durchsuchen, der angibt, des Genres ausgewählt haben, wird keine Verknüpfung hinzugefügt. Ersetzen Sie hierzu den folgenden hervorgehobenen Code innerhalb der **&lt;li&gt;** Tags: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > ein anderer Ansatz würde direkt auf der Seite mit einem Code wie folgt verknüpft werden:
   > 
   > &lt;ein Href =&quot;/Store/durchsuchen? Genre =@genreName&quot;&gt;@genreName &lt; /a&gt;
   > 
   > Obwohl dieser Ansatz funktioniert, hängt es eine hartcodierte Zeichenfolge ein. Wenn Sie später den Controller umbenennen, müssen Sie diese Anweisung manuell ändern. Eine bessere Alternative ist die Verwendung einer **HTML-Hilfsobjekt** Methode. ASP.NET MVC umfasst eine HTML-Hilfsobjekt-Methode, die für die Ausführung von Aufgaben verfügbar ist. Die **Html.ActionLink()** Hilfsmethode erleichtert Ihnen die Erstellung von HTML **&lt;eine&gt;** Links, sodass Sie sicher, dass die URL-Pfade sind ordnungsgemäß URL-codiert.
   > 
   > Htlm.ActionLink weist mehrere Überladungen. In dieser Übung werden Sie eine verwenden, die drei Parameter akzeptiert:
   > 
   > 1. Text des Links, der den Namen "Genre" angezeigt wird
   > 2. Name des Domänencontrollers Aktion (**Durchsuchen**)
   > 3. Weiterleiten von Parameterwerten, die sowohl den Namen angeben (**"Genre"**) und der Wert (**Name des Genres**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Aufgabe 9: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass jedes Genre, mit einem Link zu den entsprechenden angezeigt wird **/Store/durchsuchen** URL.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/Store** um sicherzustellen, dass jedes Genres an den entsprechenden links **/Store/durchsuchen** URL.

    ![Durchsuchen von Genres mit Links zu der Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "Genres durchsuchen, mit Links zu der Seite \"Durchsuchen\"")

    *Durchsuchen von Genres mit Links zu der Seite "Durchsuchen"*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Aufgabe 10 – mit dynamischen ViewModel-Auflistung zum Übergeben der Werte

In dieser Aufgabe lernen Sie, eine einfache und leistungsstarke Methode zum Übergeben von Werten zwischen dem Controller und die Ansicht, ohne dass Änderungen im Modell. ASP.NET MVC 4 stellt die Auflistung &quot;"ViewModel"&quot;, die auf einen dynamischen Wert zugewiesen und innerhalb von Controllern und Ansichten ebenfalls aufgerufen werden können.

Sie verwenden nun die dynamische ViewBag-Auflistung, übergeben eine Liste der &quot; **Sterne erhalten Genres** &quot; vom Controller an die Ansicht. Die Store Indexansicht wird Zugriff auf **"ViewModel"** und die Informationen anzuzeigen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreController.cs** und Ändern von **Index** Methode zum Erstellen einer Liste der Sterne Genres in ViewModel-Auflistung erhalten:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Außerdem können Sie die Syntax **"ViewBag" [&quot;Starred&quot;]** auf die Eigenschaften zugreifen.
2. Das Sternsymbol **&quot;starred.png&quot;** befindet sich auf die **Source\Assets\Images** Ordner dieser Anleitung. Um sie an die Anwendung hinzuzufügen, ziehen Sie in ihrer Inhalte mit einem **Windows Explorer** Einblick in die **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:

    ![Stern Image hinzufügen, mit der Lösung](aspnet-mvc-4-fundamentals/_static/image34.png "Stern Image hinzufügen, mit der Lösung")

    *Stern Image Hinzufügen zur Projektmappe*
3. Öffnen Sie die Ansicht **Store/Index.cshtml** und ändern Sie den Inhalt. Sie liest die &quot;Sterne erhalten&quot; -Eigenschaft in der **"ViewBag"** -Auflistung, und bitten Sie den Namen des aktuellen "Genre" in der Liste ist. In diesem Fall werden Sie einem Sternsymbol direkt auf den Link "Genre" angezeigt.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Aufgabe 11: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass die mit Sternen versehenen Genres einem Sternsymbol angezeigt.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt startet der **Startseite** Seite. Ändern Sie die URL zum **/Store** um sicherzustellen, dass jedes ausgewählte Genre die respektieren Bezeichnung aufweist:

    ![Durchsuchen von Genres mit Sternen versehenen Elemente](aspnet-mvc-4-fundamentals/_static/image35.png "Genres mit Sternen versehenen Elemente durchsuchen")

    *Durchsuchen von Genres mit Sternen versehenen Elemente*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Übung 7: Eine Tour durch neue ASP.NET MVC 4-Vorlage

In dieser Übung untersuchen Sie die Verbesserungen in den ASP.NET MVC 4-Projektvorlagen, Blick auf die wichtigsten Features der neuen Vorlage.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Aufgabe 1: Untersuchen der Internetanwendungsvorlage in ASP.NET MVC 4

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual c# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung**. **Namen** des Projekts *Music Store* und aktualisieren Sie die **Projektmappenname** zu *beginnen*, und wählen Sie einen Standort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK** .

    ![Erstellen ein neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image36.png "erstellen ein neues ASP.NET MVC 4-Projekt")

    *Erstellen ein neues ASP.NET MVC 4-Projekt*
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** Projektvorlage aus, und klicken Sie auf **OK**. Beachten Sie, dass Sie Razor oder ASPX als Ansichtsmodul auswählen können.

    ![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt wurde. Das Ziel ist, die die Anzahl von Zeichen und Tastaturanschläge erforderlich sind in einer Datei, und Aktivieren einer schnell und fließend, die Codierung von Workflows zu minimieren. Razor nutzt die vorhandenen C#/VB (oder andere) Fähigkeiten und bietet eine Vorlage Markupsyntax, die einen tollen HTML-Konstruktion Workflow ermöglicht.
4. Drücken Sie **F5** führen Sie die Projektmappe, und finden in der Vorlage erneuerten. Sie können Sie die folgenden Features überprüfen:

    1. **Modernes-Vorlagen**

        Die Vorlagen haben erneuert wurde, weitere Modern aussehende Formate bereitstellt.

        ![Vorlagen für ASP.NET MVC 4 neu gestaltet](aspnet-mvc-4-fundamentals/_static/image38.png "neu gestaltet Vorlagen von ASP.NET MVC 4")

        *Vorlagen für ASP.NET MVC 4 neu gestaltet*
    2. **Adaptive Rendering**

        Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout passen diese dynamisch auf die neue Fenstergröße an. Diese Vorlagen mithilfe der adaptiven Renderingtechnik um Desktop- und mobile Plattformen ohne Anpassung ordnungsgemäß zu rendern.

        ![ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.")

        *ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.*
5. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
6. Jetzt können Sie in der Lage, untersuchen die Projektmappe, und sehen sich die neuen Funktionen in der Projektvorlage von ASP.NET MVC 4.

    ![ASP.NET MVC4-Internet-Anwendung--Projektvorlage](aspnet-mvc-4-fundamentals/_static/image40.png "der Projekt Internetanwendungsvorlage in ASP.NET MVC 4")

    *Der Projekt Internetanwendungsvorlage in ASP.NET MVC 4*

   1. **HTML5-markup**

       Vorlage-Ansichten, um zu ermitteln, das neue Design Markup, z. B. open suchen **"About.cshtml"** anzeigen in **Startseite** Ordner.

       ![Neue Vorlage für die Verwendung von Razor und HTML5-Markup](aspnet-mvc-4-fundamentals/_static/image41.png "neue Vorlage für die Verwendung von Razor und HTML5-Markup")

       *Neue Vorlage für die Verwendung von Razor und HTML5-markup*
   2. **JavaScript-Bibliotheken enthalten**

      1. **jQuery**: jQuery vereinfacht die HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Interaktionen.
      2. **jQuery UI**: Diese Bibliothek bietet Abstraktionen für die Low-Level-Interaktion und Animationen, Erweiterte Effekte und Design beeinflusst Widgets, baut auf die jQuery-Bibliothek für JavaScript.

         > [!NOTE]
         > Sie erfahren, jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: die ASP.NET MVC 4-Standardvorlage enthält jetzt **KnockoutJS**, ein JavaScript-MVVM-Framework, das Sie umfassende und reaktionsschnelle Web-Anwendungen, die mit JavaScript und HTML-Elemente erstellen kann. Wie sind in ASP.NET MVC 3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.

          > [!NOTE]
          > Erhalten Sie weitere Informationen zur KnockOutJS-Bibliothek unter diesem Link: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: Diese Bibliothek automatisch ausgeführt, Ihre Website kompatibel mit älteren Browsern HTML5 und CSS3-Technologien mit herstellen.

          > [!NOTE]
          > Erhalten Sie weitere Informationen zu Modernizr-Bibliothek unter diesem Link: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **In der Projektmappe enthalten SimpleMembership**

       SimpleMembership wurde als Ersatz für das vorherige ASP.NET Rollen- und Mitgliedschaftseinstellungen anbietersystem entwickelt. Es verfügt über viele neue Features, die für den Entwickler sichere Webseiten auf eine flexiblere Art und Weise zu vereinfachen.

       Die Internet-Vorlage hat bereits ein paar Dinge SimpleMembership integrieren festgelegt ist, z. B. AccountController-Komponente wird vorbereitet "oauthwebsecurity" (für OAuth-kontoregistrierung "," Login "," Verwaltung "," usw. ") und Internet-Sicherheit verwenden.

       ![SimpleMembership in der Projektmappe enthalten](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership in der Projektmappe enthalten.")

       *SimpleMembership in der Projektmappe enthalten.*

       > [!NOTE]
       > Weitere Informationen finden Sie Informationen zu ["oauthwebsecurity"](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie die Grundlagen von ASP.NET MVC gelernt:

- Die Kernelemente einer MVC-Anwendung und deren Interaktion
- Vorgehensweise: Erstellen einer ASP.NET MVC-Anwendung
- Das Hinzufügen und Konfigurieren von Controllern zum Verarbeiten von Parametern übergeben wird, über die URL und Abfragezeichenfolge
- Gewusst wie: hinzufügen eine Masterseite "Layout" zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten und eine ansichtsvorlage zum Anzeigen von HTML-Inhalten zu verbessern
- Gewusst wie: Verwenden Sie das ViewModel-Muster für die Übergabe von Eigenschaften an die ansichtsvorlage zum Anzeigen dynamischer Informationen
- Verwendung von Parametern an Controller übergeben werden, in der Vorlage anzeigen
- Gewusst wie: Hinzufügen von Links zu Seiten, die in der ASP.NET MVC-Anwendung
- Das Hinzufügen und Verwenden von dynamischen Eigenschaften in einer Sicht
- Die Verbesserungen in ASP.NET MVC 4-Projektvorlagen

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](aspnet-mvc-4-fundamentals/_static/image50.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](aspnet-mvc-4-fundamentals/_static/image51.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-fundamentals/_static/image52.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-fundamentals/_static/image53.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image54.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image55.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-fundamentals/_static/image62.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image66.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

    ![Anwendung auf Windows Azure veröffentlicht](aspnet-mvc-4-fundamentals/_static/image68.png "Anwendung auf Windows Azure veröffentlicht")

    *Anwendung in Windows Azure veröffentlicht*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-fundamentals/_static/image70.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-fundamentals/_static/image71.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-fundamentals/_static/image72.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-fundamentals/_static/image73.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-fundamentals/_static/image74.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
