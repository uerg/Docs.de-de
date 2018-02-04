---
title: Ressource-basierte Autorisierung in ASP.NET Core
author: scottaddie
description: Erfahren Sie, wie bei der Authorize-Attribut ausreichen wird nicht ressourcenbasierte Autorisierung in einer app ASP.NET Core implementieren.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a><span data-ttu-id="07238-103">Ressourcenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07238-103">Resource-based authorization</span></span>

<span data-ttu-id="07238-104">Autorisierungsstrategie richtet sich nach der Ressource, auf die zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="07238-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="07238-105">Betrachten Sie ein Dokument Author-Eigenschaft besitzt.</span><span class="sxs-lookup"><span data-stu-id="07238-105">Consider a document which has an author property.</span></span> <span data-ttu-id="07238-106">Nur der Ersteller ist zulässig, um das Dokument zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="07238-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="07238-107">Daher müssen Sie das Dokument aus dem Datenspeicher abgerufen werden vor der Auswertung der Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="07238-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="07238-108">Attribut-Auswertung erfolgt, vor dem Datenbindung und vor der Ausführung der Seite "-Ereignishandler oder Aktion, die das Dokument lädt.</span><span class="sxs-lookup"><span data-stu-id="07238-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="07238-109">Aus diesen Gründen deklarative Autorisierung mit einer `[Authorize]` Attribut wird nicht ausreichen.</span><span class="sxs-lookup"><span data-stu-id="07238-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="07238-110">Stattdessen können Sie eine benutzerdefinierten Autorisierungs-Methode aufrufen&mdash;als imperative Autorisierung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="07238-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="07238-111">Verwenden der [Beispiel-apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)), die in diesem Thema beschriebenen Funktionen zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="07238-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="07238-112">[Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt](xref:security/authorization/secure-data) enthält eine Beispielapp, ressourcenbasierte Autorisierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="07238-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="07238-113">Verwenden Sie eine imperative Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07238-113">Use imperative authorization</span></span>

