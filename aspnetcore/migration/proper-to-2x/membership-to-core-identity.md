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
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851542"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrieren von ASP.NET-Mitgliedschaft Authentifizierung zu ASP.NET Core 2.0 Identity

Von [Isaac Levin](https://isaaclevin.com)

In diesem Artikel wird das Datenbankschema für ASP.NET-Apps mithilfe von Mitgliedschaft Authentifizierung für ASP.NET Core 2.0 Identity migrieren.

> [!NOTE]
> Dieses Dokument enthält die erforderlichen Schritte zum Migrieren des Datenbankschemas für ASP.NET Membership-basierte apps auf das Datenbankschema für ASP.NET Core Identität verwendet. Weitere Informationen zum Migrieren von ASP.NET-Mitgliedschaft basierende Authentifizierung zu ASP.NET Identity finden Sie unter [Migrieren von einer vorhandenen app aus der SQL-Mitgliedschaft zu ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Weitere Informationen zu ASP.NET Core Identity, finden Sie unter [Einführung in die Identität auf ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Überprüfen der Mitgliedschaft schema

Vor dem ASP.NET 2.0 wurden Entwickler beauftragt, den gesamten Prozess Authentifizierung und Autorisierung für ihre apps erstellen. Mitgliedschaft wurde mit ASP.NET 2.0 eingeführt und Bereitstellung einer Lösung Textbaustein für die Behandlung der Sicherheit in ASP.NET-Anwendungen. Entwickler konnten Bootstrapping ein Schemas in einer SQL Server-Datenbank mit der [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) Befehl. Nach dem Ausführen dieses Befehls, wurden die folgenden Tabellen in der Datenbank erstellt.

  ![Mitgliedertabellen](identity/_static/membership-tables.png)

Um vorhandene apps zu ASP.NET Core 2.0 Identity zu migrieren, müssen die Daten in diesen Tabellen in die durch das Schema der neuen Identität verwendeten Tabellen migriert werden.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identität 2.0-schema

ASP.NET Core 2.0 folgt die [Identität](/aspnet/identity/index) Prinzip in ASP.NET 4.5 eingeführt. Obwohl das Prinzip gemeinsam verwendet wird, die Implementierung zwischen den Frameworks unterscheidet, sogar zwischen verschiedenen Versionen von ASP.NET Core (finden Sie unter [Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Die schnellste Möglichkeit zum Anzeigen des Schemas für ASP.NET Core 2.0 Identität ist zum Erstellen einer neuen ASP.NET Core 2.0-app. Führen Sie folgende Schritte in Visual Studio 2017:

* Wählen Sie **Datei** > **Neu** > **Projekt** aus.
* Erstellen Sie ein neues **ASP.NET-Webanwendung für Core**, und nennen Sie das Projekt *CoreIdentitySample*.
* Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**. Diese Vorlage erzeugt eine [Razor-Seiten](xref:mvc/razor-pages/index) app. Bevor Sie auf **OK**, klicken Sie auf **Authentifizierung ändern**.
* Wählen Sie **einzelne Benutzerkonten** für die Identity-Vorlagen. Klicken Sie abschließend auf **OK**, klicken Sie dann **OK**. Visual Studio erstellt ein Projekt mithilfe der Vorlage für ASP.NET Core Identity.

ASP.NET Core 2.0 Benutzeridentität verwendet [Entity Framework Core](/ef/core) für die Interaktion mit der Datenbank, die Speichern der Authentifizierungsdaten. In der Reihenfolge für die neu erstellte app funktioniert muss eine Datenbank zum Speichern dieser Daten sein. Ist nach dem Erstellen einer neuen app, die schnellste Möglichkeit zum Überprüfen des Schemas in einer datenbankumgebung zum Erstellen der Datenbank über Entity Framework-Migrationen. Dieser Vorgang erstellt eine Datenbank, entweder lokal oder an anderer Stelle, die dieses Schema imitiert. Überprüfen Sie die vorherige Dokumentation weitere Informationen.

Führen Sie zum Erstellen einer Datenbank mit dem ASP.NET Core Identity-Schema der `Update-Database` in Visual Studio den Befehl **Package Manager Console** (PMC) Fenster&mdash;befindet sich am **Tools**  >  **NuGet Package Manager** > **Package Manager Console**. PMC unterstützt ausgeführten Entity Framework-Befehle.

Entity Framework-Befehle verwenden Sie die Verbindungszeichenfolge für die Datenbank im angegebenen *appsettings.json*. Die folgende Verbindungszeichenfolge Ziel eine Datenbank auf *"localhost"* mit dem Namen *Asp-Net-Core-Identity*. In dieser Einstellung Entity Framework für die Verwendung konfiguriert die `DefaultConnection` Verbindungszeichenfolge.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Dieser Befehl erstellt mit dem Schema angegebenen Datenbank und alle erforderlichen Daten für app-Initialisierung. Die folgende Abbildung zeigt die Tabellenstruktur, die mit den vorherigen Schritten erstellt wird.

   ![Identity-Tabellen](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrieren des Schemas

Es gibt feine Unterschiede in den Feldern für die Mitgliedschaft und ASP.NET Core Identity "und" Tabellenstrukturen. Das Muster wurde für die Authentifizierung/Autorisierung mit ASP.NET und ASP.NET Core apps erheblich geändert. Sind die wichtigsten Objekte, die immer noch mit der Identität verwendet werden *Benutzer* und *Rollen*. Hier werden Mapping-Tabellen für *Benutzer*, *Rollen*, und *UserRoles*.

### <a name="users"></a>Benutzer

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Feldname** | **Type**  |   **Feldname** | **Type**  |
|`Id` | Zeichenfolge | `aspnet_Users.UserId` | Zeichenfolge
|`UserName` | Zeichenfolge | `aspnet_Users.UserName` | Zeichenfolge
|`Email` | Zeichenfolge | `aspnet_Membership.Email` | Zeichenfolge
|`NormalizedUserName` | Zeichenfolge | `aspnet_Users.LoweredUserName` | Zeichenfolge
|`NormalizedEmail` | Zeichenfolge | `aspnet_Membership.LoweredEmail` | Zeichenfolge
|`PhoneNumber` | Zeichenfolge | `aspnet_Users.MobileAlias` | Zeichenfolge
|`LockoutEnabled` | Bit | `aspnet_Membership.IsLockedOut` | Bit

> [!NOTE]
> Nicht alle feldzuordnungen etwa 1: 1-Beziehungen aus ihrer Zugehörigkeit zu ASP.NET Core Identität aus. Die obigen Tabelle wird das Standardschema für den Mitgliedschaftsbenutzer und ordnet es dem ASP.NET Core Identity-Schema. Alle benutzerdefinierten Felder, die für die Mitgliedschaft verwendet wurden, müssen manuell zugeordnet werden. In dieser Zuordnung ist keine Zuordnung für Kennwörter als Kriterien für ein Kennwort und Kennwort Salze nicht zwischen den beiden migrieren. **Es empfohlen, lassen Sie das Kennwort als Null und bitten Sie Benutzer ihre Kennwörter zurücksetzen.** In ASP.NET Core Identität `LockoutEnd` sollte auf einen Zeitpunkt in der Zukunft festgelegt werden, wenn der Benutzer gesperrt ist. Dies wird in das Migrationsskript gezeigt.

### <a name="roles"></a>Rollen

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Feldname** | **Type**  |   **Feldname** | **Type**  |
|`Id` | Zeichenfolge | `RoleId` | Zeichenfolge
|`Name` | Zeichenfolge | `RoleName` | Zeichenfolge
|`NormalizedName` | Zeichenfolge | `LoweredRoleName` | Zeichenfolge

### <a name="user-roles"></a>Benutzerrollen

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Feldname** | **Type**  |   **Feldname** | **Type**  |
|`RoleId` | Zeichenfolge | `RoleId` | Zeichenfolge
|`UserId` | Zeichenfolge | `UserId` | Zeichenfolge

Die vorangehenden Mapping-Tabellen verweisen, für die Erstellung ein Migrationsskript für *Benutzer* und *Rollen*. Im folgende Beispiel wird davon ausgegangen, dass Sie zwei Datenbanken auf einem Datenbankserver verfügen. Eine Datenbank enthält das Schema der vorhandenen ASP.NET-Mitgliedschaft und Daten. Die andere Datenbank wurde erstellt, mit der oben beschriebenen Schritte ausführen. Kommentare sind Inlineschemainformationen enthält weitere Details.

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

Nach der Ausführung dieses Skripts wird die zuvor erstellte ASP.NET Core Identity-app mit Mitgliedschaftsbenutzern aufgefüllt. Benutzer müssen ihre Kennwörter, die vor der Anmeldung ändern.

> [!NOTE]
> Wäre das Mitgliedschaftssystem für Benutzer mit Benutzernamen, die nicht ihre e-Mail-Adresse übereinstimmt, sind Änderungen erforderlich, um die app, die dies zuvor erstellten. Die Standardvorlage erwartet `UserName` und `Email` identisch sein. Der Anmeldevorgang für Situationen, in denen sie verschiedene sind, muss geändert werden, um verwenden `UserName` anstelle von `Email`.

In der `PageModel` der Anmeldeseite, finden Sie unter *Pages\Account\Login.cshtml.cs*, entfernen Sie die `[EmailAddress]` -Attribut aus der *-e-Mail* Eigenschaft. Benennen Sie sie um *Benutzername*. Dies erfordert eine Änderung wo `EmailAddress` erwähnt ist, in der *Ansicht* und *PageModel*. Das Ergebnis sieht wie folgt aus:

 ![Fester Benutzername](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Benutzer aus der SQL-Mitgliedschaft in ASP.NET Core 2.0 Identity port. Weitere Informationen zu ASP.NET Core Identity, finden Sie unter [Einführung in die Identität](xref:security/authentication/identity).
