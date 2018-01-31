---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio"
author: rick-anderson
description: "Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio veröffentlicht wird."
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14d8dd0a5e6a99bacce3bf50b0468b20e0dddb96
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="22eeb-103">Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22eeb-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="22eeb-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) und [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="22eeb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="22eeb-105">Lesen Sie [Veröffentlichen in Azure aus Visual Studio für Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/), wenn Sie auf einem Mac arbeiten.</span><span class="sxs-lookup"><span data-stu-id="22eeb-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="22eeb-106">Einrichten</span><span class="sxs-lookup"><span data-stu-id="22eeb-106">Set up</span></span>

* <span data-ttu-id="22eeb-107">Eröffnen Sie ein [kostenloses Azure-Konto](https://aka.ms/K5y5yh), wenn Sie noch über kein Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="22eeb-108">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="22eeb-108">Create a web app</span></span>

<span data-ttu-id="22eeb-109">Klicken Sie auf der Startseite von Visual Studio auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Datei (Menü)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="22eeb-111">Vervollständigen Sie das Dialogfeld **Neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="22eeb-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="22eeb-112">Wählen Sie im linken Bereich **.NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-112">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="22eeb-113">Wählen Sie im mittleren Bereich **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="22eeb-114">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-114">Select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="22eeb-116">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="22eeb-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="22eeb-117">Wählen Sie **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-117">Select **Web Application**.</span></span>
* <span data-ttu-id="22eeb-118">Wählen Sie **Authentifizierung ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-118">Select **Change Authentication**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="22eeb-120">Das Dialogfeld **Authentifizierung ändern** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="22eeb-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="22eeb-121">Wählen Sie **Einzelne Benutzerkonten** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-121">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="22eeb-122">Klicken Sie auf **OK**, um zur Seite **Neue ASP.NET Core-Webanwendung** zurückzukehren, und klicken Sie anschließend noch einmal auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogfeld „New ASP.NET Core Web Authentication“ (Neue ASP.NET Core-Webauthentifizierung)](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="22eeb-124">Visual Studio erstellt die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="22eeb-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="22eeb-125">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="22eeb-125">Run the app</span></span>

* <span data-ttu-id="22eeb-126">Drücken Sie STRG+F5, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-126">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="22eeb-127">Testen Sie die Links **About** (Info) und **Contact** (Kontakt).</span><span class="sxs-lookup"><span data-stu-id="22eeb-127">Test the **About** and **Contact** links.</span></span>

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="22eeb-129">Registrieren eines Benutzers</span><span class="sxs-lookup"><span data-stu-id="22eeb-129">Register a user</span></span>

* <span data-ttu-id="22eeb-130">Wählen Sie **Registrieren**, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="22eeb-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="22eeb-131">Sie können eine fiktive E-Mail-Adresse verwenden.</span><span class="sxs-lookup"><span data-stu-id="22eeb-131">You can use a fictitious email address.</span></span> <span data-ttu-id="22eeb-132">Sobald Sie diese übermitteln, zeigt die Seite die folgende Fehlermeldung an:</span><span class="sxs-lookup"><span data-stu-id="22eeb-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="22eeb-133">*„Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung. SQL-Ausnahme: Die Datenbank kann nicht geöffnet werden. Das Anwenden vorhandener Migrationen für den Datenbankkontext der Anwendung kann dieses Problem beheben.“*</span><span class="sxs-lookup"><span data-stu-id="22eeb-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="22eeb-134">Wählen Sie **Migrationen anwenden** aus, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="22eeb-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="22eeb-138">Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Abmelden**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![In Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="22eeb-141">Bereitstellen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="22eeb-141">Deploy the app to Azure</span></span>

<span data-ttu-id="22eeb-142">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="22eeb-144">Führen Sie im Dialogfeld **Veröffentlichen** folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="22eeb-144">In the **Publish** dialog:</span></span>

* <span data-ttu-id="22eeb-145">Wählen Sie **Microsoft Azure App Service** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-145">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="22eeb-146">Klicken Sie auf das Zahnradsymbol, und wählen Sie anschließend **Profil erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-146">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="22eeb-147">Klicken Sie auf **Profil erstellen**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-147">Select **Create Profile**.</span></span>

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="22eeb-149">Erstellen von Azure-Ressourcen</span><span class="sxs-lookup"><span data-stu-id="22eeb-149">Create Azure resources</span></span>

<span data-ttu-id="22eeb-150">Das Dialogfeld **App Service erstellen** wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="22eeb-150">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="22eeb-151">Geben Sie Ihr Abonnement ein.</span><span class="sxs-lookup"><span data-stu-id="22eeb-151">Enter your subscription.</span></span>
* <span data-ttu-id="22eeb-152">Die Eingabefelder **App-Name**, **Ressourcengruppe** und **App Service-Plan** werden aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="22eeb-152">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="22eeb-153">Sie können diese Namen beibehalten oder ändern.</span><span class="sxs-lookup"><span data-stu-id="22eeb-153">You can keep these names or change them.</span></span>

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="22eeb-155">Wählen Sie die Registerkarte **Dienste** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-155">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="22eeb-156">Klicken Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-156">Select the green **+** icon to create a new SQL Database</span></span>

![Neue SQL-Datenbank](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="22eeb-158">Wählen Sie im Dialogfeld **SQL-Datenbank konfigurieren** die Option **Neu...** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-158">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Neue SQL-Datenbank und -Server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="22eeb-160">Das Dialogfeld **SQL Server konfigurieren** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="22eeb-160">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="22eeb-161">Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-161">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="22eeb-162">Sie können den standardmäßigen **Servernamen** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="22eeb-162">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="22eeb-163">„admin“ ist als Benutzername des Administrators nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="22eeb-163">"admin" isn't allowed as the administrator user name.</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="22eeb-165">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-165">Select **OK**.</span></span>

<span data-ttu-id="22eeb-166">Visual Studio kehrt zum Dialogfeld **App Service erstellen** zurück.</span><span class="sxs-lookup"><span data-stu-id="22eeb-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="22eeb-167">Wählen Sie im Dialogfeld **App Service erstellen** die Option **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="22eeb-169">Visual Studio erstellt die Web-App und SQL Server in Azure.</span><span class="sxs-lookup"><span data-stu-id="22eeb-169">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="22eeb-170">Dies kann einige Minuten in Anspruch nehmen.</span><span class="sxs-lookup"><span data-stu-id="22eeb-170">This step can take a few minutes.</span></span> <span data-ttu-id="22eeb-171">Weitere Informationen zu den erstellten Ressourcen finden Sie unter [Zusätzliche Ressourcen](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="22eeb-171">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="22eeb-172">Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Einstellungen**:</span><span class="sxs-lookup"><span data-stu-id="22eeb-172">When deployment completes, select **Settings**:</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="22eeb-174">Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:</span><span class="sxs-lookup"><span data-stu-id="22eeb-174">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="22eeb-175">Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-175">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="22eeb-176">Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-176">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="22eeb-177">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-177">Select **Save**.</span></span> <span data-ttu-id="22eeb-178">Visual Studio kehrt zum Dialogfeld **Veröffentlichen** zurück.</span><span class="sxs-lookup"><span data-stu-id="22eeb-178">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="22eeb-180">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-180">Click **Publish**.</span></span> <span data-ttu-id="22eeb-181">Visual Studio veröffentlicht Ihre App in Azure.</span><span class="sxs-lookup"><span data-stu-id="22eeb-181">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="22eeb-182">Wenn die Bereitstellung abgeschlossen ist, wird die App im Browser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="22eeb-182">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="22eeb-183">Testen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="22eeb-183">Test your app in Azure</span></span>

* <span data-ttu-id="22eeb-184">Testen Sie die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-184">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="22eeb-185">Registrieren eines neuen Benutzers</span><span class="sxs-lookup"><span data-stu-id="22eeb-185">Register a new user</span></span>

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="22eeb-187">Aktualisieren der App</span><span class="sxs-lookup"><span data-stu-id="22eeb-187">Update the app</span></span>

* <span data-ttu-id="22eeb-188">Bearbeiten Sie die Razor-Seite *Pages/About.cshtml*, und verändern Sie deren Inhalte.</span><span class="sxs-lookup"><span data-stu-id="22eeb-188">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="22eeb-189">Sie können beispielsweise den Absatz so verändern, dass er „Hallo ASP.NET Core!“ anzeigt: [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="22eeb-189">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="22eeb-190">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-190">Right-click on the project and select **Publish...** again.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="22eeb-192">Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="22eeb-192">After the app is published, verify the changes you made are available on Azure.</span></span>

![Der Überprüfungstask ist abgeschlossen.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="22eeb-194">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="22eeb-194">Clean up</span></span>

<span data-ttu-id="22eeb-195">Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="22eeb-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="22eeb-196">Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="22eeb-196">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="22eeb-198">Wählen Sie auf der Seite **Ressourcengruppen** **Löschen** aus.</span><span class="sxs-lookup"><span data-stu-id="22eeb-198">In the **Resource groups** page, select **Delete**.</span></span>

![Azure-Portal: Seite „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="22eeb-200">Geben Sie den Namen der Ressourcengruppe ein, und klicken Sie auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="22eeb-200">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="22eeb-201">Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.</span><span class="sxs-lookup"><span data-stu-id="22eeb-201">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="22eeb-202">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="22eeb-202">Next steps</span></span>

* [<span data-ttu-id="22eeb-203">Continuous Deployment in Azure mit Visual Studio und Git</span><span class="sxs-lookup"><span data-stu-id="22eeb-203">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="22eeb-204">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="22eeb-204">Additonal resources</span></span>

* [<span data-ttu-id="22eeb-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="22eeb-205">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="22eeb-206">Azure-Ressourcengruppen</span><span class="sxs-lookup"><span data-stu-id="22eeb-206">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="22eeb-207">Azure SQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="22eeb-207">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
