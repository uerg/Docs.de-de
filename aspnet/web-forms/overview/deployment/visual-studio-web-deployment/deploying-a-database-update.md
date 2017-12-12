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
ms.openlocfilehash: 3fd29a5b26c564d88e4128d1904fab00b57a3b7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="15561-103">ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen eines Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="15561-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="15561-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="15561-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="15561-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="15561-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="15561-106">Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="15561-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="15561-107">Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="15561-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="15561-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="15561-108">Overview</span></span>

<span data-ttu-id="15561-109">In diesem Lernprogramm stellen Sie eine Datenbank und zugehörigem codeänderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update für den Test, Staging und Produktion Umgebungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="15561-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="15561-110">Das Lernprogramm zeigt zunächst, wie eine Datenbank zu aktualisieren, die von Code First-Migrationen verwaltet wird, und klicken Sie dann später es zeigt, wie eine Datenbank durch Aktualisieren des DbDacFx-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="15561-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="15561-111">Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="15561-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="15561-112">Bereitstellen eines Datenbankupdates mithilfe von Code First-Migrationen</span><span class="sxs-lookup"><span data-stu-id="15561-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="15561-113">In diesem Abschnitt fügen Sie eine Birth Date-Spalte zu den `Person` Basisklasse für die `Student` und `Instructor` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="15561-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="15561-114">Klicken Sie dann aktualisieren Sie die Seite, die Instructor-Daten zeigt an, dass die neue Spalte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="15561-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="15561-115">Schließlich können Sie die Änderungen auf Test-, Staging- und produktionsumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="15561-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="15561-116">Hinzufügen einer Spalte zu einer Tabelle in der Anwendungsdatenbank</span><span class="sxs-lookup"><span data-stu-id="15561-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="15561-117">In der *ContosoUniversity.DAL* geöffneten Projekts *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es darf zwei schließende geschweifte Klammern, die darauf folgende):</span><span class="sxs-lookup"><span data-stu-id="15561-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="15561-118">Aktualisieren Sie als Nächstes die `Seed` Methode so, dass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="15561-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="15561-119">Open *Migrations\Configuration.cs* , und Ersetzen Sie den Codeblock, der beginnt, `var instructors = new List<Instructor>` mit den folgenden Codeblock das Geburtsdatum enthält:</span><span class="sxs-lookup"><span data-stu-id="15561-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="15561-120">Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster.</span><span class="sxs-lookup"><span data-stu-id="15561-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="15561-121">Sicherstellen, dass als ContosoUniversity.DAL ausgewählter der **Standardprojekt**.</span><span class="sxs-lookup"><span data-stu-id="15561-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="15561-122">In der **Package Manager Console** wählen **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="15561-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="15561-123">Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="15561-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="15561-124">Die `Up` Methode erstellt die Spalte aus, wenn Sie die Änderung zu implementieren und die `Down` Methode löscht die Spalte aus, wenn Sie die Änderung zurückgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="15561-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="15561-126">Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **Package Manager Console** Fenster (Stellen Sie sicher, dass das ContosoUniversity.DAL-Projekt ausgewählt ist):</span><span class="sxs-lookup"><span data-stu-id="15561-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="15561-127">Führt das Entity Framework die `Up` -Methode, und klicken Sie dann führt die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="15561-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="15561-128">Zeigen Sie die neue Spalte in der Seite "Lehrkräfte"</span><span class="sxs-lookup"><span data-stu-id="15561-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="15561-129">Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlage-Felds, um das Geburtsdatum anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="15561-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="15561-130">Fügen Sie es zwischen den Vorgängen für Hire Date und Office-Zuordnung hinzu:</span><span class="sxs-lookup"><span data-stu-id="15561-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="15561-131">(Wenn Code Einzug synchron abgerufen werden, können Sie STRG-K und dann STRG + D, um die Datei automatisch neu zu formatieren drücken.)</span><span class="sxs-lookup"><span data-stu-id="15561-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="15561-132">Führen Sie die Anwendung, und klicken Sie auf die **Lehrkräfte** Link.</span><span class="sxs-lookup"><span data-stu-id="15561-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="15561-133">Beim Laden der Seite angezeigt, dass die neue verfügt über Birth Date-Felds.</span><span class="sxs-lookup"><span data-stu-id="15561-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Seite "Lehrkräfte" mit "BirthDate"](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="15561-135">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="15561-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="15561-136">Das Datenbankupdate bereitstellen</span><span class="sxs-lookup"><span data-stu-id="15561-136">Deploy the database update</span></span>

1. <span data-ttu-id="15561-137">In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="15561-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="15561-138">In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="15561-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="15561-139">(Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt ContosoUniversity **Projektmappen-Explorer**.)</span><span class="sxs-lookup"><span data-stu-id="15561-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="15561-140">Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser geöffnet wird, um zur Startseite.</span><span class="sxs-lookup"><span data-stu-id="15561-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="15561-141">Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="15561-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="15561-142">Wenn die Anwendung versucht, den Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Datenbankschema und führt die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="15561-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="15561-143">Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Geburtsdatum** Spalte mit Datumsangaben darin.</span><span class="sxs-lookup"><span data-stu-id="15561-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="15561-144">In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Staging** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="15561-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="15561-145">Führen Sie die **Lehrkräfte** Seite in der Stagingumgebung, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="15561-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="15561-146">In der **Web eine klicken Sie auf Publish** -Symbolleiste klicken Sie auf die **Produktion** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="15561-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="15561-147">Führen Sie die **Lehrkräfte** Seite in der Produktion einsetzen, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="15561-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="15561-148">Für eine ein Update der Produktion-Anwendung, die Änderung an einer Datenbank enthält Sie auch in der Regel wäre die Anwendung offline, während der Bereitstellung mithilfe von *app\_offline.htm*, wie Sie im vorherigen Lernprogramm gesehen haben.</span><span class="sxs-lookup"><span data-stu-id="15561-148">For a a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="15561-149">Bereitstellen einer Datenbankupdate mit dem DbDacFx-Anbieter</span><span class="sxs-lookup"><span data-stu-id="15561-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="15561-150">In diesem Abschnitt fügen Sie eine *Kommentare* Spalte, um die *Benutzer* -Tabelle in der Mitgliedschaftsdatenbank und erstellen Sie eine Seite, mit dem Sie die Anzeige und Bearbeitung von Kommentare für jeden Benutzer.</span><span class="sxs-lookup"><span data-stu-id="15561-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="15561-151">Anschließend können Sie die Änderungen auf Test-, Staging- und produktionsumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="15561-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="15561-152">Hinzufügen einer Spalte zu einer Tabelle in der Mitgliedschaftsdatenbank</span><span class="sxs-lookup"><span data-stu-id="15561-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="15561-153">Öffnen Sie in Visual Studio **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="15561-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="15561-154">Erweitern Sie **(Localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **Aspnet-ContosoUniversity** (nicht **Aspnet-ContosoUniversity-Prod**) und schließlich **Tabellen**.</span><span class="sxs-lookup"><span data-stu-id="15561-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="15561-155">Wenn Sie nicht sehen **(Localdb) \v11.0** unter der **SQL Server** Knoten mit der rechten Maustaste die **SQL Server** Knoten, und klicken Sie auf **SQL-Server hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="15561-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="15561-156">In der **Verbindung mit Server herstellen** Geben Sie im Dialogfeld *(Localdb) \v11.0* als die **Servernamen**, und klicken Sie dann auf **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="15561-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="15561-157">Wenn Sie nicht sehen **Aspnet-ContosoUniversity**, führen Sie das Projekt, und melden Sie sich mit den *Admin* Anmeldeinformationen (Kennwort ist *Devpwd*), und aktualisieren Sie dann die  **Objekt-Explorer von SQL Server** Fenster.</span><span class="sxs-lookup"><span data-stu-id="15561-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="15561-158">Mit der rechten Maustaste die **Benutzer** Tabelle, und klicken Sie dann auf **Sicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="15561-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX Sicht-Designer](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="15561-160">Fügen Sie in den Designer ein *Kommentare* Spalte und *vom Datentyp nvarchar(128)* und NULL-Werte zulässt, und klicken Sie dann auf **Update**.</span><span class="sxs-lookup"><span data-stu-id="15561-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Hinzufügen von Kommentaren-Spalte](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="15561-162">In der **Vorschau der Datenbankupdates** auf **Update Database**.</span><span class="sxs-lookup"><span data-stu-id="15561-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Vorschau der Datenbankupdates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="15561-164">Erstellen Sie eine Seite zum Anzeigen und bearbeiten die neue Spalte</span><span class="sxs-lookup"><span data-stu-id="15561-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="15561-165">In **Projektmappen-Explorer**, mit der rechten Maustaste die **Konto** Ordner im ContosoUniversity-Projekt, klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element** .</span><span class="sxs-lookup"><span data-stu-id="15561-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="15561-166">Erstellen Sie ein neues **Web-Formular mithilfe Gestaltungsvorlage** und nennen Sie sie *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="15561-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="15561-167">Übernehmen Sie den Standardnamen *Site.Master* Datei als die Gestaltungsvorlage.</span><span class="sxs-lookup"><span data-stu-id="15561-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="15561-168">Kopieren Sie das folgende Markup in der `MainContent` `Content` Element (der letzte von 3 `Content` Elemente):</span><span class="sxs-lookup"><span data-stu-id="15561-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="15561-169">Mit der rechten Maustaste die *UserInfo.aspx* Seite, und klicken Sie auf **in Browser anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="15561-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="15561-170">Melden Sie sich mit Ihrem *Admin* Benutzeranmeldeinformationen (Kennwort wird *Devpwd*) und einige Kommentare hinzufügen, um einen Benutzer aus, um sicherzustellen, dass die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="15561-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="15561-172">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="15561-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="15561-173">Das Datenbankupdate bereitstellen</span><span class="sxs-lookup"><span data-stu-id="15561-173">Deploy the database update</span></span>

<span data-ttu-id="15561-174">Um mithilfe des DbDacFx-Anbieters bereitstellen, müssen Sie nur wählen Sie die **Datenbank aktualisieren** Option im Veröffentlichungsprofil.</span><span class="sxs-lookup"><span data-stu-id="15561-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="15561-175">Allerdings bei der erstmaligen Bereitstellung, wenn Sie diese Option verwendet Sie auch konfiguriert einige zusätzliche SQL­Skripts für die Ausführung: sind immer noch im Profil und Sie müssen verhindern, dass erneut auszuführen.</span><span class="sxs-lookup"><span data-stu-id="15561-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="15561-176">Öffnen der **Web veröffentlichen** Assistenten, indem Sie mit der rechten Maustaste des Projekts ContosoUniversity und auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="15561-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="15561-177">Wählen Sie die **Test** Profil.</span><span class="sxs-lookup"><span data-stu-id="15561-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="15561-178">Klicken Sie auf die **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="15561-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="15561-179">Klicken Sie unter **DefaultConnection**Option **Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="15561-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="15561-180">Deaktivieren Sie die zusätzliche Skripts, die Sie so konfiguriert, dass bei der ersten Bereitstellung ausführen:</span><span class="sxs-lookup"><span data-stu-id="15561-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="15561-181">Klicken Sie auf **Datenbankupdates konfigurieren**.</span><span class="sxs-lookup"><span data-stu-id="15561-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="15561-182">In der **Datenbankupdates konfigurieren** (Dialogfeld), deaktivieren Sie die Kontrollkästchen neben *Grant.sql* und *Aspnet-Daten-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="15561-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="15561-183">Klicken Sie auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="15561-183">Click **Close**.</span></span>
6. <span data-ttu-id="15561-184">Klicken Sie auf die **Vorschau** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="15561-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="15561-185">Klicken Sie unter **Datenbanken** und auf der rechten Seite des **DefaultConnection**, klicken Sie auf die **vorschaudatenbank** Link.</span><span class="sxs-lookup"><span data-stu-id="15561-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Datenbankvorschau](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="15561-187">Im Vorschaufenster wird das Skript aus, das in der Zieldatenbank vornehmen, das Schema der Quelldatenbank entsprechen Datenbankschema ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="15561-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="15561-188">Das Skript umfasst einen ALTER TABLE-Befehl aus, der die neue Spalte hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="15561-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="15561-189">Schließen der **Datenbankvorschau** (Dialogfeld), und klicken Sie dann auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="15561-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="15561-190">Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser geöffnet wird, um zur Startseite.</span><span class="sxs-lookup"><span data-stu-id="15561-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="15561-191">Führen Sie die Seite Benutzerinformationen (hinzufügen *Account/UserInfo.aspx* an die URL der Startseite "), stellen Sie sicher, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="15561-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="15561-192">Sie müssen hierzu melden Sie sich *Admin* und *Devpwd*.</span><span class="sxs-lookup"><span data-stu-id="15561-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="15561-193">Daten in Tabellen werden nicht standardmäßig bereitgestellt, und Sie haben nicht ein Bereitstellungsskript Daten ausgeführt wird, konfigurieren, damit Sie den Kommentar finden, den Sie in der Entwicklung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="15561-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="15561-194">Jetzt können Sie einen neuen Kommentar hinzufügen in der Stagingumgebung, um sicherzustellen, dass die Änderung für die Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="15561-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="15561-195">Gehen Sie genauso vor, Staging und Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="15561-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="15561-196">Vergessen Sie nicht die zusätzliche Skripts deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="15561-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="15561-197">Der einzige Unterschied im Vergleich zu dem Test-Profil ist, dass Sie nur ein Skript in der Stagingtabelle deaktiviert und Produktion Profile, da sie konfiguriert wurden, führen Sie nur *Aspnet-FA-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="15561-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="15561-198">Die Anmeldeinformationen für das Staging und Produktion sind die Administrator- und Prodpwd.</span><span class="sxs-lookup"><span data-stu-id="15561-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="15561-199">Für ein Update der Produktion-Anwendung, die Änderung an einer Datenbank dauern Sie in der Regel auch die Anwendung offline, während der Bereitstellung durch Hochladen *app\_offline.htm* vor der Veröffentlichung und löschen danach, wie Sie gesehen, im haben [das vorherige Lernprogramm](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="15561-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="15561-200">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="15561-200">Summary</span></span>

<span data-ttu-id="15561-201">Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank mithilfe von Code First-Migrationen und der DbDacFx-Anbieter enthalten.</span><span class="sxs-lookup"><span data-stu-id="15561-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Seite "Lehrkräfte" mit "BirthDate"](deploying-a-database-update/_static/image8.png)

![Seite "Benutzerinformationen"](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="15561-204">Der nächste Lernprogrammen wird gezeigt, wie Sie Bereitstellungen mithilfe von der Befehlszeile ausführen.</span><span class="sxs-lookup"><span data-stu-id="15561-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="15561-205">[Zurück](deploying-a-code-update.md)
[Weiter](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="15561-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
