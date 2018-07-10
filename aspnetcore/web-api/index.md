---
title: Erstellen von Web-APIs mit ASP.NET Core
author: scottaddie
description: Lernen Sie die verfügbaren Features zum Erstellen einer Web-API in ASP.NET Core kennen, und erhalten Sie Informationen zur Verwendung dieser Features.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 84e4a51a8a8ab031752ef054cba834bd202a4927
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894214"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="a9a04-103">Erstellen von Web-APIs mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9a04-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="a9a04-104">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a9a04-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a9a04-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9a04-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9a04-106">In diesem Dokument wird erläutert, wie eine Web-API in ASP.NET Core erstellt werden kann und wann jedes Feature am besten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a9a04-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="a9a04-107">Ableiten einer Klasse von ControllerBase</span><span class="sxs-lookup"><span data-stu-id="a9a04-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="a9a04-108">Leiten Sie Inhalte der [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase)-Klasse in einem Controller ab, der als Web-API fungieren soll.</span><span class="sxs-lookup"><span data-stu-id="a9a04-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="a9a04-109">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a9a04-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="a9a04-110">Die `ControllerBase`-Klasse ermöglicht den Zugriff auf mehrere Eigenschaften und Methoden.</span><span class="sxs-lookup"><span data-stu-id="a9a04-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="a9a04-111">Im vorherigen Code sind diese z.B. [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) und [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="a9a04-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="a9a04-112">Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück.</span><span class="sxs-lookup"><span data-stu-id="a9a04-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="a9a04-113">Ein Zugriff erfolgt auf die Eigenschaft [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="a9a04-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="a9a04-114">Kommentieren einer Klasse mithilfe von ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="a9a04-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="a9a04-115">In ASP.NET Core 2.1 wird das Attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben.</span><span class="sxs-lookup"><span data-stu-id="a9a04-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="a9a04-116">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a9a04-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="a9a04-117">Eine Kompatibilitätsversion von 2.1 oder höher, die über [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion) festgelegt wird, ist für die Verwendung dieses Attributs erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a9a04-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="a9a04-118">Beispielsweise legt der hervorgehobene Code in *Startup.ConfigureServices* das Kompatibilitätsflag 2.1 fest:</span><span class="sxs-lookup"><span data-stu-id="a9a04-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="a9a04-119">Das `[ApiController]`-Attribut ist in der Regel mit `ControllerBase` gekoppelt, um das REST-spezifische Verhalten für Controller zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a9a04-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="a9a04-120">Mit `ControllerBase` kann auf Methoden wie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) und [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="a9a04-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="a9a04-121">Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:</span><span class="sxs-lookup"><span data-stu-id="a9a04-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="a9a04-122">In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="a9a04-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="a9a04-123">Automatische HTTP 400-Antwort</span><span class="sxs-lookup"><span data-stu-id="a9a04-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="a9a04-124">Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="a9a04-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="a9a04-125">Der nachfolgende Code wird in Ihren Aktionen überflüssig:</span><span class="sxs-lookup"><span data-stu-id="a9a04-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="a9a04-126">Das Standardverhalten wird deaktiviert, sobald die Eigenschaft [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a9a04-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="a9a04-127">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="a9a04-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="a9a04-128">Rückschluss auf Bindungsquellparameter</span><span class="sxs-lookup"><span data-stu-id="a9a04-128">Binding source parameter inference</span></span>

