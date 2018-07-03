---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389188"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial stellen Sie eine Datenbank und zugehörigen Code-Änderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update für die Test-, Staging- und produktionsumgebungen Umgebungen bereitstellen.

Das Tutorial zeigt zunächst, wie eine Datenbank zu aktualisieren, die von Code First-Migrationen verwaltet wird, und klicken Sie dann später es zeigt, wie zum Aktualisieren einer Datenbank mit dem DbDacFx-Anbieter.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Bereitstellen eines Datenbankupdates mit Code First-Migrationen

In diesem Abschnitt fügen Sie eine Birth Date-Spalte, die `Person` Basisklasse für die `Student` und `Instructor` Entitäten. Klicken Sie dann aktualisieren Sie die Seite mit Daten für "Instructor", damit die neue Spalte angezeigt. Abschließend stellen Sie die Änderungen für Test-, Staging- und produktionsumgebungen bereit.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Hinzufügen einer Spalte einer Tabelle in der Datenbank

1. In der *ContosoUniversity.DAL* -Projekt, öffnen *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es muss zwei schließende geschweifte Klammern, die darauf folgende):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Aktualisieren Sie als Nächstes die `Seed` Methode, sodass die It einen Wert für die neue Spalte bereitstellt. Open *migrations\configuration. cs* , und Ersetzen Sie den Codeblock, der beginnt `var instructors = new List<Instructor>` mit den folgenden Codeblock, der Geburtsdatum enthält:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster. Sicherstellen, dass ContosoUniversity.DAL noch, als ausgewählt ist die **Standardprojekt**.
3. In der **-Paket-Manager-Konsole** wählen Sie im Fenster **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt. Die `Up` Methode erstellt die Spalte aus, wenn Sie die Änderung zu implementieren und die `Down` -Methode löscht die Spalte aus, wenn die Änderung zurückgesetzt werden.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **-Paket-Manager-Konsole** Fenster (Stellen Sie sicher, dass das Projekt ContosoUniversity.DAL immer noch ausgewählt ist):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework führt die `Up` -Methode, und klicken Sie dann führt die `Seed` Methode.

### <a name="display-the-new-column-in-the-instructors-page"></a>Die neue Spalte in die dozentenseite anzuzeigen

1. Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlagenfelds zum Anzeigen des Geburtsdatums. Fügen Sie sie zwischen den Vorgängen für Datum und die Office-Zuweisung Hire hinzu:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Wenn der codeeinzug nicht synchronisiert wird, können Sie STRG + K und dann STRG + D, um die Datei automatisch neu formatieren drücken.)
2. Führen Sie die Anwendung, und klicken Sie auf die **Dozenten** Link.

    Beim Laden der Seite angezeigt, dass sie die neue hat Birth Date-Felds.

    !["Dozenten", mit "BirthDate"](deploying-a-database-update/_static/image2.png)
3. Schließen Sie den Browser.

### <a name="deploy-the-database-update"></a>Bereitstellen des Datenbankupdates

1. In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.
2. In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt "ContosoUniversity" im **Projektmappen-Explorer**.)

    Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser zur Startseite geöffnet wird.
3. Führen Sie die **Dozenten** Seite, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

    Wenn die Anwendung versucht, die Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Schema der Datenbank und führt die `Seed` Methode. Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Birth Date** Spalte mit Datumsangaben in es.
4. In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Staging** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.
5. Führen Sie die **Dozenten** Seite in der Stagingumgebung, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.
6. In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Produktion** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.
7. Führen Sie die **Dozenten** Seite in der produktionsumgebung, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

    Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank umfasst Sie Unternehmen würden, in der Regel auch die Anwendung offline schalten, während der Bereitstellung mithilfe von *app\_offline.htm*, wie im vorherigen Tutorial haben Sie gesehen.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Bereitstellen eines Datenbankupdates mit dem DbDacFx-Anbieter

In diesem Abschnitt fügen Sie eine *Kommentare* Spalte, um die *Benutzer* Tabelle, die in der Mitgliedschaftsdatenbank, und erstellen Sie eine Seite, mit dem Sie anzeigen und Bearbeiten von Kommentaren für jeden Benutzer. Klicken Sie dann stellen Sie die Änderungen für Test-, Staging- und produktionsumgebungen bereit.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Hinzufügen einer Spalte einer Tabelle in der Mitgliedschaftsdatenbank

1. Öffnen Sie in Visual Studio **Objekt-Explorer von SQL Server**.
2. Erweitern Sie **(Localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **Aspnet-ContosoUniversity** (nicht **Aspnet-ContosoUniversity-Prod**) und schließlich **Tabellen**.

    Wenn Sie nicht sehen **(Localdb) \v11.0** unter der **SQL Server** Knoten mit der rechten Maustaste die **SQL Server** Knoten, und klicken Sie auf **SQL Server hinzufügen**. In der **Herstellen einer Verbindung mit Server** Geben Sie im Dialogfeld *(Localdb) \v11.0* als die **Servernamen**, und klicken Sie dann auf **Connect**.

    Wenn Sie nicht sehen **Aspnet-ContosoUniversity**, führen Sie das Projekt, und melden Sie sich mit der *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*), und aktualisieren Sie dann die  **Objekt-Explorer von SQL Server** Fenster.
3. Mit der rechten Maustaste die **Benutzer** Tabelle, und klicken Sie dann auf **Ansicht-Designer**.

    ![SSOX-Ansicht-Designer](deploying-a-database-update/_static/image3.png)
