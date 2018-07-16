---
title: Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools
author: camsoper
description: Erfahren Sie, wie Sie eine ASP.NET Core-App in Azure App Service mithilfe des Git-Befehlszeilenclients veröffentlichen.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126959"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Veröffentlichen einer ASP.NET Core-App in Azure mithilfe von Befehlszeilentools

Von [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Dieses Tutorial zeigt Ihnen, wie Sie eine ASP. NET Core-App mithilfe von Befehlszeilentools für Microsoft Azure App Service erstellen und bereitstellen. Wenn Sie fertig sind, verfügen Sie über eine Razor Pages-Web-App, die in ASP.NET Core erstellt wurde und als Azure App Service-Web-App gehostet wird. Dieses Tutorial wurde mit Windows-Befehlszeilentools geschrieben, kann aber auch auf macOS- und Linux-Umgebungen angewendet werden.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen einer Azure App Service-Website mithilfe der Azure CLI
> * Bereitstellen einer ASP.NET Core-App in Azure App Service mit dem Git-Befehlszeilentool

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Tutorial abzuschließen, benötigen Sie Folgendes:

* [Microsoft Azure-Abonnement](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/)-Befehlszeilenclient

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie ein neues Verzeichnis für die Web-App, erstellen Sie eine neue ASP.NET Core-Razor Pages-App, und führen Sie die Website dann lokal aus.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[Andere](#tab/other)

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

Testen Sie die App, indem Sie zu `http://localhost:5000` navigieren.

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

Eine Bereitstellungs-URL ist erforderlich, um die App mithilfe von Git bereitzustellen. Rufen Sie die URL wie folgt ab.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Beachten Sie die angezeigte URL Endung in `.git`. Sie wird im nächsten Schritt verwendet.

## <a name="deploy-the-app-using-git"></a>Bereitstellen der App mithilfe von Git

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

Git fordert Sie zur Eingabe der Anmeldeinformationen für die Bereitstellung auf, die zuvor festgelegt wurden. Nach der Authentifizierung wird die App mithilfe von Push an den Remotespeicherort übertragen und dort erstellt und bereitgestellt.

![Git-Bereitstellungsausgabe](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Testen der App

Testen Sie die App, indem Sie zu `https://<web app name>.azurewebsites.net` navigieren. Um die Adresse in der Cloud Shell (oder der Azure CLI) anzuzeigen, verwenden Sie Folgendes:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Die in Azure ausgeführte App](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Bereinigen

Wenn Sie die App getestet und den Code und die Ressourcen untersucht haben, löschen Sie die Web-App und den Plan, indem Sie die Ressourcengruppe löschen.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen einer Azure App Service-Website mithilfe der Azure CLI
> * Bereitstellen einer ASP.NET Core-App in Azure App Service mit dem Git-Befehlszeilentool

Im nächsten Schritt erfahren Sie, wie Sie die Befehlszeile verwenden, um eine vorhandene Web-App bereitzustellen, die CosmosDB verwendet.

> [!div class="nextstepaction"]
> [Bereitstellen in Azure über die Befehlszeile mit .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