<span data-ttu-id="07238-114">Autorisierung wird als implementiert eine [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service und registriert ist, in die Auflistung innerhalb der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="07238-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="07238-115">Der Dienst wird über zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) Seitenhandler oder Aktionen.</span><span class="sxs-lookup"><span data-stu-id="07238-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="07238-116">`IAuthorizationService`verfügt über zwei `AuthorizeAsync` methodenüberladungen: eine akzeptieren, die Ressource und den Richtliniennamen und die andere akzeptiert die Ressource und eine Liste der Anforderungen zum Auswerten.</span><span class="sxs-lookup"><span data-stu-id="07238-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07238-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07238-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07238-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07238-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="07238-119">Im folgenden Beispiel wird die Ressource, die gesichert werden in eine benutzerdefinierte geladen `Document` Objekt.</span><span class="sxs-lookup"><span data-stu-id="07238-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="07238-120">Ein `AuthorizeAsync` Überladung wird aufgerufen, um zu bestimmen, ob der aktuelle Benutzer zulässig ist, um das angegebene Dokument zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="07238-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="07238-121">Eine benutzerdefinierte Autorisierungsrichtlinie für "EditPolicy" wird die Entscheidung umgerechnet.</span><span class="sxs-lookup"><span data-stu-id="07238-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="07238-122">Finden Sie unter [benutzerdefinierte Richtlinie basierende Autorisierung](xref:security/authorization/policies) Weitere Informationen zum Erstellen von Autorisierungsrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="07238-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="07238-123">Der folgende Code-Beispielen wird davon ausgegangen, Authentifizierung ausgeführt wurde und den Satz der `User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07238-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07238-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07238-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07238-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07238-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="07238-126">Schreiben Sie einen Handler ressourcenbasierte</span><span class="sxs-lookup"><span data-stu-id="07238-126">Write a resource-based handler</span></span>

<span data-ttu-id="07238-127">Schreiben einen Handler für ressourcenbasierte Autorisierung ist wesentlich unterscheidet nicht [schreiben einen Handler für einfache Anforderungen](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="07238-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="07238-128">Erstellen Sie eine benutzerdefinierte Anforderung Klasse und Implementieren einer Anforderung Handler-Klasse.</span><span class="sxs-lookup"><span data-stu-id="07238-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="07238-129">Die Handlerklasse gibt an, die Anforderungen und die Ressourcentyp.</span><span class="sxs-lookup"><span data-stu-id="07238-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="07238-130">Z. B. einen Ereignishandler mit einem `SameAuthorRequirement` Anforderung und eine `Document` Ressource sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="07238-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07238-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07238-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07238-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07238-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="07238-133">Registrieren Sie die Anforderungen und die Ereignishandler in der `Startup.ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="07238-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="07238-134">Prozessanforderungen</span><span class="sxs-lookup"><span data-stu-id="07238-134">Operational requirements</span></span>

<span data-ttu-id="07238-135">Wenn Sie Ihre Entscheidungen basierend auf den Ergebnissen des CRUD vornehmen (**C**chine, **R**eitere, **U**palten aktualisieren, **D**Gesam) Operationen, mit dem die [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Hilfsklasse.</span><span class="sxs-lookup"><span data-stu-id="07238-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="07238-136">Diese Klasse ermöglicht Ihnen das Schreiben von eines einzelnen Handlers anstelle einer einzelnen Klasse für jeden Vorgangstyp.</span><span class="sxs-lookup"><span data-stu-id="07238-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="07238-137">Geben Sie einige Vorgangsnamen, um es verwenden zu um können:</span><span class="sxs-lookup"><span data-stu-id="07238-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="07238-138">Der Handler wird folgendermaßen implementiert, mit einem `OperationAuthorizationRequirement` Anforderung und eine `Document` Ressource:</span><span class="sxs-lookup"><span data-stu-id="07238-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07238-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07238-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07238-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07238-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="07238-141">Die vorherigen Handler überprüft den Vorgang unter Verwendung der Ressource, die Identität des Benutzers und der Anforderung `Name` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07238-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="07238-142">Um einen Handler für die operative Ressource aufzurufen, geben Sie den Vorgang beim Aufrufen von `AuthorizeAsync` in Ihrer Seitenhandler oder Aktion.</span><span class="sxs-lookup"><span data-stu-id="07238-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="07238-143">Im folgenden Beispiel wird bestimmt, ob der authentifizierte Benutzer berechtigt ist, um das angegebene Dokument anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="07238-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="07238-144">Der folgende Code-Beispielen wird davon ausgegangen, Authentifizierung ausgeführt wurde und den Satz der `User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07238-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07238-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07238-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="07238-146">Wenn die Autorisierung erfolgreich ist, wird die Seite für das Dokument anzeigen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="07238-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="07238-147">Wenn die Autorisierung fehlschlägt, aber der Benutzer authentifiziert wird, Zurückgeben von `ForbidResult` informiert authentifizierungsmiddleware, die Fehler bei der Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="07238-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="07238-148">Ein `ChallengeResult` wird zurückgegeben, wenn eine Authentifizierung durchgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="07238-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="07238-149">Für interaktive Browser-Clients kann es zum Umleiten des Benutzers zu einer Anmeldeseite um geeignet sein.</span><span class="sxs-lookup"><span data-stu-id="07238-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07238-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07238-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="07238-151">Wenn die Autorisierung erfolgreich ist, wird die Ansicht für das Dokument zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="07238-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="07238-152">Wenn die Autorisierung fehlschlägt, zurückgeben `ChallengeResult` Authentifizierung-Middleware informiert werden, dass Fehler bei der Autorisierung, die Middleware dauert die entsprechende Antwort.</span><span class="sxs-lookup"><span data-stu-id="07238-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="07238-153">Eine entsprechende Antwort konnte einen Statuscode 401 oder 403 zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="07238-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="07238-154">Für die interaktive Browser-Clients, kann dies bedeuten Weiterleiten des Benutzers zu einer Anmeldeseite um.</span><span class="sxs-lookup"><span data-stu-id="07238-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
