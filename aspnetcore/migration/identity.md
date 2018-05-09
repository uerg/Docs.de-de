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
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrieren von Authentifizierung und Identität zu ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

In der vorherigen Artikel wir [Konfiguration eines ASP.NET MVC-Projekts zu ASP.NET Core MVC migriert](xref:migration/configuration). In diesem Artikel wird die Registrierung, Anmeldung und Benutzer Verwaltungsfunktionen migriert werden.

## <a name="configure-identity-and-membership"></a>Konfigurieren von Identität und Mitgliedschaft

In ASP.NET MVC-Authentifizierung und Identität Funktionen werden konfiguriert mit ASP.NET Identity in *Startup.Auth.cs* und *IdentityConfig.cs*befindet sich im die *App_Start* Ordner. Diese Funktionen werden in ASP.NET-MVC Core in konfiguriert *Startup.cs*.

Installieren der `Microsoft.AspNetCore.Identity.EntityFrameworkCore` und `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Pakete.

Öffnen Sie dann *Startup.cs* und aktualisieren Sie die `Startup.ConfigureServices` Methode zur Verwendung von Entity Framework und Identität Diensten:

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

Zu diesem Zeitpunkt stehen zwei Typen, die im obigen Code, die noch nicht wir noch aus dem ASP.NET MVC-Projekt migriert verwiesen: `ApplicationDbContext` und `ApplicationUser`. Erstellen Sie ein neues *Modelle* Ordner, in dem ASP.NET Core Projekt, und fügen Sie zwei Klassen hinzu, die diese Typen entspricht. Die Versionen dieser Klassen in ASP.NET MVC finden */Models/IdentityModels.cs*, aber es wird eine Datei pro Klasse im migrierten Projekt verwenden, da es sich bei, die mehr klar ist.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

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

Core MVC Starter ASP.NET-Webprojekt nicht viel Anpassung von Benutzern, schließen oder die `ApplicationDbContext`. Bei der Migration einer wirkliche app müssen Sie auch alle benutzerdefinierten Eigenschaften und Methoden der Benutzer Ihrer app zu migrieren und `DbContext` Klassen sowie andere Modellklassen, Ihre app verwendet. Z. B. Wenn Ihr `DbContext` hat eine `DbSet<Album>`, müssen Sie zum Migrieren der `Album` Klasse.

Mit diesen Dateien direkt in die *Startup.cs* Datei aktualisieren kompilieren gestellt werden seine `using` Anweisungen:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Dieser app ist jetzt bereit zur Unterstützung der Authentifizierung und Identitätsdienste. Es muss nur diese Funktionen für Benutzer verfügbar gemacht haben.

## <a name="migrate-registration-and-login-logic"></a>Migrieren Sie die Logik für Registrierung und Anmeldung

Identitätsdienste für die app konfiguriert und den Datenzugriff mithilfe von Entity Framework und SQL Server konfiguriert können wir Unterstützung für die Registrierung und Anmeldung für die app hinzufügen. Bedenken Sie, dass [weiter oben in der Migrationsprozess](xref:migration/mvc#migrate-the-layout-file) wir einen Verweis auf auskommentiert *_LoginPartial* in *_Layout.cshtml*. Jetzt ist es Zeit, um diesen Code zurückgeben, kommentieren Sie es, und fügen in den erforderlichen Controllern und Ansichten, um Anmeldung Funktionen zu unterstützen.

Heben Sie die auskommentierung der `@Html.Partial` in Zeile *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Sie nun eine neue Razor-Ansicht mit der *_LoginPartial* auf die *Ansichten/freigegeben* Ordner:

Update *_LoginPartial.cshtml* durch den folgenden Code (ersetzen Sie seinen gesamten Inhalt):

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

An diesem Punkt sollten Sie die Website in Ihrem Browser aktualisieren können.

## <a name="summary"></a>Zusammenfassung

ASP.NET Core führt Änderungen auf die ASP.NET Identity-Funktionen. In diesem Artikel wurde gezeigt, wie die Authentifizierung und Benutzer-Verwaltungsfunktionen von ASP.NET Identity zu ASP.NET Core migrieren.
