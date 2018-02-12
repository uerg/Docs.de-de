---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen einer Datenbank-Update | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen eines Datenbankupdates
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm stellen Sie eine Datenbank und zugehörigem codeänderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update für den Test, Staging und Produktion Umgebungen bereitstellen.

Das Lernprogramm zeigt zunächst, wie eine Datenbank zu aktualisieren, die von Code First-Migrationen verwaltet wird, und klicken Sie dann später es zeigt, wie eine Datenbank durch Aktualisieren des DbDacFx-Anbieters.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Bereitstellen eines Datenbankupdates mithilfe von Code First-Migrationen

In diesem Abschnitt fügen Sie eine Birth Date-Spalte zu den `Person` Basisklasse für die `Student` und `Instructor` Entitäten. Klicken Sie dann aktualisieren Sie die Seite, die Instructor-Daten zeigt an, dass die neue Spalte angezeigt. Schließlich können Sie die Änderungen auf Test-, Staging- und produktionsumgebung bereitstellen.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Hinzufügen einer Spalte zu einer Tabelle in der Anwendungsdatenbank

1. In der *ContosoUniversity.DAL* geöffneten Projekts *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es darf zwei schließende geschweifte Klammern, die darauf folgende):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Aktualisieren Sie als Nächstes die `Seed` Methode so, dass sie einen Wert für die neue Spalte bereitstellt. Open *Migrations\Configuration.cs* , und Ersetzen Sie den Codeblock, der beginnt, `var instructors = new List<Instructor>` mit den folgenden Codeblock das Geburtsdatum enthält:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster. Sicherstellen, dass als ContosoUniversity.DAL ausgewählter der **Standardprojekt**.
3. In der **Package Manager Console** wählen **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt. Die `Up` Methode erstellt die Spalte aus, wenn Sie die Änderung zu implementieren und die `Down` Methode löscht die Spalte aus, wenn Sie die Änderung zurückgesetzt werden.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **Package Manager Console** Fenster (Stellen Sie sicher, dass das ContosoUniversity.DAL-Projekt ausgewählt ist):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Führt das Entity Framework die `Up` -Methode, und klicken Sie dann führt die `Seed` Methode.

### <a name="display-the-new-column-in-the-instructors-page"></a>Zeigen Sie die neue Spalte in der Seite "Lehrkräfte"

1. Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlage-Felds, um das Geburtsdatum anzuzeigen. Fügen Sie es zwischen den Vorgängen für Hire Date und Office-Zuordnung hinzu:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Wenn Code Einzug synchron abgerufen werden, können Sie STRG-K und dann STRG + D, um die Datei automatisch neu zu formatieren drücken.)
2. Führen Sie die Anwendung, und klicken Sie auf die **Lehrkräfte** Link.

    Beim Laden der Seite angezeigt, dass die neue verfügt über Birth Date-Felds.

    ![Seite "Lehrkräfte" mit "BirthDate"](deploying-a-database-update/_static/image2.png)
3. Schließen Sie den Browser.

### <a name="deploy-the-database-update"></a>Das Datenbankupdate bereitstellen

1. In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.
2. In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt ContosoUniversity **Projektmappen-Explorer**.)

    Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser geöffnet wird, um zur Startseite.
3. Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

    Wenn die Anwendung versucht, den Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Datenbankschema und führt die `Seed` Methode. Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Geburtsdatum** Spalte mit Datumsangaben darin.
4. In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Staging** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.
5. Führen Sie die **Lehrkräfte** Seite in der Stagingumgebung, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.
6. In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Produktion** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.
7. Führen Sie die **Lehrkräfte** Seite in der Produktion einsetzen, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

    Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank enthält nehmen Sie in der Regel auch die Anwendung offline, während der Bereitstellung mithilfe *app\_offline.htm*, wie Sie im vorherigen Lernprogramm gesehen haben.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Bereitstellen einer Datenbankupdate mit dem DbDacFx-Anbieter

In diesem Abschnitt fügen Sie eine *Kommentare* Spalte, um die *Benutzer* -Tabelle in der Mitgliedschaftsdatenbank und erstellen Sie eine Seite, mit dem Sie die Anzeige und Bearbeitung von Kommentare für jeden Benutzer. Anschließend können Sie die Änderungen auf Test-, Staging- und produktionsumgebung bereitstellen.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Hinzufügen einer Spalte zu einer Tabelle in der Mitgliedschaftsdatenbank

1. Öffnen Sie in Visual Studio **Objekt-Explorer von SQL Server**.
2. Erweitern Sie **(Localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **Aspnet-ContosoUniversity** (nicht **Aspnet-ContosoUniversity-Prod**) und schließlich **Tabellen**.

    Wenn Sie nicht sehen **(Localdb) \v11.0** unter der **SQL Server** Knoten mit der rechten Maustaste die **SQL Server** Knoten, und klicken Sie auf **SQL-Server hinzufügen**. In der **Verbindung mit Server herstellen** Geben Sie im Dialogfeld *(Localdb) \v11.0* als die **Servernamen**, und klicken Sie dann auf **verbinden**.

    Wenn Sie nicht sehen **Aspnet-ContosoUniversity**, führen Sie das Projekt, und melden Sie sich mit den *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*), und aktualisieren Sie dann die  **Objekt-Explorer von SQL Server** Fenster.
3. Mit der rechten Maustaste die **Benutzer** Tabelle, und klicken Sie dann auf **Sicht-Designer**.

    ![SSOX Sicht-Designer](deploying-a-database-update/_static/image3.png)
