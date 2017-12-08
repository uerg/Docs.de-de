---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4-Grundlagen | Microsoft Docs
author: rick-anderson
description: "Diese praktische Übungseinheit auf MVC (Model View Controller) Music Store eine lernprogrammanwendung basiert, eingeführt und erläutert schrittweise, wie ASP.NET MV verwenden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 086084b63cceca1c2d4e0bd4e5b654aaad6637a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4-Grundlagen
====================
durch [Web Lager Team](https://twitter.com/webcamps)

> Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store eine lernprogrammanwendung, die werden vorgestellt und erläutert schrittweise, wie ASP.NET MVC und Visual Studio verwenden. Im Labor erfahren Sie der Einfachheit halber noch Stromversorgung, der die gemeinsame Verwendung von diesen Technologien. Sie werden mit einer einfachen Anwendung gestartet und realisiert werden erst stehen Ihnen eine voll funktionsfähige ASP.NET MVC 4-Webanwendung.
> 
> In dieser Umgebung funktioniert mit ASP.NET MVC 4.
> 
> Wenn Sie die ASP.NET MVC 3-Version des Lernprogramms Anwendung untersuchen möchten, finden Sie in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).
> 
> > [!NOTE]
> > Diese praktische Übungseinheit wird davon ausgegangen, dass der Entwickler Erfahrung in der Entwicklung von webtechnologien wie HTML und JavaScript hat.
> 
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Die Music Store-Anwendung

Die Music Store-Webanwendung, die in dieser Umgebung erstellt werden besteht aus drei Hauptteilen zusammen: Warenkorb, Auschecken und Verwaltung. Besucher sind Lage Alben durchsuchen, indem "Genre" Einkaufswagen Alben hinzu, überprüfen ihre Auswahl, und fahren Sie schließlich mit Auschecken, um sich anzumelden, und führen Sie die Reihenfolge. Darüber hinaus werden Administratoren Store verfügbaren Alben sowie ihre wichtigsten Eigenschaften verwalten können.

