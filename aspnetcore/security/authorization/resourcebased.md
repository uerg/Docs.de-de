---
title: Resource Based Autorisierung
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="f2b1b-103">Resource Based Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f2b1b-103">Resource Based Authorization</span></span>

<a name=security-authorization-resource-based></a>

<span data-ttu-id="f2b1b-104">Häufig Autorisierung richtet sich nach der Ressource, auf die zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="f2b1b-105">Beispielsweise kann ein Dokument Author-Eigenschaft aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-105">For example a document may have an author property.</span></span> <span data-ttu-id="f2b1b-106">Nur der Autor des Dokuments würden dürfen, zu aktualisieren, damit die Ressource aus dem Dokumentrepository geladen werden muss, bevor eine Autorisierung Auswertung vorgenommen werden kann.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="f2b1b-107">Dies kann mit einem Attribut autorisieren ausgeführt werden, als Attribut Auswertung vor dem Datenbindung und erfolgt bevor Sie Ihren eigenen Code zum Laden einer Ressource in eine Aktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="f2b1b-108">Anstelle von deklarativer Autorisierung, die Attribut-Methode, müssen wir imperative Autorisierung verwenden, wenn ein Entwickler eine Authorize-Funktion in ihrem eigenen Code aufruft.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="f2b1b-109">Autorisieren von innerhalb des Codes</span><span class="sxs-lookup"><span data-stu-id="f2b1b-109">Authorizing within your code</span></span>

<span data-ttu-id="f2b1b-110">Autorisierung wird als ein Dienst implementiert `IAuthorizationService`, in die Auflistung registriert und können über [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) für den Zugriff auf Domänencontroller.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="f2b1b-111">`IAuthorizationService`verfügt über zwei Methoden, einer, in dem Sie übergeben die Ressource und den Richtliniennamen und die andere, an dem Sie die Ressource und eine Liste von Anforderungen zum Auswerten des übergeben.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

<span data-ttu-id="f2b1b-112">Die Auslastung der Ressource in Ihre Aktion aufrufen, rufen Sie anschließend die `AuthorizeAsync` Überladung, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-112">To call the service load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="f2b1b-113">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f2b1b-113">For example</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="f2b1b-114">Schreiben Sie einen Handler ressourcenbasierter</span><span class="sxs-lookup"><span data-stu-id="f2b1b-114">Writing a resource based handler</span></span>

<span data-ttu-id="f2b1b-115">Schreiben einen Handler für die Autorisierung ressourcenbasierter unterscheidet sich nicht, die viel zu [schreiben einen Handler für einfache Anforderungen](policies.md#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="f2b1b-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="f2b1b-116">Sie eine Anforderung erstellen und implementieren Sie anschließend einen Handler für die Anforderung vor Anforderung sowie der Ressourcentyp angeben.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="f2b1b-117">Beispielsweise würde ein Ereignishandler, der eine dokumentressource akzeptiert, die möglicherweise wie folgt aussehen;</span><span class="sxs-lookup"><span data-stu-id="f2b1b-117">For example, a handler which might accept a Document resource would look as follows;</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="f2b1b-118">Vergessen Sie nicht, müssen Sie auch registrieren Sie den Handler in der `ConfigureServices` -Methode.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-118">Don't forget you also need to register your handler in the `ConfigureServices` method;</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="f2b1b-119">Prozessanforderungen</span><span class="sxs-lookup"><span data-stu-id="f2b1b-119">Operational Requirements</span></span>

<span data-ttu-id="f2b1b-120">Wenn Sie Ihre Entscheidungen basierend auf Vorgänge wie z. B. Lese-, Schreib-, Update und Delete ausführen möchten, können Sie die `OperationAuthorizationRequirement` -Klasse in der `Microsoft.AspNetCore.Authorization.Infrastructure` Namespace.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="f2b1b-121">Diese Klasse vorgefertigten Anforderung können Sie einen einzigen Handler besitzt eine parametrisierte Vorgangsnamen schreiben, anstatt einzelne Klassen für jeden Vorgang zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="f2b1b-122">Geben zum verwenden einige Vorgangsnamen:</span><span class="sxs-lookup"><span data-stu-id="f2b1b-122">To use it provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="f2b1b-123">Der Handler dann implementiert werden kann wie folgt mithilfe ein hypothetischen `Document` Klasse wie die Ressource;</span><span class="sxs-lookup"><span data-stu-id="f2b1b-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource;</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="f2b1b-124">Sehen Sie die Handler funktioniert auf `OperationAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="f2b1b-125">Der Code innerhalb der Handler muss die Name-Eigenschaft der angegebenen Anforderung berücksichtigt dauern, beim Bereitstellen der Bewertungen.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="f2b1b-126">Einen Handler operational Ressource aufrufen, Sie den Vorgang angeben, beim Aufrufen von müssen `AuthorizeAsync` in Ihre Aktion.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="f2b1b-127">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f2b1b-127">For example</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="f2b1b-128">In diesem Beispiel wird überprüft, wenn der Benutzer zum Ausführen des Vorgangs "Lesen" für den aktuellen kann `document` Instanz.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="f2b1b-129">Nach erfolgreicher Autorisierung wird die Ansicht für das Dokument zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="f2b1b-130">Wenn die Autorisierung fehlschlägt zurückgeben `ChallengeResult` informiert Middleware-Autorisierung fehlgeschlagen ist und die Middleware dauert die entsprechende Antwort, z. B. einen Statuscode 401 oder 403 zurückgeben oder Weiterleiten des Benutzers an einer Anmeldeseite für Authentifizierung Interaktive Browser-Clients.</span><span class="sxs-lookup"><span data-stu-id="f2b1b-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
