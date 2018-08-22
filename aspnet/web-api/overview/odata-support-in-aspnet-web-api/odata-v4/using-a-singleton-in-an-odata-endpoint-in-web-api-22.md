---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen eines Singletons in OData v4 mithilfe von Web-API 2.2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Thema veranschaulicht definieren Sie einen Singleton in einem OData-Endpunkt im Web API 2.2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7562a90ae34b216dca2dd3cf541d086585735212
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826869"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="8d98d-103">Erstellen eines Singletons in OData v4 mithilfe von Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="8d98d-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="8d98d-104">durch Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="8d98d-104">by Zoe Luo</span></span>

> <span data-ttu-id="8d98d-105">In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden.</span><span class="sxs-lookup"><span data-stu-id="8d98d-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="8d98d-106">OData v4 stellt zwei zusätzliche Optionen, Singleton und Kapselung, beide Web-API 2.2 unterstützt jedoch.</span><span class="sxs-lookup"><span data-stu-id="8d98d-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="8d98d-107">In diesem Artikel zeigt, wie einen Singleton in einem OData-Endpunkt im Web API 2.2 definiert wird.</span><span class="sxs-lookup"><span data-stu-id="8d98d-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="8d98d-108">Informationen dazu, welche ein Singleton ist und wie Sie aus Ihrer Verwendung profitieren können, finden Sie unter [mit, dass ein Singleton spezielle Entität definieren](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d98d-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="8d98d-109">Zum Erstellen eines OData V4-Endpunkts in Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8d98d-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="8d98d-110">Wir erstellen einen Singleton in Ihrem Web-API-Projekt mit dem folgenden Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="8d98d-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="8d98d-112">Einen Singleton namens `Umbrella` werden basierend auf Typ definiert `Company`, und einer Entität mit dem Namen `Employees` werden basierend auf Typ definiert `Employee`.</span><span class="sxs-lookup"><span data-stu-id="8d98d-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="8d98d-113">Die Lösung, die in diesem Tutorial verwendete kann heruntergeladen werden [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="8d98d-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="8d98d-114">Definieren des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="8d98d-114">Define the data model</span></span>

1. <span data-ttu-id="8d98d-115">Definieren Sie die CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="8d98d-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="8d98d-116">Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="8d98d-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="8d98d-117">Hier `builder.Singleton<Company>("Umbrella")` weist den Modellgenerator, erstellen Sie einen Singleton namens `Umbrella` in das EDM-Modell.</span><span class="sxs-lookup"><span data-stu-id="8d98d-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="8d98d-118">Die generierte Metadaten sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="8d98d-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="8d98d-119">Aus den Metadaten sehen, dass die Navigationseigenschaft `Company` in die `Employees` Entitätenmenge gebunden ist, auf das Singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="8d98d-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="8d98d-120">Die Bindung erfolgt automatisch durch `ODataConventionModelBuilder`, da nur `Umbrella` hat die `Company` Typ.</span><span class="sxs-lookup"><span data-stu-id="8d98d-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="8d98d-121">Wenn im Modell eine Mehrdeutigkeit vorliegt, können Sie `HasSingletonBinding` , eine Navigationseigenschaft explizit in ein Singleton; binden `HasSingletonBinding` hat dieselbe Wirkung wie die Verwendung der `Singleton` Attribut in der Definition der CLR-Typ:</span><span class="sxs-lookup"><span data-stu-id="8d98d-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="8d98d-122">Definieren der Singleton-Controllers</span><span class="sxs-lookup"><span data-stu-id="8d98d-122">Define the singleton controller</span></span>

<span data-ttu-id="8d98d-123">Wie auch der EntitySet-Controller, erbt der Controller Singleton `ODataController`, und die Singleton-Controller-Name sollte `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="8d98d-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="8d98d-124">Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vorab definiert werden.</span><span class="sxs-lookup"><span data-stu-id="8d98d-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="8d98d-125">**Attributrouting** in Web-API 2.2 standardmäßig aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="8d98d-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="8d98d-126">Beispielsweise, um eine Aktion zum Verarbeiten von Abfragen definieren `Revenue` aus `Company` das attributrouting, verwenden Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8d98d-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="8d98d-127">Wenn Sie nicht zum Definieren von Attributen für jede Aktion bereit sind, definieren Sie einfach die folgenden Aktionen [OData-Konventionen für Routing](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="8d98d-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="8d98d-128">Da ein Schlüssel nicht erforderlich, für die Abfrage eines Singleton ist, sind die Aktionen, die in der Singleton-Controller definierten geringfügig von den Aktionen im Controller Entityset definiert.</span><span class="sxs-lookup"><span data-stu-id="8d98d-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="8d98d-129">Zu Referenzzwecken Methodensignaturen für jede Aktionsdefinition im Controller Singleton weiter unten.</span><span class="sxs-lookup"><span data-stu-id="8d98d-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="8d98d-130">Im Grunde ist dies alles, was, die Sie auf der Dienstseite tun müssen.</span><span class="sxs-lookup"><span data-stu-id="8d98d-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="8d98d-131">Die [Beispielprojekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält alle von der Code für die Projektmappe und der OData-Client, der zeigt, wie Sie mit der Singleton.</span><span class="sxs-lookup"><span data-stu-id="8d98d-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="8d98d-132">Der Client wird erstellt, indem Sie die Schritte in [erstellen Sie eine Client-App in OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="8d98d-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="8d98d-133">sein.</span><span class="sxs-lookup"><span data-stu-id="8d98d-133">.</span></span> 

<span data-ttu-id="8d98d-134">*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*</span><span class="sxs-lookup"><span data-stu-id="8d98d-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
