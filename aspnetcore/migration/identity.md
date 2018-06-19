---
title: Migrieren von Authentifizierung und Identität zu ASP.NET Core
author: ardalis
description: Erfahren Sie, wie Authentifizierung und Identität von eines ASP.NET MVC-Projekts zu ASP.NET Core MVC-Projekt zu migrieren.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851442"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="60ef3-103">Migrieren von Authentifizierung und Identität zu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60ef3-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="60ef3-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="60ef3-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="60ef3-105">In der vorherigen Artikel wir [Konfiguration eines ASP.NET MVC-Projekts zu ASP.NET Core MVC migriert](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="60ef3-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="60ef3-106">In diesem Artikel wird die Registrierung, Anmeldung und Benutzer Verwaltungsfunktionen migriert werden.</span><span class="sxs-lookup"><span data-stu-id="60ef3-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="60ef3-107">Konfigurieren von Identität und Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="60ef3-107">Configure Identity and Membership</span></span>

<span data-ttu-id="60ef3-108">In ASP.NET MVC-Authentifizierung und Identität Funktionen werden konfiguriert mit ASP.NET Identity in *Startup.Auth.cs* und *IdentityConfig.cs*befindet sich im die *App_Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="60ef3-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="60ef3-109">Diese Funktionen werden in ASP.NET-MVC Core in konfiguriert *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="60ef3-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="60ef3-110">Installieren der `Microsoft.AspNetCore.Identity.EntityFrameworkCore` und `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="60ef3-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="60ef3-111">Öffnen Sie dann *Startup.cs* und aktualisieren Sie die `Startup.ConfigureServices` Methode zur Verwendung von Entity Framework und Identität Diensten:</span><span class="sxs-lookup"><span data-stu-id="60ef3-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

<span data-ttu-id="60ef3-112">Zu diesem Zeitpunkt stehen zwei Typen, die im obigen Code, die noch nicht wir noch aus dem ASP.NET MVC-Projekt migriert verwiesen: `ApplicationDbContext` und `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="60ef3-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="60ef3-113">Erstellen Sie ein neues *Modelle* Ordner, in dem ASP.NET Core Projekt, und fügen Sie zwei Klassen hinzu, die diese Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="60ef3-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="60ef3-114">Die Versionen dieser Klassen in ASP.NET MVC finden */Models/IdentityModels.cs*, aber es wird eine Datei pro Klasse im migrierten Projekt verwenden, da es sich bei, die mehr klar ist.</span><span class="sxs-lookup"><span data-stu-id="60ef3-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="60ef3-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="60ef3-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="60ef3-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="60ef3-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="60ef3-117">Core MVC Starter ASP.NET-Webprojekt nicht viel Anpassung von Benutzern, schließen oder die `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="60ef3-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="60ef3-118">Bei der Migration einer wirkliche app müssen Sie auch alle benutzerdefinierten Eigenschaften und Methoden der Benutzer Ihrer app zu migrieren und `DbContext` Klassen sowie andere Modellklassen, Ihre app verwendet.</span><span class="sxs-lookup"><span data-stu-id="60ef3-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="60ef3-119">Z. B. Wenn Ihr `DbContext` hat eine `DbSet<Album>`, müssen Sie zum Migrieren der `Album` Klasse.</span><span class="sxs-lookup"><span data-stu-id="60ef3-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="60ef3-120">Mit diesen Dateien direkt in die *Startup.cs* Datei aktualisieren kompilieren gestellt werden seine `using` Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="60ef3-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="60ef3-121">Dieser app ist jetzt bereit zur Unterstützung der Authentifizierung und Identitätsdienste.</span><span class="sxs-lookup"><span data-stu-id="60ef3-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="60ef3-122">Es muss nur diese Funktionen für Benutzer verfügbar gemacht haben.</span><span class="sxs-lookup"><span data-stu-id="60ef3-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="60ef3-123">Migrieren Sie die Logik für Registrierung und Anmeldung</span><span class="sxs-lookup"><span data-stu-id="60ef3-123">Migrate registration and login logic</span></span>

<span data-ttu-id="60ef3-124">Identitätsdienste für die app konfiguriert und den Datenzugriff mithilfe von Entity Framework und SQL Server konfiguriert können wir Unterstützung für die Registrierung und Anmeldung für die app hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="60ef3-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="60ef3-125">Bedenken Sie, dass [weiter oben in der Migrationsprozess](xref:migration/mvc#migrate-the-layout-file) wir einen Verweis auf auskommentiert *_LoginPartial* in *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="60ef3-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="60ef3-126">Jetzt ist es Zeit, um diesen Code zurückgeben, kommentieren Sie es, und fügen in den erforderlichen Controllern und Ansichten, um Anmeldung Funktionen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="60ef3-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="60ef3-127">Heben Sie die auskommentierung der `@Html.Partial` in Zeile *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60ef3-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="60ef3-128">Sie nun eine neue Razor-Ansicht mit der *_LoginPartial* auf die *Ansichten/freigegeben* Ordner:</span><span class="sxs-lookup"><span data-stu-id="60ef3-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="60ef3-129">Update *_LoginPartial.cshtml* durch den folgenden Code (ersetzen Sie seinen gesamten Inhalt):</span><span class="sxs-lookup"><span data-stu-id="60ef3-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="60ef3-130">An diesem Punkt sollten Sie die Website in Ihrem Browser aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="60ef3-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="60ef3-131">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="60ef3-131">Summary</span></span>

<span data-ttu-id="60ef3-132">ASP.NET Core führt Änderungen auf die ASP.NET Identity-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="60ef3-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="60ef3-133">In diesem Artikel wurde gezeigt, wie die Authentifizierung und Benutzer-Verwaltungsfunktionen von ASP.NET Identity zu ASP.NET Core migrieren.</span><span class="sxs-lookup"><span data-stu-id="60ef3-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
