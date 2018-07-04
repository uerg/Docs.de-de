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

Die `ControllerBase`-Klasse ermöglicht den Zugriff auf verschiedene Eigenschaften und Methoden. Im vorherigen Beispiel sind diese Methoden z.B. [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) und [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Diese Methoden werden innerhalb von Aktionsmethoden aufgerufen und geben dann jeweils die Statuscodes HTTP 400 und HTTP 201 zurück. Ein Zugriff erfolgt auf die Eigenschaft [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), die ebenfalls von `ControllerBase` bereitgestellt wird, um die Anforderungsvalidierung für das Modell durchzuführen.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>Kommentieren einer Klasse mithilfe von ApiControllerAttribute

In ASP.NET Core 2.1 wird das Attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) eingeführt, um eine Klasse des Web-API-Controllers anzugeben. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Diese Attribut wird häufig mit `ControllerBase` gekoppelt, um auf nützliche Methoden und Eigenschaften zuzugreifen. Mit `ControllerBase` kann auf Methoden wie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) und [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) zugegriffen werden.

Eine weitere Vorgehensweise ist das Erstellen einer benutzerdefinierten Basiscontrollerklasse, für die das Attribut `[ApiController]` angegeben ist:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

In den folgenden Abschnitten werden praktische Features beschrieben, die vom Attribut hinzugefügt wurden.

### <a name="automatic-http-400-responses"></a>Automatische HTTP 400-Antwort

Validierungsfehler lösen automatisch eine HTTP 400-Antwort aus. Der nachfolgende Code wird in Ihren Aktionen überflüssig:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Dieses Standardverhalten wird mit folgendem Code in *Startup.ConfigureServices* deaktiviert:

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

> [!NOTE]
> Verwenden Sie **nicht** `[FromRoute]`, wenn Werte möglicherweise `%2f` (d.h. `/`) enthalten, da `%2f` nicht durch Entfernen von Escapezeichen zu `/` gemacht wird. Verwenden Sie `[FromQuery]`, wenn der Wert `%2f` enthalten könnte.

Ohne das `[ApiController]`-Attribut werden Bindungsquellattribute explizit definiert. Im folgenden Beispiel gibt das `[FromQuery]`-Attribut an, dass der Parameterwert `discontinuedOnly` in der Abfragezeichenfolge der Anforderungs-URL bereitgestellt wird:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Rückschlussregeln werden auf die Standarddatenquellen der Aktionsparameter angewendet. Diese Regeln konfigurieren die Bindungsquellen, die Sie sonst am ehesten manuell auf die Aktionsparameter anwenden. Die Bindungsquellenattribute verhalten sich wie folgt:

* Für komplexe Typparameter wird **[FromBody]** abgeleitet. Eine Ausnahme von der Regel ist jeder komplexe integrierte Typ mit spezieller Bedeutung, z.B. [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) und [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Der Rückschlusscode der Bindungsquelle ignoriert diese Typen. Wenn für eine Aktion mehr als ein Parameter explizit angegeben ist (über `[FromBody]`) oder als vom Anforderungstext gebunden hergeleitet wird, wird eine Ausnahme ausgelöst. Die folgenden Aktionssignaturen lösen beispielsweise eine Ausnahme aus:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** wird für Aktionsparameter des Typs [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) abgeleitet. Es wird für keine einfachen oder benutzerdefinierte Typen abgeleitet.
* **[FromRoute]** wird für jeden Namen von Aktionsparametern abgeleitet, die mit einem Parameter in der Routenvorlage übereinstimmen. Wenn mehrere Routen mit einem Aktionsparameter übereinstimmen, gilt jeder Routenwert als `[FromRoute]`.
* **[FromQuery]** wird für alle anderen Aktionsparameter abgeleitet.

Diese Standardrückschlussregeln werden mit folgendem Code in *Startup.ConfigureServices* deaktiviert:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Ableiten der Multipart/form-data-Anforderung

Wenn ein Aktionsparameter mit dem [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)-Attribut kommentiert wird, wird der Anforderungsinhaltstyp `multipart/form-data` abgeleitet.

Dieses Standardverhalten wird mit folgendem Code in *Startup.ConfigureServices* deaktiviert:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Anforderung für das Attributrouting

Das Attributrouting wird zu einer Anforderung. Zum Beispiel:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Sie können über [konventionelle Routen](xref:mvc/controllers/routing#conventional-routing), die in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) oder von [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure* definiert sind, auf keine Aktionen zugreifen.
::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Controlleraktions-Rückgabetypen](xref:web-api/action-return-types)
* [Benutzerdefinierte Formatierer](xref:web-api/advanced/custom-formatters)
* [Formatieren von Antwortdaten](xref:web-api/advanced/formatting)
* [Hilfeseiten mit Swagger](xref:tutorials/web-api-help-pages-using-swagger)
* [Routing zu Controlleraktionen](xref:mvc/controllers/routing)
