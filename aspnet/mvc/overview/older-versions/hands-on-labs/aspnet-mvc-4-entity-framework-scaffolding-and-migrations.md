---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen | Microsoft Docs
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder ein abgeschlossener der &quot;-Hilfsprogrammen, Formulare und Validierung&quot; praktische Übungseinheit sollten beachtet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen

Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](https://aka.ms/webcamps-training-kit)

Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder ein abgeschlossener der &quot;-Hilfsprogrammen, Formulare und Validierung&quot; praktische Übungseinheit, Sie sollten sich bewusst sein, dass der Großteil der Logik zum Erstellen, aktualisieren, auflisten und entfernen alle Datenentität, er wird wiederholt, zwischen der Anwendung. Nicht, um, die verdeutlichen, hat Ihr Modell mehrere Klassen zum Bearbeiten, werden Sie wahrscheinlich eine sehr viel Zeit mit dem Schreiben von Aktionsmethoden POST- und GET für jeden Vorgang Entität sowie einzelnen Ansichten ausgeben.

In dieser Übung erfahren Sie, wie Sie das Gerüst für ASP.NET MVC 4 verwenden, um die Baseline der Ihre Anwendung CRUD (Create, Read, Update und Delete) automatisch zu generieren. Starten eine einfache Modellklasse, und ohne eine einzige Codezeile schreiben zu müssen, erstellen Sie einen Controller, der alle CRUD-Vorgänge sowie die alle erforderlichen Ansichten enthalten soll. Nach dem Erstellen und Ausführen der einfache Lösung, müssen Sie die Anwendungsdatenbank generiert, zusammen mit der MVC-Logik und die Ansichten für die Bearbeitung von Daten.

Darüber hinaus erfahren Sie, wie einfach es ist mit Entity Framework Migrationen modellaktualisierungen in der gesamten Anwendung durchführen. Migrationen von Entity Framework können Sie Ihre Datenbank zu ändern, nachdem das Modell mit einfachen Schritten geändert hat. Mit allen diesen Denken Sie daran werden Sie möglicherweise zum Erstellen und Verwalten von Webanwendungen effizienter nutzen die neuesten Funktionen von ASP.NET MVC 4.

> [!NOTE]
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit zur Nächten enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für diese Übung finden Sie unter [Migrationen und ASP.NET MVC 4 Entity Framework Gerüstbau](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden Sie ASP.NET Gerüstbau für CRUD-Vorgänge im Controller.
- Das Datenbankmodell mithilfe von Entity Framework-Migrationen zu ändern.

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

In der folgenden Übung bilden diese praktische Übungseinheit:

1. [Mithilfe von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen](#Exercise1)

> [!NOTE]
> In dieser Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe bei der Arbeit in der Übung benötigen.


Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen

ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit zum Generieren von CRUD-Vorgänge in eine standardisierte Möglichkeit, erstellen die erforderliche Logik, die Ihre Anwendung mit der Datenbankebene interagieren kann.

In dieser Übung erfahren Sie, wie mit ASP.NET MVC 4-Gerüstbau mit Code zuerst die CRUD-Methoden erstellt werden. Anschließend erfahren Sie, wie das Modell anwenden der Änderungen in der Datenbank mithilfe von Entity Framework Migrationen aktualisieren.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Aufgabe 1 – Erstellen einer neuen ASP.NET MVC 4-Projekt mithilfe von Gerüstbau

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.
2. Wählen Sie **Datei | Neues Projekt**. In das neue Projekt im Dialogfeld unter die **Visual C# | Web** Abschnitt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt zu **MVC4andEFMigrations** und legen Sie den Speicherort **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** Ordner dieser Anleitung. Legen Sie die **Projektmappenname** auf **beginnen** und stellen Sie sicher **Projektmappenverzeichnis erstellen** aktiviert ist. Klicken Sie auf **OK**.

    ![Neues ASP.NET MVC 4-Projekt (Dialogfeld)](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")

    *Neues ASP.NET MVC 4-Projekt (Dialogfeld)*
3. In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage, und stellen Sie sicher, dass **Razor** wird dem ausgewählten **Ansichtsmodul**. Klicken Sie auf **OK**, um das Projekt zu erstellen.

    ![Neues ASP.NET MVC 4-Internetanwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "neue ASP.NET MVC 4-Internetanwendung")

    *Neues ASP.NET MVC 4-Internetanwendung*
4. Klicken Sie im Projektmappen-Explorer mit der Maustaste **Modelle** , und wählen Sie **hinzufügen | Klasse** zum Erstellen einer einfachen Klasse Person (POCO). Nennen Sie sie **Person** , und klicken Sie auf **OK**.
5. Öffnen Sie die Person-Klasse, und fügen Sie die folgenden Eigenschaften.

    (Codeausschnitt - *ASP.NET MVC 4 und Entity Framework Migrationen - Ex1 Person Eigenschaften*)


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. Klicken Sie auf **erstellen | Projektmappe** zum Speichern der Änderungen, und erstellen Sie das Projekt.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
7. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Ordners Controller, und wählen **hinzufügen | Controller**.
8. Nennen Sie den Controller *PersonController* und schließen Sie die **gerüstoptionen** mit den folgenden Werten.

   1. In der **Vorlage** Dropdown-Liste der **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework** Option.
   2. In der **Modellschemas** Dropdown-Liste der **Person** Klasse.
   3. In der **Datenkontext Klasse** Liste  **&lt;neuen Datenkontext... &gt;**. Wählen Sie einen beliebigen Namen, und klicken Sie auf **OK**.
   4. In der **Ansichten** Dropdown-Liste, stellen Sie sicher, dass **Razor** ausgewählt ist.

      ![Die Person-Controller mit Gerüst hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "den Person-Controller mit Gerüst hinzufügen")

      *Die Person-Controller mit Gerüst hinzufügen*
9. Klicken Sie auf **hinzufügen** So erstellen Sie den neuen Controller für Personen mit Gerüstbau. Sie haben jetzt die Controlleraktionen sowie die Ansichten erstellt.

    ![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "nach dem Erstellen des Person-Controllers mit Gerüstbau")

    *Nach dem Erstellen des Person-Controllers mit Gerüstbau*
10. Open **PersonController** Klasse. Beachten Sie, dass die vollständige CRUD-Aktionsmethoden automatisch generiert wurden.

   ![In den Person-Controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "innerhalb der Person-Controller")

   *In den Person-controller*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Aufgabe 2 ausgeführte Anwendung

An diesem Punkt ist die Datenbank noch nicht erstellt. In dieser Aufgabe werden Sie die Anwendung zum ersten Mal ausführen und Testen von CRUD-Vorgänge. Die Datenbank wird im Handumdrehen mit Code First erstellt werden.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Fügen Sie in den Browser **/Person** an die URL der Seite "Person" zu öffnen.

    ![Anwendung – erste Ausführung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung – erste Ausführung")

    *Die Anwendung: Führen Sie zuerst*
3. Sie werden jetzt Untersuchen der Person-Seiten und die CRUD-Vorgänge zu testen.

    1. Klicken Sie auf **neu erstellen** eine neue Person hinzufügen. Geben Sie einen Vornamen und einen Nachnamen enthalten, und klicken Sie auf **erstellen**.

        ![Eine neue Person hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "eine neue Person hinzufügen")

        *Eine neue Person hinzufügen*
    2. In der Liste der Person Sie löschen, bearbeiten oder Hinzufügen von Elementen.

        ![Personenliste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Personenliste")

        *Personenliste*
    3. Klicken Sie auf **Details** zu der Person Details zu öffnen.

        ![Details der Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person-Details")

        *Der Person-details*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück. Beachten Sie, dass Sie die gesamte CRUD-Vorgänge für die Personenentität in der gesamten - vom Modell zu den Ansichten - Anwendung erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Aufgabe 3-Aktualisierung der Datenbank mithilfe von Entity Framework-Migrationen

In dieser Aufgabe aktualisieren Sie die Datenbank mithilfe von Entity Framework Migrationen. Sie werden feststellen, wie einfach es ist, ändern Sie das Modell und Änderungen in den Datenbanken mithilfe der Entity Framework Migrationen-Funktion.

1. Öffnen Sie die Paket-Manager-Konsole. Wählen Sie **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**.
2. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Migrationen aktivieren")

    *Aktivieren von Migrationen*

    Der Enable-Migrations-Befehl erstellt die **Migrationen** Ordner, in dem ein Skript zum Initialisieren der Datenbank enthalten.

    ![Ordner](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Ordner")

    *Ordner*
3. Öffnen der **"Configuration.cs"** -Datei in den Ordner. Suchen Sie den Klassenkonstruktor, und Ändern der **AutomaticMigrationsEnabled** Wert *"true"*.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. Öffnen Sie die Person-Klasse, und fügen Sie ein Attribut für den Vornamen der Person. Mit diesem neuen Attribut ändern Sie das Modell.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. Wählen Sie **erstellen | Projektmappe** auf das Menü zum Erstellen der Anwendung.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
6. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Mit diesem Befehl wird für Änderungen in den Datenobjekten suchen, und es wird fügen Sie die erforderlichen Befehle aus, um die Datenbank entsprechend ändern.

    ![Hinzufügen einer zweiten Vornamens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ein zweiter Vorname hinzufügen")

    *Ein zweiter Vorname hinzufügen*
7. (Optional) Sie können den folgenden Befehl zum Generieren eines SQL-Skripts, mit dem differenzielle Update ausführen. Auf diese Weise können Sie die manuellen Aktualisieren der Datenbank (In diesem Fall ist es nicht erforderlich), oder übernehmen Sie die Änderungen in anderen Datenbanken:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generieren eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")

    *Generieren eines SQL-Skripts*

    ![SQL-Skript Update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Update SQL-Skript")

    *Update für SQL-Skript*
8. Geben Sie den folgenden Befehl zum Aktualisieren der Datenbank, in der Paket-Manager-Konsole:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")

    *Aktualisieren der Datenbank*

    Dadurch wird hinzugefügt der **MiddleName** Spalte in der **Personen** Tabelle entsprechend die aktuelle Definition des der **Person** Klasse.
9. Sobald die Datenbank aktualisiert wird, mit der rechten Maustaste in des Ordners Controller, und wählen Sie **hinzufügen | Controller** den Person Controller erneut (vollständig mit den gleichen Werten) hinzufügen. Hiermit wird die vorhandene Methoden und Ansichten, die das neue Attribut hinzufügen aktualisiert.

    ![Hinzufügen eines Controllers Updates](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "ein controllerupdate hinzufügen")

    *Update des Controllers*
10. Klicken Sie auf **Hinzufügen**. Wählen Sie dann die Werte **überschreiben PersonController.cs** und **überschreiben zugeordnete Ansichten** , und klicken Sie auf **OK**.

   ![Hinzufügen von einem Domänencontroller überschreiben](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Update des Controllers*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - Ausführen der Anwendung

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Open **/Person**. Beachten Sie, dass die Daten beibehalten wurde, während die Vornamen-Spalte hinzugefügt wurde, ein.

    ![Zweiter Vorname hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Vornamen hinzugefügt")

    *Zweiter Vorname hinzugefügt*
3. Wenn Sie auf **bearbeiten**, Sie werden möglicherweise ein zweiter Vorname an die aktuelle Person hinzufügen.

    ![Zweiter Vorname Edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Vornamen Edition")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgänge mit ASP.NET MVC 4-Gerüstbau über jede beliebige Modellklasse gelernt. Anschließend haben Sie gelernt ein End-to-End-Update in der Anwendung – aus der Datenbank, die den Ansichten – mit Entity Framework-Migrationen ausführen.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
