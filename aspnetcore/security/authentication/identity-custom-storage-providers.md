---
title: "Benutzerdefinierte Speicheranbieter für ASP.NET Core Identity | Microsoft Docs"
author: ardalis
description: "Wie Sie benutzerdefinierte Speicheranbieter für ASP.NET Core Identity zu konfigurieren."
keywords: "ASP.NET Core, Identität und benutzerdefinierte Speicheranbieter"
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 33d02f595ff34a297d3046894edc8a6f5bd42275
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Benutzerdefinierte Speicheranbieter für ASP.NET Core Identität

Durch [Steve Smith](http://ardalis.com)

ASP.NET Core Identität ist ein erweiterbares System können Sie zum Erstellen eines benutzerdefinierten Speicheranbieters und verbinden Sie ihn mit Ihrer app. Dieses Thema beschreibt das Erstellen eines benutzerdefinierten Speicheranbieters für ASP.NET Core Identität. Wichtige Konzepte zum Erstellen eigener Speicheranbieter, aber eine schrittweise exemplarische Vorgehensweise ist.

[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Einführung

Standardmäßig speichert die ASP.NET-Identität-Core-System Benutzerinformationen in einer SQL Server-Datenbank über Entity Framework Core. Für viele apps eignet sich dieser Ansatz gut. Möglicherweise bevorzugen Sie jedoch einen anderen Dauerhaftigkeit oder Datenschema verwenden. Zum Beispiel:

* Verwenden Sie [Azure Table Storage](https://docs.microsoft.com/azure/storage/) oder einen anderen Datenspeicher.
* Die Datenbanktabellen haben eine andere Struktur. 
* Sie möchten einen unterschiedlichen Zugriff Ansatz verwenden, z. B. [dapper, durch](https://github.com/StackExchange/Dapper). 

In jedem dieser Fälle können einen benutzerdefinierten Anbieter für Ihre Speichermechanismus schreiben und schließen Sie diesen Anbieter in Ihrer app.

ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.

Wenn Sie die .NET Core-Befehlszeilenschnittstelle verwenden zu können, fügen `-au Individual`:

```
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Die ASP.NET Core Identity-Architektur

ASP.NET Core Identity besteht aus Klassen, die aufgerufen wird, Manager und speichert. *Manager* sind allgemeine Klassen, die app-Entwickler verwendet wird, um die Vorgänge, z. B. Erstellen einer Identität eines Benutzers ausführen. *Speichert* sind auf niedrigerer Ebene Klassen, die angeben, wie die Entitäten, z. B. Benutzer und Rollen, beibehalten werden. Speichert befolgen Sie die [Repositorymusters](http://deviq.com/repository-pattern/) und sind eng gekoppelt mit der Persistenz-Mechanismus. Aus Datenspeichern, was bedeutet, dass Sie die Dauerhaftigkeit ersetzen können, ohne Änderung des Codes der Anwendung (mit Ausnahme der Konfiguration) Manager entkoppelt.

Das folgende Diagramm zeigt, wie eine Web-app mit Managern, interagiert, während der Speicher mit der Datenzugriffsebene interagieren.

![ASP.NET Core-Apps funktionieren mit-Managern (z. B. "UserManager", "RoleManager"). -Manager funktionieren mit speichert (z. B. "UserStore") die Kommunikation mit einer Datenquelle mit einer wie Entity Framework Core-Bibliothek.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Erstellen Sie zum Erstellen eines benutzerdefinierten Speicheranbieters der Datenquelle, die Datenzugriffsebene und die Store-Klassen, die Interaktion mit diesem Datenzugriffsebene (die grünen und graue Felder in der Abbildung oben). Sie müssen nicht die Managers "oder" app-Code, der mit ihnen (die blaue Felder oben) interagiert anpassen.

Beim Erstellen einer neuen Instanz des `UserManager` oder `RoleManager` Geben Sie den Typ des User-Klasse und eine Instanz der Klasse Store als Argument übergeben. Dieser Ansatz können Sie Ihren benutzerdefinierten Klassen in ASP.NET Core einbinden. 

[Konfigurieren Sie die app aus, um neue Speicheranbieter verwenden](#reconfigure-app-to-use-new-storage-provider) wird gezeigt, wie instanziieren `UserManager` und `RoleManager` mit einem benutzerdefinierten Speicher.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identität speichert-Datentypen

[ASP.NET Core Identity](https://github.com/aspnet/identity) Datentypen werden in den folgenden Abschnitten beschrieben:

### <a name="users"></a>Benutzer

Registrierte Benutzer Ihrer Website. Die [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) Typ erweitert oder als ein Beispiel für einen eigenen benutzerdefinierten Typ verwendet werden kann. Sie müssen nicht erben von einem bestimmten Typ implementieren Ihre eigenen benutzerdefinierten Identität speicherlösung.

### <a name="user-claims"></a>Benutzeransprüche

Eine Reihe von Anweisungen (oder [Ansprüche](https://msdn.microsoft.com/library/system.security.claims.claim(v=vs.110).aspx)) über den Benutzer, die die Identität des Benutzers darstellen. Kann größer Ausdruck, der die Identität des Benutzers als über Rollen erzielt werden kann.

### <a name="user-logins"></a>Benutzernamen

Informationen zu den externen Authentifizierungsanbieter (z. B. Facebook oder ein Microsoft-Konto) verwenden, wenn Sie einen Benutzer anmelden. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Rollen

Eine Autorisierungsgruppe für Ihre Website. Enthält den Rollennamen-Id und die Rolle (z. B. "Admin" oder "Employee"). [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Die Datenzugriffsebene

In diesem Thema wird davon ausgegangen, dass Sie mit der Persistenz-Mechanismus, den Sie verwenden möchten, und zum Erstellen von Entitäten für diesen Mechanismus vertraut sind. Dieses Thema bietet detaillierte Informationen zum Erstellen des Repositorys oder Datenzugriffsklassen; Es enthält einige Vorschläge für die entwurfsentscheidungen beim Arbeiten mit ASP.NET Core Identity.

Beim Entwerfen eines benutzerdefinierten Speicheranbieters der Datenzugriffsebene, stehen Ihnen viel Flexibilität. Sie müssen nur Persistenz Mechanismen für die Funktionen erstellen, die Sie in Ihrer app verwenden möchten. Beispielsweise, wenn Sie keine Rollen in Ihrer app verwenden, müssen nicht Sie Speicher für Rollen oder Zuordnungen zwischen Benutzer und Rollen erstellen. Die Technologie und die vorhandene Infrastruktur möglicherweise eine Struktur, die die standardmäßige Implementierung des ASP.NET Core Identity erheblich unterscheiden. In der Datenzugriffsebene Geben Sie die Logik zum Arbeiten mit der Struktur Ihrer Speicher-Implementierung.

Die Datenzugriffsebene enthält die Logik zum Speichern der Daten von ASP.NET Core Identität mit einer Datenquelle. Die Datenzugriffsebene für Ihre benutzerdefinierten Speicheranbieter kann die folgenden Klassen zum Speichern von Benutzer-und Rolle enthalten.

### <a name="context-class"></a>Context-Klasse

Kapselt die Informationen zum Herstellen einer Verbindung mit Ihrem Dauerhaftigkeit und Ausführen von Abfragen. Mehrere Datenklassen erfordern eine Instanz dieser Klasse, die in der Regel über Abhängigkeitsinjektion bereitgestellt. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Benutzerspeicher

Speichert und ruft Benutzerinformationen (z. B. Benutzer und Kennwort-Hash) ab. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Rolle Speicher

Speichert Rolleninformationen und abruft (z. B. die Rolle ""). [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Speicherung von userclaims.

Speichert und Anspruch Benutzerinformationen (z. B. den Anspruchstyp und-Wert) abruft. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Speicherung von userlogins.

Speichert und ruft die Benutzer-Anmeldeinformationen (z. B. eines externen Authentifizierungsanbieters) ab. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Speicher der Benutzerrollen

Speichert und abruft, welche Benutzer die Rollen zugewiesen sind. [Beispiel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Tipp:** implementieren nur die Klassen, die Sie in Ihrer app verwenden möchten.

Geben Sie in der Datenzugriffsklassen Code zum Ausführen von Datenvorgängen für die Dauerhaftigkeit. Beispielsweise in einen benutzerdefinierten Anbieter möglicherweise, müssen Sie den folgenden Code zum Erstellen eines neuen Benutzers in der *speichern* Klasse:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Die Implementierungslogik zum Erstellen des Benutzers befindet sich in der ``_usersTable.CreateAsync`` unten gezeigte Methode.

## <a name="customize-the-user-class"></a>Anpassen der User-Klasse

Wenn Sie einen Speicheranbieter zu implementieren, erstellen Sie eine Benutzerklasse entspricht dem [ `IdentityUser` Klasse](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

User-Klasse umfasst mindestens eine `Id` und ein `UserName` Eigenschaft.

Die `IdentityUser` Klasse definiert die Eigenschaften, die die ``UserManager`` wird aufgerufen, wenn das Ausführen von angeforderten Vorgänge. Der Standardtyp der `Id` -Eigenschaft ist eine Zeichenfolge, aber Sie können die erben von `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` , und geben Sie einen anderen Typ. Das Framework erwartet, dass der Speicher-Implementierung, die datentypkonvertierungen zu behandeln.

## <a name="customize-the-user-store"></a>Passen Sie den Speicher des Benutzers

Erstellen Sie eine `UserStore` -Klasse, die die Methoden für alle Vorgänge für den Benutzer bereitstellt. Diese Klasse entspricht dem [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) Klasse. In Ihrem `UserStore` -Klasse, implementieren Sie `IUserStore<TUser>` und die optionalen Schnittstellen erforderlich. Sie wählen Sie die optionale Schnittstellen implementiert die Funktionalität in Ihrer app auf Grundlage.

### <a name="optional-interfaces"></a>Optionale Schnittstellen

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Die optionale Schnittstellen erben von `IUserStore`. Sehen Sie eine teilweise implementierte beispielbenutzers speichern [hier](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

Innerhalb der `UserStore` -Klasse, verwenden Sie die Access-Datenklassen, die Sie erstellt haben, um Vorgänge auszuführen. Diese werden mithilfe der Abhängigkeitsinjektion übergeben. Beispielsweise ist in SQL Server mit Dapper Implementierung der `UserStore` -Klasse verfügt über die `CreateAsync` Methode, die eine Instanz der verwendet `DapperUsersTable` , einen neuen Datensatz einzufügen:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Schnittstellen implementiert werden, wenn Benutzerspeicher anpassen

- **IUserStore**  
 Die [IUserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) -Schnittstelle ist die einzige müssen Sie in den Speicher des Benutzers implementieren. Definiert Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Benutzern.
- **IUserClaimStore**  
 Die [IUserClaimStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie zum Aktivieren von benutzeransprüchen implementieren. Enthält Methoden zum Hinzufügen, entfernen und das Abrufen von benutzeransprüchen.
- **IUserLoginStore**  
 Die [IUserLoginStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiert die Methoden, die Sie implementieren, um externe Authentifizierungsanbieter zu aktivieren. Enthält Methoden zum Hinzufügen, entfernen und Abrufen von benutzeranmeldungen und eine Methode zum Abrufen von einem Benutzer basierend auf der Anmeldeinformationen.
- **IUserRoleStore**  
 Die [IUserRoleStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um die Zuordnung von Benutzern zu einer Rolle. Enthält Methoden zum Hinzufügen, entfernen und Abrufen von Rollen eines Benutzers und eine Methode zum Überprüfen, ob ein Benutzer einer Rolle zugewiesen ist.
- **IUserPasswordStore**  
 Die [IUserPasswordStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um verschlüsselte Kennwörter beibehalten werden. Enthält Methoden zum Abrufen und festlegen, das verschlüsselte Kennwort und eine Methode, die angibt, ob der Benutzer ein Kennwort festgelegt wurde.
- **IUserSecurityStampStore**  
 Die [IUserSecurityStampStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um Sie verwenden einen sicherheitsstempel für den, der angibt, ob die Benutzerkontoinformationen geändert hat. Dieser Zeitstempel wird aktualisiert, wenn ein Benutzer das Kennwort ändert oder hinzufügt oder entfernt Anmeldungen. Enthält Methoden zum Abrufen und Festlegen der sicherheitsstempel.
- **IUserTwoFactorStore**  
 Die [IUserTwoFactorStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um die zweistufige Authentifizierung unterstützen. Enthält Methoden zum Abrufen und festlegen, ob zweistufige Authentifizierung für einen Benutzer aktiviert ist.
- **IUserPhoneNumberStore**  
 Die [IUserPhoneNumberStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um Telefonnummern des Benutzers gespeichert. Enthält Methoden zum Abrufen und Festlegen der Telefonnummer und gibt an, ob die Telefonnummer bestätigt ist.
- **IUserEmailStore**  
 Die [IUserEmailStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um e-Mail-Adressen von Benutzern zu speichern. Enthält Methoden zum Abrufen und festlegen, die e-Mail-Adresse und gibt an, ob die e-Mail bestätigt ist.
- **IUserLockoutStore**  
 Die [IUserLockoutStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, um Informationen zum Sperren eines Kontos zu speichern. Enthält Methoden zum Nachverfolgen von zugriffsversuchsfehlern und sperren.
- **IQueryableUserStore**  
 Die [IQueryableUserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) Schnittstelle definiert den Member implementieren, um einen abfragbaren benutzerpeicher bereitzustellen.

Implementieren Sie nur die Schnittstellen, die erforderlich sind, in Ihrer app. Zum Beispiel:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin und IdentityUserRole

Die ``Microsoft.AspNet.Identity.EntityFramework`` Namespace enthält die Implementierung der [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), und [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) Klassen. Wenn Sie diese Funktionen verwenden, empfiehlt es sich, eine eigene Versionen dieser Klassen erstellen und definieren Sie die Eigenschaften für Ihre app. Manchmal ist es jedoch effizienter, diese Entitäten nicht in den Arbeitsspeicher geladen, beim Ausführen von grundlegenden Vorgänge (z. B. hinzufügen oder Entfernen eines Benutzers Anspruch). Stattdessen können die Back-End-Speicher-Klassen diese Vorgänge direkt auf die Datenquelle ausgeführt werden. Z. B. die ``UserStore.GetClaimsAsync`` -Methodenaufruf können die ``userClaimTable.FindByUserId(user.Id)`` Methode zum Ausführen einer Abfrage auf, die Tabelle direkt und eine Liste von Ansprüchen zurückgeben.

## <a name="customize-the-role-class"></a>Anpassen der Rolle ""-Klasse

Beim Implementieren eines Rollenanbieters für den Speicher können Sie eine benutzerdefinierte Rollentyp erstellen. Sie müssen eine bestimmte Schnittstelle nicht implementieren, muss jedoch ein `Id` und in der Regel muss ein `Name` Eigenschaft.

Im folgenden finden eine Beispiel-rollenklasse:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Anpassen der rollenspeicher

Sie erstellen eine ``RoleStore`` -Klasse, die die Methoden für alle Vorgänge für Rollen bereitstellt. Diese Klasse entspricht dem [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) Klasse. In der `RoleStore` -Klasse, die Sie implementieren die ``IRoleStore<TRole>`` und optional die ``IQueryableRoleStore<TRole>`` Schnittstelle.

- **IRoleStore&lt;TRole&gt;**  
 Die [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) Schnittstelle definiert die Methoden, die in der Rolle Store-Klasse implementiert. Enthält Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Rollen.
- **RoleStore&lt;TRole&gt;**  
 Anpassen `RoleStore`, erstellen Sie eine Klasse, die implementiert die `IRoleStore` Schnittstelle. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Konfigurieren Sie die app aus, um neue Speicheranbieter verwenden

Nachdem Sie einen Speicheranbieter implementiert haben, konfigurieren Sie Ihre app zur Verwendung an. Wenn Ihre app den Standardanbieter verwendet haben, ersetzen Sie ihn durch einen benutzerdefinierten Anbieter aus.

1. Entfernen Sie die `Microsoft.AspNetCore.EntityFramework.Identity` NuGet-Paket.
1. Wenn Sie der Speicheranbieter in einem separaten Projekt oder Paket befindet, fügen Sie einen Verweis darauf hinzu.
1. Ersetzen Sie alle Verweise auf `Microsoft.AspNetCore.EntityFramework.Identity` mit einer using-Anweisung für den Namespace von Ihrem Speicheranbieter.
1. In der ``ConfigureServices`` -Methode, ändern die `AddIdentity` Methode, um die benutzerdefinierten Typen verwenden. Sie können Ihre eigenen Erweiterungsmethoden für diesen Zweck erstellen. Finden Sie unter [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) ein Beispiel.
1. Wenn Sie Rollen verwenden, Aktualisieren der `RoleManager` verwenden Ihre `RoleStore` Klasse.
1. Aktualisieren Sie die Verbindungszeichenfolge sowie Anmeldeinformationen auf Ihre app-Konfiguration.

Beispiel:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Verweise

- [Benutzerdefinierte Speicheranbieter für ASP.NET Identity](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -dieses Repository enthält Links zu Community-Speicheranbieter verwaltet.