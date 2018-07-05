---
title: Einführung in die Identität in ASP.NET Core
author: rick-anderson
description: Verwenden Sie Identität mit einer ASP.NET Core-app. Enthält, die Einstellung kennwortanforderungen (RequireDigit, RequiredLength, RequiredUniqueChars usw.).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829301"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Einführung in die Identität in ASP.NET Core

Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity handelt es sich um ein Mitgliedschaftssystem Anmeldefunktionalität ermöglichen Ihrer Anwendung hinzufügen können. Benutzer können erstellen, ein Konto und die Anmeldung mit einem Benutzernamen und Kennwort, oder sie können einem externen Anmeldeanbieter wie z. B. Facebook, Google, Microsoft Account, Twitter oder andere.

Sie können konfigurieren, dass ASP.NET Core Identity um SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörter und Profildaten zu verwenden. Alternativ können Sie Ihre eigenen persistenten Speicher, z. B. Azure Table Storage verwenden. Dieses Dokument enthält Anweisungen für Visual Studio und zur Verwendung der Befehlszeilenschnittstelle.

[Anzeigen oder Herunterladen von Beispielcode.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Übersicht über die Identität

In diesem Thema werden Sie erfahren, wie Sie ASP.NET Core Identity zu verwenden, um das Hinzufügen von Funktionen zu registrieren, melden Sie sich, und melden Sie sich ein Benutzer. Ausführlichere Anweisungen zum Erstellen von apps mit ASP.NET Core Identity finden Sie im Abschnitt "Nächste Schritte" am Ende dieses Artikels.

1. Erstellen Sie ein Projekt für die ASP.NET Core-Webanwendung mit einzelnen Benutzerkonten.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   Wählen Sie in Visual Studio **Datei** > **neu** > **Projekt**. Wählen Sie **ASP.NET Core-Webanwendung** , und klicken Sie auf **OK**.

   ![Dialogfeld "Neues Projekt"](identity/_static/01-new-project.png)

   Wählen Sie eine ASP.NET Core **Webanwendung (Model-View-Controller)** für ASP.NET Core 2.x aus, und wählen Sie dann **Authentifizierung ändern**.

   ![Dialogfeld "Neues Projekt"](identity/_static/02-new-project.png)

   Ein Dialogfeld wird angezeigt, Angebot Authentifizierungsoptionen zur Verfügung. Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK** um zum vorherigen Dialogfeld zurückzukehren.

   ![Dialogfeld "Neues Projekt"](identity/_static/03-new-project-auth.png)

   Auswählen von **einzelne Benutzerkonten** weist Visual Studio zum Erstellen von Modellen, ViewModels, Ansichten, Controller und anderen Ressourcen, die als Teil der Projektvorlage für die Authentifizierung erforderlich.

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

   Wenn Sie .NET Core-CLI verwenden möchten, erstellen Sie das neue Projekt mit `dotnet new mvc --auth Individual`. Dieser Befehl erstellt ein neues Projekt mit der gleichen Identität Vorlage, die Visual Studio erstellt.

   Das erstellte Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Paket, das die Identitätsdaten und das Schema mit SQL Server beibehalten [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Konfigurieren der Identitätsdienste und Hinzufügen von Middleware in `Startup`.

   Die Identity-Dienste hinzugefügt werden, an die Anwendung in der `ConfigureServices` -Methode in der die `Startup` Klasse:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Diese Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).

   Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode. `UseAuthentication` Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Diese Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).

   Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode. `UseIdentity` Fügt der cookiebasierte Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).

3. Erstellen Sie einen Benutzer.

   Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.

   Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie zum Ausführen von Migrationen erforderlich sein. Die Anwendung aufgefordert, **Migrationen anwenden**. Aktualisieren Sie die Seite, falls erforderlich.

   ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)

   Alternativ können Sie die Verwendung von ASP.NET Core Identity mit Ihrer app ohne eine beständige Datenbank mithilfe einer in-Memory Database testen. Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die `Microsoft.EntityFrameworkCore.InMemory` zu Ihrer app-Paket, und Ändern Ihrer app-Aufruf von `AddDbContext` in `ConfigureServices` wie folgt:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Klickt der Benutzer die **registrieren** Link die `Register` Aktion wird aufgerufen, auf `AccountController`. Die `Register` Aktion wird den Benutzer erstellt, durch den Aufruf `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Wenn der Benutzer erfolgreich erstellt wurde, der angemeldete Benutzer wird durch den Aufruf von `_signInManager.SignInAsync`.

   **Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte, um sofortige Anmeldung bei der Registrierung zu verhindern.

4. Anmelden.

   Benutzer können sich anmelden, indem Sie auf die **melden Sie sich bei** oben auf den Link der Website, oder Sie können auf die Anmeldeseite navigiert werden, wenn sie versuchen, einen Teil der Website zugreifen, die Autorisierung erforderlich ist. Wenn der Benutzer das Formular auf die Anmeldeseite, sendet der `AccountController` `Login` Aktion aufgerufen wird.

   Die `Login` Aktionsaufrufen `PasswordSignInAsync` auf die `_signInManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   Die Basis `Controller` -Klasse macht eine `User` -Eigenschaft, die Sie von Controllermethoden aus zugreifen können. Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen treffen. Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).

