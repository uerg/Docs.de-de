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
ms.openlocfilehash: 0462a4cf18bba23643ed3b1b4e6b76bdbceb24a8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="17cdd-103">Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="17cdd-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="17cdd-104">Von [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="17cdd-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="17cdd-105">Dieses Tutorial zeigt Ihnen, wie Sie eine ASP. NET Core-Anwendung mithilfe von Befehlszeilentools für Microsoft Azure App Service erstellen und bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="17cdd-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="17cdd-106">Wenn Sie fertig sind, verfügen Sie über eine Webanwendung, die in ASP.NET MVC Core erstellt wurde und als Azure App Service-Web-App gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="17cdd-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="17cdd-107">Dieses Tutorial wurde mit Windows-Befehlszeilentools geschrieben, kann aber auch auf macOS- und Linux-Umgebungen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="17cdd-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="17cdd-108">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="17cdd-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17cdd-109">Erstellen einer Azure App Service-Website mithilfe der Azure CLI</span><span class="sxs-lookup"><span data-stu-id="17cdd-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="17cdd-110">Bereitstellen einer ASP.NET Core-Anwendung in Azure App Service mit dem Git-Befehlszeilentool</span><span class="sxs-lookup"><span data-stu-id="17cdd-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17cdd-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="17cdd-111">Prerequisites</span></span>

<span data-ttu-id="17cdd-112">Um dieses Tutorial abzuschließen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="17cdd-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="17cdd-113">[Microsoft Azure-Abonnement](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="17cdd-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="17cdd-114">[Git](https://www.git-scm.com/)-Befehlszeilenclient</span><span class="sxs-lookup"><span data-stu-id="17cdd-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="17cdd-115">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="17cdd-115">Create a web application</span></span>

<span data-ttu-id="17cdd-116">Erstellen Sie ein neues Verzeichnis für die Webanwendung, erstellen Sie eine neue ASP.NET Core MVC-Anwendung, und führen Sie die Website dann lokal aus.</span><span class="sxs-lookup"><span data-stu-id="17cdd-116">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="17cdd-117">Windows</span><span class="sxs-lookup"><span data-stu-id="17cdd-117">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="17cdd-118">Andere</span><span class="sxs-lookup"><span data-stu-id="17cdd-118">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Befehlszeilenausgabe](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="17cdd-120">Testen Sie die Anwendung, indem Sie zu http://localhost:5000 navigieren.</span><span class="sxs-lookup"><span data-stu-id="17cdd-120">Test the application by browsing to http://localhost:5000.</span></span>

![Die lokal ausgeführte Website](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="17cdd-122">Erstellen der Azure App Service-Instanz</span><span class="sxs-lookup"><span data-stu-id="17cdd-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="17cdd-123">Erstellen Sie mithilfe von [Azure Cloud Shell](/azure/cloud-shell/quickstart) eine Ressourcengruppe, einen App-Service-Plan und eine App Service-Web-App.</span><span class="sxs-lookup"><span data-stu-id="17cdd-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="17cdd-124">Legen Sie vor der Bereitstellung mithilfe des folgenden Befehls die Anmeldeinformationen für die Bereitstellung auf Kontoebene fest:</span><span class="sxs-lookup"><span data-stu-id="17cdd-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="17cdd-125">Eine Bereitstellungs-URL ist erforderlich, um die Anwendung mithilfe von Git bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="17cdd-125">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="17cdd-126">Rufen Sie die URL wie folgt ab.</span><span class="sxs-lookup"><span data-stu-id="17cdd-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="17cdd-127">Beachten Sie die angezeigte URL Endung in `.git`.</span><span class="sxs-lookup"><span data-stu-id="17cdd-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="17cdd-128">Sie wird im nächsten Schritt verwendet.</span><span class="sxs-lookup"><span data-stu-id="17cdd-128">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="17cdd-129">Bereitstellen der Anwendung mithilfe von Git</span><span class="sxs-lookup"><span data-stu-id="17cdd-129">Deploy the application using Git</span></span>

<span data-ttu-id="17cdd-130">Jetzt können Sie die Bereitstellung über Ihren lokalen Computer mithilfe von Git vornehmen.</span><span class="sxs-lookup"><span data-stu-id="17cdd-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="17cdd-131">Sie können alle Warnungen von Git zu Zeilenenden problemlos ignorieren.</span><span class="sxs-lookup"><span data-stu-id="17cdd-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="17cdd-132">Windows</span><span class="sxs-lookup"><span data-stu-id="17cdd-132">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="17cdd-133">Andere</span><span class="sxs-lookup"><span data-stu-id="17cdd-133">Other</span></span>](#tab/other)
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

<span data-ttu-id="17cdd-134">Git fordert Sie zur Eingabe der Anmeldeinformationen für die Bereitstellung auf, die zuvor festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="17cdd-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="17cdd-135">Nach der Authentifizierung wird die Anwendung mithilfe von Push in den Remotespeicherort übertragen, erstellt und bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="17cdd-135">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git-Bereitstellungsausgabe](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="17cdd-137">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="17cdd-137">Test the application</span></span>

<span data-ttu-id="17cdd-138">Testen Sie die Anwendung, indem Sie zu `https://<web app name>.azurewebsites.net` navigieren.</span><span class="sxs-lookup"><span data-stu-id="17cdd-138">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="17cdd-139">Um die Adresse in der Cloud Shell (oder der Azure CLI) anzuzeigen, verwenden Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="17cdd-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Die in Azure ausgeführte App](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="17cdd-141">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="17cdd-141">Clean up</span></span>

<span data-ttu-id="17cdd-142">Wenn Sie die App getestet und den Code und die Ressourcen untersucht haben, löschen Sie die Web-App und den Plan, indem Sie die Ressourcengruppe löschen.</span><span class="sxs-lookup"><span data-stu-id="17cdd-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="17cdd-143">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="17cdd-143">Next steps</span></span>

<span data-ttu-id="17cdd-144">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="17cdd-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17cdd-145">Erstellen einer Azure App Service-Website mithilfe der Azure CLI</span><span class="sxs-lookup"><span data-stu-id="17cdd-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="17cdd-146">Bereitstellen einer ASP.NET Core-Anwendung in Azure App Service mit dem Git-Befehlszeilentool</span><span class="sxs-lookup"><span data-stu-id="17cdd-146">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="17cdd-147">Im nächsten Schritt erfahren Sie, wie Sie die Befehlszeile verwenden, um eine vorhandene Web-App bereitzustellen, die CosmosDB verwendet.</span><span class="sxs-lookup"><span data-stu-id="17cdd-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="17cdd-148">Bereitstellen in Azure über die Befehlszeile mit .NET Core</span><span class="sxs-lookup"><span data-stu-id="17cdd-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
