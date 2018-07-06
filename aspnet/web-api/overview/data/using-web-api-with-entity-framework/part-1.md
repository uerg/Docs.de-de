---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Mithilfe von Web-API 2 mit Entitätsframework 6 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial erfahren Sie, dass die Grundlagen der Erstellung einer Web-Anwendung mit einer ASP.NET Web-API-back-End. Das Lernprogramm verwendet Entity Framework 6 für das Layout der Daten...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827232"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="b5e84-104">Mithilfe von Web-API 2 mit Entitätsframework 6</span><span class="sxs-lookup"><span data-stu-id="b5e84-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="b5e84-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b5e84-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b5e84-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="b5e84-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="b5e84-107">In diesem Tutorial erfahren Sie, dass die Grundlagen der Erstellung einer Web-Anwendung mit einer ASP.NET Web-API-back-End.</span><span class="sxs-lookup"><span data-stu-id="b5e84-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="b5e84-108">Das Lernprogramm verwendet Entity Framework 6 für die Datenschicht, und klicken Sie auf "Knockout.js", für die clientseitige JavaScript-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b5e84-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="b5e84-109">Das Tutorial veranschaulicht auch zum Bereitstellen der app in Azure App Service-Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="b5e84-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5e84-110">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b5e84-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b5e84-111">Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="b5e84-111">Web API 2.1</span></span>
> - [<span data-ttu-id="b5e84-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="b5e84-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="b5e84-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b5e84-113">Entity Framework 6</span></span>
> - <span data-ttu-id="b5e84-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b5e84-114">.NET 4.5</span></span>
> - <span data-ttu-id="b5e84-115">["Knockout.js"](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="b5e84-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="b5e84-116">Dieses Tutorial verwendet ASP.NET Web API 2 mit Entity Framework 6, um eine Webanwendung erstellen, die eine Back-End-Datenbank ändert.</span><span class="sxs-lookup"><span data-stu-id="b5e84-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="b5e84-117">Hier ist ein Screenshot der Anwendung, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="b5e84-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="b5e84-118">Die app verwendet eine Single-Page-Anwendung (SPA).</span><span class="sxs-lookup"><span data-stu-id="b5e84-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="b5e84-119">"Single-Page-Anwendung" ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt, und klicken Sie dann die Seite wird dynamisch aktualisiert, anstatt neue Seiten laden.</span><span class="sxs-lookup"><span data-stu-id="b5e84-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="b5e84-120">Nach dem ersten Laden der Seite werden die app mit dem Server über AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="b5e84-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="b5e84-121">Die AJAX-Anforderungen JSON-Daten zurückgeben, die die app verwendet wird, um die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="b5e84-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="b5e84-122">AJAX ist nicht neu, aber heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="b5e84-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="b5e84-123">Dieses Tutorial verwendet ["Knockout.js"](http://knockoutjs.com/), aber Sie können alle JavaScript-Clientframework verwenden.</span><span class="sxs-lookup"><span data-stu-id="b5e84-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="b5e84-124">Hier sind die wichtigsten Bausteine für diese app aus:</span><span class="sxs-lookup"><span data-stu-id="b5e84-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="b5e84-125">ASP.NET MVC erstellt, die HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="b5e84-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="b5e84-126">ASP.NET Web-API verarbeitet die AJAX-Anforderungen und JSON-Daten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b5e84-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="b5e84-127">Knockout.js-Datenbindung der HTML-Elemente, die JSON-Daten.</span><span class="sxs-lookup"><span data-stu-id="b5e84-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="b5e84-128">Entitätsframework finden Sie in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b5e84-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="b5e84-129">Finden Sie unter dieser App in Azure ausführen</span><span class="sxs-lookup"><span data-stu-id="b5e84-129">See this App Running on Azure</span></span>

<span data-ttu-id="b5e84-130">Möchten Sie die fertige Website als live-Web-app ausgeführt wird, finden Sie unter?</span><span class="sxs-lookup"><span data-stu-id="b5e84-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="b5e84-131">Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach die folgende Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="b5e84-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="b5e84-132">Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure.</span><span class="sxs-lookup"><span data-stu-id="b5e84-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="b5e84-133">Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="b5e84-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="b5e84-134">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="b5e84-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b5e84-135">[MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="b5e84-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="b5e84-136">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="b5e84-136">Create the Project</span></span>

<span data-ttu-id="b5e84-137">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5e84-137">Open Visual Studio.</span></span> <span data-ttu-id="b5e84-138">Von der **Datei** , wählen Sie im Menü **neu**, und wählen Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b5e84-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="b5e84-139">(Oder klicken Sie auf **neues Projekt** auf der Startseite.)</span><span class="sxs-lookup"><span data-stu-id="b5e84-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="b5e84-140">In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich.</span><span class="sxs-lookup"><span data-stu-id="b5e84-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="b5e84-141">Nennen Sie das Projekt BookService, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5e84-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="b5e84-142">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web-API-** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b5e84-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="b5e84-143">Wenn Sie das Projekt in einer Azure App Service hosten möchten, lassen Sie die **in der Cloud hosten** Feld aktiviert.</span><span class="sxs-lookup"><span data-stu-id="b5e84-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="b5e84-144">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b5e84-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="b5e84-145">Konfigurieren von Azure-Einstellungen (Optional)</span><span class="sxs-lookup"><span data-stu-id="b5e84-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="b5e84-146">Wenn Sie links die **Host in der Cloud** Option aktiviert, die Visual Studio fordert Sie zur Anmeldung beim Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="b5e84-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="b5e84-147">Nachdem Sie bei Azure anmelden, fordert Visual Studio Sie so konfigurieren Sie die Web-app.</span><span class="sxs-lookup"><span data-stu-id="b5e84-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="b5e84-148">Geben Sie einen Namen für die Website, wählen Sie Ihr Azure-Abonnement, und wählen Sie eine geografische Region.</span><span class="sxs-lookup"><span data-stu-id="b5e84-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="b5e84-149">Klicken Sie unter **Datenbankserver**Option **neuen Server erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b5e84-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="b5e84-150">Geben Sie einen Benutzernamen und das Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="b5e84-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="b5e84-151">Nächste</span><span class="sxs-lookup"><span data-stu-id="b5e84-151">Next</span></span>](part-2.md)
