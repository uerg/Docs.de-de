---
title: "Einführung in die Identität auf ASP.NET Core"
author: rick-anderson
description: "Verwenden von Identität mit einer ASP.NET Core-app"
keywords: "ASP.NET Core, Identität, Autorisierung, Sicherheit"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0c17daa96bc69dc0b8393811a4dfe0e5dc4a1884
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a32b4-104">Einführung in die Identität auf ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a32b4-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="a32b4-105">Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a32b4-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a32b4-106">ASP.NET Core Identität ist eine Mitgliedschaftssystem Anmeldefunktionalität für Ihre Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="a32b4-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="a32b4-107">Benutzer können ein Konto und die Anmeldung mit einem Benutzernamen erstellen und Kennwort, oder sie können einen externen Anmeldeanbieter wie Facebook, Google, Microsoft Account, Twitter oder anderen verwenden.</span><span class="sxs-lookup"><span data-stu-id="a32b4-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="a32b4-108">Sie können ASP.NET Core Identity Verwendung eine SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörtern und Profildaten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a32b4-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a32b4-109">Alternativ können Sie eigene permanenten Speicher zu, z. B. Azure-Tabellenspeicher verwenden.</span><span class="sxs-lookup"><span data-stu-id="a32b4-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="a32b4-110">Dieses Dokument enthält Anleitungen für Visual Studio und mithilfe der CLI.</span><span class="sxs-lookup"><span data-stu-id="a32b4-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="a32b4-111">Übersicht über die Identität</span><span class="sxs-lookup"><span data-stu-id="a32b4-111">Overview of Identity</span></span>

