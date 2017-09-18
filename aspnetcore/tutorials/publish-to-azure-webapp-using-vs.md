---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14ce45f0cd15b2de39f722767df076d2c0313787
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="c17b8-103">Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c17b8-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="c17b8-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) und [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c17b8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="c17b8-105">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="c17b8-105">Set up the development environment</span></span>

* <span data-ttu-id="c17b8-106">Installieren Sie [.NET Core und die Visual Studio-Tools](http://go.microsoft.com/fwlink/?LinkID=798306).</span><span class="sxs-lookup"><span data-stu-id="c17b8-106">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306).</span></span>

* <span data-ttu-id="c17b8-107">Überprüfen Sie Ihr [Azure-Konto](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c17b8-107">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="c17b8-108">Sie können [ein kostenloses Azure-Konto erhalten](https://azure.microsoft.com/pricing/free-trial/) oder [Leistungen für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="c17b8-108">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="c17b8-109">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="c17b8-109">Create a web app</span></span>

<span data-ttu-id="c17b8-110">Klicken Sie auf der Startseite von Visual Studio auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Datei (Menü)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="c17b8-112">Vervollständigen Sie das Dialogfeld **Neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="c17b8-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="c17b8-113">Wählen Sie im linken Bereich **.NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-113">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="c17b8-114">Wählen Sie im mittleren Bereich **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="c17b8-115">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-115">Select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="c17b8-117">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="c17b8-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="c17b8-118">Wählen Sie **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-118">Select **Web Application**.</span></span>

* <span data-ttu-id="c17b8-119">Wählen Sie **Authentifizierung ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-119">Select **Change Authentication**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="c17b8-121">Das Dialogfeld **Authentifizierung ändern** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c17b8-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="c17b8-122">Wählen Sie **Einzelne Benutzerkonten** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-122">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="c17b8-123">Klicken Sie auf **OK**, um zur Seite **Neue ASP.NET Core-Webanwendung** zurückzukehren, und klicken Sie anschließend noch einmal auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogfeld „New ASP.NET Core Web Authentication“ (Neue ASP.NET Core-Webauthentifizierung)](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="c17b8-125">Visual Studio erstellt die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="c17b8-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="c17b8-126">Lokales Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="c17b8-126">Run the app locally</span></span>

* <span data-ttu-id="c17b8-127">Wählen Sie zum lokalen Ausführen der App **Debuggen** und dann **Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-127">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="c17b8-128">Klicken Sie auf die Links **Info** und **Kontakt**, um zu überprüfen, ob die Webanwendung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="c17b8-128">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="c17b8-130">Wählen Sie **Registrieren**, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c17b8-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="c17b8-131">Sie können eine fiktive E-Mail-Adresse verwenden.</span><span class="sxs-lookup"><span data-stu-id="c17b8-131">You can use a fictitious email address.</span></span> <span data-ttu-id="c17b8-132">Sobald Sie diese übermitteln, zeigt die Seite die folgende Fehlermeldung an:</span><span class="sxs-lookup"><span data-stu-id="c17b8-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="c17b8-133">*„Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung. SQL-Ausnahme: Die Datenbank kann nicht geöffnet werden. Das Anwenden vorhandener Migrationen für den Datenbankkontext der Anwendung kann dieses Problem beheben.“*</span><span class="sxs-lookup"><span data-stu-id="c17b8-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="c17b8-134">Wählen Sie **Migrationen anwenden** aus, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="c17b8-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="c17b8-138">Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Abmelden**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![In Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="c17b8-141">Bereitstellen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="c17b8-141">Deploy the app to Azure</span></span>

<span data-ttu-id="c17b8-142">Schließen Sie die Webseite, kehren Sie zu Visual Studio zurück, und wählen Sie **Debuggen beenden** im Menü **Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-142">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="c17b8-143">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="c17b8-145">Wählen Sie im Dialogfeld **Veröffentlichen** **Microsoft Azure App Service** aus, und klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-145">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="c17b8-147">Geben Sie der App einen eindeutigen Namen.</span><span class="sxs-lookup"><span data-stu-id="c17b8-147">Name the app a unique name.</span></span> 

* <span data-ttu-id="c17b8-148">Wählen Sie ein MSDN-Abonnement aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-148">Select an MSDN subscription.</span></span>

* <span data-ttu-id="c17b8-149">Wählen Sie für die Ressourcengruppe **Neu...** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein.</span><span class="sxs-lookup"><span data-stu-id="c17b8-149">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="c17b8-150">Klicken Sie für den App Service-Plan auf **Neu...**, und wählen Sie einen Standort in Ihrer Nähe aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-150">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="c17b8-151">Sie können den standardmäßig generierten Namen behalten.</span><span class="sxs-lookup"><span data-stu-id="c17b8-151">You can keep the name that is generated by default.</span></span>

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="c17b8-153">Wählen Sie die Registerkarte **Dienste** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c17b8-153">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="c17b8-154">Klicken Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c17b8-154">Select the green **+** icon to create a new SQL Database</span></span>

![Neue SQL-Datenbank](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="c17b8-156">Wählen Sie im Dialogfeld **SQL-Datenbank konfigurieren** die Option **Neu...** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c17b8-156">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Neue SQL-Datenbank und -Server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="c17b8-158">Das Dialogfeld **SQL Server konfigurieren** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c17b8-158">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="c17b8-159">Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-159">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="c17b8-160">Merken Sie sich den Benutzernamen und das Kennwort, den/das Sie in diesem Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="c17b8-160">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="c17b8-161">Sie können den standardmäßigen **Servernamen** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="c17b8-161">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="c17b8-162">Geben Sie für die Datenbank und die Verbindungszeichenfolge Namen ein.</span><span class="sxs-lookup"><span data-stu-id="c17b8-162">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="c17b8-163">„admin“ ist als Benutzername des Administrators nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="c17b8-163">"admin" is not allowed as the administrator user name.</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="c17b8-165">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-165">Select **OK**.</span></span>

<span data-ttu-id="c17b8-166">Visual Studio kehrt zum Dialogfeld **App Service erstellen** zurück.</span><span class="sxs-lookup"><span data-stu-id="c17b8-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="c17b8-167">Wählen Sie im Dialogfeld **App Service erstellen** die Option **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="c17b8-169">Klicken Sie im Dialogfeld **Veröffentlichen** auf den Link **Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-169">Click the **Settings** link in the **Publish** dialog.</span></span>

![Dialogfeld „Veröffentlichen“: Bereich „Verbindung“](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="c17b8-171">Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:</span><span class="sxs-lookup"><span data-stu-id="c17b8-171">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="c17b8-172">Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-172">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="c17b8-173">Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-173">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="c17b8-174">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-174">Select **Save**.</span></span> <span data-ttu-id="c17b8-175">Visual Studio kehrt zum Dialogfeld **Veröffentlichen** zurück.</span><span class="sxs-lookup"><span data-stu-id="c17b8-175">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="c17b8-177">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-177">Click **Publish**.</span></span> <span data-ttu-id="c17b8-178">Visual Studio veröffentlicht die App in Azure und startet die Cloud-App in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="c17b8-178">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="c17b8-179">Testen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="c17b8-179">Test your app in Azure</span></span>

* <span data-ttu-id="c17b8-180">Testen Sie die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-180">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="c17b8-181">Registrieren eines neuen Benutzers</span><span class="sxs-lookup"><span data-stu-id="c17b8-181">Register a new user</span></span>

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="c17b8-183">Aktualisieren der App</span><span class="sxs-lookup"><span data-stu-id="c17b8-183">Update the app</span></span>

* <span data-ttu-id="c17b8-184">Bearbeiten Sie die Razor-Seite *Pages/About.cshtml*, und verändern Sie deren Inhalte.</span><span class="sxs-lookup"><span data-stu-id="c17b8-184">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="c17b8-185">Sie können beispielsweise den Absatz so verändern, dass er „Hallo ASP.NET Core!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="c17b8-185">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    <span data-ttu-id="c17b8-186">[!code-html[Info](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="c17b8-186">[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="c17b8-187">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-187">Right-click on the project and select **Publish...** again.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="c17b8-189">Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="c17b8-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Der Überprüfungstask ist abgeschlossen.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="c17b8-191">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="c17b8-191">Clean up</span></span>

<span data-ttu-id="c17b8-192">Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="c17b8-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="c17b8-193">Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="c17b8-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="c17b8-195">Wählen Sie auf der Seite **Ressourcengruppen** **Löschen** aus.</span><span class="sxs-lookup"><span data-stu-id="c17b8-195">In the **Resource groups** page, select **Delete**.</span></span>

![Azure-Portal: Seite „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="c17b8-197">Geben Sie den Namen der Ressourcengruppe ein, und klicken Sie auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="c17b8-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="c17b8-198">Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.</span><span class="sxs-lookup"><span data-stu-id="c17b8-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="c17b8-199">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c17b8-199">Next steps</span></span>

* [<span data-ttu-id="c17b8-200">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c17b8-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="c17b8-201">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c17b8-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="c17b8-202">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="c17b8-202">Fundamentals</span></span>](../fundamentals/index.md)
