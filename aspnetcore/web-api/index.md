---
title: Erstellen von Web-APIs mit ASP.NET Core
author: scottaddie
description: Lernen Sie die verfügbaren Features zum Erstellen einer Web-API in ASP.NET Core kennen, und erhalten Sie Informationen zur Verwendung dieser Features.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822139"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="66e1c-103">Erstellen von Web-APIs mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66e1c-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="66e1c-104">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="66e1c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="66e1c-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66e1c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66e1c-106">In diesem Dokument wird erläutert, wie eine Web-API in ASP.NET Core erstellt werden kann und wann jedes Feature am besten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="66e1c-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="66e1c-107">Ableiten einer Klasse von ControllerBase</span><span class="sxs-lookup"><span data-stu-id="66e1c-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="66e1c-108">Leiten Sie Inhalte der <xref:Microsoft.AspNetCore.Mvc.ControllerBase>-Klasse in einem Controller ab, der als Web-API fungieren soll.</span><span class="sxs-lookup"><span data-stu-id="66e1c-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="66e1c-109">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="66e1c-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="66e1c-110">Die `ControllerBase`-Klasse ermöglicht den Zugriff auf mehrere Eigenschaften und Methoden.</span><span class="sxs-lookup"><span data-stu-id="66e1c-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="66e1c-111">Zu den Beispielen im vorherigen Code zählen <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="66e1c-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="66e1c-112">Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück.</span><span class="sxs-lookup"><span data-stu-id="66e1c-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="66e1c-113">Ein Zugriff erfolgt auf die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="66e1c-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="66e1c-114">Kommentieren einer Klasse mithilfe von ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="66e1c-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="66e1c-115">In ASP.NET Core 2.1 wird das Attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben.</span><span class="sxs-lookup"><span data-stu-id="66e1c-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="66e1c-116">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="66e1c-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="66e1c-117">Eine Kompatibilitätsversion von 2.1 oder höher, die über <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> festgelegt wird, ist für die Verwendung dieses Attributs erforderlich.</span><span class="sxs-lookup"><span data-stu-id="66e1c-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="66e1c-118">Beispielsweise legt der hervorgehobene Code in *Startup.ConfigureServices* das Kompatibilitätsflag 2.1 fest:</span><span class="sxs-lookup"><span data-stu-id="66e1c-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="66e1c-119">Weitere Informationen finden Sie unter <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="66e1c-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="66e1c-120">Das `[ApiController]`-Attribut ist in der Regel mit `ControllerBase` gekoppelt, um das REST-spezifische Verhalten für Controller zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="66e1c-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="66e1c-121">Mit `ControllerBase` kann auf Methoden wie <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="66e1c-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="66e1c-122">Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:</span><span class="sxs-lookup"><span data-stu-id="66e1c-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="66e1c-123">In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="66e1c-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="66e1c-124">Automatische HTTP 400-Antwort</span><span class="sxs-lookup"><span data-stu-id="66e1c-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="66e1c-125">Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="66e1c-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="66e1c-126">Der nachfolgende Code wird in Ihren Aktionen überflüssig:</span><span class="sxs-lookup"><span data-stu-id="66e1c-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="66e1c-127">Das Standardverhalten wird deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="66e1c-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="66e1c-128">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="66e1c-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="66e1c-129">Rückschluss auf Bindungsquellparameter</span><span class="sxs-lookup"><span data-stu-id="66e1c-129">Binding source parameter inference</span></span>

