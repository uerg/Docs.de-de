---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio"
author: rick-anderson
description: "Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio veröffentlicht wird."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="6202e-104">/en-us</span><span class="sxs-lookup"><span data-stu-id="6202e-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="6202e-105">Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6202e-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="6202e-106">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) und [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6202e-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6202e-107">Lesen Sie [Veröffentlichen in Azure aus Visual Studio für Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/), wenn Sie auf einem Mac arbeiten.</span><span class="sxs-lookup"><span data-stu-id="6202e-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="6202e-108">Einrichten</span><span class="sxs-lookup"><span data-stu-id="6202e-108">Set up</span></span>

* <span data-ttu-id="6202e-109">Öffnen Sie ein [kostenloses Azure-Konto](https://aka.ms/K5y5yh), wenn Sie noch keines haben.</span><span class="sxs-lookup"><span data-stu-id="6202e-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="6202e-110">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="6202e-110">Create a web app</span></span>

<span data-ttu-id="6202e-111">Klicken Sie auf der Startseite von Visual Studio auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="6202e-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Datei (Menü)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="6202e-113">Vervollständigen Sie das Dialogfeld **Neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="6202e-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6202e-114">Wählen Sie im linken Bereich **.NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="6202e-115">Wählen Sie im mittleren Bereich **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="6202e-116">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6202e-116">Select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="6202e-118">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6202e-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="6202e-119">Wählen Sie **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-119">Select **Web Application**.</span></span>
* <span data-ttu-id="6202e-120">Wählen Sie **Authentifizierung ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-120">Select **Change Authentication**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="6202e-122">Das Dialogfeld **Authentifizierung ändern** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6202e-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="6202e-123">Wählen Sie **Einzelne Benutzerkonten** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="6202e-124">Klicken Sie auf **OK**, um zur Seite **Neue ASP.NET Core-Webanwendung** zurückzukehren, und klicken Sie anschließend noch einmal auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6202e-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogfeld „New ASP.NET Core Web Authentication“ (Neue ASP.NET Core-Webauthentifizierung)](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="6202e-126">Visual Studio erstellt die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="6202e-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6202e-127">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="6202e-127">Run the app</span></span>

* <span data-ttu-id="6202e-128">Drücken Sie STRG+F5, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6202e-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="6202e-129">Testen Sie die Links **About** (Info) und **Contact** (Kontakt).</span><span class="sxs-lookup"><span data-stu-id="6202e-129">Test the **About** and **Contact** links.</span></span>

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="6202e-131">Registrieren eines Benutzers</span><span class="sxs-lookup"><span data-stu-id="6202e-131">Register a user</span></span>

* <span data-ttu-id="6202e-132">Wählen Sie **Registrieren**, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="6202e-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="6202e-133">Sie können eine fiktive E-Mail-Adresse verwenden.</span><span class="sxs-lookup"><span data-stu-id="6202e-133">You can use a fictitious email address.</span></span> <span data-ttu-id="6202e-134">Sobald Sie diese übermitteln, zeigt die Seite die folgende Fehlermeldung an:</span><span class="sxs-lookup"><span data-stu-id="6202e-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="6202e-135">*„Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung. SQL-Ausnahme: Die Datenbank kann nicht geöffnet werden. Das Anwenden vorhandener Migrationen für den Datenbankkontext der Anwendung kann dieses Problem beheben.“*</span><span class="sxs-lookup"><span data-stu-id="6202e-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="6202e-136">Wählen Sie **Migrationen anwenden** aus, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="6202e-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="6202e-140">Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Abmelden**.</span><span class="sxs-lookup"><span data-stu-id="6202e-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![In Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="6202e-143">Bereitstellen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="6202e-143">Deploy the app to Azure</span></span>

<span data-ttu-id="6202e-144">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="6202e-146">Führen Sie im Dialogfeld **Veröffentlichen** folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="6202e-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="6202e-147">Wählen Sie **Microsoft Azure App Service** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="6202e-148">Klicken Sie auf das Zahnradsymbol, und wählen Sie anschließend **Profil erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="6202e-149">Klicken Sie auf **Profil erstellen**.</span><span class="sxs-lookup"><span data-stu-id="6202e-149">Select **Create Profile**.</span></span>

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="6202e-151">Erstellen von Azure-Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6202e-151">Create Azure resources</span></span>

<span data-ttu-id="6202e-152">Das Dialogfeld **App Service erstellen** wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6202e-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="6202e-153">Geben Sie Ihr Abonnement ein.</span><span class="sxs-lookup"><span data-stu-id="6202e-153">Enter your subscription.</span></span>
* <span data-ttu-id="6202e-154">Die Eingabefelder **App-Name**, **Ressourcengruppe** und **App Service-Plan** werden aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="6202e-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="6202e-155">Sie können diese Namen beibehalten oder ändern.</span><span class="sxs-lookup"><span data-stu-id="6202e-155">You can keep these names or change them.</span></span>

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="6202e-157">Wählen Sie die Registerkarte **Dienste** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6202e-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="6202e-158">Klicken Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6202e-158">Select the green **+** icon to create a new SQL Database</span></span>

![Neue SQL-Datenbank](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="6202e-160">Wählen Sie im Dialogfeld **SQL-Datenbank konfigurieren** die Option **Neu...** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6202e-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Neue SQL-Datenbank und -Server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="6202e-162">Das Dialogfeld **SQL Server konfigurieren** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6202e-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="6202e-163">Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6202e-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="6202e-164">Sie können den standardmäßigen **Servernamen** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="6202e-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="6202e-165">„admin“ ist als Benutzername des Administrators nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="6202e-165">"admin" is not allowed as the administrator user name.</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="6202e-167">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6202e-167">Select **OK**.</span></span>

<span data-ttu-id="6202e-168">Visual Studio kehrt zum Dialogfeld **App Service erstellen** zurück.</span><span class="sxs-lookup"><span data-stu-id="6202e-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="6202e-169">Wählen Sie im Dialogfeld **App Service erstellen** die Option **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="6202e-171">Visual Studio erstellt die Web-App und SQL Server in Azure.</span><span class="sxs-lookup"><span data-stu-id="6202e-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="6202e-172">Dies kann einige Minuten in Anspruch nehmen.</span><span class="sxs-lookup"><span data-stu-id="6202e-172">This step can take a few minutes.</span></span> <span data-ttu-id="6202e-173">Weitere Informationen zu den erstellten Ressourcen finden Sie unter [Zusätzliche Ressourcen](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="6202e-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="6202e-174">Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Einstellungen**:</span><span class="sxs-lookup"><span data-stu-id="6202e-174">When deployment completes, select **Settings**:</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="6202e-176">Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:</span><span class="sxs-lookup"><span data-stu-id="6202e-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="6202e-177">Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.</span><span class="sxs-lookup"><span data-stu-id="6202e-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="6202e-178">Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.</span><span class="sxs-lookup"><span data-stu-id="6202e-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="6202e-179">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="6202e-179">Select **Save**.</span></span> <span data-ttu-id="6202e-180">Visual Studio kehrt zum Dialogfeld **Veröffentlichen** zurück.</span><span class="sxs-lookup"><span data-stu-id="6202e-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="6202e-182">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="6202e-182">Click **Publish**.</span></span> <span data-ttu-id="6202e-183">Visual Studio veröffentlicht Ihre App in Azure.</span><span class="sxs-lookup"><span data-stu-id="6202e-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="6202e-184">Wenn die Bereitstellung abgeschlossen ist, wird die App im Browser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6202e-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="6202e-185">Testen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="6202e-185">Test your app in Azure</span></span>

* <span data-ttu-id="6202e-186">Testen Sie die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="6202e-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="6202e-187">Registrieren eines neuen Benutzers</span><span class="sxs-lookup"><span data-stu-id="6202e-187">Register a new user</span></span>

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="6202e-189">Aktualisieren der App</span><span class="sxs-lookup"><span data-stu-id="6202e-189">Update the app</span></span>

* <span data-ttu-id="6202e-190">Bearbeiten Sie die Razor-Seite *Pages/About.cshtml*, und verändern Sie deren Inhalte.</span><span class="sxs-lookup"><span data-stu-id="6202e-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="6202e-191">Sie können beispielsweise den Absatz so verändern, dass er „Hallo ASP.NET Core!“ anzeigt: [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="6202e-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="6202e-192">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-192">Right-click on the project and select **Publish...** again.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="6202e-194">Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="6202e-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Der Überprüfungstask ist abgeschlossen.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="6202e-196">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="6202e-196">Clean up</span></span>

<span data-ttu-id="6202e-197">Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="6202e-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="6202e-198">Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="6202e-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="6202e-200">Wählen Sie auf der Seite **Ressourcengruppen** **Löschen** aus.</span><span class="sxs-lookup"><span data-stu-id="6202e-200">In the **Resource groups** page, select **Delete**.</span></span>

![Azure-Portal: Seite „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="6202e-202">Geben Sie den Namen der Ressourcengruppe ein, und klicken Sie auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="6202e-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="6202e-203">Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.</span><span class="sxs-lookup"><span data-stu-id="6202e-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6202e-204">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="6202e-204">Next steps</span></span>

* [<span data-ttu-id="6202e-205">Continuous Deployment in Azure mit Visual Studio und Git</span><span class="sxs-lookup"><span data-stu-id="6202e-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="6202e-206">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6202e-206">Additonal resources</span></span>

* [<span data-ttu-id="6202e-207">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6202e-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="6202e-208">Azure-Ressourcengruppen</span><span class="sxs-lookup"><span data-stu-id="6202e-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="6202e-209">Azure SQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="6202e-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)