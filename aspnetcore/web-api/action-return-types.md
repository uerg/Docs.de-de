---
title: Rückgabetypen für Controlleraktionen in der ASP.NET Core-Web-API
author: scottaddie
description: In diesem Artikel erfahren Sie, wie die verschiedenen Rückgabetypen für Controlleraktionsmethoden in einer ASP.NET Core-Web-API verwendet werden.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 179a3e23ebc13a40b8e2d955b6adcc23d9a0f323
ms.sourcegitcommit: 8268cc67beb1bb1ca470abb0e28b15a7a71b8204
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44126721"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="66a0d-103">Rückgabetypen für Controlleraktionen in der ASP.NET Core-Web-API</span><span class="sxs-lookup"><span data-stu-id="66a0d-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="66a0d-104">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="66a0d-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="66a0d-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66a0d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66a0d-106">In ASP.NET Core haben Sie die folgenden Optionen für Rückgabetypen für Web-API-Controlleraktionen:</span><span class="sxs-lookup"><span data-stu-id="66a0d-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="66a0d-107">Spezifischer Typ</span><span class="sxs-lookup"><span data-stu-id="66a0d-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="66a0d-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="66a0d-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="66a0d-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="66a0d-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="66a0d-110">Spezifischer Typ</span><span class="sxs-lookup"><span data-stu-id="66a0d-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="66a0d-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="66a0d-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="66a0d-112">In diesem Artikel wird die Verwendung der einzelnen Rückgabetypen erklärt.</span><span class="sxs-lookup"><span data-stu-id="66a0d-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="66a0d-113">Spezifischer Typ</span><span class="sxs-lookup"><span data-stu-id="66a0d-113">Specific type</span></span>

