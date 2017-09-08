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
ms.openlocfilehash: 41682244cb9faa9a109661c09b6d7e67bca72534
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-authentication-and-identity"></a>Migrieren von Authentifizierung und Identität

<a name=migration-identity></a>

Durch [Steve Smith](http://ardalis.com)

In der vorherigen Artikel wir [Konfiguration eines ASP.NET MVC-Projekts zu ASP.NET Core MVC migriert](configuration.md). In diesem Artikel wird die Registrierung, Anmeldung und Benutzer Verwaltungsfunktionen migriert werden.

## <a name="configure-identity-and-membership"></a>Konfigurieren von Identität und Mitgliedschaft

In ASP.NET MVC werden Authentifizierung und Identität Funktionen mithilfe von ASP.NET Identity in Startup.Auth.cs und IdentityConfig.cs, im Ordner "App_Start" konfiguriert. Diese Funktionen werden in ASP.NET-MVC Core in konfiguriert *Startup.cs*.

Installieren der `Microsoft.AspNetCore.Identity.EntityFrameworkCore` und `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Pakete.

Öffnen Sie dann Startup.cs und Aktualisieren der `ConfigureServices()` Methode zur Verwendung von Entity Framework und Identität Diensten:

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

Zu diesem Zeitpunkt stehen zwei Typen, die im obigen Code, die noch nicht wir noch aus dem ASP.NET MVC-Projekt migriert verwiesen: `ApplicationDbContext` und `ApplicationUser`. Erstellen Sie ein neues *Modelle* Ordner, in dem ASP.NET Core Projekt, und fügen Sie zwei Klassen hinzu, die diese Typen entspricht. Die Versionen dieser Klassen in ASP.NET MVC finden `/Models/IdentityModels.cs`, aber es wird eine Datei pro Klasse im migrierten Projekt verwenden, da es sich bei, die mehr klar ist.

ApplicationUser.cs:

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

ApplicationDbContext.cs:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

    protected override void OnConfiguring(DbContextOptions options)
    {
      options.UseSqlServer();
    }
  }
}
```

Das ASP.NET Core MVC Starter-Webprojekt ist nicht viel Anpassung der Benutzer oder die ApplicationDbContext enthalten. Bei der Migration von einer realen Anwendung außerdem müssen Sie zum Migrieren aller benutzerdefinierten Eigenschaften und Methoden von Ihrer Anwendung Benutzer und DbContext-Klassen sowie andere Modellklassen, die Ihre Anwendung nutzt (beispielsweise, wenn Ihre ' DbContext ' ein ' DbSet 'hat<Album>, müssen Sie natürlich migrieren die Album-Klasse).

Mit diesen Dateien vorhanden, kann die Startup.cs-Datei zu kompilieren, indem Sie aktualisieren, die mithilfe vorgenommen werden Anweisungen:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Unsere Anwendung ist jetzt bereit zur Unterstützung der Authentifizierung und Identitätsdienste – es muss lediglich diese Funktionen für Benutzer verfügbar gemacht haben.

## <a name="migrate-registration-and-login-logic"></a>Registrierung und Anmeldung Logik migrieren

Identitätsdienste konfiguriert für die Anwendung und den Datenzugriff mithilfe von Entity Framework und SQL Server konfiguriert sind wir jetzt bereit, die Unterstützung für die Registrierung und Anmeldung für die Anwendung hinzufügen. Bedenken Sie, dass [weiter oben in der Migrationsprozess](mvc.md#migrate-layout-file) wir einen Verweis auf _LoginPartial in _Layout.cshtml auskommentiert. Jetzt ist es Zeit, um diesen Code zurückgeben, kommentieren Sie es, und fügen in den erforderlichen Controllern und Ansichten, um Anmeldung Funktionen zu unterstützen.

Aktualisieren Sie _Layout.cshtml; Heben Sie die auskommentierung der @Html.Partial Zeile:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Fügen Sie nun eine neue MVC-Ansichtsseite _LoginPartial aufgerufen, um die Ansichten/freigegeben Ordner hinzu:

Aktualisieren von _LoginPartial.cshtml durch den folgenden Code (ersetzen Sie seinen gesamten Inhalt):

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

An diesem Punkt sollten Sie die Website in Ihrem Browser aktualisieren können.

## <a name="summary"></a>Zusammenfassung

ASP.NET Core führt Änderungen auf die ASP.NET Identity-Funktionen. In diesem Artikel wurde gezeigt, wie die Authentifizierung und Benutzer-Verwaltungsfunktionen von einer ASP.NET Identity zu ASP.NET Core migrieren.
