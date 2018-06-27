---
title: Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools
author: camsoper
description: Erfahren Sie, wie Sie eine ASP.NET Core-App in Azure App Service mithilfe des Git-Befehlszeilenclients veröffentlichen.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 3fc068096a4b8696340787aa15120a2f97d10164
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252437"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="60bde-103">Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="60bde-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="60bde-104">Von [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="60bde-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="60bde-105">Dieses Tutorial zeigt Ihnen, wie Sie eine ASP. NET Core-App mithilfe von Befehlszeilentools für Microsoft Azure App Service erstellen und bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="60bde-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="60bde-106">Wenn Sie fertig sind, verfügen Sie über eine Razor Pages-Web-App, die in ASP.NET Core erstellt wurde und als Azure App Service-Web-App gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="60bde-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="60bde-107">Dieses Tutorial wurde mit Windows-Befehlszeilentools geschrieben, kann aber auch auf macOS- und Linux-Umgebungen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="60bde-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="60bde-108">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="60bde-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="60bde-109">Erstellen einer Azure App Service-Website mithilfe der Azure CLI</span><span class="sxs-lookup"><span data-stu-id="60bde-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="60bde-110">Bereitstellen einer ASP.NET Core-App in Azure App Service mit dem Git-Befehlszeilentool</span><span class="sxs-lookup"><span data-stu-id="60bde-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60bde-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="60bde-111">Prerequisites</span></span>

<span data-ttu-id="60bde-112">Um dieses Tutorial abzuschließen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="60bde-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="60bde-113">[Microsoft Azure-Abonnement](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="60bde-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="60bde-114">[Git](https://www.git-scm.com/)-Befehlszeilenclient</span><span class="sxs-lookup"><span data-stu-id="60bde-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="60bde-115">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="60bde-115">Create a web app</span></span>

<span data-ttu-id="60bde-116">Erstellen Sie ein neues Verzeichnis für die Web-App, erstellen Sie eine neue ASP.NET Core-Razor Pages-App, und führen Sie die Website dann lokal aus.</span><span class="sxs-lookup"><span data-stu-id="60bde-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="60bde-117">Windows</span><span class="sxs-lookup"><span data-stu-id="60bde-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="60bde-119">Andere</span><span class="sxs-lookup"><span data-stu-id="60bde-119">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Befehlszeilenausgabe](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="60bde-122">Testen Sie die App, indem Sie zu `http://localhost:5000` navigieren.</span><span class="sxs-lookup"><span data-stu-id="60bde-122">Test the app by browsing to `http://localhost:5000`.</span></span>

![Die lokal ausgeführte Website](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="60bde-124">Erstellen der Azure App Service-Instanz</span><span class="sxs-lookup"><span data-stu-id="60bde-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="60bde-125">Erstellen Sie mithilfe von [Azure Cloud Shell](/azure/cloud-shell/quickstart) eine Ressourcengruppe, einen App-Service-Plan und eine App Service-Web-App.</span><span class="sxs-lookup"><span data-stu-id="60bde-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="60bde-126">Legen Sie vor der Bereitstellung mithilfe des folgenden Befehls die Anmeldeinformationen für die Bereitstellung auf Kontoebene fest:</span><span class="sxs-lookup"><span data-stu-id="60bde-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="60bde-127">Eine Bereitstellungs-URL ist erforderlich, um die App mithilfe von Git bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="60bde-127">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="60bde-128">Rufen Sie die URL wie folgt ab.</span><span class="sxs-lookup"><span data-stu-id="60bde-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="60bde-129">Beachten Sie die angezeigte URL Endung in `.git`.</span><span class="sxs-lookup"><span data-stu-id="60bde-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="60bde-130">Sie wird im nächsten Schritt verwendet.</span><span class="sxs-lookup"><span data-stu-id="60bde-130">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="60bde-131">Bereitstellen der App mithilfe von Git</span><span class="sxs-lookup"><span data-stu-id="60bde-131">Deploy the app using Git</span></span>

<span data-ttu-id="60bde-132">Jetzt können Sie die Bereitstellung über Ihren lokalen Computer mithilfe von Git vornehmen.</span><span class="sxs-lookup"><span data-stu-id="60bde-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="60bde-133">Sie können alle Warnungen von Git zu Zeilenenden problemlos ignorieren.</span><span class="sxs-lookup"><span data-stu-id="60bde-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="60bde-134">Windows</span><span class="sxs-lookup"><span data-stu-id="60bde-134">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="60bde-135">Andere</span><span class="sxs-lookup"><span data-stu-id="60bde-135">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="60bde-136">Git fordert Sie zur Eingabe der Anmeldeinformationen für die Bereitstellung auf, die zuvor festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="60bde-136">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="60bde-137">Nach der Authentifizierung wird die App mithilfe von Push an den Remotespeicherort übertragen und dort erstellt und bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="60bde-137">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Git-Bereitstellungsausgabe](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="60bde-139">Testen der App</span><span class="sxs-lookup"><span data-stu-id="60bde-139">Test the app</span></span>

<span data-ttu-id="60bde-140">Testen Sie die App, indem Sie zu `https://<web app name>.azurewebsites.net` navigieren.</span><span class="sxs-lookup"><span data-stu-id="60bde-140">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="60bde-141">Um die Adresse in der Cloud Shell (oder der Azure CLI) anzuzeigen, verwenden Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="60bde-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Die in Azure ausgeführte App](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="60bde-143">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="60bde-143">Clean up</span></span>

<span data-ttu-id="60bde-144">Wenn Sie die App getestet und den Code und die Ressourcen untersucht haben, löschen Sie die Web-App und den Plan, indem Sie die Ressourcengruppe löschen.</span><span class="sxs-lookup"><span data-stu-id="60bde-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="60bde-145">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="60bde-145">Next steps</span></span>

<span data-ttu-id="60bde-146">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="60bde-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="60bde-147">Erstellen einer Azure App Service-Website mithilfe der Azure CLI</span><span class="sxs-lookup"><span data-stu-id="60bde-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="60bde-148">Bereitstellen einer ASP.NET Core-App in Azure App Service mit dem Git-Befehlszeilentool</span><span class="sxs-lookup"><span data-stu-id="60bde-148">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="60bde-149">Im nächsten Schritt erfahren Sie, wie Sie die Befehlszeile verwenden, um eine vorhandene Web-App bereitzustellen, die CosmosDB verwendet.</span><span class="sxs-lookup"><span data-stu-id="60bde-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="60bde-150">Bereitstellen in Azure über die Befehlszeile mit .NET Core</span><span class="sxs-lookup"><span data-stu-id="60bde-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
