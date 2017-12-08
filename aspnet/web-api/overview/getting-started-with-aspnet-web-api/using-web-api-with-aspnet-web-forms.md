---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Mithilfe von Web-API mit ASP.NET Web Forms | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="300a1-102">Mithilfe von Web-API mit ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="300a1-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="300a1-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="300a1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="300a1-104">Obwohl der ASP.NET Web-API mit ASP.NET MVC verpackt wird, ist es einfach, Web-API zu einer herkömmlichen ASP.NET Web Forms-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="300a1-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="300a1-105">Dieses Lernprogramm führt Sie durch die Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="300a1-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="300a1-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="300a1-106">Overview</span></span>

<span data-ttu-id="300a1-107">Um Web-API in einer Web Forms-Anwendung zu verwenden, gibt es zwei Hauptschritte:</span><span class="sxs-lookup"><span data-stu-id="300a1-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="300a1-108">Fügen Sie einen Web-API-Controller, die von abgeleitet ist die **ApiController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="300a1-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="300a1-109">Hinzufügen einer Routentabelle zu den **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="300a1-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="300a1-110">Erstellen eines Web Forms-Anwendungsprojekts</span><span class="sxs-lookup"><span data-stu-id="300a1-110">Create a Web Forms Project</span></span>

<span data-ttu-id="300a1-111">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="300a1-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="300a1-112">Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="300a1-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="300a1-113">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="300a1-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="300a1-114">Klicken Sie unter **Visual C#-**Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="300a1-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="300a1-115">Wählen Sie in der Liste der Projektvorlagen **ASP.NET Web Forms-Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="300a1-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="300a1-116">Geben Sie einen Namen für das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="300a1-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="300a1-117">Erstellen Sie das Modell und der Controller</span><span class="sxs-lookup"><span data-stu-id="300a1-117">Create the Model and Controller</span></span>

<span data-ttu-id="300a1-118">Dieses Lernprogramm verwendet die gleichen Modell und Controller-Klassen wie der [Einstieg](tutorial-your-first-web-api.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="300a1-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="300a1-119">Fügen Sie zuerst eine Modellklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="300a1-119">First, add a model class.</span></span> <span data-ttu-id="300a1-120">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="300a1-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="300a1-121">Benennen Sie die Klasse Product, und fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="300a1-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="300a1-122">Fügen Sie einen Web-API-Controller zu dem Projekt. eine *Controller* ist das Objekt, das HTTP-Anforderungen für die Web-API behandelt.</span><span class="sxs-lookup"><span data-stu-id="300a1-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="300a1-123">In **Projektmappen-Explorer**, mit der rechten Maustaste des Projekts.</span><span class="sxs-lookup"><span data-stu-id="300a1-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="300a1-124">Wählen Sie **neues Element hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="300a1-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="300a1-125">Klicken Sie unter **installierte Vorlagen**, erweitern Sie **Visual C#-** , und wählen Sie **Web**.</span><span class="sxs-lookup"><span data-stu-id="300a1-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="300a1-126">Wählen Sie dann aus der Liste der Vorlagen, **Web-API-Controllerklasse**.</span><span class="sxs-lookup"><span data-stu-id="300a1-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="300a1-127">Nennen Sie den Controller "ProductsController", und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="300a1-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="300a1-128">Die **neues Element hinzufügen** Assistenten wird eine Datei namens ProductsController.cs erstellt.</span><span class="sxs-lookup"><span data-stu-id="300a1-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="300a1-129">Löschen Sie die Methoden, die der Assistent enthalten, und fügen Sie die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="300a1-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="300a1-130">Weitere Informationen zu den Code in diesem Controller, finden Sie unter der [Einstieg](tutorial-your-first-web-api.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="300a1-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="300a1-131">Hinzufügen von Routinginformationen</span><span class="sxs-lookup"><span data-stu-id="300a1-131">Add Routing Information</span></span>

<span data-ttu-id="300a1-132">Als Nächstes wir fügen eine URI-Route also dieses URIs im Format &quot;/API/Produkte/&quot; an den Controller weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="300a1-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="300a1-133">In **Projektmappen-Explorer**, doppelklicken Sie auf "Global.asax", um die Code-Behind-Datei Global.asax.cs zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="300a1-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="300a1-134">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="300a1-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="300a1-135">Fügen Sie folgenden Code, der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="300a1-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="300a1-136">Weitere Informationen zu Routingtabellen, finden Sie unter [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="300a1-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="300a1-137">Fügen Sie die clientseitige AJAX</span><span class="sxs-lookup"><span data-stu-id="300a1-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="300a1-138">Das ist alles, die müssen Sie eine Web-API zu erstellen, die Clients zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="300a1-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="300a1-139">Nun fügen wir eine HTML-Seite, jQuery verwendet wird, zum Aufrufen der API, hinzu.</span><span class="sxs-lookup"><span data-stu-id="300a1-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="300a1-140">Stellen Sie sicher, dass die Gestaltungsvorlage (z. B. *Site.Master*) umfasst eine `ContentPlaceHolder` mit `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="300a1-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="300a1-141">Öffnen Sie die Datei "default.aspx".</span><span class="sxs-lookup"><span data-stu-id="300a1-141">Open the file Default.aspx.</span></span> <span data-ttu-id="300a1-142">Ersetzen Sie den Standardtext, der Sie im Abschnitt Inhalt ist an, wie dargestellt:</span><span class="sxs-lookup"><span data-stu-id="300a1-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="300a1-143">Fügen Sie einen Verweis auf die jQuery-Quelldatei in die `HeaderContent` Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="300a1-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="300a1-144">Hinweis: Sie können problemlos hinzufügen Skriptverweis per Drag & Drop die Datei aus **Projektmappen-Explorer** in das Fenster des Code-Editor.</span><span class="sxs-lookup"><span data-stu-id="300a1-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="300a1-145">Fügen Sie die folgenden Skriptblock hinzu, unter dem jQuery-Skript-Tag:</span><span class="sxs-lookup"><span data-stu-id="300a1-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="300a1-146">Wenn das Dokument geladen wird, wird dieses Skript eine AJAX-Anforderung an &quot;-api-Produkte&quot;.</span><span class="sxs-lookup"><span data-stu-id="300a1-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="300a1-147">Die Anforderung gibt eine Liste der Produkte im JSON-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="300a1-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="300a1-148">Das Skript fügt die Produktinformationen der HTML-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="300a1-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="300a1-149">Wenn Sie die Anwendung ausführen, sollten sie wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="300a1-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
