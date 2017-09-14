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
ms.openlocfilehash: 7f7df52bf51a81558818836450997281a21b5839
ms.sourcegitcommit: f303a457644ed034a49aa89edecb4e79d9028cb1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="resource-based-authorization"></a>Resource Based Autorisierung

<a name=security-authorization-resource-based></a>

Häufig Autorisierung richtet sich nach der Ressource, auf die zugegriffen wird. Beispielsweise kann ein Dokument Author-Eigenschaft aufweisen. Nur der Autor des Dokuments würden dürfen, zu aktualisieren, damit die Ressource aus dem Dokumentrepository geladen werden muss, bevor eine Autorisierung Auswertung vorgenommen werden kann. Dies kann mit einem Attribut autorisieren ausgeführt werden, als Attribut Auswertung vor dem Datenbindung und erfolgt bevor Sie Ihren eigenen Code zum Laden einer Ressource in eine Aktion ausgeführt wird. Anstelle von deklarativer Autorisierung, die Attribut-Methode, müssen wir imperative Autorisierung verwenden, wenn ein Entwickler eine Authorize-Funktion in ihrem eigenen Code aufruft.

## <a name="authorizing-within-your-code"></a>Autorisieren von innerhalb des Codes

Autorisierung wird als ein Dienst implementiert `IAuthorizationService`, in die Auflistung registriert und können über [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) für den Zugriff auf Domänencontroller.

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

`IAuthorizationService`verfügt über zwei Methoden, einer, in dem Sie übergeben die Ressource und den Richtliniennamen und die andere, an dem Sie die Ressource und eine Liste von Anforderungen zum Auswerten des übergeben.

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

Um den Dienst aufzurufen, laden Sie die Ressource in Ihre Aktion rufen Sie anschließend die `AuthorizeAsync` Überladung, die Sie benötigen. Zum Beispiel:

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

## <a name="writing-a-resource-based-handler"></a>Schreiben Sie einen Handler ressourcenbasierter

Schreiben einen Handler für die Autorisierung ressourcenbasierter unterscheidet sich nicht, die viel zu [schreiben einen Handler für einfache Anforderungen](policies.md#security-authorization-policies-based-authorization-handler). Sie eine Anforderung erstellen und implementieren Sie anschließend einen Handler für die Anforderung vor Anforderung sowie der Ressourcentyp angeben. Beispielsweise würde ein Ereignishandler, der eine dokumentressource akzeptiert, die möglicherweise wie folgt aussehen:

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

Vergessen Sie nicht, müssen Sie auch registrieren Sie den Handler in der `ConfigureServices` Methode:

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>Prozessanforderungen

Wenn Sie Ihre Entscheidungen basierend auf Vorgänge wie z. B. Lese-, Schreib-, Update und Delete ausführen möchten, können Sie die `OperationAuthorizationRequirement` -Klasse in der `Microsoft.AspNetCore.Authorization.Infrastructure` Namespace. Diese Klasse vorgefertigten Anforderung können Sie einen einzigen Handler besitzt eine parametrisierte Vorgangsnamen schreiben, anstatt einzelne Klassen für jeden Vorgang zu erstellen. Geben Sie einige Vorgangsnamen, um es verwenden zu um können:

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

Der Handler dann implementiert werden kann wie folgt mithilfe ein hypothetischen `Document` Klasse wie die Ressource:

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

Sehen Sie die Handler funktioniert auf `OperationAuthorizationRequirement`. Der Code innerhalb der Handler muss die Name-Eigenschaft der angegebenen Anforderung berücksichtigt dauern, beim Bereitstellen der Bewertungen.

Einen Handler operational Ressource aufrufen, Sie den Vorgang angeben, beim Aufrufen von müssen `AuthorizeAsync` in Ihre Aktion. Zum Beispiel:

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

In diesem Beispiel wird überprüft, wenn der Benutzer zum Ausführen des Vorgangs "Lesen" für den aktuellen kann `document` Instanz. Nach erfolgreicher Autorisierung wird die Ansicht für das Dokument zurückgegeben werden. Wenn die Autorisierung fehlschlägt zurückgeben `ChallengeResult` informiert Middleware-Autorisierung fehlgeschlagen ist und die Middleware dauert die entsprechende Antwort, z. B. einen Statuscode 401 oder 403 zurückgeben oder Weiterleiten des Benutzers an einer Anmeldeseite für Authentifizierung Interaktive Browser-Clients.
