---
title: Rollenbasierte Autorisierung in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie ASP.NET Core-Controller und Aktion Zugriff einschränken, indem Sie die Rollen an das Authorize-Attribut übergeben.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356674"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="b0934-103">Rollenbasierte Autorisierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0934-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="b0934-104">Wenn eine Identität erstellt wird möglicherweise eine oder mehrere Rollen angehören.</span><span class="sxs-lookup"><span data-stu-id="b0934-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="b0934-105">Tracy kann z. B. den Administrator- und Rollen gehören, obwohl Scott nur für die Benutzerrolle angehören kann.</span><span class="sxs-lookup"><span data-stu-id="b0934-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="b0934-106">Wie diese Rollen erstellt und verwaltet werden, hängt von der Sicherungsspeicher der Autorisierung ab.</span><span class="sxs-lookup"><span data-stu-id="b0934-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="b0934-107">Rollen werden verfügbar gemacht, für den Entwickler über die ["IsInRole"](/dotnet/api/system.security.principal.genericprincipal.isinrole) Methode für die ["ClaimsPrincipal"](/dotnet/api/system.security.claims.claimsprincipal) Klasse.</span><span class="sxs-lookup"><span data-stu-id="b0934-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> <span data-ttu-id="b0934-108">Die Informationen in diesem Artikel können **nicht** auf Razor-Seiten angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0934-108">This topic does **not** apply to Razor Pages.</span></span> <span data-ttu-id="b0934-109">Razor-Seiten unterstützt [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) und [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span><span class="sxs-lookup"><span data-stu-id="b0934-109">Razor Pages supports [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span></span> <span data-ttu-id="b0934-110">Weitere Informationen finden Sie unter [Filtermethoden für Razor-Seiten](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="b0934-110">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

::: moniker-end

## <a name="adding-role-checks"></a><span data-ttu-id="b0934-111">Hinzufügen von rollenüberprüfungen</span><span class="sxs-lookup"><span data-stu-id="b0934-111">Adding role checks</span></span>

<span data-ttu-id="b0934-112">Rollenbasierte autorisierungsprüfungen sind deklarativ&mdash;Entwickler bettet diese in ihren Code für einen Controller oder einer Aktion innerhalb eines Controllers angeben von Rollen, die der aktuelle Benutzer ein Mitglied sein muss, um die angeforderte Ressource zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b0934-112">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="b0934-113">Z. B. der folgende Code, schränkt den Zugriff auf alle Aktionen auf der `AdministrationController` für Benutzer sind, die ein Mitglied der `Administrator` Rolle:</span><span class="sxs-lookup"><span data-stu-id="b0934-113">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="b0934-114">Sie können mehrere Rollen als durch Trennzeichen getrennte Liste angeben:</span><span class="sxs-lookup"><span data-stu-id="b0934-114">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="b0934-115">Dieser Controller nur zugegriffen werden von Benutzern, die Mitglieder sind von der `HRManager` Rolle oder die `Finance` Rolle.</span><span class="sxs-lookup"><span data-stu-id="b0934-115">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="b0934-116">Wenn Sie mehrere Attribute anwenden, und klicken Sie dann ein Zugriff auf Benutzer ein Mitglied aller angegebenen Rollen sein muss; Das folgende Beispiel erfordert, dass ein Benutzer muss ein Mitglied sowohl der `PowerUser` und `ControlPanelUser` Rolle.</span><span class="sxs-lookup"><span data-stu-id="b0934-116">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="b0934-117">Sie können den Zugriff weiter einzuschränken, durch zusätzliche Rolle Authorization-Attribute auf der Aktionsebene anwenden:</span><span class="sxs-lookup"><span data-stu-id="b0934-117">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="b0934-118">In den vorherigen Code Snippet Membern der der `Administrator` Rolle oder die `PowerUser` Rolle kann auf den Controller zugreifen und die `SetTime` Aktion, aber nur Mitglieder der der `Administrator` Rolle zugreifen kann die `ShutDown` Aktion.</span><span class="sxs-lookup"><span data-stu-id="b0934-118">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="b0934-119">Sie können auch einen Controller Sperren jedoch anonyme, nicht authentifizierte Zugriff auf einzelne Aktionen zulassen.</span><span class="sxs-lookup"><span data-stu-id="b0934-119">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="b0934-120">Überprüfung der richtlinienbasierten-Funktion</span><span class="sxs-lookup"><span data-stu-id="b0934-120">Policy based role checks</span></span>

<span data-ttu-id="b0934-121">Anforderungen an die Rolle können auch mithilfe der neuen Richtlinie-Syntax wird registriert, in denen ein Entwickler eine Richtlinie beim Start als Teil der Dienstkonfiguration für die Autorisierung ausgedrückt werden.</span><span class="sxs-lookup"><span data-stu-id="b0934-121">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="b0934-122">Dieser Fehler tritt normalerweise in `ConfigureServices()` in Ihre *"Startup.cs"* Datei.</span><span class="sxs-lookup"><span data-stu-id="b0934-122">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="b0934-123">Richtlinien werden angewendet, mit der `Policy` Eigenschaft für die `AuthorizeAttribute` Attribut:</span><span class="sxs-lookup"><span data-stu-id="b0934-123">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="b0934-124">Wenn Sie eine Anforderung mehrere zulässige Rollen angeben möchten, und klicken Sie dann als Parameter zum Angeben der `RequireRole` Methode:</span><span class="sxs-lookup"><span data-stu-id="b0934-124">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="b0934-125">In diesem Beispiel autorisiert Benutzer Zugehörigkeit zu der `Administrator`, `PowerUser` oder `BackupAdministrator` Rollen.</span><span class="sxs-lookup"><span data-stu-id="b0934-125">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
