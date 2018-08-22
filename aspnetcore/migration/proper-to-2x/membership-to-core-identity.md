---
title: Migrieren von ASP.NET Membership-Authentifizierung zu ASP.NET Core 2.0-Identitätsanbieter
author: isaac2004
description: Informationen Sie zum Migrieren von vorhandener ASP.NET-Anwendungen mithilfe von Mitgliedschaft-Authentifizierung für ASP.NET Core 2.0-Identitätsanbieter.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "41826037"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="aec77-103">Migrieren von ASP.NET Membership-Authentifizierung zu ASP.NET Core 2.0-Identitätsanbieter</span><span class="sxs-lookup"><span data-stu-id="aec77-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="aec77-104">Von [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="aec77-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="aec77-105">In diesem Artikel wird das Datenbankschema für ASP.NET-Apps mithilfe von Mitgliedschaft-Authentifizierung für ASP.NET Core 2.0-Identität migrieren.</span><span class="sxs-lookup"><span data-stu-id="aec77-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="aec77-106">Dieses Dokument enthält die erforderlichen Schritte zum Migrieren des Datenbankschemas für ASP.NET Membership-basierte apps des Datenbankschemas für ASP.NET Core Identity verwendet.</span><span class="sxs-lookup"><span data-stu-id="aec77-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="aec77-107">Weitere Informationen zum Migrieren von ASP.NET Membership-basierte Authentifizierung zu ASP.NET Identity finden Sie unter [Migrieren einer vorhandenen app aus SQL-Mitgliedschaftsanbieter nach ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="aec77-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="aec77-108">Weitere Informationen zu ASP.NET Core Identity, finden Sie unter [Einführung in die Identität in ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="aec77-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="aec77-109">Überprüfung des mitgliedschaftsschemas</span><span class="sxs-lookup"><span data-stu-id="aec77-109">Review of Membership schema</span></span>

<span data-ttu-id="aec77-110">Vor ASP.NET 2.0 wurden Entwickler damit beauftragt, den Gesamtprozess für Authentifizierung und Autorisierung für ihre apps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aec77-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="aec77-111">Mitgliedschaft wurde mit ASP.NET 2.0 eingeführt und Bereitstellung einer Lösung Codebausteine für die Behandlung von Sicherheit in ASP.NET-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="aec77-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="aec77-112">Entwickler konnten für das Bootstrapping eines Schemas in SQL Server-Datenbank mit der [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) Befehl.</span><span class="sxs-lookup"><span data-stu-id="aec77-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="aec77-113">Nach der Ausführung dieses Befehls wurden in den folgenden Tabellen in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="aec77-113">After running this command, the following tables were created in the database.</span></span>

  ![Mitgliedertabellen](identity/_static/membership-tables.png)

<span data-ttu-id="aec77-115">Zum Migrieren von vorhandenen apps in ASP.NET Core 2.0-Identitätsanbieter müssen die Daten in diesen Tabellen in die von der neuen Identität Schemas verwendeten Tabellen migriert werden.</span><span class="sxs-lookup"><span data-stu-id="aec77-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="aec77-116">ASP.NET Core Identity-2.0-schema</span><span class="sxs-lookup"><span data-stu-id="aec77-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="aec77-117">ASP.NET Core 2.0 folgt die [Identität](/aspnet/identity/index) Prinzip, die in ASP.NET 4.5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="aec77-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="aec77-118">Aber das Prinzip freigegeben wird, die Implementierung zwischen den Frameworks unterscheidet auch zwischen Versionen von ASP.NET Core (finden Sie unter [Migrieren von Authentifizierungs- und Identitätseinstellungen nach ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="aec77-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="aec77-119">Die schnellste Möglichkeit zum Anzeigen des Schemas für ASP.NET Core 2.0-Identitätsanbieter ist die Erstellung eine neue ASP.NET Core 2.0-app.</span><span class="sxs-lookup"><span data-stu-id="aec77-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="aec77-120">In Visual Studio 2017, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="aec77-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="aec77-121">Wählen Sie **Datei** > **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="aec77-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="aec77-122">Erstellen Sie ein neues **ASP.NET Core-Webanwendung** und nennen Sie das Projekt *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="aec77-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="aec77-123">Wählen Sie **ASP.NET Core 2.0** in der Dropdownliste und wählen Sie dann **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="aec77-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="aec77-124">Diese Vorlage erzeugt eine [Razor-Seiten](xref:razor-pages/index) app.</span><span class="sxs-lookup"><span data-stu-id="aec77-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="aec77-125">Bevor Sie auf **OK**, klicken Sie auf **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="aec77-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="aec77-126">Wählen Sie **einzelne Benutzerkonten** für die Identity-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="aec77-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="aec77-127">Klicken Sie abschließend auf **OK**, klicken Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="aec77-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="aec77-128">Visual Studio erstellt ein Projekt mithilfe der ASP.NET Core Identity-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="aec77-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="aec77-129">ASP.NET Core 2.0 Identity verwendet [Entity Framework Core](/ef/core) für die Interaktion mit der Datenbank, die die Authentifizierungsdaten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="aec77-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="aec77-130">In der Reihenfolge für die neu erstellte app funktioniert muss es als Datenbank zum Speichern dieser Daten.</span><span class="sxs-lookup"><span data-stu-id="aec77-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="aec77-131">Die schnellste Möglichkeit, um das Schema in einer datenbankumgebung überprüfen werden nach dem Erstellen einer neuen app, die Datenbank mithilfe von Entity Framework-Migrationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aec77-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="aec77-132">Dieser Vorgang erstellt eine Datenbank, entweder lokal oder an anderer Stelle die imitiert dieses Schema.</span><span class="sxs-lookup"><span data-stu-id="aec77-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="aec77-133">Überprüfen Sie die obige Dokumentation für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="aec77-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="aec77-134">Führen Sie zum Erstellen einer Datenbank mit dem ASP.NET Core Identity-Schema der `Update-Database` in Visual Studio Befehl **-Paket-Manager-Konsole** (PMC) im Fenster&mdash;befindet sich unter **Tools**  >  **NuGet-Paket-Manager** > **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="aec77-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="aec77-135">PMC unterstützt das Ausführen Entity Framework-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="aec77-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="aec77-136">Entity Framework-Befehle verwenden die Verbindungszeichenfolge für die Datenbank, die im angegebenen *"appSettings.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="aec77-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="aec77-137">Die folgende Verbindungszeichenfolge wird eine Datenbank auf *"localhost"* mit dem Namen *Asp-Net-Core-Identity*.</span><span class="sxs-lookup"><span data-stu-id="aec77-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="aec77-138">Mit dieser Einstellung Entity Framework für die Verwendung konfiguriert die `DefaultConnection` Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="aec77-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="aec77-139">Dieser Befehl erstellt mit dem Schema der Datenbank und alle Daten, die für app-Initialisierung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="aec77-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="aec77-140">Die folgende Abbildung zeigt die Struktur der Tabelle, die mit den vorherigen Schritten erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="aec77-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Identity-Tabellen](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="aec77-142">Migrieren des Schemas</span><span class="sxs-lookup"><span data-stu-id="aec77-142">Migrate the schema</span></span>

<span data-ttu-id="aec77-143">Es gibt feine Unterschiede in den Feldern für Mitgliedschafts- und ASP.NET Core Identity "und" Tabellenstrukturen.</span><span class="sxs-lookup"><span data-stu-id="aec77-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="aec77-144">Das Muster für die Authentifizierung/Autorisierung in ASP.NET- und ASP.NET Core-apps wesentlich geändert.</span><span class="sxs-lookup"><span data-stu-id="aec77-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="aec77-145">Sind die wichtigsten Objekte, die immer noch mit der Identität verwendet werden *Benutzer* und *Rollen*.</span><span class="sxs-lookup"><span data-stu-id="aec77-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="aec77-146">Nachfolgend finden Sie Mapping-Tabellen für *Benutzer*, *Rollen*, und *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="aec77-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="aec77-147">Benutzer</span><span class="sxs-lookup"><span data-stu-id="aec77-147">Users</span></span>

| <span data-ttu-id="aec77-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="aec77-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="aec77-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="aec77-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="aec77-150">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-150">**Field Name**</span></span> | <span data-ttu-id="aec77-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-151">**Type**</span></span>  |   <span data-ttu-id="aec77-152">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-152">**Field Name**</span></span> | <span data-ttu-id="aec77-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="aec77-154">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="aec77-155">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-155">string</span></span>
|`UserName` | <span data-ttu-id="aec77-156">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="aec77-157">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-157">string</span></span>
|`Email` | <span data-ttu-id="aec77-158">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="aec77-159">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="aec77-160">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="aec77-161">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="aec77-162">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="aec77-163">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="aec77-164">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="aec77-165">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="aec77-166">Bit</span><span class="sxs-lookup"><span data-stu-id="aec77-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="aec77-167">Bit</span><span class="sxs-lookup"><span data-stu-id="aec77-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="aec77-168">Nicht alle die feldzuordnungen ähneln 1: 1-Beziehungen aus der Mitgliedschaft in ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="aec77-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="aec77-169">Die obigen Tabelle wird das Standardschema für den Mitgliedschaftsbenutzer und ordnet es dem ASP.NET Core Identity-Schema.</span><span class="sxs-lookup"><span data-stu-id="aec77-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="aec77-170">Andere benutzerdefinierte Felder, die für die Mitgliedschaft verwendet wurden, müssen manuell zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="aec77-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="aec77-171">In dieser Zuordnung ist keine Zuordnung für Kennwörter, wie sowohl Kriterien für ein Kennwort und Kennwort Salze nicht zwischen den beiden migrieren.</span><span class="sxs-lookup"><span data-stu-id="aec77-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="aec77-172">**Es wird das Kennwort als Null lassen und Benutzer dazu auffordern, ihre Kennwörter zurücksetzen, empfohlen.**</span><span class="sxs-lookup"><span data-stu-id="aec77-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="aec77-173">In ASP.NET Core Identity kann `LockoutEnd` sollte auf einen Zeitpunkt in der Zukunft festgelegt werden, wenn der Benutzer gesperrt ist. Dies wird in das Migrationsskript gezeigt.</span><span class="sxs-lookup"><span data-stu-id="aec77-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="aec77-174">Rollen</span><span class="sxs-lookup"><span data-stu-id="aec77-174">Roles</span></span>

| <span data-ttu-id="aec77-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="aec77-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="aec77-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="aec77-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="aec77-177">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-177">**Field Name**</span></span> | <span data-ttu-id="aec77-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-178">**Type**</span></span>  |   <span data-ttu-id="aec77-179">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-179">**Field Name**</span></span> | <span data-ttu-id="aec77-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="aec77-181">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-181">string</span></span> | `RoleId` | <span data-ttu-id="aec77-182">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-182">string</span></span>
|`Name` | <span data-ttu-id="aec77-183">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-183">string</span></span> | `RoleName` | <span data-ttu-id="aec77-184">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="aec77-185">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="aec77-186">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="aec77-187">Benutzerrollen</span><span class="sxs-lookup"><span data-stu-id="aec77-187">User Roles</span></span>

| <span data-ttu-id="aec77-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="aec77-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="aec77-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="aec77-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="aec77-190">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-190">**Field Name**</span></span> | <span data-ttu-id="aec77-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-191">**Type**</span></span>  |   <span data-ttu-id="aec77-192">**Name des Felds**</span><span class="sxs-lookup"><span data-stu-id="aec77-192">**Field Name**</span></span> | <span data-ttu-id="aec77-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="aec77-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="aec77-194">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-194">string</span></span> | `RoleId` | <span data-ttu-id="aec77-195">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-195">string</span></span>
|`UserId` | <span data-ttu-id="aec77-196">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-196">string</span></span> | `UserId` | <span data-ttu-id="aec77-197">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="aec77-197">string</span></span>

<span data-ttu-id="aec77-198">Verweisen Sie die vorhergehenden Zuordnungstabellen aus, wenn ein Skript zur schemamigration für erstellen *Benutzer* und *Rollen*.</span><span class="sxs-lookup"><span data-stu-id="aec77-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="aec77-199">Im folgende Beispiel wird davon ausgegangen, dass Sie zwei Datenbanken auf einem Datenbankserver verfügen.</span><span class="sxs-lookup"><span data-stu-id="aec77-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="aec77-200">Eine Datenbank enthält, die vorhandenen ASP.NET Membership-Schema und Daten.</span><span class="sxs-lookup"><span data-stu-id="aec77-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="aec77-201">Die andere Datenbank erstellt wurde, verwenden die oben beschriebene Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="aec77-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="aec77-202">Kommentare sind Inlineschemainformationen enthält weitere Details.</span><span class="sxs-lookup"><span data-stu-id="aec77-202">Comments are included inline for more details.</span></span>

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
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

<span data-ttu-id="aec77-203">Nach Abschluss des Vorgangs dieses Skripts wird die zuvor erstellten ASP.NET Core Identity-app mit Mitgliedschaftsbenutzern aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="aec77-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="aec77-204">Benutzer müssen ihre Kennwörter zu ändern, vor der Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="aec77-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="aec77-205">Hätte das Mitgliedschaftssystem für Benutzer mit Benutzernamen, die nicht ihre e-Mail-Adresse übereinstimmt, sind Änderungen erforderlich, um die app erstellt haben weiter oben, um dies zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="aec77-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="aec77-206">Die Standardvorlage erwartet `UserName` und `Email` identisch sein.</span><span class="sxs-lookup"><span data-stu-id="aec77-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="aec77-207">Melden Sie sich erneut für Situationen, in denen sie verschiedene sind, muss geändert werden, um verwenden `UserName` anstelle von `Email`.</span><span class="sxs-lookup"><span data-stu-id="aec77-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="aec77-208">In der `PageModel` der Anmeldeseite, am *Pages\Account\Login.cshtml.cs*, entfernen Sie die `[EmailAddress]` -Attribut aus der *-e-Mail* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="aec77-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="aec77-209">Benennen Sie sie in *Benutzername*.</span><span class="sxs-lookup"><span data-stu-id="aec77-209">Rename it to *UserName*.</span></span> <span data-ttu-id="aec77-210">Dies erfordert eine Änderung immer `EmailAddress` erwähnt wird, in der *Ansicht* und *"pagemodel"*.</span><span class="sxs-lookup"><span data-stu-id="aec77-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="aec77-211">Das Ergebnis sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="aec77-211">The result looks like the following:</span></span>

 ![Fester Benutzername](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="aec77-213">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="aec77-213">Next steps</span></span>

<span data-ttu-id="aec77-214">In diesem Tutorial haben Sie gelernt, wie Sie Benutzer aus der SQL-Mitgliedschaft in ASP.NET Core 2.0-Identitätsanbieter zu portieren.</span><span class="sxs-lookup"><span data-stu-id="aec77-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="aec77-215">Weitere Informationen zu ASP.NET Core Identity finden Sie unter [Einführung in die Identität](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="aec77-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
