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
# <a name="access-httpcontext-in-aspnet-core"></a>Zugreifen auf HttpContext in ASP.NET Core

ASP.NET Core-Apps greifen über die [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)-Schnittstelle und die zugehörige Standardimplementierung [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) auf `HttpContext` zu.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Verwenden von HttpContext über Razor Pages

Die Klasse [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) in Razor Pages zeigt die Eigenschaft [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) an:

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

## <a name="use-httpcontext-from-a-controller"></a>Verwenden von HttpContext über einen Controller

Controller zeigen die Eigenschaft [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) an:

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

## <a name="use-httpcontext-from-middleware"></a>Verwenden von HttpContext über Middleware

Bei der Verwendung benutzerdefinierter Middlewarekomponenten wird `HttpContext` an die Methode `Invoke` oder `InvokeAsync` übergeben. Wenn die Middleware konfiguriert ist, kann darauf zugegriffen werden:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Verwenden von HttpContext über benutzerdefinierte Komponenten

Für andere Framework- und benutzerdefinierte Komponenten, die Zugriff auf `HttpContext` erfordern, wird empfohlen, eine Abhängigkeit mithilfe des integrierten Containers [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zu registrieren. Der Abhängigkeitsinjektionscontainer stellt `IHttpContextAccessor` allen Klassen zur Verfügung, die ihn in ihren Konstruktoren als Abhängigkeit deklarieren.

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

Im vorherigen Beispiel:

* `UserRepository` deklariert seine Abhängigkeit von `IHttpContextAccessor`.
* Die Abhängigkeit wird bereitgestellt, wenn die Abhängigkeitsinjektion die Abhängigkeitskette auflöst und eine `UserRepository`-Instanz erstellt.

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
