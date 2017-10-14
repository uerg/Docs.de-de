---
title: Einfache Autorisierung
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 91f402b1cbf73e212418d197a8a7230ce22e9e1d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="089cc-103">Einfache Autorisierung</span><span class="sxs-lookup"><span data-stu-id="089cc-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="089cc-104">Autorisierung in MVC wird gesteuert durch die `AuthorizeAttribute` Attribut und seine verschiedenen Parameter.</span><span class="sxs-lookup"><span data-stu-id="089cc-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="089cc-105">Bei der einfachsten Anwenden der `AuthorizeAttribute` -Attribut auf einen Controller oder die Aktion der Zugriff auf den Controller eingeschränkt oder eine Aktion für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="089cc-105">At its simplest applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="089cc-106">Z. B. der folgende Code schränkt den Zugriff auf die `AccountController` für alle authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="089cc-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="089cc-107">Wenn Sie Autorisierung einfach auf den Controller, anstatt eine Aktion anwenden möchten gelten die `AuthorizeAttribute` -Attribut auf die Aktion selbst</span><span class="sxs-lookup"><span data-stu-id="089cc-107">If you want to apply authorization to an action rather than the controller simply apply the `AuthorizeAttribute` attribute to the action itself;</span></span>

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

<span data-ttu-id="089cc-108">Nur authentifizierte Benutzer können jetzt die Abmelde-Funktion zugreifen.</span><span class="sxs-lookup"><span data-stu-id="089cc-108">Now only authenticated users can access the logout function.</span></span>

<span data-ttu-id="089cc-109">Sie können auch die `AllowAnonymousAttribute` Attribut den Zugriff nicht authentifizierte Benutzer für die einzelnen Aktionen; z. B.</span><span class="sxs-lookup"><span data-stu-id="089cc-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions; for example</span></span>

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

<span data-ttu-id="089cc-110">Dies würde nur authentifizierte Benutzern ermöglichen, die `AccountController`, mit Ausnahme der `Login` Aktion, die jeder Benutzer, unabhängig vom jeweiligen Status authentifiziert oder nicht authentifizierte / anonym zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="089cc-110">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="089cc-111">`[AllowAnonymous]`umgeht alle Autorisierung-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="089cc-111">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="089cc-112">Wenn Sie kombinieren anwenden `[AllowAnonymous]` und ein beliebiger `[Authorize]` Attribut anschließend die Authorize-Attribute werden immer ignoriert.</span><span class="sxs-lookup"><span data-stu-id="089cc-112">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="089cc-113">Angenommen, Sie gelten `[AllowAnonymous]` auf dem Controller Ebene eine `[Authorize]` Attribute auf demselben Controller oder auf eine beliebige Aktion darin werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="089cc-113">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
