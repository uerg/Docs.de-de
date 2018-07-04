---
title: Erstellen von Web-APIs mit ASP.NET Core
author: scottaddie
description: Lernen Sie die verfügbaren Features zum Erstellen einer Web-API in ASP.NET Core kennen, und erhalten Sie Informationen zur Verwendung dieser Features.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274965"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="6b7c9-103">Erstellen von Web-APIs mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b7c9-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="6b7c9-104">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6b7c9-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6b7c9-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b7c9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6b7c9-106">In diesem Dokument wird erläutert, wie eine Web-API in ASP.NET Core erstellt werden kann und wann jedes Feature am besten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="6b7c9-107">Ableiten einer Klasse von ControllerBase</span><span class="sxs-lookup"><span data-stu-id="6b7c9-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="6b7c9-108">Leiten Sie Inhalte der [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase)-Klasse in einem Controller ab, der als Web-API fungieren soll.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="6b7c9-109">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="6b7c9-110">Die `ControllerBase`-Klasse ermöglicht den Zugriff auf verschiedene Eigenschaften und Methoden.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="6b7c9-111">Im vorherigen Beispiel sind diese Methoden z.B. [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) und [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="6b7c9-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="6b7c9-112">Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="6b7c9-113">Ein Zugriff erfolgt auf die Eigenschaft [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="6b7c9-114">Kommentieren einer Klasse mithilfe von ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="6b7c9-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="6b7c9-115">In ASP.NET Core 2.1 wird das Attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="6b7c9-116">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="6b7c9-117">Diese Attribut wird häufig mit `ControllerBase` gekoppelt, um auf nützliche Methoden und Eigenschaften zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="6b7c9-118">Mit `ControllerBase` kann auf Methoden wie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) und [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="6b7c9-119">Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="6b7c9-120">In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="6b7c9-121">Automatische HTTP 400-Antwort</span><span class="sxs-lookup"><span data-stu-id="6b7c9-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="6b7c9-122">Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="6b7c9-123">Der nachfolgende Code wird in Ihren Aktionen überflüssig:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="6b7c9-124">Dieses Standardverhalten wird mit folgendem Code in *Startup.ConfigureServices* deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="6b7c9-125">Rückschluss auf Bindungsquellparameter</span><span class="sxs-lookup"><span data-stu-id="6b7c9-125">Binding source parameter inference</span></span>

<span data-ttu-id="6b7c9-126">Ein Bindungsquellenattribut definiert den Speicherort, an dem der Wert eines Aktionsparameters vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="6b7c9-127">Es gibt die folgenden Bindungsquellattribute:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="6b7c9-128">Attribut</span><span class="sxs-lookup"><span data-stu-id="6b7c9-128">Attribute</span></span>|<span data-ttu-id="6b7c9-129">Bindungsquelle</span><span class="sxs-lookup"><span data-stu-id="6b7c9-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="6b7c9-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="6b7c9-131">Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="6b7c9-131">Request body</span></span> |
|<span data-ttu-id="6b7c9-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="6b7c9-133">Formulardaten im Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="6b7c9-133">Form data in the request body</span></span> |
|<span data-ttu-id="6b7c9-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="6b7c9-135">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="6b7c9-135">Request header</span></span> |
|<span data-ttu-id="6b7c9-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="6b7c9-137">Abfragezeichenfolge-Parameter der Anforderung</span><span class="sxs-lookup"><span data-stu-id="6b7c9-137">Request query string parameter</span></span> |
|<span data-ttu-id="6b7c9-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="6b7c9-139">Routendaten aus aktuellen Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6b7c9-139">Route data from the current request</span></span> |
|<span data-ttu-id="6b7c9-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="6b7c9-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="6b7c9-141">Der als Aktionsparameter eingefügte Anforderungsdienst.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="6b7c9-142">Verwenden Sie **nicht** `[FromRoute]`, wenn Werte möglicherweise `%2f` (d.h. `/`) enthalten, da `%2f` nicht durch Entfernen von Escapezeichen zu `/` gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="6b7c9-143">Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="6b7c9-144">Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="6b7c9-145">Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="6b7c9-146">Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="6b7c9-147">Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="6b7c9-148">Die Bindungsquellenattribute verhalten sich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="6b7c9-149">Für komplexe Typparameter wird **[FromBody]** abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="6b7c9-150">Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) und [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="6b7c9-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="6b7c9-151">Der Rückschlusscode der Bindungsquelle ignoriert diese Typen.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="6b7c9-152">Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="6b7c9-153">Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="6b7c9-154">**[FromForm]** wird für Aktionsparameter des Typs [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="6b7c9-155">Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="6b7c9-156">**[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="6b7c9-157">Wenn mehrere Routen mit einem Aktionsparameter übereinstimmen, gilt jeder Routenwert als `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="6b7c9-158">**[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="6b7c9-159">Diese Standardrückschlussregeln werden mit folgendem Code in *Startup.ConfigureServices* deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="6b7c9-160">Ableiten der Multipart/form-data-Anforderung</span><span class="sxs-lookup"><span data-stu-id="6b7c9-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="6b7c9-161">Wenn ein Aktionsparameter mit dem [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="6b7c9-162">Dieses Standardverhalten wird mit folgendem Code in *Startup.ConfigureServices* deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="6b7c9-163">Anforderung für das Attributrouting</span><span class="sxs-lookup"><span data-stu-id="6b7c9-163">Attribute routing requirement</span></span>

<span data-ttu-id="6b7c9-164">Das Attributrouting wird zu einer Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="6b7c9-165">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6b7c9-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="6b7c9-166">Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) oder von [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure* definiert sind, auf keine Aktionen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="6b7c9-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6b7c9-167">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6b7c9-167">Additional resources</span></span>

* [<span data-ttu-id="6b7c9-168">Controlleraktions-Rückgabetypen</span><span class="sxs-lookup"><span data-stu-id="6b7c9-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="6b7c9-169">Benutzerdefinierte Formatierer</span><span class="sxs-lookup"><span data-stu-id="6b7c9-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="6b7c9-170">Formatieren von Antwortdaten</span><span class="sxs-lookup"><span data-stu-id="6b7c9-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="6b7c9-171">Hilfeseiten mit Swagger</span><span class="sxs-lookup"><span data-stu-id="6b7c9-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="6b7c9-172">Routing zu Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="6b7c9-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
