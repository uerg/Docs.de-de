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
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="c8e45-103">Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8e45-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="c8e45-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) und [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c8e45-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="c8e45-105">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="c8e45-105">Set up the development environment</span></span>

* <span data-ttu-id="c8e45-106">Installieren Sie das neueste [Azure SDK für Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span><span class="sxs-lookup"><span data-stu-id="c8e45-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span></span> <span data-ttu-id="c8e45-107">Das SDK installiert Visual Studio, falls noch nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="c8e45-107">The SDK installs Visual Studio if you don't already have it.</span></span>

* <span data-ttu-id="c8e45-108">Überprüfen Sie Ihr [Azure-Konto](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c8e45-108">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="c8e45-109">Sie können [ein kostenloses Azure-Konto erhalten](https://azure.microsoft.com/pricing/free-trial/) oder [Leistungen für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="c8e45-109">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="c8e45-110">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="c8e45-110">Create a web app</span></span>

<span data-ttu-id="c8e45-111">Klicken Sie auf der Startseite von Visual Studio auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Datei (Menü)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="c8e45-113">Vervollständigen Sie das Dialogfeld **Neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="c8e45-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="c8e45-114">Wählen Sie im linken Bereich **.NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-114">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="c8e45-115">Wählen Sie im mittleren Bereich **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="c8e45-116">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-116">Select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="c8e45-118">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="c8e45-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="c8e45-119">Wählen Sie **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-119">Select **Web Application**.</span></span>

* <span data-ttu-id="c8e45-120">Wählen Sie **Authentifizierung ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-120">Select **Change Authentication**.</span></span>

![Dialogfeld "Neues Projekt"](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="c8e45-122">Das Dialogfeld **Authentifizierung ändern** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c8e45-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="c8e45-123">Wählen Sie **Einzelne Benutzerkonten** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-123">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="c8e45-124">Klicken Sie auf **OK**, um zur Seite **Neue ASP.NET Core-Webanwendung** zurückzukehren, und klicken Sie anschließend noch einmal auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Dialogfeld „New ASP.NET Core Web Authentication“ (Neue ASP.NET Core-Webauthentifizierung)](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="c8e45-126">Visual Studio erstellt die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="c8e45-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="c8e45-127">Lokales Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="c8e45-127">Run the app locally</span></span>

* <span data-ttu-id="c8e45-128">Wählen Sie zum lokalen Ausführen der App **Debuggen** und dann **Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-128">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="c8e45-129">Klicken Sie auf die Links **Info** und **Kontakt**, um zu überprüfen, ob die Webanwendung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="c8e45-129">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="c8e45-131">Wählen Sie **Registrieren**, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c8e45-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="c8e45-132">Sie können eine fiktive E-Mail-Adresse verwenden.</span><span class="sxs-lookup"><span data-stu-id="c8e45-132">You can use a fictitious email address.</span></span> <span data-ttu-id="c8e45-133">Sobald Sie diese übermitteln, zeigt die Seite die folgende Fehlermeldung an:</span><span class="sxs-lookup"><span data-stu-id="c8e45-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="c8e45-134">*„Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung. SQL-Ausnahme: Die Datenbank kann nicht geöffnet werden. Das Anwenden vorhandener Migrationen für den Datenbankkontext der Anwendung kann dieses Problem beheben.“*</span><span class="sxs-lookup"><span data-stu-id="c8e45-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="c8e45-135">Wählen Sie **Migrationen anwenden** aus, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="c8e45-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="c8e45-139">Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Abmelden**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![In Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="c8e45-142">Bereitstellen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="c8e45-142">Deploy the app to Azure</span></span>

<span data-ttu-id="c8e45-143">Schließen Sie die Webseite, kehren Sie zu Visual Studio zurück, und wählen Sie **Debuggen beenden** im Menü **Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-143">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="c8e45-144">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="c8e45-146">Wählen Sie im Dialogfeld **Veröffentlichen** **Microsoft Azure App Service** aus, und klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-146">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="c8e45-148">Geben Sie der App einen eindeutigen Namen.</span><span class="sxs-lookup"><span data-stu-id="c8e45-148">Name the app a unique name.</span></span> 

* <span data-ttu-id="c8e45-149">Wählen Sie ein Abonnement aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-149">Select a subscription.</span></span>

* <span data-ttu-id="c8e45-150">Wählen Sie für die Ressourcengruppe **Neu...** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein.</span><span class="sxs-lookup"><span data-stu-id="c8e45-150">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="c8e45-151">Klicken Sie für den App Service-Plan auf **Neu...**, und wählen Sie einen Standort in Ihrer Nähe aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-151">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="c8e45-152">Sie können den standardmäßig generierten Namen behalten.</span><span class="sxs-lookup"><span data-stu-id="c8e45-152">You can keep the name that is generated by default.</span></span>

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="c8e45-154">Wählen Sie die Registerkarte **Dienste** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c8e45-154">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="c8e45-155">Klicken Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c8e45-155">Select the green **+** icon to create a new SQL Database</span></span>

![Neue SQL-Datenbank](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="c8e45-157">Wählen Sie im Dialogfeld **SQL-Datenbank konfigurieren** die Option **Neu...** aus, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c8e45-157">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Neue SQL-Datenbank und -Server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="c8e45-159">Das Dialogfeld **SQL Server konfigurieren** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c8e45-159">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="c8e45-160">Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-160">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="c8e45-161">Merken Sie sich den Benutzernamen und das Kennwort, den/das Sie in diesem Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="c8e45-161">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="c8e45-162">Sie können den standardmäßigen **Servernamen** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="c8e45-162">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="c8e45-163">Geben Sie für die Datenbank und die Verbindungszeichenfolge Namen ein.</span><span class="sxs-lookup"><span data-stu-id="c8e45-163">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="c8e45-164">„admin“ ist als Benutzername des Administrators nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="c8e45-164">"admin" is not allowed as the administrator user name.</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="c8e45-166">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-166">Select **OK**.</span></span>

<span data-ttu-id="c8e45-167">Visual Studio kehrt zum Dialogfeld **App Service erstellen** zurück.</span><span class="sxs-lookup"><span data-stu-id="c8e45-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="c8e45-168">Wählen Sie im Dialogfeld **App Service erstellen** die Option **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="c8e45-170">Klicken Sie im Dialogfeld **Veröffentlichen** auf den Link **Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-170">Click the **Settings** link in the **Publish** dialog.</span></span>

![Dialogfeld „Veröffentlichen“: Bereich „Verbindung“](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="c8e45-172">Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:</span><span class="sxs-lookup"><span data-stu-id="c8e45-172">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="c8e45-173">Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-173">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="c8e45-174">Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-174">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="c8e45-175">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-175">Select **Save**.</span></span> <span data-ttu-id="c8e45-176">Visual Studio kehrt zum Dialogfeld **Veröffentlichen** zurück.</span><span class="sxs-lookup"><span data-stu-id="c8e45-176">Visual Studio returns to the **Publish** dialog.</span></span> 

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="c8e45-178">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-178">Click **Publish**.</span></span> <span data-ttu-id="c8e45-179">Visual Studio veröffentlicht die App in Azure und startet die Cloud-App in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="c8e45-179">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="c8e45-180">Testen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="c8e45-180">Test your app in Azure</span></span>

* <span data-ttu-id="c8e45-181">Testen Sie die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-181">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="c8e45-182">Registrieren eines neuen Benutzers</span><span class="sxs-lookup"><span data-stu-id="c8e45-182">Register a new user</span></span>

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="c8e45-184">Aktualisieren der App</span><span class="sxs-lookup"><span data-stu-id="c8e45-184">Update the app</span></span>

* <span data-ttu-id="c8e45-185">Bearbeiten Sie die Razor-Seite *Pages/About.cshtml*, und verändern Sie deren Inhalte.</span><span class="sxs-lookup"><span data-stu-id="c8e45-185">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="c8e45-186">Sie können beispielsweise den Absatz so verändern, dass er „Hallo ASP.NET Core!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="c8e45-186">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="c8e45-187">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-187">Right-click on the project and select **Publish...** again.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="c8e45-189">Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="c8e45-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Der Überprüfungstask ist abgeschlossen.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="c8e45-191">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="c8e45-191">Clean up</span></span>

<span data-ttu-id="c8e45-192">Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="c8e45-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="c8e45-193">Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="c8e45-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="c8e45-195">Wählen Sie auf der Seite **Ressourcengruppen** **Löschen** aus.</span><span class="sxs-lookup"><span data-stu-id="c8e45-195">In the **Resource groups** page, select **Delete**.</span></span>

![Azure-Portal: Seite „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="c8e45-197">Geben Sie den Namen der Ressourcengruppe ein, und klicken Sie auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="c8e45-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="c8e45-198">Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.</span><span class="sxs-lookup"><span data-stu-id="c8e45-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="c8e45-199">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c8e45-199">Next steps</span></span>

* [<span data-ttu-id="c8e45-200">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8e45-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="c8e45-201">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8e45-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="c8e45-202">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="c8e45-202">Fundamentals</span></span>](../fundamentals/index.md)
