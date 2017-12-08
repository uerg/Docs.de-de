---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Web-API 2 mit Entity Framework 6 mit | Microsoft Docs
author: MikeWasson
description: "In diesem Lernprogramm erfahren Sie, dass die Grundlagen der Erstellung einer Webanwendung mit einer ASP.NET Web API-back-End. Das Lernprogramm verwendet die Entity Framework 6 für das Layout der Daten..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 42139f8c158dd84cfc30f23c013343348b0c008a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="8dd82-104">Mithilfe von Web-API 2 mit Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8dd82-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="8dd82-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8dd82-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8dd82-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="8dd82-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="8dd82-107">In diesem Lernprogramm erfahren Sie, dass die Grundlagen der Erstellung einer Webanwendung mit einer ASP.NET Web API-back-End.</span><span class="sxs-lookup"><span data-stu-id="8dd82-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="8dd82-108">Das Lernprogramm verwendet die Entity Framework 6 für die Datenschicht und Knockout.js für die clientseitige JavaScript-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8dd82-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="8dd82-109">Im Lernprogramm wird gezeigt, wie die app auf Azure App Service-Web-Apps bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="8dd82-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8dd82-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="8dd82-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8dd82-111">Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="8dd82-111">Web API 2.1</span></span>
> - [<span data-ttu-id="8dd82-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="8dd82-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="8dd82-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8dd82-113">Entity Framework 6</span></span>
> - <span data-ttu-id="8dd82-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8dd82-114">.NET 4.5</span></span>
> - <span data-ttu-id="8dd82-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="8dd82-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="8dd82-116">Dieses Lernprogramm verwendet ASP.NET Web API 2 mit Entity Framework 6, um eine Webanwendung zu erstellen, die eine Back-End-Datenbank bearbeitet.</span><span class="sxs-lookup"><span data-stu-id="8dd82-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="8dd82-117">Hier ist ein Screenshot, der die Anwendung, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="8dd82-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="8dd82-118">Die app verwendet einen Entwurf Einzelseiten-Anwendung (SPA).</span><span class="sxs-lookup"><span data-stu-id="8dd82-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="8dd82-119">"Einzelseiten-Anwendung" ist der allgemeine Begriff für eine Webanwendung, die einzelne HTML-Seite geladen und aktualisiert dann die Seite dynamisch, anstatt neue Seiten zu laden.</span><span class="sxs-lookup"><span data-stu-id="8dd82-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="8dd82-120">Nach dem ersten Laden der Seite finden Sie die app mit dem Server über AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="8dd82-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="8dd82-121">Die AJAX-Anforderungen return JSON-Daten, die die app verwendet wird, um die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="8dd82-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="8dd82-122">AJAX ist nicht neu, jedoch heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="8dd82-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="8dd82-123">Dieses Lernprogramm verwendet [Knockout.js](http://knockoutjs.com/), aber Sie können einem JavaScript-Client-Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="8dd82-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="8dd82-124">Hier sind die wichtigsten Bausteine für diese app:</span><span class="sxs-lookup"><span data-stu-id="8dd82-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="8dd82-125">ASP.NET MVC erstellt die HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="8dd82-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="8dd82-126">ASP.NET Web API verarbeitet die AJAX-Anforderungen und JSON-Daten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8dd82-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="8dd82-127">Knockout.js Data-bindet die HTML-Elemente, die JSON-Daten.</span><span class="sxs-lookup"><span data-stu-id="8dd82-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="8dd82-128">Entity Framework finden Sie in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8dd82-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="8dd82-129">Finden Sie unter dieser App auf Azure ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="8dd82-129">See this App Running on Azure</span></span>

<span data-ttu-id="8dd82-130">Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird?</span><span class="sxs-lookup"><span data-stu-id="8dd82-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="8dd82-131">Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.</span><span class="sxs-lookup"><span data-stu-id="8dd82-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="8dd82-132">Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd82-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="8dd82-133">Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="8dd82-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="8dd82-134">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="8dd82-134">[Open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="8dd82-135">[MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="8dd82-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="8dd82-136">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="8dd82-136">Create the Project</span></span>

<span data-ttu-id="8dd82-137">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dd82-137">Open Visual Studio.</span></span> <span data-ttu-id="8dd82-138">Aus der **Datei** klicken Sie im Menü **neu**, und wählen Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="8dd82-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="8dd82-139">(Oder klicken Sie auf **neues Projekt** auf der Startseite.)</span><span class="sxs-lookup"><span data-stu-id="8dd82-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="8dd82-140">In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich.</span><span class="sxs-lookup"><span data-stu-id="8dd82-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="8dd82-141">Nennen Sie das Projekt BookService, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8dd82-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="8dd82-142">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web-API** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="8dd82-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="8dd82-143">Wenn Sie das Projekt in einem Azure-App-Dienst hosten möchten, lassen Sie die **Host in der Cloud** Feld überprüft.</span><span class="sxs-lookup"><span data-stu-id="8dd82-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="8dd82-144">Klicken Sie auf **OK** zum Erstellen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="8dd82-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="8dd82-145">Konfigurieren von Azure-Einstellungen (Optional)</span><span class="sxs-lookup"><span data-stu-id="8dd82-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="8dd82-146">Wenn Sie bleibt die **Host in der Cloud** Option aktiviert, die Visual Studio werden Sie aufgefordert, Microsoft Azure anmelden</span><span class="sxs-lookup"><span data-stu-id="8dd82-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="8dd82-147">Nachdem Sie bei Azure anmelden, werden Sie von Visual Studio aufgefordert, die Web-app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8dd82-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="8dd82-148">Geben Sie einen Namen für die Website, wählen Sie Ihr Azure-Abonnement, und wählen Sie eine geografische Region.</span><span class="sxs-lookup"><span data-stu-id="8dd82-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="8dd82-149">Klicken Sie unter **Datenbankserver**Option **Erstellen neuer Server**.</span><span class="sxs-lookup"><span data-stu-id="8dd82-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="8dd82-150">Geben Sie einen Benutzernamen und das Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="8dd82-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[<span data-ttu-id="8dd82-151">Nächste</span><span class="sxs-lookup"><span data-stu-id="8dd82-151">Next</span></span>](part-2.md)
