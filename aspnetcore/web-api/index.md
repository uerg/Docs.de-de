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
# <a name="build-web-apis-with-aspnet-core"></a>Erstellen von Web-APIs mit ASP.NET Core

Von [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

In diesem Dokument wird erläutert, wie eine Web-API in ASP.NET Core erstellt werden kann und wann jedes Feature am besten verwendet werden kann.

## <a name="derive-class-from-controllerbase"></a>Ableiten einer Klasse von ControllerBase

Leiten Sie Inhalte der [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase)-Klasse in einem Controller ab, der als Web-API fungieren soll. Zum Beispiel:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

Die `ControllerBase`-Klasse ermöglicht den Zugriff auf mehrere Eigenschaften und Methoden. Im vorherigen Code sind diese z.B. [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) und [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück. Ein Zugriff erfolgt auf die Eigenschaft [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell zu verarbeiten.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Kommentieren einer Klasse mithilfe von ApiControllerAttribute

In ASP.NET Core 2.1 wird das Attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Eine Kompatibilitätsversion von 2.1 oder höher, die über [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion) festgelegt wird, ist für die Verwendung dieses Attributs erforderlich. Beispielsweise legt der hervorgehobene Code in *Startup.ConfigureServices* das Kompatibilitätsflag 2.1 fest:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Das `[ApiController]`-Attribut ist in der Regel mit `ControllerBase` gekoppelt, um das REST-spezifische Verhalten für Controller zu ermöglichen. Mit `ControllerBase` kann auf Methoden wie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) und [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) zugegriffen werden.

Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.

### <a name="automatic-http-400-responses"></a>Automatische HTTP 400-Antwort

Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus. Der nachfolgende Code wird in Ihren Aktionen überflüssig:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Das Standardverhalten wird deaktiviert, sobald die Eigenschaft [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Rückschluss auf Bindungsquellparameter

Ein Bindungsquellenattribut definiert den Speicherort, an dem der Wert eines Aktionsparameters vorhanden ist. Es gibt die folgenden Bindungsquellattribute:

|Attribut|Bindungsquelle |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Anforderungstext |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Formulardaten im Anforderungstext |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Anforderungsheader |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Abfragezeichenfolge-Parameter der Anforderung |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Routendaten aus aktuellen Anforderungen |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Der als Aktionsparameter eingefügte Anforderungsdienst. |

> [!WARNING]
> Verwenden Sie `[FromRoute]` nicht, wenn Werte möglicherweise `%2f` enthalten (d.h. `/`). `%2f` wird nicht durch Entfernen von Escapezeichen zu `/`. Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.

Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert. Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet. Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden. Die Bindungsquellenattribute verhalten sich wie folgt:

* Für komplexe Typparameter wird **[FromBody]** abgeleitet. Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) und [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Der Rückschlusscode der Bindungsquelle ignoriert diese Typen. Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst. Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** wird für Aktionsparameter des Typs [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) abgeleitet. Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.
* **[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen. Wenn mehr als eine Route mit einem Aktionsparameter übereinstimmt, gilt jeder Routenwert als `[FromRoute]`.
* **[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.

Die Standardrückschlussregeln werden deaktiviert, wenn die Eigenschaft [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Ableiten der Multipart/form-data-Anforderung

Wenn ein Aktionsparameter mit dem [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.

Das Standardverhalten wird deaktiviert, wenn die Eigenschaft [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Anforderung für das Attributrouting

Das Attributrouting wird zu einer Anforderung. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) oder von [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure* definiert sind, auf keine Aktionen zugreifen.

::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
