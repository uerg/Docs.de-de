---
title: Benutzerdefinierte Richtlinie basierende Autorisierung
author: rick-anderson
description: "Dieses Dokument erläutert das Erstellen und Verwenden von benutzerdefinierten Autorisierungs-Policy-Handler in einer ASP.NET Core-app."
keywords: ASP.NET Core, Autorisierung, die benutzerdefinierte Richtlinie, die Autorisierungsrichtlinie
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>Benutzerdefinierte Richtlinie basierende Autorisierung

<a name="security-authorization-policies-based"></a>

Im Hintergrund der [Rolle Autorisierung](roles.md) und [Ansprüche Autorisierung](claims.md) Verwenden einer Anforderung verwendet wird, einen Handler für die Anforderung und eine vorkonfigurierte Richtlinie. Diese Bausteine ermöglichen es Ihnen, Autorisierung auswertungen in Code, sodass eine umfangreichere, wiederverwendet werden kann, und einer leicht testfähig Autorisierung Struktur auszudrücken.

Eine Autorisierungsrichtlinie setzt sich aus einer oder mehreren Anforderungen und als Teil der Dienstkonfiguration Autorisierung in beim Start der Anwendung registriert ist `ConfigureServices` in der *Startup.cs* Datei.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Hier sehen Sie sich, dass eine Richtlinie "Over21" mit einer einzelnen Anforderung erstellt wird, dass von einem Mindestalter, die als Parameter mit der Anforderung übergeben wird.

Richtlinien werden angewendet, mit der `Authorize` Attribut, indem Sie den Richtliniennamen, z. B. angeben

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Anforderungen

Eine autorisierungsanforderung ist eine Auflistung von Datenparametern, die eine Richtlinie zum Auswerten des aktuellen Benutzerprinzipals verwenden können. In unserer Mindestalter-Richtlinie ist die Anforderung haben wir einen einzelnen Parameter das Mindestalter. Implementieren einer Anforderung muss `IAuthorizationRequirement`. Dies ist eine leere, Markierungsschnittstelle. Eine parametrisierte Mindestalter-Anforderung kann wie folgt implementiert werden.

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Eine Anforderung benötigen nicht Daten oder Eigenschaften.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Autorisierung Handler

