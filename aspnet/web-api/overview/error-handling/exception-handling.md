---
uid: web-api/overview/error-handling/exception-handling
title: Ausnahmebehandlung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="129ec-102">Ausnahmebehandlung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="129ec-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="129ec-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="129ec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="129ec-104">Dieser Artikel beschreibt die Fehler- und Ausnahmebehandlung in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="129ec-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="129ec-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="129ec-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="129ec-106">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="129ec-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="129ec-107">Ausnahmefilter registrieren</span><span class="sxs-lookup"><span data-stu-id="129ec-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="129ec-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="129ec-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="129ec-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="129ec-109">HttpResponseException</span></span>

<span data-ttu-id="129ec-110">Was geschieht, wenn ein Web-API-Controller eine nicht abgefangene Ausnahme ausgelöst?</span><span class="sxs-lookup"><span data-stu-id="129ec-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="129ec-111">Standardmäßig werden die meisten Ausnahmen in einer HTTP-Antwort mit dem Statuscode 500 Internal Server Error übersetzt.</span><span class="sxs-lookup"><span data-stu-id="129ec-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="129ec-112">Die **HttpResponseException** Typ ist ein Sonderfall.</span><span class="sxs-lookup"><span data-stu-id="129ec-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="129ec-113">Diese Ausnahme gibt alle HTTP-Statuscode an, die Sie in der Ausnahmekonstruktor angeben.</span><span class="sxs-lookup"><span data-stu-id="129ec-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="129ec-114">Beispielsweise gibt die folgende Methode 404 Nichtgefunden wird, zurück, wenn die *Id* -Parameter ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="129ec-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="129ec-115">Mehr Kontrolle über die Antwort kann auch die gesamte Antwortnachricht zu erstellen und fügen Sie ihn mit dem **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="129ec-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="129ec-116">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="129ec-116">Exception Filters</span></span>

<span data-ttu-id="129ec-117">Sie können anpassen, wie Ausnahmen von Web-API durch Schreiben von behandelt eine *Ausnahmefilter*.</span><span class="sxs-lookup"><span data-stu-id="129ec-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="129ec-118">Ein Ausnahmefilter wird ausgeführt, wenn eine Controllermethode nicht behandelte Ausnahme auslöst, wird *nicht* ein **HttpResponseException** Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="129ec-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="129ec-119">Die **HttpResponseException** Typ ist ein Sonderfall, da dieser Standard entwickelt wurde speziell für die Rückgabe einer HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="129ec-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="129ec-120">Ausnahmefilter implementieren die **System.Web.Http.Filters.IExceptionFilter** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="129ec-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="129ec-121">Die einfachste Methode zum Schreiben eines Ausnahmefilters ist die Ableitung der **System.Web.Http.Filters.ExceptionFilterAttribute** Klasse, und überschreiben die **OnException** Methode.</span><span class="sxs-lookup"><span data-stu-id="129ec-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="129ec-122">Ausnahmefilter in ASP.NET Web-API sind in ASP.NET MVC vergleichbar.</span><span class="sxs-lookup"><span data-stu-id="129ec-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="129ec-123">Allerdings werden sie in einem separaten Namespace und die Funktion separat deklariert.</span><span class="sxs-lookup"><span data-stu-id="129ec-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="129ec-124">Insbesondere die **von HandleErrorAttribute** Klasse zur Verwendung in MVC-Web-API-Controller ausgelöste Ausnahmen nicht behandelt.</span><span class="sxs-lookup"><span data-stu-id="129ec-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="129ec-125">Hier ist ein Filter, der konvertiert **NotImplementedException** Ausnahmen in HTTP-Status code 501, nicht implementiert:</span><span class="sxs-lookup"><span data-stu-id="129ec-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="129ec-126">Die **Antwort** Eigenschaft von der **HttpActionExecutedContext** Objekt enthält die HTTP-Antwortnachricht, die an den Client gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="129ec-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="129ec-127">Ausnahmefilter registrieren</span><span class="sxs-lookup"><span data-stu-id="129ec-127">Registering Exception Filters</span></span>

<span data-ttu-id="129ec-128">Es gibt mehrere Möglichkeiten zum Registrieren eines Web-API-Ausnahmefilters aus:</span><span class="sxs-lookup"><span data-stu-id="129ec-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="129ec-129">Von der Aktion</span><span class="sxs-lookup"><span data-stu-id="129ec-129">By action</span></span>
- <span data-ttu-id="129ec-130">Vom Netzwerkcontroller</span><span class="sxs-lookup"><span data-stu-id="129ec-130">By controller</span></span>
- <span data-ttu-id="129ec-131">Global</span><span class="sxs-lookup"><span data-stu-id="129ec-131">Globally</span></span>

