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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="41e6b-103">Benutzerdefinierte Autorisierung-Policy-Anbietern, die mithilfe von IAuthorizationPolicyProvider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41e6b-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="41e6b-104">Durch [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="41e6b-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="41e6b-105">In der Regel bei Verwendung [Richtlinie basierende Autorisierung](xref:security/authorization/policies), Richtlinien werden durch den Aufruf registriert `AuthorizationOptions.AddPolicy` als Teil der Dienstkonfiguration für die Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="41e6b-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="41e6b-106">In einigen Szenarien ist es möglicherweise nicht möglich (oder wünschenswert) aller Autorisierungsrichtlinien auf diese Weise zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="41e6b-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="41e6b-107">In diesen Fällen können Sie eine benutzerdefinierte `IAuthorizationPolicyProvider` steuern, wie die Autorisierungsrichtlinien bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="41e6b-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="41e6b-108">Beispiele für Szenarien, bei einer benutzerdefinierten [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) kann hilfreich sein, gehören:</span><span class="sxs-lookup"><span data-stu-id="41e6b-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="41e6b-109">Verwenden einen externen Dienst, um die richtlinienauswertung sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="41e6b-110">Verwenden einen großen Bereich von Richtlinien (für verschiedene Raumnummer oder ALTER, z. B.), daher es nicht sinnvoll hinzuzufügende jede einzelne Autorisierungsrichtlinie mit einer `AuthorizationOptions.AddPolicy` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="41e6b-111">Erstellen von Richtlinien zur Laufzeit basierend auf Informationen in einer externen Datenquelle (z. B. eine Datenbank) oder autorisierungsanforderungen dynamisch mithilfe eines anderen Mechanismus bestimmen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="41e6b-112">Anpassen des Abrufens von Richtlinien</span><span class="sxs-lookup"><span data-stu-id="41e6b-112">Customizing policy retrieval</span></span>

<span data-ttu-id="41e6b-113">ASP.NET Core-apps mit einer Implementierung der der `IAuthorizationPolicyProvider` Schnittstelle, um Autorisierungsrichtlinien abzurufen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="41e6b-114">Standardmäßig [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) registriert und verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="41e6b-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="41e6b-115">`DefaultAuthorizationPolicyProvider` Gibt die Richtlinien aus der `AuthorizationOptions` in bereitgestellten ein `IServiceCollection.AddAuthorization` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="41e6b-116">Sie können dieses Verhalten anpassen, indem Sie ein anderes registrieren `IAuthorizationPolicyProvider` Implementierung in der app [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) Container.</span><span class="sxs-lookup"><span data-stu-id="41e6b-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="41e6b-117">Die `IAuthorizationPolicyProvider` Schnittstelle enthält zwei APIs:</span><span class="sxs-lookup"><span data-stu-id="41e6b-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="41e6b-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) gibt eine Autorisierungsrichtlinie für einen angegebenen Namen zurück.</span><span class="sxs-lookup"><span data-stu-id="41e6b-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="41e6b-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) gibt die Standardrichtlinie für die Autorisierung (die Richtlinie zum `[Authorize]` Attribute ohne eine Richtlinie angegeben).</span><span class="sxs-lookup"><span data-stu-id="41e6b-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="41e6b-120">Durch die Implementierung dieser zwei APIs, können Sie anpassen, wie die Autorisierungsrichtlinien bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="41e6b-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="41e6b-121">Parametrisierte autorisieren Beispiel für ein Attribut</span><span class="sxs-lookup"><span data-stu-id="41e6b-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="41e6b-122">Ein Szenario, in dem `IAuthorizationPolicyProvider` eignet sich benutzerdefinierte ermöglicht `[Authorize]` Attribute, deren Anforderungen richten sich nach Parameter.</span><span class="sxs-lookup"><span data-stu-id="41e6b-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="41e6b-123">Beispielsweise ist in [Richtlinie basierende Autorisierung](xref:security/authorization/policies) Dokumentation einer Age-basierte ("AtLeast21") Richtlinie wurde als Stichprobe verwendet.</span><span class="sxs-lookup"><span data-stu-id="41e6b-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="41e6b-124">Wenn verschiedene steuerungsaktionen in einer app für Benutzer verfügbar gemacht werden sollen *unterschiedliche* Altersgruppen, es möglicherweise sinnvoll, viele verschiedene Age-basierte Richtlinien zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="41e6b-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="41e6b-125">Statt zu registrieren alle anderen Age-basierte Richtlinien, die in die Anwendung muss `AuthorizationOptions`, können Sie die Richtlinien mit einem benutzerdefinierten dynamisch generieren `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="41e6b-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="41e6b-126">Um mit den Richtlinien einfacher zu gestalten, können Sie Aktionen mit benutzerdefinierten Autorisierungs-Attribut, z. B. kommentieren `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="41e6b-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="41e6b-127">Benutzerdefinierte Autorisierungsattribute</span><span class="sxs-lookup"><span data-stu-id="41e6b-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="41e6b-128">Autorisierungsrichtlinien werden durch ihre Namen identifiziert.</span><span class="sxs-lookup"><span data-stu-id="41e6b-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="41e6b-129">Die benutzerdefinierte `MinimumAgeAuthorizeAttribute` beschrieben muss zuvor Argumente in eine Zeichenfolge zugeordnet werden, die zum Abrufen der entsprechenden Autorisierungsrichtlinie verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="41e6b-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="41e6b-130">Hierzu können Sie durch Ableiten von `AuthorizeAttribute` und machen die `Age` Eigenschaft Wrap der `AuthorizeAttribute.Policy` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="41e6b-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="41e6b-131">Dieser Attributtyp wurde eine `Policy` Zeichenfolge basierend auf den hartcodierten-Präfix (`"MinimumAge"`) und eine ganze Zahl, die über den Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="41e6b-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="41e6b-132">Sie können auf die gleiche Weise wie andere Aktionen anwenden `Authorize` Attribute, außer dass es sich um eine ganze Zahl als Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="41e6b-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="41e6b-133">Benutzerdefinierte IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="41e6b-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="41e6b-134">Die benutzerdefinierte `MinimumAgeAuthorizeAttribute` erleichtert Anforderung Autorisierungsrichtlinien für alle gewünschten Mindestalter.</span><span class="sxs-lookup"><span data-stu-id="41e6b-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="41e6b-135">Das nächste Problem zu lösen Sie sicherstellen, dass für alle diese verschiedenen Altersgruppen Autorisierungsrichtlinien verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="41e6b-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="41e6b-136">Dies ist, wenn ein `IAuthorizationPolicyProvider` eignet.</span><span class="sxs-lookup"><span data-stu-id="41e6b-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="41e6b-137">Bei Verwendung `MinimumAgeAuthorizationAttribute`, Richtliniennamen Autorisierung werden entsprechen dem folgenden Muster `"MinimumAge" + Age`, sodass die benutzerdefinierte `IAuthorizationPolicyProvider` Autorisierungsrichtlinien durch generieren soll:</span><span class="sxs-lookup"><span data-stu-id="41e6b-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="41e6b-138">Analysieren das Alter von den Richtliniennamen an.</span><span class="sxs-lookup"><span data-stu-id="41e6b-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="41e6b-139">Mithilfe von `AuthorizationPolicyBuiler` zum Erstellen eines neuen `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="41e6b-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="41e6b-140">Hinzufügen von Anforderungen an die Richtlinie basierend auf dem Alter mit `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="41e6b-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="41e6b-141">In anderen Szenarien können `RequireClaim`, `RequireRole`, oder `RequireUserName` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="41e6b-142">Mehrere Authorization Policy-Anbieter</span><span class="sxs-lookup"><span data-stu-id="41e6b-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="41e6b-143">Bei Verwendung von benutzerdefinierten `IAuthorizationPolicyProvider` Implementierungen, Bedenken Sie, die ASP.NET Core verwendet nur eine Instanz des `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="41e6b-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="41e6b-144">Wenn ein benutzerdefinierter Anbieter können alle Richtliniennamen Autorisierungsrichtlinien bereit ist, sollten sie auf eine sicherungsanbieter zurückgegriffen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="41e6b-145">Richtliniennamen gehören diejenigen, die eine Standardrichtlinie für stammen `[Authorize]` Attribute ohne Namen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="41e6b-146">Betrachten Sie beispielsweise, dass eine Anwendung benutzerdefinierte Age-Richtlinien und Herkömmliche rollenbasierte richtlinienabrufs erforderlich.</span><span class="sxs-lookup"><span data-stu-id="41e6b-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="41e6b-147">Eine solche Anwendung konnte eine benutzerdefinierte Richtlinie Autorisierungsanbieter verwenden, die:</span><span class="sxs-lookup"><span data-stu-id="41e6b-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="41e6b-148">Versucht, Richtliniennamen zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="41e6b-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="41e6b-149">Aufrufe an einen anderen Richtlinienanbieter (z. B. `DefaultAuthorizationPolicyProvider`) Wenn der Name der Richtlinie ein Alter enthält.</span><span class="sxs-lookup"><span data-stu-id="41e6b-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="41e6b-150">Standardrichtlinie</span><span class="sxs-lookup"><span data-stu-id="41e6b-150">Default policy</span></span>

