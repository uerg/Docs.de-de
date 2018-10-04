---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Praxisnahe Übung: verwaltbare Azure-Websites: Verwalten von Änderungen und Skalierungen | Microsoft-Dokumentation'
author: rick-anderson
description: Erfahren Sie in dieser Übungseinheit, wie Microsoft Azure vereinfacht zum Erstellen und Bereitstellen von Websites in der Produktion.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 05181ae1b2d857eea45983d378b28011c1cd755a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578132"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Praxisnahe Übung: verwaltbare Azure-Websites: Verwalten von Änderungen und Skalierungen
====================
durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure vereinfacht das Erstellen und Bereitstellen von Websites für die Produktion. Aber wenn Ihre Anwendung live ist noch nicht fertig, Sie fangen gerade erst! Sie müssen sich ändernden Anforderungen, datenbankaktualisierungen, Skalierung und mehr zu behandeln. Glücklicherweise bietet Azure App Service Ihnen, viele Features, damit Sie Ihre störungsfrei ausgeführte Sites halten können.
>
> Azure bietet sichere und flexible Entwicklung, Bereitstellung und Skalierungsoptionen für Webanwendungen jeder Größe. Nutzen Sie Ihre vorhandenen Tools zum Erstellen und Bereitstellen von Anwendungen ohne umständliche Verwaltung der Infrastruktur.
>
> Bereitstellung einer Produktions-Web-Anwendung selbst in Minutenschnelle durch die einfache Bereitstellung von Inhalten, die mit Ihrem bevorzugten Entwicklungstool erstellt wurden. Sie können eine vorhandene Website direkt aus der quellcodeverwaltung mit Unterstützung für bereitstellen **Git**, **GitHub**, **Bitbucket**, **TFS**, und sogar  **DropBox**. Bereitstellen, direkt aus Ihrer bevorzugten IDE oder über Skripts mit **PowerShell** in Windows oder **CLI** Tools, die unter jedem Betriebssystem ausgeführt wird. Nach der Bereitstellung erhalten bleiben Sie mit Unterstützung für die kontinuierliche Bereitstellung ständig auf dem neuesten Stand.
>
> Azure bietet skalierbare und belastbare Speicher, Sicherung und wiederherstellungslösungen für Daten, Groß oder klein. Bei der Bereitstellung von Anwendungen in einer produktionsumgebung, die Storage-Dienste wie Tabellen, Blobs und SQL-Datenbanken können Sie Ihre Anwendung in der Cloud skalieren.
>
> Mit SQL-Datenbanken ist es wichtig, Ihre Produktivität Datenbank aktuell zu halten, wenn neue Versionen Ihrer Anwendung bereitstellen. Vielen Dank an **Entity Framework Code First-Migrationen**, wurde die Entwicklung und Bereitstellung Ihres Datenmodells zum Aktualisieren Ihrer Umgebungen in wenigen Minuten vereinfacht. Dieser praktischen Übungseinheit zeigt Ihnen die verschiedenen Themen, die auftreten können, wenn Ihre Web-app in produktionsumgebungen in Microsoft Azure bereitstellen.
>
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
>
> Weitere detaillierte dieses Themas finden Sie unter den [Building Real-World Cloud-Apps mit Azure-e-Book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Aktivieren von Entity Framework-Migrationen mit einem vorhandenen Modell
- Aktualisieren des Objektmodells und die Datenbank entsprechend mithilfe von Entity Framework-Migrationen
- Bereitstellen Sie in Azure App Service mithilfe von Git
- Rollback auf eine vorherige Bereitstellung über das Azure-Verwaltungsportal
- Verwenden von Azure Storage skalieren eine WebApp
- Konfigurieren Sie die automatische Skalierung für eine Web-app, die über das Azure-Verwaltungsportal
- Erstellen Sie und konfigurieren Sie ein auslastungstestprojekt in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Azure SDK für .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT-System zur Versionskontrolle](http://git-scm.com/download)
- Microsoft Azure-Abonnement

    - Melden Sie sich für eine [kostenlose Testversion](http://aka.ms/watk-freetrial)
    - Wenn Sie einer eine Visual Studio Professional, Test Professional, Premium oder Ultimate mit MSDN oder MSDN Platforms-Abonnent sind, aktivieren Sie Ihre [MSDN-Vorteil](http://aka.ms/watk-msdn) jetzt zum Entwickeln und Testen in Azure
    - [BizSpark](http://aka.ms/watk-bizspark) -Mitglieder erhalten automatisch das Angebot in ihr Visual Studio Ultimate mit MSDN-Abonnements
    - Mitglieder der [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials-Programm erhalten monatliche Azure-Gutschriften kostenlos

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

1. [Mithilfe von Entity Framework-Migrationen](#Exercise1)
2. [Bereitstellen einer Web-App in der Stagingumgebung](#Exercise2)
3. [Rollback der Bereitstellung durchführen in der Produktion.](#Exercise3)
4. [Skalierung mit dem Azure-Speicher](#Exercise4)
5. [Verwenden für die automatische Skalierung für Web-Apps](#Exercise5) (Optional für Visual Studio 2013 Ultimate Edition)

Geschätzte Zeit für diese testumgebung abzuschließen: **75 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient einem bestimmten Entwicklungsstil und bestimmt, Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Dialogfeld "Optionen". In dieser Übung wird beschrieben, die erforderlichen Aktionen zum Ausführen der jeweiligen Aufgabe in Visual Studio bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wenn Sie eine Sammlung mit anderen Einstellungen für Ihre Entwicklungsumgebung auswählen, unter Umständen gibt es bestehen Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Übung 1: Verwenden von Entity Framework-Migrationen

Wenn Sie eine Anwendung entwickeln, kann Ihr Datenmodell im Laufe der Zeit ändern. Diese Änderungen können das vorhandene Modell in der Datenbank auswirken, (Wenn Sie eine neue Version erstellen), und es ist wichtig, Ihre Datenbank auf dem neuesten Stand, um Fehler zu vermeiden.

Um die nachverfolgung dieser Änderungen in Ihrem Modell zu vereinfachen **Entity Framework Code First-Migrationen** automatisch erkennen von Änderungen des Modells mit dem Schema verglichen und bestimmten Code zum Aktualisieren der Datenbank, generiert Erstellen von neuen *Versionen* Ihrer Datenbank.

In dieser Übung erfahren Sie, wie Sie aktivieren **Migrationen** für Ihre Anwendung und wie Sie ganz einfach erkennen und generieren die Änderungen, um Ihre Datenbanken zu aktualisieren.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Aufgabe 1: Aktivieren von Migrationen

Wechseln Sie in dieser Aufgabe durch die Schritte zur Aktivierung **Entity Framework Code First-Migrationen** auf die **Meister Quiz** Datenbank, die das Modell ändern und zu verstehen, wie diese Änderungen berücksichtigt werden, in der die Datenbank.

1. Öffnen Sie Visual Studio, und öffnen Sie die **GeekQuiz.sln** Projektmappendatei und **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Erstellen Sie die Projektmappe, um das Herunterladen und Installieren der **NuGet** paketabhängigkeiten. Zu diesem Zweck mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **Projektmappe** , oder drücken Sie **STRG + UMSCHALT + B**.
3. Von der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **-Paket-Manager-Konsole**.
4. In der **-Paket-Manager-Konsole**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**. Eine anfängliche Migration basierend auf dem vorhandenen Modell wird erstellt.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Aktivieren von Migrationen](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Migrationen aktivieren")

    *Aktivieren von Migrationen*

    > [!NOTE]
    > Dieser Befehl fügt einen **Migrationen** Ordner Meister Quiz Projekt mit einer Datei namens **Configuration.cs**. Die **Konfiguration** Klasse können Sie Migrationen wie verhält sich, für den Kontext zu konfigurieren.
5. Mit Migrationen, die aktiviert werden, müssen Sie aktualisieren die **Konfiguration** Klasse, um die Datenbank mit den anfänglichen Daten zu füllen, **Meister Quiz** erfordert. Unter **Migrationen**, ersetzen Sie die **Configuration.cs** im Verzeichnis die Datei mit dem die **Source\Assets** Ordner dieser Anleitung.

    > [!NOTE]
    > Da **Migrationen** Ruft die **Ausgangswert** Methode mit jedem Datenbankupdate, müssen Sie achten, dass die Datensätze in der Datenbank nicht dupliziert werden. Die **AddOrUpdate** Methode helfen, um doppelte Daten zu verhindern.
6. Um eine anfängliche Migration hinzuzufügen, geben Sie den folgenden Befehl aus, und drücken Sie dann **EINGABETASTE**.

    > [!NOTE]
    > Stellen Sie sicher, dass es keine Datenbank mit dem Namen ist &quot;GeekQuizProd&quot; in der LocalDB-Instanz.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Hinzufügen von Basisschema Migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Basis schemamigration hinzufügen")

    *Hinzufügen von Basis schemamigration*

    > [!NOTE]
    > **Add-Migration** wird Gerüst die nächste Migration basierend auf Änderungen, die Sie an Ihr Modell vorgenommen haben, seit die letzte Migration erstellt wurde. In diesem Fall wie es die erste Migration des Projekts ist, sie fügen die Skripts zum Erstellen der Tabellen, die für die definiert, der **TriviaContext** Klasse.
7. Führen Sie die Migration aus, um die Datenbank zu aktualisieren, indem Sie den folgenden Befehl ausführen. Für diesen Befehl geben Sie die **ausführlich** Flag zum Anzeigen der SQL-Anweisungen, die in die Zieldatenbank angewendet wird.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Erstellen von Ausgangsdatenbank](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Ausgangsdatenbank erstellen")

    *Erstellen die ursprüngliche Datenbank*

    > [!NOTE]
    > **Update-Database** gelten alle ausstehenden Migrationen für die Datenbank. In diesem Fall erstellt er die Datenbank mithilfe der Verbindungszeichenfolge in der Datei "Web.config" definiert.
8. Wechseln Sie zu **Ansicht** Menü, und öffnen Sie **Objekt-Explorer von SQL Server**.

    ![Öffnen Sie im Objekt-Explorer von SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "in SQL Server-Objekt-Explorer öffnen")

    *In SQL Server-Objekt-Explorer öffnen*
9. In der **Objekt-Explorer von SQL Server** Fenster Verbinden mit der LocalDB-Instanz, indem Sie mit der rechten Maustaste die **SQL Server** und dann **SQL Server hinzufügen...**  Option.

    ![Hinzufügen einer SQL Server-Instanz](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Hinzufügen einer SQL Server-Instanz")

    *Hinzufügen von SQL Server-Instanz zu SQL Server-Objekt-Explorer*
10. Legen Sie die **Servernamen** zu *(Localdb) \v11.0* und lassen Sie **Windows-Authentifizierung** als Authentifizierungsmodus. Klicken Sie auf **Connect** um den Vorgang fortzusetzen.

    ![Herstellen einer Verbindung mit LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Herstellen einer Verbindung mit LocalDB")

    *Herstellen einer Verbindung mit LocalDB*
11. Öffnen der **GeekQuizProd** Datenbank und erweitern Sie die **Tabellen** Knoten. Wie Sie sehen können, die **Update-Database** -Befehl generiert alle Tabellen im definiert die **TriviaContext** Klasse. Suchen Sie die **Dbo. TriviaQuestions** Tabelle, und öffnen Sie den Knoten Columns. In der nächsten Aufgabe Sie diese Tabelle eine neue Spalte hinzufügen und aktualisieren Sie die Datenbank mit **Migrationen**.

    ![Trivia Fragen, die Spalten](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Fragen, die Spalten")

    *Trivia Fragen, die Spalten*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Aufgabe 2: Aktualisieren Schema der Datenbank, die mithilfe von Migrationen

In dieser Aufgabe verwenden Sie **Entity Framework Code First-Migrationen** , erkennen eine Änderung in Ihrem Modell und den erforderlichen Code zum Aktualisieren der Datenbank zu generieren. Aktualisieren Sie die **TriviaQuestions** Entität durch eine neue Eigenschaft hinzugefügt wird. Klicken Sie dann führen Sie Befehle aus, um eine neue Migration Einbeziehung die neue Spalte in der Tabelle zu erstellen.

1. In **Projektmappen-Explorer**, doppelklicken Sie auf die **TriviaQuestion.cs** Datei, die innerhalb der **Modelle** Ordner.
2. Fügen Sie eine neue Eigenschaft mit dem Namen **Hinweis**, wie im folgenden Codeausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. In der **-Paket-Manager-Konsole**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**. Eine neue Migration wird die Änderung in unserem Modell Reflektion erstellt werden.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > Eine Migrationsdatei besteht aus zwei Methoden, **einrichten** und **unten**.
    >
    > - Die **einrichten** Methode angeben, welche Änderungen von der aktuellen Version von unseren Anwendung-Anforderungen an die Anwendung auf die Datenbank verwendet werden.
    > - Die **unten** wird verwendet, um die Änderungen rückgängig zu machen wir hinzugefügt haben die **einrichten** Methode.
    >
    > Wenn die Datenbank-Migration die Datenbank aktualisiert wird, wird es bei allen Migrationen ausgeführt, in der Reihenfolge der Zeitstempel und nur die seit der letzten Aktualisierung nicht verwendet wurden (die \_Tabelle "MigrationHistory" hält Überblick darüber, welche Migrationen angewendet wurden). Die **einrichten** -Methode für alle Migrationen wird aufgerufen, und die Änderungen, die wir in der Datenbank angegeben haben. Wenn wir zu einer vorherigen Migration zurückkehren möchten die **unten** Methode wird aufgerufen, um die Änderungen in umgekehrter Reihenfolge zu wiederholen.
4. In der **-Paket-Manager-Konsole**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Die Ausgabe von der **Update-Database** Befehl generiert ein **Alter Table** SQL-Anweisung, um eine neue Spalte hinzufügen der **TriviaQuestions** Tabelle, wie in der folgenden Abbildung dargestellt.

    ![Fügen Sie die generierte Spalte SQL-Anweisung](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "generierte Spalte SQL-Anweisung hinzufügen")

    *Fügen Sie die generierte Spalte SQL-Anweisung*
6. In **Objekt-Explorer von SQL Server**, aktualisieren Sie die **Dbo. TriviaQuestions** Tabelle, und überprüfen Sie, ob die neue **Hinweis** Spalte angezeigt wird.

    ![Mit der neuen Spalte für den Hinweis](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "mit der neuen Spalte für den Hinweis")

    *Zeigt die neue Spalte für den Hinweis*
7. In der **TriviaQuestion.cs** fügen-Editor eine **StringLength** Einschränkung, die die *Hinweis* -Eigenschaft, wie im folgenden Codeausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. In der **-Paket-Manager-Konsole**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. In der **-Paket-Manager-Konsole**, geben Sie den folgenden Befehl aus, und drücken Sie dann die **EINGABETASTE**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Die Ausgabe des der **Update-Database** Befehl generiert eine **Alter Table** SQL-Anweisung zum Aktualisieren der *Hinweis* Spalte vom Typ der **TriviaQuestions** Tabelle, wie in der folgenden Abbildung dargestellt.

    ![Alter Column-SQL-Anweisung generiert](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter Column-SQL-Anweisung generiert")

    *Alter Column-SQL-Anweisung generiert*
11. In **Objekt-Explorer von SQL Server**, aktualisieren Sie die **Dbo. TriviaQuestions** Tabelle, und überprüfen Sie, ob die **Hinweis** Spaltentyp **nvarchar(150)**.

    ![Zeigt die neue Einschränkung](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "zeigt die neue Einschränkung")

    *Zeigt die neue Einschränkung*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Übung 2: Bereitstellen einer Web-App in der Stagingumgebung

**Web-Apps in Azure App Service** können Sie die Staging-Veröffentlichung durchführen. Bei der Veröffentlichung ein stagingwebsiteslot für jede standardmäßige Produktionswebsite erstellt und ermöglicht es Ihnen, diese Slots ohne Downtime zu wechseln. Dies ist sehr nützlich ist, überprüfen vor der Freigabe für die Öffentlichkeit Änderungen inkrementell Websiteinhalte zu integrieren und Rollback aus, wenn Änderungen werden nicht wie erwartet funktioniert.

In dieser Übung werden Sie Bereitstellen der **Meister Quiz** Anwendung in die Stagingumgebung der Web-app mithilfe von Git-quellcodeverwaltung. Zu diesem Zweck, Sie erstellen die Web-app und Bereitstellen die erforderlichen Komponenten auf das Verwaltungsportal, konfigurieren Sie eine **Git** Repository und übertragen Sie die Anwendung-Quellcode aus dem lokalen Computer im stagingslot. Aktualisieren Sie auch die Produktionsdatenbank mit dem **Code First-Migrationen** Sie in der vorherigen Übung erstellt haben. Sie werden die Anwendung in dieser testumgebung, um zu überprüfen, ob der Vorgang ausgeführt. Wenn Sie zufrieden sind, dass die It arbeitet gemäß Ihren Erwartungen, stuft Sie die Anwendung für die Produktion.

> [!NOTE]
> Um die Veröffentlichung in Stagingumgebung aktivieren zu können, muss die Web-app in **Modus "Standard"**. Beachten Sie, dass zusätzliche Gebühren anfallen, wenn Sie Ihre Web-app in den Standardmodus ändern. Weitere Informationen zu den Preisen finden Sie unter [App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Aufgabe 1 – Erstellen einer Web-App in Azure App Service

In dieser Aufgabe erstellen Sie eine Web-app in **Azure App Service** aus dem Verwaltungsportal. Konfigurieren Sie auch eine **SQL-Datenbank** zum Speichern von Anwendungsdaten und Konfigurieren von einem lokalen Git-Repository für die quellcodeverwaltung.

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.

    ![Melden Sie sich beim Azure-Verwaltungsportal](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Melden Sie sich beim Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlsleiste am unteren Rand der Seite.

    ![Erstellen einer neuen Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Erstellen einer neuen Web-app")

    *Erstellen einer neuen Web-app*
3. Klicken Sie auf **Compute**, **Website** und dann **Benutzerdefiniert erstellen**.

    ![Erstellen einer neuen Web-app mithilfe von benutzerdefinierte Erstellung](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Erstellen einer neuen Web-app mithilfe von benutzerdefinierte Erstellung")

    *Erstellen einer neuen Web-app mithilfe von benutzerdefinierte Erstellung*
4. In der **neue Website - Benutzerdefiniert erstellen** Dialogfeld geben einen verfügbaren **URL** (z. B. *Meister-Quiz*), wählen Sie einen Speicherort in der **Region** Dropdown-Liste, und wählen **Erstellen einer neuen SQL-Datenbank** in die **Datenbank** Dropdown-Liste. Wählen Sie abschließend die **veröffentlichen aus der quellcodeverwaltung** Kontrollkästchen und klicken Sie auf **Weiter**.

    ![Anpassen der neuen Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Anpassen der neuen Web-app*
5. Geben Sie die folgenden Informationen für die datenbankeinstellungen:

   - In der **Namen** Text Geben Sie einen Datenbanknamen an (z. B. *Geekquiz\_Db*)
   - Auf dem Server **Dropdownliste** Liste **neue SQL-Datenbankserver**. Alternativ können Sie einen vorhandenen Server auswählen.
   - In der **Datenbankbenutzername** und **Datenbankkennwort** Felder, geben Sie den Benutzernamen und das Kennwort für den SQL-Datenbankserver. Wenn Sie einen Server auswählen ist bereits vorhanden, Sie werden aufgefordert, das Kennwort.

     ![Die datenbankeinstellungen angeben](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Die datenbankeinstellungen angeben*
6. Klicken Sie auf **Weiter**, um fortzufahren.
7. Wählen Sie **lokales Git-Repository** für die quellcodeverwaltung verwenden, und klicken Sie auf **Weiter**.

    > [!NOTE]
    > Sie können Anmeldeinformationen für die Bereitstellung (Benutzername und Kennwort) aufgefordert.

    ![Das Git-Repository erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Das Git-Repository erstellen*
8. Warten Sie, bis die neue Web-app erstellt wird.

    > [!NOTE]
    > Standardmäßig stellt Azure Domänen bereit, mit denen *azurewebsites.net* sondern bietet auch die Möglichkeit, benutzerdefinierte Domänen, die über das Azure-Verwaltungsportal festgelegt. Allerdings können Sie benutzerdefinierte Domänen nur verwalten, bei Verwendung von bestimmten Azure App Service-Modi.
    >
    > Azure App Service ist in Free, Shared, Basic, Standard und Premium-Editionen verfügbar. Im Free und Shared-Modus werden alle Web-apps in einer Umgebung mit mehreren Mandanten ausgeführt und Kontingente für die Verwendung von CPU, Arbeitsspeicher und Netzwerk haben. Die maximale Anzahl von kostenlosen apps kann mit Ihrem Plan variieren. Im Modus "Standard" Wählen Sie an, welche apps auf dedizierten virtuellen Computern, die entsprechen ausgeführt wird, an die standardmäßige Azure-computeressourcen. Finden Sie die Web-app-Modus-Konfiguration in der **Skalierung** im Menü der Web-app.
    >
    > ![Azure App Service-Modi](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service-Modi")
    >
    > Bei Verwendung von **Shared** oder **Standard** Modus, Sie werden benutzerdefinierte Domänen für Ihre Web-app zu verwalten, indem Sie zu Ihrer app **konfigurieren** Menü und klicken Sie auf **Domänen verwalten** unter *Domänennamen*.
    >
    > ![Verwalten von Domänen](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Domänen verwalten")
    >
    > ![Benutzerdefinierte Domänen verwalten](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "benutzerdefinierte Domänen verwalten")
9. Nachdem die Web-app erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte, um zu überprüfen, dass die neue Web-app ausgeführt wird.

    ![Navigieren in der neuen Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Navigieren in der neuen Web-app*

    ![ausgeführte Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *ausgeführte Web-app*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Aufgabe 2: Erstellen der SQL-Datenbank der Produktion

In dieser Aufgabe verwenden Sie die **Entity Framework Code First-Migrationen** zum Erstellen der Datenbank als Ziel der **Azure SQL-Datenbank** Instanz, die Sie in der vorherigen Aufgabe erstellt haben.

1. Im Verwaltungsportal, wechseln Sie zu der Web-app, die Sie in der vorherigen Aufgabe erstellt haben, und wechseln Sie zu dessen **Dashboard**.
2. In der **Dashboard** auf **Verbindungszeichenfolgen anzeigen** link die **Blick** Abschnitt.

    ![Verbindungszeichenfolgen anzeigen](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Verbindungszeichenfolgen anzeigen")

    *Verbindungszeichenfolgen anzeigen*
3. Kopieren der **Verbindungszeichenfolge** Wert ein, und schließen Sie das Dialogfeld.

    ![Die Verbindungszeichenfolge im Azure-Verwaltungsportal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Verbindungszeichenfolge im Azure-Verwaltungsportal")

    *Die Verbindungszeichenfolge im Azure-Verwaltungsportal*
4. Klicken Sie auf **SQL-Datenbanken** auf die Liste der SQL-Datenbanken in Azure finden Sie unter

    ![SQL-Datenbank-Menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "im Menü die SQL-Datenbank")

    *SQL-Datenbank-Menü*
5. Suchen Sie die Datenbank, die Sie in der vorherigen Aufgabe erstellt haben, und klicken Sie auf dem Server.

    ![SQL-Datenbankserver](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL-Datenbankserver")

    *SQL-Datenbankserver*
6. In der **Schnellstart** Seite des Servers, klicken Sie auf **konfigurieren**.

    ![Menü "konfigurieren"](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menü \"konfigurieren\"")

    *Konfigurieren Sie im Menü*
7. In der **zulässige IP-Adressen** an, klicken Sie im Abschnitt **hinzufügen, die den zulässigen IP-Adressen** Link, um die IP-Adresse für die Verbindung mit dem SQL-Datenbank-Server zu aktivieren.

    ![Zulässige IP-Adressen](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "zulässige IP-Adressen")

    *Zulässige IP-Adressen*
8. Klicken Sie auf **speichern** am unteren Rand der Seite, um den Schritt abzuschließen.
9. Wechseln Sie zurück zu Visual Studio.
10. In der **-Paket-Manager-Konsole**, führen Sie den folgenden Befehl und Ersetzen Sie dabei *[YOUR-CONNECTION-STRING]* Platzhalter durch die Verbindungszeichenfolge, die Sie aus Azure kopiert

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualisieren einer Datenbank für Windows Azure SQL-Datenbank](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Datenbank aktualisieren, die für Windows Azure SQL-Datenbank")

    *Aktualisieren einer Datenbank für Azure SQL-Datenbank*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Aufgabe 3: Bereitstellen von Meister Quiz in die Stagingumgebung, die mithilfe von Git

In dieser Aufgabe können Sie bei der Veröffentlichung in Ihrer Web-app. Klicken Sie dann verwenden Sie Git zum Veröffentlichen der Anwendung Meister Quiz direkt aus Ihrem lokalen Computer in der Stagingumgebung bereit, der Web-app.

1. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Web-app unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Seiten der Web-app-Verwaltung](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Öffnen die Seiten der Web-app-Verwaltung*
2. Navigieren Sie zu der **Skalierung** Seite. Unter den **allgemeine** wählen Sie im Abschnitt **Standard** für die Konfiguration und klicken Sie auf **speichern** in der Befehlsleiste.

    > [!NOTE]
    > Zum Ausführen aller Web-apps in der aktuellen Region und Abonnement in **Standard** Modus, lassen die **Alles markieren** Kontrollkästchen der **Standorte auswählen** Konfiguration. Deaktivieren Sie andernfalls die **Alles markieren** Kontrollkästchen.

    ![Aktualisieren die Web-app in den Standardmodus](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "aktualisieren die Web-app zum Modus \"Standard\"")

    *Aktualisieren die Web-App zum Modus "Standard"*
3. Klicken Sie auf **Ja** um die Änderungen zu bestätigen.

    ![Bestätigung der Änderung in den Standardmodus](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "fortsetzen, die mit Änderungen an der Web-app-Modus")

    *Bestätigung der Änderung in den Standardmodus*
4. Wechseln Sie zu der **Dashboard** Seite, und klicken Sie auf **Enable Staging-Veröffentlichung** unter der **Blick** Abschnitt.

    ![Aktivieren der Veröffentlichung in Stagingumgebung](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "aktivieren – Staging-Veröffentlichung")

    *Aktivieren der Veröffentlichung in Stagingumgebung*
5. Klicken Sie auf **Ja** bei der Veröffentlichung zu aktivieren.

    ![Bestätigt – Staging-Veröffentlichung](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Ja klicken, um Staging-Veröffentlichung zu aktivieren")

    *Bestätigen bei der Veröffentlichung*
6. Erweitern Sie in der Liste der Web-apps die Markierung links vom Namen Ihrer Web-app im stagingslot-Website angezeigt. Es hat den Namen der Web-app gefolgt von ***(staging)***. Klicken Sie auf der Stagingsite, um zur Verwaltungsseite zu wechseln.

    ![Navigieren in der staging-Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigieren in der staging-Web-app")

    *Navigieren in der staging-app*
7. Beachten Sie, dass alle anderen Web-app-Verwaltungsseite, He-Verwaltungsseite aussieht. Navigieren Sie zu der **Bereitstellungen** Seite, und kopieren die **Git-URL** Wert. Sie werden später in dieser Übung verwendet.

    ![Kopieren den Wert der Git-URL](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopieren den Wert der Git-URL*
8. Öffnen Sie ein neues **Git Bash** Konsole und führen Sie die folgenden Befehle aus. Update der *[YOUR-APPLICATION-PATH]* Platzhalter durch den Pfad zu der **GeekQuiz** Lösung befindet sich in der **Source\Ex1-DeployingWebSiteToStaging\Begin** Ordner Dieses Lab.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git-Initialisierung und der erste commit](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git-Initialisierung und der erste commit*
9. Führen Sie den folgenden Befehl aus, um Ihre Web-app an die Remoteinstanz übertragen **Git** Repository. Ersetzen Sie den Platzhalter durch die URL, die Sie aus dem Verwaltungsportal abgerufen haben. Sie werden aufgefordert, Ihr bereitstellungskennwort.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Pushen in Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Pushen in Azure*

    > [!NOTE]
    > Wenn Sie Inhalte auf dem FTP-Host oder das GIT-Repository eine Web-app bereitstellen, müssen Sie Authentifizierung, mit der **Anmeldeinformationen für die Bereitstellung** , die Sie aus der Web-app erstellt **Schnellstart** oder **Dashboard**  Verwaltungsseiten. Wenn Sie nicht, dass Ihre Anmeldeinformationen für die Bereitstellung wissen können Sie ganz einfach mithilfe des Verwaltungsportals zurücksetzen. Öffnen Sie die Web-app **Dashboard** Seite, und klicken Sie auf die **bereitstellungsanmeldeinformationen für die zurücksetzen** Link. Geben Sie ein neues Kennwort ein, und klicken Sie auf **OK**. Anmeldeinformationen für die Bereitstellung gelten für die Verwendung mit allen Web-apps, die Ihrem Abonnement zugeordnet wurde.
10. Um die Web-app wurde erfolgreich per Push übertragen in Azure zu überprüfen, wechseln Sie zurück zum Verwaltungsportal, und klicken Sie auf **Websites**.
11. Suchen Sie Ihre Web-app aus, und erweitern Sie den Eintrag, um die stagingwebsiteslot anzuzeigen. Klicken Sie auf die **Namen** um zur Verwaltungsseite zu wechseln.
12. Klicken Sie auf **Bereitstellungen** , finden Sie unter den **Bereitstellungsverlauf**. Überprüfen Sie, ob ein **aktive Bereitstellung** mit Ihrem  *&quot;anfänglichen Commit&quot;*.

    ![Aktive Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktive Bereitstellung*
13. Klicken Sie abschließend auf **Durchsuchen** in der Befehlsleiste, um die Web-app aufzurufen.

    ![Navigieren zur WebApp](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Navigieren zur WebApp*
14. Wenn die Anwendung erfolgreich bereitgestellt wurde, sehen Sie die Anmeldeseite Meister Quiz.

    > [!NOTE]
    > Der Adress-URL der bereitgestellten Anwendung enthält den Namen der Web-app gefolgt von *-staging*.

    ![Anwendung, die in der Stagingumgebung ausgeführt wird](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Anwendung, die in der Stagingumgebung ausgeführt wird*
15. Wenn Sie die Anwendung zu untersuchen möchten, klicken Sie auf **registrieren** um einen neuen Benutzer zu registrieren. Schließen Sie die Kontodetails, indem Sie ein Benutzername, e-Mail-Adresse und Kennwort einzugeben. Als Nächstes wird die Anwendung die erste Frage des Quiz dargestellt. Beantworten Sie ein paar Fragen, um sicherzustellen, dass es wie erwartet funktioniert.

    ![Anwendung verwendet werden](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Anwendung verwendet werden*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Aufgabe 4: die Web-App für die Produktion heraufstufen

Nun, da Sie überprüft haben, dass die Web-app in der Stagingumgebung ordnungsgemäß funktioniert, können Sie es in die produktionsumgebung überführen. In dieser Aufgabe werden Sie den stagingslot-Website in den produktionsslot für den Standort ausgetauscht.

1. Wechseln Sie zurück zum Verwaltungsportal, und wählen Sie den stagingslot-Website. Klicken Sie auf **austauschen** in der Befehlsleiste.

    ![Wechseln Sie in die Produktion](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Wechseln Sie in die Produktion*
2. Klicken Sie auf **Ja** in das Dialogfeld zur Bestätigung der Swap-Vorgang fortsetzen. Azure wird den Inhalt der Produktionssite mit dem Inhalt der staging-Website sofort ausgetauscht.

    > [!NOTE]
    > Einige Einstellungen aus der bereitgestellten Version werden automatisch auf die Version (z. B. Verbindungszeichenfolge Außerkraftsetzungen "," Handlerzuordnungen "" usw.) kopiert werden, aber andere Einstellungen nicht ändert (z. B. DNS-Endpunkte, SSL-Bindungen, usw.).

    ![Swap-Vorgang bestätigen](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Swap-Vorgang bestätigen*
3. Nachdem der Austausch abgeschlossen ist, wählen Sie den produktionsslot, und klicken Sie auf **Durchsuchen** in der Befehlsleiste am Produktionsstandort zu öffnen. Beachten Sie die URL in die Adressleiste ein.

    > [!NOTE]
    > Sie müssen möglicherweise aktualisieren Sie Ihren Browser, um den Cache zu löschen. In Internet Explorer Sie erreichen dies durch Drücken von **STRG + R**.

    ![In der produktionsumgebung ausgeführte Web-app](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. In der **GitBash** -Konsole, die remote-URL für das lokale Git-Repository zur Ausrichtung auf des produktionsslots zu aktualisieren. Führen Sie zu diesem Zweck den folgenden Befehl, und Ersetzen Sie die Platzhalter durch Ihren Benutzernamen für die Bereitstellung und den Namen der Web-app ein.

    > [!NOTE]
    > In den folgenden Übungen werden Sie Änderungen mit dem Produktionsstandort statt der Stagingumgebung nur für die Einfachheit des Labs mithilfe von Push übertragen. In einem realen Szenario ist es um die Änderungen in der Stagingumgebung zu überprüfen, bevor die Weiterleitung zur Produktion empfohlen.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Übung 3: Bereitstellung Rollback in der Produktion durchführen.

Es gibt Szenarien, in dem Sie kein stagingslots zum Austausch zwischen Staging und Produktion, z. B. im laufenden Betrieb ausführen, wenn Sie mit arbeiten **Free** oder **Shared** Modus. In diesen Szenarien sollten Sie Ihre Anwendung in einer testumgebung – entweder lokal oder an einem Remotestandort – testen, bevor in einer produktionsumgebung bereitstellen. Allerdings ist es möglich, dass ein Problem, das nicht erkannt wird, während der Testphase am Produktionsstandort auftreten kann. In diesem Fall ist es wichtig, einen Mechanismus, um einfach auf eine vorherige und stabile Version der Anwendung so schnell wie möglich zu wechseln.

In **Azure App Service**, kontinuierliche Bereitstellung über quellcodeverwaltung macht diese Dank der Möglichkeit der **erneut bereitstellen** Aktion im Verwaltungsportal verfügbar. Azure überwacht die Bereitstellungen, die Commits per Push an das Repository zugeordnet und bietet eine Option aus, um die Anwendung, die mit Ihrer vorherigen Bereitstellungen, jederzeit erneut bereitzustellen.

In dieser Übung führen Sie eine Änderung an den Code in die **Meister Quiz** -Anwendung, die absichtlich Fügt eine *Fehler*. Stellt Sie bereit, die Anwendung für die Produktion, um den Fehler anzuzeigen, und klicken Sie dann Sie nutzt die Funktion erneut bereitstellen zu den vorherigen Zustand zurückkehren.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Aufgabe 1: Aktualisieren der Meister Quiz-Anwendung

In dieser Aufgabe gestalten Sie ein kleines Stück des Codes, der die **TriviaController** Klasse Teil der Programmlogik zu extrahieren, die die ausgewählte Test-Option in eine neue Methode aus der Datenbank abruft.

1. Wechseln Sie zu Visual Studio-Instanz mit der **GeekQuiz** Lösung aus der vorhergehenden Übung.
2. In **Projektmappen-Explorer**öffnen die **TriviaController.cs** Datei innerhalb der **Controller** Ordner.
3. Suchen Sie die **StoreAsync** Methode, und wählen Sie der Code in der folgenden Abbildung hervorgehoben.

    ![Wählen den code](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Wählen den code*
4. Mit der rechten Maustaste in des ausgewählten Codes, erweitern Sie die **Umgestalten** Menü **Methode extrahieren...** .

    ![Den Code extrahieren als eine neue Methode](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Wählen die Extract-Methode*
5. In der **Methode extrahieren** klicken Sie im Dialogfeld die Namen der neuen Methode *MatchesOption* , und klicken Sie auf **OK**.

    ![Der Name der Methode angeben](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Angeben des Namens für die extrahierte Methode*
6. Der ausgewählte Code wird extrahiert, in der **MatchesOption** Methode. Der resultierende Code ist im folgenden Codeausschnitt dargestellt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Drücken Sie **STRG + S** um die Änderungen zu speichern.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Aufgabe 2: die Geek-Quiz-Anwendung erneut bereitzustellen

Sie werden jetzt die Änderungen mithilfe von Push übertragen, die Sie in der vorherigen Aufgabe an das Repository, die eine neue Bereitstellung in der produktionsumgebung ausgelöst wird. Anschließend werden Sie Problembehandlung, ein Problem mit der **F12-Entwicklungstools** von Internet Explorer bereitgestellt werden soll, und führen Sie einen Rollback auf die vorherige Bereitstellung über das Azure-Verwaltungsportal.

1. Öffnen Sie ein neues **Git Bash** Verwaltungskonsole, um die aktualisierte Anwendung in Azure App Service bereitstellen.
2. Führen Sie die folgenden Befehle aus, um die Änderungen an Azure zu übertragen. Update der *[YOUR-APPLICATION-PATH]* Platzhalter durch den Pfad zu der **GeekQuiz** Lösung. Sie werden aufgefordert, Ihr bereitstellungskennwort.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Pushen umgestaltete Code in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Pushen umgestaltete Code in Azure*
3. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-app (z. B. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmeldeinformationen.
4. Drücken Sie **F12** um die Entwicklungstools zu starten, wählen die **Netzwerk** Registerkarte, und klicken Sie auf die **spielen** Schaltfläche zum Starten der Aufzeichnung.

    ![Starten die Netzwerk-Aufzeichnung](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Netzwerk Aufzeichnung starten")

    *Netzwerk-Aufzeichnung starten*
5. Eine Option des Quiz auswählen. Sie sehen, dass nichts passiert.
6. In der **F12** Fenster der Eintrag die HTTP POST-Anforderung zeigt eine HTTP **500** Ergebnis.

    ![HTTP 500 Fehlermeldung](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Fehlermeldung*
7. Wählen Sie die **Konsole** Registerkarte. Mit den Details der Fehlerursache wird ein Fehler protokolliert.

    ![Protokolliert Fehler](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Protokolliert Fehler*
8. Suchen Sie nach dem Detailabschnitt des Fehlers. Dieser Fehler wird natürlich durch die coderefactoring Sie ein Commit in den vorherigen Schritten verursacht.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`
9. Schließen Sie den Browser nicht.
10. Wechseln Sie in einer neuen Browserinstanz zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.
11. Wählen Sie **Websites** , und klicken Sie auf die Web-app, die Sie in der Übung 2 erstellt haben.
12. Navigieren Sie zu der **Bereitstellungen** Seite. Beachten Sie, dass alle Commits ausgeführt im Bereitstellungsverlauf aufgeführt sind.

    ![Liste der vorhandenen Bereitstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Liste der vorhandenen Bereitstellungen*
13. Wählen Sie den vorherigen Commit aus, und klicken Sie auf **erneut bereitstellen** auf der Befehlsleiste.

    ![Erneute Bereitstellung des vorherigen Commits](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Erneute Bereitstellung des vorherigen Commits*
14. Wenn Sie dazu aufgefordert werden, um zu bestätigen, klicken Sie auf **Ja**.

    ![Bestätigen die erneute Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zurück an die Browserinstanz mit Ihrer Web-app, und drücken Sie **STRG + F5**.
16. Klicken Sie auf eine der Optionen. Die Animation Flip jetzt erfolgen soll und das Ergebnis (*richtigen/falschen*) wird angezeigt.
17. (Optional) Wechseln Sie zu der **Git Bash** Konsole und führen Sie die folgenden Befehle, um den vorherigen Commit wiederherzustellen.

    > [!NOTE]
    > Diese Befehle erstellen einen neuen Commit, der alle Änderungen im Git-Repository wird rückgängig gemacht, die in den fehlerhaften Commit vorgenommen wurden. Azure wird dann die Anwendung mit den neuen Commit erneut bereitstellen.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Übung 4: Skalierung mit dem Azure-Speicher

**BLOBs** sind die einfachste Methode zum Speichern großer Mengen unstrukturierter Text-oder Binärdaten wie Video-, Audio- und Bilddaten. Verschieben die statische Inhalte von Ihrer Anwendung in den Speicher, hilft Ihrer Anwendung zu skalieren, durch das Speichern von Bildern oder Dokumenten direkt an den Browser.

In dieser Übung werden Sie die statische Inhalte Ihrer Anwendung auf einen Blob-Container verschoben. Konfigurieren Ihrer Anwendung hinzufügen, wird ein **ASP.NET URL rewrite-Regel** in die **"Web.config"** um Ihre Inhalte an den Blob-Container umzuleiten.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Aufgabe 1 – erstellen ein Azure Storage-Konto

In dieser Aufgabe lernen Sie, wie Sie ein neues Speicherkonto im Verwaltungsportal erstellen.

1. Navigieren Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und melden Sie sich mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet.
2. Wählen Sie **neue | Data Services | Speicher | Schnellerfassung** zum Erstellen eines neuen Speicherkontos. Geben Sie einen eindeutigen Namen für das Konto, und wählen Sie eine **Region** aus der Liste. Klicken Sie auf **Create Storage Account** um den Vorgang fortzusetzen.

    ![Erstellen eines neuen Speicherkontos](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "ein neues Speicherkonto erstellen")

    *Erstellen eines neuen Speicherkontos*
3. In der **Storage** Abschnitt, warten Sie, bis der Status des neuen Speicherkontos zu wechselt *Online* um mit den folgenden Schritt fortfahren.

    ![Das erstellte Speicherkonto](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "erstellte Speicherkonto")

    *Das erstellte Speicherkonto*
4. Klicken Sie auf den Namen des Speicherkontos, und klicken Sie dann auf die **Dashboard** Link am oberen Rand der Seite. Die **Dashboard** Seite erhalten Sie Informationen über den Status des Kontos und die Dienstendpunkte, die in Ihren Anwendungen verwendet werden können.

    ![Anzeigen von Speicherkonto-Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Speicherkonto-Dashboard anzeigen")

    *Anzeigen von Speicherkonto-Dashboard*
5. Klicken Sie auf die **Zugriffstastenverwaltung** Schaltfläche in der Navigationsleiste auf.

    ![Schaltfläche "Zugriffsschlüssel verwalten"](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Schaltfläche \"Zugriffsschlüssel verwalten\"")

    *Zugriffstasten-Schaltfläche "verwalten"*
6. In der **Zugriffsschlüssel verwalten** (Dialogfeld), kopieren die **Speicherkontonamen** und **primärer Zugriffsschlüssel** , wie Sie sie in der folgenden Übung benötigen. Schließen Sie dann das Dialogfeld ein.

    ![Dialogfeld für Zugriffsschlüssel verwalten](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Zugriffsschlüssel verwalten (Dialogfeld)")

    *Dialogfeld für die Schlüssel für den Zugriff verwalten*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Aufgabe 2: Hochladen eines Medienobjekts in Azure Blob Storage

In dieser Aufgabe verwenden Sie das Server-Explorer-Fenster aus Visual Studio zur Verbindung mit Ihrem Speicherkontos. Sie klicken Sie dann einen Blob-Container erstellen und Hochladen einer Datei mit dem Logo Meister Quiz in den Container.

1. Wechseln Sie zu Visual Studio-Instanz mit der **GeekQuiz** Lösung aus der vorhergehenden Übung.
2. Wählen Sie in der Menüleiste **Ansicht** , und klicken Sie dann auf **Server-Explorer**.
3. In **Server-Explorer**, mit der rechten Maustaste die **Azure** Knoten, und wählen **mit Azure verbinden...** . Melden Sie sich mit der Microsoft-Konto, das Ihrem Abonnement zugeordnet.

    ![Mit Windows Azure verbinden](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Verbinden mit Azure*
4. Erweitern Sie die **Azure** Knoten mit der rechten Maustaste **Storage** , und wählen Sie **Externspeicher anfügen...** .
5. In der **neues Speicherkonto hinzufügen** Dialogfeld Geben Sie die **Kontoname** und **kontoschlüssel** Sie abgerufen haben, in der vorherigen Aufgabe und auf **OK**.

    ![Dialogfeld "Neues Speicherkonto" hinzufügen](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Dialogfeld "Neues Speicherkonto" hinzufügen*
6. Das Speicherkonto sollte angezeigt werden, unter dem **Storage** Knoten. Erweitern Sie Ihr Speicherkonto, mit der rechten Maustaste **Blobs** , und wählen Sie **Blob-Container erstellen...** .

    ![Erstellen eines Blobcontainers](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob-Container erstellen")

    *Erstellen eines Blobcontainers*
7. In der **Blob-Container erstellen** Dialogfeld Feld, geben Sie einen Namen für den Blob-Container aus, und klicken Sie auf **OK**.

    ![Dialogfeld für Blob-Container erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Dialogfeld für die Blob-Container erstellen")

    *Dialogfeld für die Blob-Container erstellen*
8. Die neuen Blob-Container hinzugefügt werden sollen, um die **Blobs** Knoten. Ändern Sie die Zugriffsberechtigungen in den Container auf den Container öffentlich zu machen. Dazu, mit der Maustaste der **Images** Container, und wählen **Eigenschaften**.

    ![Images von Containereigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Containereigenschaften-images")

    *Abbildeigenschaften für container*
9. In der **Eigenschaften** legen die **öffentlichen Lesezugriff** zu **Container**.

    ![Ändern der öffentlichen Lesezugriff-Eigenschaft](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "öffentlichen Lesezugriff-Eigenschaft ändern")

    *Ändern der öffentlichen Lesezugriff-Eigenschaft*
10. Wenn Sie aufgefordert werden, wenn Sie sicher sind, Sie möchten die Public-Access-Eigenschaft zu ändern, klicken Sie auf **Ja**.

    ![Microsoft Visual Studio-Warnung](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio-Warnung")

    *Microsoft Visual Studio-Warnung*
11. In **Server-Explorer**, mit der rechten Maustaste den **Images** Blob-Container, und wählen **Blob-Container anzeigen**.

    ![Anzeigen von Blob-Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob-Container anzeigen")

    *Blob-Container anzeigen*
12. Sollten die Image-Container in einem neuen Fenster öffnen und eine Legende ohne Einträge angezeigt werden soll. Klicken Sie auf die **hochladen** Symbol, um eine Datei in den Blob-Container hochzuladen.

    ![Der Image-Container ohne Einträge](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Image-Container ohne Einträge")

    *Der Image-Container ohne Einträge*
13. In der **Blob hochladen** Dialogfeld Feld, navigieren Sie zu der **Assets** Ordner der Umgebung. Wählen Sie die **-Logo-big.png** Datei, und klicken Sie auf **öffnen**.
14. Warten Sie, bis die Datei hochgeladen wurde. Wenn der Upload abgeschlossen ist, sollte die Datei im Container Images aufgeführt werden. Der Eintrag in der Maustaste, und wählen **URL kopieren**.

    ![Kopieren von Blob-URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Kopieren von Blob-Datei-URL")

    *Kopieren von Blob-URL*
15. Öffnen Sie Internet Explorer, und fügen Sie die URL. Die folgende Abbildung, die im Browser angezeigt werden soll.

    ![Logo-big.png-Image aus dem Windows-Blobspeicher](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "-Logo-big.png Bilds aus dem Speicher")

    *Logo-big.png-Image aus Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Aufgabe 3: die Aktualisierung der Lösung zum Nutzen von statischen Inhalten aus Azure Blob Storage

In dieser Aufgabe Konfigurieren Sie die **GeekQuiz** Lösung nutzen Sie das Image hochgeladen in Azure Blob Storage (anstelle der Abbildung befindet sich in der Web-app) durch das Hinzufügen einer ASP.NET URL-Rewrite-Regel in der **"Web.config"** Datei.

1. Öffnen Sie in Visual Studio die **"Web.config"** Datei innerhalb der **GeekQuiz** Projekt, und suchen Sie die **&lt;"System.Webserver"&gt;** Element.
2. Fügen Sie den folgenden Code zum Hinzufügen eine URL rewrite-Regel, aktualisieren den Platzhalter mit den Namen Ihres Speicherkontos.

    (Codeausschnitt - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL-Umschreibung, ist der Prozess, der eine eingehende webanforderung abfangen und Umleiten der Anforderung an eine andere Ressource. Die URL-Umschreibung Regeln weist die Adressumschreibungs-Engine, wenn eine Anforderung umgeleitet werden soll, und, in dem sie umgeleitet werden soll. Adressumschreibungs Regel besteht aus zwei Zeichenfolgen: das Muster, in der angeforderten URL suchen Sie nach (in der Regel mithilfe von regulären Ausdrücken), und finden Sie die Zeichenfolge, die das Muster, wenn ersetzen. Weitere Informationen finden Sie unter [URL-Umschreibung in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Drücken Sie **STRG + S** um die Änderungen zu speichern.
4. Öffnen Sie ein neues **Git Bash** Verwaltungskonsole, um die aktualisierte Anwendung in Azure App Service bereitstellen.
5. Führen Sie die folgenden Befehle aus, um die Änderungen an Azure zu übertragen. Update der *[YOUR-APPLICATION-PATH]* Platzhalter durch den Pfad zu der **GeekQuiz** Lösung. Sie werden aufgefordert, Ihr bereitstellungskennwort.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Bereitstellen von Updates in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Bereitstellen von Updates in Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Aufgabe 4: Überprüfung

In dieser Aufgabe verwenden Sie **Internet Explorer** zum Durchsuchen der **Meister Quiz** Anwendung, und überprüfen Sie, dass die URL-rewrite-Regel für Bilder funktioniert, und Sie werden umgeleitet, auf das Bild auf gehosteten **Azure-Blob Storage**.

1. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-app (z. B. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmeldeinformationen.

    ![Zeigt die Meister Quiz Web-app mit dem Image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "mit Anzeige der Meister Quiz Web-app mit dem Image")

    *Zeigt die Meister Quiz Web-app mit dem image*
2. Drücken Sie **F12** um die Entwicklungstools zu starten, wählen die **Netzwerk** Registerkarte, und starten Sie die Aufzeichnung.

    ![Starten die Netzwerk-Aufzeichnung](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Netzwerk Aufzeichnung starten")

    *Netzwerk-Aufzeichnung starten*
3. Drücken Sie **STRG + F5** zum Aktualisieren der Webseite.
4. Sobald die Seite geladen wurde, sollte eine HTTP-Anforderung für die **/img/logo-big.png** URL mit einem HTTP **301** Ergebnis (Umleitung) und eine andere Anforderung für `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL mit dem HTTP- **200** Ergebnis.

    ![Überprüfen die URL-Umleitung](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "angezeigt, die mit den umleitungs-Entwickler-Tools")

    *Überprüfen die URL-Umleitung*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Schritt 5: Verwenden für die automatische Skalierung für Web-Apps

> [!NOTE]
> In dieser Übung ist optional, da sie Unterstützung für Web-Last erfordert &amp; Leistungstests, die nur zur Verfügung steht **Visual Studio 2013 Ultimate Edition**. Weitere Informationen zu bestimmten Features von Visual Studio 2013, Versionsvergleich [hier](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service-Web-Apps** bietet die Autoscale-Funktion für die Web-apps, die im **Modus "Standard"**. Für die automatische Skalierung ermöglicht Azure die Anzahl der Instanzen der Web-app je nach Auslastung automatisch skaliert. Wenn für die automatische Skalierung aktiviert ist, wird Azure überprüft, einmal alle fünf Minuten die CPU der Web-app und Instanzen nach Bedarf zu diesem Zeitpunkt hinzugefügt. Wenn die CPU-Auslastung gering ist, wird Azure Instanzen einmal alle zwei Stunden entfernt, um sicherzustellen, dass die Leistung der Web-app nicht beeinträchtigt wird.

Wechseln Sie in dieser Übung über die erforderlichen Schritte zum Konfigurieren der **für die automatische Skalierung** feature für die **Meister Quiz** Web-app. Diese Funktion überprüft durch Ausführen eines Auslastungstests von Visual Studio zum Generieren von genügend CPU-Auslastung der Anwendung ein instanzupgrade ausgelöst.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Aufgabe 1 – Konfigurieren von für die automatische Skalierung basierend auf der CPU-Metrik

In dieser Aufgabe verwenden Sie das Azure-Verwaltungsportal, um die Autoscale-Funktion für die Web-app zu aktivieren, die Sie in der Übung 2 erstellt haben.

1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com/)Option **Websites** , und klicken Sie auf die Web-app, die Sie in der Übung 2 erstellt haben.
2. Navigieren Sie zu der **Skalierung** Seite. Unter den **Kapazität** wählen Sie im Abschnitt **CPU** für die **nach Metrik skalieren** Konfiguration.

    > [!NOTE]
    > Bei der Skalierung von CPU, passt Azure dynamisch die Anzahl der Instanzen, die die app verwendet wird, wenn die CPU-Auslastung ändert.

    ![Auswählen, dass die Skalierung nach CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "die CPU-Metrik für die automatische Skalierung auswählen")

    *Auswählen, dass die Skalierung nach CPU*
3. Ändern der **Ziel-CPU** Konfiguration **20**-**40** Prozent.

    > [!NOTE]
    > Dieser Bereich stellt die durchschnittliche CPU-Auslastung für Ihre Web-app dar. Azure wird hinzufügen oder Entfernen von Instanzen, um Ihre Web-app in diesem Bereich bleibt. Die minimale und maximale Anzahl der Instanzen, die für die Skalierung wird angegeben, der **Instanzanzahl** Konfiguration. Azure geht nie über oder über diesen Grenzwert hinaus.
    >
    > Der Standardwert **Ziel-CPU** Werte werden nur für die Zwecke dieser testumgebung geändert. Konfigurieren Sie die CPU-Bereich mit kleinen Werten, erhöhen Sie die Wahrscheinlichkeit, dass der Trigger für die automatische Skalierung, wenn eine mittlere Auslastung der Anwendung befindet.

    ![Ändern des Ziels-CPU, der zwischen 20 und 40 Prozent liegt](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Ändern des Ziels-CPU, um zwischen 20 und 40 Prozent liegen.")

    *Ändern die Ziel-CPU um 20 bis 40 Prozent*
4. Klicken Sie auf **speichern** in der Befehlsleiste, um die Änderungen zu speichern.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Aufgabe 2: Auslastungstests mit Visual Studio

Nachdem **für die automatische Skalierung** wurde konfiguriert haben, erstellen Sie eine **Webleistungs- und Test-Projekt laden** in Visual Studio generiert eine CPU-Last für Ihre Web-app.

1. Open **Visual Studio Ultimate 2013** , und wählen Sie **Datei | Neue | Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **Webleistungs- und Auslastungstestprojekt** unter der **Visual c# | Test** Registerkarte. Stellen Sie sicher, dass **.NET Framework 4.5** wird ausgewählt, nennen Sie das Projekt *WebAndLoadTestProject*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Erstellen eines neuen Projekts für Web- und Auslastungstests](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Erstellen eines neuen Projekts für Web- und Auslastungstests")

    *Erstellen eines neuen Projekts für Web- und Auslastungstests*
3. In der **Namen "WebTest1.webtest"** mit der rechten Maustaste die **WebTest1** Knoten, und klicken Sie auf **hinzufügen anfordern**.

    ![Hinzufügen einer Anforderung WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 eine Anforderung hinzugefügt")

    *Hinzufügen einer Anforderung WebTest1*
4. In der **Eigenschaften** Fenster für den neuen Anforderungsknoten aktualisiert den **Url** Eigenschaft, um auf die URL der Web-app zu verweisen (z. B. *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Ändern die Url-Eigenschaft](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Ändern der Url-Eigenschaft")

    *Ändern die Url-Eigenschaft*
5. In der **Namen "WebTest1.webtest"** Fenster mit der rechten Maustaste **WebTest1** , und klicken Sie auf **Schleife hinzufügen...** .

    ![Hinzufügen einer Schleife zu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Hinzufügen einer Schleife zu WebTest1")

    *Hinzufügen einer Schleife zu WebTest1*
6. In der **Bedingungsregel und Elemente hinzufügen zu Schleife** wählen Sie im Dialogfeld die **For-Schleife** Regel, und ändern Sie die folgenden Eigenschaften.

   1. **Beenden Sie Wert:** 1000
   2. **Kontextparametername:** Iterator
   3. **Der Inkrementwert:** 1

      ![Die Regel für Schleife auswählen und Aktualisieren der Eigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Sie die Regel für Schleife auswählen und Aktualisieren der Eigenschaften")

      *Die Regel für Schleife auswählen und Aktualisieren der Eigenschaften*
7. Unter den **Elemente in Schleife** Abschnitt, wählen Sie die Anforderung, die Sie zuvor erstellt haben als das erste und letzte Element, für die Schleife. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Auswahl der ersten und letzten Elemente für die Schleife](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "der ersten und letzten Elemente für die Schleife auswählen")

    *Die ersten und letzten Elemente für die Schleife auswählen*
8. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WebAndLoadTestProject** projizieren, erweitern Sie die **hinzufügen** Menü **Auslastungstest...** .

    ![Hinzufügen eines Auslastungstests zum Projekt WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "eines Auslastungstests zum Projekt WebAndLoadTestProject hinzugefügt")

    *Hinzufügen eines Auslastungstests zum WebAndLoadTestProject-Projekt*
9. In der **Assistent für neuen Auslastungstest** Dialogfeld klicken Sie auf **Weiter**.

    ![Assistent für neuen Auslastungstest](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Assistent für neuen Auslastungstest")

    *Assistent für neuen Auslastungstest*
10. In der **Szenario** Seite **verwenden Sie keine Reaktionszeiten** , und klicken Sie auf **Weiter**.

    ![Auswahl nicht mit Reaktionszeiten](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "auswählen, nicht zu Reaktionszeit verwenden")

    *Auswählen, dass keine Reaktionszeit verwenden*
11. In der **Auslastungsmuster** Seite, stellen Sie sicher, dass die **Konstante Auslastung** ausgewählt ist. Ändern der **Benutzeranzahl** auf **250** -Benutzer und klicken Sie auf **Weiter**.

    ![Ändern die Benutzeranzahl auf 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "ändern die Benutzeranzahl auf 250")

    *Ändern die Benutzeranzahl auf 250*
12. In der **Testmischungsmodell** Seite **basierend auf sequenzieller Testreihenfolge** , und klicken Sie auf **Weiter**.

    ![Wählen das Testmischungsmodell](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "das Testmischungsmodell auswählen")

    *Wählen das Testmischungsmodell*
13. In der **Testmischungsmodell** auf **hinzufügen...**  auf einen Test der Mischung hinzufügen.

    ![Hinzufügen eines Tests zu testmischung](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Hinzufügen eines Tests zu testmischung")

    *Hinzufügen eines Tests zu testmischung*
14. In der **Tests hinzufügen** Dialogfeld Doppelklicken Sie auf **WebTest1** des Tests hinzufügen der **ausgewählte Tests** Liste. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Hinzufügen des Tests WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1 Test hinzufügen")

    *Die WebTest1 Test hinzufügen*
15. In der **Testmischung** auf **Weiter**.

    ![Abschließen der Seite "Testmischung"](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Abschließen der Seite \"Testmischung\"")

    *Abschließen der Seite "Testmischung"*
16. In der **Netzwerkmischung** auf **Weiter**.

    ![Klicken Sie anschließend auf der Seite für die Netzwerkmischung](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "klicken Sie anschließend auf der Seite für die Netzwerkmischung")

    *Klicken Sie auf Weiter, auf der Seite der Netzwerkmischung*
17. In der **Browsermischung** Seite **Internet Explorer 10.0** als Browsertyp, und klicken Sie auf **Weiter**.

    ![Auswählen des Browsertyps](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Browser auswählen")

    *Auswählen des Browsers*
18. In der **Indikatorensätze** auf **Weiter**.

    ![Klicken auf der Seite für die Indikatorensätze Weiter](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Weiter klicken, auf der Seite für die Indikatorensätze")

    *Klicken auf der Seite für die Indikatorensätze weiter*
19. In der **Laufzeiteinstellungen** Seite die **Dauer des Auslastungstests** zu **5 Minuten** , und klicken Sie auf **Fertig stellen**.

    ![Festlegen der Dauer des Auslastungstests auf 5 Minuten](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Festlegen der Dauer des Auslastungstests auf 5 Minuten")

    *Festlegen der Dauer des Auslastungstests auf 5 Minuten*
20. In **Projektmappen-Explorer**, doppelklicken Sie auf die **Local.settings** Datei, um die testeinstellungen zu untersuchen. Standardmäßig verwendet Visual Studio den lokalen Computer zum Ausführen der Tests an.

    > [!NOTE]
    > Alternativ können Sie das Testprojekt zum Ausführen der Auslastungstests in der Cloud mit konfigurieren **Azure Testplänen**. Azure Testpläne bietet eine cloudbasierte Auslastungstests, Tests der Dienst, der eine realistischere Auslastung simuliert, das Vermeiden von Einschränkungen der lokalen Umgebung wie CPU-Kapazität, verfügbarer Speicher und Netzwerkbandbreite. Weitere Informationen zur Verwendung von Testplänen für Azure zum Ausführen von Auslastungstests finden Sie unter [Testszenarios laden](/azure/devops/test/load-test/overview?view=vsts).

    ![Testeinstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Aufgabe 3: Überprüfung der automatischen Skalierung

Sie werden nun Ausführen des Auslastungstests, die Sie in der vorherigen Aufgabe erstellt haben, wie Ihre Web-app unter Last verhält.

1. In **Projektmappen-Explorer**, doppelklicken Sie auf **"LoadTest1.LoadTest"** Auslastungstest zu öffnen.

    ![Öffnen "LoadTest1.LoadTest"](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "\"LoadTest1.LoadTest\" Öffnen")

    *Die "LoadTest1.LoadTest" Öffnen*
2. In der **"LoadTest1.LoadTest"** Fenster, klicken Sie auf die erste Schaltfläche in der Toolbox auf die Ausführung des Auslastungstests.

    ![Ausführen des Auslastungstests](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Ausführen des Auslastungstests")

    *Ausführen des Auslastungstests*
3. Warten Sie, bis der Auslastungstest abgeschlossen ist.

    > [!NOTE]
    > Der Auslastungstest simuliert mehrere Benutzer, die gleichzeitig Anforderungen an die Web-app senden. Wenn der Test ausgeführt wird, können Sie die verfügbaren Indikatoren zum Erkennen von Fehlern, Warnungen oder anderen Informationen, die im Zusammenhang mit Ihren Auslastungstestlauf überwachen.

    ![Auslastungstest ausführen](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "warten, bis der Auslastungstest abgeschlossen ist")

    *Auslastungstest*
4. Wenn der Test abgeschlossen ist, wechseln Sie zurück zum Verwaltungsportal, und navigieren Sie zu der **Skalierung** auf der Seite Ihrer Web-app. Unter den **Kapazität** Abschnitt sollte im Diagramm, dass automatisch eine neue Instanz bereitgestellt wurde.

    ![Neue Instanz, die automatisch bereitgestellt.](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Neue Instanz, die automatisch bereitgestellt.*

    > [!NOTE]
    > Es dauert einige Minuten, bis die Änderungen im Diagramm angezeigt werden (drücken Sie die **STRG + F5** in regelmäßigen Abständen, um die Seite zu aktualisieren). Wenn Sie die Änderungen nicht angezeigt werden, können Sie Folgendes versuchen:
    >
    > - Erhöhen Sie die Dauer des Auslastungstests (z. B. zu **10 Minuten**)
    > - Reduzieren Sie die maximalen und minimalen Werte, der die **Ziel-CPU** Bereich in der Konfiguration der automatischen Skalierung Ihrer Web-App
    > - Ausführen des Auslastungstests in der Cloud mit **Azure Testplänen**. Weitere Informationen [hier](/azure/devops/test/load-test/index?view=vsts)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie gelernt, wie Sie zum Einrichten und Bereitstellen der Anwendung auf Produktions-Web-apps in Azure. Sie beginnen, indem Sie erkennen, und aktualisieren Ihre Datenbanken mithilfe von **Entity Framework Code First-Migrationen**, setzte sich durch das Bereitstellen neuer Versionen des Standorts mit **Git** und Ausführen von Rollbacks zu den neueste stabile Version Ihres Standorts. Darüber hinaus haben Sie Ihre app mit Speicher auf den statischen Inhalt in einen Blob-Container verschieben zu skalieren.
