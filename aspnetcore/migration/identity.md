---
title: "Migrieren von Authentifizierung und Identität"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: c86e9b98bcb43b383ac1077fe2749d0dfcd7392a
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2017
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="73ba0-103">Migrieren von Authentifizierung und Identität</span><span class="sxs-lookup"><span data-stu-id="73ba0-103">Migrating Authentication and Identity</span></span>

<a name=migration-identity></a>

<span data-ttu-id="73ba0-104">Durch [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="73ba0-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="73ba0-105">In der vorherigen Artikel wir [Konfiguration eines ASP.NET MVC-Projekts zu ASP.NET Core MVC migriert](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="73ba0-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="73ba0-106">In diesem Artikel wird die Registrierung, Anmeldung und Benutzer Verwaltungsfunktionen migriert werden.</span><span class="sxs-lookup"><span data-stu-id="73ba0-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="73ba0-107">Konfigurieren von Identität und Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="73ba0-107">Configure Identity and Membership</span></span>

<span data-ttu-id="73ba0-108">In ASP.NET MVC werden Authentifizierung und Identität Funktionen mithilfe von ASP.NET Identity in Startup.Auth.cs und IdentityConfig.cs, im Ordner "App_Start" konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="73ba0-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="73ba0-109">Diese Funktionen werden in ASP.NET-MVC Core in konfiguriert *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="73ba0-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="73ba0-110">Installieren der `Microsoft.AspNetCore.Identity.EntityFrameworkCore` und `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="73ba0-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="73ba0-111">Öffnen Sie dann Startup.cs und Aktualisieren der `ConfigureServices()` Methode zur Verwendung von Entity Framework und Identität Diensten:</span><span class="sxs-lookup"><span data-stu-id="73ba0-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="73ba0-112">Zu diesem Zeitpunkt stehen zwei Typen, die im obigen Code, die noch nicht wir noch aus dem ASP.NET MVC-Projekt migriert verwiesen: `ApplicationDbContext` und `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="73ba0-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="73ba0-113">Erstellen Sie ein neues *Modelle* Ordner, in dem ASP.NET Core Projekt, und fügen Sie zwei Klassen hinzu, die diese Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="73ba0-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="73ba0-114">Die Versionen dieser Klassen in ASP.NET MVC finden `/Models/IdentityModels.cs`, aber es wird eine Datei pro Klasse im migrierten Projekt verwenden, da es sich bei, die mehr klar ist.</span><span class="sxs-lookup"><span data-stu-id="73ba0-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="73ba0-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="73ba0-115">ApplicationUser.cs:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="73ba0-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="73ba0-116">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="73ba0-117">Das ASP.NET Core MVC Starter-Webprojekt ist nicht viel Anpassung der Benutzer oder die ApplicationDbContext enthalten.</span><span class="sxs-lookup"><span data-stu-id="73ba0-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="73ba0-118">Bei der Migration von einer realen Anwendung außerdem müssen Sie zum Migrieren aller benutzerdefinierten Eigenschaften und Methoden von Ihrer Anwendung Benutzer und DbContext-Klassen sowie andere Modellklassen, die Ihre Anwendung nutzt (beispielsweise, wenn Ihre ' DbContext ' ein ' DbSet 'hat<Album>, müssen Sie natürlich migrieren die Album-Klasse).</span><span class="sxs-lookup"><span data-stu-id="73ba0-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="73ba0-119">Mit diesen Dateien vorhanden, kann die Startup.cs-Datei zu kompilieren, indem Sie aktualisieren, die mithilfe vorgenommen werden Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="73ba0-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="73ba0-120">Unsere Anwendung ist jetzt bereit zur Unterstützung der Authentifizierung und Identitätsdienste – es muss lediglich diese Funktionen für Benutzer verfügbar gemacht haben.</span><span class="sxs-lookup"><span data-stu-id="73ba0-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="73ba0-121">Registrierung und Anmeldung Logik migrieren</span><span class="sxs-lookup"><span data-stu-id="73ba0-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="73ba0-122">Identitätsdienste konfiguriert für die Anwendung und den Datenzugriff mithilfe von Entity Framework und SQL Server konfiguriert sind wir jetzt bereit, die Unterstützung für die Registrierung und Anmeldung für die Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="73ba0-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="73ba0-123">Bedenken Sie, dass [weiter oben in der Migrationsprozess](mvc.md#migrate-layout-file) wir einen Verweis auf _LoginPartial in _Layout.cshtml auskommentiert.</span><span class="sxs-lookup"><span data-stu-id="73ba0-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="73ba0-124">Jetzt ist es Zeit, um diesen Code zurückgeben, kommentieren Sie es, und fügen in den erforderlichen Controllern und Ansichten, um Anmeldung Funktionen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="73ba0-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="73ba0-125">Aktualisieren Sie _Layout.cshtml; Heben Sie die auskommentierung der @Html.Partial Zeile:</span><span class="sxs-lookup"><span data-stu-id="73ba0-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="73ba0-126">Fügen Sie nun eine neue MVC-Ansichtsseite _LoginPartial aufgerufen, um die Ansichten/freigegeben Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="73ba0-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="73ba0-127">Aktualisieren von _LoginPartial.cshtml durch den folgenden Code (ersetzen Sie seinen gesamten Inhalt):</span><span class="sxs-lookup"><span data-stu-id="73ba0-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

<span data-ttu-id="73ba0-128">An diesem Punkt sollten Sie die Website in Ihrem Browser aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="73ba0-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="73ba0-129">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="73ba0-129">Summary</span></span>

<span data-ttu-id="73ba0-130">ASP.NET Core führt Änderungen auf die ASP.NET Identity-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="73ba0-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="73ba0-131">In diesem Artikel wurde gezeigt, wie die Authentifizierung und Benutzer-Verwaltungsfunktionen von einer ASP.NET Identity zu ASP.NET Core migrieren.</span><span class="sxs-lookup"><span data-stu-id="73ba0-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
