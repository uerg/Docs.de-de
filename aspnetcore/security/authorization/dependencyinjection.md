---
title: Abhängigkeitsinjektion in Anforderung Handler in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Authorization Anforderung Handler in einer ASP.NET Core-app mithilfe der Abhängigkeitsinjektion einfügen.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273721"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Abhängigkeitsinjektion in Anforderung Handler in ASP.NET Core

<a name="security-authorization-di"></a>

[Müssen Autorisierung Handler registriert werden](xref:security/authorization/policies#handler-registration) in die Auflistung während der Konfiguration (mit [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

Angenommen, mussten Sie ein Repository mit Regeln innerhalb eines ereignishandlers Autorisierung auswerten möchten, und diesem Repository registriert wurde, in die Auflistung. Die Autorisierung wird beheben und einfügen, die in Ihrem Konstruktor.

Angenommen, Sie ASP verwenden möchten. NET der Infrastruktur, die Sie einfügen möchten Protokollierung `ILoggerFactory` in den Handler. Ein Handler möglicherweise formuliert werden:

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

Registrieren Sie den Handler mit `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Eine Instanz der Ereignishandler wird beim Start der Anwendung erstellt werden und DI wird die registrierte einfügen `ILoggerFactory` in Ihrem Konstruktor.

> [!NOTE]
> Handler, die Verwendung von Entity Framework darf nicht als Singletons registriert werden.
