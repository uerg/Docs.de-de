---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="fe866-103">Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe866-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="fe866-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="fe866-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="fe866-105">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="fe866-105">Set up the development environment</span></span>

* <span data-ttu-id="fe866-106">Installieren Sie das neueste [Azure SDK für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span><span class="sxs-lookup"><span data-stu-id="fe866-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="fe866-107">Das SDK installiert Visual Studio, falls noch nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="fe866-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="fe866-108">Die SDK-Installation kann mehr als 30 Minuten dauern, wenn Ihr Computer nur wenige der Abhängigkeiten aufweist.</span><span class="sxs-lookup"><span data-stu-id="fe866-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="fe866-109">Installieren Sie [.NET Core und die Visual Studio-Tools](http://go.microsoft.com/fwlink/?LinkID=798306).</span><span class="sxs-lookup"><span data-stu-id="fe866-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="fe866-110">Überprüfen Sie Ihr [Azure-Konto](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fe866-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="fe866-111">Sie können [ein kostenloses Azure-Konto erhalten](https://azure.microsoft.com/pricing/free-trial/) oder [Leistungen für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="fe866-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="fe866-112">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="fe866-112">Create a web app</span></span>

<span data-ttu-id="fe866-113">Tippen Sie auf der Visual Studio-Startseite, auf **Neues Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="fe866-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![Startseite](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="fe866-115">Alternativ können Sie in den Menüs ein neues Projekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="fe866-116">Tippen Sie auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="fe866-116">Tap **File > New > Project...**.</span></span>

![Menü „Datei“](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="fe866-118">Vervollständigen Sie das Dialogfeld **Neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="fe866-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="fe866-119">Tippen Sie im linken Bereich auf **Web**.</span><span class="sxs-lookup"><span data-stu-id="fe866-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="fe866-120">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="fe866-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="fe866-121">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe866-121">Tap **OK**</span></span>

![Dialogfeld „Neues Projekt“](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="fe866-123">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung (.NET Core)** so vor:</span><span class="sxs-lookup"><span data-stu-id="fe866-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="fe866-124">Tippen Sie auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="fe866-124">Tap **Web Application**</span></span>

* <span data-ttu-id="fe866-125">Vergewissern Sie sich, dass **Authentifizierung** auf **Einzelne Benutzerkonten** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="fe866-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="fe866-126">Vergewissern Sie sich, dass **Host in der Cloud** **nicht** aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="fe866-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="fe866-127">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe866-127">Tap **OK**</span></span>

![Dialogfeld „Neue ASP.NET Core-Webanwendung (.NET Core)“](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="fe866-129">Lokales Testen der App</span><span class="sxs-lookup"><span data-stu-id="fe866-129">Test the app locally</span></span>

* <span data-ttu-id="fe866-130">Drücken Sie **STRG+F5**, um die App lokal auszuführen.</span><span class="sxs-lookup"><span data-stu-id="fe866-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="fe866-131">Tippen Sie auf die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="fe866-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="fe866-132">Je nach Größe Ihres Geräts müssen Sie möglicherweise auf das Navigationssymbol klicken, um die Links anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="fe866-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="fe866-134">Tippen Sie auf **Registrieren**, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="fe866-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="fe866-135">Sie können eine fiktive E-Mail-Adresse verwenden.</span><span class="sxs-lookup"><span data-stu-id="fe866-135">You can use a fictitious email address.</span></span> <span data-ttu-id="fe866-136">Sobald Sie diese übermitteln, erhalten Sie die folgende Fehlermeldung:</span><span class="sxs-lookup"><span data-stu-id="fe866-136">When you submit, you'll get the following error:</span></span>

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="fe866-140">Sie können das Problem auf zwei Arten beheben:</span><span class="sxs-lookup"><span data-stu-id="fe866-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="fe866-141">Tippen Sie auf **Migrationen anwenden**, und aktualisieren Sie die Seite. Oder</span><span class="sxs-lookup"><span data-stu-id="fe866-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="fe866-142">Führen Sie an einer Eingabeaufforderung im Verzeichnis des Projekts den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="fe866-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="fe866-143">Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Log off**.</span><span class="sxs-lookup"><span data-stu-id="fe866-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![In Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="fe866-146">Bereitstellen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="fe866-146">Deploy the app to Azure</span></span>

<span data-ttu-id="fe866-147">Stellen Sie sicher, dass die zur Bereitstellung veröffentlichte App nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fe866-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="fe866-148">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fe866-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="fe866-149">Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.</span><span class="sxs-lookup"><span data-stu-id="fe866-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="fe866-150">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.</span><span class="sxs-lookup"><span data-stu-id="fe866-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="fe866-152">Tippen Sie im Dialogfeld **Veröffentlichen** auf **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="fe866-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="fe866-154">Tippen Sie auf **Neu...**, um eine neue Ressourcengruppe zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="fe866-155">Das Erstellen einer neuen Ressourcengruppe vereinfacht das Löschen aller Azure-Ressourcen, die Sie in diesem Tutorial erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="fe866-157">Erstellen Sie eine neue Ressourcengruppe und einen App Service-Plan:</span><span class="sxs-lookup"><span data-stu-id="fe866-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="fe866-158">Tippen Sie für die Ressourcengruppe auf **Neu...**, und geben Sie einen Namen für die neue Ressourcengruppe ein.</span><span class="sxs-lookup"><span data-stu-id="fe866-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="fe866-159">Tippen Sie für den App Service-Plan auf **Neu...**, und wählen Sie einen Standort in Ihrer Nähe aus.</span><span class="sxs-lookup"><span data-stu-id="fe866-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="fe866-160">Sie können den generierten Standardnamen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="fe866-160">You can keep the default generated name</span></span>

* <span data-ttu-id="fe866-161">Tippen Sie auf **Weitere Azure Services durchsuchen**, um eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-161">Tap **Explore additional Azure services** to create a new database</span></span>

![Dialogfeld „Neue Ressourcengruppe“: Bereich „Hosting“](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="fe866-163">Tippen Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-163">Tap the green **+** icon to create a new SQL Database</span></span>

![Dialogfeld „Neue Ressourcengruppe“: Bereich „Dienste“](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="fe866-165">Tippen Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **Neu...**, um einen neuen Datenbankserver zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="fe866-167">Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und tippen Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe866-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="fe866-168">Merken Sie sich den Benutzernamen und das Kennwort, den/das Sie in diesem Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="fe866-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="fe866-169">Sie können den standardmäßigen **Servernamen** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="fe866-169">You can keep the default **Server Name**</span></span>

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="fe866-171">„admin“ ist als Benutzername des Administrators nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="fe866-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="fe866-172">Tippen Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe866-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="fe866-174">Tippen Sie im Dialogfeld **App Service erstellen** auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="fe866-174">Tap **Create** on the **Create App Service** dialog</span></span>

![Dialogfeld „App Service erstellen“](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="fe866-176">Tippen Sie im Dialogfeld **Veröffentlichen** auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="fe866-176">Tap **Next** in the **Publish** dialog</span></span>

![Dialogfeld „Veröffentlichen“: Bereich „Verbindung“](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="fe866-178">Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:</span><span class="sxs-lookup"><span data-stu-id="fe866-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="fe866-179">Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.</span><span class="sxs-lookup"><span data-stu-id="fe866-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="fe866-180">Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.</span><span class="sxs-lookup"><span data-stu-id="fe866-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="fe866-181">Tippen Sie auf **Veröffentlichen**, und warten Sie, bis Visual Studio die Veröffentlichung der App abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="fe866-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="fe866-183">Visual Studio veröffentlicht die App in Azure und startet die Cloud-App in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="fe866-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="fe866-184">Testen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="fe866-184">Test your app in Azure</span></span>

* <span data-ttu-id="fe866-185">Testen Sie die Links **About** und **Contact**.</span><span class="sxs-lookup"><span data-stu-id="fe866-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="fe866-186">Registrieren eines neuen Benutzers</span><span class="sxs-lookup"><span data-stu-id="fe866-186">Register a new user</span></span>

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="fe866-188">Aktualisieren der App</span><span class="sxs-lookup"><span data-stu-id="fe866-188">Update the app</span></span>

* <span data-ttu-id="fe866-189">Bearbeiten Sie die Razor-Ansichtsdatei `Views/Home/About.cshtml`, und ändern Sie ihren Inhalt.</span><span class="sxs-lookup"><span data-stu-id="fe866-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="fe866-190">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fe866-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="fe866-191">Klicken Sie mit der rechten Maustaste auf das Projekt, und tippen Sie erneut auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="fe866-191">Right-click on the project and tap **Publish...** again</span></span>

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="fe866-193">Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="fe866-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="fe866-194">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="fe866-194">Clean up</span></span>

<span data-ttu-id="fe866-195">Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="fe866-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="fe866-196">Wählen Sie **Ressourcengruppen** aus, und tippen Sie dann auf die Ressourcengruppe, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="fe866-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="fe866-198">Tippen Sie auf dem Blatt **Ressourcengruppe** auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="fe866-198">In the **Resource group** blade, tap **Delete**</span></span>

![Azure-Portal: Blatt „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="fe866-200">Geben Sie den Namen der Ressourcengruppe ein, und tippen Sie auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="fe866-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="fe866-201">Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.</span><span class="sxs-lookup"><span data-stu-id="fe866-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="fe866-202">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="fe866-202">Next steps</span></span>

* [<span data-ttu-id="fe866-203">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe866-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="fe866-204">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe866-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="fe866-205">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="fe866-205">Fundamentals</span></span>](../fundamentals/index.md)
