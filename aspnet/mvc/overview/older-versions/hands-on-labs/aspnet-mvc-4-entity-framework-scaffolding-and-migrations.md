---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder haben die &quot;Hilfsprogramme, Formulare und Überprüfung&quot; praktische Übungseinheiten durchführen, sollten Sie bedenken...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394204"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder haben die &quot;Hilfsprogramme, Formulare und Überprüfung&quot; praktische Übungseinheiten durchführen, sollten Sie bedenken, dass viele der Logik zu erstellen, aktualisieren, auflisten und entfernen alle Datenentität, er wird wiederholt, zwischen der Anwendung. Ganz zu schweigen davon, dass, wenn Ihr Modell mehrere Klassen zum Bearbeiten von verfügt, Sie wahrscheinlich eine beträchtliche Zeit schreiben die POST- und GET-Aktionsmethoden für jeden Vorgang für die Entität, als auch jede der Ansichten können.

In dieser Übung lernen Sie, wie Sie das ASP.NET MVC 4-Gerüst zu verwenden, zum automatischen Generieren von der Baseline der Ihrer Anwendung CRUD (Create, Read, Update und Delete). Starten über eine einfache Modellklasse und, ohne eine einzige Codezeile schreiben zu müssen, erstellen Sie einen Controller, der die CRUD-Vorgänge sowie die alle erforderlichen Ansichten enthält. Nach dem Erstellen und eine einfache Lösung ausführen, müssen Sie die Datenbank generiert werden, zusammen mit dem MVC-Logik und die Ansichten für die datenbearbeitung.

Darüber hinaus erfahren Sie, wie einfach es ist, die Entity Framework-Migrationen zu verwenden, um Modelle von Aktualisierungen in der gesamten Anwendung auszuführen. Entity Framework-Migrationen können Sie Ihre Datenbank zu ändern, nachdem das Modell mit einfachen Schritten geändert wurde. Mit all diese Denken Sie daran werden Sie zum Erstellen und Verwalten von Webanwendungen effizienter, nutzen die neuesten Funktionen von ASP.NET MVC 4.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Trainingskit, erhältlich über enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden Sie ASP.NET-Gerüstbau für CRUD-Vorgänge in Controllern.
- Ändern Sie das Datenbankmodell mit Entity Framework-Migrationen.

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

In der folgenden Übung besteht aus diesem Hands-On Lab:

