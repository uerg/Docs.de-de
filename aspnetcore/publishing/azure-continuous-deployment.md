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
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="f66a0-104">Continuous Deployment in Azure für ASP.NET Core mit Visual Studio und Git</span><span class="sxs-lookup"><span data-stu-id="f66a0-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="f66a0-105">Von [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="f66a0-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="f66a0-106">Dieses Tutorial zeigt, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie aus Visual Studio mithilfe von Continuous Deployment in Azure App Service bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="f66a0-107">Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) mithilfe von Visual Studio Team Services konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="f66a0-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="f66a0-108">Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer zuverlässigen Bereitstellungspipeline zum Veröffentlichen von Updates für Ihre App in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f66a0-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="f66a0-109">Die Pipeline kann im Azure-Portal für die folgenden Aufgaben konfiguriert werden: Erstellen von Builds, Ausführen von Tests, Bereitstellen in einem Stagingslot und anschließendes Bereitstellen in der Produktion.</span><span class="sxs-lookup"><span data-stu-id="f66a0-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="f66a0-110">Für dieses Tutorial benötigen Sie ein Microsoft Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="f66a0-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="f66a0-111">Wenn Sie kein Konto haben, können Sie [Ihre Leistungen für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oder [sich für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f66a0-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f66a0-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="f66a0-112">Prerequisites</span></span>

