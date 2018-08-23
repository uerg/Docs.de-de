---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 5c9b0c71e2e0d35645e975e9adb7086e65bcf4c3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835588"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="97887-103">ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="97887-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="97887-104">durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="97887-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="97887-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="97887-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="97887-106">Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="97887-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="97887-107">Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="97887-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="97887-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="97887-108">Overview</span></span>

<span data-ttu-id="97887-109">In diesem Tutorial stellen Sie eine Datenbank und zugehörigen Code-Änderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update für die Test-, Staging- und produktionsumgebungen Umgebungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="97887-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="97887-110">Das Tutorial zeigt zunächst, wie eine Datenbank zu aktualisieren, die von Code First-Migrationen verwaltet wird, und klicken Sie dann später es zeigt, wie zum Aktualisieren einer Datenbank mit dem DbDacFx-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="97887-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="97887-111">Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="97887-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="97887-112">Bereitstellen eines Datenbankupdates mit Code First-Migrationen</span><span class="sxs-lookup"><span data-stu-id="97887-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="97887-113">In diesem Abschnitt fügen Sie eine Birth Date-Spalte, die `Person` Basisklasse für die `Student` und `Instructor` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="97887-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="97887-114">Klicken Sie dann aktualisieren Sie die Seite mit Daten für "Instructor", damit die neue Spalte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="97887-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="97887-115">Abschließend stellen Sie die Änderungen für Test-, Staging- und produktionsumgebungen bereit.</span><span class="sxs-lookup"><span data-stu-id="97887-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="97887-116">Hinzufügen einer Spalte einer Tabelle in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="97887-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="97887-117">In der *ContosoUniversity.DAL* -Projekt, öffnen *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es muss zwei schließende geschweifte Klammern, die darauf folgende):</span><span class="sxs-lookup"><span data-stu-id="97887-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="97887-118">Aktualisieren Sie als Nächstes die `Seed` Methode, sodass die It einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="97887-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="97887-119">Open *migrations\configuration. cs* , und Ersetzen Sie den Codeblock, der beginnt `var instructors = new List<Instructor>` mit den folgenden Codeblock, der Geburtsdatum enthält:</span><span class="sxs-lookup"><span data-stu-id="97887-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="97887-120">Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster.</span><span class="sxs-lookup"><span data-stu-id="97887-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="97887-121">Sicherstellen, dass ContosoUniversity.DAL noch, als ausgewählt ist die **Standardprojekt**.</span><span class="sxs-lookup"><span data-stu-id="97887-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="97887-122">In der **-Paket-Manager-Konsole** wählen Sie im Fenster **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="97887-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="97887-123">Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="97887-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="97887-124">Die `Up` Methode erstellt die Spalte aus, wenn Sie die Änderung zu implementieren und die `Down` -Methode löscht die Spalte aus, wenn die Änderung zurückgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="97887-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="97887-126">Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **-Paket-Manager-Konsole** Fenster (Stellen Sie sicher, dass das Projekt ContosoUniversity.DAL immer noch ausgewählt ist):</span><span class="sxs-lookup"><span data-stu-id="97887-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="97887-127">Entity Framework führt die `Up` -Methode, und klicken Sie dann führt die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="97887-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="97887-128">Die neue Spalte in die dozentenseite anzuzeigen</span><span class="sxs-lookup"><span data-stu-id="97887-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="97887-129">Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlagenfelds zum Anzeigen des Geburtsdatums.</span><span class="sxs-lookup"><span data-stu-id="97887-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="97887-130">Fügen Sie sie zwischen den Vorgängen für Datum und die Office-Zuweisung Hire hinzu:</span><span class="sxs-lookup"><span data-stu-id="97887-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="97887-131">(Wenn der codeeinzug nicht synchronisiert wird, können Sie STRG + K und dann STRG + D, um die Datei automatisch neu formatieren drücken.)</span><span class="sxs-lookup"><span data-stu-id="97887-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="97887-132">Führen Sie die Anwendung, und klicken Sie auf die **Dozenten** Link.</span><span class="sxs-lookup"><span data-stu-id="97887-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="97887-133">Beim Laden der Seite angezeigt, dass sie die neue hat Birth Date-Felds.</span><span class="sxs-lookup"><span data-stu-id="97887-133">When the page loads, you see that it has the new birth date field.</span></span>

    !["Dozenten", mit "BirthDate"](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="97887-135">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="97887-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="97887-136">Bereitstellen des Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="97887-136">Deploy the database update</span></span>

1. <span data-ttu-id="97887-137">In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="97887-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="97887-138">In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.</span><span class="sxs-lookup"><span data-stu-id="97887-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="97887-139">(Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt "ContosoUniversity" im **Projektmappen-Explorer**.)</span><span class="sxs-lookup"><span data-stu-id="97887-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="97887-140">Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser zur Startseite geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="97887-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="97887-141">Führen Sie die **Dozenten** Seite, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="97887-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="97887-142">Wenn die Anwendung versucht, die Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Schema der Datenbank und führt die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="97887-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="97887-143">Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Birth Date** Spalte mit Datumsangaben in es.</span><span class="sxs-lookup"><span data-stu-id="97887-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="97887-144">In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Staging** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.</span><span class="sxs-lookup"><span data-stu-id="97887-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="97887-145">Führen Sie die **Dozenten** Seite in der Stagingumgebung, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="97887-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="97887-146">In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste klicken Sie auf die **Produktion** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.</span><span class="sxs-lookup"><span data-stu-id="97887-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="97887-147">Führen Sie die **Dozenten** Seite in der produktionsumgebung, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="97887-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="97887-148">Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank umfasst Sie Unternehmen würden, in der Regel auch die Anwendung offline schalten, während der Bereitstellung mithilfe von *app\_offline.htm*, wie im vorherigen Tutorial haben Sie gesehen.</span><span class="sxs-lookup"><span data-stu-id="97887-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="97887-149">Bereitstellen eines Datenbankupdates mit dem DbDacFx-Anbieter</span><span class="sxs-lookup"><span data-stu-id="97887-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="97887-150">In diesem Abschnitt fügen Sie eine *Kommentare* Spalte, um die *Benutzer* Tabelle, die in der Mitgliedschaftsdatenbank, und erstellen Sie eine Seite, mit dem Sie anzeigen und Bearbeiten von Kommentaren für jeden Benutzer.</span><span class="sxs-lookup"><span data-stu-id="97887-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="97887-151">Klicken Sie dann stellen Sie die Änderungen für Test-, Staging- und produktionsumgebungen bereit.</span><span class="sxs-lookup"><span data-stu-id="97887-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="97887-152">Hinzufügen einer Spalte einer Tabelle in der Mitgliedschaftsdatenbank</span><span class="sxs-lookup"><span data-stu-id="97887-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="97887-153">Öffnen Sie in Visual Studio **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="97887-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="97887-154">Erweitern Sie **(Localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **Aspnet-ContosoUniversity** (nicht **Aspnet-ContosoUniversity-Prod**) und schließlich **Tabellen**.</span><span class="sxs-lookup"><span data-stu-id="97887-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="97887-155">Wenn Sie nicht sehen **(Localdb) \v11.0** unter der **SQL Server** Knoten mit der rechten Maustaste die **SQL Server** Knoten, und klicken Sie auf **SQL Server hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="97887-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="97887-156">In der **Herstellen einer Verbindung mit Server** Geben Sie im Dialogfeld *(Localdb) \v11.0* als die **Servernamen**, und klicken Sie dann auf **Connect**.</span><span class="sxs-lookup"><span data-stu-id="97887-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="97887-157">Wenn Sie nicht sehen **Aspnet-ContosoUniversity**, führen Sie das Projekt, und melden Sie sich mit der *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*), und aktualisieren Sie dann die  **Objekt-Explorer von SQL Server** Fenster.</span><span class="sxs-lookup"><span data-stu-id="97887-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="97887-158">Mit der rechten Maustaste die **Benutzer** Tabelle, und klicken Sie dann auf **Ansicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="97887-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX-Ansicht-Designer](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="97887-160">Fügen Sie im Designer ein *Kommentare* Spalte und legen Sie ihn *vom Datentyp nvarchar(128)* und NULL-Werte zulässt, und klicken Sie dann auf **Update**.</span><span class="sxs-lookup"><span data-stu-id="97887-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Hinzufügen von Kommentaren-Spalte](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="97887-162">In der **Vorschau der Datenbankupdates** auf **Update Database**.</span><span class="sxs-lookup"><span data-stu-id="97887-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Vorschau der Datenbankupdates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="97887-164">Erstellen Sie eine Seite zum Anzeigen und bearbeiten die neue Spalte</span><span class="sxs-lookup"><span data-stu-id="97887-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="97887-165">In **Projektmappen-Explorer**, mit der rechten Maustaste die **Konto** Ordner im Projekt ContosoUniversity klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element** .</span><span class="sxs-lookup"><span data-stu-id="97887-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="97887-166">Erstellen Sie ein neues **Web Form-Masterseite mithilfe von** und nennen Sie sie *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="97887-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="97887-167">Übernehmen Sie den Standardnamen *Site.Master* Datei als Masterseite.</span><span class="sxs-lookup"><span data-stu-id="97887-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="97887-168">Kopieren Sie das folgende Markup in der `MainContent` `Content` Element (das letzte 3 `Content` Elemente):</span><span class="sxs-lookup"><span data-stu-id="97887-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="97887-169">Mit der rechten Maustaste die *UserInfo.aspx* Seite, und klicken Sie auf **in Browser anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="97887-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="97887-170">Melden Sie sich mit Ihrem *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*) und fügen Sie einige Kommentare zu einem Benutzer, um sicherzustellen, dass die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="97887-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="97887-172">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="97887-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="97887-173">Bereitstellen des Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="97887-173">Deploy the database update</span></span>

