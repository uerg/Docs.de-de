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
# <a name="identity-model-customization"></a><span data-ttu-id="c09ae-103">Anpassung der Identity-Modell</span><span class="sxs-lookup"><span data-stu-id="c09ae-103">Identity model customization</span></span>

<span data-ttu-id="c09ae-104">Durch [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="c09ae-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="c09ae-105">ASP.NET Core Identität bietet ein Framework zum Verwalten und Speichern von Benutzerkonten in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="c09ae-106">Identität wird dem Projekt hinzugefügt, wenn "Einzelne Benutzerkonten" als Authentifizierungsmechanismus ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="c09ae-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="c09ae-107">Standardmäßig Identität stellt eine Entity Framework (EF) verwenden Core-Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="c09ae-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="c09ae-108">Dieser Artikel beschreibt, wie das Identitätsmodell angepasst wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="c09ae-109">Identität und EF Core Migrationen</span><span class="sxs-lookup"><span data-stu-id="c09ae-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="c09ae-110">Untersuchung des Modells, es ist sinnvoll, das Verständnis der Funktionsweise der Identität mit [EF Core Migrationen](/ef/core/managing-schemas/migrations/) erstellen und Aktualisieren einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c09ae-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="c09ae-111">Auf der obersten Ebene versteht man das:</span><span class="sxs-lookup"><span data-stu-id="c09ae-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="c09ae-112">Definieren oder Aktualisieren einer [Datenmodell in Code](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="c09ae-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="c09ae-113">Fügen Sie eine Migration aus, um dieses Modell in der Änderungen zu übersetzen, die auf die Datenbank angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="c09ae-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="c09ae-114">Überprüfen Sie, dass die Migration Ihre Absichten richtig darstellt.</span><span class="sxs-lookup"><span data-stu-id="c09ae-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="c09ae-115">Wenden Sie die Migration zum Aktualisieren der Datenbank um mit dem Modell synchronisiert werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="c09ae-116">Wiederholen Sie die Schritte 1 bis 4 weiter verfeinert und lassen Sie die Datenbank synchronisiert.</span><span class="sxs-lookup"><span data-stu-id="c09ae-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="c09ae-117">Migrationen werden hinzugefügt und mithilfe von angewendet:</span><span class="sxs-lookup"><span data-stu-id="c09ae-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="c09ae-118">Die [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC), wenn Sie Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="c09ae-119">Die [Dotnet Befehle](/ef/core/miscellaneous/cli/dotnet) in der Befehlszeile angegeben.</span><span class="sxs-lookup"><span data-stu-id="c09ae-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="c09ae-120">Auswählen der Migrationen Link auf der Seite "Fehler", wenn die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="c09ae-121">ASP.NET Core verfügt über einen Entwicklungszeit Seite Fehlerhandler, der verwendet werden kann, um Migrationen angewendet werden soll, wenn die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="c09ae-122">Für Produktionsanwendungen ist es oft besser geeigneten SQL-Skripts aus dem Migrationen generieren und diese an die Datenbank als Teil einer gesteuerten Bereitstellung der Anwendung und Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="c09ae-123">Wenn eine neue Anwendung mit der Identität erstellt wird, haben die Schritte 1 und 2 oben bereits abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="c09ae-124">D. h. das ursprüngliche Datenmodell bereits vorhanden ist, und die anfängliche Migration zum Projekt hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="c09ae-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="c09ae-125">Die anfängliche Migration muss weiterhin auf die Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="c09ae-126">Die anfängliche Migration kann erfolgen, durch Ausführen der Update-Database (PMC), Befehls Dotnet Ef Datenbank Update (.NET Core-CLI) oder mithilfe der Seite "Fehler", wenn die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="c09ae-127">Die vorhergehenden Schritte müssen wiederholt werden, wie Änderungen am Modell vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="c09ae-128">Das Identitätsmodell</span><span class="sxs-lookup"><span data-stu-id="c09ae-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="c09ae-129">Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="c09ae-129">Entity types</span></span>

<span data-ttu-id="c09ae-130">Das Identitätsmodell besteht aus sieben Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="c09ae-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="c09ae-131">`User` -Benutzer darstellt</span><span class="sxs-lookup"><span data-stu-id="c09ae-131">`User` - represents the user</span></span>
* <span data-ttu-id="c09ae-132">`Role` – Stellt eine Rolle</span><span class="sxs-lookup"><span data-stu-id="c09ae-132">`Role` - represents a role</span></span>
* <span data-ttu-id="c09ae-133">`UserClaim` – Stellt einen Anspruch, der ein Benutzer besitzt</span><span class="sxs-lookup"><span data-stu-id="c09ae-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="c09ae-134">`UserToken` – Stellt einen Authentifizierungstoken für einen Benutzer</span><span class="sxs-lookup"><span data-stu-id="c09ae-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="c09ae-135">`UserLogin` -Ordnet eine Anmeldung ein Benutzers</span><span class="sxs-lookup"><span data-stu-id="c09ae-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="c09ae-136">`RoleClaim` – Stellt einen Anspruch, der für alle Benutzer innerhalb einer Rolle gewährt wird</span><span class="sxs-lookup"><span data-stu-id="c09ae-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="c09ae-137">`UserRole` -Entität, die Benutzer und Rollen zuordnet, verknüpfen</span><span class="sxs-lookup"><span data-stu-id="c09ae-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="c09ae-138">Typbeziehungen Entität</span><span class="sxs-lookup"><span data-stu-id="c09ae-138">Entity type relationships</span></span>

<span data-ttu-id="c09ae-139">Diese Entitätstypen werden auf folgende Weise miteinander verknüpft:</span><span class="sxs-lookup"><span data-stu-id="c09ae-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="c09ae-140">Jede `User` können viele haben `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="c09ae-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="c09ae-141">Jede `User` können viele haben `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="c09ae-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="c09ae-142">Jede `User` können viele haben `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="c09ae-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="c09ae-143">Jede `Role` haben viele verknüpft sind `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="c09ae-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="c09ae-144">Jede `User` haben viele zugeordnete `Roles`, und jede `Role` mit vielen Benutzern zugeordnet werden</span><span class="sxs-lookup"><span data-stu-id="c09ae-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="c09ae-145">Dies ist eine viele-zu-viele-Beziehung, die eine Jointabelle in der Datenbank erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="c09ae-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="c09ae-146">Die verknüpften Tabelle wird dargestellt, indem die `UserRole` Entität.</span><span class="sxs-lookup"><span data-stu-id="c09ae-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="c09ae-147">Standardkonfiguration für Modell</span><span class="sxs-lookup"><span data-stu-id="c09ae-147">Default model configuration</span></span>

<span data-ttu-id="c09ae-148">Identity definiert eine Reihe von "Kontextklassen" die von erben [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) konfigurieren und verwenden das Modell.</span><span class="sxs-lookup"><span data-stu-id="c09ae-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="c09ae-149">Diese Konfiguration erfolgt mithilfe der [EF Core Code First Fluent-API](/ef/core/modeling/) in der [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) -Methode der Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="c09ae-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="c09ae-150">Die Standardkonfiguration ist:</span><span class="sxs-lookup"><span data-stu-id="c09ae-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="c09ae-151">Generische Typen des Modells</span><span class="sxs-lookup"><span data-stu-id="c09ae-151">Model generic types</span></span>

<span data-ttu-id="c09ae-152">Identity definiert die Standard-CLR-Typen für jeden der oben aufgeführten Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="c09ae-153">Diese Typen werden alle mit dem Präfix "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, und `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="c09ae-154">Anstatt diese Typen direkt verwenden, können die Typen als Basisklassen für die Typen der Anwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="c09ae-155">Die `DbContext` von Identity definierte Klassen sind generisch, sodass andere CLR-Typen für eine oder mehrere Entitätstypen im Modell verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="c09ae-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="c09ae-156">Dieser generischen Typen können auch für den Typ des Primärschlüssels Benutzer geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="c09ae-157">Bei Verwendung von Unterstützung für Rollen, Identität mit einer [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) Klasse sollten verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="c09ae-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="c09ae-158">Es ist auch möglich, Identity ohne Rollen (nur Ansprüche), verwenden Sie in diesem Fall eine `IdentityUserContext` Klasse sollten verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="c09ae-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="c09ae-159">Anpassen des Modells</span><span class="sxs-lookup"><span data-stu-id="c09ae-159">Customizing the model</span></span>

<span data-ttu-id="c09ae-160">Der Ausgangspunkt für das Modell anpassen ist aus der entsprechenden Kontexttyp abgeleitet. finden Sie im vorherigen Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="c09ae-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="c09ae-161">Dieser Kontext wird üblicherweise bezeichnet `ApplicationDbContext` und wird erstellt, indem die Vorlagen für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c09ae-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="c09ae-162">Der Kontext wird verwendet, um das Modell auf zwei Arten konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="c09ae-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="c09ae-163">Durch Angabe Entität und Schlüssel-Typen für die generischen Typparameter an.</span><span class="sxs-lookup"><span data-stu-id="c09ae-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="c09ae-164">Durch das Überschreiben von `OnModelCreating` so ändern Sie die Zuordnung dieser Typen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="c09ae-165">Zum Überschreiben `OnModelCreating`, `base.OnModelCreating` muss zuerst aufgerufen werden, das Überschreiben Konfiguration als Nächstes aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="c09ae-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="c09ae-166">EF Core hat in der Regel eine Last One Wins-Richtlinie für die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c09ae-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="c09ae-167">Beispielsweise, wenn die `ToTable` für eine Entität ist zunächst mit einem Tabellennamen aufgerufen, und klicken Sie dann erneut mit einem anderen Tabellennamen später, und Sie dann der Tabellennamen im zweiten Aufruf wird das Projekt, das verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="c09ae-168">Verwenden einen benutzerdefinierten Benutzertyp</span><span class="sxs-lookup"><span data-stu-id="c09ae-168">Using a custom User type</span></span>

<span data-ttu-id="c09ae-169">Um einen benutzerdefinierten Benutzertyp zu verwenden, erstellen Sie den Typ und haben sie die von erben `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="c09ae-170">Es ist üblich, benennen Sie diesen Typ `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="c09ae-171">Dieser Typ wird in der Regel zusätzliche Eigenschaften nicht im Basistyp aufweisen, andernfalls gibt es wäre kein Wert erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="c09ae-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="c09ae-172">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c09ae-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="c09ae-173">Als Nächstes verwenden Sie diesen Typ als generisches Argument für den Kontext:</span><span class="sxs-lookup"><span data-stu-id="c09ae-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="c09ae-174">Update `ConfigureServices` der neuen `ApplicationUser` Klasse:</span><span class="sxs-lookup"><span data-stu-id="c09ae-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="c09ae-175">Besteht keine Notwendigkeit, überschreiben `OnModelCreating` da EF Core zugeordnet, wird hier die `CustomTag` Eigenschaft gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="c09ae-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="c09ae-176">Jedoch müssen die Datenbank aktualisiert werden, um ein neues erhalten `CustomTag` Spalte.</span><span class="sxs-lookup"><span data-stu-id="c09ae-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="c09ae-177">Zu diesem Zweck fügen Sie eine Migration aus, und klicken Sie dann die Datenbank zu aktualisieren, wie in beschrieben [Identitäts- und EF Core Migrationen](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="c09ae-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="c09ae-178">Ändern den Typ des Schlüssels</span><span class="sxs-lookup"><span data-stu-id="c09ae-178">Changing the key type</span></span>

<span data-ttu-id="c09ae-179">Ändern den Typ des eine primäre Schlüsselspalte (PK) aus, nach dem Erstellen der Datenbank ist auf vielen Datenbanksystemen problematisch.</span><span class="sxs-lookup"><span data-stu-id="c09ae-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="c09ae-180">Der Primärschlüssel in der Regel ändern umfasst das Löschen und Neuerstellen der Tabelle.</span><span class="sxs-lookup"><span data-stu-id="c09ae-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="c09ae-181">Aus diesem Grund wird empfohlen, dass Schlüsseltypen in der anfänglichen Migration angegeben werden, dass die Zieltypen aus Schlüssel erstellt werden, wenn die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c09ae-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="c09ae-182">Wenn die Datenbank, klicken Sie dann erstellt wurde `Drop-Database` (PMC) oder `dotnet ef database drop` (.NET Core-CLI) kann verwendet werden, um es zu löschen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="c09ae-183">Nachdem bestätigt wurde, dass die Datenbank nicht vorhanden ist, entfernen Sie mit die anfängliche Migration `Remove-Migration` (PMC) oder `dotnet ef migrations remove` (.NET Core-CLI).</span><span class="sxs-lookup"><span data-stu-id="c09ae-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="c09ae-184">Update der `ApplicationDbContext` verwenden eine andere Basisklasse für die neue Schlüsseltyp angeben `TKey`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="c09ae-185">Beispielsweise verwendet ein `Guid` Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="c09ae-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="c09ae-186">Beachten Sie, dass die generischen Klassen `IdentityUser<TKey>` und `IdentityRole<TKey>` muss auch angegeben werden, um den Typ des neuen Schlüssels verwenden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="c09ae-187">`ConfigureServices` muss aktualisiert werden, um die Allgemeines Benutzerkonto zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="c09ae-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="c09ae-188">Wenn ein benutzerdefinierter `ApplicationUser` ist verwendeten ADO.NET-Anbieter ab, ein update zu vererben `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="c09ae-189">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c09ae-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="c09ae-190">Hinzufügen von Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="c09ae-190">Adding navigation properties</span></span>

<span data-ttu-id="c09ae-191">Ändern der Modellkonfiguration für Beziehungen kann eine schwieriger als das vornehmen weiterer Änderungen sein.</span><span class="sxs-lookup"><span data-stu-id="c09ae-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="c09ae-192">Muss darauf geachtet werden, ersetzen Sie die vorhandenen Beziehungen, anstatt eine neue zusätzliche Beziehungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="c09ae-193">Insbesondere muss die geänderte Beziehung gleiche Fremdschlüsseleigenschaft als die vorhandene Beziehung angeben.</span><span class="sxs-lookup"><span data-stu-id="c09ae-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="c09ae-194">Z. B. die Beziehung zwischen `Users` und `UserClaims` ist standardmäßig als angegeben:</span><span class="sxs-lookup"><span data-stu-id="c09ae-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="c09ae-195">Der Fremdschlüssel für diese Beziehung wird angegeben, wie die `UserClaim.UserId` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c09ae-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="c09ae-196">`HasMany` und `WithOne` ohne Argumente zum Erstellen der Beziehung ohne Navigationseigenschaften aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="c09ae-197">Hinzufügen eine Navigationseigenschaft auf `ApplicationUser` ermöglichen zugeordnete `UserClaims` vom Benutzer verwiesen werden:</span><span class="sxs-lookup"><span data-stu-id="c09ae-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="c09ae-198">Beachten Sie, dass die `TKey` für `IdentityUserClaim<TKey>` angegebenen Typ für den Primärschlüssel von Benutzern – in diesem Fall `string` da wir die Standardeinstellungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="c09ae-199">Es ist **nicht** für den primären Schlüsseltyp der `UserClaim` Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="c09ae-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="c09ae-200">Nun, dass die Navigationseigenschaft vorhanden ist es im konfiguriert sein `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="c09ae-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="c09ae-201">Beachten Sie, dass die Vertrauensstellung konfiguriert ist, genau so, wie sie zuvor nur für eine Navigationseigenschaft, die im Aufruf angegeben war `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="c09ae-202">Die Navigationseigenschaften, die nur in der EF-Modell nicht in der Datenbank vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="c09ae-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="c09ae-203">Da sich der Fremdschlüssel für die Beziehung nicht geändert hat, erfordert diese Art von Änderung Modell keine die Datenbank aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="c09ae-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="c09ae-204">Dies kann durch Hinzufügen einer Migrations nach der Änderung überprüft werden: die `Up` und `Down` Methoden sind leer.</span><span class="sxs-lookup"><span data-stu-id="c09ae-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="c09ae-205">Alle Navigationseigenschaften des Benutzers hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c09ae-205">Adding all User navigation properties</span></span>

<span data-ttu-id="c09ae-206">Ist mit der im obigen Abschnitt als Leitfaden, hier ein Beispiel, das unidirektionale Navigationseigenschaften für alle Beziehungen für den Benutzer konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="c09ae-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="c09ae-207">Hinzufügen des Benutzers und die Rolle Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="c09ae-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="c09ae-208">Mithilfe der im obigen Abschnitt als Leitfaden, Sie hier ein Beispiel, das Navigationseigenschaften für alle Beziehungen auf Benutzer und die Rolle konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="c09ae-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="c09ae-209">Notizen:</span><span class="sxs-lookup"><span data-stu-id="c09ae-209">Notes:</span></span>

* <span data-ttu-id="c09ae-210">In diesem Beispiel enthält auch die `UserRole` verknüpfen Sie die Entität, die erforderlich sind, um die m: n-Beziehung aus Benutzern Rollen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="c09ae-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="c09ae-211">Denken Sie daran, ändern Sie die Typen von Navigationseigenschaften berücksichtigt `ApplicationXxx` Typen sind nun anstelle von verwendeten `IdentityXxx` Typen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="c09ae-212">Denken Sie daran, verwenden Sie die `ApplicationXxx` in der allgemeinen `ApplicationContext` Definition.</span><span class="sxs-lookup"><span data-stu-id="c09ae-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="c09ae-213">Alle Navigationseigenschaften hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c09ae-213">Adding all navigation properties</span></span>

<span data-ttu-id="c09ae-214">Ist mit der im obigen Abschnitt als Leitfaden, hier ein Beispiel, das Navigationseigenschaften für alle Beziehungen auf für alle Entitätstypen konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="c09ae-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="c09ae-215">Zusammengesetzte Schlüssel verwenden</span><span class="sxs-lookup"><span data-stu-id="c09ae-215">Using composite keys</span></span>

<span data-ttu-id="c09ae-216">In den vorherigen Abschnitten dargestellt, ändern den Typ des Schlüssels im Identitätsmodell verwendet.</span><span class="sxs-lookup"><span data-stu-id="c09ae-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="c09ae-217">Ändern die wichtigsten Identitätsmodell Verwendung zusammengesetzter Schlüssel wird nicht unterstützt oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="c09ae-218">Mithilfe eines zusammengesetzten Schlüssels mit der Identität beinhaltet die Interaktion des Identity Manager-Codes, mit dem Modell, also eine nicht unterstützte Anpassung und würde den Rahmen dieses Dokuments sprengen ändern.</span><span class="sxs-lookup"><span data-stu-id="c09ae-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="c09ae-219">Ändern der Namen der Datenbanktabelle oder-Spalte und facets</span><span class="sxs-lookup"><span data-stu-id="c09ae-219">Changing table/column names and facets</span></span>

<span data-ttu-id="c09ae-220">Um die Namen von Tabellen und Spalten zu ändern, rufen `base.OnModelCreating`, und fügen Sie dann auf Konfiguration, um die Standardwerte zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="c09ae-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="c09ae-221">Geben Sie beispielsweise Folgendes ein, um den Namen aller Tabellen an die Identität ändern:</span><span class="sxs-lookup"><span data-stu-id="c09ae-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="c09ae-222">Beachten Sie, dass diese Beispiele für die Identity-Standardtypen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c09ae-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="c09ae-223">Wenn Sie einen Anwendungstyp, wie z. B. `ApplicationUser` konfigurieren Sie dann dieses Typs statt der Standardtyp.</span><span class="sxs-lookup"><span data-stu-id="c09ae-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="c09ae-224">Hier ist ein Beispiel, das einige Spaltennamen ändert:</span><span class="sxs-lookup"><span data-stu-id="c09ae-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="c09ae-225">Einige Typen von Datenbankspalten können konfiguriert werden, mit bestimmten _Facets_ z. B. die maximale Zeichenfolgenlänge zulässig.</span><span class="sxs-lookup"><span data-stu-id="c09ae-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="c09ae-226">Hier ist ein Beispiel, Max. Spaltenlängen für mehrere Zeichenfolgeneigenschaften in das Modell:</span><span class="sxs-lookup"><span data-stu-id="c09ae-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="c09ae-227">Zuordnung zu einem anderen schema</span><span class="sxs-lookup"><span data-stu-id="c09ae-227">Mapping to a different schema</span></span>

<span data-ttu-id="c09ae-228">Schemas können in anderen Datenbank-Anbietern anders Verhalten, aber für SQL Server, der Standardwert ist alle Tabellen im Schema "Dbo" zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="c09ae-229">Dies kann geändert werden, um stattdessen die Tabellen in ein anderes Schema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c09ae-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="c09ae-230">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c09ae-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="c09ae-231">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="c09ae-231">Lazy loading</span></span>

<span data-ttu-id="c09ae-232">In diesem Abschnitt wird die Unterstützung für das verzögerte Laden von Proxys im Identitätsmodell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c09ae-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="c09ae-233">Lazy-Laden ist nützlich, da Navigationseigenschaften verwendet werden, ohne die erste sicherzustellen, die sie geladen werden können.</span><span class="sxs-lookup"><span data-stu-id="c09ae-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="c09ae-234">Entitätstypen können vorgenommen werden geeignet für verzögertes Laden auf verschiedene Weise, wie beschrieben in der [EF Basisdokumentation](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="c09ae-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="c09ae-235">Der Einfachheit halber verwenden wir verzögerte Laden von Proxys, wofür die:</span><span class="sxs-lookup"><span data-stu-id="c09ae-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="c09ae-236">Installation von der [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) Paket.</span><span class="sxs-lookup"><span data-stu-id="c09ae-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="c09ae-237">Ein Aufruf von `.UseLazyLoadingProxies()` in `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c09ae-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="c09ae-238">Öffentliche Entitätstypen mit öffentlichen virtuellen Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c09ae-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="c09ae-239">Hier ist ein Beispiel eines Aufrufs `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="c09ae-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="c09ae-240">Verweisen Sie zurück in den Beispielen oben für die Entitätstypen Navigationseigenschaften hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c09ae-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
