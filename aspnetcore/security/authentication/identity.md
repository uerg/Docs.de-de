---
title: "Einführung in die Identität auf ASP.NET Core"
author: rick-anderson
description: "Verwenden von Identität mit einer ASP.NET Core-app"
keywords: "ASP.NET Core, Identität, Autorisierung, Sicherheit"
ms.author: riande
manager: wpickett
ms.date: 7/7/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 5a76cac1d64718b9dece3a3201db06c8192fb6f3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Einführung in die Identität auf ASP.NET Core

Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](https://ardalis.com/)

ASP.NET Core Identität ist eine Mitgliedschaftssystem Anmeldefunktionalität für Ihre Anwendung hinzufügen können. Benutzer können ein Konto und die Anmeldung mit einem Benutzernamen erstellen und Kennwort, oder sie können einen externen Anmeldeanbieter wie Facebook, Google, Microsoft Account, Twitter oder anderen verwenden.

Sie können ASP.NET Core Identity Verwendung eine SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörtern und Profildaten konfigurieren. Alternativ können Sie eigene permanenten Speicher zu, z. B. Azure-Tabellenspeicher verwenden. Dieses Dokument enthält Anleitungen für Visual Studio und mithilfe der CLI.

## <a name="overview-of-identity"></a>Übersicht über die Identität

In diesem Thema werden Sie erfahren, wie ASP.NET Core Identity zu verwenden, um Funktionen zu registrieren, melden Sie sich hinzufügen und Abmelden eines Benutzers. Ausführlichere Anweisungen zum Erstellen von apps mithilfe von ASP.NET Core Identity finden Sie unter dem Abschnitt "Nächste Schritte" am Ende dieses Artikels.

1.  Erstellen Sie ein ASP.NET Core-Webanwendungsprojekt mit einzelnen Benutzerkonten an.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    Wählen Sie in Visual Studio **Datei** -> **neu** -> **Projekt**. Wählen Sie die **ASP.NET-Webanwendung** aus der **neues Projekt** (Dialogfeld). Auswählen einer ASP.NET Core **Webanwendung** mit **einzelne Benutzerkonten** als Authentifizierungsmethode.

    Hinweis: Sie müssen auswählen, **einzelne Benutzerkonten**.
 
    ![Dialogfeld "Neues Projekt"](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)
    Wenn Sie die .NET Core-Befehlszeilenschnittstelle verwenden zu können, erstellen Sie das neue Projekt mit ``dotnet new mvc --auth Individual``. Dadurch wird ein neues Projekt mit der gleichen Identität Vorlagencode erstellt, die Visual Studio erstellt.
 
    Das Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Paket, d. h. die Identitätsdaten und das Schema zur Verwendung von SQL Server beibehalten wird [Entity Framework Core](https://docs.microsoft.com/ef/).
    
    ---
 
2.  Konfigurieren von Identitätsdiensten und Hinzufügen von Middleware in `Startup`.

    Die Identity-Dienste werden hinzugefügt, an die Anwendung in der `ConfigureServices` Methode in der `Startup` Klasse:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).
    
    Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode. `UseAuthentication`Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).
    
    Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode. `UseIdentity`Fügt der Cookie-basierte Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).

3.  Erstellen Sie einen Benutzer.

    Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.

    Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie für die Ausführung von Migrationen erforderlich sein. Die Anwendung aufgefordert, **gelten Migrationen**:
    
    ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)
    
    Alternativ können Sie testen, mithilfe von ASP.NET Core Identity mit Ihrer app ohne einen persistenten Datenbank mithilfe einer in-Memory-Datenbank. Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die ``Microsoft.EntityFrameworkCore.InMemory`` auf Ihre app Packen und ändern Sie Ihre app Aufruf ``AddDbContext`` in ``ConfigureServices`` wie folgt:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Wenn der Benutzer klickt der **registrieren** Link, der ``Register`` Aktion wird aufgerufen, auf ``AccountController``. Die ``Register`` Aktion erstellt Benutzer durch Aufrufen von `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Wenn der Benutzer erfolgreich erstellt wurde, wird der Anmeldung des Benutzers durch den Aufruf von ``_signInManager.SignInAsync``.

    **Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte zum sofortigen Anmeldung bei der Registrierung zu verhindern.
 
4.  Anmelden.
 
    Benutzer können sich anmelden, indem Sie auf die **melden Sie sich** Link am oberen Rand der Website oder möglicherweise der Anmeldeseite navigiert werden, wenn Benutzer versuchen, ein Teil des Standorts, auf dem Autorisierung erforderlich ist. Wenn der Benutzer das Formular auf der Anmeldeseite sendet die ``AccountController`` ``Login`` Aktion aufgerufen wird.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    Die ``Login`` Aktion Aufrufe ``PasswordSignInAsync`` auf die ``_signInManager`` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion).
 
    Die Basis ``Controller`` Klasse macht ein ``User`` -Eigenschaft, die Sie über Controllermethoden zugreifen können. Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen. Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).
 
5.  Melden Sie sich ab.
 
    Klicken auf die **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Der vorangehende Code oben Aufrufe der `_signInManager.SignOutAsync` Methode. Die `SignOutAsync` Methode löscht den Ansprüchen des Benutzers in einem Cookie gespeichert.
 
6.  Die Konfiguration.

    Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse überschreiben können. Sie müssen nicht so konfigurieren Sie ``IdentityOptions`` Wenn Sie die Standardverhalten verwenden.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Weitere Informationen zum Konfigurieren von Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).
    
    Sie können zudem konfigurieren den Datentyp des Primärschlüssels, finden Sie unter [konfigurieren Identität Primärschlüssel-Datentyp](xref:security/authentication/identity-primary-key-configuration).
 
7.  Zeigen Sie die Datenbank an.

    Wenn Ihre app auf eine SQL Server-Datenbank (Standard unter Windows und Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank der app erstellt anzeigen. Sie können **SQL Server Management Studio**. Wählen Sie alternativ in Visual Studio **Ansicht** -> **Objekt-Explorer von SQL Server**. Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**. Die Datenbank mit dem Namen * *Aspnet - <*Name Ihres Projekts*>-<*Datumszeichenfolge*> ** wird angezeigt.

    ![Kontextmenü für AspNetUsers-Datenbanktabelle](identity/_static/04-db.png)
    
    Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. AspNetUsers** Tabelle, und wählen Sie **Ansichtsdaten**.

## <a name="identity-components"></a>Identity-Komponenten

Die primäre Verweisassembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`. Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identität und ist durch enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Diese Abhängigkeiten sind erforderlich, um das Identitätssystem in ASP.NET Core-Anwendungen verwenden:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Enthält die erforderlichen Typen um Identität mit Entity Framework Core verwenden.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core ist Microsofts empfohlene datenzugriffstechnologie für relationale Datenbanken, wie z. B. SQL Server. Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.

## <a name="migrating-to-aspnet-core-identity"></a>Migrieren zu ASP.NET Core Identität

Weitere Informationen und Anweisungen zur Migration Ihrer vorhandenen Identität Filiale finden Sie unter [Migrieren von Authentifizierung und Identität](xref:migration/identity).

## <a name="next-steps"></a>Nächste Schritte

* [Migrating Authentication and Identity (Migration von Authentifizierungs- und Identitätseinstellungen)](xref:migration/identity)
* [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm)
* [Zweistufige Authentifizierung mit SMS](xref:security/authentication/2fa)
* [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](xref:security/authentication/social/index)
