---
title: Migrieren von ASP.NET-Mitgliedschaft Authentifizierung zu ASP.NET Core 2.0 Identity
author: isaac2004
description: Informationen Sie zum Migrieren von vorhandenen ASP.NET-Apps mithilfe von Mitgliedschaft Authentifizierung für ASP.NET Core 2.0 Identity.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="ce8e1-103">Migrieren von ASP.NET-Mitgliedschaft Authentifizierung zu ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="ce8e1-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="ce8e1-104">Von [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="ce8e1-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="ce8e1-105">In diesem Artikel wird das Datenbankschema für ASP.NET-Apps mithilfe von Mitgliedschaft Authentifizierung für ASP.NET Core 2.0 Identity migrieren.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="ce8e1-106">Dieses Dokument enthält die erforderlichen Schritte zum Migrieren des Datenbankschemas für ASP.NET Membership-basierte apps auf das Datenbankschema für ASP.NET Core Identität verwendet.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="ce8e1-107">Weitere Informationen zum Migrieren von ASP.NET-Mitgliedschaft basierende Authentifizierung zu ASP.NET Identity finden Sie unter [Migrieren von einer vorhandenen app aus der SQL-Mitgliedschaft zu ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="ce8e1-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="ce8e1-108">Weitere Informationen zu ASP.NET Core Identity, finden Sie unter [Einführung in die Identität auf ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="ce8e1-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="ce8e1-109">Überprüfen der Mitgliedschaft schema</span><span class="sxs-lookup"><span data-stu-id="ce8e1-109">Review of Membership schema</span></span>

<span data-ttu-id="ce8e1-110">Vor dem ASP.NET 2.0 wurden Entwickler beauftragt, den gesamten Prozess Authentifizierung und Autorisierung für ihre apps erstellen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="ce8e1-111">Mitgliedschaft wurde mit ASP.NET 2.0 eingeführt und Bereitstellung einer Lösung Textbaustein für die Behandlung der Sicherheit in ASP.NET-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="ce8e1-112">Entwickler konnten Bootstrapping ein Schemas in einer SQL Server-Datenbank mit der [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) Befehl.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="ce8e1-113">Nach dem Ausführen dieses Befehls, wurden die folgenden Tabellen in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-113">After running this command, the following tables were created in the database.</span></span>

  ![Mitgliedertabellen](identity/_static/membership-tables.png)

<span data-ttu-id="ce8e1-115">Um vorhandene apps zu ASP.NET Core 2.0 Identity zu migrieren, müssen die Daten in diesen Tabellen in die durch das Schema der neuen Identität verwendeten Tabellen migriert werden.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="ce8e1-116">ASP.NET Core Identität 2.0-schema</span><span class="sxs-lookup"><span data-stu-id="ce8e1-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="ce8e1-117">ASP.NET Core 2.0 folgt die [Identität](/aspnet/identity/index) Prinzip in ASP.NET 4.5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="ce8e1-118">Obwohl das Prinzip gemeinsam verwendet wird, die Implementierung zwischen den Frameworks unterscheidet, sogar zwischen verschiedenen Versionen von ASP.NET Core (finden Sie unter [Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="ce8e1-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="ce8e1-119">Die schnellste Möglichkeit zum Anzeigen des Schemas für ASP.NET Core 2.0 Identität ist zum Erstellen einer neuen ASP.NET Core 2.0-app.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="ce8e1-120">Führen Sie folgende Schritte in Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="ce8e1-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="ce8e1-121">Wählen Sie **Datei** > **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="ce8e1-122">Erstellen Sie ein neues **ASP.NET-Webanwendung für Core**, und nennen Sie das Projekt *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="ce8e1-123">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="ce8e1-124">Diese Vorlage erzeugt eine [Razor-Seiten](xref:mvc/razor-pages/index) app.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="ce8e1-125">Bevor Sie auf **OK**, klicken Sie auf **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="ce8e1-126">Wählen Sie **einzelne Benutzerkonten** für die Identity-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="ce8e1-127">Klicken Sie abschließend auf **OK**, klicken Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="ce8e1-128">Visual Studio erstellt ein Projekt mithilfe der Vorlage für ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="ce8e1-129">ASP.NET Core 2.0 Benutzeridentität verwendet [Entity Framework Core](/ef/core) für die Interaktion mit der Datenbank, die Speichern der Authentifizierungsdaten.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="ce8e1-130">In der Reihenfolge für die neu erstellte app funktioniert muss eine Datenbank zum Speichern dieser Daten sein.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="ce8e1-131">Ist nach dem Erstellen einer neuen app, die schnellste Möglichkeit zum Überprüfen des Schemas in einer datenbankumgebung zum Erstellen der Datenbank über Entity Framework-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="ce8e1-132">Dieser Vorgang erstellt eine Datenbank, entweder lokal oder an anderer Stelle, die dieses Schema imitiert.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="ce8e1-133">Überprüfen Sie die vorherige Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="ce8e1-134">Führen Sie zum Erstellen einer Datenbank mit dem ASP.NET Core Identity-Schema der `Update-Database` in Visual Studio den Befehl **Package Manager Console** (PMC) Fenster&mdash;befindet sich am **Tools**  >  **NuGet Package Manager** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="ce8e1-135">PMC unterstützt ausgeführten Entity Framework-Befehle.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="ce8e1-136">Entity Framework-Befehle verwenden Sie die Verbindungszeichenfolge für die Datenbank im angegebenen *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="ce8e1-137">Die folgende Verbindungszeichenfolge Ziel eine Datenbank auf *"localhost"* mit dem Namen *Asp-Net-Core-Identity*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="ce8e1-138">In dieser Einstellung Entity Framework für die Verwendung konfiguriert die `DefaultConnection` Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="ce8e1-139">Dieser Befehl erstellt mit dem Schema angegebenen Datenbank und alle erforderlichen Daten für app-Initialisierung.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="ce8e1-140">Die folgende Abbildung zeigt die Tabellenstruktur, die mit den vorherigen Schritten erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Identity-Tabellen](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="ce8e1-142">Migrieren des Schemas</span><span class="sxs-lookup"><span data-stu-id="ce8e1-142">Migrate the schema</span></span>

<span data-ttu-id="ce8e1-143">Es gibt feine Unterschiede in den Feldern für die Mitgliedschaft und ASP.NET Core Identity "und" Tabellenstrukturen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="ce8e1-144">Das Muster wurde für die Authentifizierung/Autorisierung mit ASP.NET und ASP.NET Core apps erheblich geändert.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="ce8e1-145">Sind die wichtigsten Objekte, die immer noch mit der Identität verwendet werden *Benutzer* und *Rollen*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="ce8e1-146">Hier werden Mapping-Tabellen für *Benutzer*, *Rollen*, und *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="ce8e1-147">Benutzer</span><span class="sxs-lookup"><span data-stu-id="ce8e1-147">Users</span></span>

| <span data-ttu-id="ce8e1-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="ce8e1-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ce8e1-150">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-150">**Field Name**</span></span> | <span data-ttu-id="ce8e1-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-151">**Type**</span></span>  |   <span data-ttu-id="ce8e1-152">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-152">**Field Name**</span></span> | <span data-ttu-id="ce8e1-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="ce8e1-154">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="ce8e1-155">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-155">string</span></span>
|`UserName` | <span data-ttu-id="ce8e1-156">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="ce8e1-157">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-157">string</span></span>
|`Email` | <span data-ttu-id="ce8e1-158">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="ce8e1-159">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="ce8e1-160">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="ce8e1-161">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="ce8e1-162">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="ce8e1-163">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="ce8e1-164">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="ce8e1-165">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="ce8e1-166">Bit</span><span class="sxs-lookup"><span data-stu-id="ce8e1-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="ce8e1-167">Bit</span><span class="sxs-lookup"><span data-stu-id="ce8e1-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="ce8e1-168">Nicht alle feldzuordnungen etwa 1: 1-Beziehungen aus ihrer Zugehörigkeit zu ASP.NET Core Identität aus.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="ce8e1-169">Die obigen Tabelle wird das Standardschema für den Mitgliedschaftsbenutzer und ordnet es dem ASP.NET Core Identity-Schema.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="ce8e1-170">Alle benutzerdefinierten Felder, die für die Mitgliedschaft verwendet wurden, müssen manuell zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="ce8e1-171">In dieser Zuordnung ist keine Zuordnung für Kennwörter als Kriterien für ein Kennwort und Kennwort Salze nicht zwischen den beiden migrieren.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="ce8e1-172">**Es empfohlen, lassen Sie das Kennwort als Null und bitten Sie Benutzer ihre Kennwörter zurücksetzen.**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="ce8e1-173">In ASP.NET Core Identität `LockoutEnd` sollte auf einen Zeitpunkt in der Zukunft festgelegt werden, wenn der Benutzer gesperrt ist. Dies wird in das Migrationsskript gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="ce8e1-174">Rollen</span><span class="sxs-lookup"><span data-stu-id="ce8e1-174">Roles</span></span>

| <span data-ttu-id="ce8e1-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="ce8e1-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ce8e1-177">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-177">**Field Name**</span></span> | <span data-ttu-id="ce8e1-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-178">**Type**</span></span>  |   <span data-ttu-id="ce8e1-179">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-179">**Field Name**</span></span> | <span data-ttu-id="ce8e1-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="ce8e1-181">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-181">string</span></span> | `RoleId` | <span data-ttu-id="ce8e1-182">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-182">string</span></span>
|`Name` | <span data-ttu-id="ce8e1-183">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-183">string</span></span> | `RoleName` | <span data-ttu-id="ce8e1-184">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="ce8e1-185">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="ce8e1-186">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="ce8e1-187">Benutzerrollen</span><span class="sxs-lookup"><span data-stu-id="ce8e1-187">User Roles</span></span>

| <span data-ttu-id="ce8e1-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="ce8e1-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="ce8e1-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ce8e1-190">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-190">**Field Name**</span></span> | <span data-ttu-id="ce8e1-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-191">**Type**</span></span>  |   <span data-ttu-id="ce8e1-192">**Feldname**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-192">**Field Name**</span></span> | <span data-ttu-id="ce8e1-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="ce8e1-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="ce8e1-194">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-194">string</span></span> | `RoleId` | <span data-ttu-id="ce8e1-195">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-195">string</span></span>
|`UserId` | <span data-ttu-id="ce8e1-196">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-196">string</span></span> | `UserId` | <span data-ttu-id="ce8e1-197">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce8e1-197">string</span></span>

<span data-ttu-id="ce8e1-198">Die vorangehenden Mapping-Tabellen verweisen, für die Erstellung ein Migrationsskript für *Benutzer* und *Rollen*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="ce8e1-199">Im folgende Beispiel wird davon ausgegangen, dass Sie zwei Datenbanken auf einem Datenbankserver verfügen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="ce8e1-200">Eine Datenbank enthält das Schema der vorhandenen ASP.NET-Mitgliedschaft und Daten.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="ce8e1-201">Die andere Datenbank wurde erstellt, mit der oben beschriebenen Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="ce8e1-202">Kommentare sind Inlineschemainformationen enthält weitere Details.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="ce8e1-203">Nach der Ausführung dieses Skripts wird die zuvor erstellte ASP.NET Core Identity-app mit Mitgliedschaftsbenutzern aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="ce8e1-204">Benutzer müssen ihre Kennwörter, die vor der Anmeldung ändern.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="ce8e1-205">Wäre das Mitgliedschaftssystem für Benutzer mit Benutzernamen, die nicht ihre e-Mail-Adresse übereinstimmt, sind Änderungen erforderlich, um die app, die dies zuvor erstellten.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="ce8e1-206">Die Standardvorlage erwartet `UserName` und `Email` identisch sein.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="ce8e1-207">Der Anmeldevorgang für Situationen, in denen sie verschiedene sind, muss geändert werden, um verwenden `UserName` anstelle von `Email`.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="ce8e1-208">In der `PageModel` der Anmeldeseite, finden Sie unter *Pages\Account\Login.cshtml.cs*, entfernen Sie die `[EmailAddress]` -Attribut aus der *-e-Mail* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="ce8e1-209">Benennen Sie sie um *Benutzername*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-209">Rename it to *UserName*.</span></span> <span data-ttu-id="ce8e1-210">Dies erfordert eine Änderung wo `EmailAddress` erwähnt ist, in der *Ansicht* und *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="ce8e1-211">Das Ergebnis sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="ce8e1-211">The result looks like the following:</span></span>

 ![Fester Benutzername](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="ce8e1-213">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ce8e1-213">Next steps</span></span>

<span data-ttu-id="ce8e1-214">In diesem Lernprogramm haben Sie gelernt, wie Benutzer aus der SQL-Mitgliedschaft in ASP.NET Core 2.0 Identity port.</span><span class="sxs-lookup"><span data-stu-id="ce8e1-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="ce8e1-215">Weitere Informationen zu ASP.NET Core Identity, finden Sie unter [Einführung in die Identität](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="ce8e1-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