<span data-ttu-id="a9a04-129">Ein Bindungsquellenattribut definiert den Speicherort, an dem der Wert eines Aktionsparameters vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a9a04-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="a9a04-130">Es gibt die folgenden Bindungsquellattribute:</span><span class="sxs-lookup"><span data-stu-id="a9a04-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="a9a04-131">Attribut</span><span class="sxs-lookup"><span data-stu-id="a9a04-131">Attribute</span></span>|<span data-ttu-id="a9a04-132">Bindungsquelle</span><span class="sxs-lookup"><span data-stu-id="a9a04-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="a9a04-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="a9a04-134">Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="a9a04-134">Request body</span></span> |
|<span data-ttu-id="a9a04-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="a9a04-136">Formulardaten im Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="a9a04-136">Form data in the request body</span></span> |
|<span data-ttu-id="a9a04-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="a9a04-138">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="a9a04-138">Request header</span></span> |
|<span data-ttu-id="a9a04-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="a9a04-140">Abfragezeichenfolge-Parameter der Anforderung</span><span class="sxs-lookup"><span data-stu-id="a9a04-140">Request query string parameter</span></span> |
|<span data-ttu-id="a9a04-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="a9a04-142">Routendaten aus aktuellen Anforderungen</span><span class="sxs-lookup"><span data-stu-id="a9a04-142">Route data from the current request</span></span> |
|<span data-ttu-id="a9a04-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="a9a04-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="a9a04-144">Der als Aktionsparameter eingefügte Anforderungsdienst.</span><span class="sxs-lookup"><span data-stu-id="a9a04-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="a9a04-145">Verwenden Sie `[FromRoute]` nicht, wenn Werte möglicherweise `%2f` enthalten (d.h. `/`).</span><span class="sxs-lookup"><span data-stu-id="a9a04-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="a9a04-146">`%2f` wird nicht durch Entfernen von Escapezeichen zu `/`.</span><span class="sxs-lookup"><span data-stu-id="a9a04-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="a9a04-147">Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.</span><span class="sxs-lookup"><span data-stu-id="a9a04-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="a9a04-148">Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert.</span><span class="sxs-lookup"><span data-stu-id="a9a04-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="a9a04-149">Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:</span><span class="sxs-lookup"><span data-stu-id="a9a04-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="a9a04-150">Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="a9a04-151">Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden.</span><span class="sxs-lookup"><span data-stu-id="a9a04-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="a9a04-152">Die Bindungsquellenattribute verhalten sich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="a9a04-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="a9a04-153">Für komplexe Typparameter wird **[FromBody]** abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="a9a04-154">Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) und [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="a9a04-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="a9a04-155">Der Rückschlusscode der Bindungsquelle ignoriert diese Typen.</span><span class="sxs-lookup"><span data-stu-id="a9a04-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="a9a04-156">Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a9a04-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="a9a04-157">Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:</span><span class="sxs-lookup"><span data-stu-id="a9a04-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="a9a04-158">**[FromForm]** wird für Aktionsparameter des Typs [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="a9a04-159">Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="a9a04-160">**[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a9a04-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="a9a04-161">Wenn mehr als eine Route mit einem Aktionsparameter übereinstimmt, gilt jeder Routenwert als `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="a9a04-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="a9a04-162">**[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="a9a04-163">Die Standardrückschlussregeln werden deaktiviert, wenn die Eigenschaft [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a9a04-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="a9a04-164">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="a9a04-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="a9a04-165">Ableiten der Multipart/form-data-Anforderung</span><span class="sxs-lookup"><span data-stu-id="a9a04-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="a9a04-166">Wenn ein Aktionsparameter mit dem [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a9a04-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="a9a04-167">Das Standardverhalten wird deaktiviert, wenn die Eigenschaft [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) auf `true` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a9a04-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="a9a04-168">Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:</span><span class="sxs-lookup"><span data-stu-id="a9a04-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="a9a04-169">Anforderung für das Attributrouting</span><span class="sxs-lookup"><span data-stu-id="a9a04-169">Attribute routing requirement</span></span>

<span data-ttu-id="a9a04-170">Das Attributrouting wird zu einer Anforderung.</span><span class="sxs-lookup"><span data-stu-id="a9a04-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="a9a04-171">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a9a04-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="a9a04-172">Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) oder von [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure* definiert sind, auf keine Aktionen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="a9a04-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a9a04-173">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a9a04-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
