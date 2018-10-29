---
title: Ressourcenbasierte Autorisierung in ASP.NET Core
author: scottaddie
description: Erfahren Sie, wie Sie die ressourcenbasierte Autorisierung in einer ASP.NET Core-app zu implementieren, wenn ein Authorize-Attributs ausreichen wird nicht.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206695"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="f6ba1-103">Ressourcenbasierte Autorisierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6ba1-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="f6ba1-104">Autorisierungsstrategie für die hängt von der Ressource, auf die zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="f6ba1-105">Erwägen Sie ein Dokument, das eine Author-Eigenschaft hat.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-105">Consider a document which has an author property.</span></span> <span data-ttu-id="f6ba1-106">Nur der Autor kann das Dokument zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="f6ba1-107">Daher muss das Dokument aus dem Datenspeicher abgerufen werden, vor der Auswertung der Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="f6ba1-108">Attribut-Auswertung erfolgt, bevor Sie die Datenbindung und Ausführung der Seitenhandler für die oder Aktion, die das Dokument geladen wird.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="f6ba1-109">Aus diesen Gründen deklarative Autorisierung mit einem `[Authorize]` Attribut wird nicht ausreichen.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="f6ba1-110">Stattdessen können Sie eine benutzerdefinierte Autorisierung-Methode aufrufen&mdash;einen Stil, der als imperative Autorisierung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="f6ba1-111">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f6ba1-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f6ba1-112">[Erstellen einer ASP.NET Core-app mit Benutzerdaten, die durch Autorisierung geschützt](xref:security/authorization/secure-data) enthält eine Beispiel-app, die ressourcenbasierte Autorisierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="f6ba1-113">Verwenden Sie imperative Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f6ba1-113">Use imperative authorization</span></span>

<span data-ttu-id="f6ba1-114">Autorisierung wird als implementiert eine [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service und registriert ist, in der Sammlung von Diensten in der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="f6ba1-115">Der Dienst über zur Verfügung gestellt wird [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) Seitenhandler oder Aktionen.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="f6ba1-116">`IAuthorizationService` verfügt über zwei `AuthorizeAsync` Überladungen der Methode: ein akzeptieren der Ressource und den Richtliniennamen und die andere akzeptiert die Ressource und eine Liste der Anforderungen zum Auswerten.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6ba1-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6ba1-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="f6ba1-119">Im folgenden Beispiel wird die Ressource, die gesichert werden geladen, in ein benutzerdefiniertes `Document` Objekt.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="f6ba1-120">Ein `AuthorizeAsync` Überladung wird aufgerufen, um zu bestimmen, ob der aktuelle Benutzer zulässig ist, um das angegebene Dokument zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="f6ba1-121">Die Entscheidung wird eine benutzerdefinierte Autorisierungsrichtlinie von "EditPolicy" umgerechnet.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="f6ba1-122">Finden Sie unter [benutzerdefinierte richtlinienbasierte Autorisierung](xref:security/authorization/policies) Weitere Informationen zum Erstellen von Autorisierungsrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="f6ba1-123">Der folgende Code-Beispielen wird davon ausgegangen, Authentifizierung ausgeführt wurde und legen die `User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6ba1-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6ba1-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="f6ba1-126">Schreiben Sie einen Handler ressourcenbasierte</span><span class="sxs-lookup"><span data-stu-id="f6ba1-126">Write a resource-based handler</span></span>

<span data-ttu-id="f6ba1-127">Schreiben einen Handler für die ressourcenbasierte Autorisierung nicht viel anders als ist [einen Handler für die einfache Anforderungen schreiben](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="f6ba1-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="f6ba1-128">Erstellen Sie eine benutzerdefinierte Anforderung-Klasse, und implementieren Sie eine Anforderung Handler-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="f6ba1-129">Die Handlerklasse gibt die Anforderung und den Ressourcentyp an.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="f6ba1-130">Z. B. einen Ereignishandler mit einem `SameAuthorRequirement` Anforderung und eine `Document` Ressource sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f6ba1-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6ba1-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6ba1-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="f6ba1-133">Registrieren Sie die Anforderungen und die Ereignishandler in der `Startup.ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="f6ba1-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="f6ba1-134">Betriebsanforderungen</span><span class="sxs-lookup"><span data-stu-id="f6ba1-134">Operational requirements</span></span>

<span data-ttu-id="f6ba1-135">Wenn Sie Entscheidungen basierend auf den Ergebnissen des CRUD (Create, Read, Update, Delete)-Operationen vornehmen, verwenden Sie die [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Helper-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="f6ba1-136">Diese Klasse ermöglicht Ihnen, einen einzelnen Handler, anstatt eine einzelne Klasse für jeden Vorgangstyp zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="f6ba1-137">Geben Sie einige Vorgangsnamen, um es zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="f6ba1-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="f6ba1-138">Der Handler wird wie folgt implementiert, mit einem `OperationAuthorizationRequirement` Anforderung und eine `Document` Ressource:</span><span class="sxs-lookup"><span data-stu-id="f6ba1-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6ba1-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6ba1-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="f6ba1-141">Der vorherige Handler überprüft den Vorgang mit der Ressource, die Identität des Benutzers und der Anforderung des `Name` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="f6ba1-142">Um einen Handler für die operative Ressource aufzurufen, geben Sie den Vorgang aufrufen `AuthorizeAsync` im Seitenhandler für die oder Aktion.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="f6ba1-143">Im folgenden Beispiel wird bestimmt, ob der authentifizierte Benutzer zum Anzeigen des angegebenen Dokuments zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="f6ba1-144">Der folgende Code-Beispielen wird davon ausgegangen, Authentifizierung ausgeführt wurde und legen die `User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6ba1-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="f6ba1-146">Bei einer erfolgreichen Autorisierung wird die Seite zum Anzeigen des Dokuments zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="f6ba1-147">Wenn die Autorisierung fehlschlägt, aber der Benutzer wird authentifiziert, Zurückgeben von `ForbidResult` informiert alle authentifizierungsmiddleware, die Fehler bei der Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="f6ba1-148">Ein `ChallengeResult` wird zurückgegeben, wenn die Authentifizierung ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="f6ba1-149">Für die interaktive Browser-Clients kann es für den Benutzer zu einer Anmeldeseite umleiten geeignet sein.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6ba1-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6ba1-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="f6ba1-151">Die Ansicht für das Dokument wird zurückgegeben, bei einer erfolgreichen Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="f6ba1-152">Wenn die Autorisierung fehlschlägt, Rückgabe `ChallengeResult` informiert Sie Middleware für die Authentifizierung, dass Fehler bei der Autorisierung, und die Middleware kann die entsprechende Antwort.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="f6ba1-153">Eine entsprechende Antwort konnte einen Statuscode 401 oder 403 zurück.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="f6ba1-154">Für die interaktive Browser-Clients kann dies bedeuten, Weiterleiten des Benutzers zu einer Anmeldeseite um.</span><span class="sxs-lookup"><span data-stu-id="f6ba1-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
