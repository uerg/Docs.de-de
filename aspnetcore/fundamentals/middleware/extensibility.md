---
title: Factorybezogene Middlewareaktivierung in ASP.NET Core
author: guardrex
description: Informationen zur Verwendung von stark typisierter Middleware mit einer factorybezogenen Aktivierungsimplementierung von ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 57ff9db2edbf307f2442443dc14e69b0498f7475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Factorybezogene Middlewareaktivierung in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Bei [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) handelt es sich um einen Erweiterungspunkt für die [Middleware](xref:fundamentals/middleware/index)-Aktivierung.

Die Erweiterungsmethode `UseMiddleware` überprüft, ob der registrierte Typ einer Middleware `IMiddleware` implementiert. Falls dies der Fall ist, wird die im Container registrierte `IMiddlewareFactory`-Instanz im Container verwendet, um die `IMiddleware`-Implementierung aufzulösen, anstatt die konventionsbasierte Middlewareaktivierungslogik zu verwenden. Die Middleware wird als bereichsbezogener oder vorübergehender Dienst im Dienstcontainer der App registriert.

Vorteile:

* Aktivierung pro Anforderung (Injection von bereichsbezogenen Diensten)
* Starke Typisierung der Middleware

`IMiddleware` wird pro Anforderung aktiviert, sodass bereichsbezogene Dienste in den Konstruktor der Middleware eingefügt werden können.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Die Beispiel-App stellt Middleware dar, die wie folgt aktiviert wurde:

* Über Konventionen (`ConventionalMiddleware`). Weitere Informationen zur konventionellen Middlewareaktivierung finden Sie im Artikel [Middleware](xref:fundamentals/middleware/index).
* Über die Implementierung einer [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)-Instanz (`IMiddlewareMiddleware`). Die Standard-[MiddlewareFactory-Klasse](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktiviert die Middleware.

Die Middlewareimplementierungen funktionieren auf identische Weise und erfassen den Wert, der von einem Abfragezeichenfolgenparameter (`key`) bereitgestellt wird. Die Middlewares verwenden einen eingefügten Datenbankkontext (einen bereichsbezogenen Dienst), um den Abfragezeichenwert in einer In-Memory Database zu erfassen.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiert Middleware für die Anforderungspipeline der App. Die [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_)-Methode verarbeitet Anforderungen und gibt einen `Task` zurück, der für die Ausführung der Middleware steht.

Über Konventionen aktivierte Middleware:

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Über `MiddlewareFactory` aktivierte Middleware:

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Für Middlewares werden Erweiterungen erstellt:

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Es ist nicht möglich, Objekte mit `UseMiddleware` an eine Middleware zu übergeben, die über eine Factory aktiviert wurde:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Die Middleware, die über eine Factory aktiviert wurde, wird dem integrierten Container in der *Startup.cs*-Datei hinzugefügt:

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Beide Middlewares werden in der Anforderungsverarbeitungspipeline in `Configure` registriert:

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

Die [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)-Instanz stellt Methoden zur Verfügung, um Middleware zu erstellen. Die Implementierung der Middlewarefactory wird im Container als bereichsbezogener Dienst registriert.

Die Standardimplementierung von `IMiddlewareFactory`, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), befindet sich im Paket [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([Verweisquelle](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Middleware](xref:fundamentals/middleware/index)
