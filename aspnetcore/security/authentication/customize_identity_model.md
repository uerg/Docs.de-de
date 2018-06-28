---
title: Anpassung der Identity-Modell
author: ajcvickers
description: Dieser Artikel beschreibt, wie Sie das zugrunde liegende Datenmodell von Entity Framework Core für ASP.NET Core Identity anpassen.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036893"
---
# <a name="identity-model-customization"></a>Anpassung der Identity-Modell

Durch [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core Identität bietet ein Framework zum Verwalten und Speichern von Benutzerkonten in ASP.NET Core-Anwendungen. Identität wird dem Projekt hinzugefügt, wenn "Einzelne Benutzerkonten" als Authentifizierungsmechanismus ausgewählt ist. Standardmäßig Identität stellt eine Entity Framework (EF) verwenden Core-Datenmodell. Dieser Artikel beschreibt, wie das Identitätsmodell angepasst wird.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Identität und EF Core Migrationen

Untersuchung des Modells, es ist sinnvoll, das Verständnis der Funktionsweise der Identität mit [EF Core Migrationen](/ef/core/managing-schemas/migrations/) erstellen und Aktualisieren einer Datenbank. Auf der obersten Ebene versteht man das:

1. Definieren oder Aktualisieren einer [Datenmodell in Code](/ef/core/modeling/).
1. Fügen Sie eine Migration aus, um dieses Modell in der Änderungen zu übersetzen, die auf die Datenbank angewendet werden können.
1. Überprüfen Sie, dass die Migration Ihre Absichten richtig darstellt.
1. Wenden Sie die Migration zum Aktualisieren der Datenbank um mit dem Modell synchronisiert werden.
1. Wiederholen Sie die Schritte 1 bis 4 weiter verfeinert und lassen Sie die Datenbank synchronisiert.

Migrationen werden hinzugefügt und mithilfe von angewendet:

* Die [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC), wenn Sie Visual Studio verwenden.
* Die [Dotnet Befehle](/ef/core/miscellaneous/cli/dotnet) in der Befehlszeile angegeben.
* Auswählen der Migrationen Link auf der Seite "Fehler", wenn die Anwendung ausgeführt wird.

ASP.NET Core verfügt über einen Entwicklungszeit Seite Fehlerhandler, der verwendet werden kann, um Migrationen angewendet werden soll, wenn die Anwendung ausgeführt wird. Für Produktionsanwendungen ist es oft besser geeigneten SQL-Skripts aus dem Migrationen generieren und diese an die Datenbank als Teil einer gesteuerten Bereitstellung der Anwendung und Datenbank bereitstellen.

Wenn eine neue Anwendung mit der Identität erstellt wird, haben die Schritte 1 und 2 oben bereits abgeschlossen. D. h. das ursprüngliche Datenmodell bereits vorhanden ist, und die anfängliche Migration zum Projekt hinzugefügt wurde. Die anfängliche Migration muss weiterhin auf die Datenbank angewendet werden. Die anfängliche Migration kann erfolgen, durch Ausführen der Update-Database (PMC), Befehls Dotnet Ef Datenbank Update (.NET Core-CLI) oder mithilfe der Seite "Fehler", wenn die Anwendung ausgeführt wird.

Die vorhergehenden Schritte müssen wiederholt werden, wie Änderungen am Modell vorgenommen werden.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Das Identitätsmodell

### <a name="entity-types"></a>Entitätstypen

Das Identitätsmodell besteht aus sieben Entitätstypen:

* `User` -Benutzer darstellt
* `Role` – Stellt eine Rolle
* `UserClaim` – Stellt einen Anspruch, der ein Benutzer besitzt
* `UserToken` – Stellt einen Authentifizierungstoken für einen Benutzer
* `UserLogin` -Ordnet eine Anmeldung ein Benutzers
* `RoleClaim` – Stellt einen Anspruch, der für alle Benutzer innerhalb einer Rolle gewährt wird
* `UserRole` -Entität, die Benutzer und Rollen zuordnet, verknüpfen

### <a name="entity-type-relationships"></a>Typbeziehungen Entität

Diese Entitätstypen werden auf folgende Weise miteinander verknüpft:

* Jede `User` können viele haben `UserClaims`
* Jede `User` können viele haben `UserLogins`
* Jede `User` können viele haben `UserTokens`
* Jede `Role` haben viele verknüpft sind `RoleClaims`
* Jede `User` haben viele zugeordnete `Roles`, und jede `Role` mit vielen Benutzern zugeordnet werden
  * Dies ist eine viele-zu-viele-Beziehung, die eine Jointabelle in der Datenbank erforderlich ist. Die verknüpften Tabelle wird dargestellt, indem die `UserRole` Entität.

### <a name="default-model-configuration"></a>Standardkonfiguration für Modell

Identity definiert eine Reihe von "Kontextklassen" die von erben [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) konfigurieren und verwenden das Modell. Diese Konfiguration erfolgt mithilfe der [EF Core Code First Fluent-API](/ef/core/modeling/) in der [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) -Methode der Context-Klasse. Die Standardkonfiguration ist:

```CSharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Generische Typen des Modells

Identity definiert die Standard-CLR-Typen für jeden der oben aufgeführten Entitätstypen. Diese Typen werden alle mit dem Präfix "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, und `IdentityUserRole`.

Anstatt diese Typen direkt verwenden, können die Typen als Basisklassen für die Typen der Anwendung verwendet werden. Die `DbContext` von Identity definierte Klassen sind generisch, sodass andere CLR-Typen für eine oder mehrere Entitätstypen im Modell verwendet werden können. Dieser generischen Typen können auch für den Typ des Primärschlüssels Benutzer geändert werden.

Bei Verwendung von Unterstützung für Rollen, Identität mit einer [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) Klasse sollten verwendet werden:

```CSharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>

```

Es ist auch möglich, Identity ohne Rollen (nur Ansprüche), verwenden Sie in diesem Fall eine `IdentityUserContext` Klasse sollten verwendet werden:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Anpassen des Modells

Der Ausgangspunkt für das Modell anpassen ist aus der entsprechenden Kontexttyp abgeleitet. finden Sie im vorherigen Abschnitt. Dieser Kontext wird üblicherweise bezeichnet `ApplicationDbContext` und wird erstellt, indem die Vorlagen für ASP.NET Core.

Der Kontext wird verwendet, um das Modell auf zwei Arten konfigurieren:
* Durch Angabe Entität und Schlüssel-Typen für die generischen Typparameter an.
* Durch das Überschreiben von `OnModelCreating` so ändern Sie die Zuordnung dieser Typen.

Zum Überschreiben `OnModelCreating`, `base.OnModelCreating` muss zuerst aufgerufen werden, das Überschreiben Konfiguration als Nächstes aufgerufen werden soll. EF Core hat in der Regel eine Last One Wins-Richtlinie für die Konfiguration. Beispielsweise, wenn die `ToTable` für eine Entität ist zunächst mit einem Tabellennamen aufgerufen, und klicken Sie dann erneut mit einem anderen Tabellennamen später, und Sie dann der Tabellennamen im zweiten Aufruf wird das Projekt, das verwendet wird.

### <a name="using-a-custom-user-type"></a>Verwenden einen benutzerdefinierten Benutzertyp

Um einen benutzerdefinierten Benutzertyp zu verwenden, erstellen Sie den Typ und haben sie die von erben `IdentityUser`. Es ist üblich, benennen Sie diesen Typ `ApplicationUser`. Dieser Typ wird in der Regel zusätzliche Eigenschaften nicht im Basistyp aufweisen, andernfalls gibt es wäre kein Wert erstellt wurde. Zum Beispiel:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Als Nächstes verwenden Sie diesen Typ als generisches Argument für den Kontext:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Update `ConfigureServices` der neuen `ApplicationUser` Klasse:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Besteht keine Notwendigkeit, überschreiben `OnModelCreating` da EF Core zugeordnet, wird hier die `CustomTag` Eigenschaft gemäß der Konvention. Jedoch müssen die Datenbank aktualisiert werden, um ein neues erhalten `CustomTag` Spalte. Zu diesem Zweck fügen Sie eine Migration aus, und klicken Sie dann die Datenbank zu aktualisieren, wie in beschrieben [Identitäts- und EF Core Migrationen](#identity-migrations).

### <a name="changing-the-key-type"></a>Ändern den Typ des Schlüssels

Ändern den Typ des eine primäre Schlüsselspalte (PK) aus, nach dem Erstellen der Datenbank ist auf vielen Datenbanksystemen problematisch. Der Primärschlüssel in der Regel ändern umfasst das Löschen und Neuerstellen der Tabelle. Aus diesem Grund wird empfohlen, dass Schlüsseltypen in der anfänglichen Migration angegeben werden, dass die Zieltypen aus Schlüssel erstellt werden, wenn die Datenbank erstellt wird.

Wenn die Datenbank, klicken Sie dann erstellt wurde `Drop-Database` (PMC) oder `dotnet ef database drop` (.NET Core-CLI) kann verwendet werden, um es zu löschen.

Nachdem bestätigt wurde, dass die Datenbank nicht vorhanden ist, entfernen Sie mit die anfängliche Migration `Remove-Migration` (PMC) oder `dotnet ef migrations remove` (.NET Core-CLI).

Update der `ApplicationDbContext` verwenden eine andere Basisklasse für die neue Schlüsseltyp angeben `TKey`. Beispielsweise verwendet ein `Guid` Schlüssel:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Beachten Sie, dass die generischen Klassen `IdentityUser<TKey>` und `IdentityRole<TKey>` muss auch angegeben werden, um den Typ des neuen Schlüssels verwenden. `ConfigureServices` muss aktualisiert werden, um die Allgemeines Benutzerkonto zu verwenden:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Wenn ein benutzerdefinierter `ApplicationUser` ist verwendeten ADO.NET-Anbieter ab, ein update zu vererben `IdentityUser<TKey>`. Zum Beispiel:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Hinzufügen von Navigationseigenschaften

Ändern der Modellkonfiguration für Beziehungen kann eine schwieriger als das vornehmen weiterer Änderungen sein. Muss darauf geachtet werden, ersetzen Sie die vorhandenen Beziehungen, anstatt eine neue zusätzliche Beziehungen zu erstellen. Insbesondere muss die geänderte Beziehung gleiche Fremdschlüsseleigenschaft als die vorhandene Beziehung angeben. Z. B. die Beziehung zwischen `Users` und `UserClaims` ist standardmäßig als angegeben:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Der Fremdschlüssel für diese Beziehung wird angegeben, wie die `UserClaim.UserId` Eigenschaft. `HasMany` und `WithOne` ohne Argumente zum Erstellen der Beziehung ohne Navigationseigenschaften aufgerufen werden.

Hinzufügen eine Navigationseigenschaft auf `ApplicationUser` ermöglichen zugeordnete `UserClaims` vom Benutzer verwiesen werden:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Beachten Sie, dass die `TKey` für `IdentityUserClaim<TKey>` angegebenen Typ für den Primärschlüssel von Benutzern – in diesem Fall `string` da wir die Standardeinstellungen verwenden. Es ist **nicht** für den primären Schlüsseltyp der `UserClaim` Entitätstyp.

Nun, dass die Navigationseigenschaft vorhanden ist es im konfiguriert sein `OnModelCreating`:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

Beachten Sie, dass die Vertrauensstellung konfiguriert ist, genau so, wie sie zuvor nur für eine Navigationseigenschaft, die im Aufruf angegeben war `HasMany`.

Die Navigationseigenschaften, die nur in der EF-Modell nicht in der Datenbank vorhanden sein. Da sich der Fremdschlüssel für die Beziehung nicht geändert hat, erfordert diese Art von Änderung Modell keine die Datenbank aktualisiert werden. Dies kann durch Hinzufügen einer Migrations nach der Änderung überprüft werden: die `Up` und `Down` Methoden sind leer.

### <a name="adding-all-user-navigation-properties"></a>Alle Navigationseigenschaften des Benutzers hinzufügen

Ist mit der im obigen Abschnitt als Leitfaden, hier ein Beispiel, das unidirektionale Navigationseigenschaften für alle Beziehungen für den Benutzer konfiguriert:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="adding-user-and-role-navigation-properties"></a>Hinzufügen des Benutzers und die Rolle Navigationseigenschaften

Mithilfe der im obigen Abschnitt als Leitfaden, Sie hier ein Beispiel, das Navigationseigenschaften für alle Beziehungen auf Benutzer und die Rolle konfiguriert ist:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Notizen:

* In diesem Beispiel enthält auch die `UserRole` verknüpfen Sie die Entität, die erforderlich sind, um die m: n-Beziehung aus Benutzern Rollen zu navigieren.
* Denken Sie daran, ändern Sie die Typen von Navigationseigenschaften berücksichtigt `ApplicationXxx` Typen sind nun anstelle von verwendeten `IdentityXxx` Typen.
* Denken Sie daran, verwenden Sie die `ApplicationXxx` in der allgemeinen `ApplicationContext` Definition.

### <a name="adding-all-navigation-properties"></a>Alle Navigationseigenschaften hinzufügen

Ist mit der im obigen Abschnitt als Leitfaden, hier ein Beispiel, das Navigationseigenschaften für alle Beziehungen auf für alle Entitätstypen konfiguriert:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });

    }
}
```

### <a name="using-composite-keys"></a>Zusammengesetzte Schlüssel verwenden

In den vorherigen Abschnitten dargestellt, ändern den Typ des Schlüssels im Identitätsmodell verwendet. Ändern die wichtigsten Identitätsmodell Verwendung zusammengesetzter Schlüssel wird nicht unterstützt oder empfohlen. Mithilfe eines zusammengesetzten Schlüssels mit der Identität beinhaltet die Interaktion des Identity Manager-Codes, mit dem Modell, also eine nicht unterstützte Anpassung und würde den Rahmen dieses Dokuments sprengen ändern.

### <a name="changing-tablecolumn-names-and-facets"></a>Ändern der Namen der Datenbanktabelle oder-Spalte und facets

Um die Namen von Tabellen und Spalten zu ändern, rufen `base.OnModelCreating`, und fügen Sie dann auf Konfiguration, um die Standardwerte zu überschreiben. Geben Sie beispielsweise Folgendes ein, um den Namen aller Tabellen an die Identität ändern:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Beachten Sie, dass diese Beispiele für die Identity-Standardtypen verwendet. Wenn Sie einen Anwendungstyp, wie z. B. `ApplicationUser` konfigurieren Sie dann dieses Typs statt der Standardtyp.

Hier ist ein Beispiel, das einige Spaltennamen ändert:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Einige Typen von Datenbankspalten können konfiguriert werden, mit bestimmten _Facets_ z. B. die maximale Zeichenfolgenlänge zulässig. Hier ist ein Beispiel, Max. Spaltenlängen für mehrere Zeichenfolgeneigenschaften in das Modell:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="mapping-to-a-different-schema"></a>Zuordnung zu einem anderen schema

Schemas können in anderen Datenbank-Anbietern anders Verhalten, aber für SQL Server, der Standardwert ist alle Tabellen im Schema "Dbo" zu erstellen. Dies kann geändert werden, um stattdessen die Tabellen in ein anderes Schema zu erstellen. Zum Beispiel:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Lazy Loading

In diesem Abschnitt wird die Unterstützung für das verzögerte Laden von Proxys im Identitätsmodell hinzugefügt. Lazy-Laden ist nützlich, da Navigationseigenschaften verwendet werden, ohne die erste sicherzustellen, die sie geladen werden können.

Entitätstypen können vorgenommen werden geeignet für verzögertes Laden auf verschiedene Weise, wie beschrieben in der [EF Basisdokumentation](/ef/core/querying/related-data#lazy-loading). Der Einfachheit halber verwenden wir verzögerte Laden von Proxys, wofür die:

* Installation von der [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) Paket.
* Ein Aufruf von `.UseLazyLoadingProxies()` in `AddDbContext`.
* Öffentliche Entitätstypen mit öffentlichen virtuellen Navigationseigenschaften.

Hier ist ein Beispiel eines Aufrufs `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Verweisen Sie zurück in den Beispielen oben für die Entitätstypen Navigationseigenschaften hinzugefügt.
