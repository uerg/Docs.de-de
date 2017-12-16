---
title: Benutzerdefinierte Richtlinie basierende Autorisierung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen und Verwenden von benutzerdefinierten Autorisierungs-Policy-Handler zum Erzwingen von autorisierungsanforderungen in einer ASP.NET Core-app.
keywords: ASP.NET Core, Autorisierung, die benutzerdefinierte Richtlinie, die Autorisierungsrichtlinie
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="839b2-104">Benutzerdefinierte Richtlinie basierende Autorisierung</span><span class="sxs-lookup"><span data-stu-id="839b2-104">Custom policy-based authorization</span></span>

<span data-ttu-id="839b2-105">Im Hintergrund [rollenbasierte Autorisierung](xref:security/authorization/roles) und [anspruchsbasierte Autorisierung](xref:security/authorization/claims) erforderlich, einen Handler für die Anforderung und eine vorkonfigurierte Richtlinie verwenden.</span><span class="sxs-lookup"><span data-stu-id="839b2-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="839b2-106">Diese Bausteine unterstützen den Ausdruck der auswertungen für die Autorisierung im Code.</span><span class="sxs-lookup"><span data-stu-id="839b2-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="839b2-107">Das Ergebnis ist eine umfangreichere, wiederverwendbare, testfähig Autorisierung-Struktur.</span><span class="sxs-lookup"><span data-stu-id="839b2-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="839b2-108">Eine Autorisierungsrichtlinie besteht aus einer oder mehreren Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="839b2-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="839b2-109">Wird als Teil der Dienstkonfiguration Autorisierung in registriert die `ConfigureServices` Methode der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="839b2-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="839b2-110">Im vorherigen Beispiel wird eine Richtlinie "AtLeast21" erstellt.</span><span class="sxs-lookup"><span data-stu-id="839b2-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="839b2-111">Er verfügt über eine einzelne Anforderung, dass von einem Mindestalter, die als Parameter an die Anforderung angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="839b2-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="839b2-112">Richtlinien werden angewendet, mit der `[Authorize]` Attribut mit dem Richtliniennamen.</span><span class="sxs-lookup"><span data-stu-id="839b2-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="839b2-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="839b2-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="839b2-114">Anforderungen</span><span class="sxs-lookup"><span data-stu-id="839b2-114">Requirements</span></span>

<span data-ttu-id="839b2-115">Eine autorisierungsanforderung ist eine Auflistung von Datenparametern, die eine Richtlinie zum Auswerten des aktuellen Benutzerprinzipals verwenden können.</span><span class="sxs-lookup"><span data-stu-id="839b2-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="839b2-116">In unserer "AtLeast21"-Richtlinie, die Anforderung ist ein einzelner Parameter&mdash;das Mindestalter.</span><span class="sxs-lookup"><span data-stu-id="839b2-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="839b2-117">Implementiert eine Anforderung `IAuthorizationRequirement`, dies ist eine leere Markierungsschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="839b2-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="839b2-118">Eine parametrisierte Mindestalter-Anforderung kann wie folgt implementiert:</span><span class="sxs-lookup"><span data-stu-id="839b2-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="839b2-119">Eine Anforderung benötigen nicht Daten oder Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="839b2-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="839b2-120">Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="839b2-120">Authorization handlers</span></span>

