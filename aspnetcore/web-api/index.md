---
title: Erstellen von Web-APIs mit ASP.NET Core
author: scottaddie
description: Lernen Sie die verfügbaren Features zum Erstellen einer Web-API in ASP.NET Core kennen, und erhalten Sie Informationen zur Verwendung dieser Features.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 950f4e8afa13bf297ea8658ef1c1bea0c9b62936
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234591"
---
# <a name="build-web-apis-with-aspnet-core"></a>Erstellen von Web-APIs mit ASP.NET Core

Von [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

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

Eine Kompatibilitätsversion von 2.1 oder höher, die über <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> festgelegt wird, ist für die Verwendung dieses Attributs erforderlich. Beispielsweise legt der hervorgehobene Code in *Startup.ConfigureServices* das Kompatibilitätsflag 2.2 fest:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Weitere Informationen finden Sie unter <xref:mvc/compatibility-version>.

Das `[ApiController]`-Attribut ist in der Regel mit `ControllerBase` gekoppelt, um das REST-spezifische Verhalten für Controller zu ermöglichen. Mit `ControllerBase` kann auf Methoden wie <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> zugegriffen werden.

Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.

### <a name="problem-details-responses-for-error-status-codes"></a>Problemdetailantworten für Fehlerstatuscodes

ASP.NET Core 2.1 und höher enthält [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), einen auf der [RFC 7807-Spezifikation](https://tools.ietf.org/html/rfc7807) basierten Typen. Der Typ `ProblemDetails` bietet ein standardisiertes Format zum Übermitteln maschinenlesbarer Details zu Fehlern in einer HTTP-Antwort.

In ASP.NET Core 2.2 und höher wandelt MVC Fehlerstatuscodeergebnisse (Statuscode 400 und höher) in Ergebnisse mit `ProblemDetails` um. Betrachten Sie folgenden Code:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

Die HTTP-Antwort für das `NotFound`-Ergebnis hat den Statuscode 404 mit einem `ProblemDetails`-Körper wie dem folgenden:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

Das Feature für Problemdetails erfordert das Kompatibilitätsflag 2.2 oder höher. Das Standardverhalten wird deaktiviert, sobald die Eigenschaft [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> auf `true` festgelegt wird. Der folgende hervorgehobene Code von `Startup.ConfigureServices` deaktiviert Problemdetails:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

Verwenden Sie die [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> -->-Eigenschaft zum Konfigurieren des Inhalts der `ProblemDetails`-Antwort. Der folgenden Code aktualisiert beispielsweise die `type`-Eigenschaft für 404-Antworten:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a>Automatische HTTP 400-Antwort

Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus. Der nachfolgende Code wird in Ihren Aktionen überflüssig:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Verwenden Sie <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> zum Anpassen der Ausgabe der Antwort.

Das Standardverhalten wird deaktiviert, sobald die Eigenschaft <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> auf `true` festgelegt wird. Fügen Sie nach `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` folgenden Code in *Startup.ConfigureServices* hinzu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

Mit dem Kompatibilitätsflag 2.2 oder höher wird für 400-Antworten standardmäßig der Typ <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> zurückgegeben. Verwenden Sie die Eigenschaft [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> -->, um das ASP.NET Core 2.1-Fehlerformat zu verwenden.

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