<span data-ttu-id="66a0d-114">Die einfachste Aktion gibt einen primitiven oder komplexen Datentyp zurück (z.B. `string` oder einen benutzerdefinierten Objekttyp).</span><span class="sxs-lookup"><span data-stu-id="66a0d-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="66a0d-115">Die folgende Aktion gibt eine Auflistung benutzerdefinierter `Product`-Objekte zurück:</span><span class="sxs-lookup"><span data-stu-id="66a0d-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="66a0d-116">Ohne bekannte Bedingungen, gegen die während der Ausführung einer Aktion Schutzmaßnahmen ergriffen werden müssen, reicht die Rückgabe eines spezifischen Typs möglicherweise aus.</span><span class="sxs-lookup"><span data-stu-id="66a0d-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="66a0d-117">Die vorherige Aktion akzeptiert keine Parameter, weshalb eine Validierung von Parametereinschränkungen nicht erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="66a0d-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="66a0d-118">Wenn bekannte Bedingungen bei einer Aktion berücksichtig werden müssen, werden mehrere Rückgabepfade eingeführt.</span><span class="sxs-lookup"><span data-stu-id="66a0d-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="66a0d-119">In diesem Fall ist es üblich, einen [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult)-Rückgabetyp mit dem primitiven oder komplexen Rückgabetyp zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="66a0d-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="66a0d-120">Für diesen Aktionstyp sind entweder der Rückgabetyp [IActionResult](#iactionresult-type) oder [ActionResult\<T>](#actionresultt-type) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="66a0d-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="66a0d-121">IActionResult-Typ</span><span class="sxs-lookup"><span data-stu-id="66a0d-121">IActionResult type</span></span>

<span data-ttu-id="66a0d-122">Der Rückgabetyp [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) eignet sich in Fällen, in denen mehrere [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult)-Rückgabetypen in einer Aktion möglich sind.</span><span class="sxs-lookup"><span data-stu-id="66a0d-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="66a0d-123">Die `ActionResult`-Typen stellen verschiedene HTTP-Statuscodes dar.</span><span class="sxs-lookup"><span data-stu-id="66a0d-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="66a0d-124">Einige gängige Rückgabetypen, die in diese Kategorie fallen, sind etwa [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) und [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="66a0d-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="66a0d-125">Da es bei einer Aktion mehrere Rückgabetypen und Pfade gibt, sollte das Attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) großzügig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="66a0d-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="66a0d-126">Dieses Attribut erzeugt aussagekräftigere Antwortdetails für API-Hilfeseiten, die mit Tools wie [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="66a0d-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="66a0d-127">`[ProducesResponseType]` gibt die bekannten Typen und HTTP-Statuscodes an, die von der Aktion zurückgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="66a0d-128">Synchrone Aktion</span><span class="sxs-lookup"><span data-stu-id="66a0d-128">Synchronous action</span></span>

<span data-ttu-id="66a0d-129">Bei der folgenden synchronen Aktion gibt es zwei mögliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="66a0d-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="66a0d-130">In der vorherigen Aktion wird der Statuscode 404 zurückgegeben, wenn das durch `id` dargestellte Produkt im zugrunde liegenden Datenspeicher nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="66a0d-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="66a0d-131">Die Hilfsmethode [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) wird als Verknüpfung zu `return new NotFoundResult();` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="66a0d-132">Ist das Produkt vorhanden, wird ein `Product`-Objekt, das die Nutzlast darstellt, mit dem Statuscode 200 zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="66a0d-133">Die Hilfsmethode [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) wird als Kurzform für `return new OkObjectResult(product);` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="66a0d-134">Asynchrone Aktion</span><span class="sxs-lookup"><span data-stu-id="66a0d-134">Asynchronous action</span></span>

<span data-ttu-id="66a0d-135">Bei der folgenden asynchronen Aktion gibt es zwei mögliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="66a0d-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="66a0d-136">Bei der vorherigen Aktion wird der Statuscode 400 zurückgegeben, wenn die Modellvalidierung fehlschlägt und die Hilfsmethode [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="66a0d-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="66a0d-137">Das folgende Modell gibt beispielsweise an, dass Anforderungen die `Name`-Eigenschaft und einen Wert bereitstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="66a0d-138">Aus diesem Grund führt die fehlgeschlagene Bereitstellung eines geeigneten `Name` in der Anforderung zu einer fehlgeschlagenen Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="66a0d-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="66a0d-139">Der zweite bekannte Rückgabecode der vorherigen Aktion lautet 201 und wird von der Hilfsmethode [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) erzeugt.</span><span class="sxs-lookup"><span data-stu-id="66a0d-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="66a0d-140">In diesem Pfad wird das `Product`-Objekt zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="66a0d-141">ActionResult\<T>-Typ</span><span class="sxs-lookup"><span data-stu-id="66a0d-141">ActionResult\<T> type</span></span>

<span data-ttu-id="66a0d-142">In ASP.NET Core 2.1 wird der Rückgabetyp [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) für Web-API-Controlleraktionen eingeführt.</span><span class="sxs-lookup"><span data-stu-id="66a0d-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="66a0d-143">Damit wird die Rückgabe eines von [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) abgeleiteten Typs oder eines [spezifischen Typs](#specific-type) ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="66a0d-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="66a0d-144">`ActionResult<T>` besitzt gegenüber dem [IActionResult-Typ](#iactionresult-type) die folgenden Vorteile:</span><span class="sxs-lookup"><span data-stu-id="66a0d-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="66a0d-145">Die `Type`-Eigenschaft des [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute)-Attributs kann ausgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="66a0d-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="66a0d-146">`[ProducesResponseType(200, Type = typeof(Product))]` wird beispielsweise zu `[ProducesResponseType(200)]` vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="66a0d-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="66a0d-147">Der erwartete Rückgabetyp der Aktion wird stattdessen von `T` in `ActionResult<T>` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66a0d-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="66a0d-148">[Implizite Umwandlungsoperatoren](/dotnet/csharp/language-reference/keywords/implicit) unterstützen die Konvertierung von `T` und `ActionResult` in `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="66a0d-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="66a0d-149">`T` wird in [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) konvertiert, wodurch `return new ObjectResult(T);` in `return T;` vereinfacht wird.</span><span class="sxs-lookup"><span data-stu-id="66a0d-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="66a0d-150">C# unterstützt keine impliziten Umwandlungsoperatoren in Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="66a0d-151">Daher ist die Konvertierung der Schnittstelle in einen konkreten Typ erforderlich, um `ActionResult<T>` verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="66a0d-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="66a0d-152">Die Verwendung von `IEnumerable` im folgenden Beispiel funktioniert also nicht:</span><span class="sxs-lookup"><span data-stu-id="66a0d-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="66a0d-153">Eine Möglichkeit zur Korrektur des oben stehenden Codes besteht darin, `_repository.GetProducts().ToList();` zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="66a0d-154">Die meisten Aktionen haben einen bestimmten Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="66a0d-154">Most actions have a specific return type.</span></span> <span data-ttu-id="66a0d-155">Während der Ausführung der Aktion können unerwartete Bedingungen auftreten, wodurch der spezifische Typ nicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="66a0d-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="66a0d-156">So kann beispielsweise die Modellvalidierung des Eingabeparameters einer Aktion fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="66a0d-157">In diesem Fall wird üblicherweise der entsprechende `ActionResult`-Typ anstatt des spezifischen Typs zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="66a0d-158">Synchrone Aktion</span><span class="sxs-lookup"><span data-stu-id="66a0d-158">Synchronous action</span></span>

<span data-ttu-id="66a0d-159">Bei der folgenden synchronen Aktion gibt es zwei mögliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="66a0d-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="66a0d-160">Im vorherigen Code wird ein Statuscode 404 zurückgegeben, wenn das Produkt in der Datenbank nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="66a0d-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="66a0d-161">Ist das Produkt vorhanden, wird das entsprechende `Product`-Objekt zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="66a0d-162">Vor ASP.NET Core 2.1 hätte die Zeile `return product;` stattdessen `return Ok(product);` gelautet.</span><span class="sxs-lookup"><span data-stu-id="66a0d-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="66a0d-163">Seit ASP.NET Core 2.1 wird der Rückschluss auf die Bindungsquelle des Aktionsparameters aktiviert, wenn eine Controllerklasse mit dem `[ApiController]`-Attribut ausgestattet ist.</span><span class="sxs-lookup"><span data-stu-id="66a0d-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="66a0d-164">Ein Parametername, der dem Namen einer Routenvorlage entspricht, wird automatisch mithilfe der Routendaten der Anforderung gebunden.</span><span class="sxs-lookup"><span data-stu-id="66a0d-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="66a0d-165">Folglich wird der `id`-Parameter der vorherigen Aktion nicht explizit mit dem Attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) versehen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="66a0d-166">Asynchrone Aktion</span><span class="sxs-lookup"><span data-stu-id="66a0d-166">Asynchronous action</span></span>

<span data-ttu-id="66a0d-167">Bei der folgenden asynchronen Aktion gibt es zwei mögliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="66a0d-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="66a0d-168">Schlägt die Modellvalidierung fehlt, wird die [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_)-Methode zur Rückgabe eines Statuscodes 400 aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="66a0d-169">Die [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate)-Eigenschaft wird mit den entsprechenden Validierungsfehlern übergeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="66a0d-170">Bei erfolgreicher Modellvalidierung wird das Produkt in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="66a0d-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="66a0d-171">Der Statuscode 201 wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="66a0d-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="66a0d-172">Seit ASP.NET Core 2.1 wird der Rückschluss auf die Bindungsquelle des Aktionsparameters aktiviert, wenn eine Controllerklasse mit dem `[ApiController]`-Attribut ausgestattet ist.</span><span class="sxs-lookup"><span data-stu-id="66a0d-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="66a0d-173">Komplexe Typparameter werden automatisch mithilfe des Anforderungstexts gebunden.</span><span class="sxs-lookup"><span data-stu-id="66a0d-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="66a0d-174">Folglich wird der `product`-Parameter der vorherigen Aktion nicht explizit mit dem Attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) versehen.</span><span class="sxs-lookup"><span data-stu-id="66a0d-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="66a0d-175">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="66a0d-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
