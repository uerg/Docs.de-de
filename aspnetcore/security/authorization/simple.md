---
title: Einfache Autorisierung
author: rick-anderson
description: "Dieses Dokument erläutert, wie die Authorize-Attribut verwenden, um Zugriff zu ASP.NET Core-Controllern und Aktionen."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f1d5671785da815f2f4fcf5bef1352f4c9e62877
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="33739-103">Einfache Autorisierung</span><span class="sxs-lookup"><span data-stu-id="33739-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="33739-104">Autorisierung in MVC wird gesteuert durch die `AuthorizeAttribute` Attribut und seine verschiedenen Parameter.</span><span class="sxs-lookup"><span data-stu-id="33739-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="33739-105">Am einfachsten, Anwenden der `AuthorizeAttribute` -Attribut auf einen Controller oder die Aktion der Zugriff auf den Controller eingeschränkt oder eine Aktion für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="33739-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="33739-106">Z. B. der folgende Code schränkt den Zugriff auf die `AccountController` für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="33739-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="33739-107">Wenn Sie Autorisierung auf dem Controller, anstatt eine Aktion anwenden möchten, wenden Sie die `AuthorizeAttribute` -Attribut auf die Aktion selbst:</span><span class="sxs-lookup"><span data-stu-id="33739-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="33739-108">Jetzt nur authentifizierte Benutzer zugreifen können die `Logout` Funktion.</span><span class="sxs-lookup"><span data-stu-id="33739-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="33739-109">Sie können auch die `AllowAnonymousAttribute` Attribut, um den Zugriff durch nicht authentifizierte Benutzer auf einzelne Aktionen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="33739-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="33739-110">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="33739-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="33739-111">Dies würde nur authentifizierte Benutzern ermöglichen, die `AccountController`, mit Ausnahme der `Login` Aktion, die jeder Benutzer, unabhängig vom jeweiligen Status authentifiziert oder nicht authentifizierte / anonym zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="33739-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="33739-112">`[AllowAnonymous]`umgeht alle Autorisierung-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="33739-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="33739-113">Wenn Sie kombinieren anwenden `[AllowAnonymous]` und ein beliebiger `[Authorize]` Attribut anschließend die Authorize-Attribute werden immer ignoriert.</span><span class="sxs-lookup"><span data-stu-id="33739-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="33739-114">Angenommen, Sie gelten `[AllowAnonymous]` auf dem Controller Ebene eine `[Authorize]` Attribute auf demselben Controller oder auf eine beliebige Aktion darin werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="33739-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
