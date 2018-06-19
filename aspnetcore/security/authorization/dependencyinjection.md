---
title: Abhängigkeitsinjektion in Anforderung Handler in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Authorization Anforderung Handler in einer ASP.NET Core-app mithilfe der Abhängigkeitsinjektion einfügen.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 4de7f0e49ade459968f8c30fbad76ce96a65815f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072993"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="5e58c-103">Abhängigkeitsinjektion in Anforderung Handler in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e58c-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="5e58c-104">[Müssen Autorisierung Handler registriert werden](xref:security/authorization/policies#handler-registration) in die Auflistung während der Konfiguration (mit [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="5e58c-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="5e58c-105">Angenommen, mussten Sie ein Repository mit Regeln innerhalb eines ereignishandlers Autorisierung auswerten möchten, und diesem Repository registriert wurde, in die Auflistung.</span><span class="sxs-lookup"><span data-stu-id="5e58c-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="5e58c-106">Die Autorisierung wird beheben und einfügen, die in Ihrem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="5e58c-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="5e58c-107">Angenommen, Sie ASP verwenden möchten. NET der Infrastruktur, die Sie einfügen möchten Protokollierung `ILoggerFactory` in den Handler.</span><span class="sxs-lookup"><span data-stu-id="5e58c-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="5e58c-108">Ein Handler möglicherweise formuliert werden:</span><span class="sxs-lookup"><span data-stu-id="5e58c-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="5e58c-109">Registrieren Sie den Handler mit `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="5e58c-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="5e58c-110">Eine Instanz der Ereignishandler wird beim Start der Anwendung erstellt werden und DI wird die registrierte einfügen `ILoggerFactory` in Ihrem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="5e58c-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="5e58c-111">Handler, die Verwendung von Entity Framework darf nicht als Singletons registriert werden.</span><span class="sxs-lookup"><span data-stu-id="5e58c-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