![Music Store Bildschirme](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store Bildschirme")

*Music Store Bildschirme*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4-Essentials

Music Store-Anwendung erstellt, die mit **Model View Controller (MVC)**, eine architektonische Muster, das trennt eine Anwendung in drei Hauptkomponenten:

- **Modelle**: Modellobjekte sind die Teile der Anwendung, die die Domänenlogik implementieren. Modellobjekte wird häufig auch abrufen, und der Modellzustand in einer Datenbank speichern.
- **Ansichten:** Ansichten sind die Komponenten, die Benutzeroberfläche (UI) der Anwendung anzeigen. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre der Bearbeitungsansicht von Alben, die Textfelder und eine Dropdown-Liste basierend auf den aktuellen Status eines Objekts Album anzeigt.
- **Domänencontroller:** Controller sind die Komponenten behandeln Benutzerinteraktionen, bearbeiten das Modell und letztlich wählen Sie eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs. In einer MVC-Anwendung zeigt die Ansicht nur Informationen; der Controller behandelt und antwortet auf Benutzereingaben und Interaktion.

Das MVC-Muster unterstützt Sie beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und Benutzeroberflächen-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen liegen. Aufgrund dieser Trennung können Sie die Komplexität bei der Erstellung einer Anwendung verwalten, wie Sie auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können. Darüber hinaus erleichtert das MVC-Schema zum Testen von Anwendungen, auch wenn Sie die Verwendung von Test-driven Development (TDD) zum Erstellen von Anwendungen.

Die **ASP.NET-MVC** Framework bietet eine Alternative zum ASP.NET Web Forms-Muster für das Erstellen von ASP.NET MVC-basierten Webanwendungen. Die **ASP.NET-MVC** Framework ist eine kompakte, mitgliedschaftsbasierte zu testendes Präsentationsframework, (wie bei Web Forms-basierten Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und Mitgliedschaft basierende integriert Authentifizierung, sodass Sie die gesamte Leistungsfähigkeit von .NET Framework Core abrufen. Dies ist hilfreich, wenn Sie bereits mit ASP.NET Web Forms vertraut sind, da alle Bibliotheken, die Sie bereits verwenden, ebenfalls in ASP.NET MVC 4 verfügbar sind.

Der lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung begünstigt darüber hinaus auch die parallele Entwicklung. Z. B. ein Entwickler kann für die Sicht arbeiten, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler auf die Geschäftslogik im Modell konzentrieren kann.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen einer ASP.NET MVC-Anwendung von Grund auf Grundlage des Lernprogramms Music Store-Anwendung
- Hinzufügen von Domänencontroller zum Behandeln von URLs zur Startseite des Standorts und seine wichtigsten Funktionen durchsuchen
- Fügen Sie eine Ansicht, um den Inhalt angezeigt, zusammen mit den Stil anzupassen
- Fügen Sie Modellklassen zum enthalten und Verwalten von Daten und Domäne Logik hinzu
- Verwenden Sie ViewModel-Muster, um Informationen aus Controlleraktionen an die Ansichtsvorlagen zu übergeben.
- Untersuchen Sie die neue ASP.NET MVC 4-Vorlage für Internetanwendungen

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:

1. [Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt MusicStore](#Exercise1)
2. [Übung 2: Erstellen eines Domänencontrollers](#Exercise2)
3. [Übung 3: Übergeben von Parametern zu einem Domänencontroller](#Exercise3)
4. [Übung 4: Erstellen einer Ansicht](#Exercise4)
5. [Übung 5: Erstellen eines Modells anzeigen](#Exercise5)
6. [Übung 6: Verwenden von Parametern in der Ansicht](#Exercise6)
7. [Übung 7: Lap um neue ASP.NET MVC 4-Projektvorlage](#Exercise7)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt MusicStore

In dieser Übung erfahren Sie, wie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für Web sowie seiner Organisation Hauptordner zu erstellen. Darüber hinaus erfahren Sie, wie fügen einen neuen Domänencontroller, und stellen sie eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Aufgabe 1: Erstellen der ASP.NET MVC-Webanwendungsprojekts

1. In dieser Aufgabe erstellen Sie eine leere ASP.NET MVC-Anwendungsprojekt mit der Visual Studio für MVC-Vorlage. Starten Sie **Visual Studio Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. In der **neues Projekt** aktivieren Sie im Dialogfeld die **ASP.NET MVC 4-Webanwendung** Projekttyp, befindet sich im **Visual c#** **Web** Vorlage Liste.
4. Ändern der **Namen** auf *MvcMusicStore*.
5. Legen Sie den Speicherort für die Projektmappe in einem neuen **beginnen** Ordner in dieser Übung Quellordner, z. B. **[HOL-den-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. Klicken Sie auf **OK**.

    ![Dialogfeld Neues Projekt erstellen](aspnet-mvc-4-fundamentals/_static/image2.png "Dialogfeld Neues Projekt erstellen")

    *Dialogfeld Neues Projekt erstellen*
6. In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **grundlegende** Vorlage und stellen Sie sicher, dass die **Ansichtsmodul** ausgewählt ist **Razor**. Klicken Sie auf **OK**.

    ![Neues ASP.NET MVC 4-Projekt (Dialogfeld)](aspnet-mvc-4-fundamentals/_static/image3.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")

    *Neues ASP.NET MVC 4-Projekt (Dialogfeld)*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Aufgabe 2: Untersuchen der Projektmappenstruktur

ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, mit die Sie die Erstellung von Webanwendungen ermöglichen das MVC-Muster unterstützen kann. Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung mit der erforderlichen Ordnern, Elementvorlagen und Konfigurationsdatei Einträge.

In dieser Aufgabe untersuchen Sie die Lösungsstruktur werden die Elemente erläutert, die beteiligt sind und ihre Beziehungen. Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig verwendet eine &quot;Konvention über Konfiguration&quot; Ansatz und lässt einige Standard-Annahmen basierend auf den Ordner benennen Konventionen.

1. Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die im Projektmappen-Explorer auf der rechten Seite erstellt wurde:

    ![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")

    *ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*

    1. **Controller**. Dieser Ordner enthält die Controllerklassen. In einer MVC-basierten Anwendung sind Domänencontroller verantwortlich für die Behandlung von End-Benutzerinteraktion, bearbeiten das Modell und letztendlich eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs auswählen.

        > [!NOTE]
        > Das MVC-Framework erfordert die Namen aller Controller endet nicht mit &quot;Controller&quot;– z. B. HomeController, LoginController oder ProductController.
    2. **Modelle**. Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen. Dies schließt in der Regel Code, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert. In der Regel werden die eigentlichen Modellobjekte in separaten Klassenbibliotheken. Wenn Sie eine neue Anwendung erstellen, können Sie allerdings umfassen Klassen und klicken Sie dann im Entwicklungszyklus in separate Klassenbibliotheken zu einem späteren Zeitpunkt verschieben.
    3. **Sichten**. Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich. Ansichten verwenden aspx, .ascx, cshtml und Master-Dateien zusätzlich zu anderen Dateien, die zum Rendern von Ansichten verknüpft werden. Ordner Views enthält einen Ordner für jeden Controller. der Ordner den Namen mit dem Controller-Namenspräfix. Angenommen, Sie haben einen Controller mit dem Namen **HomeController**, der Ordner Views enthält einen Ordner mit der Bezeichnung Home. Wird standardmäßig beim Laden von ASP.NET MVC-Framework einer Sicht sucht es nach einer ASPX-Datei mit dem angeforderten Namen im Ordner "Views\controllerName" (**Ansichten [ControllerName] [Aktion] aspx**) oder (**Ansichten [ControllerName] [Aktion] cshtml**) für die Razor-Ansichten.

    > [!NOTE]
    > Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Webanwendung die **"Global.asax"** standardmäßig Datei globale URL-routing und verwendet die **"Web.config"** Datei zum Konfigurieren der Anwendung.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Aufgabe 3: Hinzufügen einer HomeController

In ASP.NET-Anwendungen, die das MVC-Framework nicht verwenden, ist eine Benutzerinteraktion Seiten und herum durch das Auslösen und Behandeln von Ereignissen aus diesen Seiten angeordnet. Im Gegensatz dazu ist Benutzerinteraktion in ASP.NET-MVC-Anwendungen in Controllern und deren Aktionsmethoden organisiert.

ASP.NET MVC-Framework ordnet andererseits, URLs Klassen, die als Domänencontroller bezeichnet werden. Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen Sie die entsprechende Anwendungslogik und die Antwort zurück an den Client senden zu bestimmen (HTML-Seite anzeigen, eine Datei herunterladen, Umleiten zu einer anderen URL usw.). Im Fall von HTML anzeigen, ruft eine Controllerklasse in der Regel eine separate Ansichtskomponente zum Generieren von HTML-Markup für die Anforderung. In einer MVC-Anwendung zeigt die Ansicht nur Informationen; der Controller behandelt und antwortet auf Benutzereingaben und Interaktion.

In dieser Aufgabe fügen Sie eine Controllerklasse, die URLs zur Startseite des Standorts Music Store behandelt.

1. Mit der rechten Maustaste **Controller** Ordner im Projektmappen-Explorer wählen **hinzufügen** und dann **Controller** Befehl:

    ![Hinzufügen eines Befehls Controller](aspnet-mvc-4-fundamentals/_static/image5.png "fügen Sie einen Controllerbefehl")

    *Controllerbefehl hinzufügen*
2. Die **Controller hinzufügen** Dialogfeld wird angezeigt. Nennen Sie den Controller *HomeController* , und drücken Sie **hinzufügen**.

    ![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image6.png "Controller-Dialogfeld "hinzufügen"")

    *Controller-Dialogfeld "hinzufügen"*
3. Die Datei **HomeController.cs** wird erstellt, der **Controller** Ordner. Um die **HomeController** geben eine Zeichenfolge zurück, dessen Index-Aktion bei, ersetzen Sie die **Index** -Methode durch folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex1 HomeController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser testen werden.

1. Drücken Sie **F5** um die Anwendung auszuführen. Kompilieren des Projekts, und der lokalen IIS-Webserver startet. Lokalen IIS-Webserver wird einen Webbrowser auf die URL des Webservers automatisch geöffnet.

    ![In einem Webbrowser ausgeführte Anwendung](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung in einem Webbrowser ausgeführt wird")

    *Anwendung in einem Webbrowser ausgeführt wird*

    > [!NOTE]
    > Dem lokalen IIS-Webserver wird eine zufällige freie Portnummer an der Website ausführen. In der obigen Abbildung dem Standort ausgeführt wird, am `http://localhost:50103/`, sodass Port 50103 verwendet wird. Die Portnummer kann variieren.
2. Schließen Sie den Browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Übung 2: Erstellen eines Domänencontrollers

In dieser Übung erfahren Sie, wie beim Aktualisieren des Controllers, um eine einfache Funktionalität der Music Store-Anwendung zu implementieren. Dieser Controller definieren Aktionsmethoden, um die folgenden spezifischen Anforderungen zu verarbeiten:

- Eine Auflistung-Seite mit der Musik Genres im Music Store
- Seite "Durchsuchen", die alle Musikalben für einen bestimmten "Genre" aufgelistet sind
- Seite "Details", das Informationen zu einem bestimmten Musikalbum anzeigt

Für den Rahmen dieser Übung gibt diese Aktionen nun einfach eine Zeichenfolge zurück.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse

In dieser Aufgabe fügen Sie einen neuen Domänencontroller.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web 2012**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex02 CreatingAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Hinzufügen eines neuen Controllers. Zu diesem Zweck Maustaste die **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann die **Controller** Befehl. Ändern der **Controllernamen** auf *StoreController*, und klicken Sie auf **hinzufügen**.

    ![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image8.png "Controller-Dialogfeld "hinzufügen"")

    *Controller-Dialogfeld "hinzufügen"*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Aufgabe 2: Ändern der StoreController Aktionen

In dieser Aufgabe ändern Sie die Controller-Methoden, die aufgerufen werden **Aktionen**. Aktionen sind verantwortlich für die Behandlung von URL-Anforderungen und bestimmen den Inhalt, der wieder an den Browser oder Benutzer, der die URL aufgerufen gesendet werden soll.

1. Die **StoreController** -Klasse verfügt bereits über ein **Index** Methode. Sie verwenden es später in dieser Übung die Seite zu implementieren, die alle Genres von Music Store aufgelistet. Ersetzen Sie jetzt einfach die **Index** Methode durch den folgenden Code, der eine Zeichenfolge zurückgibt, &quot;Hello aus Store.Index()&quot;:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Hinzufügen **Durchsuchen** und **Details** Methoden. Zu diesem Zweck fügen Sie den folgenden Code aus, um die **StoreController**:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser testen werden.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt gestartet wird, der **Home** Seite. Ändern Sie die URL, um zu überprüfen, ob jede Aktion-Implementierung.

    1. **/ Speichern**. Sehen Sie  **&quot;Hello aus Store.Index()&quot;**.
    2. **/ Store/Durchsuchen**. Sehen Sie  **&quot;Hello aus Store.Browse()&quot;**.
    3. **/ Store/Detail-**. Sehen Sie  **&quot;Hello aus Store.Details()&quot;**.

        ![Durchsuchen von StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse durchsuchen")

        */Store/Browse durchsuchen*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Übung 3: Übergeben von Parametern zu einem Domänencontroller

Bis jetzt haben Sie Konstante Zeichenfolgen aus Controller zurückgeben wurde. In dieser Übung erfahren Sie, wie Parameter auf einen Domänencontroller mithilfe der URL und Abfragezeichenfolge, und nehmen Sie dann die methodenaktionen reagiert mit Text an den Browser zu übergeben.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Aufgabe 1: Hinzufügen von "Genre"-Parameter StoreController

In dieser Aufgabe verwenden Sie die **Querystring** Parameter zum Senden der **Durchsuchen** Aktionsmethode in der **StoreController**.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex03 PassingParametersToAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Open **StoreController** Klasse. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
4. Ändern der **Durchsuchen** Methode, die einen Zeichenfolgenparameter an die Anforderung für einen bestimmten "Genre" hinzufügen. ASP.NET MVC automatisch alle Querystring übergeben oder Parameter mit den Namen der formularbereitstellung **"Genre"** auf diese Aktionsmethode beim Aufrufen. Ersetzen Sie hierzu die **Durchsuchen** -Methode durch folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > Verwenden Sie die **HttpUtility.HtmlEncode durchführen** Hilfsprogrammmethode zum verhindert, dass Benutzer Räumen Javascript in der Ansicht mit einem Link wie   **/Store/durchsuchen? "Genre" =&lt;Skript&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.
    > 
    > Weitere Hinweise finden Sie auf [diesem Msdn-Artikel](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser testen und verwenden Sie die **"Genre"** Parameter.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt gestartet wird, der **Home** Seite. Ändern Sie die URL zum   */speichern/suchen? "Genre" = Disco* um sicherzustellen, dass die Aktion den Parameter "Genre" empfängt.

    ![Durchsuchen von StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre durchsuchen Disco =")

    *Durchsuchen Sie /Store/Browse? "Genre" Disco =*
3. Schließen Sie den Browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Aufgabe 3: Hinzufügen eines Id-Parameters, die in die URL eingebettet

In dieser Aufgabe verwenden Sie die **URL** übergeben ein **Id** Parameter an die **Details** Aktionsmethode von der **StoreController**. ASP.NETs standardmäßig routing-Konvention ist, das eine URL-Segment nach dem Methodennamen Aktion zu behandeln, als einen Parameter namens **Id**. Wenn der Aktionsmethode Parameter Id verfügt, wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben. In der URL **Store/Informationen/5**, **Id** als interpretiert **5**.

1. Ändern der **Details** Methode der **StoreController**, Hinzufügen von einer **Int** Parameter namens **Id**. Ersetzen Sie hierzu die **Details** -Methode durch folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser testen und verwenden Sie die **Id** Parameter.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt gestartet wird, der **Home** Seite. Ändern Sie die URL zum */Store/Details/5* um sicherzustellen, dass die Aktion der Id-Parameter erhält.

    ![Durchsuchen von StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 durchsuchen")

    */Store/Details/5 durchsuchen*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Übung 4: Erstellen einer Ansicht

Bisher haben Sie Zeichenfolgen aus Controlleraktionen zurückgegeben wurde. Obwohl dies ist eine hilfreiche Möglichkeit zum Verständnis der Funktionsweise von Domänencontrollern, ist es nicht, wie Ihre echten Web-Anwendungen erstellt werden. Ansichten sind Komponenten, die einen besseren Ansatz zum Generieren von HTML-Codes zurück an den Browser mit der Verwendung von Vorlagendateien zu bieten.

In dieser Übung erfahren Sie, wie eine Layout-Gestaltungsvorlage zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet für das Aussehen und Verhalten der Website und schließlich eine Vorlage anzeigen HomeController zurückzugebenden HTML aktivieren verbessern hinzugefügt werden.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Aufgabe 1: Ändern der Datei \_layout.cshtml

Die Datei **~/Views/Shared/\_layout.cshtml** ermöglicht es Ihnen so richten Sie eine Vorlage für allgemeine HTML, über die gesamte Website zu verwenden. In dieser Aufgabe fügen Sie eine master-Layoutseite mit einer allgemeinen Header mit Links in den Bereich auf der Startseite und Speicher.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex04 CreatingAView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Die Datei  **\_layout.cshtml** das HTML-Container-Layout für alle Seiten auf der Website enthält. Es enthält die  **&lt;html&gt;**  -Element für die HTML-Antwort als auch die  **&lt;Head&gt;**  und  **&lt;Text&gt;**  Elemente. **@RenderBody()** innerhalb des HTML-Text zu identifizieren Regionen dieser Ansicht Vorlagen werden in der Lage, sich mit dynamischen Inhalten zu füllen.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Fügen Sie einen allgemeinen Header mit Links in den Bereich auf der Startseite und Speicher auf allen Seiten der Website hinzu. Zu diesem Zweck fügen Sie den folgenden Code, der unten &lt;Text&gt; Anweisung.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Enthalten Sie ein Div um Textabschnitt Rand jeder Seite zu rendern. Ersetzen Sie  **@RenderBody()** durch den folgenden Code der Higlighted: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Wussten Sie schon? Visual Studio 2012 weist Ausschnitte, die zum Hinzufügen von häufig verwendeten Code in HTML, Codedateien und mehr erleichtern! Versuchen Sie es, indem Sie Folgendes eingeben  **&lt;Div&gt;**  und **Registerkarte** zweimal, um eine vollständige einfügen **Div** Tag.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Aufgabe 2: Hinzufügen von CSS-Stylesheet

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zum Anzeigen von grundlegenden Formen und validierungsmeldungen enthält nur. Sie zusätzliche CSS-Images (die möglicherweise von einem Designer bereitgestellt werden) verwenden und damit das Aussehen und Verhalten des Standorts zu verbessern.

In dieser Aufgabe fügen Sie eine CSS-Stylesheet, um die Formatvorlagen der Site zu definieren.

1. Die CSS-Datei und die Bilder befinden sich die **Source\Assets\Content** Ordner dieser Anleitung. Um diese an die Anwendung hinzuzufügen, ziehen Sie ihren Inhalt aus einem **Windows Explorer** Fenster in der **Projektmappen-Explorer** in Visual Studio Express für Web, wie unten dargestellt:

    ![Ziehen die Formatvorlage Inhalt](aspnet-mvc-4-fundamentals/_static/image12.png "Stil Inhalt ziehen")

    *Ziehen die Formatvorlage Inhalt*
2. Ein Warndialogfeld angezeigt nach einer Bestätigung ersetzen **"Site.CSS" ändern** Datei- und einige vorhandener Bilder. Überprüfen Sie **für alle Elemente übernehmen** , und klicken Sie auf **Ja**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Aufgabe 3: Hinzufügen einer Vorlage für eine Sicht

In dieser Aufgabe fügen Sie eine Vorlage anzeigen, um HTML-Antwort zu generieren, die das Layout-Gestaltungsvorlage verwendet wird, und CSS in dieser Übung hinzugefügt.

1. Um eine Vorlage anzeigen beim Durchsuchen der Homepage der Website zu verwenden, müssen Sie zuerst an, dass eine Zeichenfolge zurückgegeben, sondern die **HomeController Index** Methode zurückgegeben wird ein **Ansicht**. Öffnen **HomeController** Klasse, und Ändern seiner **Index** -Methode zur Rückgabe einer **ActionResult**, und zurückgeben **View()**.

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex4 HomeController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Nun müssen Sie eine entsprechende Ansichtenvorlage hinzufügen. Hierzu **mit der rechten Maustaste** innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.

    ![Hinzufügen einer Ansicht von innerhalb der Methode Index](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Ansicht von innerhalb der Index-Methode")

    *Hinzufügen einer Ansicht von innerhalb der Index-Methode*
3. Die **Ansicht hinzufügen** wird ein Dialogfeld angezeigt, um eine Vorlagendatei Ansicht zu generieren. Standardmäßig füllt dieses Dialogfeld den Namen der Vorlage anzeigen, damit diese die Aktionsmethode übereinstimmt, die verwendet werden. Da Sie verwendet die **Ansicht hinzufügen** Kontextmenü innerhalb der **Index** Aktionsmethode innerhalb der HomeController die **Ansicht hinzufügen** Dialogfeld Index mit dem Standardnamen für die Sicht hat. Klicken Sie auf **Hinzufügen**.

    ![View-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image14.png "Ansicht-Dialogfeld "hinzufügen"")

    *View-Dialogfeld "hinzufügen"*
4. Visual Studio generiert eine **Index.cshtml** Vorlage anzeigen, in der **Views\Home den** Ordner und dann geöffnet.

    ![Home Indexansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Home Indexansicht erstellt")

    *Home Indexansicht erstellt*

    > [!NOTE]
    > Name und Speicherort der der **Index.cshtml** Datei spielt und folgt die Standard-ASP.NET-MVC-Benennungskonventionen.
    > 
    > Der Ordner \Views\**Home** den Controllernamen entspricht (**Home** Controller). Den Namen der Sicht (**Index**), entspricht der Aktionsmethode des Controllers der Ansicht anzeigen werden.
    > 
    > Auf diese Weise vermeidet ASP.NET-MVC den Namen oder Speicherort der Vorlage für eine Sicht explizit angeben, wenn Sie diese Namenskonvention mithilfe eine Sicht zurück.
5. Generierte Ansichtenvorlage basiert auf der  **\_layout.cshtml** Vorlage, die zuvor definiert. Aktualisieren Sie die ViewBag.Title-Eigenschaft, um **Home**, und ändern Sie den Hauptinhalt zu **Dies ist die Startseite**, wie im folgenden Code gezeigt:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wählen Sie **MvcMusicStore** Projekt im Projektmappen-Explorer und drücken Sie **F5** um die Anwendung auszuführen.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Aufgabe 4: Überprüfung

Um sicherzustellen, dass Sie alle Schritte richtig im vorherigen Schritt ausgeführt haben, gehen Sie wie folgt aus:

Mit der Anwendung in einem Browser geöffnet wird sollten Sie Folgendes beachten:

1. Die HomeController Index Aktionsmethode gefunden und angezeigt der **\Views\Home\Index.cshtml** Vorlage anzeigen, auch wenn der Code aufgerufen **zurückgeben View()**, da gefolgt von der Vorlage Anzeigen der standardmäßige Benennungskonvention zu verwenden.
2. Auf der Startseite zeigt die Begrüßungsnachricht, der definiert, die innerhalb der **\Views\Home\Index.cshtml** Vorlage anzeigen.
3. Die Startseite wird mithilfe der  **\_layout.cshtml** Vorlage, und damit die Willkommensnachricht innerhalb der standardmäßigen Website HTML-Layout enthalten ist.

    ![Mit dem definierten LayoutPage und Stil Indexansicht Home](aspnet-mvc-4-fundamentals/_static/image16.png "Home Indexansicht, die mit dem definierten LayoutPage und Stil")

    *Home Indexansicht mit dem definierten LayoutPage und Stil*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Übung 5: Erstellen eines Modells anzeigen

Bisher vorgenommenen Ihre Ansichten hartcodiert HTML angezeigt, aber damit dynamischer Webanwendungen erstellen können, sollten die Vorlage anzeigen Informationen vom Controller erhalten. Ist ein allgemeines Verfahren für diesen Zweck verwendet werden soll die **ViewModel** Muster, welches einen Controller ermöglicht so verpacken Sie die Informationen, die zum Generieren der entsprechenden HTML-Antwort erforderlich.

In dieser Übung erfahren Sie, wie eine ViewModel-Klasse erstellen, und fügen die erforderlichen Eigenschaften: die Anzahl der in den Speicher und eine Liste von diesen Genres Genres. Sie werden auch die StoreController Verwendung erstellte ViewModel aktualisiert, und schließlich erstellen Sie eine neue Vorlage anzeigen, in denen die genannten Eigenschaften auf der Seite angezeigt wird.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Aufgabe 1: Erstellen einer ViewModel-Klasse

In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, die im Speicher "Genre" Angebot Szenario implementiert wird.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex05 CreatingAViewModel\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Erstellen einer **ViewModels** Ordner für das ViewModel. Zu diesem Zweck der obersten Ebene Maustaste **MvcMusicStore** -Projekt, wählen **hinzufügen** und dann **neuer Ordner**.

    ![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "einen neuen Ordner hinzufügen")

    *Hinzufügen eines neuen Ordners*
4. Benennen Sie den Ordner *ViewModels*.

    ![ViewModels-Ordner im Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels-Ordner im Projektmappen-Explorer")

    *ViewModels-Ordner im Projektmappen-Explorer*
5. Erstellen einer **ViewModel** Klasse. Zu diesem Zweck mit der Maustaste auf die **ViewModels** Ordner vor kurzem erstellt wurde, wählen Sie **hinzufügen** und dann **neues Element**. Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *StoreIndexViewModel.cs*, klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "Hinzufügen einer neuen Klasse")

    *Hinzufügen einer neuen Klasse*

    ![Erstellen von StoreIndexViewModel Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "erstellen StoreIndexViewModel-Klasse")

    *Erstellen von StoreIndexViewModel-Klasse*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Aufgabe 2: Hinzufügen von Eigenschaften für das ViewModel-Klasse

Es gibt zwei Parameter an die Vorlage anzeigen aus der StoreController übergeben werden, um die erwartete HTML-Antwort zu generieren: die Anzahl der in den Speicher und eine Liste von diesen Genres Genres.

In dieser Aufgabe fügen Sie dieser 2 Eigenschaften der **StoreIndexViewModel** Klasse: **NumberOfGenres** (eine Ganzzahl) und **Genres** (eine Liste von Zeichenfolgen).

1. Hinzufügen **NumberOfGenres** und **Genres** Eigenschaften der **StoreIndexViewModel** Klasse. Zu diesem Zweck fügen Sie der Klassendefinition die 2 folgenden Zeilen hinzu:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreIndexViewModel Eigenschaften*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > Die **{abrufen; festlegen;}**  Notation nutzt # Funktion automatisch implementierte Eigenschaften. Bietet die Vorteile einer Eigenschaft ohne uns einen dahinter liegende Feld nicht deklariert werden.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Aufgabe 3 - Update StoreController die StoreIndexViewModel verwenden

Die **StoreIndexViewModel** -Klasse kapselt die benötigten Informationen zum Übergeben von **StoreController**des **Index** Methode, um eine Vorlage anzeigen, um eine Antwort zu generieren. .

In dieser Aufgabe aktualisieren Sie die **StoreController** verwenden die **StoreIndexViewModel**.

1. Open **StoreController** Klasse.

    ![Öffnen StoreController Klasse](aspnet-mvc-4-fundamentals/_static/image21.png "öffnende StoreController-Klasse")

    *Öffnende StoreController-Klasse*
2. Zum Verwenden der **StoreIndexViewModel** -Klasse aus den **StoreController**, fügen Sie den folgenden Namespace am Anfang der **StoreController** Code:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreIndexViewModel mit ViewModels*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Ändern der **StoreController**des **Index** Aktionsmethode so, dass er erstellt und füllt eine **StoreIndexViewModel** -Objekt und übergibt Sie es dann, um eine Ansicht aus, um Generieren einer HTML-Antwort mit.

    > [!NOTE]
    > Im Labor &quot;ASP.NET MVC-Modelle und Datenzugriff&quot; schreiben Sie Code, der die Liste der Store Genres aus einer Datenbank abruft. In den folgenden Code, erstellen Sie eine **Liste** von Genres dummy-Daten, die mit Daten Auffüllen der **StoreIndexViewModel**.
    > 
    > Nach dem Erstellen und Einrichten der **StoreIndexViewModel** übergeben, es wird als Argument an die **Ansicht** Methode. Dies gibt an, dass die Vorlage anzeigen zum Generieren einer HTML-Antwort, damit dieses Objekt verwendet.
4. Ersetzen Sie die **Index** -Methode durch folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreController Index Methode*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > Wenn Sie nicht mit c# vertraut sind, Sie können davon ausgehen, dass die Verwendung **Var** bedeutet, dass die **ViewModel** Variable wird spät gebunden. Das ist nicht richtig - Typrückschluss basierend auf, was Sie der Variable zuzuweisen ermitteln, ob der C#-Compiler verwendeten **ViewModel** ist vom Typ **StoreIndexViewModel**. Auch durch das Kompilieren des lokalen **ViewModel** -Variable als ein **StoreIndexViewModel** Geben Sie Get kompilierzeitüberprüfung und Unterstützung von Visual Studio Code-Editors.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Aufgabe 4: Erstellen einer Ansichtenvorlage für die, die StoreIndexViewModel verwendet.

In dieser Aufgabe erstellen Sie eine Vorlage anzeigen, die ein StoreIndexViewModel-Objekt, das vom Controller übergebenen nutzt, um eine Liste von Genres anzuzeigen.

1. Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht-Dialogfeld hinzufügen** kennt die **StoreIndexViewModel** Klasse. Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.

    ![Beim Erstellen des Projekts](aspnet-mvc-4-fundamentals/_static/image22.png "beim Erstellen des Projekts")

    *Beim Erstellen des Projekts*
2. Erstellen Sie eine neue Ansichtenvorlage für die ein. Zu diesem Zweck Maustaste innerhalb der **Index** Methode, und wählen **Ansicht hinzufügen**.

    ![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "eine Sicht hinzufügen")

    *Hinzufügen einer Ansicht*
3. Da die **Ansicht-Dialogfeld hinzufügen** aufgerufen wurde, aus der **StoreController**, wird standardmäßig in der Vorlage anzeigen hinzugefügt eine **\Views\Store\Index.cshtml** Datei. Überprüfen Sie die **erstellen Sie eine stark typisierte-anzeigen** Kontrollkästchen, und wählen Sie dann **StoreIndexViewModel** als die **Modellschemas**. Stellen Sie außerdem sicher, dass das Ansichtsmodul ausgewählt ist **Razor**. Klicken Sie auf **Hinzufügen**.

    ![View-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image24.png "Ansicht-Dialogfeld "hinzufügen"")

    *View-Dialogfeld "hinzufügen"*

    Die **\Views\Store\Index.cshtml** Vorlagendatei Ansicht erstellt und geöffnet wird. Anhand der Informationen bereitgestellt, um die **Ansicht hinzufügen** Dialogfeld im letzten Schritt die Ansicht, die Vorlage erwarten, dass ein **StoreIndexViewModel** -Instanz wie die Daten zum Generieren einer HTML-Antwort. Sie werden feststellen, dass die Vorlage übernimmt eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C# geschrieben.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Aufgabe 5: Aktualisieren der Vorlage anzeigen

In dieser Aufgabe aktualisieren Sie die Vorlage anzeigen, die in der vorhergehenden Aufgabe zum Abrufen der Anzahl von Genres und deren Namen auf der Seite erstellt.

> [!NOTE]
> Verwenden Sie @ Syntax (so genannte &quot;Codeausdrücke&quot;) zum Ausführen von Code in der Vorlage anzeigen.


1. In der **Index.cshtml** Datei, in der **Store** Ordner, ersetzen Sie den Code durch Folgendes:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > Sobald Sie fertig sind, geben Sie den Zeitraum nach dem Begriff **Modell**, Visual Studio Intellisense zeigt eine Liste der möglichen Eigenschaften und Methoden zur Auswahl.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Abrufen von Modelleigenschaften und für Methoden mit Visual Studio IntelliSense*
    > 
    > Die **Modell** Eigenschaftenverweise der **StoreIndexViewModel** Objekt, das der Controller an der Vorlage für die Ansicht übergeben. Dies bedeutet, dass Sie alle Daten auf dem Controller an der Vorlage anzeigen über übergeben zugreifen können die **Modell** -Eigenschaft, und formatieren Sie ihn in eine entsprechende HTML-Antwort innerhalb der Vorlage anzeigen.
    > 
    > Können Sie wählen Sie einfach die **NumberOfGenres** Eigenschaft von Intellisense auflisten, anstatt ihn ein, und klicken Sie dann diese Eingabe automatisch zu vervollständigen wird es durch Drücken der **Tab-Taste**.
2. Schleife in der Liste der "Genre" **StoreIndexViewModel** , und erstellen Sie ein HTML  **&lt;Ul&gt;**  Liste mit einer **Foreach** Schleife.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie **/speichern**. Sehen Sie die Liste von Genres übergeben der **StoreIndexViewModel** -Objekt aus der **StoreController** der Vorlage anzeigen.

    ![Anzeigen einer Liste von Genres Ansicht](aspnet-mvc-4-fundamentals/_static/image26.png "anzeigen, die eine Liste von Genres anzeigen")

    *Anzeigen einer Liste von Genres anzeigen*
4. Schließen Sie den Browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Übung 6: Verwenden von Parametern in der Ansicht

Übung 3 haben Sie gelernt, wie Parameter an den Controller übergeben wird. In dieser Übung erfahren Sie, wie diese Parameter in der Vorlage anzeigen verwendet werden. Zu diesem Zweck werden Sie zuerst mit Modellklassen bekannt gemacht werden, die Sie zum Verwalten Ihrer Daten und Domänenlogik unterstützt. Darüber hinaus erfahren Sie, wie Links zu Seiten in der ASP.NET MVC-Anwendung erstellt wird, unabhängig von der Dinge URL-Pfade, die Codierung.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Aufgabe 1: Hinzufügen von Modellklassen

Im Gegensatz zu ViewModels, die erstellt werden, um Informationen vom Controller an die Ansicht zu übergeben, werden Modellklassen zum enthalten und Verwalten von Daten und Domänenlogik erstellt. In dieser Aufgabe fügen Sie zwei Modellklassen zur Darstellung dieser Konzepte: **"Genre"** und **Album**.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**
2. In der **Datei** Menü wählen **geöffneten Projekt**. Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex06 UsingParametersInView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**. Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Hinzufügen einer **"Genre"** Modellschemas. Zu diesem Zweck Maustaste die **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *Genre.cs*, klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")

    *Ein neues Element hinzufügen*

    ![Hinzufügen von "Genre" Modellklasse](aspnet-mvc-4-fundamentals/_static/image28.png "Modellklasse "Genre" hinzufügen")

    *Fügen Sie "Genre"-Modell-Klasse*
4. Hinzufügen einer **Namen** Eigenschaft der Klasse "Genre". Fügen Sie hierzu den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 "Genre"*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Befolgen die gleiche Vorgehensweise wie zuvor, Hinzufügen einer **Album** Klasse. Zu diesem Zweck Maustaste die **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *Album.cs*, klicken Sie dann auf **hinzufügen**.
6. Fügen Sie zwei Eigenschaften zur Klasse Album: **"Genre"** und **Titel**. Fügen Sie hierzu den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 Album*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Aufgabe 2: Hinzufügen einer StoreBrowseViewModel

Ein **StoreBrowseViewModel** wird in dieser Aufgabe verwendet werden, um die Alben anzeigen, die einem ausgewählten "Genre" entsprechen. In dieser Aufgabe wird diese Klasse erstellen und fügen Sie dann zwei Eigenschaften zum Behandeln der **"Genre"** und seine **Album**der Liste.

1. Hinzufügen einer **StoreBrowseViewModel** Klasse. Zu diesem Zweck Maustaste die **ViewModels** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option. Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *StoreBrowseViewModel.cs*, klicken Sie dann auf **hinzufügen**.
2. Hinzufügen eines Verweises auf die Modelle in **StoreBrowseViewModel** Klasse. Fügen Sie hierzu die folgenden Namespace verwenden:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Zwei Eigenschaften hinzufügen **StoreBrowseViewModel** Klasse: **"Genre"** und **Alben**. Fügen Sie hierzu den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > Was ist **Liste&lt;Album&gt;**  ?: dieser Definition wird mithilfe der **Liste&lt;T&gt;**  Typ, in dem **T** schränkt die auf welche Elemente dieses Typs **Liste** gehören, in diesem Fall **Album** (oder eines seiner Nachfolger).
    > 
    > Diese Fähigkeit zum Entwerfen von Klassen und Methoden, die die Angabe von einem oder mehreren Typen verzögern, bis die Klasse oder Methode deklariert und instanziiert, indem Clientcode ein Feature von C#-Sprache ist namens **Generika**.
    > 
    > **Liste&lt;T&gt;**  entspricht dem generischen der **ArrayList** geben und steht in den **System.Collections.Generic** Namespace. Einer der Vorteile der Verwendung von **Generika** ist, da der Typ angegeben wird, Sie nicht überprüfen Vorgänge wie die Elemente in die Umwandlung von Typen kümmern müssen **Album** wie mit einer **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Aufgabe 3: verwenden das neue ViewModel in der StoreController

In dieser Aufgabe ändern Sie die **StoreController**des **Durchsuchen** und **Details** Aktionsmethoden der neuen **StoreBrowseViewModel** .

1. Hinzufügen eines Verweises auf den Ordner Models in **StoreController** Klasse. Zu diesem Zweck, erweitern Sie die **Controller** Ordner in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** Klasse. Fügen Sie dann den folgenden Code hinzu:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Ersetzen Sie die **Durchsuchen** Aktionsmethode mit den **StoreViewBrowseController** Klasse. Erstellen Sie eine "Genre" und zwei neue Alben Objekte mit dummy-Daten (in der nächsten praktische Übungseinheit verwenden Sie echte Daten von einer Datenbank). Ersetzen Sie hierzu die **Durchsuchen** -Methode durch folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Ersetzen Sie die **Details** Aktionsmethode mit den **StoreViewBrowseController** Klasse. Erstellen Sie ein neues **Album** zu zurückzugebenden Objekt zu den **Ansicht**. Ersetzen Sie hierzu die **Details** -Methode durch folgenden Code:

    (Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Aufgabe 4: Hinzufügen eines Sicht zum Durchsuchen einer Vorlage

In dieser Aufgabe fügen Sie eine **Durchsuchen** Anzeigen von Alben für einen bestimmten "Genre" gefunden.

1. Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht hinzufügen** Dialogfeld kennt die **ViewModel** -Klasse. Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.
2. Hinzufügen einer **Durchsuchen** anzeigen. Zu diesem Zweck mit der Maustaste in die **Durchsuchen** Aktionsmethode von der **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.
3. In der **Ansicht hinzufügen** Dialogfeld Vergewissern Sie sich, dass der Ansichtsname ist **Durchsuchen**. Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **StoreBrowseViewModel** aus der **Modellschemas** Dropdownliste. Lassen Sie die anderen Felder, die Standardwerte. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen einer Ansicht durchsuchen](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer Ansicht durchsuchen")

    *Hinzufügen einer Ansicht durchsuchen*
4. Ändern der **Browse.cshtml** zum Anzeigen von Informationen für die "Genre", den Zugriff auf die **StoreBrowseViewModel** -Objekt, das an die Ansichtenvorlage übergeben wird. Ersetzen Sie dazu den Inhalt durch Folgendes: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **Durchsuchen** Methode ruft Alben aus der **Durchsuchen** Methodenaktion.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum   **/speichern/suchen? "Genre" = Disco** , stellen Sie sicher, dass die Aktion zwei Alben zurückgibt.

    ![Disco Alben Store durchsuchen](aspnet-mvc-4-fundamentals/_static/image30.png "Disco Alben Store durchsuchen")

    *Disco Alben Store durchsuchen*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Aufgabe 6: Anzeigen von Informationen zu bestimmten Album

In diesem Schritt implementieren Sie die **Store/Detail-** können Sie Informationen zu einem bestimmten Album anzeigen. In dieser praktischen Übungseinheit alles, was Sie über das Album zeigt ist bereits Bestandteil der **Ansicht** Vorlage. In diesem Fall erstellen Sie anstelle einer **StoreDetailsViewModel** -Klasse, verwenden Sie die aktuelle **StoreBrowseViewModel** Vorlage Album an sie übergibt.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Fügen Sie einen neuen **Details** anzeigen für die **StoreController**des **Details** Aktionsmethode. Zu diesem Zweck Maustaste die **Details** Methode in der **StoreController** Klasse, und klicken Sie auf **Ansicht hinzufügen**.
2. In der **Ansicht hinzufügen** Dialogfeld, überprüfen Sie, ob die **Sichtname** ist **Details**. Überprüfen der **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **Album** aus der **Modellschemas** Dropdownliste aus. Lassen Sie die anderen Felder, die Standardwerte. Klicken Sie anschließend auf **Hinzufügen**. Dies erstellt und öffnet eine **\Views\Store\Details.cshtml** Datei.

    ![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "eine Detailansicht hinzufügen")

    *Hinzufügen einer Detailansicht*
3. Ändern der **Details.cshtml** Datei auf dem Album Informationen anzuzeigen, den Zugriff auf die **Album** -Objekt, das an die Ansichtenvorlage übergeben wird. Ersetzen Sie dazu den Inhalt durch Folgendes: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **Details** Sicht Ruft Informationen zu den Album aus der **Details Aktion** Methode.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt gestartet wird, der **Home** Seite. Ändern Sie die URL zum **/Store/Details/5** das Album Informationen überprüfen.

    ![Durchsuchen von Alben Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Alben Detail durchsuchen")

    *Durchsuchen des Albums Detail*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Aufgabe 8: Hinzufügen von Verknüpfungen zwischen Seiten

In dieser Aufgabe fügen Sie einen Link in der Ansicht speichern, haben einen Link in jedem Namen "Genre" in das entsprechende **/Store/durchsuchen** URL. Wenn Sie z. B. auf eine "Genre", klicken auf diese Weise **Disco**, wird es navigieren Sie zu **/Store/durchsuchen? "Genre" = Disco** URL.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Update der **Index** Seite, um einen Link zum Hinzufügen der **Durchsuchen** Seite. Klicken Sie hierzu in der **Projektmappen-Explorer** erweitern Sie die **Ansichten** Ordner, und dann die **Store** Ordner und doppelklicken Sie auf die **Index.cshtml** Seite.
2. Fügen Sie einen Link auf die Ansicht durchsuchen, der angibt, die "Genre" ausgewählt. Ersetzen Sie hierzu die folgenden hervorgehobenen Code innerhalb der  **&lt;li&gt;**  Tags: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > ein anderer Ansatz würde direkt auf der Seite mit einem Code wie den folgenden verknüpfen:
    > 
    > &lt;ein Href =&quot;/Store/durchsuchen? "Genre" =@genreName&quot;&gt;@genreName &lt; /a&gt;
    > 
    > Obwohl dieser Ansatz funktioniert, hängt es eine hartcodierte Zeichenfolge ein. Wenn Sie später den Controller umbenennen, müssen Sie diese Anweisung manuell ändern. Eine bessere Alternative ist die Verwendung einer **HTML-Hilfsobjekt** Methode. ASP.NET MVC umfasst eine HTML-Hilfsobjekt-Methode die für Aufgaben verfügbar ist. Die **Html.ActionLink()** Hilfsmethode erleichtert das Erstellen von HTML  **&lt;eine&gt;**  Links, die sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.
    > 
    > Htlm.ActionLink weist mehrere Überladungen. In dieser Übung werden Sie eine verwenden, die drei Parameter akzeptiert:
    > 
    > 1. Text des Links, der den Namen "Genre" angezeigt wird
    > 2. Controller Aktionsname (**Durchsuchen**)
    > 3. Weiterleiten von Parameterwerten, die beide den Namen angeben (**"Genre"**) und dem Wert (**"Genre" Namen**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Aufgabe 9: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass jede "Genre", mit einem Link zu der entsprechenden angezeigt wird **/Store/durchsuchen** URL.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/speichern** um sicherzustellen, dass jedes Genres an die entsprechende links **/Store/durchsuchen** URL.

    ![Durchsuchen von Genres mit Links auf der Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "durchsuchen Genres mit Links auf der Seite "Durchsuchen"")

    *Durchsuchen von Genres mit Links auf der Seite "Durchsuchen"*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Aufgabe 10 - mit dynamischen ViewModel Auflistung zum Übergeben von Werten

In dieser Aufgabe erfahren Sie, eine einfache und leistungsstarke Methode zum Übergeben von Werten zwischen dem Controller und die Ansicht, ohne Änderungen im Modell. ASP.NET MVC 4 stellt die Auflistung &quot;ViewModel&quot;, dynamische ausnahmslos zugewiesen und innerhalb der Controller und Ansichten ebenfalls zugegriffen werden können.

Verwenden Sie jetzt die ViewBag dynamische Auflistung zum Übergeben von &quot; **mit Stern Genres** &quot; auf dem Controller auf die Ansicht. Die Columnstore-Index-Ansicht wird der Zugriff auf **ViewModel** und die Informationen anzuzeigen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreController.cs** und Ändern von **Index** Methode zum Erstellen einer Liste von Genres in ViewModel Auflistung markiert:


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Sie können auch die Syntax **ViewBag [&quot;Starred&quot;]** Zugriff auf die Eigenschaften.
2. Das Sternsymbol  **&quot;starred.png&quot;**  dient in der **Source\Assets\Images** Ordner dieser Anleitung. Um die Anwendung hinzugefügt haben, ziehen Sie ihren Inhalt aus einem **Windows Explorer** Fenster in der **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:

    ![Stern Bild hinzufügen, der Projektmappe](aspnet-mvc-4-fundamentals/_static/image34.png "Stern Bild der Projektmappe hinzufügen")

    *Stern Bild hinzufügen zur Projektmappe*
3. Öffnen Sie die Ansicht **Store/Index.cshtml** und ändern Sie den Inhalt. Sie liest die &quot;mit Stern&quot; Eigenschaft in der **ViewBag** -Auflistung, und gefragt, ob der Name der aktuellen "Genre" in der Liste befindet. In diesem Fall zeigt das einem Sternsymbol rechts auf den Link "Genre".
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Aufgabe 11: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass die mit Sternchen Genres einem Sternsymbol angezeigt.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt gestartet wird, der **Home** Seite. Ändern Sie die URL zum **/speichern** überprüfen, ob jede ausgewählte "Genre" respektieren Bezeichnung verfügt:

    ![Durchsuchen von Genres mit Elementen mit Sternchen](aspnet-mvc-4-fundamentals/_static/image35.png "Genres mit Elementen mit Sternchen durchsuchen")

    *Durchsuchen von Genres mit mit Sternchen Elementen*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Übung 7: Lap um neue ASP.NET MVC 4-Vorlage

In dieser Übung untersuchen Sie die hier enthaltenen Erweiterungen der ASP.NET MVC 4-Projektvorlagen erstellen einem Blick die wichtigsten Features der neuen Vorlage.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Aufgabe 1: Untersuchen der ASP.NET MVC 4 Internet Application-Vorlage

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung**. **Namen** des Projekts *MusicStore* und Aktualisieren der **Projektmappenname** auf *beginnen*, dann wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**.

    ![Erstellen ein neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image36.png "erstellen ein neues ASP.NET MVC 4-Projekt")

    *Erstellen ein neues ASP.NET MVC 4-Projekt*
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**. Beachten Sie, dass Sie das Ansichtsmodul Razor oder ASPX auswählen können.

    ![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt. Das Ziel besteht darin, die Anzahl von Zeichen und Tastatureingaben in einer Datei erforderlich, eine schnelle und Fluid Codierung Workflow aktivieren zu minimieren. Razor nutzt die vorhandenen C#/VB (oder andere) Sprachkenntnisse und übermittelt Sie eine Vorlage Markup-Syntax, die einen awesome HTML-Konstruktion-Workflow ermöglicht.
4. Drücken Sie **F5** führen Sie die Projektmappe, und die erneuerte Vorlage angezeigt. Sie können die folgenden Funktionen Auschecken:

    1. **Modernes Vorlagen**

        Die Vorlagen haben erneuert wurde, mehr Modern aussehende Arten bereitstellen.

        ![Vorlagen für ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled Vorlagen")

        *ASP.NET MVC 4 restyled Vorlagen*
    2. **Adaptives Rendering**

        Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout dynamisch an die Größe des neuen Fensters anpasst. Diese Vorlagen mithilfe der adaptiven Renderingtechnik um ordnungsgemäß auf Desktop- und mobile Plattformen ohne Anpassung zu rendern.

        ![ASP.NET MVC 4-Projektvorlage in anderen Browser Größen](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in anderen Browser Größen")

        *ASP.NET MVC 4-Projektvorlage in anderen Browser Größen*
5. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
6. Nun können Sie in der Lage, untersuchen die Projektmappe, und sehen Sie sich einige der neuen Funktionen, die in der Projektvorlage von ASP.NET MVC 4 eingeführt.

    ![ASP.NET MVC 4-Internet-Anwendung –--Projektvorlage](aspnet-mvc-4-fundamentals/_static/image40.png "der Projektvorlage Internet ASP.NET MVC 4")

    *Die ASP.NET MVC 4 Internet Application-Projektvorlage*

    1. **HTML5-markup**

        Durchsuchen Sie die Ansichten der Vorlagen so ermitteln Sie das neue Design Markup, z. B. open **About.cshtml** in anzeigen **Home** Ordner.

        ![Neue Vorlage für die Verwendung von Razor und HTML5-Markups](aspnet-mvc-4-fundamentals/_static/image41.png "neue Vorlage für die Verwendung von Razor und HTML5-Markups")

        *Neue Vorlage für die Verwendung von Razor und HTML5-Markups*
    2. **JavaScript-Bibliotheken enthalten**

        1. **jQuery**: jQuery vereinfacht die HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Aktivitäten.
        2. **jQuery UI**: Diese Bibliothek bietet Abstraktionen für die Low-Level-Interaktion und Animation, erweiterte Auswirkungen und Design beeinflusst Widgets, baut auf dem jQuery JavaScript-Bibliothek.

            > [!NOTE]
            > Sie können erfahren, jQuery und jQuery UI in [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
        3. **KnockoutJS**: der ASP.NET MVC 4-Standardvorlage enthält jetzt **KnockoutJS**, ein MVVM JavaScript-Framework, das umfangreiche und extrem reaktionsschnellen Webanwendungen, die mit JavaScript und HTML erstellen kann. Wie werden in ASP.NET MVC 3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.

            > [!NOTE]
            > Sie erhalten weitere Informationen zur KnockOutJS Library unter diesem Link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
        4. **Modernizr**: Diese Bibliothek automatisch ausgeführt, macht Ihre Website mit älteren Browsern kompatibel, bei Verwendung von HTML5- und CSS3-Technologien.

            > [!NOTE]
            > Sie erhalten weitere Informationen zu Modernizr-Bibliothek unter diesem Link: [http://www.modernizr.com/](http://www.modernizr.com/).
    3. **In der Projektmappe enthaltenen SimpleMembership**

        SimpleMembership wurde als Ersatz für das vorherige ASP.NET Rollen- und Mitgliedschaftseinstellungen anbietersystem entwickelt. Er verfügt über viele neue Features, die für den Entwickler sichere Webseiten in ein flexibleres erleichtern.

        Die Internet-Vorlage bereits eingerichtet hat einige Punkte, die SimpleMembership integrieren, z. B. die AccountController OAuthWebSecurity (für OAuth kontoregistrierung, Login, Verwaltung, usw.) und -Websicherheit verwenden vorbereitet wird.

        ![SimpleMembership in der Projektmappe enthaltenen](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership in der Projektmappe enthaltenen")

        *SimpleMembership in der Projektmappe enthaltenen*

        > [!NOTE]
        > Weitere Informationen finden Sie Informationen zu [OAuthWebSecurity](https://msdn.microsoft.com/en-us/library/jj158393(v=vs.111).aspx) in MSDN.

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben Sie gelernt, dass die Grundlagen von ASP.NET MVC:

- Kernelemente des MVC-Anwendung und ihre Interaktion
- Vorgehensweise: Erstellen einer ASP.NET MVC-Anwendung
- Zum Hinzufügen und Konfigurieren von Domänencontrollern, um Parameter zu behandeln, übergeben die URL und Abfragezeichenfolge
- Zum Hinzufügen einer Layoutseite master zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten und Ansichtenvorlage zum Anzeigen von HTML-Inhalte zu verbessern
- Wie das ViewModel-Muster zur Weitergabe der Eigenschaften der Vorlage anzeigen dynamischer Informationen anzuzeigen
- Verwendung von Parametern übergeben auf Domänencontroller in der Vorlage anzeigen
- Zum Hinzufügen von Links zu Seiten in der ASP.NET MVC-Anwendung
- Gewusst wie: Hinzufügen und Verwenden von dynamischen Eigenschaften in einer Ansicht
- Die Verbesserungen in ASP.NET MVC 4-Projektvorlagen

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-fundamentals/_static/image50.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](aspnet-mvc-4-fundamentals/_static/image51.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-fundamentals/_static/image52.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-fundamentals/_static/image53.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image54.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image55.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-fundamentals/_static/image62.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

    - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
    - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
    - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
    - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

    ![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Zielverbindungszeichenfolge konfigurieren")

    *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-fundamentals/_static/image66.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

    ![Anwendung in Windows Azure veröffentlicht](aspnet-mvc-4-fundamentals/_static/image68.png "Veröffentlichung der Anwendung in Windows Azure")

    *Anwendung, die auf Windows Azure veröffentlicht werden soll*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-fundamentals/_static/image70.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-fundamentals/_static/image71.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-fundamentals/_static/image72.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-fundamentals/_static/image73.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-fundamentals/_static/image74.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