Ein Authorization-Handler ist für die Auswertung von Eigenschaften einer Anforderung verantwortlich. Der Handler für die Autorisierung muss anhand einer bereitgestellten bewerten `AuthorizationHandlerContext` entscheiden, ob Autorisierung zugelassen wird. Kann die Anforderung besteht [mehrere Handler](policies.md#security-authorization-policies-based-multiple-handlers). Handler erben müssen `AuthorizationHandler<T>` , wobei T die Anforderung wird verarbeitet.

<a name="security-authorization-handler-example"></a>

Der Handler Mindestalter kann wie folgt aussehen:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Im obigen Code untersuchen wir zuerst um festzustellen, ob der aktuelle Benutzer principal hat ein Geburtsdatum Anspruch, der durch einen Aussteller ist bekannt und Vertrauensstellung ausgegeben wurde. Wenn der Anspruch nicht vorhanden ist kann nicht wir autorisiert, sodass wir zurückgegeben werden. Wenn wir einen Anspruch haben, wir herausfinden, wie ALT der Benutzer ist und, wenn sie das Mindestalter übergebener die Anforderung erfüllen dann Autorisierung wurde erfolgreich ausgeführt. Nach dem Autorisierung erfolgreich ist, die wir rufen `context.Succeed()` in der Anforderung, die wurde erfolgreich als Parameter übergeben.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Handler-Registrierung
Handler werden z. B. in der Auflistung der Dienste während der Konfiguration erfasst.

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Jeder Handler hinzugefügt, mit der Services-Auflistung mit `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` in Ihrer Handlerklasse übergeben.

## <a name="what-should-a-handler-return"></a>Was sollte ein Handler zurückgeben?

Sehen Sie unserer [Handler Beispiel](policies.md#security-authorization-handler-example) , die die `Handle()` Methode verfügt über keinen Wert zurückgibt, wie wir geben Erfolg oder Fehler?

* Ein Handler gibt die erfolgreiche Ausführung durch den Aufruf `context.Succeed(IAuthorizationRequirement requirement)`, übergeben der Anforderung, die erfolgreich überprüft wurde.

* Ein Ereignishandler muss nicht im allgemeinen Fehler zu behandeln, wie andere Handler für die gleiche Anforderung erfolgreich ausgeführt werden können.

* Um Fehler zu gewährleisten, selbst wenn andere Handler für eine Anforderung erfolgreich ist, rufen Sie `context.Fail`.

Unabhängig davon, was Sie in den Handler aufrufen werden alle Handler für eine Anforderung aufgerufen werden, wenn eine Richtlinie der Anforderung erforderlich ist. Anforderungen an, wie z. B. Protokollierung, haben Nebeneffekte, die immer stattfinden wird dadurch auch wenn `context.Fail()` in einen anderen Handler aufgerufen wurde.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Warum würde ich mehrere Handler für eine Anforderung?

In Fällen soll Auswertung auf eine **oder** Grundlage Sie mehrere Handler für eine einzelne Anforderung implementieren. Microsoft hat es sich beispielsweise um Türen, die nur mit Schlüssel Karten zu öffnen. Wenn Sie Ihre Schlüsselkarte zu Hause lassen, die Apparate druckt einen temporären Aufkleber und öffnet die Tür für Sie. In diesem Fall müssten Sie eine einzelne Anforderung *EnterBuilding*, aber mehrere Handler, die jeweils eine einzelne Anforderung überprüfen.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Nun sind die beiden Handler vorausgesetzt [registriert](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) Wenn eine Richtlinie auswertet der `EnterBuildingRequirement` richtlinienauswertung ist erfolgreich, wenn die beiden Ereignishandler erfolgreich ausgeführt wird.

## <a name="using-a-func-to-fulfill-a-policy"></a>Func verwenden, um eine Richtlinie zu erfüllen.

Es kann durchaus vorkommen, in denen Erfüllung einer Richtlinie einfach zu express im Code ist. Es ist möglich, geben Sie einfach eine `Func<AuthorizationHandlerContext, bool>` beim Konfigurieren der Richtlinie mit den `RequireAssertion` Richtlinie-Generator.

Z. B. den vorherigen `BadgeEntryHandler` könnte folgendermaßen umgeschrieben werden:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Zugreifen auf die MVC-Anforderungskontext in Handlern abfangen

Die `Handle` Methode müssen Sie in einem Ereignishandler für die Autorisierung implementieren verfügt über zwei Parameter ein `AuthorizationContext` und `Requirement` Sie verarbeiten. Frameworks wie z. B. Mvc- oder Jabbr sind frei, um ein Objekt zum Hinzufügen der `Resource` Eigenschaft auf die `AuthorizationContext` für die Weiterleitung über zusätzliche Informationen.

Beispielsweise MVC übergibt eine Instanz des `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in die Ressourceneigenschaft "dient zum Zugriff HttpContext", "RouteData" und "Alles" else MVC bereitstellt.

Die Verwendung der `Resource` Eigenschaft ist für bestimmte Framework. Mithilfe der Informationen in der `Resource` Eigenschaft schränkt die Autorisierungsrichtlinien auf bestimmten Frameworks. Sollten Sie eine Umwandlung der `Resource` Eigenschaft mit der `as` -Schlüsselwort, und überprüfen Sie die Umwandlung wurde erfolgreich ausgeführt werden, um sicherzustellen, dass Ihr Code keine stürzt ab mit `InvalidCastExceptions` bei Ausführung auf anderen Frameworks;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
