---
title: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
author: rachelappel
description: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="50712-103">Veröffentlichen einer ASP.NET Core SignalR-app an eine Azure-Web-App</span><span class="sxs-lookup"><span data-stu-id="50712-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="50712-104">[Azure-Web-App](/azure/app-service/app-service-web-overview) ist ein [Microsoft cloud-computing](https://azure.microsoft.com/) Plattformdienst zum Hosten von Web-apps, einschließlich ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="50712-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="50712-105">Veröffentlichen der App</span><span class="sxs-lookup"><span data-stu-id="50712-105">Publish the app</span></span>

<span data-ttu-id="50712-106">Visual Studio bietet integrierte Tools für die Veröffentlichung in einer Azure-Web-App.</span><span class="sxs-lookup"><span data-stu-id="50712-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="50712-107">Visual Studio Code-Benutzer können [Azure-CLI](/cli/azure) Befehle aus, um apps in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="50712-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="50712-108">Dieser Artikel behandelt die publishing mit den Tools in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50712-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="50712-109">Zum Veröffentlichen einer app mit Azure-CLI finden Sie unter [Veröffentlichen einer ASP.NET Core-app in Azure mit Befehlszeilentools](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="50712-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="50712-110">Mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="50712-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="50712-111">Überprüfen Sie, ob **neu erstellen** eingecheckt wird die **Veröffentlichungsziel auswählen** Dialogfeld, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="50712-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Kommissionierung veröffentlichen Ziel](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="50712-113">Geben Sie die folgenden Informationen in den **Anwendungsdienst erstellen** Dialogfeld und wählen Sie **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="50712-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="50712-114">Element</span><span class="sxs-lookup"><span data-stu-id="50712-114">Item</span></span> | <span data-ttu-id="50712-115">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="50712-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="50712-116">**App-name**</span><span class="sxs-lookup"><span data-stu-id="50712-116">**App name**</span></span> | <span data-ttu-id="50712-117">Ein eindeutiger Name der app.</span><span class="sxs-lookup"><span data-stu-id="50712-117">A unique name of the app.</span></span> |
| <span data-ttu-id="50712-118">**Abonnement**</span><span class="sxs-lookup"><span data-stu-id="50712-118">**Subscription**</span></span> | <span data-ttu-id="50712-119">Das Azure-Abonnement, das die app verwendet.</span><span class="sxs-lookup"><span data-stu-id="50712-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="50712-120">**Ressourcengruppe**</span><span class="sxs-lookup"><span data-stu-id="50712-120">**Resource Group**</span></span> | <span data-ttu-id="50712-121">Die Gruppe von verwandten Ressourcen, zu denen die app gehört.</span><span class="sxs-lookup"><span data-stu-id="50712-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="50712-122">**Hosting-Plan**</span><span class="sxs-lookup"><span data-stu-id="50712-122">**Hosting Plan**</span></span> | <span data-ttu-id="50712-123">Der Tarif für die Web-app.</span><span class="sxs-lookup"><span data-stu-id="50712-123">The pricing plan for the web app.</span></span> |

![Erstellen der app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="50712-125">Visual Studio führt die folgenden Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="50712-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="50712-126">Erstellt ein Veröffentlichungsprofil mit veröffentlichungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="50712-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="50712-127">Erstellt oder verwendet eine vorhandene *Azure-Web-App* mit den angegebenen Details.</span><span class="sxs-lookup"><span data-stu-id="50712-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="50712-128">Die app wird veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="50712-128">Publishes the app.</span></span>
* <span data-ttu-id="50712-129">Startet einen Webbrowser, und klicken Sie mit der veröffentlichten Webanwendung geladen.</span><span class="sxs-lookup"><span data-stu-id="50712-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="50712-130">Beachten Sie das Format der URL für die app ist *{Anwendungsname} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="50712-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="50712-131">Beispielsweise eine app namens `SignalRChattR` verfügt über eine URL, die aussieht `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="50712-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="50712-132">Wenn ein HTTP-502.2-Fehler auftritt, finden Sie unter [bereitstellen ASP.NET Core Preview-Version nach Azure App Service](xref:host-and-deploy/azure-apps/index) zur Fehlerbehebung.</span><span class="sxs-lookup"><span data-stu-id="50712-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="50712-133">SignalR-Web-app konfigurieren</span><span class="sxs-lookup"><span data-stu-id="50712-133">Configure SignalR web app</span></span>

<span data-ttu-id="50712-134">ASP.NET SignalR Core apps, die veröffentlicht werden, wie Sie benötigen ein Azure-Web-App [ARR Affinität](https://en.wikipedia.org/wiki/Application_Request_Routing) aktiviert.</span><span class="sxs-lookup"><span data-stu-id="50712-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="50712-135">[WebSockets](xref:fundamentals/websockets) aktiviert sein sollten, um den Transport WebSockets-Funktion zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="50712-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="50712-136">Navigieren Sie im Azure-Portal zu **Anwendungseinstellungen** für Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="50712-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="50712-137">Legen Sie **WebSockets** auf **auf**, und vergewissern Sie sich **ARR Affinität** ist **auf**.</span><span class="sxs-lookup"><span data-stu-id="50712-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure Web-app-Einstellungen im Azure-portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="50712-139">WebSockets und andere Transporte [beschränkt werden basierend auf der App Service-Plan](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="50712-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="50712-140">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="50712-140">Related resources</span></span>

* [<span data-ttu-id="50712-141">Veröffentlichen einer ASP.NET Core-app in Azure mit Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="50712-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="50712-142">Veröffentlichen einer ASP.NET Core-app in Azure mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50712-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="50712-143">Hosten und ASP.NET Core Vorschau-apps in Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="50712-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