<span data-ttu-id="a32b4-112">In diesem Thema werden Sie erfahren, wie ASP.NET Core Identity zu verwenden, um Funktionen zu registrieren, melden Sie sich hinzufügen und Abmelden eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="a32b4-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="a32b4-113">Ausführlichere Anweisungen zum Erstellen von apps mithilfe von ASP.NET Core Identity finden Sie unter dem Abschnitt "Nächste Schritte" am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="a32b4-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="a32b4-114">Erstellen Sie ein ASP.NET Core-Webanwendungsprojekt mit einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="a32b4-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a32b4-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a32b4-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="a32b4-116">Wählen Sie in Visual Studio **Datei** -> **neu** -> **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="a32b4-117">Wählen Sie die **ASP.NET-Webanwendung** aus der **neues Projekt** (Dialogfeld).</span><span class="sxs-lookup"><span data-stu-id="a32b4-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="a32b4-118">Auswählen einer ASP.NET Core **Webanwendung** mit **einzelne Benutzerkonten** als Authentifizierungsmethode.</span><span class="sxs-lookup"><span data-stu-id="a32b4-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="a32b4-119">Hinweis: Sie müssen auswählen, **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Dialogfeld "Neues Projekt"](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a32b4-121">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a32b4-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="a32b4-122">Wenn Sie die .NET Core-Befehlszeilenschnittstelle verwenden zu können, erstellen Sie das neue Projekt mit ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="a32b4-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="a32b4-123">Dadurch wird ein neues Projekt mit der gleichen Identität Vorlagencode erstellt, die Visual Studio erstellt.</span><span class="sxs-lookup"><span data-stu-id="a32b4-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="a32b4-124">Das Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Paket, d. h. die Identitätsdaten und das Schema zur Verwendung von SQL Server beibehalten wird [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="a32b4-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="a32b4-125">Konfigurieren von Identitätsdiensten und Hinzufügen von Middleware in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a32b4-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="a32b4-126">Die Identity-Dienste werden hinzugefügt, an die Anwendung in der `ConfigureServices` Methode in der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="a32b4-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a32b4-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a32b4-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="a32b4-128">Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a32b4-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="a32b4-129">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="a32b4-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="a32b4-130">`UseAuthentication`Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a32b4-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a32b4-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a32b4-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="a32b4-132">Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a32b4-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="a32b4-133">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="a32b4-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="a32b4-134">`UseIdentity`Fügt der Cookie-basierte Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a32b4-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="a32b4-135">Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a32b4-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="a32b4-136">Erstellen Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a32b4-136">Create a user.</span></span>

    <span data-ttu-id="a32b4-137">Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.</span><span class="sxs-lookup"><span data-stu-id="a32b4-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="a32b4-138">Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie für die Ausführung von Migrationen erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="a32b4-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="a32b4-139">Die Anwendung aufgefordert, **gelten Migrationen**:</span><span class="sxs-lookup"><span data-stu-id="a32b4-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="a32b4-141">Alternativ können Sie testen, mithilfe von ASP.NET Core Identity mit Ihrer app ohne einen persistenten Datenbank mithilfe einer in-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a32b4-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="a32b4-142">Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die ``Microsoft.EntityFrameworkCore.InMemory`` auf Ihre app Packen und ändern Sie Ihre app Aufruf ``AddDbContext`` in ``ConfigureServices`` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="a32b4-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="a32b4-143">Wenn der Benutzer klickt der **registrieren** Link, der ``Register`` Aktion wird aufgerufen, auf ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="a32b4-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="a32b4-144">Die ``Register`` Aktion erstellt Benutzer durch Aufrufen von `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion):</span><span class="sxs-lookup"><span data-stu-id="a32b4-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="a32b4-145">Wenn der Benutzer erfolgreich erstellt wurde, wird der Anmeldung des Benutzers durch den Aufruf von ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="a32b4-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="a32b4-146">**Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte zum sofortigen Anmeldung bei der Registrierung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="a32b4-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="a32b4-147">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="a32b4-147">Log in.</span></span>
 
    <span data-ttu-id="a32b4-148">Benutzer können sich anmelden, indem Sie auf die **melden Sie sich** Link am oberen Rand der Website oder möglicherweise der Anmeldeseite navigiert werden, wenn Benutzer versuchen, ein Teil des Standorts, auf dem Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a32b4-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="a32b4-149">Wenn der Benutzer das Formular auf der Anmeldeseite sendet die ``AccountController`` ``Login`` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a32b4-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="a32b4-150">Die ``Login`` Aktion Aufrufe ``PasswordSignInAsync`` auf die ``_signInManager`` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion).</span><span class="sxs-lookup"><span data-stu-id="a32b4-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="a32b4-151">Die Basis ``Controller`` Klasse macht ein ``User`` -Eigenschaft, die Sie über Controllermethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="a32b4-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="a32b4-152">Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen.</span><span class="sxs-lookup"><span data-stu-id="a32b4-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a32b4-153">Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="a32b4-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="a32b4-154">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="a32b4-154">Log out.</span></span>
 
    <span data-ttu-id="a32b4-155">Klicken auf die **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.</span><span class="sxs-lookup"><span data-stu-id="a32b4-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="a32b4-156">Der vorangehende Code oben Aufrufe der `_signInManager.SignOutAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="a32b4-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="a32b4-157">Die `SignOutAsync` Methode löscht den Ansprüchen des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a32b4-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="a32b4-158">Die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a32b4-158">Configuration.</span></span>

    <span data-ttu-id="a32b4-159">Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="a32b4-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="a32b4-160">Sie müssen nicht so konfigurieren Sie ``IdentityOptions`` Wenn Sie die Standardverhalten verwenden.</span><span class="sxs-lookup"><span data-stu-id="a32b4-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a32b4-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a32b4-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a32b4-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a32b4-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="a32b4-163">Weitere Informationen zum Konfigurieren von Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="a32b4-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="a32b4-164">Sie können zudem konfigurieren den Datentyp des Primärschlüssels, finden Sie unter [konfigurieren Identität Primärschlüssel-Datentyp](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="a32b4-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="a32b4-165">Zeigen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="a32b4-165">View the database.</span></span>

    <span data-ttu-id="a32b4-166">Wenn Ihre app auf eine SQL Server-Datenbank (Standard unter Windows und Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank der app erstellt anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a32b4-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="a32b4-167">Sie können **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="a32b4-168">Wählen Sie alternativ in Visual Studio **Ansicht** -> **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="a32b4-169">Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="a32b4-170">Die Datenbank mit dem Namen * *Aspnet - <*Name Ihres Projekts*>-<*Datumszeichenfolge*> ** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a32b4-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Kontextmenü für AspNetUsers-Datenbanktabelle](identity/_static/04-db.png)
    
    <span data-ttu-id="a32b4-172">Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. AspNetUsers** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="a32b4-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="a32b4-173">Identity-Komponenten</span><span class="sxs-lookup"><span data-stu-id="a32b4-173">Identity Components</span></span>

<span data-ttu-id="a32b4-174">Die primäre Verweisassembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="a32b4-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="a32b4-175">Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identität und ist durch enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a32b4-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="a32b4-176">Diese Abhängigkeiten sind erforderlich, um das Identitätssystem in ASP.NET Core-Anwendungen verwenden:</span><span class="sxs-lookup"><span data-stu-id="a32b4-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="a32b4-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Enthält die erforderlichen Typen um Identität mit Entity Framework Core verwenden.</span><span class="sxs-lookup"><span data-stu-id="a32b4-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="a32b4-178">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core ist Microsofts empfohlene datenzugriffstechnologie für relationale Datenbanken, wie z. B. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a32b4-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="a32b4-179">Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="a32b4-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="a32b4-180">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="a32b4-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a32b4-181">Migrieren zu ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="a32b4-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a32b4-182">Weitere Informationen und Anweisungen zur Migration Ihrer vorhandenen Identität Filiale finden Sie unter [Migrieren von Authentifizierung und Identität](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a32b4-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a32b4-183">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a32b4-183">Next Steps</span></span>

* [<span data-ttu-id="a32b4-184">Migrating Authentication and Identity (Migration von Authentifizierungs- und Identitätseinstellungen)</span><span class="sxs-lookup"><span data-stu-id="a32b4-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="a32b4-185">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="a32b4-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a32b4-186">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="a32b4-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="a32b4-187">Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern</span><span class="sxs-lookup"><span data-stu-id="a32b4-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
