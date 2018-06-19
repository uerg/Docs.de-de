---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen Sie einen Singleton in OData v4 mithilfe von Web-API 2.2 | Microsoft Docs
author: rick-anderson
description: In diesem Thema wird gezeigt, wie einen Singleton in einem OData-Endpunkt im Web-API 2.2 definiert.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508209"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="42f78-103">Erstellen Sie einen Singleton in OData v4 mithilfe von Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="42f78-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="42f78-104">durch Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="42f78-104">by Zoe Luo</span></span>

> <span data-ttu-id="42f78-105">Bisher konnte eine Entität nur zugegriffen werden, wenn es in einer Entitätenmenge gekapselt wurden.</span><span class="sxs-lookup"><span data-stu-id="42f78-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="42f78-106">OData v4 bietet zwei zusätzliche Optionen Singleton und Aufnahme, beide WebAPI 2.2 unterstützt jedoch.</span><span class="sxs-lookup"><span data-stu-id="42f78-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="42f78-107">In diesem Artikel wird gezeigt, wie einen Singleton in einem OData-Endpunkt im Web-API 2.2 definiert.</span><span class="sxs-lookup"><span data-stu-id="42f78-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="42f78-108">Informationen über welche Singleton ist, und erläutert, wie Sie aus ihrer Verwendung profitieren können, finden Sie unter [mit Singleton spezielle Entität definieren](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="42f78-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="42f78-109">Zum Erstellen einer OData V4-Endpunkts in der Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="42f78-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="42f78-110">Wir erstellen einen Singleton in Ihrer Web-API-Projekt mithilfe des folgenden-Datenmodells:</span><span class="sxs-lookup"><span data-stu-id="42f78-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="42f78-112">Ein Singleton, mit dem Namen `Umbrella` basierend auf Typ definiert `Company`, und einer Entität mit der Bezeichnung `Employees` basierend auf Typ definierten `Employee`.</span><span class="sxs-lookup"><span data-stu-id="42f78-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="42f78-113">Die Lösung, die in diesem Lernprogramm verwendete heruntergeladen werden kann [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="42f78-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="42f78-114">Definieren des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="42f78-114">Define the data model</span></span>

1. <span data-ttu-id="42f78-115">Definieren Sie die CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="42f78-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="42f78-116">Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="42f78-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="42f78-117">Hier `builder.Singleton<Company>("Umbrella")` weist den Modellgenerator, erstellen Sie einen Singleton, mit dem Namen `Umbrella` im EDM-Modell.</span><span class="sxs-lookup"><span data-stu-id="42f78-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="42f78-118">Die generierte Metadaten wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="42f78-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="42f78-119">Aus den Metadaten sehen, der die Navigationseigenschaft `Company` in der `Employees` Entitätenmenge an den Singleton gebunden ist `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="42f78-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="42f78-120">Die Bindung erfolgt automatisch vom `ODataConventionModelBuilder`, da nur `Umbrella` hat die `Company` Typ.</span><span class="sxs-lookup"><span data-stu-id="42f78-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="42f78-121">Wenn keine Mehrdeutigkeiten im Modell vorhanden ist, können Sie `HasSingletonBinding` , eine Navigationseigenschaft explizit in ein Singleton; binden `HasSingletonBinding` hat dieselbe Wirkung wie die Verwendung der `Singleton` Attribut in der Definition des CLR-Typ:</span><span class="sxs-lookup"><span data-stu-id="42f78-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="42f78-122">Definieren Sie den Singleton-controller</span><span class="sxs-lookup"><span data-stu-id="42f78-122">Define the singleton controller</span></span>

<span data-ttu-id="42f78-123">Wie den Controller EntitySet erbt die Singleton-Controller von `ODataController`, des Controllernamens Singleton sollte auch `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="42f78-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="42f78-124">Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vorab definiert werden.</span><span class="sxs-lookup"><span data-stu-id="42f78-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="42f78-125">**-Attribut routing** WebApi 2.2 standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="42f78-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="42f78-126">Um beispielsweise eine Aktion zum Verarbeiten von Abfragen definieren `Revenue` aus `Company` mithilfe von routing-Attribut, verwenden Sie die folgenden:</span><span class="sxs-lookup"><span data-stu-id="42f78-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="42f78-127">Wenn Sie nicht zum Definieren von Attributen für jede Aktion bereit sind, definieren Sie nur die folgenden Aktionen [OData-Konventionen für das Routing](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="42f78-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="42f78-128">Da ein Schlüssel nicht erforderlich, zum Abfragen von Singleton ist, werden die Aktionen im Controller Singleton definiert geringfügig von Aktionen im Controller Entityset definiert.</span><span class="sxs-lookup"><span data-stu-id="42f78-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="42f78-129">Zu Referenzzwecken sind Methodensignaturen für jede Aktionsdefinition im Controller Singleton unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="42f78-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="42f78-130">Im Grunde ist dies alles, was, die Sie auf der Dienstseite ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="42f78-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="42f78-131">Die [Beispielprojekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält alle des Codes für die Projektmappe und der OData-Client, der zeigt, wie Sie den Singleton.</span><span class="sxs-lookup"><span data-stu-id="42f78-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="42f78-132">Der Client wird erstellt, indem Sie die Schritte in [erstellen Sie eine Client-App für OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="42f78-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="42f78-133">.</span><span class="sxs-lookup"><span data-stu-id="42f78-133">.</span></span> 

<span data-ttu-id="42f78-134">*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*</span><span class="sxs-lookup"><span data-stu-id="42f78-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