<span data-ttu-id="97887-174">Um mithilfe des DbDacFx-Anbieters bereitstellen, müssen Sie nur auswählen der **Aktualisieren einer Datenbank** Option im Veröffentlichungsprofil.</span><span class="sxs-lookup"><span data-stu-id="97887-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="97887-175">Allerdings für die anfängliche Bereitstellung bei der Verwendung dieser Option Sie auch konfiguriert einige zusätzliche SQL­Skripts ausgeführt: sind noch in der das Profil und Sie müssen verhindern, dass sie erneut ausführen.</span><span class="sxs-lookup"><span data-stu-id="97887-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="97887-176">Öffnen der **Webveröffentlichung** Assistenten, indem Sie mit der rechten Maustaste in des Projekts ContosoUniversity und auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="97887-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="97887-177">Wählen Sie die **Test** Profil.</span><span class="sxs-lookup"><span data-stu-id="97887-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="97887-178">Klicken Sie auf die **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="97887-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="97887-179">Klicken Sie unter **DefaultConnection**Option **Aktualisieren einer Datenbank**.</span><span class="sxs-lookup"><span data-stu-id="97887-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="97887-180">Deaktivieren Sie die zusätzlichen Skripts, die Sie konfiguriert haben, führen Sie für die anfängliche Bereitstellung:</span><span class="sxs-lookup"><span data-stu-id="97887-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="97887-181">Klicken Sie auf **Datenbankupdates konfigurieren**.</span><span class="sxs-lookup"><span data-stu-id="97887-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="97887-182">In der **Datenbankupdates konfigurieren** (Dialogfeld), deaktivieren Sie die Kontrollkästchen neben *Grant.sql* und *Aspnet-Daten-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="97887-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="97887-183">Klicken Sie auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="97887-183">Click **Close**.</span></span>
6. <span data-ttu-id="97887-184">Klicken Sie auf die **Vorschau** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="97887-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="97887-185">Klicken Sie unter **Datenbanken** vor und nach der **DefaultConnection**, klicken Sie auf die **datenbankvorschau** Link.</span><span class="sxs-lookup"><span data-stu-id="97887-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Database (Vorschau)](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="97887-187">Im Vorschaufenster wird das Skript, das in der Zieldatenbank, mit dem Schema der Quelldatenbank überein Datenbankschema vornehmen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="97887-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="97887-188">Das Skript enthält einen ALTER TABLE-Befehl, der die neue Spalte hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="97887-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="97887-189">Schließen der **Datenbankvorschau** (Dialogfeld), und klicken Sie dann auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="97887-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="97887-190">Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser zur Startseite geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="97887-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="97887-191">Führen Sie die Seite "Benutzerinformationen" (hinzufügen *Account/UserInfo.aspx* an die URL der Startseite) zu überprüfen, ob das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="97887-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="97887-192">Sie müssen für die Anmeldung durch Eingabe *Admin* und *Devpwd*.</span><span class="sxs-lookup"><span data-stu-id="97887-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="97887-193">Daten in Tabellen werden nicht standardmäßig bereitgestellt, und ein datenbereitstellungsskripts ausgeführt wird, können Sie nicht konfigurieren, damit Sie den Kommentar finden, den Sie in der Entwicklung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="97887-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="97887-194">Sie können einen neuen Kommentar jetzt hinzufügen, in der Stagingumgebung, um sicherzustellen, dass die Änderung in der Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="97887-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="97887-195">Führen Sie das gleiche Verfahren für die Bereitstellung in Staging- und produktionsumgebungen.</span><span class="sxs-lookup"><span data-stu-id="97887-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="97887-196">Vergessen Sie nicht die zusätzlichen Skripts deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="97887-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="97887-197">Der einzige Unterschied im Vergleich zu Testprofils besteht, dass Sie nur ein Skript in der Stagingtabelle deaktiviert, und Produktion, da sie konfiguriert wurden, führen Sie nur ein Profil *Aspnet-Prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="97887-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="97887-198">Die Anmeldeinformationen für das Staging und Produktion sind Administrator- und Prodpwd.</span><span class="sxs-lookup"><span data-stu-id="97887-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="97887-199">Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank umfasst Sie Unternehmen würden, in der Regel auch die Anwendung offline schalten, während der Bereitstellung durch Hochladen *app\_offline.htm* vor der Veröffentlichung und löschen danach, wie in [das vorherige Tutorial](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="97887-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="97887-200">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="97887-200">Summary</span></span>

<span data-ttu-id="97887-201">Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank mit Code First-Migrationen und der DbDacFx-Anbieter enthalten.</span><span class="sxs-lookup"><span data-stu-id="97887-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

!["Dozenten", mit "BirthDate"](deploying-a-database-update/_static/image8.png)

![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="97887-204">Im nächste Tutorial erfahren Sie, wie Sie Bereitstellungen über die Befehlszeile ausführen.</span><span class="sxs-lookup"><span data-stu-id="97887-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97887-205">[Zurück](deploying-a-code-update.md)
> [Weiter](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="97887-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