<span data-ttu-id="f66a0-113">In diesem Tutorial wird davon ausgegangen, dass Sie Folgendes bereits installiert haben:</span><span class="sxs-lookup"><span data-stu-id="f66a0-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="f66a0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f66a0-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="f66a0-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (Laufzeit und Tools)</span><span class="sxs-lookup"><span data-stu-id="f66a0-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="f66a0-116">[Git](https://git-scm.com/downloads) für Windows</span><span class="sxs-lookup"><span data-stu-id="f66a0-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="f66a0-117">Erstellen einer ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="f66a0-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="f66a0-118">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f66a0-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="f66a0-119">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="f66a0-120">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="f66a0-121">Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **.NET Core** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="f66a0-122">Benennen Sie das Projekt mit `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="f66a0-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="f66a0-123">Wählen Sie **Neues Git-Repository** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Dialogfeld "Neues Projekt"](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="f66a0-125">Wählen Sie im Dialogfeld **Neues ASP.NET Core-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="f66a0-127">Das neuste Release von .NET Core ist 2.0.</span><span class="sxs-lookup"><span data-stu-id="f66a0-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="f66a0-128">Lokales Ausführen der Web-App</span><span class="sxs-lookup"><span data-stu-id="f66a0-128">Running the web app locally</span></span>

1. <span data-ttu-id="f66a0-129">Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** -> **Debuggen starten** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="f66a0-130">Alternativ können Sie **F5** drücken.</span><span class="sxs-lookup"><span data-stu-id="f66a0-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="f66a0-131">Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment.</span><span class="sxs-lookup"><span data-stu-id="f66a0-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="f66a0-132">Sobald es erfolgt ist, wird die ausgeführte App im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-132">Once it is complete, the browser will show the running app.</span></span>

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="f66a0-134">Nach Überprüfen der ausgeführten Web-App schließen Sie den Browser und klicken auf der Symbolleiste von Visual Studio auf das Symbol „Debuggen beenden“, um die App zu beenden.</span><span class="sxs-lookup"><span data-stu-id="f66a0-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="f66a0-135">Erstellen einer Web-App im Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="f66a0-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="f66a0-136">Die folgenden Schritte führen Sie durch die Erstellung einer Web-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="f66a0-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="f66a0-137">Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.</span><span class="sxs-lookup"><span data-stu-id="f66a0-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="f66a0-138">Tippen Sie links oben im Portal auf **NEU**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="f66a0-139">Tippen Sie auf **Web + Mobil** > **Web-App**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="f66a0-141">Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.</span><span class="sxs-lookup"><span data-stu-id="f66a0-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="f66a0-143">Der in **App Service-Name** eingegebene Name muss eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="f66a0-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="f66a0-144">Das Portal erzwingt diese Regel, wenn Sie versuchen, den Namen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="f66a0-145">Nachdem Sie einen anderen Wert eingegeben haben, müssen Sie jedes Vorkommen von **SampleWebAppDemo** in diesem Tutorial durch diesen Wert ersetzen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="f66a0-146">Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="f66a0-147">Wenn Sie einen neuen Plan erstellen, wählen Sie den Tarif, den Standort und andere Optionen aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="f66a0-148">Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="f66a0-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="f66a0-149">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-149">Click **Create**.</span></span> <span data-ttu-id="f66a0-150">Azure stellt Ihre Web-App bereit und startet sie.</span><span class="sxs-lookup"><span data-stu-id="f66a0-150">Azure will provision and start your web app.</span></span>

    ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="f66a0-152">Aktivieren der Git-Veröffentlichung für die neue Web-App</span><span class="sxs-lookup"><span data-stu-id="f66a0-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="f66a0-153">Bei Git handelt es sich um ein verteiltes Versionskontrollsystem, mit dem Sie Ihre Azure App Service-Web-App bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="f66a0-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="f66a0-154">Sie speichern den Code, den Sie für Ihre Web-App schreiben, in einem lokalen Git-Repository, und stellen Ihren Code in Azure bereit, indem Sie ihn mithilfe von Push in ein Remoterepository übertragen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="f66a0-155">Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, sofern Sie noch nicht angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="f66a0-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="f66a0-156">Klicken Sie auf **App Services**, um eine Liste von App Services anzuzeigen, die Ihrem Azure-Abonnement zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="f66a0-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="f66a0-157">Wählen Sie die Web-App aus, die Sie im vorherigen Abschnitt dieses Tutorials erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="f66a0-158">Klicken Sie auf dem Blatt **Bereitstellung** auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="f66a0-160">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-160">Click **OK**.</span></span>

6. <span data-ttu-id="f66a0-161">Wenn Sie zuvor keine Anmeldeinformationen für die Bereitstellung zum Veröffentlichen einer Web-App oder anderen App Service-App festgelegt haben, richten Sie diese nun ein:</span><span class="sxs-lookup"><span data-stu-id="f66a0-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="f66a0-162">Klicken Sie auf **Einstellungen** > **Anmeldeinformationen für die Bereitstellung**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="f66a0-163">Das Blatt **Anmeldeinformationen für die Bereitstellung festlegen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="f66a0-164">Erstellen Sie einen Benutzernamen und ein Kennwort.</span><span class="sxs-lookup"><span data-stu-id="f66a0-164">Create a user name and password.</span></span>  <span data-ttu-id="f66a0-165">Sie benötigen dieses Kennwort später bei der Einrichtung von Git.</span><span class="sxs-lookup"><span data-stu-id="f66a0-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="f66a0-166">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-166">Click **Save**.</span></span>

7. <span data-ttu-id="f66a0-167">Klicken Sie auf dem Blatt **Web-App** auf **Einstellungen** > **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="f66a0-168">Die URL des Git-Remoterepositorys, in dem die Bereitstellung erfolgt, wird unter **GIT-URL** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="f66a0-169">Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="f66a0-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="f66a0-171">Veröffentlichen der App in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f66a0-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="f66a0-172">In diesem Abschnitt erstellen Sie ein lokales Git-Repository mit Visual Studio und führen einen Pushvorgang aus diesem Repository in Azure aus, um Ihre Web-App bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="f66a0-173">Folgende Schritte müssen ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="f66a0-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="f66a0-174">Fügen Sie die Einstellung des Remoterepositorys mithilfe Ihres GIT-URL-Werts hinzu, damit Sie Ihr lokales Repository in Azure bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="f66a0-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="f66a0-175">Führen Sie für Ihre Projektänderungen einen Commit aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-175">Commit your project changes.</span></span>

   * <span data-ttu-id="f66a0-176">Übertragen Sie Ihre Projektänderungen mithilfe von Push aus Ihrem lokalen Repository in Ihr Remoterepository.</span><span class="sxs-lookup"><span data-stu-id="f66a0-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="f66a0-177">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="f66a0-178">Der **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-178">The **Team Explorer** will be displayed.</span></span>

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="f66a0-180">Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="f66a0-181">Klicken Sie in **Repositoryeinstellungen** im Abschnitt **Remotes** auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="f66a0-182">Das Dialogfeld **Remote hinzufügen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="f66a0-183">Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.</span><span class="sxs-lookup"><span data-stu-id="f66a0-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="f66a0-184">Legen Sie den Wert für **Fetch** auf die **Git-URL** fest, die Sie zuvor in diesem Tutorial aus Azure kopiert haben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="f66a0-185">Beachten Sie, dass dies die URL ist, die mit **.git** endet.</span><span class="sxs-lookup"><span data-stu-id="f66a0-185">Note that this is the URL that ends with **.git**.</span></span>

    ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="f66a0-187">Als Alternative können Sie das Remoterepository im **Befehlsfenster** angeben, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und den Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="f66a0-188">Beispiel: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="f66a0-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="f66a0-189">Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="f66a0-190">Vergewissern Sie sicher, dass Sie Ihren Namen und Ihre E-Mail-Adresse festgelegt haben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="f66a0-191">Sie müssen möglicherweise auch **Aktualisieren** auswählen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="f66a0-192">Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="f66a0-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="f66a0-193">Geben Sie eine Commit-Nachricht ein, z.B. **Anfänglicher Push Nr. 1**, und klicken Sie auf **Commit ausführen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="f66a0-194">Diese Aktion erstellt einen lokalen *Commit*.</span><span class="sxs-lookup"><span data-stu-id="f66a0-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="f66a0-195">Als Nächstes müssen Sie eine *Synchronisierung* mit Azure durchführen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-195">Next, you need to *sync* with Azure.</span></span>

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="f66a0-197">Als Alternative können Sie für Ihre Änderungen im **Befehlsfenster** einen Commit ausführen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und die Git-Befehle eingeben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="f66a0-198">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f66a0-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="f66a0-199">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="f66a0-200">Die Eingabeaufforderung wird in Ihrem Projektverzeichnis geöffnet.</span><span class="sxs-lookup"><span data-stu-id="f66a0-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="f66a0-201">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="f66a0-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="f66a0-202">Geben Sie das Kennwort Ihrer Azure-**Anmeldeinformationen für die Bereitstellung** ein, das Sie zuvor in Azure erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f66a0-203">Ihr Kennwort wird bei der Eingabe nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="f66a0-204">Über diesen Befehl startet das Übertragen Ihrer lokalen Projektdateien mithilfe von Push in Azure.</span><span class="sxs-lookup"><span data-stu-id="f66a0-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="f66a0-205">Die Ausgabe des obigen Befehls endet mit der Meldung, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="f66a0-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="f66a0-206">Wenn Sie an einem Projekt mit anderen zusammenarbeiten müssen, sollten Sie zwischen den Pushübertragungen in Azure eine Pushübertragung auf [GitHub](https://github.com) erwägen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="f66a0-207">Überprüfen der aktiven Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="f66a0-207">Verify the Active Deployment</span></span>

<span data-ttu-id="f66a0-208">Sie können überprüfen, ob Sie die Web-App erfolgreich aus Ihrer lokalen Umgebung in Azure übertragen haben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="f66a0-209">Sie sehen, dass die erfolgreiche Bereitstellung aufgeführt ist.</span><span class="sxs-lookup"><span data-stu-id="f66a0-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="f66a0-210">Wählen Sie Ihre Web-App im [Azure-Portal](https://portal.azure.com) aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="f66a0-211">Wählen Sie dann **Bereitstellung** > **Bereitstellungsoptionen** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="f66a0-213">Ausführen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="f66a0-213">Run the app in Azure</span></span>

<span data-ttu-id="f66a0-214">Nachdem Sie Ihre Web-App in Azure bereitgestellt haben, können Sie die App ausführen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="f66a0-215">Dazu gibt es zwei Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="f66a0-215">This can be done in two ways:</span></span>

* <span data-ttu-id="f66a0-216">Wechseln Sie im Azure-Portal zum Blatt „Web-App“ Ihrer Web-App, und klicken Sie auf **Durchsuchen**, um Ihre App in Ihrem Standardbrowser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="f66a0-217">Öffnen Sie einen Browser, und geben Sie die URL Ihrer Web-App ein.</span><span class="sxs-lookup"><span data-stu-id="f66a0-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="f66a0-218">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f66a0-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="f66a0-219">Aktualisieren Ihrer Web-App und erneutes Veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="f66a0-219">Update your web app and republish</span></span>

<span data-ttu-id="f66a0-220">Nachdem Sie Änderungen am lokalen Code vorgenommen haben, können Sie sie erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="f66a0-221">Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f66a0-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="f66a0-222">Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="f66a0-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="f66a0-223">Speichern Sie die Änderungen an *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f66a0-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="f66a0-224">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="f66a0-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="f66a0-225">Der **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f66a0-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="f66a0-226">Geben Sie eine Commit-Nachricht ein. Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f66a0-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="f66a0-227">Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.</span><span class="sxs-lookup"><span data-stu-id="f66a0-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="f66a0-228">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="f66a0-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="f66a0-229">Als Alternative können Sie Ihre Änderungen im **Befehlsfenster** mithilfe von Push übertragen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und einen Git-Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="f66a0-230">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f66a0-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="f66a0-231">Anzeigen der aktualisierten Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="f66a0-231">View the updated web app in Azure</span></span>

<span data-ttu-id="f66a0-232">Zeigen Sie Ihre aktualisierte Web-App an, indem Sie im Azure-Portal auf dem Blatt „Web-App“ **Durchsuchen** auswählen oder einen Browser öffnen und die URL der Web-App eingeben.</span><span class="sxs-lookup"><span data-stu-id="f66a0-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="f66a0-233">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f66a0-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="f66a0-234">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f66a0-234">Additional Resources</span></span>

* [<span data-ttu-id="f66a0-235">Veröffentlichung und Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="f66a0-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="f66a0-236">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="f66a0-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