1. [Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen](#Exercise1)

> [!NOTE]
> In dieser Übung wird zusammen mit einem **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe bei der Arbeit mit der Aufgabe benötigen.


Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen

ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit zum Generieren der CRUD-Vorgänge auf standardisierte Weise erstellen die erforderliche Logik, mit dem Ihre Anwendung mit der Datenbankschicht zu interagieren.

In dieser Übung lernen Sie, wie mit ASP.NET MVC 4-Gerüstbau mit Code zuerst die CRUD-Methoden erstellen. Anschließend erfahren Sie, wie Sie das Modell anwenden der Änderungen in der Datenbank mit Entity Framework-Migrationen aktualisieren.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Aufgabe 1: Erstellen einer neuen ASP.NET MVC 4-Projekt mithilfe von Gerüstbau

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.
2. Wählen Sie **Datei | Neues Projekt**. In das neue Projekt im Dialogfeld unter die **Visual c# | Web** wählen Sie im Abschnitt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt zu **MVC4andEFMigrations** , und legen Sie den Speicherort auf **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** Ordner dieser Anleitung. Legen Sie die **Projektmappenname** zu **beginnen** und stellen Sie sicher **Projektmappenverzeichnis erstellen** aktiviert ist. Klicken Sie auf **OK**.

    ![Dialogfeld Neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")

    *Neues ASP.NET MVC 4-Projekt (Dialogfeld)*
3. In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage, und stellen Sie sicher, dass **Razor** die ausgewählte **Ansichts-Engine**. Klicken Sie auf **OK**, um das Projekt zu erstellen.

    ![Neue ASP.NET MVC 4-Internetanwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "neue ASP.NET MVC 4-Internetanwendung")

    *Neue ASP.NET MVC 4-Internetanwendung*
4. Klicken Sie im Projektmappen-Explorer mit der Maustaste **Modelle** , und wählen Sie **hinzufügen | Klasse** um eine einfache Klasse Person (POCO) zu erstellen. Nennen Sie sie **Person** , und klicken Sie auf **OK**.
5. Öffnen Sie die Person-Klasse und fügen Sie die folgenden Eigenschaften.

    (Codeausschnitt - *ASP.NET MVC 4 und Entity Framework-Migrationen - Ex1 Personeneigenschaften*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Klicken Sie auf **erstellen | Erstellen Sie Projektmappe** zum Speichern der Änderungen, und erstellen Sie das Projekt.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
7. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Controllers", und wählen **hinzufügen | Controller**.
8. Nennen Sie den Controller *PersonController* und führen Sie die **gerüstoptionen** mit den folgenden Werten.

   1. In der **Vorlage** Dropdown-Liste die **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework** Option.
   2. In der **Modellklasse** Dropdown-Liste die **Person** Klasse.
   3. In der **Datenkontext Klasse** Liste  **&lt;neuer Datenkontext... &gt;**. Wählen Sie einen beliebigen Namen ein, und klicken Sie auf **OK**.
   4. In der **Ansichten** Dropdown-Liste, stellen Sie sicher, dass **Razor** ausgewählt ist.

      ![Die Person-Controller mit Gerüst hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "den Person-Controller mit Gerüst hinzufügen")

      *Die Person-Controller mit Gerüst hinzufügen*
9. Klicken Sie auf **hinzufügen** auf den neuen Controller für Personen mit Gerüst zu erstellen. Sie haben jetzt die Controlleraktionen sowie die Ansichten erstellt.

    ![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "nach dem Erstellen des Person-Controllers mit Gerüstbau")

    *Nach dem Erstellen des Person-Controllers mit Gerüstbau*
10. Open **PersonController** Klasse. Beachten Sie, dass die vollständige CRUD-Aktionsmethoden automatisch generiert wurden.

   ![In den Person-Controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "innerhalb der Person-Controller")

   *In den Person-controller*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

An diesem Punkt ist die Datenbank noch nicht erstellt. In dieser Aufgabe werden die Anwendung zum ersten Mal ausführen und Testen Sie die CRUD-Vorgänge. Die Datenbank wird im laufenden Betrieb mit Code First erstellt werden.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Fügen Sie im Browser **/Person** an die URL der Seite "Person" zu öffnen.

    ![Anwendung – erste Ausführung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung – erste Ausführung")

    *Der Anwendung: ersten Ausführen*
3. Jetzt Untersuchen der Person-Seiten und testen die CRUD-Vorgänge.

    1. Klicken Sie auf **neu erstellen** eine neue Person hinzufügen. Geben Sie einen Vornamen und einen Nachnamen ein, und klicken Sie auf **erstellen**.

        ![Eine neue Person hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "eine neue Person hinzufügen")

        *Eine neue Person hinzufügen*
    2. In der Liste der Person können Sie zu löschen, bearbeiten oder Hinzufügen von Elementen.

        ![Personenliste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Person-Liste")

        *Person-Liste*
    3. Klicken Sie auf **Details** um der Person zu öffnen.

        ![Person Details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person Details")

        *Person-details*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück. Beachten Sie, dass Sie in der gesamten Anwendung – von des Modells in den Ansichten: die gesamte CRUD-Vorgänge für die Personenentität erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Aufgabe 3: Aktualisieren der Datenbank mithilfe von Entity Framework-Migrationen

In dieser Aufgabe aktualisieren Sie die Datenbank mithilfe von Entity Framework-Migrationen. Sie werden feststellen, wie einfach es ist, ändern Sie das Modell und die Änderungen widerzuspiegeln, in Ihren Datenbanken mithilfe der Funktion von Entity Framework-Migrationen.

1. Öffnen Sie die Paket-Manager-Konsole. Wählen Sie **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole**.
2. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Migrationen aktivieren")

    *Aktivieren von Migrationen*

    Der Aktivierung der Migration Befehl erstellt die **Migrationen** Ordner, in dem ein Skript zum Initialisieren der Datenbank enthalten.

    ![Ordner "Migrations"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Ordner \"Migrations\"")

    *Ordner "Migrations"*
3. Öffnen der **Configuration.cs** -Datei im Ordner "Migrations". Der Konstruktor der Klasse zu suchen und ändern Sie die **AutomaticMigrationsEnabled** Wert *"true"*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Öffnen Sie die Person-Klasse, und fügen Sie ein Attribut für den Vornamen einer Person hinzu. Mit diesem neuen Attribut ändern Sie das Modell.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Wählen Sie **erstellen | Erstellen Sie Projektmappe** im Menü, um die Anwendung zu erstellen.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
6. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Dieser Befehl sucht nach Änderungen in den Datenobjekten, und klicken Sie dann, fügen sie die erforderlichen Befehle, die Datenbank entsprechend zu ändern.

    ![Hinzufügen einer zweiten Vornamens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "einen Zweitnamen hinzufügen")

    *Ein zweiter Vorname hinzufügen*
7. (Optional) Sie können den folgenden Befehl zum Generieren eines SQL-Skripts mit dem Update, differenzielle ausführen. Auf diese Weise können Sie die Datenbank manuell zu aktualisieren (In diesem Fall es ist nicht erforderlich), oder wenden Sie die Änderungen in anderen Datenbanken:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generieren eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")

    *Generieren eines SQL-Skripts*

    ![Update für SQL-Skript](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Update für SQL-Skript")

    *Update für SQL-Skript*
8. Geben Sie den folgenden Befehl zum Aktualisieren der Datenbank, in der Paket-Manager-Konsole:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")

    *Aktualisieren der Datenbank*

    Hiermit wird die **MiddleName** -Spalte in der **Personen** Tabelle entsprechend die aktuelle Definition der der **Person** Klasse.
9. Nachdem die Datenbank aktualisiert wurde, mit der rechten Maustaste in den Ordner "Controller", und wählen Sie **hinzufügen | Controller** zum Hinzufügen der Person Controllers erneut (vollständig mit den gleichen Werten). Dadurch werden die vorhandenen Methoden und Ansichten, die das neue Attribut hinzufügen aktualisiert.

    ![Hinzufügen von einem controllerupdate](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Hinzufügen von einem controllerupdate")

    *Update des Controllers*
10. Klicken Sie auf **Hinzufügen**. Wählen Sie dann die Werte **überschreiben PersonController.cs** und **überschreiben zugeordnete Ansichten** , und klicken Sie auf **OK**.

   ![Das Überschreiben einer Controller hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Update des Controllers*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - Ausführen der Anwendung

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Open **/Person**. Beachten Sie, dass die Daten beibehalten wurde, während die zweite Vorname-Spalte hinzugefügt wurde, ein.

    ![Zweiter Vorname hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Vornamen hinzugefügt")

    *Zweiter Vorname hinzugefügt*
3. Wenn Sie auf **bearbeiten**, ein zweiter Vorname an die aktuelle Person hinzufügen können.

    ![Zweiter Vorname Edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Vornamen-Edition")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgänge mit ASP.NET MVC 4-Gerüstbau jede beliebige Modellklasse verwenden. Klicken Sie dann haben Sie gelernt, ein Update für die End-to-End in Ihrer Anwendung – aus der Datenbank, die den Ansichten – führen Sie mithilfe von Entity Framework-Migrationen.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
