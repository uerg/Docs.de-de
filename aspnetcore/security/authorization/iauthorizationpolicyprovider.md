---
title: Benutzerdefinierte Autorisierung Richtlinie Anbieter in ASP.NET Core
author: mjrousos
description: Erfahren Sie, wie eine benutzerdefinierte IAuthorizationPolicyProvider in einer ASP.NET Core-app verwenden, um Autorisierungsrichtlinien dynamisch zu generieren.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Benutzerdefinierte Autorisierung-Policy-Anbietern, die mithilfe von IAuthorizationPolicyProvider in ASP.NET Core 

Durch [Mike Rousos](https://github.com/mjrousos)

In der Regel bei Verwendung [Richtlinie basierende Autorisierung](xref:security/authorization/policies), Richtlinien werden durch den Aufruf registriert `AuthorizationOptions.AddPolicy` als Teil der Dienstkonfiguration für die Autorisierung. In einigen Szenarien ist es möglicherweise nicht möglich (oder wünschenswert) aller Autorisierungsrichtlinien auf diese Weise zu registrieren. In diesen Fällen können Sie eine benutzerdefinierte `IAuthorizationPolicyProvider` steuern, wie die Autorisierungsrichtlinien bereitgestellt werden.

Beispiele für Szenarien, bei einer benutzerdefinierten [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) kann hilfreich sein, gehören:

* Verwenden einen externen Dienst, um die richtlinienauswertung sicherzustellen.
* Verwenden einen großen Bereich von Richtlinien (für verschiedene Raumnummer oder ALTER, z. B.), daher es nicht sinnvoll hinzuzufügende jede einzelne Autorisierungsrichtlinie mit einer `AuthorizationOptions.AddPolicy` aufrufen.
* Erstellen von Richtlinien zur Laufzeit basierend auf Informationen in einer externen Datenquelle (z. B. eine Datenbank) oder autorisierungsanforderungen dynamisch mithilfe eines anderen Mechanismus bestimmen.

## <a name="customizing-policy-retrieval"></a>Anpassen des Abrufens von Richtlinien

ASP.NET Core-apps mit einer Implementierung der der `IAuthorizationPolicyProvider` Schnittstelle, um Autorisierungsrichtlinien abzurufen. Standardmäßig [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) registriert und verwendet wird. `DefaultAuthorizationPolicyProvider` Gibt die Richtlinien aus der `AuthorizationOptions` in bereitgestellten ein `IServiceCollection.AddAuthorization` aufrufen.

Sie können dieses Verhalten anpassen, indem Sie ein anderes registrieren `IAuthorizationPolicyProvider` Implementierung in der app [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) Container. 

Die `IAuthorizationPolicyProvider` Schnittstelle enthält zwei APIs:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) gibt eine Autorisierungsrichtlinie für einen angegebenen Namen zurück.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) gibt die Standardrichtlinie für die Autorisierung (die Richtlinie zum `[Authorize]` Attribute ohne eine Richtlinie angegeben). 

Durch die Implementierung dieser zwei APIs, können Sie anpassen, wie die Autorisierungsrichtlinien bereitgestellt werden.

## <a name="parameterized-authorize-attribute-example"></a>Parametrisierte autorisieren Beispiel für ein Attribut

