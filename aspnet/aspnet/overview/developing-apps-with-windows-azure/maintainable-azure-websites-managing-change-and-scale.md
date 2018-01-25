---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Praktische Übungseinheit: Azure-Websites verwaltbar: verwalten, ändern und Skalierung | Microsoft Docs"
author: rick-anderson
description: Microsoft Azure erleichtert das Erstellen und Bereitstellen von Websites bis hin zur Produktion. Aber Sie sind nicht fertig, sobald die Anwendung aktiv ist, Sie sind nur erste Schritte! Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3d24c633368abc14efcd9fcf200a4d05c5b182c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Praktische Übungseinheit: Azure-Websites verwaltbar: verwalten, ändern und Skalierung
====================
durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure erleichtert das Erstellen und Bereitstellen von Websites bis hin zur Produktion. Aber Sie sind nicht fertig, sobald die Anwendung aktiv ist, Sie sind nur erste Schritte! Sie müssen die veränderten Anforderungen, Datenbankupdates, Skalierung und mehr zu behandeln. Glücklicherweise verfügt Azure App Service Sie behandelt, mit viele Features können Sie die reibungslose Ausführung Websites zu halten.
> 
> Azure bietet sichere und flexible Entwicklung, Bereitstellung und Skalierungsoptionen für jede Größe Web-Anwendung. Nutzen Sie vorhandene Tools zum Erstellen und Bereitstellen von Anwendungen ohne den Aufwand der Verwaltung der Infrastruktur.
> 
> Bereitstellen einer Produktions-Web-Anwendung selbst in Minuten einfache Bereitstellung von Inhalten, die mit Ihrem bevorzugten Entwicklungstool erstellt. Sie können eine vorhandene Website direkt aus der quellcodeverwaltung mit Unterstützung für bereitstellen **Git**, **GitHub**, **Bitbucket**, **TFS**, und sogar  **DropBox**. Bereitstellen direkt über Ihre bevorzugte IDE oder über Skripts mit **PowerShell** in Windows oder **CLI** Tools auf allen Betriebssystemen ausgeführt wird. Nach der Bereitstellung zu halten Sie Ihre Standorte mit Unterstützung für die dauerhafte Bereitstellung ständig auf dem neuesten Stand.
> 
> Azure bietet skalierbare und robuste cloudspeicher, Sicherung und Wiederherstellung Lösungen für alle Daten, Groß oder klein. Können Sie Anwendungen Speicherdienste, z. B. Tabellen, Blobs und SQL-Datenbanken zu einer produktiven Umgebung bereitstellen möchten, die Skalierung der Anwendung in der Cloud.
> 
> Mit SQL-Datenbanken ist es wichtig, Ihre produktive Datenbank aktuell zu halten, wenn Sie neue Versionen der Anwendung bereitstellen. Dank an **Entity Framework Code First-Migrationen**, um Ihre Umgebungen in Minuten aktualisieren wurde die Entwicklung und Bereitstellung Ihres Datenmodells vereinfacht. Dieser praktischen Übungseinheit werden Sie in den anderen Themen an, die auftreten können, wenn Sie Ihre Web-app in produktionsumgebungen in Microsoft Azure bereitstellen.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).
> 
> Weitere ausführliche Beschreibung in diesem Thema finden Sie unter der [Building Real-World Cloud-Apps mit Azure-e-Book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Aktivieren Sie die Entity Framework-Migrationen mit einem vorhandenen Modell
- Aktualisieren des Objektmodells und die Datenbank entsprechend mithilfe von Entity Framework-Migrationen
- Bereitstellen Sie auf Azure App Service mithilfe von Git
- Rollback zu einer vorherigen Bereitstellung über das Azure-Verwaltungsportal
- Mit der Skalierung einer Web-app in Azure-Speicher
- Konfigurieren Sie die automatische Skalierung für eine Web-app mit dem Azure-Verwaltungsportal
- Erstellen Sie und konfigurieren Sie ein Testprojekt Auslastungstest in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Azure SDK für .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT-Versionskontrollsystem](http://git-scm.com/download)
- Microsoft Azure-Abonnement 

    - Melden Sie sich für eine [kostenlose Testversion](http://aka.ms/watk-freetrial)
    - Wenn Sie eine Visual Studio Professional Test Professional, Premium oder Ultimate mit MSDN oder MSDN-Plattformen Abonnenten sind, aktivieren Ihrer [MSDN Benefit](http://aka.ms/watk-msdn) jetzt zu starten, entwickeln und Testen von in Azure
    - [BizSpark](http://aka.ms/watk-bizspark) Mitglieder erhalten automatisch die Azure nutzen, über die Visual Studio Ultimate mit MSDN-Abonnements
    - Mitglieder der [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials-Programms erhalten monatlichen Azure gebührenfreie Gutschriften

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie Windows-Explorer und Navigieren in des Labors **Quelle** Ordner.
2. Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.
3. Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

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

1. [Verwenden von Entity Framework-Migrationen](#Exercise1)
2. [Bereitstellen einer Web-App in die Stagingumgebung](#Exercise2)
3. [Ausführen des Rollbacks für Bereitstellung in der Produktion](#Exercise3)
4. [Skalierung mit dem Azure-Speicher](#Exercise4)
5. [Verwenden zur automatischen Skalierung für Web-Apps](#Exercise5) (Optional für Visual Studio 2013 Ultimate Edition)

Geschätzte Zeit zum Abschließen dieser testumgebung: **75 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt. In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Übung 1: Verwenden von Entity Framework-Migrationen

Wenn Sie eine Anwendung entwickeln, kann Ihr Datenmodell mit der Zeit ändern. Diese Änderungen beeinträchtigen, die das vorhandene Modell in der Datenbank (Wenn Sie eine neue Version erstellen), und es ist wichtig, Ihre Datenbank auf dem neuesten Stand, um Fehler zu vermeiden.

Zur Vereinfachung der Überwachung dieser Änderungen in Ihrem Modell **Entity Framework Code First-Migrationen** automatisch erkennen von Änderungen, die Ihr Modell mit dem Schema verglichen wird, und bestimmte Code zum Aktualisieren der Datenbank generiert Erstellen neuer *Versionen* Ihrer Datenbank.

Diese Übung zeigt Ihnen, zum Aktivieren der **Migrationen** für die Anwendung, und wie Sie leicht erkennen und Generieren von Änderungen, um die Datenbanken zu aktualisieren.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Aufgabe 1: Aktivieren von Migrationen

In dieser Aufgabe führen Sie durch die Schritte zum Aktivieren der **Entity Framework Code First-Migrationen** auf die **Meister Quiz** Datenbank, ändern das Modell und verstehen, wie diese Änderungen berücksichtigt werden die die Datenbank.

1. Öffnen Sie Visual Studio, und öffnen Sie die **GeekQuiz.sln** Projektmappendatei aus **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Erstellen Sie die Projektmappe, um das Herunterladen und Installieren der **NuGet** Abhängigkeiten-Paket. Zu diesem Zweck mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **Projektmappe** oder drücken Sie **STRG + UMSCHALT + B**.
3. Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**.
4. In der **Package Manager Console**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**. Eine anfängliche Migration basierend auf dem vorhandenen Modell werden erstellt.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Aktivieren von Migrationen](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Migrationen aktivieren")

    *Aktivieren von Migrationen*

    > [!NOTE]
    > Mit diesem Befehl wird eine **Migrationen** Ordner Meister Quiz Projekt mit einer Datei namens **"Configuration.cs"**. Die **Konfiguration** -Klasse können Sie Migrationen für den Kontext Verhalten konfigurieren.
5. Bei Migrationen aktiviert, müssen Sie aktualisieren die **Konfiguration** Klasse, um die Datenbank mit den anfänglichen Daten aufgefüllt, **Meister Quiz** erfordert. Unter **Migrationen**, ersetzen Sie die **"Configuration.cs"** Datei mit dem Element befindet sich der **Source\Assets** Ordner dieser Anleitung.

    > [!NOTE]
    > Da **Migrationen** aufrufen, wird die **Ausgangswert** -Methode mit jedem Datenbankupdate müssen Sie darauf achten, dass die Datensätze in der Datenbank nicht dupliziert werden. Die **AddOrUpdate** Methode hilft doppelte Daten verhindert.
6. Um eine anfängliche Migration hinzuzufügen, geben Sie den folgenden Befehl ein, und drücken Sie dann **EINGABETASTE**.

    > [!NOTE]
    > Stellen Sie sicher, dass es keine Datenbank mit dem Namen ist &quot;GeekQuizProd&quot; in der LocalDB-Instanz.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Hinzufügen von basisschemaänderungen Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "hinzufügen basisschemaänderungen Migrations")

    *Hinzufügen von basisschemaänderungen Migrations*

    > [!NOTE]
    > **Hinzufügen-Migration** wird das Gerüst für die nächste Migration basierend auf Änderungen, die Sie mit dem Modell vorgenommen haben, seit der Erstellung der letzten Migrations erstellen. In diesem Fall entspricht der ersten Migration des Projekts, dabei werden hinzugefügt. die Skripts zum Erstellen der Tabellen, die definiert, der **TriviaContext** Klasse.
7. Führen Sie die Migration aus, um die Datenbank zu aktualisieren, indem Sie den folgenden Befehl ausführen. Für diesen Befehl geben Sie die **ausführlich** -Flag in die Zieldatenbank angewendeten SQL-Anweisungen anzuzeigen.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Erstellen von Ausgangsdatenbank](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Ausgangsdatenbank erstellen")

    *Erstellen die ursprüngliche Datenbank*

    > [!NOTE]
    > **Update-Database '** alle ausstehenden Migrationen auf die Datenbank anwenden. In diesem Fall wird die Datenbank mithilfe der Verbindungszeichenfolge in der Datei "Web.config" definierten erstellt.
8. Wechseln Sie zu **Ansicht** , und öffnen Sie im Menü **Objekt-Explorer von SQL Server**.

    ![Öffnen Sie im Objekt-Explorer von SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "in SQL Server-Objekt-Explorer öffnen")

    *Öffnen Sie im Objekt-Explorer von SQL Server*
9. In der **Objekt-Explorer von SQL Server** Fenster Verbinden mit der LocalDB-Instanz, indem Sie mit der rechten Maustaste die **SQL Server** Knoten klicken und auswählen **SQL-Server hinzufügen...**  Option.

    ![Hinzufügen einer SQL Server-Instanz](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "eine SQL Server-Instanz hinzufügen")

    *Hinzufügen einer SQL Server-Instanz auf SQL Server-Objekt-Explorer*
10. Legen Sie die **Servernamen** auf *(Localdb) \v11.0* und lassen Sie **Windows-Authentifizierung** als Authentifizierungsmodus. Klicken Sie auf **verbinden** um den Vorgang fortzusetzen.

    ![Herstellen einer Verbindung mit LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Herstellen einer Verbindung mit LocalDB")

    *Herstellen einer Verbindung mit LocalDB*
11. Öffnen der **GeekQuizProd** Datenbank und erweitern Sie die **Tabellen** Knoten. Wie Sie sehen können, die **Update-Database** Befehl generiert alle Tabellen, die definiert, der **TriviaContext** Klasse. Suchen Sie die **Dbo. TriviaQuestions** Tabelle, und öffnen Sie den Knoten Columns. In der nächsten Aufgabe wird in dieser Tabelle eine neue Spalte hinzufügen und aktualisieren Sie die Datenbank mit **Migrationen**.

    ![Faszinierende Fragen Spalten](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "faszinierende Fragen Spalten")

    *Faszinierende Fragen Spalten*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Aufgabe 2 – Update Migrationen mit Datenbankschema

In dieser Aufgabe verwenden Sie **Entity Framework Code First-Migrationen** so erkennen eine Änderung in Ihrem Modell und generieren den erforderlichen Code zum Aktualisieren der Datenbank. Aktualisieren Sie die **TriviaQuestions** Entität, indem eine neue Eigenschaft hinzugefügt. Dann führen Sie-Befehle zum Erstellen einer neuen Migrations aus, um die neue Spalte in der Tabelle enthalten.

1. In **Projektmappen-Explorer**, doppelklicken Sie auf die **TriviaQuestion.cs** -Datei innerhalb der **Modelle** Ordner.
2. Hinzufügen einer neuen Eigenschaft mit dem Namen **Hinweis**, wie im folgenden Codeausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. In der **Package Manager Console**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**. Eine neue Migration wird die Änderung in unserem Modell reflektieren erstellt werden.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Hinzufügen-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "hinzufügen-Migration")

    *Hinzufügen-Migration*

    > [!NOTE]
    > Eine Migrationsdatei besteht aus zwei Methoden **einrichten** und **unten**.
    > 
    > - Die **einrichten** Methode wird verwendet, um anzugeben, welche Änderungen von der aktuellen Version von unserer Anwendung müssen für die Datenbank angewendet.
    > - Die **unten** wird verwendet, um die Änderungen umzukehren, wir, um hinzugefügt haben, die **einrichten** Methode.
    > 
    > Wenn die Datenbankmigration zur Aktualisierung der Datenbank werden sie bei allen Migrationen ausgeführt, in der Reihenfolge, Timestamp, und nur solche, die seit dem letzten Update nicht verwendet wurden (die \_MigrationHistory-Tabelle der nachverfolgt welche Migrationen angewendet wurden). Die **einrichten** bei allen Migrationen wird aufgerufen, und nehmen die Änderungen haben wir in der Datenbank angegeben. Wenn wir eine vorherige Migration zurückkehren möchten die **unten** Methode wird aufgerufen, um die Änderungen in umgekehrter Reihenfolge erneut.
4. In der **Package Manager Console**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Die Ausgabe der **Update-Database** Befehl generiert ein **Alter Table** SQL-Anweisung, um eine neue Spalte hinzufügen der **TriviaQuestions** Tabelle, wie in der folgenden Abbildung gezeigt.

    ![Hinzufügen von Spalte SQL-Anweisung generiert](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "generierte Spalte SQL-Anweisung hinzufügen")

    *Hinzufügen von Spalte generierte SQL-Anweisung*
6. In **Objekt-Explorer von SQL Server**, aktualisieren Sie die **Dbo. TriviaQuestions** Tabelle, und überprüfen Sie, ob die neue **Hinweis** Spalte wird angezeigt.

    ![Mit der neuen Spalte für den Hinweis](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "neue Hinweis-Spalte anzeigen")

    *Der neue Hinweis Spalte anzeigen*
7. Wieder in die **TriviaQuestion.cs** fügen-Editor eine **StringLength** Einschränkung der *Hinweis* Eigenschaft, wie im folgenden Codeausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. In der **Package Manager Console**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. In der **Package Manager Console**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Die Ausgabe der **Update-Database** Befehl generiert ein **Alter Table** SQL-Anweisung zum Aktualisieren der *Hinweis* Spalte vom Typ der **TriviaQuestions** Tabelle, wie in der folgenden Abbildung gezeigt.

    ![Alter Column-SQL-Anweisung generiert](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter Column-SQL-Anweisung generiert")

    *Alter Column-SQL-Anweisung generiert*
11. In **Objekt-Explorer von SQL Server**, aktualisieren Sie die **Dbo. TriviaQuestions** Tabelle, und überprüfen Sie, ob die **Hinweis** Spaltentyp **nvarchar(150)**.

    ![Mit der neuen Einschränkung](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "mit der neuen Einschränkung")

    *Die neue Einschränkung anzeigen*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Übung 2: Bereitstellen einer Web-App in die Stagingumgebung

**Web-Apps in Azure App Service** können Sie die Veröffentlichung in einer Stagingumgebung durchführen. Veröffentlichen in einer Stagingumgebung ein stagingwebsiteslot für jede standardproduktionswebsite erstellt und ermöglicht es Ihnen, diese Slots ohne Ausfallzeit austauschen. Dies ist besonders hilfreich ist, überprüfen die Änderungen vor dem Freigeben von öffentlich, inkrementell Websiteinhalt integrieren und Rollback, wenn Änderungen nicht ordnungsgemäß funktionieren.

Sie werden in dieser Übung Bereitstellen der **Meister Quiz** Anwendung in der Stagingumgebung Ihrer Web-App mithilfe der Git-quellcodeverwaltung. Zu diesem Zweck Sie Web-app erstellen und Bereitstellen der erforderlichen Komponenten auf das Verwaltungsportal, konfigurieren Sie eine **Git** Repository und übertragen Sie die Anwendung Quellcode, aus dem lokalen Computer um staging-Slot. Aktualisieren Sie auch die Produktionsdatenbank mit der **Code First-Migrationen** Sie in der vorherigen Übung erstellt haben. Führen Sie dann die Anwendung in dieser testumgebung, um den Vorgang zu überprüfen. Wenn Sie zufrieden sind gemäß Ihren Erwartungen funktioniert, werden Sie die Anwendung bis hin zur Produktion heraufgestuft.

> [!NOTE]
> Zum Veröffentlichen in einer Stagingumgebung zu aktivieren, muss die Web-app in **Modus "Standard"**. Beachten Sie, dass zusätzliche Kosten anfallen, wenn Sie Ihre Web-app in den Standardmodus ändern. Weitere Informationen zur Preisgestaltung finden Sie unter [App Service-Preisen](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Aufgabe 1 – erstellen eine Web-App in Azure App Service

In dieser Aufgabe erstellen Sie eine Web-app in **Azure App Service** über das Verwaltungsportal. Konfigurieren Sie auch eine **SQL-Datenbank** persistent Speichern von Anwendungsdaten und konfigurieren ein lokales Git-Repository für die Datenquellen-Steuerelements.

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.

    ![Melden Sie sich beim Azure-Verwaltungsportal](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Melden Sie sich beim Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlsleiste am unteren Rand der Seite.

    ![Erstellen eine neue Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "erstellen eine neue Web-app")

    *Erstellen eine neue Web-app*
3. Klicken Sie auf **berechnen**, **Website** und dann **Benutzerdefiniert erstellen**.

    ![Erstellen eine neue Web-app verwenden, erstellen Sie benutzerdefinierte](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "erstellen eine neue Web-app mithilfe von Benutzerdefiniert erstellen")

    *Erstellen eine neue Web-app mithilfe von Benutzerdefiniert erstellen*
4. In der **neue Website - Benutzerdefiniert erstellen** Dialogfeld geben einen verfügbaren **URL** (z. B. *Meister Quiz*), wählen Sie einen Speicherort in der **Region** Dropdown-Liste aus, und wählen Sie **erstellen Sie eine neue SQL­Datenbank** in der **Datenbank** Dropdown-Liste. Wählen Sie abschließend die **veröffentlichen aus der quellcodeverwaltung** Kontrollkästchen und klicken Sie auf **Weiter**.

    ![Anpassen der neuen Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Anpassen der neuen Web-app*
5. Geben Sie die folgenden Informationen für die datenbankeinstellungen:

    - In der **Namen** Text Geben Sie einen Datenbanknamen an (z. B. *Geekquiz\_Db*)
    - Auf dem Server **Dropdownelement** Liste **neue SQL-Datenbankserver**. Alternativ können Sie einen vorhandenen Server auswählen.
    - In der **Datenbankbenutzername** und **Datenbankkennwort** Felder, geben Sie den Administratorbenutzernamen und das Kennwort für den SQL-Datenbankserver. Wenn Sie einen Server auswählen ist bereits vorhanden, Sie werden aufgefordert, das Kennwort.

    ![Datenbankeinstellungen angeben](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *Datenbankeinstellungen angeben*
6. Klicken Sie auf **Weiter**, um fortzufahren.
7. Wählen Sie **lokales Git-Repository** für die quellcodeverwaltung verwenden, und klicken Sie auf **Weiter**.

    > [!NOTE]
    > Sie können für die bereitstellungsanmeldeinformationen (Benutzername und Kennwort) aufgefordert.

    ![Erstellen das Git-Repository](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Erstellen das Git-repository*
8. Warten Sie, bis die neuen Web-app erstellt wird.

    > [!NOTE]
    > Azure bietet standardmäßig Domänen *azurewebsites.net* aber auch bietet Ihnen die Möglichkeit, benutzerdefinierte Domänen, die mit dem Azure-Verwaltungsportal festgelegt. Allerdings können Sie nur benutzerdefinierte Domänen verwalten, wenn Sie bestimmte Azure App Service-Modi verwenden.
    > 
    > Azure App Service ist kostenlos, freigegeben, Basic, Standard und Premium-Editionen verfügbar. Im Modus kostenlos und freigegeben alle Web-apps in einer Umgebung mit mehreren Mandanten ausgeführt und haben Kontingente für die Verwendung von CPU, Arbeitsspeicher und Netzwerk. Die maximale Anzahl von kostenlose apps kann mit Ihrem Plan variieren. Im Modus "Standard" Wählen Sie an, welche apps, die Ausführung auf dedizierten virtuellen Computern, die entsprechen in der standard-Azure-Serverressourcen. Finden Sie die Konfiguration der Web-app-Modus in den **Skalierung** Menü Ihrer Web-App.
    > 
    > ![Azure App Service-Modi](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "-Azure App Service-Modi")
    > 
    > Bei Verwendung von **Shared** oder **Standard** -Modus werden benutzerdefinierte Domänen für Ihre Web-app zu verwalten, navigieren Sie zu Ihrer app **konfigurieren** Menü und klicken auf **Domänen verwalten** unter *Domänennamen*.
    > 
    > ![Verwalten von Domänen](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Verwalten von Domänen")
    > 
    > ![Verwalten von benutzerdefinierten Domänen](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "benutzerdefinierte Domänen verwalten")
9. Sobald die Web-app erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte zum Überprüfen, dass die neuen Web-app ausgeführt wird.

    ![Navigieren in der neuen Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Navigieren in der neuen Web-app*

    ![Web-app ausgeführt wird](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Web-app ausgeführt wird*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Aufgabe 2: Erstellen der SQL-Produktionsdatenbank

In dieser Aufgabe verwenden Sie die **Entity Framework Code First-Migrationen** zum Erstellen der Datenbank als Ziel der **Azure SQL-Datenbank** Instanz, die Sie in der vorherigen Aufgabe erstellt haben.

1. Klicken Sie im Verwaltungsportal, navigieren Sie zu der Web-app, die Sie in der vorherigen Aufgabe erstellt haben, und wechseln Sie zu dessen **Dashboard**.
2. In der **Dashboard** auf **Verbindungszeichenfolgen anzeigen** Verknüpfung unter den **kurzer Blick** Abschnitt.

    ![Anzeige von Verbindungszeichenfolgen](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Verbindungszeichenfolgen anzeigen")

    *Verbindungszeichenfolgen anzeigen*
3. Kopieren der **Verbindungszeichenfolge** Wert und das Dialogfeld zu schließen.

    ![Verbindungszeichenfolge in Azure-Verwaltungsportal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Verbindungszeichenfolge im Azure-Verwaltungsportal")

    *Verbindungszeichenfolge in Azure-Verwaltungsportal*
4. Klicken Sie auf **SQL-Datenbanken** auf die Liste der SQL-Datenbanken in Azure finden Sie unter

    ![SQL-Datenbank im Menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "im Menü die SQL-Datenbank")

    *Im Menü die SQL-Datenbank*
5. Suchen Sie die Datenbank aus, die Sie in der vorherigen Aufgabe erstellt haben, und klicken Sie auf dem Server.

    ![SQL-Datenbankserver](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL-Datenbankserver")

    *SQL-Datenbankserver*
6. In der **Schnellstart** Seite des Servers, klicken Sie auf **konfigurieren**.

    ![Konfigurieren Sie im Menü](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menü konfigurieren")

    *Konfigurieren Sie im Menü*
7. In der **zulässige IP-Adressen** Abschnitt, klicken Sie auf **zu den zulässigen IP-Adressen hinzufügen** Link, um die IP-Adresse für die Verbindung mit dem SQL-Datenbankserver zu aktivieren.

    ![Zulässige IP-Adressen](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "zulässige IP-Adressen")

    *Zulässige IP-Adressen*
8. Klicken Sie auf **speichern** am unteren Rand der Seite zum Abschließen des Schritts.
9. Wechseln Sie zurück zu Visual Studio.
10. In der **Package Manager Console**, führen Sie den folgenden Befehl Ersetzen *[YOUR-VERBINDUNGSZEICHENFOLGE]* -Platzhalter durch die Verbindungszeichenfolge, die Sie kopiert, von Azure haben

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualisierungsdatenbank, die für Windows Azure SQL-Datenbank](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Datenbank aktualisieren, die für Windows Azure SQL-Datenbank")

    *Aktualisieren der Datenbank, die für Azure SQL-Datenbank*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Aufgabe 3: Bereitstellen von Meister Quiz in die Stagingumgebung mithilfe von Git

In dieser Aufgabe aktivieren Sie in der Web-app Veröffentlichen in einer Stagingumgebung. Anschließend werden Sie Git verwenden, die Anwendung Meister Quiz direkt aus Ihrem lokalen Computer in der Stagingumgebung Ihrer Web-App veröffentlichen.

1. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Web-app unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Verwaltungsseiten der Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Öffnen die Verwaltungsseiten der Web-app*
2. Navigieren Sie zu der **Skalierung** Seite. Klicken Sie unter der **allgemeine** Abschnitt **Standard** für die Konfiguration und klicken Sie auf **speichern** in der Befehlsleiste.

    > [!NOTE]
    > Auszuführende alle Web-apps in der aktuellen Region und demselben Abonnement in **Standard** Modus, lassen Sie die **Alles auswählen** Kontrollkästchen der **Sites wählen** Konfiguration. Andernfalls deaktivieren Sie die **Alles markieren** Kontrollkästchen.

    ![Aktualisieren der Web-app zum Modus "Standard"](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Aktualisieren der Web-app zum Modus "Standard"")

    *Aktualisieren der Web-App zum Modus "Standard"*
3. Klicken Sie auf **Ja** um die Änderungen zu bestätigen.

    ![Bestätigen die Änderung in den Standardmodus](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "fortsetzen, die mit Änderungen an der Web-app-Modus")

    *Bestätigen die Änderung an der Modus "Standard"*
4. Wechseln Sie zu der **Dashboard** Seite, und klicken Sie auf **Aktivieren einer Veröffentlichung Stagingumgebung** unter der **kurzer Blick** Abschnitt.

    ![Aktivieren der Veröffentlichung in einer Stagingumgebung](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "durch das Aktivieren einer Veröffentlichung Stagingumgebung")

    *Aktivieren der Veröffentlichung in Stagingumgebungen*
5. Klicken Sie auf **Ja** Veröffentlichen in einer Stagingumgebung zu aktivieren.

    ![Bestätigt die Publishing bereitgestellt](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "klicken Sie auf Ja, zum Veröffentlichen in einer Stagingumgebung zu aktivieren.")

    *Bestätigen die Veröffentlichung in Stagingumgebungen*
6. Erweitern Sie in der Liste der Web-apps die Markierung links vom Ihrer Web-app-Name der stagingwebsiteslot angezeigt. Es wurde der Name Ihrer Web-App, gefolgt von ***(staging)***. Klicken Sie auf die Stagingsite, um zur Seite "Verwaltung" zu wechseln.

    ![Navigieren in den staging-Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigieren zu den staging-Web-app")

    *Navigieren Sie in der staging-App*
7. Beachten Sie, dass diese He-Management-Seite wie jede andere WebApp-Verwaltungsseite aussieht. Navigieren Sie zu der **Bereitstellungen** Seite, und kopieren Sie die **Git-URL** Wert. Sie werden weiter unten in dieser Übung verwendet.

    ![Kopieren den Git-URL-Wert](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopieren den Git-URL-Wert*
8. Öffnen Sie ein neues **Git Bash** Konsole, und führen Sie die folgenden Befehle. Update der *[YOUR-ANWENDUNGSPFAD]* Platzhalter durch den Pfad zu der **GeekQuiz** Projektmappe befindet sich in der **Source\Ex1 DeployingWebSiteToStaging\Begin** Ordner "" Diese Übung.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git-Initialisierung und ersten Commits](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git-Initialisierung und ersten Commits*
9. Führen Sie den folgenden Befehl aus, um Ihre Web-app mit der Remoteinstanz per Push übertragen **Git** Repository. Ersetzen Sie den Platzhalter durch die URL, die Sie aus dem Verwaltungsportal erhalten haben. Sie werden aufgefordert, Ihr Kennwort für die Bereitstellung.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Auf Windows Azure übertragen](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Auf Azure übertragen*

    > [!NOTE]
    > Wenn Sie Inhalte auf dem FTP-Host oder die GIT-Repository einer Web-app bereitstellen, Sie müssen zur Authentifizierung verwenden die **bereitstellungsanmeldeinformationen** , die Sie erstellt haben, aus der Web-app **Schnellstart** oder **Dashboard**  Verwaltungsseiten. Wenn Sie nicht, dass Ihre Anmeldeinformationen für die Bereitstellung wissen können Sie problemlos mit dem Verwaltungsportal zurücksetzen. Öffnen Sie die Web-app **Dashboard** Seite, und klicken Sie auf die **die bereitstellungsanmeldeinformationen für die zurücksetzen** Link. Geben Sie ein neues Kennwort ein, und klicken Sie auf **OK**. Anmeldeinformationen für die Bereitstellung gelten für die Verwendung mit allen Web-apps, die Ihrem Abonnement zugeordnet wurde.
10. Um sicherzustellen, die Web-app in Azure erfolgreich verschoben wurde, kehren Sie zum Verwaltungsportal, und klicken Sie auf **Websites**.
11. Suchen Sie Ihre Web-app, und erweitern Sie den Eintrag, um die stagingwebsiteslot anzuzeigen. Klicken Sie auf die **Namen** um zur Seite "Verwaltung" zu wechseln.
12. Klicken Sie auf **Bereitstellungen** , finden Sie unter der **Bereitstellungsverlauf**. Stellen Sie sicher, dass es ist ein **aktive Bereitstellung** mit Ihrem  *&quot;anfängliche Commit&quot;*.

    ![Aktive Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktive Bereitstellung*
13. Klicken Sie abschließend auf **Durchsuchen** in der Befehlsleiste, fahren Sie mit der Web-app.

    ![Durchsuchen von Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Durchsuchen von Web-app*
14. Wenn die Anwendung erfolgreich bereitgestellt wird, sehen Sie die Anmeldeseite Meister Quiz.

    > [!NOTE]
    > Die URL der bereitgestellten Anwendung enthält den Namen der Ihre Web-app, gefolgt von *-staging*.

    ![In der Stagingumgebung ausgeführte Anwendung](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *In der Stagingumgebung ausgeführte Anwendung*
15. Wenn Sie die Anwendung vertraut machen möchten, klicken Sie auf **registrieren** um einen neuen Benutzer zu registrieren. Schließen Sie die Kontodetails, indem Sie einen Benutzernamen, e-Mail-Adresse und ein Kennwort eingeben. Im nächsten Schritt zeigt die Anwendung die erste Frage des Quiz. Beantworten Sie einige Fragen, um sicherzustellen, dass er wie erwartet funktioniert.

    ![Anwendung verwendet werden](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Anwendung verwendet werden*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Aufgabe 4: Heraufstufen der Web-App für die Produktion

Nun, dass Sie überprüft haben, dass die Web-app in der Stagingumgebung ordnungsgemäß funktioniert, können Sie sie bis hin zur Produktion heraufzustufen. In dieser Aufgabe wird der stagingwebsiteslot mit dem Standort bereitstellungsslot ausgetauscht werden.

1. Wechseln Sie zurück zum Verwaltungsportal, und wählen Sie den stagingslot-Website. Klicken Sie auf **austauschen** in der Befehlsleiste.

    ![Wechseln Sie zur Produktion](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Wechseln Sie zur Produktion*
2. Klicken Sie auf **Ja** im Dialogfeld zur Bestätigung der Swap-Vorgang fortsetzen. Den Inhalt des Produktionsstandorts mit dem Inhalt der staging-Website wird von Azure sofort ausgetauscht werden.

    > [!NOTE]
    > Einige Einstellungen aus der bereitgestellten Version automatisch die Produktionsversion (z. B. Verbindungszeichenfolge überschreibt, Handlerzuordnungen usw.) kopiert werden, aber andere Einstellungen werden nicht geändert (z. B. DNS-Endpunkte, SSL-Bindungen, usw.).

    ![Swap-Vorgang bestätigen](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Swap-Vorgang bestätigen*
3. Nachdem der Austausch abgeschlossen ist, wählen Sie den produktionsslot zu übergeben, und klicken Sie auf **Durchsuchen** in der Befehlsleiste des Produktionsstandorts zu öffnen. Beachten Sie die URL in die Adressleiste ein.

    > [!NOTE]
    > Sie müssen möglicherweise aktualisieren Sie Ihren Browser, um den Zwischenspeicher zu löschen. In Internet Explorer hierzu können Sie durch Drücken von **STRG + R**.

    ![Web-app, die in der produktionsumgebung ausgeführt wird](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. In der **GitBash** -Konsole, aktualisieren Sie die remote-URL für den lokalen Git-Repository zum produktionsslot abzielen. Führen Sie hierzu den folgenden Befehl, und Ersetzen Sie die Platzhalter durch Ihren Benutzernamen für die Bereitstellung und der Name Ihrer Web-App ein.

    > [!NOTE]
    > In der folgenden Übung werden Sie Änderungen per Push auf des Produktionsstandorts statt der Stagingumgebung nur für die Einfachheit der Umgebung übertragen. In einem realen Szenario wird empfohlen, die Änderungen in der Stagingumgebung zu überprüfen, bevor bis hin zur Produktion heraufgestuft.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Übung 3: Ausführen des Rollbacks für Bereitstellung in der Produktion

Es gibt Szenarien, in denen Sie keine staging-Slot ausführen hot-Austausch zwischen den Staging- und produktionsumgebungen, z. B. bei Verwendung mit **frei** oder **Shared** Modus. In jenen Szenarien sollten Sie Ihre Anwendung in einer testumgebung – entweder lokal oder an einem Remotestandort – Testen vor der Bereitstellung bis hin zur Produktion. Es ist jedoch möglich, dass ein Problem nicht erkannt wird, während der Testphase am Produktionsstandort auftreten kann. In diesem Fall ist es wichtig, einen Mechanismus zum problemlos wechseln auf eine vorherige und stabiler und ausgereifter Version der Anwendung so schnell wie möglich zu verfügen.

In **Azure App Service**, dauerhaften Bereitstellung über quellcodeverwaltung macht diese mögliche Dank der **erneut bereitstellen** Aktion im Verwaltungsportal verfügbar. Azure verfolgt die per Push auf das Repository übertragen Commits zugeordneten Bereitstellungen und bietet eine Option aus, um die Anwendung mithilfe eines Ihrer vorherigen Bereitstellungen zu einem beliebigen Zeitpunkt erneut bereitstellen.

In dieser Übung führen Sie eine Änderung an den Code in der **Meister Quiz** Anwendung, die absichtlich Fügt eine *Fehler*. Die Anwendung bis hin zur Produktion für die Fehlermeldung wird bereitgestellt, und klicken Sie dann Sie werden Nutzen der Funktion erneut bereitstellen, auf den vorherigen Zustand zurückgesetzt.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Aufgabe 1: Aktualisieren der Meister Quiz-Anwendung

In diesem Schritt gestalten Sie einen kleinen Teil des Codes von der **TriviaController** Klasse Teil der Logik zu extrahieren, die die ausgewählte Quiz-Option in eine neue Methode aus der Datenbank abruft.

1. Wechseln Sie zu der Instanz von Visual Studio mit der **GeekQuiz** Lösung aus der vorherigen Übung.
2. In **Projektmappen-Explorer**öffnen die **TriviaController.cs** Datei innerhalb der **Controller** Ordner.
3. Suchen Sie die **StoreAsync** Methode, und wählen Sie der Code in der folgenden Abbildung hervorgehoben.

    ![Wenn Sie den Code auswählen](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Wenn Sie den Code auswählen*
4. Mit der rechten Maustaste in des ausgewählten Codes, erweitern Sie die **Umgestalten** Menü **Methode extrahieren...** .

    ![Den Code extrahieren als eine neue Methode](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Wählen die Extract-Methode*
5. In der **Methode extrahieren** Dialogfeld Feld "Name" die neue Methode *MatchesOption* , und klicken Sie auf **OK**.

    ![Dabei werden der Methodenname](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Angeben des Namens für die extrahierten-Methode*
6. Wird in der ausgewählte Code extrahiert die **MatchesOption** Methode. Der resultierende Code wird im folgenden Codeausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Drücken Sie **STRG + S** um die Änderungen zu speichern.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Aufgabe 2 – die Meister Quiz Anwendung erneut bereitzustellen

Sie werden jetzt die Änderungen mithilfe von Push übertragen, die in der vorherigen Aufgabe im Repository vorgenommen wird, die eine neue Bereitstellung in der produktionsumgebung ausgelöst wird. Klicken Sie dann, Fehlerbehebung Sie ein Problem mit der **F12-Entwicklungstools** von Internet Explorer bereitgestellt, und führen Sie dann einen Rollback auf die vorherige Bereitstellung aus dem Azure-Verwaltungsportal.

1. Öffnen Sie ein neues **Git Bash** Konsole, um die aktualisierte Anwendung auf Azure App Service bereitzustellen.
2. Führen Sie die folgenden Befehle, die Änderungen in Azure zu übertragen. Update der *[YOUR-ANWENDUNGSPFAD]* Platzhalter durch den Pfad zu der **GeekQuiz** Lösung. Sie werden aufgefordert, Ihr Kennwort für die Bereitstellung.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Der Speicherort der umgestaltete Code in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Der Speicherort der umgestaltete Code in Azure*
3. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-app (z. B. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmeldeinformationen.
4. Drücken Sie **F12** um die Entwicklungstools zu starten, wählen die **Netzwerk** Registerkarte, und klicken Sie auf die **wiedergeben** Schaltfläche zum Starten der Aufzeichnung.

    ![Starten des Netzwerk-Aufzeichnung](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Netzwerk Aufzeichnung starten")

    *Netzwerk-Aufzeichnung starten*
5. Eine Option für das Quiz auswählen. Sie sehen, dass nichts passiert.
6. In der **F12** Fenster, den Eintrag, der die HTTP POST-Anforderung enthält eine HTTP **500** Ergebnis.

    ![HTTP 500 Fehler](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Fehler*
7. Wählen Sie die **Konsole** Registerkarte. Mit den Details der Ursache ist ein Fehler protokolliert.

    ![Protokollierte Fehler](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Protokollierte Fehler*
8. Suchen Sie nach dem Detailabschnitt des Fehlers. Es ist offensichtlich, wird dieser Fehler durch den Code, der Sie ein Commit in den vorherigen Schritten Umgestaltung verursacht.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`
9. Schließen Sie den Browser nicht.
10. Wechseln Sie in einer neuen Browserinstanz zu den [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.
11. Wählen Sie **Websites** , und klicken Sie auf die Web-app, die Sie in der Übung 2 erstellt haben.
12. Navigieren Sie zu der **Bereitstellungen** Seite. Beachten Sie, dass alle ausgeführten Commits in der Bereitstellungsverlauf aufgeführt sind.

    ![Liste der vorhandenen Bereitstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Liste der vorhandenen Bereitstellungen*
13. Wählen Sie die letzten Commit aus, und klicken Sie auf **erneut bereitstellen** in der Befehlszeile.

    ![Erneutes Bereitstellen des vorherigen Commits](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Erneutes Bereitstellen des vorherigen Commits*
14. Wenn Sie dazu aufgefordert werden, um zu bestätigen, klicken Sie auf **Ja**.

    ![Bestätigen die erneute Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zurück an die Browserinstanz mit Ihrer Web-app, und drücken Sie **STRG + F5**.
16. Klicken Sie auf eine der Optionen. Die Flip Animation jetzt erfolgen soll und das Ergebnis (*richtigen/falschen*) wird angezeigt.
17. (Optional) Wechseln Sie zu der **Git Bash** Konsole, und führen Sie die folgenden Befehle, um die letzten Commit wiederherzustellen.

    > [!NOTE]
    > Diese Befehle erstellen einen neuen Commit, der alle Änderungen in das Git-Repository wird rückgängig gemacht, die in der fehlerhaften Commit vorgenommen wurden. Azure die Anwendung mit der neuen Commit klicken Sie dann erneut bereit.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Übung 4: Skalierung mit dem Azure-Speicher

**BLOBs** sind die einfachste Methode zum Speichern großer Mengen unstrukturierter Text-oder Binärdaten, z. B. Video, Audio und Images. Verschieben die statische Inhalte Ihrer Anwendung in den Speicher, wird Ihre Anwendung skalieren, indem Sie die Bilder oder Dokumente direkt an den Browser bereitstellt.

In dieser Übung wird den statischen Inhalt der Anwendung, um einen Blob-Container verschoben werden. Konfigurieren die Anwendung hinzufügen wird ein **ASP.NET URL rewrite Regel** in der **"Web.config"** Ihres Inhalts an den Blob-Container umleiten.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Aufgabe 1 – erstellen ein Azure Storage-Konto

In dieser Aufgabe erfahren Sie, wie Sie ein neues Speicherkonto mithilfe des Verwaltungsportals erstellen.

1. Navigieren Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.
2. Wählen Sie **neue | Datendienste | Speicher | Schnellerfassung** beim Starten der Erstellung eines neuen Speicherkontos. Geben Sie einen eindeutigen Namen für das Konto, und wählen Sie eine **Region** aus der Liste. Klicken Sie auf **Speicherkonto erstellen** um den Vorgang fortzusetzen.

    ![Erstellen eines neuen Speicherkontos](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "ein neues Speicherkonto erstellen")

    *Erstellen eines neuen Speicherkontos*
3. In der **Speicher** Abschnitt, warten Sie, bis ändert sich der Status des neuen Speicherkontos zu *Online* weiterzuverwenden mit den folgenden Schritt aus.

    ![Erstellte Speicherkonto](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "erstellte Speicherkonto")

    *Erstellte Speicherkonto*
4. Klicken Sie auf den Namen des Speicherkontos, und klicken Sie dann auf die **Dashboard** Link am oberen Rand der Seite. Die **Dashboard** Seite bietet Ihnen Informationen über den Status des Kontos und die Dienstendpunkte, die in Ihren Anwendungen verwendet werden können.

    ![Anzeigen von Speicherkonto-Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Speicherkonto-Dashboard anzeigen")

    *Anzeigen von Speicherkonto-Dashboard*
5. Klicken Sie auf die **Zugriffstastenverwaltung** Schaltfläche in der Navigationsleiste.

    ![Schaltfläche "Zugriffsschlüssel verwalten"](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Zugriffstastenverwaltung-Schaltfläche")

    *Verwalten von Zugriffstasten-Schaltfläche*
6. In der **Zugriffsschlüssel verwalten** (Dialogfeld), kopieren die **Speicherkontoname** und **primärer Zugriffsschlüssel** , wie Sie in der folgenden Übung benötigt werden. Schließen Sie dann das Dialogfeld.

    ![Verwalten Zugriffsschlüssel Dialogfeld](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Zugriffsschlüssel verwalten (Dialogfeld)")

    *Verwalten des Zugriffsschlüssels (Dialogfeld)*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Aufgabe 2: Hochladen eines Medienobjekts in Azure Blob-Speicher

In dieser Aufgabe verwenden Sie das Server-Explorer-Fenster in Visual Studio für die Verbindung mit Ihrem Speicherkonto. Anschließend erstellen Sie einen Blob-Container und Hochladen einer Datei mit dem Logo Meister Quiz in den Container.

1. Wechseln Sie zu der Instanz von Visual Studio mit der **GeekQuiz** Lösung aus der vorherigen Übung.
2. Wählen Sie in der Menüleiste **Ansicht** , und klicken Sie dann auf **Server-Explorer**.
3. In **Server-Explorer**, mit der rechten Maustaste die **Azure** Knoten, und wählen **mit Azure verbinden...** . Melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.

    ![Herstellen einer Verbindung mit Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Herstellen einer Verbindung mit Azure*
4. Erweitern Sie die **Azure** Knoten mit der rechten Maustaste **Speicher** , und wählen Sie **Externspeicher anfügen...** .
5. In der **neues Speicherkonto hinzufügen** Dialogfeld Geben Sie die **Kontoname** und **kontoschlüssel** Sie erworben haben, in der vorherigen Aufgabe und auf **OK**.

    ![Neues Speicherkonto Dialogfeld "hinzufügen"](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Neues Speicherkonto Dialogfeld "hinzufügen"*
6. Ihr Speicherkonto sollte angezeigt werden, unter der **Speicher** Knoten. Erweitern Sie Ihr Speicherkonto an, mit der rechten Maustaste **Blobs** , und wählen Sie **Blob-Container erstellen...** .

    ![Blob-Container erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob-Container erstellen")

    *Blob-Container erstellen*
7. In der **Erstellen eines Blobcontainers** Dialogfeld Feld, geben Sie einen Namen für den Blob-Container aus, und klicken Sie auf **OK**.

    ![Dialogfeld für Blob-Container erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob-Container erstellen (Dialogfeld)")

    *Erstellen von Blob-Container (Dialogfeld)*
8. Die neuen Blob-Container hinzugefügt werden sollen, um die **Blobs** Knoten. Ändern Sie die Zugriffsberechtigungen in den Container für den Container als öffentliches Profil festzulegen. Zu diesem Zweck Maustaste die **Bilder** Container, und wählen **Eigenschaften**.

    ![erstellt Abbilder für Containereigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Bilder Containereigenschaften")

    *Bilder Containereigenschaften*
9. In der **Eigenschaften** legen die **öffentlichen Lesezugriff** auf **Container**.

    ![Öffentlichen Lesezugriff auf die Änderung](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "öffentlichen Lesezugriff auf die Änderung")

    *Ändern von öffentlichen Lesezugriff-Eigenschaft*
10. Wenn Sie aufgefordert werden, wenn Sie sicher sind, klicken Sie zum Ändern der Eigenschaft öffentlichen Zugriff werden sollen **Ja**.

    ![Microsoft Visual Studio-Warnung](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio-Warnung")

    *Microsoft Visual Studio-Warnung*
11. In **Server-Explorer**, mit der rechten Maustaste die **Bilder** Blob-Container, und wählen **Blob-Container anzeigen**.

    ![Anzeigen von Blob-Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob-Container anzeigen")

    *Blob-Container anzeigen*
12. Der Image-Container sollte in einem neuen Fenster geöffnet, und eine Legende ohne Einträge angezeigt werden soll. Klicken Sie auf die **hochladen** Symbol, um eine Datei in den Blob-Container hochzuladen.

    ![Der Image-Container ohne Einträge](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Image-Container ohne Einträge")

    *Der Image-Container ohne Einträge*
13. In der **Blob hochladen** Dialogfeld Feld, navigieren Sie zu der **Bestand** Ordner die Umgebung fertiggestellt. Wählen Sie die **Logo big.png** Datei, und klicken Sie auf **öffnen**.
14. Warten Sie, bis die Datei hochgeladen wurde. Wenn der Upload abgeschlossen ist, sollte die Datei im Container Bilder aufgeführt. Der Eintrag in der Maustaste, und wählen **URL kopieren**.

    ![Kopieren von Blob-URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Kopieren von Blob-Datei-URL")

    *Kopieren von Blob-URL*
15. Öffnen Sie Internet Explorer, und fügen Sie die URL. Die folgende Abbildung sollte im Browser angezeigt werden.

    ![Blob-Speicher von Windows-Logo big.png Bereitstellungsabbilds unter](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "Logo big.png Image aus dem Speicher")

    *logo-big.png image from Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Aufgabe 3: aktualisieren die Lösung, um statischen Inhalt von Azure Blob-Speicher nutzen.

In dieser Aufgabe Konfigurieren Sie die **GeekQuiz** Lösung nutzen Sie das Bild hochgeladen in Azure Blob Storage (statt dem Image befindet sich in der Web-app) durch Hinzufügen einer ASP.NET URL-neuschreibungsregel in der **"Web.config"**Datei.

1. Öffnen Sie in Visual Studio die **"Web.config"** Datei innerhalb der **GeekQuiz** Projekt, und suchen Sie die  **&lt;"System.Webserver"&gt;**  Element.
2. Fügen Sie den folgenden Code zum Hinzufügen eine URL-rewrite Regel, aktualisieren den Platzhalter mit den Namen des Speicherkontos.

    (Codeausschnitt - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URLs versteht man eingehende Webanforderungen durch Controlleraktionsmethoden abfangen und die Anforderung zu einer anderen Ressource umleiten. Die Regeln umschreiben URL weist das Umschreiben von Modul, wenn eine Anforderung umgeleitet werden muss, und, in denen sollten umgeleitet werden. Eine Umschreiben von Regel besteht aus zwei Zeichenfolgen: das entsprechende Muster, in der angeforderten URL gesucht (normalerweise mithilfe von regulären Ausdrücken), und Ersetzen Sie das Muster, wenn die Zeichenfolge gefunden. Weitere Informationen finden Sie unter [URLs in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Drücken Sie **STRG + S** um die Änderungen zu speichern.
4. Öffnen Sie ein neues **Git Bash** Konsole, um die aktualisierte Anwendung auf Azure App Service bereitzustellen.
5. Führen Sie die folgenden Befehle, die Änderungen in Azure zu übertragen. Update der *[YOUR-ANWENDUNGSPFAD]* Platzhalter durch den Pfad zu der **GeekQuiz** Lösung. Sie werden aufgefordert, Ihr Kennwort für die Bereitstellung.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Update wird in Azure bereitstellen.](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Update wird in Azure bereitstellen.*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Aufgabe 4: Überprüfung

In dieser Aufgabe verwenden Sie **Internet Explorer** zum Durchsuchen der **Meister Quiz** Anwendung, und überprüfen Sie, dass die URL-rewrite-Regel für Bilder funktioniert, und Sie werden umgeleitet, auf das Abbild auf gehostete **Azure-Blob Speicher**.

1. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-app (z. B. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmeldeinformationen.

    ![Zeigt die Meister Quiz Web-app mit dem Image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Meister Quiz Web-app mit dem Image anzeigen")

    *Zeigt die Meister Quiz Web-app mit dem image*
2. Drücken Sie **F12** um die Entwicklungstools zu starten, wählen die **Netzwerk** Registerkarte und beginnen mit der Aufzeichnung.

    ![Starten des Netzwerk-Aufzeichnung](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Netzwerk Aufzeichnung starten")

    *Netzwerk-Aufzeichnung starten*
3. Drücken Sie **STRG + F5** zum Aktualisieren der Webseite.
4. Sobald die Seite vollständig geladen wurde, sehen Sie eine HTTP-Anforderung für die **/img/logo-big.png** URL mit einem HTTP- **301** Ergebnis (Redirect) und eine andere Anforderung zum `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL mit dem HTTP- **200** Ergebnis.

    ![Überprüfen die URL-Umleitung](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "angezeigt, die mit der Umleitung Entwicklungstools")

    *Überprüfen die URL-Umleitung*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Übung 5: Automatische Skalierung für Web-Apps verwenden

> [!NOTE]
> In dieser Übung ist optional, da sie Unterstützung für den erfordert &amp; Leistungstests, die nur **Visual Studio 2013 Ultimate Edition**. Weitere Informationen zu bestimmten Funktionen von Visual Studio 2013 Versionsvergleich [hier](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** bietet die automatische Skalierung für webapps, die im **Modus "Standard"**. Automatische Skalierung ermöglicht Azure die Anzahl der Instanzen eines je nach Auslastung Ihrer Web-app automatisch zu skalieren. Wenn automatische Skalierung aktiviert ist, wird Azure überprüft, einmal alle fünf Minuten die CPU Ihrer Web-app und Instanzen nach Bedarf zu diesem Zeitpunkt hinzugefügt. Wenn die CPU-Auslastung nicht ausreicht, wird Azure Instanzen einmal alle zwei Stunden entfernen, um sicherzustellen, dass die Leistung Ihrer Web-App nicht beeinträchtigt wird.

In dieser Übung führen Sie über die erforderlichen Schritte zum Konfigurieren der **zur automatischen Skalierung** feature für die **Meister Quiz** Web-app. Diese Funktion überprüft durch Ausführen eines Auslastungstests von Visual Studio zum Generieren von genügend CPU-Auslastung der Anwendung ein Upgrade der Instanz ausgelöst.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Aufgabe 1: Konfigurieren von automatischen Skalierung basierend auf der CPU-Metrik

In dieser Aufgabe verwenden Sie das Azure-Verwaltungsportal auf um die Funktion für automatische Skalierung für die Web-app zu aktivieren, die Sie in der Übung 2 erstellt haben.

1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com/)Option **Websites** , und klicken Sie auf die Web-app, die Sie in der Übung 2 erstellt haben.
2. Navigieren Sie zu der **Skalierung** Seite. Klicken Sie unter der **Kapazität** Abschnitt **CPU** für die **nach Metrik skalieren** Konfiguration.

    > [!NOTE]
    > Für die Skalierung nach CPU aus, passt die Anzahl der Instanzen, die die app verwendet, wenn die CPU-Auslastung ändert dynamisch von Azure an.

    ![Auswahl zur Skalierung nach CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "die CPU-Metrik für die automatische Skalierung auswählen")

    *Skalierung nach CPU auswählen*
3. Ändern der **Ziel-CPU** Konfiguration **20**-**40** Prozent.

    > [!NOTE]
    > Dieser Bereich stellt die durchschnittliche CPU-Nutzung für Ihre Web-app dar. Azure hinzu oder Entfernen von Instanzen, um Ihre Web-app in diesem Bereich bleibt. Die minimale und maximale Anzahl der Instanzen, die für die Skalierung verwendet wird angegeben, der **Instanzenanzahl** Konfiguration. Azure geht nie oberhalb oder über diesen Grenzwert hinaus.
    > 
    > Die Standardeinstellung **Ziel-CPU** Werte nur für die im Rahmen dieser Übung geändert werden. Durch Konfigurieren des CPU-Bereichs mit kleinen Werten, werden Sie sich die Chancen erhöhen Trigger automatische Skalierung, wenn eine moderate Last für die Anwendung befindet.

    ![Ändern die Ziel-CPU, als 20 bis 40 Prozent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Ändern des Ziels-CPU, um zwischen 20 und 40 Prozent liegen.")

    *Ändern der Ziel-CPU, um 20 bis 40 Prozent zu sein.*
4. Klicken Sie auf **speichern** in der Befehlsleiste, um die Änderungen zu speichern.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Aufgabe 2 – Auslastungstests mit Visual Studio

Nun, dass **zur automatischen Skalierung** wurde konfiguriert, erstellen Sie eine **Webleistungs- und Test-Projekt geladen** in Visual Studio einige CPU-Auslastung für Ihre Web-app generiert.

1. Open **Visual Studio Ultimate 2013** , und wählen Sie **Datei | Neue | Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **Webleistungs- und Auslastungstestprojekt** unter der **Visual C# | Test** Registerkarte. Stellen Sie sicher, dass **.NET Framework 4.5** ist ausgewählt, nennen Sie das Projekt *WebAndLoadTestProject*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Erstellen eines neuen Projekts für Web- und Auslastungstests](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Erstellen eines neuen Projekts für Web- und Auslastungstests")

    *Erstellen eines neuen Projekts für Web- und Auslastungstests*
3. In der **WebTest1.webtest** mit der rechten Maustaste die **WebTest1** Knoten, und klicken Sie auf **anfordern hinzufügen**.

    ![Hinzufügen einer Anforderung WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 eine Anforderung hinzugefügt")

    *Hinzufügen einer Anforderung WebTest1*
4. In der **Eigenschaften** Fenster für den neuen Anforderungsknoten aktualisiert die **Url** Eigenschaft, um auf die URL Ihrer Web-App zu verweisen (z. B.  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Ändern der Url-Eigenschaft](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Ändern der Url-Eigenschaft")

    *Ändern der Url-Eigenschaft*
5. In der **WebTest1.webtest** Fenster mit der rechten Maustaste **WebTest1** , und klicken Sie auf **Schleife hinzufügen...** .

    ![Hinzufügen einer Schleife zu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Hinzufügen einer Schleife zu WebTest1")

    *Hinzufügen einer Schleife zu WebTest1*
6. In der **Bedingungsregel hinzufügen und Elemente zur Schleife** wählen Sie im Dialogfeld die **For-Schleife** -Regel und ändern Sie die folgenden Eigenschaften.

    1. **Beenden Sie Wert:** 1000
    2. **Kontext der Parametername:** Iterator
    3. **Inkrementwert:** 1

    ![Auswählen der For-Schleife-Regel, und aktualisieren Sie die Eigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Sie die For-Schleife-Regel auswählen und Aktualisieren der Eigenschaften")

    *Auswählen der For-Schleife-Regel, und Aktualisieren der Eigenschaften*
7. Klicken Sie unter der **Elemente in der Schleife** Abschnitt, wählen Sie die Anforderung, die Sie zuvor das erste und letzte Element, für die Schleife werden erstellt. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Auswahl der ersten und letzten Elemente für die Schleife](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Auswahl der ersten und letzten Elemente für die Schleife")

    *Auswahl der ersten und letzten Elemente für die Schleife*
8. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WebAndLoadTestProject** Projekt, erweitern Sie die **hinzufügen** Menü **Auslastungstest...** .

    ![Das Projekt WebAndLoadTestProject einen Auslastungstest hinzugefügt](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject-Projekt einen Auslastungstest hinzugefügt")

    *Hinzufügen eines Auslastungstests zum WebAndLoadTestProject-Projekt*
9. In der **Assistent für neuen Auslastungstest** (Dialogfeld), klicken Sie auf **Weiter**.

    ![Assistent für neuen Auslastungstest](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Assistent für neuen Auslastungstest")

    *Assistent für neuen Auslastungstest*
10. In der **Szenario** Seite **verwenden Sie keine Reaktionszeiten ein** , und klicken Sie auf **Weiter**.

    ![Auswählen von Verwendung von Reaktionszeiten](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "auswählen, für die Verwendung von Reaktionszeiten")

    *Verwendung von Reaktionszeiten auswählen*
11. In der **Auslastungsmuster** Seite, stellen Sie sicher, dass die **Konstante Auslastung** ausgewählt ist. Ändern der **Benutzeranzahl** auf **250** Benutzer und klicken Sie auf **Weiter**.

    ![Ändern die Benutzeranzahl auf 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "ändern die Benutzeranzahl auf 250")

    *Ändern die Benutzeranzahl auf 250*
12. In der **Testmischungsmodell** Seite **basierend auf sequenzieller Testreihenfolge Reihenfolge** , und klicken Sie auf **Weiter**.

    ![Wählen das Testmischungsmodell](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "das Testmischungsmodell auswählen")

    *Wählen das Testmischungsmodell*
13. In der **Testmischungsmodell** auf **hinzufügen...**  der Mischung einen Test hinzu.

    ![Hinzufügen eines Tests zu testmischung](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "ein Tests zu testmischung hinzufügen")

    *Hinzufügen eines Tests zu testmischung*
14. In der **Tests hinzufügen** (Dialogfeld), doppelklicken Sie auf **WebTest1** hinzuzufügende des Tests, die **ausgewählter Tests** Liste. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Hinzufügen von Tests WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1 Test hinzufügen")

    *Den Test WebTest1 hinzufügen*
15. In der **Testmischung** auf **Weiter**.

    ![Abschließen der Seite "Testmischung"](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Abschließen der Seite "Testmischung"")

    *Abschließen der Seite "Testmischung"*
16. In der **Netzwerkmischung** auf **Weiter**.

    ![Als Nächstes auf der Seite Netzwerkmischung auf](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Klick als Nächstes auf der Seite Netzwerkmischung")

    *Klicken Sie auf Weiter, auf der Seite Netzwerkmischung*
17. In der **Browsermischung** Seite **Internet Explorer 10.0** als Browsertyp und auf **Weiter**.

    ![Auswählen des Browsertyps](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "den Browsertyp auswählen")

    *Den Browsertyp auswählen*
18. In der **Indikatorensätze** auf **Weiter**.

    ![Klicken auf der Seite Indikatorensätze Weiter](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Weiter klicken, auf der Seite Indikatorensätze")

    *Klicken Sie auf Weiter, auf der Seite Indikatorensätze*
19. In der **Testlaufeinstellungen** Seite der **Dauer des Auslastungstests** auf **5 Minuten** , und klicken Sie auf **Fertig stellen**.

    ![Festlegen der Dauer des Auslastungstests auf 5 Minuten](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Festlegen der Dauer des Auslastungstests auf 5 Minuten")

    *Festlegen der Dauer des Auslastungstests auf 5 Minuten*
20. In **Projektmappen-Explorer**, doppelklicken Sie auf die **Local.settings** Datei, um die testeinstellungen zu untersuchen. Standardmäßig verwendet Visual Studio Ihrem lokalen Computer zum Ausführen der Tests.

    > [!NOTE]
    > Alternativ können Sie das Testprojekt zum Ausführen von Auslastungstests in der Cloud mithilfe konfigurieren **Visual Studio Online (VSO)**. VSO stellt eine Cloud-basierten Last testen Diensts, die eine realistischere Auslastung simuliert, die lokale Umgebung Einschränkungen wie CPU-Kapazität, Speicherplatz und Netzwerkbandbreite zu vermeiden. Weitere Informationen zur Verwendung von VSO zum Ausführen von Auslastungstests finden Sie unter [in diesem Artikel](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Testeinstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Aufgabe 3: Überprüfung der automatischen Skalierung

Sie werden nun führen Sie den Auslastungstest aus, den Sie in der vorherigen Aufgabe erstellt haben, wie Ihre Web-app unter Last verhält.

1. In **Projektmappen-Explorer**, doppelklicken Sie auf **"LoadTest1.LoadTest"** Auslastungstest zu öffnen.

    ![Öffnen "LoadTest1.LoadTest"](maintainable-azure-websites-managing-change-and-scale/_static/image99.png ""LoadTest1.LoadTest" Öffnen")

    *Die "LoadTest1.LoadTest" Öffnen*
2. In der **"LoadTest1.LoadTest"** Fenster, klicken Sie auf die erste Schaltfläche in der Toolbox auf den Auslastungstest ausführen.

    ![Ausführen des Auslastungstests](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Ausführen des Auslastungstests")

    *Ausführen des Auslastungstests*
3. Warten Sie, bis der Auslastungstest abgeschlossen ist.

    > [!NOTE]
    > Der Auslastungstest wird mehrere Benutzer, die gleichzeitig senden von Anfragen an die Web-app simuliert. Wenn der Test ausgeführt wird, können Sie die verfügbaren Leistungsindikatoren zum Erkennen von Fehlern, Warnungen und andere Informationen im Zusammenhang mit der Auslastungstestlauf überwachen.

    ![Auslastungstest ausführen](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "warten, bis der Auslastungstest abgeschlossen ist")

    *Auslastungstest*
4. Nachdem der Test abgeschlossen ist, wechseln Sie zurück zum Verwaltungsportal, und navigieren Sie zu der **Skalierung** Ihre Web-app auf der Seite. Klicken Sie unter der **Kapazität** Abschnitt Daraufhin sollte im Diagramm eine neue Instanz automatisch bereitgestellt wurde.

    ![Neue-Instanz automatisch bereitgestellt.](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Neue-Instanz automatisch bereitgestellt.*

    > [!NOTE]
    > Dauert möglicherweise einige Minuten, damit die Änderungen im Diagramm angezeigt (drücken Sie **STRG + F5** in regelmäßigen Abständen, um die Seite zu aktualisieren). Wenn Sie die Änderungen nicht angezeigt werden, können Sie Folgendes versuchen:
    > 
    > - Die Dauer des Auslastungstests erhöhen (z. B. um **10 Minuten**)
    > - Reduzieren Sie die maximalen und minimalen Werte der **Ziel-CPU** Bereich in der Konfiguration zur automatischen Skalierung Ihrer Web-App
    > - Ausführen des Auslastungstests in der Cloud mit **Visual Studio Online**. Weitere Informationen [hier](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie gelernt, wie zum Einrichten und Ihre Anwendung in Produktion Web-apps in Azure bereitstellen. Sie begonnen haben, erkennen und aktualisieren die Datenbanken auf Grundlage **Entity Framework Code First-Migrationen**, klicken Sie dann durch die Bereitstellung neuer Versionen Ihres Standorts mithilfe Fortsetzung **Git** und Ausführen von Rollbacks zu der neueste stabile Version Ihres Standorts. Darüber hinaus haben Sie gelernt, wie beim Skalieren Ihrer app Speicher verwendet, um Ihre statischen Inhalte in einem Blob-Container zu verschieben.
