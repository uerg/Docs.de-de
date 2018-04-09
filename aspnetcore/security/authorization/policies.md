---
title: Gruppenrichtlinien-basierte Autorisierung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen und Verwenden von Authorization Policy Handler zum Erzwingen von autorisierungsanforderungen in einer ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="5027d-103">Gruppenrichtlinien-basierte Autorisierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5027d-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="5027d-104">Im Hintergrund [rollenbasierte Autorisierung](xref:security/authorization/roles) und [anspruchsbasierte Autorisierung](xref:security/authorization/claims) erforderlich, einen Handler für die Anforderung und eine vorkonfigurierte Richtlinie verwenden.</span><span class="sxs-lookup"><span data-stu-id="5027d-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="5027d-105">Diese Bausteine unterstützen den Ausdruck der auswertungen für die Autorisierung im Code.</span><span class="sxs-lookup"><span data-stu-id="5027d-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="5027d-106">Das Ergebnis ist eine umfangreichere, wiederverwendbare, testfähig Autorisierung-Struktur.</span><span class="sxs-lookup"><span data-stu-id="5027d-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="5027d-107">Eine Autorisierungsrichtlinie besteht aus einer oder mehreren Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="5027d-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="5027d-108">Wird als Teil der Dienstkonfiguration Autorisierung in registriert die `Startup.ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="5027d-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="5027d-109">Im vorherigen Beispiel wird eine Richtlinie "AtLeast21" erstellt.</span><span class="sxs-lookup"><span data-stu-id="5027d-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="5027d-110">Es wurde eine einzelne Anforderung&mdash;, die von einem Mindestalter, die als Parameter an die Anforderung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="5027d-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="5027d-111">Richtlinien werden angewendet, mit der `[Authorize]` Attribut mit dem Richtliniennamen.</span><span class="sxs-lookup"><span data-stu-id="5027d-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="5027d-112">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5027d-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="5027d-113">Anforderungen</span><span class="sxs-lookup"><span data-stu-id="5027d-113">Requirements</span></span>

<span data-ttu-id="5027d-114">Eine autorisierungsanforderung ist eine Auflistung von Datenparametern, die eine Richtlinie zum Auswerten des aktuellen Benutzerprinzipals verwenden können.</span><span class="sxs-lookup"><span data-stu-id="5027d-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="5027d-115">In unserer "AtLeast21"-Richtlinie, die Anforderung ist ein einzelner Parameter&mdash;das Mindestalter.</span><span class="sxs-lookup"><span data-stu-id="5027d-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="5027d-116">Implementiert eine Anforderung [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), dies ist eine leere Markierungsschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="5027d-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="5027d-117">Eine parametrisierte Mindestalter-Anforderung kann wie folgt implementiert:</span><span class="sxs-lookup"><span data-stu-id="5027d-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="5027d-118">Eine Anforderung benötigen nicht Daten oder Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="5027d-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="5027d-119">Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="5027d-119">Authorization handlers</span></span>

<span data-ttu-id="5027d-120">Ein Authorization-Handler ist für die Auswertung von Eigenschaften für eine Anforderung verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="5027d-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="5027d-121">Der Handler für die Autorisierung wertet die Anforderungen für ein bereitgestelltes [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) zu bestimmen, ob der Zugriff zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="5027d-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="5027d-122">Kann die Anforderung besteht [mehrere Handler](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="5027d-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="5027d-123">Ein Ereignishandler kann erben [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), wobei `TRequirement` ist die Anforderung behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="5027d-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="5027d-124">Alternativ können Sie ein Handler implementiert möglicherweise [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , mehr als eine Art der Anforderung zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="5027d-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="5027d-125">Verwenden Sie einen Handler für eine Anforderung</span><span class="sxs-lookup"><span data-stu-id="5027d-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="5027d-126">Im folgenden finden ein Beispiel für eine direkte Beziehung, in der ein Mindestalter-Handler eine einzelne Anforderung nutzt:</span><span class="sxs-lookup"><span data-stu-id="5027d-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="5027d-127">Der vorangehende Code bestimmt, ob der aktuelle Benutzer principal aufweist, ein Geburtsdatum Anspruch, der von einem bekannten und vertrauenswürdigen Aussteller ausgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="5027d-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="5027d-128">Autorisierung kann nicht auftreten, wenn der Anspruch nicht vorhanden ist, in diesem Fall eine vollständige Aufgabe zurückgegeben hat.</span><span class="sxs-lookup"><span data-stu-id="5027d-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="5027d-129">Wenn ein Anspruch vorhanden ist, wird das Alter des Benutzers berechnet.</span><span class="sxs-lookup"><span data-stu-id="5027d-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="5027d-130">Wenn der Benutzer das Mindestalter definiert, indem Sie die Anforderung erfüllt, wird die Autorisierung erfolgreich angesehen.</span><span class="sxs-lookup"><span data-stu-id="5027d-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="5027d-131">Wenn die Autorisierung erfolgreich war, `context.Succeed` mit zufrieden Anforderung als einzigen Parameter aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="5027d-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="5027d-132">Verwenden Sie einen Ereignishandler für mehrere Anforderungen</span><span class="sxs-lookup"><span data-stu-id="5027d-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="5027d-133">Im folgenden finden ein Beispiel für eine 1: n-Beziehung, in der ein Berechtigung-Handler drei Anforderungen nutzt:</span><span class="sxs-lookup"><span data-stu-id="5027d-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="5027d-134">Der vorangehende Code durchläuft [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;eine Eigenschaft nicht mit der Anforderungen als erfolgreich gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="5027d-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="5027d-135">Wenn der Benutzer eine Leseberechtigung verfügt, muss er entweder "Besitzer" oder "Sponsor" aus, Zugriff auf die angeforderte Ressource sein.</span><span class="sxs-lookup"><span data-stu-id="5027d-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="5027d-136">Wenn der Benutzer hat bearbeiten oder löschen, muss er Besitzer Zugriff auf die angeforderte Ressource sein.</span><span class="sxs-lookup"><span data-stu-id="5027d-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="5027d-137">Wenn die Autorisierung erfolgreich war, `context.Succeed` mit zufrieden Anforderung als einzigen Parameter aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="5027d-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="5027d-138">Handler-Registrierung</span><span class="sxs-lookup"><span data-stu-id="5027d-138">Handler registration</span></span>

<span data-ttu-id="5027d-139">Handler werden in der Auflistung der Dienste während der Konfiguration registriert.</span><span class="sxs-lookup"><span data-stu-id="5027d-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="5027d-140">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5027d-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="5027d-141">Jeder Handler, die Dienste-Auflistung hinzugefügt wird, durch den Aufruf `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="5027d-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="5027d-142">Was sollte ein Handler zurückgeben?</span><span class="sxs-lookup"><span data-stu-id="5027d-142">What should a handler return?</span></span>

