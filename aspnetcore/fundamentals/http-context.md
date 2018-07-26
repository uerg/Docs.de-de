---
title: Zugreifen auf HttpContext in ASP.NET Core
author: coderandhiker
description: Anleitung zum Zugreifen auf HttpContext in ASP.NET Core
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202706"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="1e895-103">Zugreifen auf HttpContext in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e895-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="1e895-104">ASP.NET Core-Apps greifen über die [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)-Schnittstelle und die zugehörige Standardimplementierung [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) auf `HttpContext` zu.</span><span class="sxs-lookup"><span data-stu-id="1e895-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="1e895-105">Verwenden von HttpContext über Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1e895-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="1e895-106">Die Klasse [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) in Razor Pages zeigt die Eigenschaft [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) an:</span><span class="sxs-lookup"><span data-stu-id="1e895-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="1e895-107">Verwenden von HttpContext über einen Controller</span><span class="sxs-lookup"><span data-stu-id="1e895-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="1e895-108">Controller zeigen die Eigenschaft [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) an:</span><span class="sxs-lookup"><span data-stu-id="1e895-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="1e895-109">Verwenden von HttpContext über Middleware</span><span class="sxs-lookup"><span data-stu-id="1e895-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="1e895-110">Bei der Verwendung benutzerdefinierter Middlewarekomponenten wird `HttpContext` an die Methode `Invoke` oder `InvokeAsync` übergeben. Wenn die Middleware konfiguriert ist, kann darauf zugegriffen werden:</span><span class="sxs-lookup"><span data-stu-id="1e895-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="1e895-111">Verwenden von HttpContext über benutzerdefinierte Komponenten</span><span class="sxs-lookup"><span data-stu-id="1e895-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="1e895-112">Für andere Framework- und benutzerdefinierte Komponenten, die Zugriff auf `HttpContext` erfordern, wird empfohlen, eine Abhängigkeit mithilfe des integrierten Containers [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="1e895-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1e895-113">Der Abhängigkeitsinjektionscontainer stellt `IHttpContextAccessor` allen Klassen zur Verfügung, die ihn in ihren Konstruktoren als Abhängigkeit deklarieren.</span><span class="sxs-lookup"><span data-stu-id="1e895-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="1e895-114">Im vorherigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1e895-114">In the preceding example:</span></span>

* <span data-ttu-id="1e895-115">`UserRepository` deklariert seine Abhängigkeit von `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="1e895-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="1e895-116">Die Abhängigkeit wird bereitgestellt, wenn die Abhängigkeitsinjektion die Abhängigkeitskette auflöst und eine `UserRepository`-Instanz erstellt.</span><span class="sxs-lookup"><span data-stu-id="1e895-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
