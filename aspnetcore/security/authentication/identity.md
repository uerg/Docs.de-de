---
title: Einführung in die Identität in ASP.NET Core
author: rick-anderson
description: Verwenden Sie Identität mit einer ASP.NET Core-app. Enthält, die Einstellung kennwortanforderungen (RequireDigit, RequiredLength, RequiredUniqueChars usw.).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095638"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="cd5c7-104">Einführung in die Identität in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd5c7-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="cd5c7-105">Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cd5c7-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cd5c7-106">ASP.NET Core Identity handelt es sich um ein Mitgliedschaftssystem Anmeldefunktionalität ermöglichen Ihrer Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="cd5c7-107">Benutzer können erstellen, ein Konto und die Anmeldung mit einem Benutzernamen und Kennwort, oder sie können einem externen Anmeldeanbieter wie z. B. Facebook, Google, Microsoft Account, Twitter oder andere.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="cd5c7-108">Sie können konfigurieren, dass ASP.NET Core Identity um SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörter und Profildaten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="cd5c7-109">Alternativ können Sie Ihre eigenen persistenten Speicher, z. B. Azure Table Storage verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="cd5c7-110">Dieses Dokument enthält Anweisungen für Visual Studio und zur Verwendung der Befehlszeilenschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="cd5c7-111">Anzeigen oder Herunterladen von Beispielcode.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="cd5c7-112">(Gewusst wie: herunterladen)</span><span class="sxs-lookup"><span data-stu-id="cd5c7-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="cd5c7-113">Übersicht über die Identität</span><span class="sxs-lookup"><span data-stu-id="cd5c7-113">Overview of Identity</span></span>

