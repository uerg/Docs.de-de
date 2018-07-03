---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Kapselung in OData v4-Web-API 2.2 mit | Microsoft-Dokumentation
author: rick-anderson
description: In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden. OData v4 stellt zwei zusätzliche Optionen, die Singleton-als auch Nachteile jedoch...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365606"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="97fb3-104">Kapselung in OData v4 mithilfe von Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="97fb3-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="97fb3-105">durch Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="97fb3-105">by Jinfu Tan</span></span>

> <span data-ttu-id="97fb3-106">In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden.</span><span class="sxs-lookup"><span data-stu-id="97fb3-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="97fb3-107">OData v4 stellt zwei zusätzliche Optionen, Singleton und Kapselung, beide Web-API 2.2 unterstützt jedoch.</span><span class="sxs-lookup"><span data-stu-id="97fb3-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="97fb3-108">In diesem Thema veranschaulicht, wie eine Kapselung in einem OData-Endpunkt in der Web-API 2.2 zu definieren.</span><span class="sxs-lookup"><span data-stu-id="97fb3-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="97fb3-109">Weitere Informationen zu Kapselung, finden Sie unter [Kapselung kommt mit OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="97fb3-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="97fb3-110">Zum Erstellen eines OData V4-Endpunkts in Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="97fb3-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="97fb3-111">Zunächst erstellen ein Domänenmodell für die Kapselung in OData-Diensts wir mit diesem Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="97fb3-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Datenmodell](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="97fb3-113">Ein Konto enthält viele PaymentInstruments (PI), aber nicht definieren wir eine Entitätenmenge für einen PI.</span><span class="sxs-lookup"><span data-stu-id="97fb3-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="97fb3-114">Stattdessen können der PIs nur über ein Konto zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="97fb3-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="97fb3-115">Sie können die Lösung, die in diesem Thema verwendet [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="97fb3-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="97fb3-116">Definieren des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="97fb3-116">Defining the data model</span></span>

1. <span data-ttu-id="97fb3-117">Definieren Sie die CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="97fb3-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="97fb3-118">Die `Contained` Attribut wird für Navigationseigenschaften Containment verwendet.</span><span class="sxs-lookup"><span data-stu-id="97fb3-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="97fb3-119">Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="97fb3-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="97fb3-120">Die `ODataConventionModelBuilder` übernimmt das EDM-Modell erstellen, wenn die `Contained` Attribut wird die entsprechende Navigationseigenschaft hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="97fb3-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="97fb3-121">Wenn die Eigenschaft ein Sammlungstyp ist eine `GetCount(string NameContains)` Funktion wird auch erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="97fb3-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="97fb3-122">Die generierte Metadaten sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="97fb3-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="97fb3-123">Die `ContainsTarget` Attribut gibt an, dass die Navigationseigenschaft eine Kapselung ist.</span><span class="sxs-lookup"><span data-stu-id="97fb3-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="97fb3-124">Definieren des enthaltenden Entität Set-Controllers</span><span class="sxs-lookup"><span data-stu-id="97fb3-124">Define the containing entity set controller</span></span>

<span data-ttu-id="97fb3-125">Enthaltene Entitäten keine eigenen Controller; die Aktion, die im enthaltenden Entität Satz Controller definiert ist.</span><span class="sxs-lookup"><span data-stu-id="97fb3-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="97fb3-126">In diesem Beispiel ist ein AccountsController, aber keine PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="97fb3-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="97fb3-127">Ist der OData-Pfad mindestens 4 Segmente, nur Attribut routing funktioniert, z. B. `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` im oben genannten Controller.</span><span class="sxs-lookup"><span data-stu-id="97fb3-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="97fb3-128">Andernfalls sowohl Attribut als auch beim herkömmlichen routing funktioniert: z. B. `GetPayInPIs(int key)` entspricht `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="97fb3-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="97fb3-129">*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*</span><span class="sxs-lookup"><span data-stu-id="97fb3-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
