---
title: Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core
author: rick-anderson
description: "Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="29947-103">Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29947-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="29947-104">Von [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="29947-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="29947-105">Dieses Lernprogramm zeigt, wie eine ASP.NET Core-Web-app mit Visual Studio erstellen und Bereitstellen von Visual Studio nach Azure App Service mithilfe der kontinuierlichen Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="29947-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="29947-106">Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](/azure/app-service/app-service-web-overview) mithilfe von Visual Studio Team Services konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="29947-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="29947-107">Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer robusten Bereitstellungspipeline zum Veröffentlichen von Updates für apps, die in Azure App Service gehostet.</span><span class="sxs-lookup"><span data-stu-id="29947-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="29947-108">Die Pipeline kann konfiguriert werden, aus dem Azure-Portal erstellen, Ausführen von Tests, staging-Slot, und klicken Sie dann für die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="29947-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="29947-109">Um dieses Lernprogramm abzuschließen, ist ein Microsoft Azure-Konto erforderlich.</span><span class="sxs-lookup"><span data-stu-id="29947-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="29947-110">Ein Konto abrufen [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) oder [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="29947-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29947-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="29947-111">Prerequisites</span></span>

<span data-ttu-id="29947-112">In diesem Lernprogramm wird davon ausgegangen, dass die folgende Software installiert ist:</span><span class="sxs-lookup"><span data-stu-id="29947-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="29947-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29947-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="29947-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (Common Language Runtime und die Tools)</span><span class="sxs-lookup"><span data-stu-id="29947-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="29947-115">[Git](https://git-scm.com/downloads) für Windows</span><span class="sxs-lookup"><span data-stu-id="29947-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="29947-116">Erstellen einer ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="29947-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="29947-117">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29947-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="29947-118">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="29947-119">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="29947-120">Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **.NET Core** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29947-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="29947-121">Benennen Sie das Projekt mit `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="29947-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="29947-122">Wählen Sie die **erstellen neue Git-Repository** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="29947-122">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Dialogfeld "Neues Projekt"](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="29947-124">Wählen Sie im Dialogfeld **Neues ASP.NET Core-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="29947-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="29947-126">Der neueste Version von .NET Core ist 2.0.</span><span class="sxs-lookup"><span data-stu-id="29947-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="29947-127">Lokales Ausführen der Web-App</span><span class="sxs-lookup"><span data-stu-id="29947-127">Running the web app locally</span></span>

1. <span data-ttu-id="29947-128">Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** > **Debuggen starten** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="29947-129">Als Alternative, drücken Sie die **F5**.</span><span class="sxs-lookup"><span data-stu-id="29947-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="29947-130">Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment.</span><span class="sxs-lookup"><span data-stu-id="29947-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="29947-131">Sobald er abgeschlossen ist, zeigt der Browser die ausgeführte app.</span><span class="sxs-lookup"><span data-stu-id="29947-131">Once it's complete, the browser shows the running app.</span></span>

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="29947-133">Nach dem Überprüfen der ausgeführten Web-app, schließen Sie den Browser, und wählen Sie das Symbol "Debuggen beenden" auf der Symbolleiste von Visual Studio die app zu beenden.</span><span class="sxs-lookup"><span data-stu-id="29947-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="29947-134">Erstellen einer Web-App im Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="29947-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="29947-135">Die folgenden Schritte aus erstellen Web-app im Azure-Portal:</span><span class="sxs-lookup"><span data-stu-id="29947-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="29947-136">Melden Sie sich auf die [Azure-Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="29947-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="29947-137">Wählen Sie **neu** am oben links auf der Portal-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="29947-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="29947-138">Wählen Sie **Web + Mobil** > **Web-App**.</span><span class="sxs-lookup"><span data-stu-id="29947-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="29947-140">Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.</span><span class="sxs-lookup"><span data-stu-id="29947-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="29947-142">Die **Name des App Service** Name muss eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="29947-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="29947-143">Das Portal erzwingt diese Regel aus, wenn der Name angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="29947-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="29947-144">Wenn Sie einen anderen Wert haben, ersetzen Sie diesen Wert für jedes Auftreten der **SampleWebAppDemo** in diesem Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="29947-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="29947-145">Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen.</span><span class="sxs-lookup"><span data-stu-id="29947-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="29947-146">Wenn Sie einen neuen Plan zu erstellen, wählen Sie den Tarif, den Speicherort und andere Optionen.</span><span class="sxs-lookup"><span data-stu-id="29947-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="29947-147">Weitere Informationen zu App Service-Pläne, finden Sie unter [Azure App Service-Pläne detaillierte Übersicht über](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="29947-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="29947-148">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-148">Select **Create**.</span></span> <span data-ttu-id="29947-149">Azure bereitstellen und starten Sie die Web-app.</span><span class="sxs-lookup"><span data-stu-id="29947-149">Azure will provision and start the web app.</span></span>

   ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="29947-151">Aktivieren der Git-Veröffentlichung für die neue Web-App</span><span class="sxs-lookup"><span data-stu-id="29947-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="29947-152">Bei Git handelt es sich um eine verteilte Versionskontrollsystem, die zum Bereitstellen einer Azure App Service-Web-app verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="29947-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="29947-153">Web-app-Code in einem lokalen Git-Repository gespeichert ist, und der Code in Azure bereitgestellt wird, indem Sie auf einem remote-Repository übertragen.</span><span class="sxs-lookup"><span data-stu-id="29947-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="29947-154">Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="29947-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="29947-155">Wählen Sie **Anwendungsdienste** zum Anzeigen einer Liste von der app-Dienste, die mit dem Azure-Abonnement verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="29947-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="29947-156">Wählen Sie die Web-app, die im vorherigen Abschnitt dieses Lernprogramms erstellt.</span><span class="sxs-lookup"><span data-stu-id="29947-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="29947-157">Klicken Sie auf dem Blatt **Bereitstellung** auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**.</span><span class="sxs-lookup"><span data-stu-id="29947-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="29947-159">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="29947-159">Select **OK**.</span></span>

1. <span data-ttu-id="29947-160">Wenn Anmeldeinformationen für die Veröffentlichung von einer Web-app oder eine andere App Service-app für die Bereitstellung zuvor eingerichtet wurde dies nicht getan haben, richten sie nun:</span><span class="sxs-lookup"><span data-stu-id="29947-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="29947-161">Wählen Sie **Einstellungen** > **bereitstellungsanmeldeinformationen**.</span><span class="sxs-lookup"><span data-stu-id="29947-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="29947-162">Die **legen Sie Anmeldeinformationen für die Bereitstellung** Blatt wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29947-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="29947-163">Erstellen Sie einen Benutzernamen und ein Kennwort.</span><span class="sxs-lookup"><span data-stu-id="29947-163">Create a user name and password.</span></span> <span data-ttu-id="29947-164">Speichern Sie das Kennwort für die spätere Verwendung beim Einrichten von Git.</span><span class="sxs-lookup"><span data-stu-id="29947-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="29947-165">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="29947-165">Select **Save**.</span></span>

1. <span data-ttu-id="29947-166">In der **Web-App** Blatt select **Einstellungen** > **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="29947-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="29947-167">Die URL der remote Git-Repositorys zur Bereitstellung auf unterhalb **GIT-URL**.</span><span class="sxs-lookup"><span data-stu-id="29947-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="29947-168">Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="29947-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="29947-170">Veröffentlichen Sie die Web-app in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="29947-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="29947-171">Erstellen Sie in diesem Abschnitt ein lokales Git-Repository mit Visual Studio und Push aus diesem Repository in Azure zum Bereitstellen der Web-app.</span><span class="sxs-lookup"><span data-stu-id="29947-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="29947-172">Folgende Schritte müssen ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="29947-172">The steps involved include the following:</span></span>

* <span data-ttu-id="29947-173">Fügen Sie die remote-Repository-Einstellung, die mit den GIT-URL-Wert, damit das lokale Repository in Azure bereitgestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="29947-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="29947-174">Festgeschrieben Sie projektänderungen werden.</span><span class="sxs-lookup"><span data-stu-id="29947-174">Commit project changes.</span></span>
* <span data-ttu-id="29947-175">Push projektänderungen aus dem lokalen Repository in das Remoterepository in Azure.</span><span class="sxs-lookup"><span data-stu-id="29947-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="29947-176">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="29947-177">Die **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29947-177">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="29947-179">Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="29947-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="29947-180">In der **entfernten Datenbanken** Teil der **-Repositoryeinstellungen werden**Option **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="29947-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="29947-181">Die **Remote hinzufügen** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29947-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="29947-182">Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.</span><span class="sxs-lookup"><span data-stu-id="29947-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="29947-183">Legen Sie den Wert für **Fetch** auf die **Git-URL** , die zuvor in diesem Lernprogramm aus Azure kopiert.</span><span class="sxs-lookup"><span data-stu-id="29947-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="29947-184">Beachten Sie, dass dies die URL ist, die mit **.git** endet.</span><span class="sxs-lookup"><span data-stu-id="29947-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="29947-186">Geben Sie als Alternative, die remote-Repository aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie den Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="29947-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="29947-187">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="29947-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="29947-188">Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="29947-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="29947-189">Vergewissern Sie sich, dass der Name und e-Mail-Adresse festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="29947-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="29947-190">Wählen Sie **Update** bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="29947-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="29947-191">Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="29947-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="29947-192">Geben Sie eine Commit-Nachricht, z. B. **anfängliche Push Nr. 1** , und wählen Sie **Commit**.</span><span class="sxs-lookup"><span data-stu-id="29947-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="29947-193">Diese Aktion erstellt eine *Commit* lokal.</span><span class="sxs-lookup"><span data-stu-id="29947-193">This action creates a *commit* locally.</span></span>

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="29947-195">Als Alternative können Commit ändert sich von der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie die Git-Befehle eingeben.</span><span class="sxs-lookup"><span data-stu-id="29947-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="29947-196">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="29947-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="29947-197">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**.</span><span class="sxs-lookup"><span data-stu-id="29947-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="29947-198">Der Eingabeaufforderung in das Projektverzeichnis wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="29947-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="29947-199">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="29947-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="29947-200">Geben Sie den Azure **bereitstellungsanmeldeinformationen** Kennwort, die zuvor in Azure erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="29947-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="29947-201">Dieser Befehl startet den Prozess der lokalen Projektdateien auf Azure zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="29947-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="29947-202">Die Ausgabe der obige Befehl endet mit einer Meldung, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="29947-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="29947-203">Wenn Zusammenarbeit für das Projekt erforderlich ist, sollten Sie erwägen, pushen an [GitHub](https://github.com) vor dem auf Azure übertragen.</span><span class="sxs-lookup"><span data-stu-id="29947-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="29947-204">Überprüfen der aktiven Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="29947-204">Verify the Active Deployment</span></span>

<span data-ttu-id="29947-205">Stellen Sie sicher, dass die Web-app-Übertragung von der lokalen Umgebung in Azure erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="29947-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="29947-206">In der [Azure-Portal](https://portal.azure.com), wählen Sie die Web-app.</span><span class="sxs-lookup"><span data-stu-id="29947-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="29947-207">Wählen Sie **Bereitstellung** > **Bereitstellungsoptionen**.</span><span class="sxs-lookup"><span data-stu-id="29947-207">Select **Deployment** > **Deployment options**.</span></span>

![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="29947-209">Ausführen der App in Azure</span><span class="sxs-lookup"><span data-stu-id="29947-209">Run the app in Azure</span></span>

<span data-ttu-id="29947-210">Nun, dass die Web-app in Azure bereitgestellt wird, führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="29947-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="29947-211">Dies kann auf zwei Arten erfolgen:</span><span class="sxs-lookup"><span data-stu-id="29947-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="29947-212">Suchen Sie im Azure-Portal Blatt für die Web-app für die Web-app ein.</span><span class="sxs-lookup"><span data-stu-id="29947-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="29947-213">Wählen Sie **Durchsuchen** um die app in der Standard-Webbrowser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="29947-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="29947-214">Öffnen Sie einen Browser, und geben Sie die URL für die Web-app.</span><span class="sxs-lookup"><span data-stu-id="29947-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="29947-215">Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="29947-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="29947-216">Die Web-app aktualisieren und erneut veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="29947-216">Update the web app and republish</span></span>

<span data-ttu-id="29947-217">Nach Änderungen an den lokalen Code zu veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="29947-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="29947-218">Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="29947-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="29947-219">Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="29947-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="29947-220">Speichern Sie die Änderungen zu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="29947-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="29947-221">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="29947-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="29947-222">Die **Team Explorer** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29947-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="29947-223">Geben Sie eine Commit-Nachricht, z. B. `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="29947-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="29947-224">Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.</span><span class="sxs-lookup"><span data-stu-id="29947-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="29947-225">Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="29947-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="29947-226">Als Alternative können mithilfe von Push übertragen die Änderungen aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie einen Git-Befehl eingeben.</span><span class="sxs-lookup"><span data-stu-id="29947-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="29947-227">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="29947-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="29947-228">Anzeigen der aktualisierten Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="29947-228">View the updated web app in Azure</span></span>

<span data-ttu-id="29947-229">Die aktualisierte Web-app anzeigen, indem er **Durchsuchen** aus dem Blade der Web-app in der Azure-Verwaltungsportal oder durch einen Browser öffnen und die URL für die Web-app eingeben.</span><span class="sxs-lookup"><span data-stu-id="29947-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="29947-230">Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="29947-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="29947-231">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="29947-231">Additional resources</span></span>

* [<span data-ttu-id="29947-232">Verwenden Sie zum Erstellen und Veröffentlichen einer Azure-Web-App mit Continuous Deployment VSTS</span><span class="sxs-lookup"><span data-stu-id="29947-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="29947-233">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="29947-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