<span data-ttu-id="129ec-132">Um den Filter auf eine bestimmte Aktion anwenden, fügen Sie den Filter auf die Aktion als Attribut hinzu:</span><span class="sxs-lookup"><span data-stu-id="129ec-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="129ec-133">Zum Anwenden des Filters für alle Aktionen auf einem Domänencontroller fügen Sie den Filter auf die Controllerklasse als Attribut hinzu:</span><span class="sxs-lookup"><span data-stu-id="129ec-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="129ec-134">Um den Filter Global auf alle Web-API-Controller anzuwenden, fügen Sie eine Instanz des Filters, der die **GlobalConfiguration.Configuration.Filters** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="129ec-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="129ec-135">Batchverarbeitungsorchestrierung Filter in dieser Sammlung gelten für alle Web-API-Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="129ec-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="129ec-136">Wenn Sie die Projektvorlage "ASP.NET MVC 4-Webanwendung" verwenden, um das Projekt zu erstellen, speichern Sie Ihre Web-API-Konfigurationscode innerhalb der `WebApiConfig` -Klasse, die in der App befindet\_Startordner:</span><span class="sxs-lookup"><span data-stu-id="129ec-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="129ec-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="129ec-137">HttpError</span></span>

<span data-ttu-id="129ec-138">Die **HttpError** Objekt bietet eine konsistente Möglichkeit, Fehlerinformationen im Antworttext zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="129ec-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="129ec-139">Im folgende Beispiel wird gezeigt, wie das zurückzugebende HTTP-Statuscode 404 (Nichtgefunden) mit einem **HttpError** im Antworttext.</span><span class="sxs-lookup"><span data-stu-id="129ec-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="129ec-140">**CreateErrorResponse** ist eine Erweiterungsmethode definiert der **System.Net.Http.HttpRequestMessageExtensions** Klasse.</span><span class="sxs-lookup"><span data-stu-id="129ec-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="129ec-141">Intern **CreateErrorResponse** erstellt eine **HttpError** Instanz, und erstellt dann ein **HttpResponseMessage** , enthält die **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="129ec-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="129ec-142">Wenn die Methode erfolgreich ist, gibt es in diesem Beispiel das Produkt in der HTTP-Antwort zurück</span><span class="sxs-lookup"><span data-stu-id="129ec-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="129ec-143">Wenn das angeforderte Produkt nicht gefunden wird, die HTTP-Antwort enthält jedoch eine **HttpError** im Hauptteil Anforderung.</span><span class="sxs-lookup"><span data-stu-id="129ec-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="129ec-144">Die Antwort kann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="129ec-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="129ec-145">Beachten Sie, dass die **HttpError** in diesem Beispiel wird in JSON serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="129ec-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="129ec-146">Ein Vorteil der Verwendung von **HttpError** ist, dass darin über dasselbe [Inhalt Aushandlung](../formats-and-model-binding/content-negotiation.md) und Serialisierungsprozess wie jede andere stark typisiertes Modell.</span><span class="sxs-lookup"><span data-stu-id="129ec-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="129ec-147">HttpError und Modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="129ec-147">HttpError and Model Validation</span></span>

<span data-ttu-id="129ec-148">Für die modellüberprüfung, können Sie der Modellzustand, übergeben **CreateErrorResponse**, die Validierungsfehler in die Antwort eingeschlossen werden sollen:</span><span class="sxs-lookup"><span data-stu-id="129ec-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="129ec-149">In diesem Beispiel wird möglicherweise die folgende Antwort zurück:</span><span class="sxs-lookup"><span data-stu-id="129ec-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="129ec-150">Weitere Informationen zur modellvalidierung finden Sie unter [Modellvalidierung in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="129ec-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="129ec-151">Verwenden von HttpError mit HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="129ec-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="129ec-152">In den vorherigen Beispielen zurückgegeben ein **HttpResponseMessage** Nachricht aus der Controlleraktion, aber Sie können auch **HttpResponseException** zurückzugebenden ein **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="129ec-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="129ec-153">Auf diese Weise können Sie ein stark typisiertes Modell. im Erfolgsfall normalen zurück, bei der Rückgabe von weiterhin **HttpError** , wenn ein Fehler aufgetreten ist:</span><span class="sxs-lookup"><span data-stu-id="129ec-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
