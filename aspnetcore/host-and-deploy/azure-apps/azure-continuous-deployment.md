---
title: Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="6aa83-103">Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6aa83-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="6aa83-104">Von [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="6aa83-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="6aa83-105">Dieses Lernprogramm zeigt, wie eine ASP.NET Core-Web-app mit Visual Studio erstellen und Bereitstellen von Visual Studio nach Azure App Service mithilfe der kontinuierlichen Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="6aa83-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="6aa83-106">Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](/azure/app-service/app-service-web-overview) mithilfe von Visual Studio Team Services konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="6aa83-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="6aa83-107">Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer robusten Bereitstellungspipeline zum Veröffentlichen von Updates für apps, die in Azure App Service gehostet.</span><span class="sxs-lookup"><span data-stu-id="6aa83-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="6aa83-108">Die Pipeline kann konfiguriert werden, aus dem Azure-Portal erstellen, Ausführen von Tests, staging-Slot, und klicken Sie dann für die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="6aa83-109">Um dieses Lernprogramm abzuschließen, ist ein Microsoft Azure-Konto erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6aa83-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="6aa83-110">Ein Konto abrufen [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) oder [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="6aa83-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6aa83-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="6aa83-111">Prerequisites</span></span>

<span data-ttu-id="6aa83-112">In diesem Lernprogramm wird davon ausgegangen, dass die folgende Software installiert ist:</span><span class="sxs-lookup"><span data-stu-id="6aa83-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="6aa83-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6aa83-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="6aa83-114">[Git](https://git-scm.com/downloads) für Windows</span><span class="sxs-lookup"><span data-stu-id="6aa83-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="6aa83-115">Erstellen einer ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="6aa83-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="6aa83-116">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6aa83-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="6aa83-117">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="6aa83-118">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="6aa83-119">Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **.NET Core** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="6aa83-120">Benennen Sie das Projekt mit `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="6aa83-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="6aa83-121">Wählen Sie die **erstellen neue Git-Repository** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Dialogfeld "Neues Projekt"](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="6aa83-123">Wählen Sie im Dialogfeld **Neues ASP.NET Core-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="6aa83-125">Der neueste Version von .NET Core ist 2.0.</span><span class="sxs-lookup"><span data-stu-id="6aa83-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="6aa83-126">Lokales Ausführen der Web-App</span><span class="sxs-lookup"><span data-stu-id="6aa83-126">Running the web app locally</span></span>

1. <span data-ttu-id="6aa83-127">Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** > **Debuggen starten** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="6aa83-128">Als Alternative, drücken Sie die **F5**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="6aa83-129">Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment.</span><span class="sxs-lookup"><span data-stu-id="6aa83-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="6aa83-130">Sobald er abgeschlossen ist, zeigt der Browser die ausgeführte app.</span><span class="sxs-lookup"><span data-stu-id="6aa83-130">Once it's complete, the browser shows the running app.</span></span>

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="6aa83-132">Nach dem Überprüfen der ausgeführten Web-app, schließen Sie den Browser, und wählen Sie das Symbol "Debuggen beenden" auf der Symbolleiste von Visual Studio die app zu beenden.</span><span class="sxs-lookup"><span data-stu-id="6aa83-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="6aa83-133">Erstellen einer Web-App im Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="6aa83-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="6aa83-134">Die folgenden Schritte aus erstellen Web-app im Azure-Portal:</span><span class="sxs-lookup"><span data-stu-id="6aa83-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="6aa83-135">Melden Sie sich auf die [Azure-Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6aa83-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="6aa83-136">Wählen Sie **neu** am oben links auf der Portal-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="6aa83-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="6aa83-137">Wählen Sie **Web + Mobil** > **Web-App**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="6aa83-139">Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.</span><span class="sxs-lookup"><span data-stu-id="6aa83-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="6aa83-141">Die **Name des App Service** Name muss eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="6aa83-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="6aa83-142">Das Portal erzwingt diese Regel aus, wenn der Name angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="6aa83-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="6aa83-143">Wenn Sie einen anderen Wert haben, ersetzen Sie diesen Wert für jedes Auftreten der **SampleWebAppDemo** in diesem Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="6aa83-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="6aa83-144">Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="6aa83-145">Wenn Sie einen neuen Plan zu erstellen, wählen Sie den Tarif, den Speicherort und andere Optionen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="6aa83-146">Weitere Informationen zu App Service-Pläne, finden Sie unter [Azure App Service-Pläne detaillierte Übersicht über](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="6aa83-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="6aa83-147">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-147">Select **Create**.</span></span> <span data-ttu-id="6aa83-148">Azure bereitstellen und starten Sie die Web-app.</span><span class="sxs-lookup"><span data-stu-id="6aa83-148">Azure will provision and start the web app.</span></span>

   ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="6aa83-150">Aktivieren der Git-Veröffentlichung für die neue Web-App</span><span class="sxs-lookup"><span data-stu-id="6aa83-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="6aa83-151">Bei Git handelt es sich um eine verteilte Versionskontrollsystem, die zum Bereitstellen einer Azure App Service-Web-app verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6aa83-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="6aa83-152">Web-app-Code in einem lokalen Git-Repository gespeichert ist, und der Code in Azure bereitgestellt wird, indem Sie auf einem remote-Repository übertragen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="6aa83-153">Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6aa83-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="6aa83-154">Wählen Sie **Anwendungsdienste** zum Anzeigen einer Liste von der app-Dienste, die mit dem Azure-Abonnement verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="6aa83-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="6aa83-155">Wählen Sie die Web-app, die im vorherigen Abschnitt dieses Lernprogramms erstellt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="6aa83-156">Klicken Sie auf dem Blatt **Bereitstellung** auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="6aa83-158">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-158">Select **OK**.</span></span>

1. <span data-ttu-id="6aa83-159">Wenn Anmeldeinformationen für die Veröffentlichung von einer Web-app oder eine andere App Service-app für die Bereitstellung zuvor eingerichtet wurde dies nicht getan haben, richten sie nun:</span><span class="sxs-lookup"><span data-stu-id="6aa83-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="6aa83-160">Wählen Sie **Einstellungen** > **bereitstellungsanmeldeinformationen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="6aa83-161">Die **legen Sie Anmeldeinformationen für die Bereitstellung** Blatt wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="6aa83-162">Erstellen Sie einen Benutzernamen und ein Kennwort.</span><span class="sxs-lookup"><span data-stu-id="6aa83-162">Create a user name and password.</span></span> <span data-ttu-id="6aa83-163">Speichern Sie das Kennwort für die spätere Verwendung beim Einrichten von Git.</span><span class="sxs-lookup"><span data-stu-id="6aa83-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="6aa83-164">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-164">Select **Save**.</span></span>

1. <span data-ttu-id="6aa83-165">In der **Web-App** Blatt select **Einstellungen** > **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="6aa83-166">Die URL der remote Git-Repositorys zur Bereitstellung auf unterhalb **GIT-URL**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="6aa83-167">Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="6aa83-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="6aa83-169">Veröffentlichen Sie die Web-app in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6aa83-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="6aa83-170">Erstellen Sie in diesem Abschnitt ein lokales Git-Repository mit Visual Studio und Push aus diesem Repository in Azure zum Bereitstellen der Web-app.</span><span class="sxs-lookup"><span data-stu-id="6aa83-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="6aa83-171">Folgende Schritte müssen ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="6aa83-171">The steps involved include the following:</span></span>

* <span data-ttu-id="6aa83-172">Fügen Sie die remote-Repository-Einstellung, die mit den GIT-URL-Wert, damit das lokale Repository in Azure bereitgestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6aa83-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="6aa83-173">Festgeschrieben Sie projektänderungen werden.</span><span class="sxs-lookup"><span data-stu-id="6aa83-173">Commit project changes.</span></span>
* <span data-ttu-id="6aa83-174">Push projektänderungen aus dem lokalen Repository in das Remoterepository in Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa83-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="6aa83-175">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="6aa83-176">Die **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-176">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="6aa83-178">Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="6aa83-179">In der **entfernten Datenbanken** Teil der **-Repositoryeinstellungen werden**Option **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="6aa83-180">Die **Remote hinzufügen** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="6aa83-181">Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.</span><span class="sxs-lookup"><span data-stu-id="6aa83-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="6aa83-182">Legen Sie den Wert für **Fetch** auf die **Git-URL** , die zuvor in diesem Lernprogramm aus Azure kopiert.</span><span class="sxs-lookup"><span data-stu-id="6aa83-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="6aa83-183">Beachten Sie, dass dies die URL ist, die mit **.git** endet.</span><span class="sxs-lookup"><span data-stu-id="6aa83-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="6aa83-185">Geben Sie als Alternative, die remote-Repository aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie den Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="6aa83-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="6aa83-186">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6aa83-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="6aa83-187">Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="6aa83-188">Vergewissern Sie sich, dass der Name und e-Mail-Adresse festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="6aa83-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="6aa83-189">Wählen Sie **Update** bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="6aa83-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="6aa83-190">Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="6aa83-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="6aa83-191">Geben Sie eine Commit-Nachricht, z. B. **anfängliche Push Nr. 1** , und wählen Sie **Commit**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="6aa83-192">Diese Aktion erstellt eine *Commit* lokal.</span><span class="sxs-lookup"><span data-stu-id="6aa83-192">This action creates a *commit* locally.</span></span>

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="6aa83-194">Als Alternative können Commit ändert sich von der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie die Git-Befehle eingeben.</span><span class="sxs-lookup"><span data-stu-id="6aa83-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="6aa83-195">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6aa83-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="6aa83-196">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="6aa83-197">Der Eingabeaufforderung in das Projektverzeichnis wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6aa83-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="6aa83-198">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="6aa83-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="6aa83-199">Geben Sie den Azure **bereitstellungsanmeldeinformationen** Kennwort, die zuvor in Azure erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="6aa83-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="6aa83-200">Dieser Befehl startet den Prozess der lokalen Projektdateien auf Azure zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="6aa83-201">Die Ausgabe der obige Befehl endet mit einer Meldung, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="6aa83-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="6aa83-202">Wenn Zusammenarbeit für das Projekt erforderlich ist, sollten Sie erwägen, pushen an [GitHub](https://github.com) vor dem auf Azure übertragen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="6aa83-203">Überprüfen der aktiven Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="6aa83-203">Verify the Active Deployment</span></span>

<span data-ttu-id="6aa83-204">Stellen Sie sicher, dass die Web-app-Übertragung von der lokalen Umgebung in Azure erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="6aa83-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="6aa83-205">In der [Azure-Portal](https://portal.azure.com), wählen Sie die Web-app.</span><span class="sxs-lookup"><span data-stu-id="6aa83-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="6aa83-206">Wählen Sie **Bereitstellung** > **Bereitstellungsoptionen**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-206">Select **Deployment** > **Deployment options**.</span></span>

![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="6aa83-208">Ausführen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="6aa83-208">Run the app in Azure</span></span>

<span data-ttu-id="6aa83-209">Nun, dass die Web-app in Azure bereitgestellt wird, führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="6aa83-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="6aa83-210">Dies kann auf zwei Arten erfolgen:</span><span class="sxs-lookup"><span data-stu-id="6aa83-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="6aa83-211">Suchen Sie im Azure-Portal Blatt für die Web-app für die Web-app ein.</span><span class="sxs-lookup"><span data-stu-id="6aa83-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="6aa83-212">Wählen Sie **Durchsuchen** um die app in der Standard-Webbrowser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="6aa83-213">Öffnen Sie einen Browser, und geben Sie die URL für die Web-app.</span><span class="sxs-lookup"><span data-stu-id="6aa83-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="6aa83-214">Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="6aa83-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="6aa83-215">Die Web-app aktualisieren und erneut veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="6aa83-215">Update the web app and republish</span></span>

<span data-ttu-id="6aa83-216">Nach Änderungen an den lokalen Code zu veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="6aa83-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="6aa83-217">Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6aa83-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="6aa83-218">Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="6aa83-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="6aa83-219">Speichern Sie die Änderungen zu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6aa83-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="6aa83-220">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="6aa83-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="6aa83-221">Die **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6aa83-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="6aa83-222">Geben Sie eine Commit-Nachricht, z. B. `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="6aa83-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="6aa83-223">Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.</span><span class="sxs-lookup"><span data-stu-id="6aa83-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="6aa83-224">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="6aa83-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="6aa83-225">Als Alternative können mithilfe von Push übertragen die Änderungen aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie einen Git-Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="6aa83-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="6aa83-226">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6aa83-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="6aa83-227">Anzeigen der aktualisierten Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="6aa83-227">View the updated web app in Azure</span></span>

<span data-ttu-id="6aa83-228">Die aktualisierte Web-app anzeigen, indem er **Durchsuchen** aus dem Blade der Web-app in der Azure-Verwaltungsportal oder durch einen Browser öffnen und die URL für die Web-app eingeben.</span><span class="sxs-lookup"><span data-stu-id="6aa83-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="6aa83-229">Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="6aa83-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6aa83-230">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6aa83-230">Additional resources</span></span>

* [<span data-ttu-id="6aa83-231">Verwenden Sie zum Erstellen und Veröffentlichen einer Azure-Web-App mit Continuous Deployment VSTS</span><span class="sxs-lookup"><span data-stu-id="6aa83-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="6aa83-232">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="6aa83-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
