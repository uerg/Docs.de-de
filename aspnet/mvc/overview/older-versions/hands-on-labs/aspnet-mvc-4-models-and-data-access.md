---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4-Modellen und Datenzugriff | Microsoft Docs
author: rick-anderson
description: "Hinweis: Diese praktische Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC haben. Wenn Sie nicht ASP.NET MVC, bevor Sie verwendet haben, empfehlen wir Sie ASP.NET MVC 4 besprochen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bf4bb5c6f9ad8339c3597b0d6666c7077ac476e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4-Modellen und Datenzugriff
====================
durch [Web Lager Team](https://twitter.com/webcamps)

> [!NOTE]
> Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**. Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC 4-Grundlagen** praktische Übungseinheit.
> 
> Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).


In **ASP.NET MVC-Grundlagen** praktische Übungseinheit, Sie haben wurde übergeben hartcodierte Daten durch den Controller, um die Vorlagen anzeigen. Allerdings um eine echte Webanwendung zu erstellen, sollten Sie eine echte Datenbank verwendet.

Diese praktische Übungseinheit wird gezeigt, wie ein Datenbankmodul zum Speichern und Abrufen der erforderlichen Daten für die Music Store-Anwendung verwenden. Um zu erreichen, dass Sie mit einer vorhandenen Datenbank startet und das Entity Data Model daraus erstellen. In dieser Übungseinheit erfüllen Sie die **Database First** Ansatz als auch die **Code First** Ansatz.

Aber Sie können auch die **Model First** Ansatz, erstellen Sie das gleiche Modell mit den Tools und die Datenbank dann daraus zu generieren.