<span data-ttu-id="cd5c7-114">In diesem Thema werden Sie erfahren, wie Sie ASP.NET Core Identity zu verwenden, um das Hinzufügen von Funktionen zu registrieren, melden Sie sich, und melden Sie sich ein Benutzer.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="cd5c7-115">Ausführlichere Anweisungen zum Erstellen von apps mit ASP.NET Core Identity finden Sie im Abschnitt "Nächste Schritte" am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="cd5c7-116">Erstellen Sie ein Projekt für die ASP.NET Core-Webanwendung mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd5c7-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd5c7-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="cd5c7-118">Wählen Sie in Visual Studio **Datei** > **neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="cd5c7-119">Wählen Sie **ASP.NET Core-Webanwendung** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Dialogfeld "Neues Projekt"](identity/_static/01-new-project.png)

   <span data-ttu-id="cd5c7-121">Wählen Sie eine ASP.NET Core **Webanwendung (Model-View-Controller)** für ASP.NET Core 2.x aus, und wählen Sie dann **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Dialogfeld "Neues Projekt"](identity/_static/02-new-project.png)

   <span data-ttu-id="cd5c7-123">Ein Dialogfeld wird angezeigt, Angebot Authentifizierungsoptionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="cd5c7-124">Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK** um zum vorherigen Dialogfeld zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Dialogfeld "Neues Projekt"](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="cd5c7-126">Auswählen von **einzelne Benutzerkonten** weist Visual Studio zum Erstellen von Modellen, ViewModels, Ansichten, Controller und anderen Ressourcen, die als Teil der Projektvorlage für die Authentifizierung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cd5c7-127">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="cd5c7-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="cd5c7-128">Wenn Sie .NET Core-CLI verwenden möchten, erstellen Sie das neue Projekt mit `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="cd5c7-129">Dieser Befehl erstellt ein neues Projekt mit der gleichen Identität Vorlage, die Visual Studio erstellt.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="cd5c7-130">Das erstellte Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Paket, das die Identitätsdaten und das Schema mit SQL Server beibehalten [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="cd5c7-131">Konfigurieren der Identitätsdienste und Hinzufügen von Middleware in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="cd5c7-132">Die Identity-Dienste hinzugefügt werden, an die Anwendung in der `ConfigureServices` -Methode in der die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="cd5c7-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cd5c7-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cd5c7-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="cd5c7-134">Diese Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="cd5c7-135">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="cd5c7-136">`UseAuthentication` Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cd5c7-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cd5c7-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="cd5c7-138">Diese Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="cd5c7-139">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="cd5c7-140">`UseIdentity` Fügt der cookiebasierte Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="cd5c7-141">Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="cd5c7-142">Erstellen Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-142">Create a user.</span></span>

   <span data-ttu-id="cd5c7-143">Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="cd5c7-144">Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie zum Ausführen von Migrationen erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="cd5c7-145">Die Anwendung aufgefordert, **Migrationen anwenden**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="cd5c7-146">Aktualisieren Sie die Seite, falls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-146">Refresh the page if needed.</span></span>

   ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)

   <span data-ttu-id="cd5c7-148">Alternativ können Sie die Verwendung von ASP.NET Core Identity mit Ihrer app ohne eine beständige Datenbank mithilfe einer in-Memory Database testen.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="cd5c7-149">Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die `Microsoft.EntityFrameworkCore.InMemory` zu Ihrer app-Paket, und Ändern Ihrer app-Aufruf von `AddDbContext` in `ConfigureServices` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cd5c7-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="cd5c7-150">Klickt der Benutzer die **registrieren** Link die `Register` Aktion wird aufgerufen, auf `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="cd5c7-151">Die `Register` Aktion wird den Benutzer erstellt, durch den Aufruf `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection):</span><span class="sxs-lookup"><span data-stu-id="cd5c7-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="cd5c7-152">Wenn der Benutzer erfolgreich erstellt wurde, der angemeldete Benutzer wird durch den Aufruf von `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="cd5c7-153">**Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte, um sofortige Anmeldung bei der Registrierung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="cd5c7-154">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-154">Log in.</span></span>

   <span data-ttu-id="cd5c7-155">Benutzer können sich anmelden, indem Sie auf die **melden Sie sich bei** oben auf den Link der Website, oder Sie können auf die Anmeldeseite navigiert werden, wenn sie versuchen, einen Teil der Website zugreifen, die Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="cd5c7-156">Wenn der Benutzer das Formular auf die Anmeldeseite, sendet der `AccountController` `Login` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="cd5c7-157">Die `Login` Aktionsaufrufen `PasswordSignInAsync` auf die `_signInManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="cd5c7-158">Die Basis `Controller` -Klasse macht eine `User` -Eigenschaft, die Sie von Controllermethoden aus zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="cd5c7-159">Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen treffen.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="cd5c7-160">Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="cd5c7-161">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-161">Log out.</span></span>

   <span data-ttu-id="cd5c7-162">Auf der **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="cd5c7-163">Der vorangehende Code über Aufrufe der `_signInManager.SignOutAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="cd5c7-164">Die `SignOutAsync` Methode löscht der Ansprüche des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="cd5c7-165">Die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-165">Configuration.</span></span>

   <span data-ttu-id="cd5c7-166">Identität verfügt über einige Standardverhaltensweisen, die in der app "Startup"-Klasse überschrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="cd5c7-167">`IdentityOptions` müssen Sie nicht konfiguriert werden, wenn Sie die Standardverhalten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="cd5c7-168">Der folgende Code legt mehrere Kennwortoptionen Stärke fest:</span><span class="sxs-lookup"><span data-stu-id="cd5c7-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cd5c7-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cd5c7-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cd5c7-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cd5c7-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="cd5c7-171">Weitere Informationen zum Konfigurieren der Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="cd5c7-172">Den Datentyp des Primärschlüssels, kann auch Konfigurieren finden Sie unter [konfigurieren Primärschlüssel Daten Identitätstyp](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="cd5c7-173">Zeigen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-173">View the database.</span></span>

   <span data-ttu-id="cd5c7-174">Wenn Ihre app mit eine SQL Server-Datenbank (die Standardeinstellung auf Windows und für Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank die erstellte app anzeigen.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="cd5c7-175">Sie können **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="cd5c7-176">Wählen Sie alternativ in Visual Studio **Ansicht** > **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="cd5c7-177">Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="cd5c7-178">Die Datenbank mit dem Namen `aspnet-<name of your project>-<guid>` wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Über das Kontextmenü für die Datenbank-Tabelle "aspnetusers"](identity/_static/04-db.png)

   <span data-ttu-id="cd5c7-180">Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. "Aspnetusers"** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="cd5c7-181">Überprüfen, ob Identität</span><span class="sxs-lookup"><span data-stu-id="cd5c7-181">Verify Identity works</span></span>

    <span data-ttu-id="cd5c7-182">Der Standardwert *ASP.NET Core-Webanwendung* -Projektvorlage kann Benutzer auf eine Aktion in der Anwendung ohne Anmeldenamen.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="cd5c7-183">Um sicherzustellen, dass ASP.NET Identity funktioniert, fügen eine`[Authorize]` -Attribut auf die `About` Aktion die `Home` Controller.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd5c7-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd5c7-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="cd5c7-185">Führen Sie das Projekt mit **STRG** + **F5** und navigieren Sie zu der **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="cd5c7-186">Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt ASP.NET leitet Sie zur Anmeldeseite anmelden oder registrieren.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cd5c7-187">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="cd5c7-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="cd5c7-188">Öffnen Sie ein Befehlsfenster und navigieren Sie zum Stamm des Projekts Verzeichnis mit dem `.csproj` Datei.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="cd5c7-189">Führen Sie die ["Dotnet run"](/dotnet/core/tools/dotnet-run) Befehl aus, um die app ausführen:</span><span class="sxs-lookup"><span data-stu-id="cd5c7-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="cd5c7-190">Durchsuchen Sie die URL in die Ausgabe der ["Dotnet run"](/dotnet/core/tools/dotnet-run) Befehl.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="cd5c7-191">Zeigen Sie die URL sollte auf `localhost` mit eine generierte Portnummer.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="cd5c7-192">Navigieren Sie zu der **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-192">Navigate to the **About** page.</span></span> <span data-ttu-id="cd5c7-193">Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt ASP.NET leitet Sie zur Anmeldeseite anmelden oder registrieren.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="cd5c7-194">Identitätskomponenten</span><span class="sxs-lookup"><span data-stu-id="cd5c7-194">Identity Components</span></span>

<span data-ttu-id="cd5c7-195">Die primäre Referenz-Assembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="cd5c7-196">Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identity und befindet sich durch `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="cd5c7-197">Diese Abhängigkeiten müssen für das Identitätssystem in ASP.NET Core-Anwendungen verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="cd5c7-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="cd5c7-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` : Enthält die erforderlichen Typen um die Identität mit Entity Framework Core verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="cd5c7-199">`Microsoft.EntityFrameworkCore.SqlServer` – Entity Framework Core wird von Microsoft empfohlene datenzugriffstechnologie für relationale Datenbanken wie SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="cd5c7-200">Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="cd5c7-201">`Microsoft.AspNetCore.Authentication.Cookies` -Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="cd5c7-202">Migrieren zu ASP.NET Core-Identität</span><span class="sxs-lookup"><span data-stu-id="cd5c7-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="cd5c7-203">Für Weitere Informationen und Anleitungen zur Migration Ihrer vorhandenen Identität finden Sie unter Speichern [Migrieren von Authentifizierungs- und Identitätseinstellungen](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="cd5c7-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="cd5c7-204">Festlegen der kennwortsicherheit</span><span class="sxs-lookup"><span data-stu-id="cd5c7-204">Setting password strength</span></span>

<span data-ttu-id="cd5c7-205">Finden Sie unter [Konfiguration](#pw) für ein Beispiel, das die Anforderungen für die Mindestlänge für Kennwörter legt diese fest.</span><span class="sxs-lookup"><span data-stu-id="cd5c7-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd5c7-206">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="cd5c7-206">Next Steps</span></span>

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
