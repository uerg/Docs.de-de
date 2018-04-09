---
title: Einfache Autorisierung in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie die Authorize-Attribut verwenden, um Zugriff zu ASP.NET Core-Controllern und Aktionen.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="38f06-103">Einfache Autorisierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38f06-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="38f06-104">Autorisierung in MVC wird gesteuert durch die `AuthorizeAttribute` Attribut und seine verschiedenen Parameter.</span><span class="sxs-lookup"><span data-stu-id="38f06-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="38f06-105">Am einfachsten, Anwenden der `AuthorizeAttribute` -Attribut auf einen Controller oder die Aktion der Zugriff auf den Controller eingeschränkt oder eine Aktion für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="38f06-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="38f06-106">Z. B. der folgende Code schränkt den Zugriff auf die `AccountController` für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="38f06-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="38f06-107">Wenn Sie Autorisierung auf dem Controller, anstatt eine Aktion anwenden möchten, wenden Sie die `AuthorizeAttribute` -Attribut auf die Aktion selbst:</span><span class="sxs-lookup"><span data-stu-id="38f06-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="38f06-108">Jetzt nur authentifizierte Benutzer zugreifen können die `Logout` Funktion.</span><span class="sxs-lookup"><span data-stu-id="38f06-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="38f06-109">Sie können auch die `AllowAnonymous` Attribut, um den Zugriff durch nicht authentifizierte Benutzer auf einzelne Aktionen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="38f06-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="38f06-110">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38f06-110">For example:</span></span>

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

<span data-ttu-id="38f06-111">Dies würde nur authentifizierte Benutzern ermöglichen, die `AccountController`, mit Ausnahme der `Login` Aktion, die jeder Benutzer, unabhängig vom jeweiligen Status authentifiziert oder nicht authentifizierte / anonym zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="38f06-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="38f06-112">`[AllowAnonymous]` umgeht alle Autorisierung-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="38f06-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="38f06-113">Wenn Sie kombinieren anwenden `[AllowAnonymous]` und ein beliebiger `[Authorize]` Attribut anschließend die Authorize-Attribute werden immer ignoriert.</span><span class="sxs-lookup"><span data-stu-id="38f06-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="38f06-114">Angenommen, Sie gelten `[AllowAnonymous]` auf dem Controller Ebene eine `[Authorize]` Attribute auf demselben Controller oder auf eine beliebige Aktion darin werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="38f06-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
