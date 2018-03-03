---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4-Hilfsprogrammen, Formulare und Validierung | Microsoft Docs
author: rick-anderson
description: "In ASP.NET MVC 4-Modelle und Daten Zugriff praktische Übungseinheit haben Sie laden und Anzeigen von Daten aus der Datenbank wurde. In dieser praktischen Übungseinheit werden Sie zum Hinzufügen der..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 243db3708ac4311d423c4c137f503f072f5553e6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4-Hilfsprogrammen, Formulare und Überprüfung

Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](https://aka.ms/webcamps-training-kit)

In **ASP.NET MVC 4-Modellen und Datenzugriff** praktische Übungseinheit, Sie wurden laden und Anzeigen von Daten aus der Datenbank. In dieser praktischen Übungseinheit, fügen Sie auf der **Music Store** Anwendung die Möglichkeit, diese Daten zu bearbeiten.

Mit diesem Ziel Denken Sie daran erstellen Sie zuerst den Controller, der die Aktionen erstellen, lesen, aktualisieren und löschen (CRUD) von Alben unterstützt. Generieren Sie eine Indexansicht Vorlage nutzen ASP.NETs Gerüstbau-Funktion auf die Alben-Eigenschaften in einer HTML-Tabelle anzuzeigen. Um die Ansicht zu verbessern, fügen Sie einer benutzerdefinierten HTML-Hilfsobjekt, der eine lange Beschreibung abgeschnitten wird.

Danach fügen Sie, dass die bearbeiten und Erstellen von Sichten, mit denen Sie die Alben in der Datenbank mit Hilfe des Form-Elemente wie Dropdownlisten ändern.

Abschließend können Sie Benutzer, der ein Album löschen und auch Sie hindert sie falsche Daten validieren von Eingaben eingeben.

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**. Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC-Grundlagen** praktische Übungseinheit.

Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.

> [!NOTE]
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für diese Übung finden Sie unter [ASP.NET MVC 4-Hilfsprogrammen, Formulare und Validierung](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen Sie einen Controller zur Unterstützung von CRUD-Vorgänge
- Generieren Sie einen Index-Ansicht zum Anzeigen von Eigenschaften der Entität in einer HTML-Tabelle
- Hinzufügen einer benutzerdefinierten HTML-Hilfsmethoden
- Erstellen und Anpassen einer Ansicht bearbeiten
- Unterscheiden Sie zwischen Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren
- Hinzufügen und Anpassen einer Create View
- Behandeln Sie das Löschen einer Entität
- Validieren von Benutzereingaben

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang B: mithilfe von Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Die folgenden Übungen bilden diese praktische Übungseinheit:

1. [Der Speicher-Manager-Controller und die Indexansicht erstellen](#Exercise1)
2. [Hinzufügen von HTML-Hilfsobjekt](#Exercise2)
3. [Erstellen die Bearbeitungsansicht](#Exercise3)
4. [Hinzufügen einer Ansicht erstellen](#Exercise4)
5. [Behandlung von löschen](#Exercise5)
6. [Hinzufügen der Validierung](#Exercise6)
7. [Mithilfe von unaufdringliche jQuery auf Clientseite](#Exercise7)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Übung 1: Erstellen der Speicher-Manager-Controller und die Indexansicht

In dieser Übung erfahren Sie zum Erstellen eines neuen Controllers CRUD-Vorgänge zu unterstützen, passen dessen Index-Aktion-Methode, um eine Liste von Alben aus der Datenbank und schließlich generiert eine Indexansicht Vorlage nutzen ASP.NETs Gerüstbau zurückzugeben die Funktion auf die Alben-Eigenschaften in einer HTML-Tabelle anzuzeigen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Aufgabe 1: Erstellen der StoreManagerController

In dieser Aufgabe erstellen Sie einen neuen Domänencontroller aufgerufen **StoreManagerController** CRUD-Vorgänge zu unterstützen.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-CreatingTheStoreManagerController/Begin/** Ordner.

    1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Fügen Sie einen neuen Domänencontroller hinzu. Zu diesem Zweck Maustaste die **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann die **Controller** Befehl. Ändern der **Controller** **Namen** auf **StoreManagerController** und stellen Sie sicher, dass die Option **MVC-Controller mit leeren Lese-/schreibaktionen**ausgewählt ist. Klicken Sie auf **Hinzufügen**.

    ![Dialogfeld "Controller hinzufügen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Controller "hinzufügen"")

    *Controller-Dialogfeld "hinzufügen"*

    Es wird eine neue Domänencontroller-Klasse generiert. Da Sie zum Hinzufügen von Aktionen für Lese-/Schreibzugriff Stubmethoden für diese angegeben werden häufig verwendete CRUD-Aktionen mit TODO-Kommentare ausgefüllt wird, fragt nach, die anwendungsspezifische Logik einschließen erstellt.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Aufgabe 2: Anpassen der StoreManager-Index

In diesem Schritt passen Sie die Aktionsmethode StoreManager Index, um eine Ansicht mit der Liste der Alben aus der Datenbank zurückgegeben.

1. Fügen Sie die folgenden, in der Klasse StoreManagerController *mit* Direktiven.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 mit MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Hinzufügen eines Felds auf die **StoreManagerController** zum Speichern von einer Instanz von **MusicStoreEntities.**

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementieren Sie die Aktion StoreManagerController Index, um eine Ansicht mit der Liste der Alben zurückzugeben.

    Der Controller aktionslogik werden ähnelt der StoreController Index-Aktion, die zuvor geschrieben. Verwenden Sie LINQ, um alle Alben, einschließlich Informationen zu "Genre" und Interpreten für die Anzeige abzurufen.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 StoreManagerController Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Aufgabe 3: erstellen die Indexansicht

In dieser Aufgabe erstellen Sie die Indexansicht-Vorlage zum Anzeigen der Liste von Alben zurückgegebenes der **StoreManager** Controller.

1. Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht-Dialogfeld hinzufügen** kennt die **Album** -Klasse. Wählen Sie **erstellen | Erstellen von MvcMusicStore** zum Erstellen des Projekts.
2. Mit der rechten Maustaste innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.

    ![Fügen Sie die Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ansicht hinzufügen")

    *Hinzufügen einer Ansicht von innerhalb der Index-Methode*
3. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **Index**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdownliste aus. Wählen Sie **Liste** aus der **Gerüst Vorlage** Dropdownliste aus. Lassen Sie die **Ansichtsmodul** auf **Razor** und die anderen Felder mit ihren Wert, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Indexansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Hinzufügen der Indexansicht für einen")

    *Hinzufügen der Indexansicht für einen*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Aufgabe 4: Anpassen das Gerüst für die Indexansicht

In diesem Schritt passen Sie die einfache Ansicht-Vorlage, die mit ASP.NET MVC-Gerüstbau-Funktion so, dass sie die gewünschten Felder anzeigen, erstellt.

> [!NOTE]
> Die **Gerüstbau** -Unterstützung in ASP.NET MVC generiert eine einfache Ansichtenvorlage, die alle Felder im Modell Album aufgelistet. **Gerüstbau** bietet eine schnelle Möglichkeit zum Einstieg mit einer stark typisierten Ansicht: anstatt die Sicht Vorlage manuell zu schreiben, schnell Gerüstbau eine Standardvorlage generiert und dann können Sie den generierten Code ändern.


1. Überprüfen Sie den Code erstellt. Die generierten Liste von Feldern werden Teil des folgenden HTML Tabelle mit **Gerüstbau** zum Anzeigen von Tabellendaten zu verwenden ist.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Ersetzen Sie die  **&lt;Tabelle&gt;**  Code durch den folgenden Code zur ausschließlichen Anzeige der **"Genre"**, **Interpreten**, **Albumtitel**, und **Preis** Felder. Dadurch werden gelöscht, die **AlbumId** und **Album Art URL** Spalten. Es ändert sich auch, GenreId und ArtistId Spalten zum Anzeigen ihrer verknüpfte Klasseneigenschaften eines **Artist.Name** und **Genre.Name**, und entfernt die **Details** Link.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Ändern Sie die folgenden Beschreibungen.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **Index** Vorlage anzeigen Zeigt eine Liste von Alben gemäß den Entwurf der vorigen Schritte.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager** zu überprüfen, ob eine Liste von Alben angezeigt wird, deren **Titel**, **Interpreten** und **"Genre"**.

    ![Durchsuchen die Liste der Alben](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Durchsuchen der Liste von Alben")

    *Durchsuchen die Liste der Alben*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Übung 2: Hinzufügen von HTML-Hilfsobjekt

Die Indexseite StoreManager hat ein mögliches Problem: Titel und den Namen der Interpret-Eigenschaften können sowohl werden lang genug ist, deaktivieren Sie die Formatierung der Tabelle ausgelöst werden soll. In dieser Übung erfahren Sie, wie Sie eine benutzerdefinierte HTML-Hilfsobjekt, um das Abschneiden von Text hinzufügen.

In der folgenden Abbildung sehen Sie, wie das Format aufgrund der Länge des Texts geändert wird, wenn Sie eine kleines Browsergröße verwenden.

![Durchsuchen die Liste der Alben mit nicht abgeschnittener Text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Text Durchsuchen der Liste von Alben mit nicht abgeschnitten")

*Durchsuchen die Liste der Alben mit abgeschnittener nicht text*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Aufgabe 1 – Erweitern von HTML-Hilfsobjekt.

In dieser Aufgabe fügen Sie eine neue Methode **Truncate** auf die **HTML** in ASP.NET MVC-Ansichten verfügbar gemachtes Objekt zu. Implementieren Sie zu diesem Zweck ein **Erweiterungsmethode** für die integrierte **System.Web.Mvc.HtmlHelper** von ASP.NET MVC bereitgestellte-Klasse.

> [!NOTE]
> Weitere Informationen zu **Erweiterungsmethoden**, besuchen Sie die in diesem Msdn-Artikel. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex2-AddingAnHTMLHelper/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen des StoreManager Index anzuzeigen. Zu diesem Zweck im Projektmappen-Explorer erweitern die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **Index.cshtml** Datei.
3. Fügen Sie den folgenden Code unter der  **@model**  zum Definieren der **Truncate** Hilfsmethode.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Aufgabe 2 - Abschneiden Text auf der Seite

In dieser Aufgabe verwenden Sie die **Truncate** Methode zum Abschneiden von Text in der Vorlage anzeigen.

1. Öffnen des StoreManager Index anzuzeigen. Zu diesem Zweck im Projektmappen-Explorer erweitern die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **Index.cshtml** Datei.
2. Ersetzen Sie die Zeilen, die veranschaulichen die **Namen Künstlers** und des Albums **Titel**. Ersetzen Sie hierzu die folgenden Zeilen ein.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **Index** Vorlage anzeigen abschneidet, Titel und Interpret-Name des Albums.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager** lange überprüfen Texte in der **Titel** und **Interpreten** Spalte abgeschnitten werden.

    ![Titel und Künstler Namen gekürzt](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Titel und Künstler Namen abgeschnitten")

    *Abgeschnittene Titel und Künstler Namen*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Übung 3: Erstellen der Bearbeitungsansicht

In dieser Übung erfahren Sie, zum Erstellen eines Formulars, um Speicher-Manager so bearbeiten Sie die Verwaltung zu ermöglichen. Durchsuchen sie die **/StoreManager/Edit/id** URL (**Id** wird die eindeutige Id des Albums bearbeiten), wodurch einen HTTP-GET-Aufruf an den Server.

Die Aktionsmethode Controller bearbeiten wird das entsprechende Album aus der Datenbank abrufen, erstellen Sie eine **StoreManagerViewModel** Objekt, das (zusammen mit der eine Liste von Künstler und Genres) zu kapseln, und übergeben Sie ihn dann, um eine Ansicht aus, um Rendern von HTML-Seite an dem Benutzer. Diese Seite enthält eine  **&lt;Formular&gt;**  Element mit Textfelder und Dropdownlisten zur Bearbeitung der Eigenschaften Album.

Sobald der Benutzer klickt und die Formularwerte Album aktualisiert der **speichern** Schaltfläche, die Änderungen werden gesendet, über HTTP-POST aufgerufen, **/StoreManager/Edit/id**. Obwohl die URL wie in den letzten Aufruf unverändert bleibt, ASP.NET MVC identifiziert, diesmal, es ist ein HTTP-POST und aus diesem Grund führt eine andere Methode des bearbeiten-Aktion (eine Angabe **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Aufgabe 1 – Implementieren der HTTP-GET bearbeiten Action-Methode

In dieser Aufgabe wird der HTTP-GET-Version der Aktionsmethode bearbeiten zum Abrufen der entsprechenden Album aus der Datenbank sowie eine Liste aller Genres und Künstler implementiert werden. Es wird verpackt diese Daten in der **StoreManagerViewModel** Objekt definiert, die im letzten Schritt, der dann an eine Ansichtenvorlage zum Rendern der Antwort mit übergeben wird.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex3-CreatingTheEditView/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **StoreManagerController** Klasse. Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie die **HTTP-GET bearbeiten** Aktionsmethode mit den folgenden Code zum Abrufen der entsprechenden **Album** als auch die **Genres** und **Künstler**aufgeführt.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-GET*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Verwenden Sie **System.Web.Mvc** **SelectList** für Künstler und Genres statt der **System.Collections.Generic** Liste.
    > 
    > **SelectList** ist eine sauberere Methode zum Auffüllen von HTML-Dropdownlisten und verwalten z. B. die aktuelle Auswahl. Instanziieren und später zum Einrichten dieser ViewModel-Objekte in der Controlleraktion treffen cleaner Szenario Formular bearbeiten.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Aufgabe 2: Erstellen der Bearbeitungsansicht

In dieser Aufgabe erstellen Sie eine Vorlage bearbeiten anzeigen, die später die Albumeigenschaften angezeigt werden.

1. Erstellen Sie die Bearbeitungsansicht. Zu diesem Zweck Maustaste innerhalb der **bearbeiten** Aktionsmethode, und wählen **Ansicht hinzufügen**.
2. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **bearbeiten**. Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Datenklasse anzeigen** Dropdown-Menü. Wählen Sie **bearbeiten** aus der **Gerüst Vorlage** Dropdownliste aus. Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "eine Bearbeitungsansicht hinzufügen")

    *Hinzufügen einer Bearbeitungsansicht*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt die Eigenschaften-Werte für das Album als Parameter übergeben.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1** um sicherzustellen, dass die Eigenschaften-Werte für das übergeben Album angezeigt werden.

    ![Durchsuchen des Albums Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "des Albums Bearbeitungsansicht durchsuchen")

    *Durchsuchen des Albums Bearbeitungsansicht*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Aufgabe 4: Implementieren von Dropdownlisten in der Vorlage Album-Editor

In dieser Aufgabe fügen Sie Dropdownlisten der Vorlage anzeigen, die in der vorhergehenden Aufgabe erstellt hinzu, damit der Benutzer aus einer Liste von Künstler und Genres auswählen kann.

1. Alle ersetzen die **Album** Fieldset-Code durch Folgendes:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Ein **Html.DropDownList** Helper Dropdownlisten für die Auswahl Künstler und Genres Rendern hinzugefügt wurde. Die Parameter zu übergeben, um **Html.DropDownList** sind:
    > 
    > 1. Der Name des Formularfelds (**&quot;ArtistId&quot;**).
    > 2. Die **SelectList** der Werte für die Dropdownliste aus.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt Dropdowns statt Interpreten und ID "Genre" Textfelder.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1** zu überprüfen, ob sie Dropdownlisten statt Interpreten und ID "Genre" Text-Felder angezeigt.

    ![Durchsuchen des Albums Ansicht bearbeiten mit Dropdownlisten](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "durchsuchen Album Bearbeitungsansicht mit Dropdownlisten")

    *Durchsuchen des Albums Bearbeitungsansicht, diesmal mit Dropdownlisten*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Aufgabe 6: Implementieren der HTTP-POST bearbeiten Action-Methode

Nun, dass die Ansicht bearbeiten wie erwartet angezeigt wird, müssen Sie zur Implementierung der HTTP-POST-Aktion bearbeiten-Methode, um dem Album vorgenommenen Änderungen zu speichern.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** aus der **Controller** Ordner.
2. Ersetzen Sie **HTTP-POST bearbeiten** Aktion-Methode durch den folgenden Code (Beachten Sie, dass die Methode, die ersetzt werden müssen überladene Version, die zwei Parameter empfängt):

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-POST*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Diese Methode wird ausgeführt werden, wenn der Benutzer klickt auf die **speichern** Schaltfläche der Sicht und führt einen HTTP-POST für die Formularwerte zurück an den Server um dauerhaft in der Datenbank. Die Decorator **[HttpPost]** gibt an, dass die Methode für die HTTP-POST-Szenarien verwendet werden soll. Die Methode nimmt ein **Album** Objekt. ASP.NET MVC erstellt automatisch das Album-Objekt aus der gebuchten &lt;Formular&gt; Werte.
    > 
    > Die Methode führt folgende Schritte aus:
    > 
    > 1. Wenn das Modell gültig ist:
    > 
    >     1. Aktualisieren Sie den Album-Eintrag im Kontext als eine geänderte Objekt kennzeichnen.
    >     2. Die Änderungen zu speichern und an die Indexansicht umleiten.
    > 2. Wenn das Modell nicht gültig ist, füllt es das ViewBag-Objekt mit dem **GenreId** und **ArtistId**, wird die Ansicht mit dem empfangenen Album-Objekt, damit der Benutzer kann zurückgegeben führen Sie ein Update erforderliches.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager bearbeiten** Ansichtsseite speichert die aktualisierten Album Daten tatsächlich in der Datenbank.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1**. Ändern Sie den Titel Album in **laden** , und klicken Sie auf **speichern**. Stellen Sie sicher, dass die Albumtitel tatsächlich in der Liste der Alben geändert.

    ![Aktualisieren ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "ein Album aktualisieren")

    *Ein Album aktualisieren*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Übung 4: Hinzufügen einer Ansicht erstellen

Nun, dass der **StoreManagerController** unterstützt die **bearbeiten** Fähigkeit, in dieser Übung Sie erfahren, wie zum Hinzufügen einer Create View-Vorlage, damit Sie das Speichern von Managern neue Alben zur Anwendung hinzufügen.

Wie Sie mit der Bearbeitungsfunktion getan haben, implementieren Sie das Create-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:

1. Eine Aktionsmethode wird ein leeres Formular angezeigt, wenn der Speicher-Manager zum ersten Mal besuchen der **/StoreManager/Create** URL.
2. Eine zweite Aktionsmethode behandelt das Szenario, in dem der Speicher-Manager klickt der **speichern** Schaltfläche im Formular und sendet die Werte zurück, an die **/StoreManager/Create** URL als eine HTTP-POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Aufgabe 1: Erstellen von HTTP-GET-Aktionsmethode implementieren

In diesem Schritt implementieren Sie die HTTP-GET-Version von der Create Action-Methode zum Abrufen einer Liste aller Genres und Künstler, verpackt diese Daten in einem **StoreManagerViewModel** -Objekt, das dann an eine Ansichtenvorlage übergeben wird.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex4-AddingACreateView/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Open **StoreManagerController** Klasse. Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie die **erstellen** Aktion-Methode durch den folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex4 StoreManagerController HTTP-GET erstellen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Aufgabe 2: Hinzufügen der Ansicht erstellen

In dieser Aufgabe fügen Sie die Create View-Vorlage, die ein neues (leeres) Album Formular angezeigt werden.

1. Mit der rechten Maustaste innerhalb der **erstellen** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.
2. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **erstellen**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdown-Menü und **erstellen** aus der **Gerüst Vorlage** Dropdownliste aus. Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Erstellungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "hinzufügen-a-erstellen-view.png")

    *Hinzufügen der Ansicht erstellen*
3. Update der **GenreId** und **ArtistId** Felder, die eine Dropdown-Liste verwendet wird, wie unten dargestellt:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **erstellen** Ansichtsseite zeigt ein leeres Formular ein Album.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Create**. Stellen Sie sicher, dass ein leeres Formular zum Ausfüllen der Eigenschaften der neuen Album angezeigt wird.

    ![Erstellen Sie die Ansicht mit einem leeren Formular](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View mit einem leeren Formular")

    *Ansicht mit einem leeren Formular erstellen*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Aufgabe 4: Implementieren des HTTP-POST-Create Aktionsmethode

In diesem Schritt implementieren Sie die HTTP-POST-Version der Create-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **speichern** Schaltfläche. Die Methode sollte das neue Album in der Datenbank speichern.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** Klasse. Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie **Erstellen von HTTP-POST** Aktion-Methode durch den folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex4 StoreManagerController HTTP - POST Aktion erstellen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Die Erstellungsaktion ist ziemlich ähnelt der vorherigen bearbeiten-Aktionsmethode, aber statt der Festlegung des Objekts, geändert wurde, wird es zum Kontext hinzugefügt wird.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager erstellen** Ansichtsseite können Sie ein neues Album erstellen und leitet dann an die Indexansicht StoreManager.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Create**. Füllen Sie alle Felder des Formulars mit Daten für ein neues Album, wie Sie in der folgenden Abbildung:

    ![Erstellen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "ein Album erstellen")

    *Erstellen ein Album*
3. Stellen Sie sicher, dass Sie die Indexansicht StoreManager umgeleitet abrufen, die soeben erstellte neue Album enthält.

    ![Neues Album erstellt](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "neues Album erstellt")

    *Neues Album erstellt*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Übung 5: Behandlung von löschen

Die Möglichkeit, Alben löschen wurde noch nicht implementiert. Dies ist zum werden in dieser Übung. Wie vorher, implementieren Sie die Delete-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:

1. Eine Aktionsmethode wird eine Form der Bestätigung angezeigt.
2. Eine zweite Aktionsmethode verarbeitet der Übermittlung des Formulars

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Aufgabe 1 – Implementieren der HTTP-GET-Delete-Aktion-Methode

In dieser Aufgabe wird die HTTP-GET-Version von der Delete-Aktion-Methode zum Abrufen von Informationen für das Album implementiert werden.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex5-HandlingDeletion/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Open **StoreManagerController** Klasse. Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Die Löschaktion der Controller ist identisch mit der vorherigen Store Details Controlleraktion: anschließend fragt es die **Album** Objekt aus der Datenbank der **Id** bereitgestellt, die in der URL und gibt die entsprechende **Ansicht**. Ersetzen Sie hierzu die HTTP-GET **löschen** Aktion-Methode durch den folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex5 löschen Behandlung von HTTP-GET löschen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Mit der rechten Maustaste innerhalb der **löschen** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.
5. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **löschen**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdown-Menü. Wählen Sie **löschen** aus der **Gerüst Vorlage** Dropdownliste aus. Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Löschansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Hinzufügen einer Löschansicht")

    *Hinzufügen einer Löschansicht*
6. Die Vorlage "löschen" zeigt alle Felder aus dem Modell. Sie werden nur die Album Titel angezeigt. Ersetzen Sie dazu den Inhalt der Ansicht mit den folgenden Code:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **löschen** Ansichtsseite zeigt eine Form der Bestätigung löschen.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager**. Wählen Sie ein Album zu löschen, indem Sie auf **löschen** und stellen Sie sicher, dass die neue Ansicht hochgeladen wurde.

    ![Löschen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "ein Album löschen")

    *Löschen ein Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Aufgabe 3 – Implementieren der HTTP-POST-Delete-Aktion-Methode

In diesem Schritt implementieren Sie die HTTP-POST-Version der Delete-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **löschen** Schaltfläche. Die Methode sollte das Album in der Datenbank löschen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** Klasse. Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie **HTTP-POST löschen** Aktion-Methode durch den folgenden Code:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex5 löschen Behandlung von HTTP-POST löschen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager Delete** Ansichtsseite können Sie ein Album löschen und leitet dann an die Indexansicht StoreManager.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager**. Wählen Sie ein Album zu löschen, indem Sie auf **löschen.** Bestätigen Sie den Vorgang, indem Sie auf **löschen** Schaltfläche:

    ![Löschen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "ein Album löschen")

    *Löschen ein Album*
3. Stellen Sie sicher, dass Album gelöscht wurde, da sie nicht in erscheint die **Index** Seite.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Übung 6: Hinzufügen einer Validierung

Das Erstellen und Bearbeiten von Formulare, die erfüllt sein führen zurzeit keine Art von Validierung. Wenn der Benutzer ein erforderliches Feld leer oder Typ Buchstaben im Feld "Preis" verlässt, wird der erste Fehler erhalten Sie aus der Datenbank sein.

Sie können die Anwendung durch Hinzufügen von Datenanmerkungen zu Ihrer Modellklasse Validierung hinzugefügt. Datenanmerkungen ermöglichen es, beschreibt die gewünschten Regeln auf die Modelleigenschaften angewendet, und ASP.NET MVC übernimmt erzwingen und die entsprechende Meldung an Benutzer anzeigen.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Aufgabe 1: Hinzufügen von Datenanmerkungen

In dieser Aufgabe fügen Sie Datenanmerkungen dem Album-Modell, die die Seite "erstellen und Bearbeiten von" vornehmen, werden validierungsmeldungen gegebenenfalls angezeigt.

Für eine einfache Modellklasse, Hinzufügen einer Anmerkung Daten nur erfolgt durch Hinzufügen einer **mit** -Anweisung für **System.ComponentModel.DataAnnotation**, platzieren dann eine **[erforderlich]**Attribut in der entsprechenden Eigenschaften. Im folgende Beispiel würde machen die **Namen** Eigenschaft ein erforderliches Feld in der Ansicht.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Dies ist ein wenig komplexer in Fällen, wie diese Anwendung, in dem das Entity Data Model generiert wird. Wenn Sie direkt mit der Modellklassen Datenanmerkungen hinzugefügt haben, würden sie überschrieben, wenn Sie das Modell aus der Datenbank zu aktualisieren. Sie können stattdessen machen Metadaten partiellen Klassen ist vorhanden, um die Anmerkungen zu speichern und mit dem Modell verknüpft sind Klassen, mit der **[MetadataType]** Attribut.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex6-AddingValidation/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **Album.cs** aus der **Modelle** Ordner.
3. Ersetzen Sie **Album.cs** Inhalt mit der hervorgehobene Code, damit er wie folgt aussieht:

    > [!NOTE]
    > Die Zeile **[DisplayFormat(ConvertEmptyStringToNull=false)]** gibt an, dass leere Zeichenfolgen aus dem Modell nicht auf null konvertiert werden, wenn das Feld "Daten" in der Datenquelle aktualisiert wird. Diese Einstellung wird eine Ausnahme vermeiden, wenn das Entity Framework null-Werte für das Modell, weist bevor Datenanmerkung Felder überprüft.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex6 Album Metadaten Teilklasse*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Dies **Album** partielle Klasse verfügt über eine **MetadataType** Attribut verweist auf die **AlbumMetaData** Klasse für die Datenanmerkungen. Dies sind einige der Datenanmerkung Attribute, die Sie verwenden, um das Modell Album versehen:
    > 
    > - Erforderlich. – gibt an, dass die Eigenschaft ein Pflichtfeld ist.
    > - DisplayName - definiert den Text für Formularfelder und validierungsmeldungen verwendet werden soll
    > - DisplayFormat - gibt an, wie Datenfelder angezeigt und formatiert sind.
    > - StringLength - definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben
    > - Bereich - weist den maximalen und minimalen Wert für ein numerisches Feld
    > - ScaffoldColumn - ermöglicht das Ausblenden von Feldern aus Editor Formularen

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass die Seiten erstellen und Bearbeiten von Feldern, überprüfen, verwenden die Anzeigenamen in der vorhergehenden Aufgabe ausgewählt.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/StoreManager/Create**. Stellen Sie sicher, dass die Anzeigenamen der in der partiellen Klasse entspricht (z. B. **Album Art URL** anstelle von **AlbumArtUrl**)
3. Klicken Sie auf **erstellen**, ohne das Formular auszufüllen. Stellen Sie sicher, dass Sie die entsprechenden validierungsmeldungen erhalten.

    ![Überprüft die Felder in der Seite "erstellen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "überprüft die Felder in der Seite "erstellen"")

    *Überprüfte Felder in der Seite "erstellen"*
4. Sie können überprüfen, ob das gleiche mit geschieht die **bearbeiten** Seite. Ändern Sie die URL zum **/StoreManager/Edit/1** und stellen Sie sicher, dass die Anzeigenamen der in der partiellen Klasse entspricht (z. B. **Album Art URL** anstelle von **AlbumArtUrl**). Leere der **Titel** und **Preis** Felder, und klicken Sie auf **speichern**. Stellen Sie sicher, dass Sie die entsprechenden validierungsmeldungen erhalten.

    ![Überprüfte Felder bearbeiten (Seite)](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    Überprüfte Felder bearbeiten (Seite)

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Übung 7: Unter Verwendung von unaufdringliche jQuery auf Clientseite

In dieser Übung erfahren Sie, wie MVC 4 unaufdringliche jQuery-Validierung auf Clientseite zu aktivieren.

> [!NOTE]
> Die unaufdringliche jQuery verwendet Data-Ajax-Präfix JavaScript Aktionsmethoden auf den Server anstelle von intrusively Ausgeben von Inline-Clientskripts aufgerufen.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Aufgabe 1: Ausführen der Anwendung vor dem Aktivieren unaufdringliche jQuery

In dieser Aufgabe führen Sie die Anwendung vor dem Einfügen von jQuery damit beide zertifikatsvalidierungsmodelle verglichen werden kann.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex7-UnobtrusivejQueryValidation/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Drücken Sie **F5**, um die Anwendung auszuführen.
3. Das Projekt wird auf der Startseite gestartet. Navigieren Sie **/StoreManager/Create** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, um sicherzustellen, dass Sie validierungsmeldungen abrufen:

    ![Die Clientvalidierung deaktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Clientvalidierung deaktiviert")

    *Die Clientvalidierung deaktiviert*
4. Öffnen Sie im Browser die HTML-Quellcode aus:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Aufgabe 2: Aktivieren der unaufdringlichen Clientvalidierung

In dieser Aufgabe aktivieren Sie jQuery **unaufdringliche Clientvalidierung** aus **"Web.config"** -Datei, die standardmäßig auf "false" in allen neuen ASP.NET MVC 4-Projekten festgelegt ist. Fügen Sie außerdem, dass die erforderlichen Verweise zur jQuery unaufdringlichen Clientvalidierung Arbeit stellen Skripts.

1. Open **"Web.config"** Projekt Stammverzeichnis der Datei, und stellen Sie sicher, dass die **ClientValidationEnabled** und **UnobtrusiveJavaScriptEnabled** SchlüsselWertefestgelegtsind**"true"**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Sie können auch die Clientvalidierung durch Code am Global.asax.cs dieselben Ergebnisse abrufen aktivieren:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Darüber hinaus können Sie ClientValidationEnabled-Attributs in jedem Controller haben Sie ein benutzerdefiniertes Verhalten zuweisen.
2. Open **Create.cshtml** am **Views\StoreManager**.
3. Stellen Sie sicher, dass die folgenden Skriptdateien **jquery.validate** und **jquery.validate.unobtrusive**, verwiesen wird, in die Sicht kann die &quot; **~/bundles/jqueryval** &quot; Paket.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Alle diese jQuery-Bibliotheken sind in neuen MVC 4-Projekte enthalten. Sie finden weitere Bibliotheken in der **/Skripts** Ordner "Projekt".
    > 
    > Clientbibliotheken funktionieren, müssen Sie einen Verweis auf die jQuery-Framework-Klassenbibliothek hinzufügen, um diese Überprüfung vornehmen zu können. Da dieser Referenz in bereits hinzugefügt wurde die  **\_Layout.cshtml** Datei, Sie müssen nicht in diese spezielle Ansicht hinzufügen.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Aufgabe 3: Ausführen der Anwendung mithilfe von unaufdringliche jQuery-Überprüfung

In dieser Aufgabe testen Sie, die die **StoreManager** Vorlage führt die clientseitige Validierung jQuery-Clientbibliotheken verwenden, wenn der Benutzer ein neues Album erstellt Ansicht erstellen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Navigieren Sie **/StoreManager/Create** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, um sicherzustellen, dass Sie validierungsmeldungen abrufen:

    ![Die Clientvalidierung mit jQuery aktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Clientvalidierung mit jQuery aktiviert")

    *Die Clientvalidierung mit jQuery aktiviert*
3. Öffnen Sie im Browser den Quellcode für die Ansicht erstellen:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > Für jede Clientvalidierungsregel unaufdringliche jQuery Fügt ein Attribut mit Daten-Val -*Rulename*=&quot;*Nachricht*&quot;. Im folgenden finden Sie eine Liste der Tags, unauffälliger jQuery fügt die html-Eingabefeld Clientvalidierung ausführen:
    > 
    > - Daten-val
    > - Daten-Val-Anzahl
    > - Val-Datenbereich
    > - Daten-Val-Range-min / Daten-Val-Range-max
    > - Val erforderlichen Daten
    > - Val-Datenlänge
    > - Daten-Val-Länge-Max / Daten-Val-Länge-min
    > 
    > Alle Datenwerte mit Modell gefüllt **-Datenanmerkung**. Anschließend kann die gesamte Logik, die auf Serverseite arbeitet auf Clientseite ausgeführt werden. Price-Attribut hat beispielsweise die folgende datenanmerkung im Modell an:
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > Ist nach der Verwendung unaufdringliche jQuery, der generierte Code ein:
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben Sie gelernt so ändern Sie die in der Datenbank mit der Verwendung der folgenden gespeicherten Daten von Benutzern aktiviert:

- Controlleraktionen wie Index, erstellen, bearbeiten, löschen
- ASP.NETs Gerüstbau-Funktion zum Anzeigen von Eigenschaften in einer HTML-Tabelle
- Auftreten von benutzerdefinierte HTML-Hilfsmethoden, um Benutzer zu verbessern.
- Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren
- Eine freigegebene Editor-Vorlage für ähnliche Ansichtsvorlagen wie erstellen und bearbeiten
- Form-Elemente wie Dropdownlisten
- Datenanmerkungen zur modellvalidierung
- Clientseitige Validierung mit unaufdringliche jQuery-Bibliothek

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
