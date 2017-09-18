---
title: Continuous Deployment in Azure mit Visual Studio und Git
author: rick-anderson
description: "Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwenden von Git für Continuous Deployment in Azure App Service bereitstellen."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="26fcf-104">Continuous Deployment in Azure für ASP.NET Core mit Visual Studio und Git</span><span class="sxs-lookup"><span data-stu-id="26fcf-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="26fcf-105">Von [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="26fcf-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="26fcf-106">Dieses Tutorial zeigt, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie aus Visual Studio mithilfe von Continuous Deployment in Azure App Service bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="26fcf-107">Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) mithilfe von Visual Studio Team Services konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="26fcf-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="26fcf-108">Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer zuverlässigen Bereitstellungspipeline zum Veröffentlichen von Updates für Ihre App in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="26fcf-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="26fcf-109">Die Pipeline kann im Azure-Portal für die folgenden Aufgaben konfiguriert werden: Erstellen von Builds, Ausführen von Tests, Bereitstellen in einem Stagingslot und anschließendes Bereitstellen in der Produktion.</span><span class="sxs-lookup"><span data-stu-id="26fcf-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="26fcf-110">Für dieses Tutorial benötigen Sie ein Microsoft Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="26fcf-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="26fcf-111">Wenn Sie kein Konto haben, können Sie [Ihre Leistungen für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oder [sich für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="26fcf-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26fcf-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="26fcf-112">Prerequisites</span></span>

<span data-ttu-id="26fcf-113">In diesem Tutorial wird davon ausgegangen, dass Sie Folgendes bereits installiert haben:</span><span class="sxs-lookup"><span data-stu-id="26fcf-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="26fcf-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26fcf-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="26fcf-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (Laufzeit und Tools)</span><span class="sxs-lookup"><span data-stu-id="26fcf-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime and tooling)</span></span>

* <span data-ttu-id="26fcf-116">[Git](https://git-scm.com/downloads) für Windows</span><span class="sxs-lookup"><span data-stu-id="26fcf-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="26fcf-117">Erstellen einer ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="26fcf-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="26fcf-118">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26fcf-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="26fcf-119">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="26fcf-120">Wählen Sie die Projektvorlage **ASP.NET-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="26fcf-121">Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **Web** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="26fcf-122">Benennen Sie das Projekt mit `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="26fcf-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="26fcf-123">Wählen Sie **Neues Git-Repository** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Dialogfeld "Neues Projekt"](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="26fcf-125">Wählen Sie im Dialogfeld **Neues ASP.NET-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="26fcf-127">Lokales Ausführen der Web-App</span><span class="sxs-lookup"><span data-stu-id="26fcf-127">Running the web app locally</span></span>

1. <span data-ttu-id="26fcf-128">Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** -> **Debuggen starten** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="26fcf-129">Alternativ können Sie **F5** drücken.</span><span class="sxs-lookup"><span data-stu-id="26fcf-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="26fcf-130">Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment.</span><span class="sxs-lookup"><span data-stu-id="26fcf-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="26fcf-131">Sobald es erfolgt ist, wird die ausgeführte App im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-131">Once it is complete, the browser will show the running app.</span></span>

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="26fcf-133">Nach Überprüfen der ausgeführten Web-App schließen Sie den Browser und klicken auf der Symbolleiste von Visual Studio auf das Symbol „Debuggen beenden“, um die App zu beenden.</span><span class="sxs-lookup"><span data-stu-id="26fcf-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="26fcf-134">Erstellen einer Web-App im Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="26fcf-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="26fcf-135">Die folgenden Schritte führen Sie durch die Erstellung einer Web-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="26fcf-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="26fcf-136">Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.</span><span class="sxs-lookup"><span data-stu-id="26fcf-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="26fcf-137">Tippen Sie links oben im Portal auf **NEU**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="26fcf-138">Tippen Sie auf **Web + Mobil** > **Web-App**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="26fcf-140">Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.</span><span class="sxs-lookup"><span data-stu-id="26fcf-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="26fcf-142">Der in **App Service-Name** eingegebene Name muss eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="26fcf-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="26fcf-143">Das Portal erzwingt diese Regel, wenn Sie versuchen, den Namen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="26fcf-144">Nachdem Sie einen anderen Wert eingegeben haben, müssen Sie jedes Vorkommen von **SampleWebAppDemo** in diesem Tutorial durch diesen Wert ersetzen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="26fcf-145">Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="26fcf-146">Wenn Sie einen neuen Plan erstellen, wählen Sie den Tarif, den Standort und andere Optionen aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="26fcf-147">Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="26fcf-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="26fcf-148">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-148">Click **Create**.</span></span> <span data-ttu-id="26fcf-149">Azure stellt Ihre Web-App bereit und startet sie.</span><span class="sxs-lookup"><span data-stu-id="26fcf-149">Azure will provision and start your web app.</span></span>

    ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="26fcf-151">Aktivieren der Git-Veröffentlichung für die neue Web-App</span><span class="sxs-lookup"><span data-stu-id="26fcf-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="26fcf-152">Bei Git handelt es sich um ein verteiltes Versionskontrollsystem, mit dem Sie Ihre Azure App Service-Web-App bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="26fcf-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="26fcf-153">Sie speichern den Code, den Sie für Ihre Web-App schreiben, in einem lokalen Git-Repository, und stellen Ihren Code in Azure bereit, indem Sie ihn mithilfe von Push in ein Remoterepository übertragen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="26fcf-154">Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, sofern Sie noch nicht angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="26fcf-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="26fcf-155">Klicken Sie unten im Navigationsbereich auf **Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="26fcf-156">Klicken Sie auf **Web-Apps**, um eine Liste von Web-Apps anzuzeigen, die Ihrem Azure-Abonnement zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="26fcf-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="26fcf-157">Wählen Sie die Web-App aus, die Sie im vorherigen Abschnitt dieses Tutorials erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="26fcf-158">Wenn das Blatt **Einstellungen** nicht angezeigt wird, wählen Sie **Einstellungen** auf dem Blatt **Web-App** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="26fcf-159">Klicken Sie auf dem Blatt **Einstellungen** auf **Bereitstellungsquelle** > **Quelle auswählen** > **Lokales Git-Repository**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="26fcf-161">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-161">Click **OK**.</span></span>

8. <span data-ttu-id="26fcf-162">Wenn Sie zuvor keine Anmeldeinformationen für die Bereitstellung zum Veröffentlichen einer Web-App oder anderen App Service-App festgelegt haben, richten Sie diese nun ein:</span><span class="sxs-lookup"><span data-stu-id="26fcf-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="26fcf-163">Klicken Sie auf **Einstellungen** > **Anmeldeinformationen für die Bereitstellung**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="26fcf-164">Das Blatt **Anmeldeinformationen für die Bereitstellung festlegen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="26fcf-165">Erstellen Sie einen Benutzernamen und ein Kennwort.</span><span class="sxs-lookup"><span data-stu-id="26fcf-165">Create a user name and password.</span></span>  <span data-ttu-id="26fcf-166">Sie benötigen dieses Kennwort später bei der Einrichtung von Git.</span><span class="sxs-lookup"><span data-stu-id="26fcf-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="26fcf-167">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-167">Click **Save**.</span></span>

9. <span data-ttu-id="26fcf-168">Klicken Sie auf dem Blatt **Web-App** auf **Einstellungen** > **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="26fcf-169">Die URL des Git-Remoterepositorys, in dem die Bereitstellung erfolgt, wird unter **GIT-URL** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="26fcf-170">Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="26fcf-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="26fcf-172">Veröffentlichen der App in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="26fcf-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="26fcf-173">In diesem Abschnitt erstellen Sie ein lokales Git-Repository mit Visual Studio und führen einen Pushvorgang aus diesem Repository in Azure aus, um Ihre Web-App bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="26fcf-174">Folgende Schritte müssen ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="26fcf-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="26fcf-175">Fügen Sie die Einstellung des Remoterepositorys mithilfe Ihres GIT-URL-Werts hinzu, damit Sie Ihr lokales Repository in Azure bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="26fcf-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="26fcf-176">Führen Sie für Ihre Projektänderungen einen Commit aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-176">Commit your project changes.</span></span>

   * <span data-ttu-id="26fcf-177">Übertragen Sie Ihre Projektänderungen mithilfe von Push aus Ihrem lokalen Repository in Ihr Remoterepository.</span><span class="sxs-lookup"><span data-stu-id="26fcf-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="26fcf-178">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="26fcf-179">Der **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-179">The **Team Explorer** will be displayed.</span></span>

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="26fcf-181">Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="26fcf-182">Klicken Sie in **Repositoryeinstellungen** im Abschnitt **Remotes** auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="26fcf-183">Das Dialogfeld **Remote hinzufügen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="26fcf-184">Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.</span><span class="sxs-lookup"><span data-stu-id="26fcf-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="26fcf-185">Legen Sie den Wert für **Fetch** auf die **Git-URL** fest, die Sie zuvor in diesem Tutorial aus Azure kopiert haben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="26fcf-186">Beachten Sie, dass dies die URL ist, die mit **.git** endet.</span><span class="sxs-lookup"><span data-stu-id="26fcf-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="26fcf-188">Als Alternative können Sie das Remoterepository im **Befehlsfenster** angeben, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und den Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="26fcf-189">Beispiel: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="26fcf-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="26fcf-190">Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="26fcf-191">Vergewissern Sie sicher, dass Sie Ihren Namen und Ihre E-Mail-Adresse festgelegt haben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="26fcf-192">Sie müssen möglicherweise auch **Aktualisieren** auswählen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="26fcf-193">Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="26fcf-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="26fcf-194">Geben Sie eine Commit-Nachricht ein, z.B. **Anfänglicher Push Nr. 1**, und klicken Sie auf **Commit ausführen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="26fcf-195">Diese Aktion erstellt einen lokalen *Commit*.</span><span class="sxs-lookup"><span data-stu-id="26fcf-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="26fcf-196">Als Nächstes müssen Sie eine *Synchronisierung* mit Azure durchführen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-196">Next, you need to *sync* with Azure.</span></span>

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="26fcf-198">Als Alternative können Sie für Ihre Änderungen im **Befehlsfenster** einen Commit ausführen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und die Git-Befehle eingeben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="26fcf-199">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="26fcf-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="26fcf-200">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="26fcf-201">Die Eingabeaufforderung wird in Ihrem Projektverzeichnis geöffnet.</span><span class="sxs-lookup"><span data-stu-id="26fcf-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="26fcf-202">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="26fcf-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="26fcf-203">Geben Sie das Kennwort Ihrer Azure-**Anmeldeinformationen für die Bereitstellung** ein, das Sie zuvor in Azure erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="26fcf-204">Ihr Kennwort wird bei der Eingabe nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="26fcf-205">Über diesen Befehl startet das Übertragen Ihrer lokalen Projektdateien mithilfe von Push in Azure.</span><span class="sxs-lookup"><span data-stu-id="26fcf-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="26fcf-206">Die Ausgabe des obigen Befehls endet mit der Meldung, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="26fcf-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="26fcf-207">Wenn Sie an einem Projekt mit anderen zusammenarbeiten müssen, sollten Sie zwischen den Pushübertragungen in Azure eine Pushübertragung auf [GitHub](https://github.com) erwägen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="26fcf-208">Überprüfen der aktiven Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="26fcf-208">Verify the Active Deployment</span></span>

<span data-ttu-id="26fcf-209">Sie können überprüfen, ob Sie die Web-App erfolgreich aus Ihrer lokalen Umgebung in Azure übertragen haben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="26fcf-210">Sie sehen, dass die erfolgreiche Bereitstellung aufgeführt ist.</span><span class="sxs-lookup"><span data-stu-id="26fcf-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="26fcf-211">Wählen Sie Ihre Web-App im [Azure-Portal](https://portal.azure.com) aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="26fcf-212">Klicken Sie dann auf **Einstellungen** > **Continuous Deployment**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="26fcf-214">Ausführen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="26fcf-214">Run the app in Azure</span></span>

<span data-ttu-id="26fcf-215">Nachdem Sie Ihre Web-App in Azure bereitgestellt haben, können Sie die App ausführen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="26fcf-216">Dazu gibt es zwei Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="26fcf-216">This can be done in two ways:</span></span>

* <span data-ttu-id="26fcf-217">Wechseln Sie im Azure-Portal zum Blatt „Web-App“ Ihrer Web-App, und klicken Sie auf **Durchsuchen**, um Ihre App in Ihrem Standardbrowser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="26fcf-218">Öffnen Sie einen Browser, und geben Sie die URL Ihrer Web-App ein.</span><span class="sxs-lookup"><span data-stu-id="26fcf-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="26fcf-219">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="26fcf-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="26fcf-220">Aktualisieren Ihrer Web-App und erneutes Veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="26fcf-220">Update your web app and republish</span></span>

<span data-ttu-id="26fcf-221">Nachdem Sie Änderungen am lokalen Code vorgenommen haben, können Sie sie erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="26fcf-222">Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="26fcf-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="26fcf-223">Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="26fcf-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="26fcf-224">Speichern Sie die Änderungen an *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="26fcf-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="26fcf-225">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="26fcf-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="26fcf-226">Der **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26fcf-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="26fcf-227">Geben Sie eine Commit-Nachricht ein. Beispiel:</span><span class="sxs-lookup"><span data-stu-id="26fcf-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="26fcf-228">Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.</span><span class="sxs-lookup"><span data-stu-id="26fcf-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="26fcf-229">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="26fcf-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="26fcf-230">Als Alternative können Sie Ihre Änderungen im **Befehlsfenster** mithilfe von Push übertragen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und einen Git-Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="26fcf-231">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="26fcf-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="26fcf-232">Anzeigen der aktualisierten Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="26fcf-232">View the updated web app in Azure</span></span>

<span data-ttu-id="26fcf-233">Zeigen Sie Ihre aktualisierte Web-App an, indem Sie im Azure-Portal auf dem Blatt „Web-App“ **Durchsuchen** auswählen oder einen Browser öffnen und die URL der Web-App eingeben.</span><span class="sxs-lookup"><span data-stu-id="26fcf-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="26fcf-234">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="26fcf-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="26fcf-235">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="26fcf-235">Additional Resources</span></span>

* [<span data-ttu-id="26fcf-236">Veröffentlichung und Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="26fcf-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="26fcf-237">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="26fcf-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