![Datenbank erste im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Vs. Model First")

*Datenbank erste im Vergleich zu Model First*

Nach dem Generieren des Modells, sind Sie die entsprechenden Korrekturen der StoreController die Store-Ansichten mit den Daten aus der Datenbank, anstelle von hartcodierten Daten bereitstellen werden. Sie müssen keine die Ansichtsvorlagen ändern, da die StoreController an den Vorlagen anzeigen, die gleichen ViewModels zurückgegeben werden sollen jedoch dieses Mal die Daten aus der Datenbank stammen.

**Der erste Ansatz von Code**

Der Code First-Ansatz erlaubt es uns so definieren Sie das Modell aus dem Code, ohne das Generieren von Klassen, die in der Regel mit dem Framework verbunden sind.

Im Code zuerst Modellobjekte mit POCOs, definiert werden &quot;Plain Old CLR Objects&quot;. POCOs sind einfache einfache Klassen, die keine Vererbung und Schnittstellen nicht implementieren. Wir können die Datenbank automatisch generieren, von ihnen oder wir können eine vorhandene Datenbank verwenden und die Zuordnung für die Klasse aus dem Code zu generieren.

Die Vorteile der Verwendung dieser Ansatz ist, dass das Modell wie die POCOs Klassen nicht mit der Zuordnung Framework gekoppelt sind unabhängig von der Persistenz-Framework (in diesem Fall Entity Framework), bleibt.

> [!NOTE]
> In dieser Umgebung basiert auf ASP.NET MVC 4 und eine Version der beispielanwendung Music Store angepasst und nur die Funktionen, die in dieser praktischen Übungseinheit gezeigt entsprechend minimiert.
> 
> Wenn Sie die gesamte untersuchen möchten **Music Store** lernprogrammanwendung Sie in finden [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).


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

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:

1. [Übung 1: Hinzufügen einer Datenbank](#Exercise1)
2. [Übung 2: Erstellen einer Datenbank mithilfe von Code First](#Exercise2)
3. [Übung 3: Abfragen der Datenbank mit Parametern](#Exercise3)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **35 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Übung 1: Hinzufügen einer Datenbank

In dieser Übung erfahren Sie, wie zum Hinzufügen einer Datenbank mit den Tabellen der Anwendung MusicStore der Projektmappe um die Daten zu nutzen. Sobald die Datenbank mit dem Modell generiert, und der Projektmappe hinzugefügt, ändern Sie die StoreController-Klasse, um die Vorlage für die Ansicht mit den Daten aus der Datenbank, anstelle von hartcodierten Werte geben.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Aufgabe 1: Hinzufügen einer Datenbank

In dieser Aufgabe fügen Sie eine bereits erstellte Datenbank mit den wichtigsten Tabellen der Anwendung MusicStore zur Projektmappe hinzu.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-AddingADatabaseDBFirst/Begin/** Ordner.

    1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Hinzufügen **MvcMusicStore** Datenbankdatei. In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen **MvcMusicStore.mdf**. Zu diesem Zweck Maustaste **App\_Daten** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**. Navigieren Sie zu **\Source\Assets** , und wählen Sie die **MvcMusicStore.mdf** Datei.

    ![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "ein vorhandenes Element hinzufügen")

    *Ein vorhandenes Element hinzufügen*

    ![Datenbankdatei MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf-Datenbankdatei")

    *MvcMusicStore.mdf-Datenbankdatei*

    Die Datenbank wurde dem Projekt hinzugefügt. Selbst wenn die Datenbank innerhalb der Lösung befindet, können Sie Abfragen und aktualisieren Sie es an, wie er in einen anderen Datenbankserver gehostet wurde.

    ![MvcMusicStore Datenbank im Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore Datenbank im Projektmappen-Explorer")

    *MvcMusicStore Datenbank im Projektmappen-Explorer*
3. Überprüfen Sie die Verbindung mit der Datenbank. Doppelklicken Sie hierzu auf **MvcMusicStore.mdf** zum Herstellen einer Verbindung.

    ![Herstellen einer Verbindung mit MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Herstellen einer Verbindung mit MvcMusicStore.mdf")

    *Herstellen einer Verbindung mit MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Aufgabe 2: erstellen ein Datenmodell

In dieser Aufgabe erstellen Sie ein Datenmodell, um die Interaktion mit der Datenbank, die in der vorherigen Aufgabe hinzugefügt.

1. Erstellen Sie ein Datenmodell, die die Datenbank darstellen. Klicken Sie dazu im Projektmappen-Explorer mit der rechten Maustaste die **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Vorlage und dann die **ADO.NET Entity Data Model** Element. Ändern Sie den Namen des Daten-Modell zu **StoreDB.edmx** , und klicken Sie auf **hinzufügen**.

    ![Hinzufügen von StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET Entity Data Model hinzufügen")

    *Hinzufügen von StoreDB ADO.NET Entity Data Model*
2. Die **Entity Data Model-Assistenten** wird angezeigt. Dieser Assistent führt Sie durch die Erstellung der Modellebene. Da das Modell basierend auf der vorhandenen Datenbank Recentyl hinzugefügt erstellt werden soll, wählen **aus Datenbank generieren** , und klicken Sie auf **Weiter**.

    ![Auswählen des Modellinhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "den Modellinhalt auswählen")

    *Den Modellinhalt auswählen*
3. Da zum Generieren eines Modells aus einer Datenbank müssen Sie die zu verwendende Verbindung an. Klicken Sie auf **neue Verbindung**.
4. Wählen Sie **Microsoft SQL Server-Datenbankdatei** , und klicken Sie auf **Fortfahren**.

    ![Wählen Sie die Datenquelle](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")

    *Dialogfeld "Quelle" Daten auswählen*
5. Klicken Sie auf **Durchsuchen** und wählen Sie die Datenbank **MvcMusicStore.mdf** Sie befindet sich der **App\_Daten** Ordner, und klicken Sie auf **OK**.

    ![Verbindungseigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")

    *Verbindungseigenschaften*
6. Die generierte Klasse sollte haben den gleichen Namen wie die Verbindungszeichenfolge für die Entität, so ändern Sie den Namen in **MusicStoreEntities** , und klicken Sie auf **Weiter**.

    ![Wählen die Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "die Datenverbindung auswählen")

    *Die Datenverbindung auswählen*
7. Wählen Sie die Datenbankobjekte zu verwenden. Wie das Entitätsmodell nur die Tabellen der Datenbank verwenden, wählen Sie die **Tabellen** aus, und stellen Sie sicher, dass die **Fremdschlüsselspalten in das Modell einbeziehen** und **in den Singular oder Plural setzen generiert Objektnamen** -Optionen sind auch ausgewählt. Ändern Sie den Modell-Namespace zu **MvcMusicStore.Model** , und klicken Sie auf **Fertig stellen**.

    ![Wählen die Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Datenbankobjekte auswählen")

    *Datenbankobjekte auswählen*

    > [!NOTE]
    > Wenn eine Sicherheitswarnungs-Dialogfeld angezeigt wird, klicken Sie auf **OK** führen Sie die Vorlage und die Klassen für die Modellelemente zu generieren.
8. Eine Entitätsdiagramm für die Datenbank wird angezeigt, während eine separate Klasse, die ordnet jede Tabelle in der Datenbank erstellt wird. Z. B. die **Alben** Tabelle dargestellte ein **Album** -Klasse, in dem jede Spalte in der Tabelle an Klasseneigenschaften zugeordnet wird. Dadurch können Sie Abfragen und Arbeiten mit Objekten, die Zeilen in der Datenbank darstellen.

    ![Entitätsdiagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entität-Diagramm")

    *Entitätsdiagramm*

> [!NOTE]
> Die T4-Vorlagen (TT) Code ausführen, um die Entitäten Klassen zu generieren und überschreibt die vorhandenen Klassen mit dem gleichen Namen. In diesem Beispiel die Klassen &quot;Album&quot;, &quot;"Genre"&quot; und &quot;Interpreten&quot; durch den generierten Code außer Kraft gesetzt wurden.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Aufgabe 3: Erstellen der Anwendung

In dieser Aufgabe werden überprüfen Sie, dass zwar die modellgenerierung wurden entfernt. die **Album**, **"Genre"** und **Interpreten** Modellklassen, das Projekt erfolgreich erstellt mit das neue Modell Datenklassen.

1. Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.

    ![Beim Erstellen des Projekts](aspnet-mvc-4-models-and-data-access/_static/image13.png "beim Erstellen des Projekts")

    *Beim Erstellen des Projekts*
2. Das Projekt erstellt erfolgreich. Warum ist es weiterhin funktionsfähig? Es funktioniert, da die Datenbanktabellen Felder verfügen, die die Eigenschaften, die Sie verwendet haben, in den entfernten Klassen **Album** und **"Genre"**.

    ![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich war")

    *Builds erfolgreich war*
3. Während der Designer die Entitäten in einem Diagramm Format angezeigt wird, sind sie wirklich über C#-Klassen. Erweitern Sie die **StoreDB.edmx** Knoten im Projektmappen-Explorer und dann **StoreDB.tt**, sehen Sie die neue erzeugten Entitäten.

    ![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "generierte Dateien")

    *Generierte Dateien*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, anstatt Sie hartcodierte Daten verwenden, es die Datenbank zum Abruf der Informationen abfragt.

1. Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld der Klasse zum Halten von einer Instanz von der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:

    (Codeausschnitt - *Modelle und des Datenzugriffs - Ex1 StoreDB*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank. Update **Durchsuchen** Aktionsmethode einen "Genre" mit allen Abrufen der **Alben**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store durchsuchen*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Verwenden Sie eine Funktion von .NET aufgerufen **LINQ** (Language-integrated Query), stark typisierte Abfrageausdrücke anhand dieser Sammlungen - schreiben Code für die Datenbank ausgeführt und zurückgegeben-Objekten, die Sie programmieren können vor.
    > 
    > Weitere Informationen über LINQ finden Sie auf der [Msdn-Website](https://msdn.microsoft.com/en-us/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Update **Index** Aktionsmethode, um alle Genres abzurufen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Columnstore-Index*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Update **Index** Aktionsmethode alle Genres abrufen und Transformieren von der Auflistung auf eine Liste.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store GenreMenu*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen Sie, dass die Columnstore-Index-Seite die in der Datenbank anstelle der hartcodiert diejenigen gespeicherten Genres jetzt angezeigt werden. Besteht keine Notwendigkeit zum Ändern der Vorlage anzeigen, da die **StoreController** ist Zurückgeben der gleichen Entitäten wie zuvor zwar diesmal die Daten aus der Datenbank stammen.

1. Erneutes Erstellen von Projektmappen und drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Überprüfen Sie, ob das Menü des **Genres** ist nicht mehr eine hartcodierte Liste, und die Daten direkt aus der Datenbank abgerufen.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Jetzt alle "Genre" und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.

    ![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "durchsuchen Alben aus der Datenbank")

    *Durchsuchen von Alben aus der Datenbank*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Übung 2: Erstellen einer Datenbank zunächst mithilfe von Code

In dieser Übung erfahren Sie, wie die Code First-Ansatz zu verwenden, um eine Datenbank mit den Tabellen der MusicStore Anwendung erstellen und zum Zugreifen auf ihre Daten.

Nachdem das Modell generiert wird, ändern Sie die StoreController, um die Vorlage für die Ansicht mit den Daten aus der Datenbank, anstelle von hartcodierten Werte bereitzustellen.

> [!NOTE]
> Wenn Sie Übung 1 abgeschlossen haben und bereits mit dem Database First-Ansatz funktioniert haben, erfahren Sie jetzt die gleichen Ergebnisse mit einem anderen Prozess abrufen. Aufgaben, die auch in zahlreichen Übung 1 sind wurden gekennzeichnet, um die Lesbarkeit zu erhöhen. Wenn Sie Übung 1 wurden nicht abgeschlossen, aber die Code First-Ansatz erfahren möchten, können Sie aus dieser Übung beginnen und erhalten eine vollständige Abdeckung des Themas.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Aufgabe 1: Auffüllen von Beispieldaten

In dieser Aufgabe werden Sie die Datenbank mit Beispieldaten aufgefüllt, wenn sie mithilfe von Code First gestufte erstellt wird.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex2-CreatingADatabaseCodeFirst/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Hinzufügen der **SampleData.cs** Datei wird in der **Modelle** Ordner. Zu diesem Zweck Maustaste **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**. Navigieren Sie zu **\Source\Assets** , und wählen Sie die **SampleData.cs** Datei.

    ![Beispieldaten aufzufüllen Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispieldaten aufzufüllen, Code")

    *Beispieldaten aufzufüllen, code*
3. Öffnen der **Global.asax.cs** Datei, und fügen Sie die folgenden *mit* Anweisungen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax Using-Direktiven*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. In der **Anwendung\_Start()** Methode fügen Sie die folgende Zeile zum Festlegen der datenbankinitialisierers hinzu.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax SetInitializer*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank

Nun, dass Sie unseren Projekt bereits eine Datenbank hinzugefügt haben, Schreiben Sie der **"Web.config"** Datei die Verbindungszeichenfolge.

1. Fügen Sie eine Verbindungszeichenfolge zur **"Web.config"**. Öffnen Sie hierzu **"Web.config"** am Projektstamm und Ersetzen Sie die Verbindungszeichenfolge mit dem Namen DefaultConnection durch diese Zeile in der  **&lt;ConnectionStrings&gt;**  Abschnitt:

    ![Speicherort der Datei "Web.config"](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei "Web.config"")

    *Speicherort der Datei "Web.config"*


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Aufgabe 3: Arbeiten mit dem Modell

Nun, dass Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit Tabellen der Datenbank. In dieser Aufgabe erstellen Sie eine Klasse, die die Datenbank mit dem Code First verknüpft werden soll. Denken Sie daran, dass eine vorhandene POCO-Modell-Klasse, die geändert werden soll, vorhanden ist.

> [!NOTE]
> Wenn Sie Übung 1 abgeschlossen haben, werden Sie bemerken, dass dieser Schritt von einem Assistenten durchgeführt wurde. Indem Sie Code First, erstellen Sie manuell Klassen, die Datenentitäten verknüpft werden soll.


1. Öffnen Sie die POCO-Modellklasse **"Genre"** aus **Modelle** Projektordner und enthalten eine-ID. Verwenden Sie eine Int-Eigenschaft mit dem Namen **GenreId**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste "Genre"*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Zum Arbeiten mit Code First-Konventionen benötigen die Klasse "Genre" eine Primärschlüsseleigenschaft, die automatisch erkannt wird.
    > 
    > Erfahren Sie mehr über Code First-Konventionen in diesem [Msdn-Artikel](https://msdn.microsoft.com/en-us/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Öffnen Sie das Modell POCO-Klasse jetzt **Album** aus **Modelle** Projektordner und die Fremdschlüssel enthalten, Erstellen von Eigenschaften mit den Namen **GenreId** und  **ArtistId**. Diese Klasse bereits haben die **GenreId** für den Primärschlüssel.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Album*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Öffnen Sie die POCO-Modellklasse **Interpreten** und enthalten die **ArtistId** Eigenschaft.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Interpreten*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Mit der rechten Maustaste die **Modelle** Projektordner, und wählen **hinzufügen | Klasse**. Nennen Sie die Datei **MusicStoreEntities.cs**. Klicken Sie auf **hinzufügen.**

    ![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")

    *Ein neues Element hinzufügen*

    ![Hinzufügen von class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 hinzufügen")

    *Hinzufügen einer Klasse*
5. Öffnen Sie die Klasse, die Sie soeben erstellt haben, **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System.Data.Entity** und **System.Data.Entity.Infrastructure**.


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Ersetzen Sie die Deklaration der Klasse zum Erweitern der **DbContext** Klasse: deklarieren einen öffentlichen **DBSet** und überschreiben **OnModelCreating** Methode. Nach diesem Schritt erhalten Sie eine Domänenklasse, die das Modell mit dem Entity Framework verknüpft wird. Ersetzen Sie zu diesem Zweck des Klasse Codes durch Folgendes:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste MusicStoreEntities*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > Mit Entity Framework **DbContext** und **DBSet** werden POCO-Klasse "Genre" Abfragen. Durch die Erweiterung **OnModelCreating** -Methode, geben Sie der **Code** wie "Genre" in einer Datenbanktabelle zugeordnet werden. Weitere Informationen zu ' DbContext ' und ' DbSet ' finden Sie in diesem Msdn-Artikel: [Link](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die StoreController-Klasse so, dass anstelle von hartcodierten Daten sie es aus der Datenbank abgerufen werden.

> [!NOTE]
> Diese Aufgabe ist auch in zahlreichen Übung 1.
> 
> Wenn Sie Übung 1 abgeschlossen werden Sie bemerken, diese Schritte sind identisch in beiden Ansätzen (Datenbank zunächst oder Code first). Sie unterscheiden sich in wie die Daten mit dem Modell verknüpft ist, der Zugriff auf Datenentitäten ist jedoch noch transparent vom Controller.


1. Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld der Klasse zum Halten von einer Instanz von der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:

    (Codeausschnitt - *Modelle und des Datenzugriffs - Ex1 StoreDB*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank. Update **Durchsuchen** Aktionsmethode einen "Genre" mit allen Abrufen der **Alben**.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store durchsuchen*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Verwenden Sie eine Funktion von .NET aufgerufen **LINQ** (Language-integrated Query), stark typisierte Abfrageausdrücke anhand dieser Sammlungen - schreiben Code für die Datenbank ausgeführt und zurückgegeben-Objekten, die Sie programmieren können vor.
    > 
    > Weitere Informationen über LINQ finden Sie auf der [Msdn-Website](https://msdn.microsoft.com/en-us/library/bb397926(v=vs.110).aspx).
3. Update **Index** Aktionsmethode, um alle Genres abzurufen.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Columnstore-Index*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Update **Index** Aktionsmethode alle Genres abrufen und Transformieren von der Auflistung auf eine Liste.

    (Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store GenreMenu*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen Sie, dass die Columnstore-Index-Seite die in der Datenbank anstelle der hartcodiert diejenigen gespeicherten Genres jetzt angezeigt werden. Besteht keine Notwendigkeit zum Ändern der Vorlage anzeigen, da die **StoreController** ist die gleiche zurückgeben **StoreIndexViewModel** wie zuvor, dieses Mal jedoch die Daten aus der Datenbank stammen.

1. Erneutes Erstellen von Projektmappen und drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Überprüfen Sie, ob das Menü des **Genres** ist nicht mehr eine hartcodierte Liste, und die Daten direkt aus der Datenbank abgerufen.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Jetzt alle "Genre" und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.

    ![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "durchsuchen Alben aus der Datenbank")

    *Durchsuchen von Alben aus der Datenbank*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Übung 3: Abfragen der Datenbank mit Parametern

In dieser Übung erfahren Sie, zum Abfragen der Datenbank mithilfe von Parametern und Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturiert werden, eine Funktion, die die Zahl Datenbank reduziert Abrufen von Daten in eine effizientere Methode zugreift.

> [!NOTE]
> Weitere Informationen zu Abfrage Ergebnis strukturiert werden, finden Sie auf der folgenden [Msdn-Artikel](https://msdn.microsoft.com/en-us/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Aufgabe 1: Ändern von StoreController Alben aus Datenbank abrufen

In dieser Aufgabe ändern Sie die **StoreController** Klasse Zugriff auf die Datenbank zum Abrufen von Alben aus einem bestimmten "Genre".

1. Öffnen der **beginnen** Lösung finden Sie unter der **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** Ordner, wenn Sie Code First-Ansatz verwenden möchten oder **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** Benutzerordner, falls Sie Database First-Ansatz verwenden möchten. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **StoreController** -Klasse zum Ändern der **Durchsuchen** Aktionsmethode. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
3. Ändern der **Durchsuchen** Aktionsmethode Alben für einen bestimmten "Genre" abgerufen. Ersetzen Sie hierzu den folgenden Code ein:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > Um eine Auflistung der Entität aufzufüllen, müssen Sie die **Include** Methode, um anzugeben, die Alben zu abgerufen werden soll. Sie können die. **Single()** Erweiterung in LINQ, da in diesem Fall nur ein "Genre" für ein Album erwartet wird. Die **Single()** Methode nimmt einen Lambda-Ausdruck als Parameter, die in diesem Fall ein einzelnes Objekt für "Genre" gibt an, sodass ihrem Namen den definierten Wert entspricht.
    > 
    > Sie werden eine Funktion nutzen, die Ihnen die Möglichkeit, andere verknüpften Entitäten angezeigt werden, die ebenfalls geladen werden sollen, wenn das Objekt "Genre" abgerufen wird. Diese Funktion wird aufgerufen, **Abfrage Ergebnis strukturieren**, und ermöglicht es Ihnen, wie oft erforderlich, um Zugriff auf die Datenbank zum Abrufen von Informationen zu reduzieren. In diesem Fall sollten Sie die Alben für die "Genre" vorab abzurufen, die Sie abrufen.
    > 
    > Die Abfrage enthält **Genres.Include (&quot;Alben&quot;)** um anzugeben, dass verwandte Alben ebenfalls verwendet werden soll. Dies führt zu einer Anwendung eine effizientere, da er "Genre" und Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe werden Sie die Anwendung auszuführen und Alben mit einem bestimmten "Genre" aus der Datenbank abrufen.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/Store/durchsuchen? "Genre" Pop =** um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.

    ![Durchsuchen nach "Genre"](aspnet-mvc-4-models-and-data-access/_static/image24.png "durchsuchen nach "Genre"")

    *Durchsuchen/Store/durchsuchen? "Genre" Pop =*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Aufgabe 3: Zugreifen auf Alben nach Id

In dieser Aufgabe werden Sie in der vorherigen Prozedur zum Abrufen von Alben anhand ihrer Id. wiederholen.

1. Schließen Sie den Browser, die ggf. zu Visual Studio zurückzukehren. Öffnen der **StoreController** -Klasse zum Ändern der **Details** Aktionsmethode. Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.
2. Ändern der **Details** Aktionsmethode beim Abrufen der Details Alben basierend auf ihren **Id**. Ersetzen Sie hierzu den folgenden Code ein:

    (Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe wird die Anwendung in einem Webbrowser ausführen und erhalten Album anhand ihrer Id.

1. Drücken Sie **F5** um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL zum **/Store/Details/51** oder die Genres durchsuchen, und wählen Sie ein Album, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.

    ![Durchsuchen von Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Details durchsuchen")

    */Store/Details/51 durchsuchen*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben Sie gelernt, die Grundlagen von ASP.NET MVC-Modelle und Datenzugriff abschließen, indem Sie die **Database First** Ansatz als auch die **Code First** Ansatz:

- Wie die Lösung eine Datenbank hinzugefügt, um die Daten zu nutzen.
- Gewusst wie: Aktualisieren der Verwaltungscontroller um Ansichtsvorlagen mit den Daten aus der Datenbank statt hartcodierten bereitzustellen
- Gewusst wie: Abfragen der Datenbank, die mit Parametern
- Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturiert werden, eine Funktion, die reduziert die Anzahl der Zugriff auf die Datenbank, Abrufen von Daten in eine effizientere Methode
- Gewusst wie: Database First und Code First Ansätze in Microsoft Entity Framework zu verwenden, um die Datenbank mit dem Modell verknüpfen

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](aspnet-mvc-4-models-and-data-access/_static/image34.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image37.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image38.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-models-and-data-access/_static/image45.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

    - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
    - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
    - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
    - Geben Sie einen neuen Datenbanknamen ein.

    ![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Zielverbindungszeichenfolge konfigurieren")

    *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-models-and-data-access/_static/image49.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-models-and-data-access/_static/image52.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-models-and-data-access/_static/image53.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-models-and-data-access/_static/image54.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-models-and-data-access/_static/image55.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-models-and-data-access/_static/image56.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