4. Fügen Sie in den Designer ein *Kommentare* Spalte und *vom Datentyp nvarchar(128)* und NULL-Werte zulässt, und klicken Sie dann auf **Update**.

    ![Hinzufügen von Kommentaren-Spalte](deploying-a-database-update/_static/image4.png)
5. In der **Vorschau der Datenbankupdates** auf **Update Database**.

    ![Vorschau der Datenbankupdates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Erstellen Sie eine Seite zum Anzeigen und bearbeiten die neue Spalte

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Konto** Ordner im ContosoUniversity-Projekt, klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element** .
2. Erstellen Sie ein neues **Web-Formular mithilfe Gestaltungsvorlage** und nennen Sie sie *UserInfo.aspx*. Übernehmen Sie den Standardnamen *Site.Master* Datei als die Gestaltungsvorlage.
3. Kopieren Sie das folgende Markup in der `MainContent` `Content` Element (der letzte von 3 `Content` Elemente):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Mit der rechten Maustaste die *UserInfo.aspx* Seite, und klicken Sie auf **in Browser anzeigen**.
5. Melden Sie sich mit Ihrem *Admin* Benutzeranmeldeinformationen (Kennwort wird *Devpwd*) und einige Kommentare hinzufügen, um einen Benutzer aus, um sicherzustellen, dass die Seite ordnungsgemäß funktioniert.

    ![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image6.png)
6. Schließen Sie den Browser.

## <a name="deploy-the-database-update"></a>Das Datenbankupdate bereitstellen

Um mithilfe des DbDacFx-Anbieters bereitstellen, müssen Sie nur wählen Sie die **Datenbank aktualisieren** Option im Veröffentlichungsprofil. Allerdings bei der erstmaligen Bereitstellung, wenn Sie diese Option verwendet Sie auch konfiguriert einige zusätzliche SQL­Skripts für die Ausführung: sind immer noch im Profil und Sie müssen verhindern, dass erneut auszuführen.

1. Öffnen der **Web veröffentlichen** Assistenten, indem Sie mit der rechten Maustaste des Projekts ContosoUniversity und auf **veröffentlichen**.
2. Wählen Sie die **Test** Profil.
3. Klicken Sie auf die **Einstellungen** Registerkarte.
4. Klicken Sie unter **DefaultConnection**Option **Datenbank aktualisieren**.
5. Deaktivieren Sie die zusätzliche Skripts, die Sie so konfiguriert, dass bei der ersten Bereitstellung ausführen:

    1. Klicken Sie auf **Datenbankupdates konfigurieren**.
    2. In der **Datenbankupdates konfigurieren** (Dialogfeld), deaktivieren Sie die Kontrollkästchen neben *Grant.sql* und *Aspnet-Daten-dev.sql*.
    3. Klicken Sie auf **Schließen**.
6. Klicken Sie auf die **Vorschau** Registerkarte.
7. Klicken Sie unter **Datenbanken** und auf der rechten Seite des **DefaultConnection**, klicken Sie auf die **vorschaudatenbank** Link.

    ![Datenbankvorschau](deploying-a-database-update/_static/image7.png)

    Im Vorschaufenster wird das Skript aus, das in der Zieldatenbank vornehmen, das Schema der Quelldatenbank entsprechen Datenbankschema ausgeführt werden. Das Skript umfasst einen ALTER TABLE-Befehl aus, der die neue Spalte hinzugefügt.
8. Schließen der **Datenbankvorschau** (Dialogfeld), und klicken Sie dann auf **veröffentlichen**.

    Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser geöffnet wird, um zur Startseite.
9. Führen Sie die Seite Benutzerinformationen (hinzufügen *Account/UserInfo.aspx* an die URL der Startseite "), stellen Sie sicher, dass das Update erfolgreich bereitgestellt wurde. Sie müssen hierzu melden Sie sich *Admin* und *Devpwd*.

    Daten in Tabellen werden nicht standardmäßig bereitgestellt, und Sie haben nicht ein Bereitstellungsskript Daten ausgeführt wird, konfigurieren, damit Sie den Kommentar finden, den Sie in der Entwicklung hinzugefügt. Jetzt können Sie einen neuen Kommentar hinzufügen in der Stagingumgebung, um sicherzustellen, dass die Änderung für die Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.
10. Gehen Sie genauso vor, Staging und Produktion bereitstellen.

    Vergessen Sie nicht die zusätzliche Skripts deaktivieren. Der einzige Unterschied im Vergleich zu dem Test-Profil ist, dass Sie nur ein Skript in der Stagingtabelle deaktiviert und Produktion Profile, da sie konfiguriert wurden, führen Sie nur *Aspnet-FA-data.sql*.

    Die Anmeldeinformationen für das Staging und Produktion sind die Administrator- und Prodpwd.

    Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank dauern Sie in der Regel auch die Anwendung offline, während der Bereitstellung durch Hochladen *app\_offline.htm* vor der Veröffentlichung und löschen danach, wie Sie gesehen, im haben [das vorherige Lernprogramm](deploying-a-code-update.md).

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank mithilfe von Code First-Migrationen und der DbDacFx-Anbieter enthalten.

![Seite "Lehrkräfte" mit "BirthDate"](deploying-a-database-update/_static/image8.png)

![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image9.png)

Der nächste Lernprogrammen wird gezeigt, wie Sie Bereitstellungen mithilfe von der Befehlszeile ausführen.

>[!div class="step-by-step"]
[Zurück](deploying-a-code-update.md)
[Weiter](command-line-deployment.md)
