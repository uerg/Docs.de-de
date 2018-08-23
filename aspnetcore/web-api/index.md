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
# <a name="build-web-apis-with-aspnet-core"></a>Erstellen von Web-APIs mit ASP.NET Core

Von [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

In diesem Dokument wird erläutert, wie eine Web-API in ASP.NET Core erstellt werden kann und wann jedes Feature am besten verwendet werden kann.

## <a name="derive-class-from-controllerbase"></a>Ableiten einer Klasse von ControllerBase

Leiten Sie Inhalte der <xref:Microsoft.AspNetCore.Mvc.ControllerBase>-Klasse in einem Controller ab, der als Web-API fungieren soll. Zum Beispiel:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

Die `ControllerBase`-Klasse ermöglicht den Zugriff auf mehrere Eigenschaften und Methoden. Zu den Beispielen im vorherigen Code zählen <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück. Ein Zugriff erfolgt auf die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell zu verarbeiten.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Kommentieren einer Klasse mithilfe von ApiControllerAttribute

In ASP.NET Core 2.1 wird das Attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Eine Kompatibilitätsversion von 2.1 oder höher, die über <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> festgelegt wird, ist für die Verwendung dieses Attributs erforderlich. Beispielsweise legt der hervorgehobene Code in *Startup.ConfigureServices* das Kompatibilitätsflag 2.1 fest:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Weitere Informationen finden Sie unter <xref:mvc/compatibility-version>.

Das `[ApiController]`-Attribut ist in der Regel mit `ControllerBase` gekoppelt, um das REST-spezifische Verhalten für Controller zu ermöglichen. Mit `ControllerBase` kann auf Methoden wie <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> zugegriffen werden.

Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.

### <a name="automatic-http-400-responses"></a>Automatische HTTP 400-Antwort

Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus. Der nachfolgende Code wird in Ihren Aktionen überflüssig:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Das Standardverhalten wird deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Rückschluss auf Bindungsquellparameter

Ein Bindungsquellenattribut definiert den Speicherort, an dem der Wert eines Aktionsparameters vorhanden ist. Es gibt die folgenden Bindungsquellattribute:

|Attribut|Bindungsquelle |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Anforderungstext |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Formulardaten im Anforderungstext |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Anforderungsheader |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Abfragezeichenfolge-Parameter der Anforderung |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Routendaten aus aktuellen Anforderungen |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Der als Aktionsparameter eingefügte Anforderungsdienst. |

> [!WARNING]
> Verwenden Sie `[FromRoute]` nicht, wenn Werte möglicherweise `%2f` enthalten (d.h. `/`). `%2f` wird nicht durch Entfernen von Escapezeichen zu `/`. Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.

Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert. Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet. Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden. Die Bindungsquellenattribute verhalten sich wie folgt:

* Für komplexe Typparameter wird **[FromBody]** abgeleitet. Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. <xref:Microsoft.AspNetCore.Http.IFormCollection> und <xref:System.Threading.CancellationToken>. Der Rückschlusscode der Bindungsquelle ignoriert diese Typen. `[FromBody]` wird für einfache Typen wie `string` oder `int` nicht abgeleitet. Deshalb sollte das `[FromBody]`-Attribut für einfache Typen verwendet werden, wenn diese Funktionsweise gewünscht ist. Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst. Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** wird für Aktionsparameter des Typs <xref:Microsoft.AspNetCore.Http.IFormFile> und <xref:Microsoft.AspNetCore.Http.IFormFileCollection> abgeleitet. Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.
* **[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen. Wenn mehr als eine Route mit einem Aktionsparameter übereinstimmt, gilt jeder Routenwert als `[FromRoute]`.
* **[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.

Die standardmäßig festgelegten Rückschlussregeln werden deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Ableiten der Multipart/form-data-Anforderung

Wenn ein Aktionsparameter mit dem [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.

Das Standardverhalten wird deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Anforderung für das Attributrouting

Das Attributrouting wird zu einer Anforderung. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> oder von <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure* definiert sind, nicht auf Aktionen zugreifen.

::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
