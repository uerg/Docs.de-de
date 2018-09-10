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
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Rückgabetypen für Controlleraktionen in der ASP.NET Core-Web-API

Von [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

In ASP.NET Core haben Sie die folgenden Optionen für Rückgabetypen für Web-API-Controlleraktionen:

::: moniker range=">= aspnetcore-2.1"

* [Spezifischer Typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Spezifischer Typ](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

In diesem Artikel wird die Verwendung der einzelnen Rückgabetypen erklärt.

## <a name="specific-type"></a>Spezifischer Typ

Die einfachste Aktion gibt einen primitiven oder komplexen Datentyp zurück (z.B. `string` oder einen benutzerdefinierten Objekttyp). Die folgende Aktion gibt eine Auflistung benutzerdefinierter `Product`-Objekte zurück:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Ohne bekannte Bedingungen, gegen die während der Ausführung einer Aktion Schutzmaßnahmen ergriffen werden müssen, reicht die Rückgabe eines spezifischen Typs möglicherweise aus. Die vorherige Aktion akzeptiert keine Parameter, weshalb eine Validierung von Parametereinschränkungen nicht erforderlich ist.

Wenn bekannte Bedingungen bei einer Aktion berücksichtig werden müssen, werden mehrere Rückgabepfade eingeführt. In diesem Fall ist es üblich, einen [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult)-Rückgabetyp mit dem primitiven oder komplexen Rückgabetyp zu kombinieren. Für diesen Aktionstyp sind entweder der Rückgabetyp [IActionResult](#iactionresult-type) oder [ActionResult\<T>](#actionresultt-type) erforderlich.

## <a name="iactionresult-type"></a>IActionResult-Typ

Der Rückgabetyp [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) eignet sich in Fällen, in denen mehrere [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult)-Rückgabetypen in einer Aktion möglich sind. Die `ActionResult`-Typen stellen verschiedene HTTP-Statuscodes dar. Einige gängige Rückgabetypen, die in diese Kategorie fallen, sind etwa [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) und [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Da es bei einer Aktion mehrere Rückgabetypen und Pfade gibt, sollte das Attribut [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) großzügig verwendet werden. Dieses Attribut erzeugt aussagekräftigere Antwortdetails für API-Hilfeseiten, die mit Tools wie [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) erstellt wurden. `[ProducesResponseType]` gibt die bekannten Typen und HTTP-Statuscodes an, die von der Aktion zurückgegeben werden sollen.

### <a name="synchronous-action"></a>Synchrone Aktion

Bei der folgenden synchronen Aktion gibt es zwei mögliche Rückgabetypen:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

In der vorherigen Aktion wird der Statuscode 404 zurückgegeben, wenn das durch `id` dargestellte Produkt im zugrunde liegenden Datenspeicher nicht vorhanden ist. Die Hilfsmethode [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) wird als Verknüpfung zu `return new NotFoundResult();` aufgerufen. Ist das Produkt vorhanden, wird ein `Product`-Objekt, das die Nutzlast darstellt, mit dem Statuscode 200 zurückgegeben. Die Hilfsmethode [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) wird als Kurzform für `return new OkObjectResult(product);` aufgerufen.

### <a name="asynchronous-action"></a>Asynchrone Aktion

Bei der folgenden asynchronen Aktion gibt es zwei mögliche Rückgabetypen:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Bei der vorherigen Aktion wird der Statuscode 400 zurückgegeben, wenn die Modellvalidierung fehlschlägt und die Hilfsmethode [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) aufgerufen wird. Das folgende Modell gibt beispielsweise an, dass Anforderungen die `Name`-Eigenschaft und einen Wert bereitstellen müssen. Aus diesem Grund führt die fehlgeschlagene Bereitstellung eines geeigneten `Name` in der Anforderung zu einer fehlgeschlagenen Modellvalidierung.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Der zweite bekannte Rückgabecode der vorherigen Aktion lautet 201 und wird von der Hilfsmethode [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) erzeugt. In diesem Pfad wird das `Product`-Objekt zurückgegeben.

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a>ActionResult\<T>-Typ

In ASP.NET Core 2.1 wird der Rückgabetyp [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) für Web-API-Controlleraktionen eingeführt. Damit wird die Rückgabe eines von [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) abgeleiteten Typs oder eines [spezifischen Typs](#specific-type) ermöglicht. `ActionResult<T>` besitzt gegenüber dem [IActionResult-Typ](#iactionresult-type) die folgenden Vorteile:

* Die `Type`-Eigenschaft des [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute)-Attributs kann ausgeschlossen werden. `[ProducesResponseType(200, Type = typeof(Product))]` wird beispielsweise zu `[ProducesResponseType(200)]` vereinfacht. Der erwartete Rückgabetyp der Aktion wird stattdessen von `T` in `ActionResult<T>` abgeleitet.
* [Implizite Umwandlungsoperatoren](/dotnet/csharp/language-reference/keywords/implicit) unterstützen die Konvertierung von `T` und `ActionResult` in `ActionResult<T>`. `T` wird in [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) konvertiert, wodurch `return new ObjectResult(T);` in `return T;` vereinfacht wird.

C# unterstützt keine impliziten Umwandlungsoperatoren in Schnittstellen. Daher ist die Konvertierung der Schnittstelle in einen konkreten Typ erforderlich, um `ActionResult<T>` verwenden zu können. Die Verwendung von `IEnumerable` im folgenden Beispiel funktioniert also nicht:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

Eine Möglichkeit zur Korrektur des oben stehenden Codes besteht darin, `_repository.GetProducts().ToList();` zurückzugeben.

Die meisten Aktionen haben einen bestimmten Rückgabetyp. Während der Ausführung der Aktion können unerwartete Bedingungen auftreten, wodurch der spezifische Typ nicht zurückgegeben wird. So kann beispielsweise die Modellvalidierung des Eingabeparameters einer Aktion fehlschlagen. In diesem Fall wird üblicherweise der entsprechende `ActionResult`-Typ anstatt des spezifischen Typs zurückgegeben.

### <a name="synchronous-action"></a>Synchrone Aktion

Bei der folgenden synchronen Aktion gibt es zwei mögliche Rückgabetypen:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Im vorherigen Code wird ein Statuscode 404 zurückgegeben, wenn das Produkt in der Datenbank nicht vorhanden ist. Ist das Produkt vorhanden, wird das entsprechende `Product`-Objekt zurückgegeben. Vor ASP.NET Core 2.1 hätte die Zeile `return product;` stattdessen `return Ok(product);` gelautet.

> [!TIP]
> Seit ASP.NET Core 2.1 wird der Rückschluss auf die Bindungsquelle des Aktionsparameters aktiviert, wenn eine Controllerklasse mit dem `[ApiController]`-Attribut ausgestattet ist. Ein Parametername, der dem Namen einer Routenvorlage entspricht, wird automatisch mithilfe der Routendaten der Anforderung gebunden. Folglich wird der `id`-Parameter der vorherigen Aktion nicht explizit mit dem Attribut [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) versehen.

### <a name="asynchronous-action"></a>Asynchrone Aktion

Bei der folgenden asynchronen Aktion gibt es zwei mögliche Rückgabetypen:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Schlägt die Modellvalidierung fehlt, wird die [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_)-Methode zur Rückgabe eines Statuscodes 400 aufgerufen. Die [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate)-Eigenschaft wird mit den entsprechenden Validierungsfehlern übergeben. Bei erfolgreicher Modellvalidierung wird das Produkt in der Datenbank erstellt. Der Statuscode 201 wird zurückgegeben.

> [!TIP]
> Seit ASP.NET Core 2.1 wird der Rückschluss auf die Bindungsquelle des Aktionsparameters aktiviert, wenn eine Controllerklasse mit dem `[ApiController]`-Attribut ausgestattet ist. Komplexe Typparameter werden automatisch mithilfe des Anforderungstexts gebunden. Folglich wird der `product`-Parameter der vorherigen Aktion nicht explizit mit dem Attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) versehen.

::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
