---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 – Hilfsprogramme, Formulare und Überprüfung | Microsoft-Dokumentation
author: rick-anderson
description: In ASP.NET MVC 4-Modelle und Daten zugreifen, praktische Übungseinheit wurde beim Laden und Anzeigen von Daten aus der Datenbank. Fügen Sie in dieser praktischen Übungseinheit die...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828067"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 – Hilfsprogramme, Formulare und Überprüfung

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

In **ASP.NET MVC 4-Modelle und Datenzugriff** praktische Übungseinheit, Sie wurden laden und Anzeigen von Daten aus der Datenbank. Fügen Sie in dieser praktischen Übungseinheit die **Music Store** Anwendung die Möglichkeit, diese Daten zu bearbeiten.

Mit diesem Ziel vor Augen erstellen Sie zunächst den Controller, der die Aktionen erstellen, lesen, aktualisieren und löschen (CRUD) von Alben unterstützt werden. Generieren Sie eine Ansicht "Index"-Vorlage nutzen ASP.NETs gerüstfeature Alben Eigenschaften in einer HTML-Tabelle angezeigt. Um diese Ansicht zu verbessern, fügen Sie einer benutzerdefinierten HTML-Hilfsobjekt, der eine lange Beschreibung abgeschnitten wird.

Fügen Sie anschließend, dass das Bearbeiten und erstellen Sie Ansichten, mit denen Sie in der Datenbank mithilfe des Form-Elemente wie Dropdownlisten Alben ändern.

Schließlich können Sie Benutzer, die ein Album gelöscht, und auch Sie werden verhindert, dass sie falsche Daten eingeben, indem Sie ihre Eingabe zu überprüfen.

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC**. Wenn Sie nicht verwendet haben **ASP.NET MVC** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Grundlagen von ASP.NET MVC** Hands-On Lab.