Ein Szenario, in dem `IAuthorizationPolicyProvider` eignet sich benutzerdefinierte ermöglicht `[Authorize]` Attribute, deren Anforderungen richten sich nach Parameter. Beispielsweise ist in [Richtlinie basierende Autorisierung](xref:security/authorization/policies) Dokumentation einer Age-basierte ("AtLeast21") Richtlinie wurde als Stichprobe verwendet. Wenn verschiedene steuerungsaktionen in einer app für Benutzer verfügbar gemacht werden sollen *unterschiedliche* Altersgruppen, es möglicherweise sinnvoll, viele verschiedene Age-basierte Richtlinien zu verwenden. Statt zu registrieren alle anderen Age-basierte Richtlinien, die in die Anwendung muss `AuthorizationOptions`, können Sie die Richtlinien mit einem benutzerdefinierten dynamisch generieren `IAuthorizationPolicyProvider`. Um mit den Richtlinien einfacher zu gestalten, können Sie Aktionen mit benutzerdefinierten Autorisierungs-Attribut, z. B. kommentieren `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Benutzerdefinierte Autorisierungsattribute

Autorisierungsrichtlinien werden durch ihre Namen identifiziert. Die benutzerdefinierte `MinimumAgeAuthorizeAttribute` beschrieben muss zuvor Argumente in eine Zeichenfolge zugeordnet werden, die zum Abrufen der entsprechenden Autorisierungsrichtlinie verwendet werden kann. Hierzu können Sie durch Ableiten von `AuthorizeAttribute` und machen die `Age` Eigenschaft Wrap der `AuthorizeAttribute.Policy` Eigenschaft.

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

Dieser Attributtyp wurde eine `Policy` Zeichenfolge basierend auf den hartcodierten-Präfix (`"MinimumAge"`) und eine ganze Zahl, die über den Konstruktor übergeben.

Sie können auf die gleiche Weise wie andere Aktionen anwenden `Authorize` Attribute, außer dass es sich um eine ganze Zahl als Parameter verwendet.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Benutzerdefinierte IAuthorizationPolicyProvider

Die benutzerdefinierte `MinimumAgeAuthorizeAttribute` erleichtert Anforderung Autorisierungsrichtlinien für alle gewünschten Mindestalter. Das nächste Problem zu lösen Sie sicherstellen, dass für alle diese verschiedenen Altersgruppen Autorisierungsrichtlinien verfügbar sind. Dies ist, wenn ein `IAuthorizationPolicyProvider` eignet.

Bei Verwendung `MinimumAgeAuthorizationAttribute`, Richtliniennamen Autorisierung werden entsprechen dem folgenden Muster `"MinimumAge" + Age`, sodass die benutzerdefinierte `IAuthorizationPolicyProvider` Autorisierungsrichtlinien durch generieren soll:

* Analysieren das Alter von den Richtliniennamen an.
* Mithilfe von `AuthorizationPolicyBuiler` zum Erstellen eines neuen `AuthorizationPolicy`
* Hinzufügen von Anforderungen an die Richtlinie basierend auf dem Alter mit `AuthorizationPolicyBuilder.AddRequirements`. In anderen Szenarien können `RequireClaim`, `RequireRole`, oder `RequireUserName` stattdessen.

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Mehrere Authorization Policy-Anbieter

Bei Verwendung von benutzerdefinierten `IAuthorizationPolicyProvider` Implementierungen, Bedenken Sie, die ASP.NET Core verwendet nur eine Instanz des `IAuthorizationPolicyProvider`. Wenn ein benutzerdefinierter Anbieter können alle Richtliniennamen Autorisierungsrichtlinien bereit ist, sollten sie auf eine sicherungsanbieter zurückgegriffen. Richtliniennamen gehören diejenigen, die eine Standardrichtlinie für stammen `[Authorize]` Attribute ohne Namen.

Betrachten Sie beispielsweise, dass eine Anwendung benutzerdefinierte Age-Richtlinien und Herkömmliche rollenbasierte richtlinienabrufs erforderlich. Eine solche Anwendung konnte eine benutzerdefinierte Richtlinie Autorisierungsanbieter verwenden, die:

* Versucht, Richtliniennamen zu analysieren. 
* Aufrufe an einen anderen Richtlinienanbieter (z. B. `DefaultAuthorizationPolicyProvider`) Wenn der Name der Richtlinie ein Alter enthält.

## <a name="default-policy"></a>Standardrichtlinie

Zusätzlich zur Bereitstellung von benannten Autorisierungsrichtlinien, eine benutzerdefinierte `IAuthorizationPolicyProvider` muss implementieren `GetDefaultPolicyAsync` ermöglichen eine Autorisierungsrichtlinie für `[Authorize]` Attribute ohne ein Richtlinienname angegeben.

In vielen Fällen einen authentifizierten Benutzer dieses Autorisierungsattribut nur erfordern, damit Sie die erforderliche Richtlinie mit einem Aufruf von vornehmen können `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Wie bei der alle Aspekte einer benutzerdefinierten `IAuthorizationPolicyProvider`, Sie können hierzu Bedarf anpassen. In einigen Fällen:

* Standard-Autorisierungsrichtlinien möglicherweise nicht verwendet werden.
* Die Standardrichtlinie abrufen kann an ein Fallback delegiert werden `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Verwenden eine benutzerdefinierte IAuthorizationPolicyProvider

Zum Verwenden von benutzerdefinierter Richtlinien von einem `IAuthorizationPolicyProvider`, müssen Sie:

* Registrieren Sie die entsprechende `AuthorizationHandler` Typen mit abhängigkeiteneinschleusung (beschrieben [Richtlinie basierende Autorisierung](xref:security/authorization/policies#authorization-handlers)), wie bei allen Richtlinie basierende autorisierungsszenarien.
* Registrieren Sie die benutzerdefinierte `IAuthorizationPolicyProvider` Geben Sie die app abhängigkeitsauflistung Injection-Dienst (in `Startup.ConfigureServices`) um den Standardanbieter für die Richtlinie zu ersetzen.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Eine vollständige benutzerdefinierte `IAuthorizationPolicyProvider` Beispiel steht in den [Aspnet/AuthSamples GitHub-Repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