<span data-ttu-id="66e1c-130">Ein Bindungsquellenattribut definiert den Speicherort, an dem der Wert eines Aktionsparameters vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="66e1c-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="66e1c-131">Es gibt die folgenden Bindungsquellattribute:</span><span class="sxs-lookup"><span data-stu-id="66e1c-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="66e1c-132">Attribut</span><span class="sxs-lookup"><span data-stu-id="66e1c-132">Attribute</span></span>|<span data-ttu-id="66e1c-133">Bindungsquelle</span><span class="sxs-lookup"><span data-stu-id="66e1c-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="66e1c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="66e1c-135">Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="66e1c-135">Request body</span></span> |
|<span data-ttu-id="66e1c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="66e1c-137">Formulardaten im Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="66e1c-137">Form data in the request body</span></span> |
|<span data-ttu-id="66e1c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="66e1c-139">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="66e1c-139">Request header</span></span> |
|<span data-ttu-id="66e1c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="66e1c-141">Abfragezeichenfolge-Parameter der Anforderung</span><span class="sxs-lookup"><span data-stu-id="66e1c-141">Request query string parameter</span></span> |
|<span data-ttu-id="66e1c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="66e1c-143">Routendaten aus aktuellen Anforderungen</span><span class="sxs-lookup"><span data-stu-id="66e1c-143">Route data from the current request</span></span> |
|<span data-ttu-id="66e1c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="66e1c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="66e1c-145">Der als Aktionsparameter eingefügte Anforderungsdienst.</span><span class="sxs-lookup"><span data-stu-id="66e1c-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="66e1c-146">Verwenden Sie `[FromRoute]` nicht, wenn Werte möglicherweise `%2f` enthalten (d.h. `/`).</span><span class="sxs-lookup"><span data-stu-id="66e1c-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="66e1c-147">`%2f` wird nicht durch Entfernen von Escapezeichen zu `/`.</span><span class="sxs-lookup"><span data-stu-id="66e1c-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="66e1c-148">Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.</span><span class="sxs-lookup"><span data-stu-id="66e1c-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="66e1c-149">Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert.</span><span class="sxs-lookup"><span data-stu-id="66e1c-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="66e1c-150">Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:</span><span class="sxs-lookup"><span data-stu-id="66e1c-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="66e1c-151">Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="66e1c-152">Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden.</span><span class="sxs-lookup"><span data-stu-id="66e1c-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="66e1c-153">Die Bindungsquellenattribute verhalten sich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="66e1c-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="66e1c-154">Für komplexe Typparameter wird **[FromBody]** abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="66e1c-155">Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. <xref:Microsoft.AspNetCore.Http.IFormCollection> und <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="66e1c-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="66e1c-156">Der Rückschlusscode der Bindungsquelle ignoriert diese Typen.</span><span class="sxs-lookup"><span data-stu-id="66e1c-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="66e1c-157">`[FromBody]` wird für einfache Typen wie `string` oder `int` nicht abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="66e1c-158">Deshalb sollte das `[FromBody]`-Attribut für einfache Typen verwendet werden, wenn diese Funktionsweise gewünscht ist.</span><span class="sxs-lookup"><span data-stu-id="66e1c-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="66e1c-159">Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="66e1c-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="66e1c-160">Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:</span><span class="sxs-lookup"><span data-stu-id="66e1c-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="66e1c-161">**[FromForm]** wird für Aktionsparameter des Typs <xref:Microsoft.AspNetCore.Http.IFormFile> und <xref:Microsoft.AspNetCore.Http.IFormFileCollection> abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="66e1c-162">Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="66e1c-163">**[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="66e1c-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="66e1c-164">Wenn mehr als eine Route mit einem Aktionsparameter übereinstimmt, gilt jeder Routenwert als `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="66e1c-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="66e1c-165">**[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="66e1c-166">Die standardmäßig festgelegten Rückschlussregeln werden deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="66e1c-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="66e1c-167">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="66e1c-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="66e1c-168">Ableiten der Multipart/form-data-Anforderung</span><span class="sxs-lookup"><span data-stu-id="66e1c-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="66e1c-169">Wenn ein Aktionsparameter mit dem [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="66e1c-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="66e1c-170">Das Standardverhalten wird deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="66e1c-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="66e1c-171">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="66e1c-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="66e1c-172">Anforderung für das Attributrouting</span><span class="sxs-lookup"><span data-stu-id="66e1c-172">Attribute routing requirement</span></span>

<span data-ttu-id="66e1c-173">Das Attributrouting wird zu einer Anforderung.</span><span class="sxs-lookup"><span data-stu-id="66e1c-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="66e1c-174">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="66e1c-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="66e1c-175">Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> oder von <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure* definiert sind, nicht auf Aktionen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="66e1c-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="66e1c-176">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="66e1c-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