<span data-ttu-id="839b2-121">Ein Authorization-Handler ist für die Auswertung von Eigenschaften für eine Anforderung verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="839b2-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="839b2-122">Der Handler für die Autorisierung wertet die Anforderungen für ein bereitgestelltes `AuthorizationHandlerContext` zu bestimmen, ob der Zugriff zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="839b2-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="839b2-123">Kann die Anforderung besteht [mehrere Handler](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="839b2-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="839b2-124">Handler erben `AuthorizationHandler<T>`, wobei `T` ist die Anforderung behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="839b2-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="839b2-125">Der Handler Mindestalter kann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="839b2-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="839b2-126">Der vorangehende Code bestimmt, ob der aktuelle Benutzer principal aufweist, ein Geburtsdatum Anspruch, der von einem bekannten und vertrauenswürdigen Aussteller ausgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="839b2-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="839b2-127">Autorisierung kann nicht auftreten, wenn der Anspruch nicht vorhanden ist, in diesem Fall eine vollständige Aufgabe zurückgegeben hat.</span><span class="sxs-lookup"><span data-stu-id="839b2-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="839b2-128">Wenn ein Anspruch vorhanden ist, wird das Alter des Benutzers berechnet.</span><span class="sxs-lookup"><span data-stu-id="839b2-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="839b2-129">Wenn der Benutzer das Mindestalter definiert, indem Sie die Anforderung erfüllt, wird die Autorisierung erfolgreich angesehen.</span><span class="sxs-lookup"><span data-stu-id="839b2-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="839b2-130">Wenn die Autorisierung erfolgreich war, `context.Succeed` mit die Anforderung erfüllt, als Parameter aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="839b2-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="839b2-131">Handler-Registrierung</span><span class="sxs-lookup"><span data-stu-id="839b2-131">Handler registration</span></span>

<span data-ttu-id="839b2-132">Handler werden in der Auflistung der Dienste während der Konfiguration registriert.</span><span class="sxs-lookup"><span data-stu-id="839b2-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="839b2-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="839b2-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="839b2-134">Jeder Handler, die Dienste-Auflistung hinzugefügt wird, durch den Aufruf `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="839b2-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="839b2-135">Was sollte ein Handler zurückgeben?</span><span class="sxs-lookup"><span data-stu-id="839b2-135">What should a handler return?</span></span>

<span data-ttu-id="839b2-136">Beachten Sie, dass die `Handle` Methode in der [Handler Beispiel](#security-authorization-handler-example) gibt keinen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="839b2-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="839b2-137">Wie der Status Erfolg oder Fehler angezeigt wird?</span><span class="sxs-lookup"><span data-stu-id="839b2-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="839b2-138">Ein Handler gibt die erfolgreiche Ausführung durch den Aufruf `context.Succeed(IAuthorizationRequirement requirement)`, übergeben der Anforderung, die erfolgreich überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="839b2-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="839b2-139">Ein Ereignishandler muss nicht im allgemeinen Fehler zu behandeln, wie andere Handler für die gleiche Anforderung erfolgreich ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="839b2-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="839b2-140">Aufrufen, um Fehler, auch wenn andere Handler für die Anforderung erfolgreich zu garantieren, `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="839b2-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="839b2-141">Unabhängig davon, was Sie in den Handler aufrufen werden alle Handler für eine Anforderung aufgerufen werden, wenn eine Richtlinie der Anforderung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="839b2-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="839b2-142">Anforderungen an, wie z. B. Protokollierung, haben Nebeneffekte, die immer stattfinden wird dadurch auch wenn `context.Fail()` in einen anderen Handler aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="839b2-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="839b2-143">Warum würde ich mehrere Handler für eine Anforderung?</span><span class="sxs-lookup"><span data-stu-id="839b2-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="839b2-144">In Fällen soll Auswertung auf eine **oder** Basis, mehrere Handler für eine einzelne Anforderung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="839b2-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="839b2-145">Microsoft hat es sich beispielsweise um Türen, die nur mit Schlüssel Karten zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="839b2-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="839b2-146">Wenn Sie Ihre Schlüsselkarte zu Hause lassen, wird die Apparate druckt einen temporären Aufkleber und öffnet die Tür für Sie.</span><span class="sxs-lookup"><span data-stu-id="839b2-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="839b2-147">In diesem Fall müssten Sie eine einzelne Anforderung *BuildingEntry*, aber mehrere Handler, die jeweils eine einzelne Anforderung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="839b2-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="839b2-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="839b2-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="839b2-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="839b2-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="839b2-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="839b2-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="839b2-151">Stellen Sie sicher, dass die beiden Ereignishandler sind [registriert](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="839b2-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="839b2-152">Wenn die beiden Ereignishandler ist erfolgreich, wenn eine Richtlinie ausgewertet wird die `BuildingEntryRequirement`, richtlinienauswertung erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="839b2-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="839b2-153">Func verwenden, um eine Richtlinie zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="839b2-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="839b2-154">Möglicherweise gibt es Situationen, in welche, die verhindert eine Richtlinie einfach zu express im Code ist.</span><span class="sxs-lookup"><span data-stu-id="839b2-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="839b2-155">Es ist möglich, geben Sie einen `Func<AuthorizationHandlerContext, bool>` beim Konfigurieren der Richtlinie mit den `RequireAssertion` Richtlinie-Generator.</span><span class="sxs-lookup"><span data-stu-id="839b2-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="839b2-156">Beispielsweise der vorherigen `BadgeEntryHandler` könnte folgendermaßen umgeschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="839b2-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="839b2-157">Zugreifen auf die MVC-Anforderungskontext in Handlern abfangen</span><span class="sxs-lookup"><span data-stu-id="839b2-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="839b2-158">Die `HandleRequirementAsync` Methode, die Sie, in einem Ereignishandler für die Autorisierung implementieren verfügt über zwei Parameter: eine `AuthorizationHandlerContext` und `TRequirement` Sie verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="839b2-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="839b2-159">Frameworks wie z. B. Mvc- oder Jabbr sind frei, um ein Objekt zum Hinzufügen der `Resource` Eigenschaft auf die `AuthorizationHandlerContext` um zusätzliche Informationen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="839b2-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="839b2-160">Beispielsweise MVC übergibt eine Instanz des [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in der `Resource` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="839b2-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="839b2-161">Diese Eigenschaft ermöglicht den Zugriff auf `HttpContext`, `RouteData`, und alles, was sonst von Razor-Seiten und MVC bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="839b2-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="839b2-162">Die Verwendung der `Resource` Eigenschaft ist für bestimmte Framework.</span><span class="sxs-lookup"><span data-stu-id="839b2-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="839b2-163">Mithilfe der Informationen in der `Resource` Eigenschaft schränkt die Autorisierungsrichtlinien auf bestimmten Frameworks.</span><span class="sxs-lookup"><span data-stu-id="839b2-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="839b2-164">Sollten Sie eine Umwandlung der `Resource` Eigenschaft mit der `as` -Schlüsselwort, und bestätigen Sie dann die Umwandlung wurde erfolgreich ausgeführt werden, um sicherzustellen, dass Ihr Code keine stürzt ab mit einer `InvalidCastException` bei Ausführung auf anderen Frameworks:</span><span class="sxs-lookup"><span data-stu-id="839b2-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
