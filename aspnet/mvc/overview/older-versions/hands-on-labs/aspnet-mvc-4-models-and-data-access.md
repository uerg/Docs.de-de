---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 – Modelle und Datenzugriff | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC verfügen. Wenn Sie nicht ASP.NET MVC, bevor Sie verwendet haben, empfehlen wir Ihnen über ASP.NET MVC 4 wechseln...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 26896e6ee3c02e8f939296ecbfb8b7d500940765
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835584"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 – Modelle und Datenzugriff

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC**. Wenn Sie nicht verwendet haben **ASP.NET MVC** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Grundlagen von ASP.NET MVC 4** Hands-On Lab.

Dieses Lab führt Sie durch die Verbesserungen und neue Funktionen, die zuvor beschriebenen durch Anwenden von geringen Änderungen mit einer Beispiel-Webanwendung, die im Quellordner bereitgestellt.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [ASP.NET MVC 4-Modelle und Datenzugriff](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

In **Grundlagen von ASP.NET MVC** praktische Übungseinheit, Sie haben wurde übergeben hartcodierten Daten vom Controller an die Ansichtsvorlagen. Um eine echte Webanwendung zu erstellen, Sie sollten jedoch eine echte Datenbank verwenden.

Diese praktische Übungseinheit erfahren Sie, wie mit einer Datenbank-Engine zum Speichern und Abrufen der Daten, die für die Music Store-Anwendung erforderlich sind. Damit dies erreicht wird, Sie beginnen mit einer vorhandenen Datenbank und das Entity Data Model daraus erstellen. In dieser Übung werden Sie erfüllen die **Database First** Ansatz als auch die **Code First** Ansatz.

Allerdings können Sie auch verwenden die **Model First** Ansatz, erstellen Sie das gleiche Modell mit den Tools und klicken Sie dann die Datenbank aus zu generieren.

![Datenbank erste im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. Model First")

*Datenbank erste im Vergleich zu Model First*

Nach dem Generieren des Modells, werden Sie die entsprechenden Anpassungen in die StoreController können Sie die Store-Ansichten mit den Daten aus der Datenbank anstelle von hartcodierten Daten bereitstellen vornehmen. Sie benötigen keine Änderung an die Ansichtsvorlagen vornehmen, da die StoreController die gleichen ViewModels, die Ansichtsvorlagen zurückgegeben wird zwar dieses Mal die Daten aus der Datenbank stammen.

**Code First-Ansatz**

Der Code First-Ansatz ermöglicht uns, definieren das Modell aus dem Code, ohne das Generieren von Klassen, die in der Regel mit dem Framework gekoppelt sind.

Im Code zuerst Modellobjekte werden mit definiert POCOs, &quot;Plain Old CLR Objects&quot;. POCOs sind einfache einfache Klassen, die keine Vererbung und implementieren keine Schnittstellen. Wir können die Datenbank automatisch generieren, von ihnen oder wir können eine vorhandene Datenbank verwenden und die klassenzuordnung aus dem Code zu generieren.

Die Vorteile der Verwendung dieses Ansatzes ist, dass das Modell unabhängig von das persistenzframework (in diesem Fall Entity Framework), bleibt, wie die POCOs Klassen nicht mit der Mapping-Framework gekoppelt sind.

> [!NOTE]
> Diese Testumgebung basiert auf ASP.NET MVC 4 und eine Version der Music Store-beispielanwendung angepasst und entsprechend nur die Funktionen, die in dieser praktischen Übungseinheit gezeigt minimiert.
> 
> Wenn Sie die gesamte kennenlernen möchten **Music Store** lernprogrammanwendung, die Sie es im finden [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).


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

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:

1. [Übung 1: Hinzufügen einer Datenbank](#Exercise1)
2. [Übung 2: Erstellen einer Datenbank mithilfe von Code First](#Exercise2)
3. [Übung 3: Abfragen der Datenbank mit Parametern](#Exercise3)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **35 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Übung 1: Hinzufügen einer Datenbank

In dieser Übung lernen Sie, wie Sie eine Datenbank mit den Tabellen der Music Store-Anwendung, die Lösung zum Nutzen der Daten hinzufügen. Sobald die Datenbank mit dem Modell generiert, und der Projektmappe hinzugefügt, ändern Sie die StoreController-Klasse, um die ansichtsvorlage mit den Daten aus der Datenbank, anstatt hartcodierte Werte bereitzustellen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Aufgabe 1: Hinzufügen einer Datenbank

In dieser Aufgabe fügen Sie eine bereits erstellte Datenbank mit die Haupttabellen der Music Store-Anwendung, die Lösung.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex1-AddingADatabaseDBFirst/Anfang/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Hinzufügen **MvcMusicStore** Datenbankdatei. In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen **MvcMusicStore.mdf**. Zu diesem Zweck Maustaste **App\_Daten** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**. Navigieren Sie zu **\Source\Assets** , und wählen Sie die **MvcMusicStore.mdf** Datei.

    ![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "ein vorhandenes Element hinzufügen")

    *Hinzufügen eines vorhandenen Elements*

    ![Datenbankdatei MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf-Datenbankdatei")

    *MvcMusicStore.mdf-Datenbankdatei*

    Die Datenbank wurde dem Projekt hinzugefügt. Auch wenn die Datenbank in der Projektmappe befindet, können Sie Abfragen und aktualisieren Sie sie, wie es in einen anderen Datenbankserver gehostet wurde.

    ![MvcMusicStore-Datenbank im Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore-Datenbank im Projektmappen-Explorer")

    *MvcMusicStore-Datenbank im Projektmappen-Explorer*
3. Überprüfen Sie die Verbindung mit der Datenbank aus. Zu diesem Zweck doppelklicken Sie auf **MvcMusicStore.mdf** zum Herstellen einer Verbindung.

    ![Herstellen einer Verbindung mit MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf mit")

    *Herstellen einer Verbindung mit MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Aufgabe 2: Erstellen eines Datenmodells

In dieser Aufgabe erstellen Sie ein Datenmodell für die Interaktion mit der Datenbank, die in der vorherigen Aufgabe hinzugefügt.

1. Erstellen Sie ein Datenmodell, die die Datenbank darstellt. Dazu klicken Sie im Projektmappen-Explorer mit der rechten Maustaste die **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Vorlage und dann die **ADO.NET Entity Data Model** Element. Ändern Sie den Namen des Daten-Modell zu **StoreDB.edmx** , und klicken Sie auf **hinzufügen**.

    ![Hinzufügen von StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET Entity Data Model hinzufügen")

    *Hinzufügen von StoreDB ADO.NET Entity Data Model*
2. Die **Entity Data Model-Assistenten** wird angezeigt. Dieser Assistent führt Sie durch die Erstellung der Modellschicht. Da das Modell basierend auf der vorhandenen Datenbank Recentyl hinzugefügt erstellt werden soll, wählen Sie **aus Datenbank generieren** , und klicken Sie auf **Weiter**.

    ![Auswählen des Modellinhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "Modellinhalt auswählen")

    *Den Modellinhalt auswählen*
3. Da Sie ein Modell aus einer Datenbank generieren, müssen Sie die zu verwendende Verbindung an. Klicken Sie auf **neue Verbindung**.
4. Wählen Sie **Microsoft SQL Server-Datenbankdatei** , und klicken Sie auf **Weiter**.

    ![Wählen Sie die Datenquelle](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")

    *Wählen Sie die Daten-Dialogfeld "Quelle"*
5. Klicken Sie auf **Durchsuchen** , und wählen Sie die Datenbank **MvcMusicStore.mdf** Sie befindet sich der **App\_Daten** Ordner, und klicken Sie auf **OK**.

    ![Verbindungseigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")

    *Verbindungseigenschaften*
6. Die generierte Klasse sollten den gleichen Namen wie die Verbindungszeichenfolge für die Entität haben, so ändern Sie den Namen **MusicStoreEntities** , und klicken Sie auf **Weiter**.

    ![Wählen die Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "die Datenverbindung auswählen")

    *Die Datenverbindung auswählen*
7. Wählen Sie die Datenbankobjekte verwenden. Da das Entity Model nur die Tabellen der Datenbank verwendet wird, wählen Sie die **Tabellen** aus, und stellen Sie sicher, dass die **Fremdschlüsselspalten in das Modell einzuschließen** und **in den Singular oder Plural setzen generiert Objektnamen** Optionen werden ebenfalls ausgewählt. Ändern Sie den Modell-Namespace zu **MvcMusicStore.Model** , und klicken Sie auf **Fertig stellen**.

    ![Auswählen der Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Datenbankobjekte auswählen")

    *Datenbankobjekte auswählen*

    > [!NOTE]
    > Wenn eine Sicherheitswarnung-Dialogfeld angezeigt wird, klicken Sie auf **OK** führen Sie die Vorlage und die Klassen für die Modellelemente zu generieren.
8. Einem Entitätsdiagramm für die Datenbank wird angezeigt, während eine separate Klasse, die ordnet jede Tabelle in der Datenbank erstellt wird. Z. B. die **Alben** Tabelle wird dargestellt werden, indem ein **Album** -Klasse, in dem jede Spalte in der Tabelle einer Klasseneigenschaft zugeordnet wird. Dadurch können Sie Abfragen und Arbeiten mit Objekten, die Zeilen in der Datenbank darstellen.

    ![Entitätsdiagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entitätsdiagramm")

    *Entitätsdiagramm*

    > [!NOTE]
    > Die T4-Vorlagen (.tt) führen Sie Code zum Generieren der Entitäten und überschreibt die vorhandenen Klassen mit dem gleichen Namen. In diesem Beispiel die Klassen &quot;Album&quot;, &quot;"Genre"&quot; und &quot;Interpreten&quot; überschrieben wurden, mit dem generierten Code.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Aufgabe 3: Erstellen der Anwendung

In dieser Aufgabe Sie prüft, dass zwar der Erstellung der Modelle wurden entfernt. die **Album**, **"Genre"** und **Interpreten** Modellklassen und das Projekt erstellt wurde erfolgreich mithilfe von die neuen Data Model-Klassen.

1. Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.

    ![Beim Erstellen des Projekts](aspnet-mvc-4-models-and-data-access/_static/image13.png "beim Erstellen des Projekts")

    *Beim Erstellen des Projekts*
2. Das Projekt wird erfolgreich erstellt. Warum funktioniert es weiterhin? Es funktioniert, da die Datenbanktabellen Felder verfügen, die die Eigenschaften, die Sie verwendet haben, in den entfernten Klassen **Album** und **"Genre"**.

    ![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich")

    *Builds erfolgreich*
3. Der Designer die Entitäten in einem Diagramm-Format angezeigt wird, sind eigentlich c#-Klassen. Erweitern Sie die **StoreDB.edmx** Knoten im Projektmappen-Explorer und dann **StoreDB.tt**, sehen Sie die neuen generierten Entitäten.

    ![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "generierte Dateien")

    *Generierte Dateien*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, anstatt hartcodierte Daten, es die Datenbank zum Abrufen der Informationen abfragt.

1. Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld hinzu, auf die Klasse, um eine Instanz von aufzunehmen der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:

    (Codeausschnitt - *Modelle und Datenzugriff – Ex1 StoreDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank verfügbar. Update **Durchsuchen** Aktionsmethode zum Abrufen einer "Genre" mit allen dem **Alben**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store durchsuchen*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Verwenden Sie eine Funktion von .NET namens **LINQ** (Language integrated Query) zum Schreiben von stark typisierten-Abfrageausdrücken für diese Sammlungen - zurück und führen Sie Code für die Datenbank-Objekten, die Sie programmieren können, vor.
    > 
    > Weitere Informationen über LINQ finden Sie auf die [Msdn-Website](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Update **Index** Aktionsmethode, die alle Genres abrufen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Update **Index** Aktionsmethode, die alle Genres abrufen und Transformieren der Auflistung in einer Liste.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen, dass die Indexseite der Store jetzt Genres in die Datenbank anstatt durch die hartcodierte diejenigen gespeichert angezeigt werden. Es ist nicht erforderlich, um die ansichtsvorlage zu ändern, da die **StoreController** gibt die gleichen Entitäten wie zuvor zwar dieses Mal die Daten aus der Datenbank stammen.

1. Erneutes Erstellen von Projektmappen und drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Überprüfen Sie, ob Sie im Menü des **Genres** ist nicht mehr eine feste Liste, und die Daten direkt aus der Datenbank abgerufen.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Nun wechseln Sie zu jedem "Genre", und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.

    ![Durchsuchen Sie Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "durchsuchen Alben aus der Datenbank")

    *Durchsuchen Alben aus der Datenbank*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Übung 2: Erstellen einer Datenbank, die mithilfe von Code First

In dieser Übung lernen Sie, wie Sie die Code First-Ansatz zu verwenden, um eine Datenbank mit den Tabellen der Music Store-Anwendung zu erstellen, und wie auf ihre Daten zugreifen.

Nachdem das Modell generiert wird, ändern Sie die StoreController, um die ansichtsvorlage mit den Daten aus der Datenbank, anstatt hartcodierte Werte bereitzustellen.

> [!NOTE]
> Wenn Sie die Übung 1 abgeschlossen haben und bereits mit dem Database First-Ansatz gearbeitet haben, lernen Sie jetzt die gleichen Ergebnisse mit einem anderen Prozess abrufen. Aufgaben, die gemeinsamen Übung 1 sind wurden gekennzeichnet, um Ihre Lesbarkeit zu erhöhen. Wenn Sie Übung 1 nicht abgeschlossen, aber der Code First-Ansatz erfahren möchten, können Sie aus dieser Übung beginnen und erhalten eine vollständige Abdeckung des Themas.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Aufgabe 1: Auffüllen der Beispieldaten

In dieser Aufgabe füllen Sie die Datenbank mit Beispieldaten bei der gestufte mit Code First-Erstellung.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex2-CreatingADatabaseCodeFirst/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Hinzufügen der **SampleData.cs** -Datei in die **Modelle** Ordner. Zu diesem Zweck Maustaste **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**. Navigieren Sie zu **\Source\Assets** , und wählen Sie die **SampleData.cs** Datei.

    ![Beispieldaten Auffüllen Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispieldaten Auffüllen Code")

    *Beispieldaten Auffüllen code*
3. Öffnen der **"Global.asax.cs"** Datei, und fügen Sie die folgenden *mit* Anweisungen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax Using-Direktiven*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. In der **Anwendung\_Start()** Methode hinzufügen, die folgende Zeile hinzu, legen Sie den Datenbankinitialisierer.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank

Nun, da Sie bereits eine Datenbank zu Ihrem Projekt hinzugefügt haben, Schreiben Sie der **"Web.config"** Datei die Verbindungszeichenfolge.

1. Fügen Sie eine Verbindungszeichenfolge zur **"Web.config"**. Öffnen Sie zu diesem Zweck **"Web.config"** am Stammverzeichnis des Projekts, und ersetzen, die die Verbindungszeichenfolge mit dem Namen DefaultConnection mit der folgenden Zeile in der **&lt;ConnectionStrings&gt;** Abschnitt:

    ![Speicherort der Datei "Web.config"](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei \"Web.config\"")

    *Speicherort der Datei "Web.config"*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Aufgabe 3: mit dem Modell arbeiten

Nun, dass Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit Tabellen der Datenbank. In dieser Aufgabe erstellen Sie eine Klasse, die für die Datenbank mit Code First verknüpft werden soll. Denken Sie daran, dass eine vorhandene POCO-Modell-Klasse, die geändert werden soll, vorhanden ist.

> [!NOTE]
> Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass dieser Schritt von einem Assistenten durchgeführt wurde. Indem Sie Code First, erstellen Sie manuell Klassen, die auf Datenentitäten verknüpft wird.

1. Öffnen Sie die POCO-Klasse **"Genre"** aus **Modelle** Projektordner, und eine-ID einschließen Verwenden Sie eine Int-Eigenschaft mit dem Namen **GenreId**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code zuerst "Genre"*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Zum Arbeiten mit Code First-Konventionen müssen die Klasse "Genre" eine Primärschlüsseleigenschaft, die automatisch erkannt wird.
    > 
    > Erfahren Sie mehr über Code First-Konventionen in diesem [Msdn-Artikel](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Öffnen Sie nun die POCO-Klasse **Album** aus **Modelle** Projektordner und die Fremdschlüssel enthalten, Erstellen von Eigenschaften mit den Namen **GenreId** und  **ArtistId**. Diese Klasse bereits haben die **GenreId** für den Primärschlüssel.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code zuerst Albums*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Öffnen Sie die POCO-Klasse **Interpreten** und enthalten die **ArtistId** Eigenschaft.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Interpreten*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Mit der rechten Maustaste die **Modelle** Projektordner, und wählen **hinzufügen | Klasse**. Nennen Sie die Datei **MusicStoreEntities.cs**. Klicken Sie auf **hinzufügen.**

    ![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")

    *Hinzufügen eines neuen Elements*

    ![Hinzufügen von class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 hinzufügen")

    *Hinzufügen einer Klasse*
5. Öffnen Sie die Klasse, die Sie soeben erstellt haben, **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System.Data.Entity** und **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Ersetzen Sie die Klassendeklaration in erweitern die **"DbContext"** Klasse: deklarieren eine öffentlichen **"DbSet"** und überschreiben **"onmodelcreating"** Methode. Nach diesem Schritt erhalten Sie eine Domänenklasse, die das Modell mit dem Entity Framework verknüpft wird. Zu diesem Zweck ersetzen Sie den Code der Klasse durch Folgendes:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Mit Entity Framework **"DbContext"** und **"DbSet"** können zum Abfragen der POCO-Klasse "Genre". Durch die Erweiterung **"onmodelcreating"** -Methode, die Sie angeben werden, in der **Code** wie "Genre" in einer Datenbanktabelle zugeordnet werden soll. Weitere Informationen zu "DbContext" und "DbSet" finden Sie in diesem Msdn-Artikel: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die Klasse StoreController, anstatt hartcodierte Daten, damit sie aus der Datenbank abgerufen wird.

> [!NOTE]
> Diese Aufgabe ist, enthält, die auch Übung 1.
> 
> Wenn Sie die Übung 1 abgeschlossen sehen Sie diese Schritte sind bei beiden Ansätzen identisch (Datenbank zunächst oder Code first). Sie unterscheiden sich in wie die Daten mit dem Modell verknüpft werden, aber der Zugriff auf Datenentitäten ist noch transparent vom Controller.


1. Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld hinzu, auf die Klasse, um eine Instanz von aufzunehmen der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:

    (Codeausschnitt - *Modelle und Datenzugriff – Ex1 StoreDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank verfügbar. Update **Durchsuchen** Aktionsmethode zum Abrufen einer "Genre" mit allen dem **Alben**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store durchsuchen*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Verwenden Sie eine Funktion von .NET namens **LINQ** (Language integrated Query) zum Schreiben von stark typisierten-Abfrageausdrücken für diese Sammlungen - zurück und führen Sie Code für die Datenbank-Objekten, die Sie programmieren können, vor.
    > 
    > Weitere Informationen über LINQ finden Sie auf die [Msdn-Website](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Update **Index** Aktionsmethode, die alle Genres abrufen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Update **Index** Aktionsmethode, die alle Genres abrufen und Transformieren der Auflistung in einer Liste.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen, dass die Indexseite der Store jetzt Genres in die Datenbank anstatt durch die hartcodierte diejenigen gespeichert angezeigt werden. Besteht keine Notwendigkeit, die die ansichtsvorlage zu ändern, da die **StoreController** gibt dasselbe **StoreIndexViewModel** wie zuvor, aber dieses Mal werden die Daten aus der Datenbank stammen.

1. Erneutes Erstellen von Projektmappen und drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Überprüfen Sie, ob Sie im Menü des **Genres** ist nicht mehr eine feste Liste, und die Daten direkt aus der Datenbank abgerufen.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Nun wechseln Sie zu jedem "Genre", und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.

    ![Durchsuchen Sie Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "durchsuchen Alben aus der Datenbank")

    *Durchsuchen Alben aus der Datenbank*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Übung 3: Abfragen der Datenbank mit Parametern

Klicken Sie in dieser Übung lernen Sie Gewusst wie: Abfragen der Datenbank, die mit Parametern und Vorgehensweise verwenden Sie die Abfrage Ergebnis strukturieren, ein Feature, das die Anzahl Datenbank reduziert Abrufen von Daten in eine effizientere Methode zugreift.

> [!NOTE]
> Weitere Informationen über die Abfrage Ergebnis strukturieren, finden Sie auf die folgenden [Msdn-Artikel](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Aufgabe 1: Ändern von StoreController Alben aus der Datenbank abrufen

In dieser Aufgabe ändern Sie die **StoreController** Klasse Zugriff auf die Datenbank zum Abrufen von Alben aus einem bestimmten Genre.

1. Öffnen der **beginnen** Lösung befindet sich auf die **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** Ordner aus, wenn Sie Code First-Ansatz verwenden möchten oder **Source\ Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** Ordner aus, wenn Sie Database First-Ansatz verwenden möchten. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **StoreController** Klasse so ändern Sie die **Durchsuchen** Aktionsmethode. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
3. Ändern der **Durchsuchen** Aktionsmethode Alben für eine spezifische Genre abrufen. Ersetzen Sie hierzu den folgenden Code ein:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Um eine Auflistung der Entität aufzufüllen, müssen Sie die **Include** Methode, um anzugeben, die Alben zu abgerufen werden soll. Sie können die. **Single()** -Erweiterung in LINQ, da in diesem Fall nur eine "Genre" für ein Album erwartet wird. Die **Single()** Methode nimmt einen Lambdaausdruck als Parameter, die in diesem Fall ein einzelnes Objekt für "Genre" gibt an, so an, dass der Name der definierten Wert entspricht.
> 
> Sie werden eine Funktion nutzen, mit dem Sie an andere verknüpften Entitäten, die ebenfalls geladen werden sollen, wenn das Objekt "Genre" abgerufen wird. Dieses Feature heißt **Abfrage Ergebnis strukturieren**, und ermöglicht es Ihnen, wie oft erforderlich, um Zugriff auf die Datenbank zum Abrufen von Informationen zu reduzieren. In diesem Szenario werden Sie Alben für das Genre vorab abzurufen, die Sie abrufen möchten.
> 
> Die Abfrage enthält **Genres.Include (&quot;Alben&quot;)** an, wenn auch verwandte Alben werden sollen. Dies führt zu einer Anwendung effizienter, da es sowohl "Genre" und "Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und Alben von einer bestimmten "Genre" aus der Datenbank abrufen.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/Store/durchsuchen? Genre = Pop** um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.

    ![Durchsuchen nach Genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "durchsuchen nach Genre")

    *Durchsuchen/Store/durchsuchen? Genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Aufgabe 3: Zugreifen auf Alben nach Id

In dieser Aufgabe werden Sie in der vorherigen Prozedur zum Abrufen von Alben anhand ihrer Id. wiederholen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio zurückzukehren. Öffnen der **StoreController** Klasse so ändern Sie die **Details** Aktionsmethode. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
2. Ändern der **Details** Aktionsmethode beim Abrufen der Details von Alben basierend auf ihren **Id**. Ersetzen Sie hierzu den folgenden Code ein:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser ausführen und erfragen Sie Album anhand ihrer Id.

1. Drücken Sie **F5** zum Ausführen der Anwendung.
2. Das Projekt, die auf der Startseite wird gestartet. Ändern Sie die URL zum **/Store/Details/51** oder Genres durchsuchen, und wählen Sie ein Album, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.

    ![Durchsuchen Sie die Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Informationen durchsuchen")

    */Store/Details/51 durchsuchen*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie gelernt, die Grundlagen von ASP.NET MVC-Modelle und Datenzugriff, indem die **Database First** Ansatz als auch die **Code First** Ansatz:

- Wie Sie die Lösung eine Datenbank hinzugefügt, um die Daten zu verarbeiten.
- Gewusst wie: Aktualisieren der Verwaltungscontroller zum Anzeigen von Vorlagen mit den Daten aus der Datenbank statt einer hartcodierten bereitstellen
- Gewusst wie: Abfragen der Datenbank, die mit Parametern
- Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturieren, ein Feature, das die Anzahl von Datenbankzugriffe reduziert das Abrufen von Daten in eine effizientere Methode
- Wie Sie Database First und Code First-Ansätze in Microsoft Entity Framework zu verwenden, um die Datenbank mit dem Modell verknüpfen

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](aspnet-mvc-4-models-and-data-access/_static/image34.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image37.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image38.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-models-and-data-access/_static/image45.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image49.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-models-and-data-access/_static/image52.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-models-and-data-access/_static/image53.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-models-and-data-access/_static/image54.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-models-and-data-access/_static/image55.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-models-and-data-access/_static/image56.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