Dieses Lab führt Sie durch die Verbesserungen und neue Funktionen, die zuvor beschriebenen durch Anwenden von geringen Änderungen mit einer Beispiel-Webanwendung, die im Quellordner bereitgestellt.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [ASP.NET MVC 4-Hilfsprogramme, Formulare und Überprüfung](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen Sie einen Controller zur Unterstützung von CRUD-Vorgänge
- Generieren Sie eine Ansicht "Index" zum Anzeigen von Eigenschaften der Entität in eine HTML-Tabelle
- Hinzufügen einer benutzerdefinierten HTML-Hilfsmethoden
- Erstellen und Anpassen einer Ansicht bearbeiten
- Unterscheiden Sie zwischen Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren.
- Hinzufügen und Anpassen einer Ansicht erstellen
- Behandelt das Löschen einer Entität
- Validieren von Benutzereingaben

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang B: Verwenden von Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Die folgenden Übungen besteht aus diesem Hands-On Lab:

1. [Erstellen der Store Manager-Controller und die Ansicht "Index"](#Exercise1)
2. [Ein HTML-Hilfsobjekt hinzufügen](#Exercise2)
3. [Erstellen die Bearbeitungsansicht](#Exercise3)
4. [Hinzufügen einer Ansicht erstellen](#Exercise4)
5. [Löschung verarbeiten](#Exercise5)
6. [Hinzufügen der Validierung](#Exercise6)
7. [Unter Verwendung von unaufdringliche jQuery auf Clientseite](#Exercise7)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Übung 1: Erstellen der Store Manager-Controller und die Ansicht "Index"

In dieser Übung erfahren Sie, wie zum Erstellen eines neuen Controllers CRUD-Vorgänge zu unterstützen, passen die Index-Aktionsmethode, um eine Liste der Alben aus der Datenbank und generiert schließlich eine Ansicht "Index"-Vorlage nutzen ASP.NETs Gerüstbau zurückzugeben die Funktion Alben Eigenschaften in einer HTML-Tabelle angezeigt.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Aufgabe 1: Erstellen der StoreManagerController

In dieser Aufgabe erstellen Sie einen neuen Controller namens **StoreManagerController** CRUD-Vorgänge zu unterstützen.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex1-CreatingTheStoreManagerController/Anfang/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Fügen Sie einen neuen Controller hinzu. Dazu, mit der Maustaste der **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und klicken Sie dann die **Controller** Befehl. Ändern der **Controller** **Namen** zu **StoreManagerController** und stellen Sie sicher, dass die Option **MVC-Controller mit leeren Lese-/schreibaktionen**ausgewählt ist. Klicken Sie auf **Hinzufügen**.

    ![Dialogfeld zum Hinzufügen Controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Controller-Dialogfeld \"hinzufügen\"")

    *Controller-Dialogfeld "hinzufügen"*

    Es wird eine neue Controllerklasse generiert. Da Sie zum Hinzufügen von Aktionen für Lese-/Schreibzugriff Stubmethoden, angegeben werden häufig verwendete CRUD-Aktionen mit TODO-Kommentare ausgefüllt, aufgefordert wird, sollen die anwendungsspezifische Logik erstellt.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Aufgabe 2: Anpassen der StoreManager-Index

In dieser Aufgabe passen Sie die StoreManager Index-Aktionsmethode, um eine Ansicht mit der Liste der Alben aus der Datenbank zurückzugeben.

1. Fügen Sie Folgendes hinzu, in der Klasse StoreManagerController *mit* Anweisungen.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Ex1 mit MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Hinzufügen eines Felds auf die **StoreManagerController** um eine Instanz von aufzunehmen **MusicStoreEntities.**

    (Codeausschnitt - *ASP.NET MVC 4 – Hilfsprogramme und Formulare und Überprüfung - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementieren Sie die StoreManagerController Index-Aktion, um eine Ansicht mit der Liste der Alben zurückzugeben.

    Die Logik des Controllers-Aktion werden ähnelt stark der StoreControllers Index-Aktion, die zuvor geschrieben. Verwenden Sie LINQ, um alle Alben, einschließlich Informationen zu "Genre" und Interpreten für die Anzeige abzurufen.

    (Codeausschnitt - *ASP.NET MVC 4 – Hilfsprogramme und Formulare und Überprüfung - Ex1 StoreManagerController Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Aufgabe 3: Erstellen der Ansicht "Index"

In dieser Aufgabe erstellen Sie die Ansicht "Index"-Vorlage zum Anzeigen der Liste der Alben zurückgegebenes der **StoreManager** Controller.

1. Vor dem Erstellen der neuen Vorlage anzeigen, sollten Sie das Projekt erstellen, damit die **anzeigen-Dialogfeld hinzufügen** kennt die **Album** zu verwendende Klasse an. Wählen Sie **erstellen | Erstellen von MvcMusicStore** zum Erstellen des Projekts.
2. Klicken Sie auf auf die **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird die **Ansicht hinzufügen** Dialogfeld.

    ![Ansicht hinzufügen](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ansicht hinzufügen")

    *Hinzufügen einer Ansicht aus, in der Index-Methode*
3. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **Index**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellklasse** Dropdown-Liste. Wählen Sie **Liste** aus der **Gerüst Vorlage** Dropdownliste aus. Lassen Sie die **Ansichts-Engine** zu **Razor** und die anderen Felder mit ihren Standardwerten Wert, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Ansicht "Index"](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Hinzufügen einer Ansicht \"Index\"")

    *Hinzufügen einer Ansicht "Index"*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Aufgabe 4: Anpassen das Gerüst der Ansicht "Index"

In dieser Aufgabe wird angepasst, die einfache ansichtsvorlage erstellt, die mit ASP.NET MVC-gerüstfeature so, dass sie die gewünschten Felder anzuzeigen.

> [!NOTE]
> Die **Gerüstbau** Unterstützung in ASP.NET MVC generiert eine einfache Vorlage Anzeigen der alle Felder im Modell Album aufgeführt sind. **Gerüstbau** bietet eine schnelle Möglichkeit zum Einstieg in eine stark typisierte Ansicht: anstatt die ansichtsvorlage manuell schreiben, schnell Gerüstbau eine Standardvorlage generiert und dann können Sie den generierten Code ändern.


1. Überprüfen Sie den Code erstellt wird. Die generierte Liste der Felder werden Teil des folgenden HTML Tabelle, die **Gerüstbau** zum Anzeigen von Tabellendaten verwendet.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Ersetzen Sie die **&lt;Tabelle&gt;** Code durch den folgenden Code zur Anzeige der **"Genre"**, **Interpreten**, **Albumtitel**, und **Preis** Felder. Dies löscht die **AlbumId** und **Album Art URL** Spalten. Es ändert sich auch, GenreId und ArtistId Spalten zum Anzeigen ihrer Klasseneigenschaften verknüpfte **Artist.Name** und **Genre.Name**, und entfernt die **Details** Link.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Ändern Sie die folgenden Beschreibungen.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **Index** ansichtsvorlage zeigt eine Liste der Alben der Entwurf der vorherigen Schritte entsprechend.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager** um sicherzustellen, dass eine Liste der Alben mit angezeigt wird, deren **Titel**, **Interpreten** und **"Genre"**.

    ![Durchsuchen die Liste der Alben](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "durchsuchen die Liste der Alben")

    *Durchsuchen die Liste der Alben*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Übung 2: Hinzufügen ein HTML-Hilfsprogramm

Die Indexseite mit StoreManager hat ein potenzielles Problem: Titel und die Namen der Interpreten Eigenschaften können sowohl werden lang genug ist, deaktivieren Sie die Formatierung der Tabelle ausgelöst. In dieser Übung lernen Sie, wie Sie eine benutzerdefinierte HTML-Hilfsfunktion, um das Abschneiden von Text hinzufügen.

In der folgenden Abbildung sehen Sie, wie das Format aufgrund der Länge des Texts geändert wird, wenn Sie eine kleines Browser-Größe verwenden.

![Durchsuchen die Liste der Alben mit Text nicht abgeschnitten](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "durchsuchen die Liste der Alben mit Text nicht abgeschnitten")

*Durchsuchen die Liste der Alben mit nicht Text abgeschnitten*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Aufgabe 1: erweitern das HTML-Hilfsobjekt

In dieser Aufgabe fügen Sie eine neue Methode **Truncate** auf die **HTML** in ASP.NET MVC-Ansichten verfügbar gemachtes Objekt zu. Implementieren Sie zu diesem Zweck eine **Erweiterungsmethode** für die integrierte **System.Web.Mvc.HtmlHelper** von ASP.NET MVC bereitgestellte Klasse.

> [!NOTE]
> Weitere Informationen zu **Erweiterungsmethoden**, finden Sie unter diesem Msdn-Artikel. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex2-AddingAnHTMLHelper/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen Sie die StoreManager Ansicht "Index". Zu diesem Zweck im Projektmappen-Explorer erweitern Sie die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **"Index.cshtml"** Datei.
3. Fügen Sie den folgenden Code unter der <strong>@model</strong> Richtlinie zum Definieren der <strong>Truncate</strong> Hilfsmethode.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Aufgabe 2 - Abschneiden von Text auf der Seite

In dieser Aufgabe verwenden Sie die **Truncate** Methode, um den Text in die ansichtsvorlage abgeschnitten.

1. Öffnen Sie die StoreManager Ansicht "Index". Zu diesem Zweck im Projektmappen-Explorer erweitern Sie die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **"Index.cshtml"** Datei.
2. Ersetzen Sie die Zeilen, die zeigen, die **Interpreten** und des "Album" **Titel**. Ersetzen Sie hierzu die folgenden Zeilen ein.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **Index** ansichtsvorlage schneidet, Titel und die Namen der Interpreten des Albums.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zu **/StoreManager** um zu überprüfen, ob lange Texte in der **Titel** und **Interpreten** Spalte werden abgeschnitten.

    ![Titel und Interpreten Namen abgeschnitten](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "abgeschnitten, Titel und Interpreten Namen")

    *Abgeschnittene Titel und die Namen der Künstler diese geändert hat*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Übung 3: Erstellen der Bearbeitungsansicht

In dieser Übung lernen Sie, wie erstellen Sie ein Formular zum Zulassen von Speicher-Manager, um ein Album zu bearbeiten. Durchsuchen sie die **/StoreManager/Edit/id** URL (**Id** wird die eindeutige Id des Albums bearbeiten), wodurch eine HTTP-GET-Aufrufs an den Server.

Die Aktionsmethode Controller bearbeiten wird das entsprechende Album aus der Datenbank abrufen, erstellen eine **StoreManagerViewModel** Objekt, das (zusammen mit der eine Liste mit Interpreten und Genres) zu kapseln, und übergibt diese dann an eine ansichtsvorlage deaktivieren Rendern von HTML-Seite zurück an dem Benutzer. Diese Seite enthält eine **&lt;Formular&gt;** -Element mit Textfelder und Dropdownfelder zum Bearbeiten der Eigenschaften von Album.

Sobald der Benutzer klickt und die Formularwerte Album aktualisiert der **speichern** Schaltfläche, die Änderungen werden gesendet, über einen Rückruf aus eine HTTP-POST, **/StoreManager/Edit/id**. Obwohl die URL der gleiche wie in den letzten Aufruf verbleibt, ASP.NET MVC identifiziert, die dieses Mal, es ist ein HTTP-POST und aus diesem Grund führt eine andere Methode des bearbeiten-Aktion (eine ergänzt **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Aufgabe 1: Implementieren von HTTP-GET-Edit-Aktion-Methode

In dieser Aufgabe implementieren Sie die HTTP-GET-Version, der die Aktionsmethode "Bearbeiten" zum Abrufen der entsprechenden Album aus der Datenbank sowie eine Liste aller Genres und Künstler. Sie werden diese Daten Verpacken, in der **StoreManagerViewModel** Objekt definiert, die im letzten Schritt, die Sie dann auf eine ansichtsvorlage zum Rendern der Antwort mit übergeben werden.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex3-CreatingTheEditView/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **StoreManagerController** Klasse. Erweitern Sie dazu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie die **HTTP-GET bearbeiten** Aktionsmethode mit den folgenden Code zum Abrufen von den entsprechenden **Album** als auch die **Genres** und **Künstler**aufgeführt.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-GET*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Verwenden Sie **System.Web.Mvc** **SelectList** Interpreten und Genres statt der **System.Collections.Generic** Liste.
    > 
    > **SelectList** ist eine sauberere Methode zum Auffüllen von HTML-Dropdownlisten und Dinge wie die aktuelle Auswahl zu verwalten. Das Bearbeiten-Formular-Szenario werden lesbarer gestalten, instanziieren und diese ViewModel-Objekte in die Controlleraktion später einrichten.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Aufgabe 2: Erstellen der Bearbeitungsansicht

In dieser Aufgabe erstellen Sie eine Vorlage bearbeiten anzeigen, die später die Album-Eigenschaften angezeigt werden.

1. Erstellen Sie die Bearbeitungsansicht. Klicken Sie dazu auf, auf die **bearbeiten** Aktionsmethode, und wählen **Ansicht hinzufügen**.
2. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **bearbeiten**. Überprüfen Sie die **eine stark typisierte Ansicht erstellen,** Kontrollkästchen, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Datenklasse anzeigen** Dropdown-Liste. Wählen Sie **bearbeiten** aus der **Gerüst Vorlage** Dropdownliste aus. Behalten Sie die anderen Felder auch die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "eine Bearbeitungsansicht hinzufügen")

    *Hinzufügen einer Bearbeitungsansicht*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt die Eigenschaften-Werte für das Album, die als Parameter übergeben.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1** um sicherzustellen, dass die Eigenschaften-Werte für das Album übergeben angezeigt werden.

    ![Durchsuchen des Albums Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Durchsuchen des Albums Bearbeitungsansicht")

    *Durchsuchen des Albums Bearbeitungsansicht*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Aufgabe 4: Implementieren von Dropdown-Elemente in der Vorlage Album-Editor

In dieser Aufgabe fügen Sie Dropdownlisten die ansichtsvorlage erstellt, in der letzten Aufgabe hinzu, damit der Benutzer aus einer Liste von Künstlern und Genres auswählen kann.

1. Ersetzen Sie alle der **Album** Fieldset-Code durch Folgendes:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Ein **Html.DropDownList** Hilfsprogramm zum Rendern von Dropdownlisten zum Auswählen von Künstlern und Genres hinzugefügt wurde. Die Parameter zu übergeben, um **Html.DropDownList** sind:
    > 
    > 1. Der Name des Formularfelds (**&quot;ArtistId&quot;**).
    > 2. Die **SelectList** der Werte für den Dropdown-Pfeil.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt Dropdownlisten statt Interpreten und das Genre-ID-Text-Felder.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1** um sicherzustellen, dass der Dropdown-Elemente Interpreten und das Genre-ID-Text-Felder angezeigt.

    ![Durchsuchen des Albums Bearbeiten-Ansicht mit Dropdownlisten](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "durchsuchen Album Bearbeiten-Ansicht mit Dropdownlisten")

    *Durchsuchen des Albums-Edit-Ansicht mit Dropdownlisten*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Aufgabe 6: Implementieren der Bearbeiten Sie die HTTP-POST-Aktion-Methode

Nun, dass die Ansicht bearbeiten wie erwartet angezeigt wird, müssen Sie zur Implementierung der HTTP-POST-Aktion bearbeiten-Methode, um die Änderungen in das Album zu speichern.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** aus der **Controller** Ordner.
2. Ersetzen Sie dies **HTTP-POST bearbeiten** Aktion-Methodencode durch Folgendes (Beachten Sie, dass die Methode, die ersetzt werden müssen überladene Version, die zwei Parameter empfängt):

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-POST*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Diese Methode wird ausgeführt, wenn der Benutzer klickt auf die **speichern** Schaltfläche der Ansicht und führt einen HTTP-POST von die Formularwerte an den Server um dauerhaft in der Datenbank gespeichert werden. Das Decorator-Element **[HttpPost]** gibt an, dass die Methode für diese HTTP-POST-Szenarien verwendet werden soll. Die Methode akzeptiert eine **Album** Objekt. ASP.NET MVC erstellt automatisch das Album-Objekt aus der bereitgestellten &lt;Formular&gt; Werte.
    > 
    > Die Methode führt die folgenden Schritte aus:
    > 
    > 1. Wenn das Modell gültig ist:
    > 
    >     1. Aktualisieren Sie den Eintrag "Album" im Kontext als ein geändertes Objekt kennzeichnen.
    >     2. Speichern Sie die Änderungen, und leiten Sie zur Indexansicht.
    > 2. Wenn das Modell nicht gültig ist, füllt es die ViewBag-Klasse mit dem **GenreId** und **ArtistId**, wird die Ansicht mit dem empfangenen Album-Objekt, damit der Benutzer kann zurückgegeben führen Sie ein Update erforderliches.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager bearbeiten** Ansichtsseite speichert die aktualisierten Album Daten tatsächlich in der Datenbank.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/Edit/1**. Ändern Sie den Titel Album zu **Load** und klicken Sie auf **speichern**. Stellen Sie sicher, dass die Albumtitel tatsächlich in der Liste der Alben geändert.

    ![Aktualisieren eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "ein Album aktualisieren")

    *Aktualisieren eines Albums*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Übung 4: Hinzufügen einer Create-Ansicht

Nun, dass die **StoreManagerController** unterstützt die **bearbeiten** Möglichkeit, die in dieser Übung Sie erfahren, wie zum Hinzufügen einer Create View-Vorlage, damit Sie das Speichern von Manager neue Alben zur Anwendung hinzufügen.

Wie Sie mit der Funktion bearbeiten, implementieren Sie die erstellen-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:

1. Eine Aktionsmethode wird ein leeres Formular angezeigt, wenn Speicher-Manager zum ersten Mal besuchen der **/StoreManager/erstellen** URL.
2. Eine zweite Aktionsmethode behandelt das Szenario, in denen klickt auf der Store Manager die **speichern** Schaltfläche im Formular aus, und sendet die Werte zurück, an die **/StoreManager/erstellen** URL als eine HTTP-POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Aufgabe 1: Implementieren der Aktionsmethode erstellen HTTP-GET

In dieser Aufgabe implementieren Sie die HTTP-GET-Version, der die Create-Aktion-Methode zum Abrufen einer Liste mit allen Genres und Interpreten, Verpacken diese Daten in einem **StoreManagerViewModel** -Objekt, das dann an eine ansichtsvorlage übergeben wird.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex4-AddingACreateView/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Open **StoreManagerController** Klasse. Erweitern Sie dazu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie die **erstellen** Aktion-Methodencode durch Folgendes:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Aktion Ex4 StoreManagerController HTTP-GET-erstellen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Aufgabe 2: Hinzufügen der Ansicht "erstellen"

In dieser Aufgabe fügen Sie die Create View-Vorlage, die ein neues-(leeres)-Albumformular angezeigt werden.

1. Klicken Sie auf auf die **erstellen** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.
2. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **erstellen**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellklasse** Dropdown-Menü und **erstellen** aus der **Gerüst Vorlage** Dropdownliste aus. Behalten Sie die anderen Felder auch die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Erstellungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "hinzufügen-a-erstellen-view.png")

    *Hinzufügen der Ansicht "erstellen"*
3. Update der **GenreId** und **ArtistId** Felder, die eine Dropdown-Liste verwendet wird, wie unten dargestellt:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **erstellen** Ansichtsseite wird ein leeres Albumformular ein angezeigt.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/erstellen**. Stellen Sie sicher, dass ein leeres Formular angezeigt wird, für das Auffüllen von Eigenschaften für die neuen Album.

    ![Erstellen Sie die Ansicht mit einem leeren Formular](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View mit einem leeren Formular")

    *Ansicht mit einem leeren Formular erstellen*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Aufgabe 4: Implementierung der HTTP-POST erstellen Action-Methode

In dieser Aufgabe implementieren Sie die HTTP-POST-Version der erstellen-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **speichern** Schaltfläche. Die Methode sollte das neue Album in der Datenbank gespeichert.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** Klasse. Erweitern Sie dazu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie dies **erstellen HTTP-POST** Aktion-Methodencode durch Folgendes:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Ex4 StoreManagerController HTTP - POST Aktion erstellen*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Die Create-Aktion ähnelt sehr der vorherigen Edit-Aktionsmethode, aber statt das Objekt als geändert, wird es in den Kontext hinzugefügt wird.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager erstellen** Ansichtsseite können Sie ein neues Album zu erstellen, und klicken Sie dann zur Indexansicht StoreManager leitet.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/erstellen**. Füllen Sie alle Formularfelder mit Daten für ein neues Album, wie in der folgenden Abbildung:

    ![Erstellen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "ein Album erstellen")

    *Erstellen eines Albums*
3. Stellen Sie sicher, dass Sie zur Indexansicht StoreManager umgeleitet, die den soeben erstellten neue Album enthält.

    ![Neues Album erstellt](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "neues Album erstellt")

    *Neues Album erstellt*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Schritt 5: Behandeln von löschen

Die Möglichkeit, Alben löschen wurde noch nicht implementiert. Dies wird in dieser Übung zu. Wie zuvor implementieren Sie die löschen-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:

1. Eine Aktionsmethode wird eine Form der Bestätigung angezeigt.
2. Eine zweite Aktionsmethode übernimmt die Übermittlung des Formulars

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Aufgabe 1: Implementieren der HTTP-GET-löschen-Aktion-Methode

Implementieren Sie die HTTP-GET-Version, der die Delete-Aktion-Methode zum Abrufen von Informationen für das Album des in dieser Aufgabe.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex5-HandlingDeletion/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Open **StoreManagerController** Klasse. Erweitern Sie dazu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
3. Die Löschaktion der Controller ist identisch mit der vorherigen Aktion des Store-Details-Controller: Abfragen der **Album** Objekt aus der Datenbank mithilfe der **Id** bereitgestellt, in der URL und gibt die entsprechende **Ansicht**. Ersetzen Sie dazu die HTTP-GET **löschen** Aktion-Methodencode durch Folgendes:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Ex5 löschen verarbeiten HTTP-GET-löschen-Aktion*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Klicken Sie auf auf die **löschen** Aktionsmethode, und wählen **Ansicht hinzufügen**. Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.
5. Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **löschen**. Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellklasse** Dropdown-Liste. Wählen Sie **löschen** aus der **Gerüst Vorlage** Dropdown-Liste. Behalten Sie die anderen Felder auch die Standardwerte, und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Delete-Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Hinzufügen einer Löschansicht")

    *Hinzufügen einer Löschansicht*
6. Die Vorlage löschen, zeigt alle Felder aus dem Modell. Sie werden nur das Album, den Titel angezeigt. Zu diesem Zweck ersetzen Sie den Inhalt der Ansicht durch den folgenden Code ein:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager** **löschen** Ansichtsseite zeigt eine Form der Bestätigung löschen.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager**. Wählen Sie ein Album zu löschen, indem Sie auf **löschen** und stellen Sie sicher, dass die neue Ansicht hochgeladen wurde.

    ![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "ein Album gelöscht")

    *Löschen eines Albums*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Aufgabe 3: Implementieren der HTTP-POST Delete-Aktion-Methode

In dieser Aufgabe implementieren Sie die HTTP-POST-Version der Delete-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **löschen** Schaltfläche. Die Methode sollte das Album in der Datenbank löschen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren. Open **StoreManagerController** Klasse. Erweitern Sie dazu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie dies **HTTP-POST löschen** Aktion-Methodencode durch Folgendes:

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung - Ex5 löschen verarbeiten HTTP-POST-löschen-Aktion*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe testen Sie, die die **StoreManager löschen** Ansichtsseite ermöglicht das Löschen eines Albums und leitet dann an die Indexansicht StoreManager weiter.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager**. Wählen Sie ein Album zu löschen, indem Sie auf **löschen.** Bestätigen Sie den Löschvorgang, indem Sie auf **löschen** Schaltfläche:

    ![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "ein Album gelöscht")

    *Löschen eines Albums*
3. Stellen Sie sicher, dass das Album gelöscht wurde, da es sich nicht in erscheint die **Index** Seite.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Übung 6: Hinzufügen einer Validierung

Derzeit führen die erstellen und Bearbeiten von Formen, die Sie verfügen keine Art von Validierung. Wenn der Benutzer ein erforderliches Feld ist leer oder Typ Buchstaben in das Preisfeld verlässt, wird der erste Fehler erhalten Sie aus der Datenbank sein.

Sie können die Anwendung durch Hinzufügen von Datenanmerkungen zu Ihrer Modellklasse Validierung hinzufügen. Datenanmerkungen ermöglichen, beschreiben die gewünschten Regeln angewendet werden, um Ihre Eigenschaften des Modells und ASP.NET MVC übernimmt erzwingen und die entsprechende Meldung für Benutzer anzeigen.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Aufgabe 1: Hinzufügen von Datenanmerkungen

In dieser Aufgabe fügen Sie, dass von Datenanmerkungen auf dem Album-Modell, das der Seite "erstellen" und "Bearbeiten" zu machen, werden validierungsmeldungen bei Bedarf anzuzeigen.

Für eine einfache Modellklasse, Hinzufügen einer Anmerkung Daten nur erfolgt durch Hinzufügen einer **mit** -Anweisung für **System.ComponentModel.DataAnnotation**, platzieren Sie dann eine **[erforderlich]** Attribut für die entsprechenden Eigenschaften. Im folgende Beispiel würde machen die **Namen** -Eigenschaft ein Pflichtfeld in der Ansicht.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Dies ist in Fällen, wie diese Anwendung ein wenig komplexer, an dem das Entity Data Model generiert wird. Wenn Sie direkt an die Modellklassen Datenanmerkungen hinzugefügt haben, würden sie überschrieben werden, wenn Sie das Modell aus der Datenbank aktualisieren. Sie können stattdessen machen Metadaten partiellen Klassen, die zum Speichern von Anmerkungen vorhanden ist und mit dem Modell zugeordnet sind Klassen, mit der **["metadatatype"]** Attribut.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex6-AddingValidation/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **Album.cs** aus der **Modelle** Ordner.
3. Ersetzen Sie dies **Album.cs** Inhalt mit den hervorgehobenen Code, damit sie wie folgt aussieht:

    > [!NOTE]
    > Die Zeile **[DisplayFormat(ConvertEmptyStringToNull=false)]** gibt an, dass leere Zeichenfolgen aus dem Modell nicht auf null konvertiert werden, wenn das Datenfeld in der Datenquelle aktualisiert wird. Diese Einstellung wird eine Ausnahme vermeiden, wenn Entity Framework null-Werte für das Modell zugewiesen werden, bevor Datenanmerkung Felder überprüft.

    (Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Überprüfung: partielle Ex6 Album Metadatenklasse*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Dies **Album** partielle Klasse verfügt über eine **"metadatatype"** Attribut verweist auf die **AlbumMetaData** -Klasse für die Datenanmerkungen. Dies sind einige der Datenanmerkung Attribute, die Sie verwenden, um das Modell Album mit Anmerkungen versehen:
    > 
    > - Erforderlich – gibt an, dass die Eigenschaft ein erforderliches Feld.
    > - DisplayName - definiert den Text auf Formularfelder und von überprüfungsnachrichten verwendet werden
    > - DisplayFormat – gibt an, wie Datenfelder angezeigt und formatiert sind.
    > - StringLength - definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben
    > - Bereich – weist den maximalen und minimalen Wert für ein numerisches Feld
    > - ScaffoldColumn – ermöglicht das Ausblenden von Feldern von Editor-Formularen

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass die Seiten erstellen und Bearbeiten von Feldern, überprüft unter Verwendung der Anzeige in der vorhergehenden Aufgabe ausgewählt.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/StoreManager/erstellen**. Stellen Sie sicher, dass die Anzeigenamen in der partiellen Klasse übereinstimmen (z. B. **Album Art URL** anstelle von **AlbumArtUrl**)
3. Klicken Sie auf **erstellen**, ohne das Formular ausfüllen. Stellen Sie sicher, dass Sie die entsprechende Überprüfung Nachrichten erhalten.

    ![Überprüft die Felder in der Seite "erstellen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "überprüft die Felder in der Seite \"erstellen\"")

    *Überprüfte Felder in der Seite "erstellen"*
4. Überprüfen, ob Sie dasselbe gilt, mit der **bearbeiten** Seite. Ändern Sie die URL zum **/StoreManager/Edit/1** und stellen Sie sicher, dass die Anzeigenamen in der partiellen Klasse übereinstimmen (z. B. **Album Art URL** anstelle von **AlbumArtUrl**). Leere der **Titel** und **Preis** Felder, und klicken Sie auf **speichern**. Stellen Sie sicher, dass Sie die entsprechende Überprüfung Nachrichten erhalten.

    ![Überprüfte Felder in der Seite "Bearbeiten"](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Überprüfte Felder in der Seite "Bearbeiten"*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Übung 7: Verwendung von unaufdringliche jQuery auf Clientseite

In dieser Übung lernen Sie, wie Sie MVC 4 unaufdringliche jQuery-Validierung auf Clientseite zu aktivieren.

> [!NOTE]
> Die unaufdringliche jQuery verwendet Data-Ajax-Präfix JavaScript zum Aufrufen der Aktionsmethoden, auf dem Server statt auf intrusively Ausgeben von Inline-Clientskripts.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Aufgabe 1: Ausführen der Anwendung vor dem Aktivieren unaufdringliche jQuery

In dieser Aufgabe führen Sie die Anwendung vor, einschließlich jQuery, um den beide Überprüfung Modelle verglichen werden soll.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex7-UnobtrusivejQueryValidation/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Drücken Sie **F5**, um die Anwendung auszuführen.
3. Das Projekt, die auf der Startseite wird gestartet. Navigieren Sie **/StoreManager/erstellen** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, sodass stellen Sie sicher, dass Sie validierungsmeldungen erhalten:

    ![Die Clientvalidierung deaktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "die Clientvalidierung deaktiviert")

    *Die Clientvalidierung deaktiviert*
4. Öffnen Sie im Browser den HTML-Quellcode aus:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Aufgabe 2: Aktivieren der unaufdringlichen Clientvalidierung

In dieser Aufgabe aktivieren Sie jQuery **unaufdringliche Clientvalidierung** aus **"Web.config"** -Datei, die standardmäßig auf "false" in allen neuen ASP.NET MVC 4-Projekten festgelegt ist. Fügen Sie außerdem, dass die erforderlichen Verweise auf jQuery Unobtrusive Client Validation Arbeit stellen Skripts.

1. Öffnen **"Web.config"** Datei am Stammverzeichnis des Projekts, und stellen Sie sicher, dass die **ClientValidationEnabled** und **UnobtrusiveJavaScriptEnabled** Schlüssel-Werte werden festgelegt, um **"true"**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Sie können auch die Clientvalidierung durch Code in "Global.asax.cs" für die gleichen Ergebnisse erhalten aktivieren:
    > 
    > **HtmlHelper.ClientValidationEnabled = True;**
    > 
    > Darüber hinaus können Sie ClientValidationEnabled-Attributs in jedem Controller haben Sie ein benutzerdefiniertes Verhalten zuweisen.
2. Open **Create.cshtml** am **Views\StoreManager**.
3. Stellen Sie sicher, dass die folgenden Skriptdateien **jquery.validate** und **jquery.validate.unobtrusive**, auf die in der Ansicht Ausmaße der &quot; **~/bundles/jqueryval** &quot; Paket.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Alle diese jQuery-Bibliotheken sind in neuen MVC 4-Projekte enthalten. Finden Sie weitere Bibliotheken in der **/Skripts** Ordner von Ihrem Projekt.
    > 
    > Bibliotheken funktionieren, müssen Sie einen Verweis auf die jQuery-Framework-Bibliothek hinzufügen, um diese Validierung zu machen. Da dieser Verweis bereits, in hinzugefügt wurde der  **\_Layout.cshtml** -Datei, Sie müssen nicht in dieser Ansicht hinzufügen.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Aufgabe 3: Ausführen der Anwendung mithilfe von unaufdringliche jQuery-Validierung

In dieser Aufgabe testen Sie, die die **StoreManager** Vorlage führt die Validierung auf Clientseite jQuery-Bibliotheken verwenden, wenn der Benutzer ein neues Album erstellt Ansicht erstellen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Das Projekt, die auf der Startseite wird gestartet. Navigieren Sie **/StoreManager/erstellen** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, sodass stellen Sie sicher, dass Sie validierungsmeldungen erhalten:

    ![Validierung mit jQuery aktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validierung mit jQuery aktiviert")

    *Validierung mit jQuery aktiviert*
3. Öffnen Sie im Browser den Quellcode für die Ansicht erstellen:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Für jede Clientvalidierungsregel, fügt die unaufdringliche jQuery ein Attribut mit Daten: Val -*Rulename*=&quot;*Nachricht*&quot;. Im folgenden finden Sie eine Liste von Tags, unauffälliger jQuery Fügt HTML-Feld für die Validierung ausführen:
   > 
   > - Val-Daten
   > - Daten-Val-Anzahl
   > - Val-Datenbereich
   > - Daten-Val-Bereich: min / Daten-Val-Range-max '
   > - Val erforderlichen Daten
   > - Val-Datenlänge
   > - Daten-Val-Länge-max ' / Daten-Val-Länge-min
   > 
   > Alle Datenwerte werden mit Modell gefüllt **Datenanmerkung**. Anschließend kann die gesamte Logik, die auf Serverseite funktioniert auf Clientseite ausgeführt werden. Price-Attribut hat beispielsweise die folgende datenanmerkung im Modell:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Ist nach der Verwendung von unaufdringliche jQuery, der generierte Code ein:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie gelernt, Benutzern so ändern Sie die in der Datenbank mit der Verwendung der folgenden gespeicherten Daten ermöglichen:

- Controlleraktionen wie z. B. Index erstellen, bearbeiten, löschen
- ASP.NETs gerüstfeature zum Anzeigen von Eigenschaften in einer HTML-Tabelle
- Auftreten von benutzerdefinierten HTML-Hilfsprogramme, um Benutzer zu verbessern.
- Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren.
- Eine freigegebene Editor-Vorlage für ähnliche Ansichtsvorlagen erstellen und bearbeiten
- Form-Elemente wie Dropdownlisten
- Datenanmerkungen bei der modellüberprüfung
- Clientseitige Validierung mit unaufdringliche jQuery-Bibliothek

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
