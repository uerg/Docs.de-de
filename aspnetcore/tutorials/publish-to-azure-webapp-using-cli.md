---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools | Microsoft Docs"
description: Erfahren Sie, wie Sie eine Microsoft Azure-App mithilfe von ASP.NET Core und dem Git-Befehlszeilenclient erstellen und bereitstellen.
services: multiple
keywords: ASP.NET Core, Azure, App Service, Git, Befehlszeile
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0bcff4f79356b960f663dcebb1d79a108417dbd2
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>Bereitstellen einer ASP.NET Core-Anwendung in Azure App Service über die Befehlszeile

Von [Cam Soper](https://twitter.com/camsoper)

Dieses Tutorial zeigt Ihnen, wie Sie eine ASP. NET Core-Anwendung mithilfe von Befehlszeilentools für Microsoft Azure App Service erstellen und bereitstellen.  Wenn Sie fertig sind, verfügen Sie über eine Webanwendung, die in ASP.NET MVC Core erstellt wurde und als Azure App Service-Web-App gehostet wird.  Dieses Tutorial wurde mit Windows-Befehlszeilentools geschrieben, kann aber auch auf macOS- und Linux-Umgebungen angewendet werden.  

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen einer Azure App Service-Website mithilfe der Azure CLI
> * Bereitstellen einer ASP.NET Core-Anwendung in Azure App Service mit dem Git-Befehlszeilentool

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie Folgendes:

* [Microsoft Azure-Abonnement](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* [Git](https://www.git-scm.com/)-Befehlszeilenclient

## <a name="create-a-web-application"></a>Erstellen einer Webanwendung

Erstellen Sie ein neues Verzeichnis für die Webanwendung, erstellen Sie eine neue ASP.NET Core MVC-Anwendung, und führen Sie die Website dann lokal aus.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Andere](#tab/other)
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

Testen Sie die Anwendung, indem Sie zu http://localhost:5000 navigieren.

![Die lokal ausgeführte Website](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a>Erstellen der Azure App Service-Instanz

Erstellen Sie mithilfe von [Azure Cloud Shell](/azure/cloud-shell/quickstart) eine Ressourcengruppe, einen App-Service-Plan und eine App Service-Web-App.

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

Legen Sie vor der Bereitstellung mithilfe des folgenden Befehls die Anmeldeinformationen für die Bereitstellung auf Kontoebene fest:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Eine Bereitstellungs-URL ist erforderlich, um die Anwendung mithilfe von Git bereitzustellen.  Rufen Sie die URL wie folgt ab.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Beachten Sie die angezeigte URL Endung in `.git`. Sie wird im nächsten Schritt verwendet.

## <a name="deploy-the-application-using-git"></a>Bereitstellen der Anwendung mithilfe von Git

Jetzt können Sie die Bereitstellung über Ihren lokalen Computer mithilfe von Git vornehmen.

> [!NOTE]
> Sie können alle Warnungen von Git zu Zeilenenden problemlos ignorieren.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
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

# <a name="othertabother"></a>[Andere](#tab/other)
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

Git fordert Sie zur Eingabe der Anmeldeinformationen für die Bereitstellung auf, die zuvor festgelegt wurden.  Nach der Authentifizierung wird die Anwendung mithilfe von Push in den Remotespeicherort übertragen, erstellt und bereitgestellt.

![Git-Bereitstellungsausgabe](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Testen der Anwendung

Testen Sie die Anwendung, indem Sie zu `https://<web app name>.azurewebsites.net` navigieren.  Um die Adresse in der Cloud Shell (oder der Azure CLI) anzuzeigen, verwenden Sie Folgendes:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Die in Azure ausgeführte App](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Bereinigung

Wenn Sie die App getestet und den Code und die Ressourcen untersucht haben, löschen Sie die Web-App und den Plan, indem Sie die Ressourcengruppe löschen.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen einer Azure App Service-Website mithilfe der Azure CLI
> * Bereitstellen einer ASP.NET Core-Anwendung in Azure App Service mit dem Git-Befehlszeilentool

Im nächsten Schritt erfahren Sie, wie Sie die Befehlszeile verwenden, um eine vorhandene Web-App bereitzustellen, die CosmosDB verwendet.

> [!div class="nextstepaction"]
> [Bereitstellen in Azure über die Befehlszeile mit .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
