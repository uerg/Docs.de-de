---
uid: mvc/overview/getting-started/introduction/getting-started
title: Erste Schritte mit ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier mit Visual Studio 2015 verfügbar. Das neue Lernprogramm verwendet ASP.NET Core MVC 6, die viele Improvem bietet...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868320"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="a8db2-104">Erste Schritte mit ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="a8db2-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="a8db2-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a8db2-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="a8db2-106">In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC 5 Webanwendung mit [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a8db2-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="a8db2-107">Endgültige Quelle Lernprogramm befindet sich auf [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="a8db2-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="a8db2-108">Dieses Lernprogramm wurde von geschrieben [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , und [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="a8db2-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="a8db2-109">Sie benötigen ein Azure-Konto um diese app in Azure bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="a8db2-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="a8db2-110">Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -erhalten Sie Gutschriften können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="a8db2-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="a8db2-111">Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="a8db2-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="a8db2-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="a8db2-112">Getting Started</span></span>

<span data-ttu-id="a8db2-113">Starten, indem Sie installieren und Ausführen von [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a8db2-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="a8db2-114">Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="a8db2-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="a8db2-115">Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="a8db2-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="a8db2-116">In Visual Studio wird eine Liste entlang des unteren mit verschiedenen verfügbaren Optionen für Sie.</span><span class="sxs-lookup"><span data-stu-id="a8db2-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="a8db2-117">Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="a8db2-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="a8db2-118">(Z. B. statt der Auswahl **neues Projekt** aus der **starten** Seite können Sie mithilfe des Menüs und auswählen **Datei** &gt; **neues Projekt**.)</span><span class="sxs-lookup"><span data-stu-id="a8db2-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="a8db2-119">Erstellen einer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="a8db2-119">Creating Your First Application</span></span>

<span data-ttu-id="a8db2-120">Klicken Sie auf **neues Projekt**, wählen Sie dann Visual c# auf der linken Seite dann **Web** und wählen Sie dann **ASP.NET-Webanwendung ((.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="a8db2-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="a8db2-121">Namen für das Projekt "MvcMovie", und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8db2-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="a8db2-122">In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC** , und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8db2-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="a8db2-123">Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche!</span><span class="sxs-lookup"><span data-stu-id="a8db2-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="a8db2-124">Dies ist eine einfache "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a8db2-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="a8db2-125">der Projekt, und es werden eine gute zum Starten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a8db2-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="a8db2-126">Klicken Sie auf F5, um mit dem Debuggen beginnen.</span><span class="sxs-lookup"><span data-stu-id="a8db2-126">Click F5 to start debugging.</span></span> <span data-ttu-id="a8db2-127">F5 bewirkt, dass Visual Studio starten [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) , und führen Sie Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="a8db2-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="a8db2-128">Visual Studio dann öffnet einen Browser und Startseite der Anwendung geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a8db2-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="a8db2-129">Beachten Sie, die besagt, die Adressleiste des Browsers dass `localhost:port#` und nicht etwas wie `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a8db2-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a8db2-130">Grund hierfür ist, `localhost` verweist immer auf Ihrem lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie soeben erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="a8db2-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="a8db2-131">Wenn Visual Studio ein Webprojekt ausgeführt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="a8db2-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a8db2-132">In der folgenden Abbildung ist die Portnummer 1234.</span><span class="sxs-lookup"><span data-stu-id="a8db2-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="a8db2-133">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="a8db2-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="a8db2-134">Direkt einsatzbereiten bietet diese Standardvorlage Seiten "Home", "Wenden Sie sich an" und "zu.</span><span class="sxs-lookup"><span data-stu-id="a8db2-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="a8db2-135">Der Abbildung oben zeigen nicht die **Home**, **zu** und **wenden Sie sich an** Links.</span><span class="sxs-lookup"><span data-stu-id="a8db2-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="a8db2-136">Je nach Größe Ihres Browserfensters müssen Sie auf das Symbol "Navigation", um die folgenden Links finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="a8db2-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="a8db2-137">Die Anwendung bietet auch Unterstützung zum Registrieren, und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="a8db2-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="a8db2-138">Der nächste Schritt besteht ändern, wie diese Anwendung funktioniert, und erfahren etwas über ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a8db2-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="a8db2-139">Schließen Sie die ASP.NET MVC-Anwendung, und wir ändern Sie Teil des Codes zu.</span><span class="sxs-lookup"><span data-stu-id="a8db2-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="a8db2-140">Eine Liste der aktuellen Lernprogramme, finden Sie unter [MVC empfohlen Artikel](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="a8db2-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a8db2-141">Finden Sie unter dieser App auf Azure ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="a8db2-141">See this App Running on Azure</span></span>

<span data-ttu-id="a8db2-142">Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird?</span><span class="sxs-lookup"><span data-stu-id="a8db2-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a8db2-143">Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.</span><span class="sxs-lookup"><span data-stu-id="a8db2-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="a8db2-144">Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure.</span><span class="sxs-lookup"><span data-stu-id="a8db2-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a8db2-145">Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="a8db2-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a8db2-146">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="a8db2-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a8db2-147">[MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="a8db2-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a8db2-148">Nächste</span><span class="sxs-lookup"><span data-stu-id="a8db2-148">Next</span></span>](adding-a-controller.md)
