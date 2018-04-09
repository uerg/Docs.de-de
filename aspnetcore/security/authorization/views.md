---
title: View-basierte Autorisierung in ASP.NET Core MVC
author: rick-anderson
description: Dieses Dokument veranschaulicht, wie zum Einfügen und Nutzen von dem Prüfdienst innerhalb einer ASP.NET Core Razor-Ansicht.
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="98109-103">View-basierte Autorisierung in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="98109-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="98109-104">Ein Entwickler möchte häufig einblenden, ausblenden oder ändern Sie andernfalls eine Benutzeroberfläche, die basierend auf der Identität des aktuellen Benutzers.</span><span class="sxs-lookup"><span data-stu-id="98109-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="98109-105">Sie können dem Prüfdienst in MVC-Ansichten über zugreifen [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="98109-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="98109-106">Um den Autorisierungsdienst in einer Razor-Ansicht einfügen möchten, verwenden Sie die `@inject` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="98109-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="98109-107">Wenn Sie dem Prüfdienst in jeder Ansicht möchten, fügen die `@inject` -Direktive in der *_ViewImports.cshtml* Datei von der *Ansichten* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="98109-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="98109-108">Weitere Informationen finden Sie unter [Dependency Injection in Ansichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="98109-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="98109-109">Verwenden Sie den eingefügten Autorisierungsdienst aufzurufenden `AuthorizeAsync` auf genau die gleiche Weise Sie überprüfen, während der würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="98109-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="98109-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="98109-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="98109-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="98109-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="98109-112">In einigen Fällen wird die Ressource die Ansichtsmodell sein.</span><span class="sxs-lookup"><span data-stu-id="98109-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="98109-113">Aufrufen `AuthorizeAsync` auf genau die gleiche Weise Sie überprüfen, während der würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="98109-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="98109-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="98109-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="98109-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="98109-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="98109-116">Im vorangehenden Code wird das Modell als eine Ressource, die die richtlinienauswertung ausführen sollten übergeben, zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="98109-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="98109-117">Verlassen Sie sich nicht auf die Umschaltfläche Sichtbarkeit Ihrer app von Elementen der Benutzeroberfläche, wie die einzige autorisierungsprüfung.</span><span class="sxs-lookup"><span data-stu-id="98109-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="98109-118">Ausblenden eines Benutzeroberflächenelements möglicherweise nicht vollständig Zugriff auf seine zugeordneten Controlleraktion verhindern.</span><span class="sxs-lookup"><span data-stu-id="98109-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="98109-119">Betrachten Sie beispielsweise die Schaltfläche im vorherigen Codeausschnitt.</span><span class="sxs-lookup"><span data-stu-id="98109-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="98109-120">Benutzer kann Aufrufen der `Edit` Aktionsmethode, wenn der relative Ressource ist die URL ist */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="98109-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="98109-121">Aus diesem Grund die `Edit` Aktionsmethode sollte eine eigene autorisierungsprüfung ausführen.</span><span class="sxs-lookup"><span data-stu-id="98109-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
