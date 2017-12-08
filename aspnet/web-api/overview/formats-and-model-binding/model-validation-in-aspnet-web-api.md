---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model-Validierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="2126a-102">Modellvalidierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="2126a-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2126a-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2126a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2126a-104">Wenn ein Client Daten an Ihre Web-API sendet, möchten Sie häufig zum Überprüfen der Daten vor dem Ausführen der Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="2126a-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="2126a-105">In diesem Artikel werden die Kommentieren die Modelle, verwenden die Anmerkungen zur datenüberprüfung und Behandeln von Validierungsfehlern in Ihre Web-API veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="2126a-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2126a-106">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="2126a-106">Data Annotations</span></span>

<span data-ttu-id="2126a-107">In ASP.NET Web-API können Sie Attribute aus der [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) Namespace Überprüfungsregeln für Eigenschaften für das Modell festlegen.</span><span class="sxs-lookup"><span data-stu-id="2126a-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="2126a-108">Betrachten Sie das folgende Modell:</span><span class="sxs-lookup"><span data-stu-id="2126a-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2126a-109">Wenn Sie modellvalidierung in ASP.NET MVC verwendet haben, sollte dies bekannt vorkommen.</span><span class="sxs-lookup"><span data-stu-id="2126a-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="2126a-110">Die **erforderlich** Attribut besagt, dass die `Name` Eigenschaft darf nicht null sein.</span><span class="sxs-lookup"><span data-stu-id="2126a-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="2126a-111">Die **Bereich** Attribut laut `Weight` muss zwischen 0 und 999 liegen.</span><span class="sxs-lookup"><span data-stu-id="2126a-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="2126a-112">Nehmen wir an, dass ein Client eine POST-Anforderung mit der folgenden JSON-Darstellung sendet:</span><span class="sxs-lookup"><span data-stu-id="2126a-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="2126a-113">Beachten Sie, dass der Client nicht eingeschlossen war die `Name` -Eigenschaft, die als markiert ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2126a-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="2126a-114">Beim Web-API in JSON konvertiert eine `Product` Instanz überprüft die `Product` gegen die Validierungsattribute.</span><span class="sxs-lookup"><span data-stu-id="2126a-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="2126a-115">In der Controlleraktion können Sie überprüfen, ob das Modell gültig ist:</span><span class="sxs-lookup"><span data-stu-id="2126a-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="2126a-116">Modellvalidierung garantiert nicht, dass Clientdaten sicher ist.</span><span class="sxs-lookup"><span data-stu-id="2126a-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="2126a-117">Zusätzliche Validierung möglicherweise in anderen Ebenen der Anwendung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="2126a-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="2126a-118">Die Datenschicht (z. B. u. u. foreign Key-Einschränkungen eingehalten werden.) Das Lernprogramm [mithilfe des Web-API mit Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) untersucht einige dieser Probleme.</span><span class="sxs-lookup"><span data-stu-id="2126a-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="2126a-119">**"Unzureichende Buchung"**: Unzureichende Buchung tritt auf, wenn der Client und einige Eigenschaften zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="2126a-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="2126a-120">Nehmen Sie z. B. an, dass der Client die folgenden sendet:</span><span class="sxs-lookup"><span data-stu-id="2126a-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="2126a-121">Der Client hat hier keinen Werte für `Price` oder `Weight`.</span><span class="sxs-lookup"><span data-stu-id="2126a-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="2126a-122">Der JSON-Formatierer weist den Standardwert 0 (null), um die fehlenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="2126a-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2126a-123">Der Modellzustand ist gültig, da NULL ein gültiger Wert für diese Eigenschaften ist.</span><span class="sxs-lookup"><span data-stu-id="2126a-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="2126a-124">Ob dies ein Problem aufgetreten ist, hängt von Ihrem Szenario ab.</span><span class="sxs-lookup"><span data-stu-id="2126a-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="2126a-125">Beispielsweise kann in einem Updatevorgang sollen zur Unterscheidung zwischen "0" und "nicht festgelegt."</span><span class="sxs-lookup"><span data-stu-id="2126a-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="2126a-126">Um Clients zum Festlegen eines Werts zu erzwingen, stellen Sie die Eigenschaft NULL-Werte zulässt, und legen die **erforderlich** Attribut:</span><span class="sxs-lookup"><span data-stu-id="2126a-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="2126a-127">**"Stark buchen"**: ein Client kann auch senden *weitere* Daten als erwartet.</span><span class="sxs-lookup"><span data-stu-id="2126a-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="2126a-128">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2126a-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="2126a-129">Hier der JSON-Code enthält eine Eigenschaft ("Color"), die in nicht vorhanden ist die `Product` Modell.</span><span class="sxs-lookup"><span data-stu-id="2126a-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="2126a-130">In diesem Fall wird das JSON-Formatierungsprogramm einfach dieser Wert ignoriert.</span><span class="sxs-lookup"><span data-stu-id="2126a-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="2126a-131">(Der XML-Formatierer hat die gleiche Funktion.) Übermäßige Buchung verursacht Probleme, wenn Ihr Modell über Eigenschaften, die Sie verfügt schreibgeschützt sein sollte.</span><span class="sxs-lookup"><span data-stu-id="2126a-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="2126a-132">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2126a-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="2126a-133">Sie nicht möchten, dass Benutzer beim Aktualisieren der `IsAdmin` Eigenschaft und erhöhen Sie selbst auf Administratoren!</span><span class="sxs-lookup"><span data-stu-id="2126a-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="2126a-134">Die sicherste Strategie besteht darin, eine Modellklasse verwenden, die exakt übereinstimmt, was der Client zum Senden darf:</span><span class="sxs-lookup"><span data-stu-id="2126a-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="2126a-135">Brad Wilsons Blog-Post "[Input Validation Vs. Model-Validierung in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"verfügt über eine gute Erörterung von unzureichende Buchung und zu senden.</span><span class="sxs-lookup"><span data-stu-id="2126a-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="2126a-136">Obwohl im Beitrag zu ASP.NET MVC 2 ist, sind die Probleme weiterhin relevant, Web-API.</span><span class="sxs-lookup"><span data-stu-id="2126a-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="2126a-137">Behandeln von Validierungsfehlern</span><span class="sxs-lookup"><span data-stu-id="2126a-137">Handling Validation Errors</span></span>

<span data-ttu-id="2126a-138">Einen Fehler an den Client werden von Web-API nicht automatisch zurück, wenn die Validierung fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="2126a-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="2126a-139">Es liegt im Ermessen der Controlleraktion Modellzustand überprüfen und entsprechend reagieren.</span><span class="sxs-lookup"><span data-stu-id="2126a-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="2126a-140">Sie können auch erstellen, einen Aktionsfilter, um dem Modellzustand zu überprüfen, bevor der Controlleraktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2126a-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="2126a-141">Der folgende Code zeigt ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2126a-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="2126a-142">Wenn die modellvalidierung fehlschlägt, gibt dieser Filter eine HTTP-Antwort, die die Validierungsfehler enthält.</span><span class="sxs-lookup"><span data-stu-id="2126a-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="2126a-143">In diesem Fall wird der Controlleraktion nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2126a-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="2126a-144">Fügen Sie zum Anwenden dieser Filter auf alle Web-API-Controller eine Instanz des Filters, der die **HttpConfiguration.Filters** Auflistung während der Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2126a-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="2126a-145">Eine andere Möglichkeit besteht in der Filter als Attribut auf einzelnen Domänencontrollern oder Controlleraktionen festgelegt:</span><span class="sxs-lookup"><span data-stu-id="2126a-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