<span data-ttu-id="41e6b-151">Zusätzlich zur Bereitstellung von benannten Autorisierungsrichtlinien, eine benutzerdefinierte `IAuthorizationPolicyProvider` muss implementieren `GetDefaultPolicyAsync` ermöglichen eine Autorisierungsrichtlinie für `[Authorize]` Attribute ohne ein Richtlinienname angegeben.</span><span class="sxs-lookup"><span data-stu-id="41e6b-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="41e6b-152">In vielen Fällen einen authentifizierten Benutzer dieses Autorisierungsattribut nur erfordern, damit Sie die erforderliche Richtlinie mit einem Aufruf von vornehmen können `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="41e6b-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="41e6b-153">Wie bei der alle Aspekte einer benutzerdefinierten `IAuthorizationPolicyProvider`, Sie können hierzu Bedarf anpassen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="41e6b-154">In einigen Fällen:</span><span class="sxs-lookup"><span data-stu-id="41e6b-154">In some cases:</span></span>

* <span data-ttu-id="41e6b-155">Standard-Autorisierungsrichtlinien möglicherweise nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="41e6b-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="41e6b-156">Die Standardrichtlinie abrufen kann an ein Fallback delegiert werden `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="41e6b-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="41e6b-157">Verwenden eine benutzerdefinierte IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="41e6b-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="41e6b-158">Zum Verwenden von benutzerdefinierter Richtlinien von einem `IAuthorizationPolicyProvider`, müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="41e6b-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="41e6b-159">Registrieren Sie die entsprechende `AuthorizationHandler` Typen mit abhängigkeiteneinschleusung (beschrieben [Richtlinie basierende Autorisierung](xref:security/authorization/policies#authorization-handlers)), wie bei allen Richtlinie basierende autorisierungsszenarien.</span><span class="sxs-lookup"><span data-stu-id="41e6b-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="41e6b-160">Registrieren Sie die benutzerdefinierte `IAuthorizationPolicyProvider` Geben Sie die app abhängigkeitsauflistung Injection-Dienst (in `Startup.ConfigureServices`) um den Standardanbieter für die Richtlinie zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="41e6b-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="41e6b-161">Eine vollständige benutzerdefinierte `IAuthorizationPolicyProvider` Beispiel steht in den [Aspnet/AuthSamples GitHub-Repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="41e6b-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