<span data-ttu-id="5027d-143">Beachten Sie, dass die `Handle` Methode in der [Handler Beispiel](#security-authorization-handler-example) gibt keinen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="5027d-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="5027d-144">Wie der Status Erfolg oder Fehler angezeigt wird?</span><span class="sxs-lookup"><span data-stu-id="5027d-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="5027d-145">Ein Handler gibt die erfolgreiche Ausführung durch den Aufruf `context.Succeed(IAuthorizationRequirement requirement)`, übergeben der Anforderung, die erfolgreich überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="5027d-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="5027d-146">Ein Handler muss im allgemeinen Fehler zu behandeln, wie andere Handler für die gleiche Anforderung erfolgreich ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="5027d-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="5027d-147">Aufrufen, um Fehler, auch wenn andere Handler für die Anforderung erfolgreich zu garantieren, `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="5027d-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="5027d-148">Bei Festlegung auf `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) -Eigenschaft (verfügbar in ASP.NET Core 1.1 und höher) kurzgeschlossen wird die Ausführung der Ereignishandler beim `context.Fail` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="5027d-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="5027d-149">`InvokeHandlersAfterFailure` Standardmäßig `true`, in diesem Fall alle Ereignishandler aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="5027d-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="5027d-150">Anforderungen für die Nebeneffekte, wie z. B. Protokollierung, erzeugen, die immer stattfinden dadurch auch wenn `context.Fail` in einen anderen Handler aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="5027d-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="5027d-151">Warum würde ich mehrere Handler für eine Anforderung?</span><span class="sxs-lookup"><span data-stu-id="5027d-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="5027d-152">In Fällen soll Auswertung auf eine **oder** Basis, mehrere Handler für eine einzelne Anforderung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="5027d-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="5027d-153">Microsoft hat es sich beispielsweise um Türen, die nur mit Schlüssel Karten zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="5027d-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="5027d-154">Wenn Sie Ihre Schlüsselkarte zu Hause lassen, wird die Apparate druckt einen temporären Aufkleber und öffnet die Tür für Sie.</span><span class="sxs-lookup"><span data-stu-id="5027d-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="5027d-155">In diesem Fall müssten Sie eine einzelne Anforderung *BuildingEntry*, aber mehrere Handler, die jeweils eine einzelne Anforderung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5027d-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="5027d-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="5027d-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="5027d-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="5027d-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="5027d-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="5027d-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="5027d-159">Stellen Sie sicher, dass die beiden Ereignishandler sind [registriert](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="5027d-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="5027d-160">Wenn die beiden Ereignishandler ist erfolgreich, wenn eine Richtlinie ausgewertet wird die `BuildingEntryRequirement`, richtlinienauswertung erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5027d-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="5027d-161">Func verwenden, um eine Richtlinie zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="5027d-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="5027d-162">Möglicherweise gibt es Situationen, in welche, die verhindert eine Richtlinie einfach zu express im Code ist.</span><span class="sxs-lookup"><span data-stu-id="5027d-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="5027d-163">Es ist möglich, geben Sie einen `Func<AuthorizationHandlerContext, bool>` beim Konfigurieren der Richtlinie mit den `RequireAssertion` Richtlinie-Generator.</span><span class="sxs-lookup"><span data-stu-id="5027d-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="5027d-164">Beispielsweise der vorherigen `BadgeEntryHandler` könnte folgendermaßen umgeschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="5027d-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="5027d-165">Zugreifen auf die MVC-Anforderungskontext in Handlern abfangen</span><span class="sxs-lookup"><span data-stu-id="5027d-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="5027d-166">Die `HandleRequirementAsync` Methode, die Sie, in einem Ereignishandler für die Autorisierung implementieren verfügt über zwei Parameter: eine `AuthorizationHandlerContext` und `TRequirement` Sie verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="5027d-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="5027d-167">Frameworks wie z. B. Mvc- oder Jabbr sind frei, um ein Objekt zum Hinzufügen der `Resource` Eigenschaft auf die `AuthorizationHandlerContext` um zusätzliche Informationen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="5027d-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="5027d-168">Beispielsweise MVC übergibt eine Instanz des [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in der `Resource` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5027d-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="5027d-169">Diese Eigenschaft ermöglicht den Zugriff auf `HttpContext`, `RouteData`, und alles, was sonst von Razor-Seiten und MVC bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="5027d-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="5027d-170">Die Verwendung der `Resource` Eigenschaft ist für bestimmte Framework.</span><span class="sxs-lookup"><span data-stu-id="5027d-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="5027d-171">Mithilfe der Informationen in der `Resource` Eigenschaft schränkt die Autorisierungsrichtlinien auf bestimmten Frameworks.</span><span class="sxs-lookup"><span data-stu-id="5027d-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="5027d-172">Sollten Sie eine Umwandlung der `Resource` Eigenschaft mit der `as` -Schlüsselwort, und bestätigen Sie dann die Umwandlung wurde erfolgreich ausgeführt werden, um sicherzustellen, dass Ihr Code keine stürzt ab mit einer `InvalidCastException` bei Ausführung auf anderen Frameworks:</span><span class="sxs-lookup"><span data-stu-id="5027d-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
