---
title: Factorybezogene Middlewareaktivierung in ASP.NET Core
author: guardrex
description: Informationen zur Verwendung von stark typisierter Middleware mit einer factorybezogenen Aktivierungsimplementierung von ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 44987dbc20b0419865a23e64b60a5dc3f436743a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277071"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="4daac-103">Factorybezogene Middlewareaktivierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4daac-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="4daac-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4daac-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4daac-105">Bei [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) handelt es sich um einen Erweiterungspunkt für die [Middleware](xref:fundamentals/middleware/index)-Aktivierung.</span><span class="sxs-lookup"><span data-stu-id="4daac-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="4daac-106">Die Erweiterungsmethode `UseMiddleware` überprüft, ob der registrierte Typ einer Middleware `IMiddleware` implementiert.</span><span class="sxs-lookup"><span data-stu-id="4daac-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="4daac-107">Falls dies der Fall ist, wird die im Container registrierte `IMiddlewareFactory`-Instanz im Container verwendet, um die `IMiddleware`-Implementierung aufzulösen, anstatt die konventionsbasierte Middlewareaktivierungslogik zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="4daac-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="4daac-108">Die Middleware wird als bereichsbezogener oder vorübergehender Dienst im Dienstcontainer der App registriert.</span><span class="sxs-lookup"><span data-stu-id="4daac-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="4daac-109">Vorteile:</span><span class="sxs-lookup"><span data-stu-id="4daac-109">Benefits:</span></span>

* <span data-ttu-id="4daac-110">Aktivierung pro Anforderung (Injection von bereichsbezogenen Diensten)</span><span class="sxs-lookup"><span data-stu-id="4daac-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="4daac-111">Starke Typisierung der Middleware</span><span class="sxs-lookup"><span data-stu-id="4daac-111">Strong typing of middleware</span></span>

<span data-ttu-id="4daac-112">`IMiddleware` wird pro Anforderung aktiviert, sodass bereichsbezogene Dienste in den Konstruktor der Middleware eingefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="4daac-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="4daac-113">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4daac-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4daac-114">Die Beispiel-App stellt Middleware dar, die wie folgt aktiviert wurde:</span><span class="sxs-lookup"><span data-stu-id="4daac-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="4daac-115">Per Konvention</span><span class="sxs-lookup"><span data-stu-id="4daac-115">Convention.</span></span> <span data-ttu-id="4daac-116">Weitere Informationen zur konventionellen Middlewareaktivierung finden Sie im Artikel [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="4daac-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="4daac-117">Durch die Implementierung von [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware)</span><span class="sxs-lookup"><span data-stu-id="4daac-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="4daac-118">Die Standard-[MiddlewareFactory-Klasse](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktiviert die Middleware.</span><span class="sxs-lookup"><span data-stu-id="4daac-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="4daac-119">Die Middlewareimplementierungen funktionieren auf identische Weise und erfassen den Wert, der von einem Abfragezeichenfolgenparameter (`key`) bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="4daac-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="4daac-120">Die Middlewares verwenden einen eingefügten Datenbankkontext (einen bereichsbezogenen Dienst), um den Abfragezeichenwert in einer In-Memory Database zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="4daac-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="4daac-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="4daac-121">IMiddleware</span></span>

<span data-ttu-id="4daac-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiert Middleware für die Anforderungspipeline der App.</span><span class="sxs-lookup"><span data-stu-id="4daac-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="4daac-123">Die [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_)-Methode verarbeitet Anforderungen und gibt einen `Task` zurück, der für die Ausführung der Middleware steht.</span><span class="sxs-lookup"><span data-stu-id="4daac-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="4daac-124">Über Konventionen aktivierte Middleware:</span><span class="sxs-lookup"><span data-stu-id="4daac-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="4daac-125">Über `MiddlewareFactory` aktivierte Middleware:</span><span class="sxs-lookup"><span data-stu-id="4daac-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="4daac-126">Für Middlewares werden Erweiterungen erstellt:</span><span class="sxs-lookup"><span data-stu-id="4daac-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="4daac-127">Es ist nicht möglich, Objekte mit `UseMiddleware` an eine Middleware zu übergeben, die über eine Factory aktiviert wurde:</span><span class="sxs-lookup"><span data-stu-id="4daac-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="4daac-128">Die Middleware, die über eine Factory aktiviert wurde, wird dem integrierten Container in der *Startup.cs*-Datei hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="4daac-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="4daac-129">Beide Middlewares werden in der Anforderungsverarbeitungspipeline in `Configure` registriert:</span><span class="sxs-lookup"><span data-stu-id="4daac-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="4daac-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="4daac-130">IMiddlewareFactory</span></span>

<span data-ttu-id="4daac-131">Die [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)-Instanz stellt Methoden zur Verfügung, um Middleware zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4daac-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="4daac-132">Die Implementierung der Middlewarefactory wird im Container als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="4daac-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="4daac-133">Die Standardimplementierung von `IMiddlewareFactory`, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), befindet sich im Paket [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([Verweisquelle](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="4daac-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4daac-134">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4daac-134">Additional resources</span></span>

* [<span data-ttu-id="4daac-135">Middleware</span><span class="sxs-lookup"><span data-stu-id="4daac-135">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="4daac-136">Middlewareaktivierung mit einem Drittanbietercontainer</span><span class="sxs-lookup"><span data-stu-id="4daac-136">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