4. Fügen Sie im Designer ein *Kommentare* Spalte und legen Sie ihn *vom Datentyp nvarchar(128)* und NULL-Werte zulässt, und klicken Sie dann auf **Update**.

    ![Hinzufügen von Kommentaren-Spalte](deploying-a-database-update/_static/image4.png)
5. In der **Vorschau der Datenbankupdates** auf **Update Database**.

    ![Vorschau der Datenbankupdates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Erstellen Sie eine Seite zum Anzeigen und bearbeiten die neue Spalte

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Konto** Ordner im Projekt ContosoUniversity klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element** .
2. Erstellen Sie ein neues **Web Form-Masterseite mithilfe von** und nennen Sie sie *UserInfo.aspx*. Übernehmen Sie den Standardnamen *Site.Master* Datei als Masterseite.
3. Kopieren Sie das folgende Markup in der `MainContent` `Content` Element (das letzte 3 `Content` Elemente):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Mit der rechten Maustaste die *UserInfo.aspx* Seite, und klicken Sie auf **in Browser anzeigen**.
5. Melden Sie sich mit Ihrem *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*) und fügen Sie einige Kommentare zu einem Benutzer, um sicherzustellen, dass die Seite ordnungsgemäß funktioniert.

    ![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image6.png)
6. Schließen Sie den Browser.

## <a name="deploy-the-database-update"></a>Bereitstellen des Datenbankupdates

Um mithilfe des DbDacFx-Anbieters bereitstellen, müssen Sie nur auswählen der **Aktualisieren einer Datenbank** Option im Veröffentlichungsprofil. Allerdings für die anfängliche Bereitstellung bei der Verwendung dieser Option Sie auch konfiguriert einige zusätzliche SQL­Skripts ausgeführt: sind noch in der das Profil und Sie müssen verhindern, dass sie erneut ausführen.

1. Öffnen der **Webveröffentlichung** Assistenten, indem Sie mit der rechten Maustaste in des Projekts ContosoUniversity und auf **veröffentlichen**.
2. Wählen Sie die **Test** Profil.
3. Klicken Sie auf die **Einstellungen** Registerkarte.
4. Klicken Sie unter **DefaultConnection**Option **Aktualisieren einer Datenbank**.
5. Deaktivieren Sie die zusätzlichen Skripts, die Sie konfiguriert haben, führen Sie für die anfängliche Bereitstellung:

    1. Klicken Sie auf **Datenbankupdates konfigurieren**.
    2. In der **Datenbankupdates konfigurieren** (Dialogfeld), deaktivieren Sie die Kontrollkästchen neben *Grant.sql* und *Aspnet-Daten-dev.sql*.
    3. Klicken Sie auf **Schließen**.
6. Klicken Sie auf die **Vorschau** Registerkarte.
7. Klicken Sie unter **Datenbanken** vor und nach der **DefaultConnection**, klicken Sie auf die **datenbankvorschau** Link.

    ![Database (Vorschau)](deploying-a-database-update/_static/image7.png)

    Im Vorschaufenster wird das Skript, das in der Zieldatenbank, mit dem Schema der Quelldatenbank überein Datenbankschema vornehmen ausgeführt wird. Das Skript enthält einen ALTER TABLE-Befehl, der die neue Spalte hinzugefügt.
8. Schließen der **Datenbankvorschau** (Dialogfeld), und klicken Sie dann auf **veröffentlichen**.

    Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser zur Startseite geöffnet wird.
9. Führen Sie die Seite "Benutzerinformationen" (hinzufügen *Account/UserInfo.aspx* an die URL der Startseite) zu überprüfen, ob das Update wurde erfolgreich bereitgestellt wurde. Sie müssen für die Anmeldung durch Eingabe *Admin* und *Devpwd*.

    Daten in Tabellen werden nicht standardmäßig bereitgestellt, und ein datenbereitstellungsskripts ausgeführt wird, können Sie nicht konfigurieren, damit Sie den Kommentar finden, den Sie in der Entwicklung hinzugefügt. Sie können einen neuen Kommentar jetzt hinzufügen, in der Stagingumgebung, um sicherzustellen, dass die Änderung in der Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.
10. Führen Sie das gleiche Verfahren für die Bereitstellung in Staging- und produktionsumgebungen.

    Vergessen Sie nicht die zusätzlichen Skripts deaktivieren. Der einzige Unterschied im Vergleich zu Testprofils besteht, dass Sie nur ein Skript in der Stagingtabelle deaktiviert, und Produktion, da sie konfiguriert wurden, führen Sie nur ein Profil *Aspnet-Prod-data.sql*.

    Die Anmeldeinformationen für das Staging und Produktion sind Administrator- und Prodpwd.

    Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank umfasst Sie Unternehmen würden, in der Regel auch die Anwendung offline schalten, während der Bereitstellung durch Hochladen *app\_offline.htm* vor der Veröffentlichung und löschen danach, wie in [das vorherige Tutorial](deploying-a-code-update.md).

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank mit Code First-Migrationen und der DbDacFx-Anbieter enthalten.

!["Dozenten", mit "BirthDate"](deploying-a-database-update/_static/image8.png)

![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image9.png)

Im nächste Tutorial erfahren Sie, wie Sie Bereitstellungen über die Befehlszeile ausführen.

> [!div class="step-by-step"]
> [Zurück](deploying-a-code-update.md)
> [Weiter](command-line-deployment.md)