5. Melden Sie sich ab.

   Auf der **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Der vorangehende Code über Aufrufe der `_signInManager.SignOutAsync` Methode. Die `SignOutAsync` Methode löscht der Ansprüche des Benutzers in einem Cookie gespeichert.

<a name="pw"></a>
6. Die Konfiguration.

   Identität verfügt über einige Standardverhaltensweisen, die in der app "Startup"-Klasse überschrieben werden können. `IdentityOptions` müssen Sie nicht konfiguriert werden, wenn Sie die Standardverhalten zu verwenden. Der folgende Code legt mehrere Kennwortoptionen Stärke fest:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Weitere Informationen zum Konfigurieren der Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).

   Den Datentyp des Primärschlüssels, kann auch Konfigurieren finden Sie unter [konfigurieren Primärschlüssel Daten Identitätstyp](xref:security/authentication/identity-primary-key-configuration).

7. Zeigen Sie die Datenbank an.

   Wenn Ihre app mit eine SQL Server-Datenbank (die Standardeinstellung auf Windows und für Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank die erstellte app anzeigen. Sie können **SQL Server Management Studio**. Wählen Sie alternativ in Visual Studio **Ansicht** > **Objekt-Explorer von SQL Server**. Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**. Die Datenbank mit dem Namen `aspnet-<name of your project>-<guid>` wird angezeigt.

   ![Über das Kontextmenü für die Datenbank-Tabelle "aspnetusers"](identity/_static/04-db.png)

   Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. "Aspnetusers"** Tabelle, und wählen Sie **Ansichtsdaten**.

8. Überprüfen, ob Identität

    Der Standardwert *ASP.NET Core-Webanwendung* -Projektvorlage kann Benutzer auf eine Aktion in der Anwendung ohne Anmeldenamen. Um sicherzustellen, dass ASP.NET Identity funktioniert, fügen eine`[Authorize]` -Attribut auf die `About` Aktion die `Home` Controller.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Führen Sie das Projekt mit **STRG** + **F5** und navigieren Sie zu der **zu** Seite. Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt ASP.NET leitet Sie zur Anmeldeseite anmelden oder registrieren.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

    Öffnen Sie ein Befehlsfenster und navigieren Sie zum Stamm des Projekts Verzeichnis mit dem `.csproj` Datei. Führen Sie die ["Dotnet run"](/dotnet/core/tools/dotnet-run) Befehl aus, um die app ausführen:

    ```csharp
    dotnet run 
    ```

    Durchsuchen Sie die URL in die Ausgabe der ["Dotnet run"](/dotnet/core/tools/dotnet-run) Befehl. Zeigen Sie die URL sollte auf `localhost` mit eine generierte Portnummer. Navigieren Sie zu der **zu** Seite. Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt ASP.NET leitet Sie zur Anmeldeseite anmelden oder registrieren.

    ---

## <a name="identity-components"></a>Identitätskomponenten

Die primäre Referenz-Assembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`. Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identity und befindet sich durch `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Diese Abhängigkeiten müssen für das Identitätssystem in ASP.NET Core-Anwendungen verwenden werden:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` : Enthält die erforderlichen Typen um die Identität mit Entity Framework Core verwenden.

* `Microsoft.EntityFrameworkCore.SqlServer` – Entity Framework Core wird von Microsoft empfohlene datenzugriffstechnologie für relationale Datenbanken wie SQL Server. Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.

## <a name="migrating-to-aspnet-core-identity"></a>Migrieren zu ASP.NET Core-Identität

Für Weitere Informationen und Anleitungen zur Migration Ihrer vorhandenen Identität finden Sie unter Speichern [Migrieren von Authentifizierungs- und Identitätseinstellungen](xref:migration/identity).

## <a name="setting-password-strength"></a>Festlegen der kennwortsicherheit

Finden Sie unter [Konfiguration](#pw) für ein Beispiel, das die Anforderungen für die Mindestlänge für Kennwörter legt diese fest.

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren von Authentifizierungs- und Identitätseinstellungen](xref:migration/identity)
* [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm)
* [Zweistufige Authentifizierung mit SMS](xref:security/authentication/2fa)
* [Facebook, Google und externe Anbieter-Authentifizierung](xref:security/authentication/social/index)
