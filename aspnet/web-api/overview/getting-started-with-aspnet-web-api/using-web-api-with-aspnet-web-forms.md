---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Verwenden Web-API mit ASP.NET Web Forms | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: a14bf0abd8c5d603cf3859891f855415cf3df9f3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835259"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="48953-102">Verwenden Web-API mit ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="48953-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="48953-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48953-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="48953-104">Obwohl ASP.NET Web-API mit ASP.NET MVC gepackt wurde, ist es einfach, eine herkömmliche ASP.NET Web Forms-Anwendung-Web-API hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48953-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="48953-105">Dieses Tutorial führt Sie durch die Schritte.</span><span class="sxs-lookup"><span data-stu-id="48953-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="48953-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="48953-106">Overview</span></span>

<span data-ttu-id="48953-107">Um Web-API in einer Web Forms-Anwendung verwenden zu können, gibt es zwei Hauptschritte:</span><span class="sxs-lookup"><span data-stu-id="48953-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="48953-108">Fügen Sie einen Web-API-Controller, die von abgeleitet der **ApiController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="48953-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="48953-109">Fügen Sie eine Routingtabelle die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="48953-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="48953-110">Erstellen einer Web Forms-Projekt</span><span class="sxs-lookup"><span data-stu-id="48953-110">Create a Web Forms Project</span></span>

<span data-ttu-id="48953-111">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="48953-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="48953-112">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="48953-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="48953-113">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="48953-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="48953-114">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="48953-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="48953-115">Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET Web Forms-Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="48953-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="48953-116">Geben Sie einen Namen für das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="48953-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="48953-117">Erstellen Sie das Modell und Controller</span><span class="sxs-lookup"><span data-stu-id="48953-117">Create the Model and Controller</span></span>

<span data-ttu-id="48953-118">In diesem Tutorial verwendet die gleichen Modell und Controller-Klassen als die [Einstieg](tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="48953-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="48953-119">Fügen Sie zunächst eine Modellklasse.</span><span class="sxs-lookup"><span data-stu-id="48953-119">First, add a model class.</span></span> <span data-ttu-id="48953-120">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48953-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="48953-121">Benennen Sie die Product-Klasse, und fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="48953-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="48953-122">Fügen Sie einen Web-API-Controller zum Projekt., ein *Controller* ist das Objekt, das HTTP-Anforderungen für Web-API behandelt.</span><span class="sxs-lookup"><span data-stu-id="48953-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="48953-123">In **Projektmappen-Explorer**, mit der rechten Maustaste in des Projekts.</span><span class="sxs-lookup"><span data-stu-id="48953-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="48953-124">Wählen Sie **neues Element hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48953-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="48953-125">Klicken Sie unter **installierte Vorlagen**, erweitern Sie **Visual C#-** , und wählen Sie **Web**.</span><span class="sxs-lookup"><span data-stu-id="48953-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="48953-126">Wählen Sie dann aus der Liste der Vorlagen, **Web API Controller Class**.</span><span class="sxs-lookup"><span data-stu-id="48953-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="48953-127">Nennen Sie den Controller "ProductsController", und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48953-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="48953-128">Die **neues Element hinzufügen** Assistenten wird eine Datei namens ProductsController.cs erstellt.</span><span class="sxs-lookup"><span data-stu-id="48953-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="48953-129">Löschen Sie die Methoden, die der Assistent enthalten, und fügen Sie die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="48953-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="48953-130">Weitere Informationen zu den Code in diesem Controller, finden Sie unter den [Einstieg](tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="48953-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="48953-131">Hinzufügen von Routinginformationen</span><span class="sxs-lookup"><span data-stu-id="48953-131">Add Routing Information</span></span>

<span data-ttu-id="48953-132">Anschließend wir fügen eine URI-Route also diese URIs der Form &quot;/API/Produkte/&quot; an den Controller weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="48953-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="48953-133">In **Projektmappen-Explorer**, doppelklicken Sie auf "Global.asax", um den Code-Behind-Datei "Global.asax.cs" zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="48953-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="48953-134">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="48953-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="48953-135">Klicken Sie dann fügen Sie den folgenden Code der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="48953-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="48953-136">Weitere Informationen zu Routingtabellen finden Sie unter [Routing in ASP.NET Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="48953-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="48953-137">Hinzufügen der clientseitigen AJAX</span><span class="sxs-lookup"><span data-stu-id="48953-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="48953-138">Das ist alles, die müssen Sie eine Web-API zu erstellen, die Clients zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="48953-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="48953-139">Nun fügen Sie eine HTML-Seite, die jQuery zum Aufrufen der API verwendet.</span><span class="sxs-lookup"><span data-stu-id="48953-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="48953-140">Stellen Sie sicher, dass die Masterseite (z. B. *Site.Master*) umfasst eine `ContentPlaceHolder` mit `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="48953-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="48953-141">Öffnen Sie die Datei "default.aspx".</span><span class="sxs-lookup"><span data-stu-id="48953-141">Open the file Default.aspx.</span></span> <span data-ttu-id="48953-142">Ersetzen Sie den Standardtext, der im Abschnitt Inhalt ist an, wie dargestellt:</span><span class="sxs-lookup"><span data-stu-id="48953-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="48953-143">Fügen Sie einen Verweis auf die jQuery-Quelldatei in die `HeaderContent` Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="48953-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="48953-144">Hinweis: Sie können problemlos hinzufügen den Skriptverweis durch Ziehen und Ablegen der Datei aus **Projektmappen-Explorer** in das Fenster des Code-Editor.</span><span class="sxs-lookup"><span data-stu-id="48953-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="48953-145">Fügen Sie den nachstehenden Skriptbaustein unterhalb des jQuery-Skript-Tags hinzu:</span><span class="sxs-lookup"><span data-stu-id="48953-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="48953-146">Wenn das Dokument geladen wird, wird dieses Skript eine AJAX-Anforderung an &quot;-api/Produkte&quot;.</span><span class="sxs-lookup"><span data-stu-id="48953-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="48953-147">Die Anforderung gibt eine Liste der Produkte im JSON-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="48953-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="48953-148">Das Skript fügt die Produktinformationen in den HTML-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="48953-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="48953-149">Wenn Sie die Anwendung ausführen, sollte es wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="48953-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
