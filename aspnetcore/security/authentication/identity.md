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
ms.openlocfilehash: 5718336868f3ee5ab08162ae2bc885c695d19a1d
ms.sourcegitcommit: f3366461010da37981cf7fc092b9b9613eb4ca89
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="e2c22-104">Einführung in die Identität auf ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2c22-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="e2c22-105">Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="e2c22-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="e2c22-106">ASP.NET Core Identität ist eine Mitgliedschaftssystem Anmeldefunktionalität für Ihre Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="e2c22-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="e2c22-107">Benutzer können ein Konto und die Anmeldung mit einem Benutzernamen erstellen und Kennwort, oder sie können einen externen Anmeldeanbieter wie Facebook, Google, Microsoft Account, Twitter oder anderen verwenden.</span><span class="sxs-lookup"><span data-stu-id="e2c22-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="e2c22-108">Sie können ASP.NET Core Identity Verwendung eine SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörtern und Profildaten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e2c22-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="e2c22-109">Alternativ können Sie eigene permanenten Speicher zu, z. B. Azure-Tabellenspeicher verwenden.</span><span class="sxs-lookup"><span data-stu-id="e2c22-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="e2c22-110">Dieses Dokument enthält Anleitungen für Visual Studio und mithilfe der CLI.</span><span class="sxs-lookup"><span data-stu-id="e2c22-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="e2c22-111">Übersicht über die Identität</span><span class="sxs-lookup"><span data-stu-id="e2c22-111">Overview of Identity</span></span>

<span data-ttu-id="e2c22-112">In diesem Thema werden Sie erfahren, wie ASP.NET Core Identity zu verwenden, um Funktionen zu registrieren, melden Sie sich hinzufügen und Abmelden eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="e2c22-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="e2c22-113">Ausführlichere Anweisungen zum Erstellen von apps mithilfe von ASP.NET Core Identity finden Sie unter dem Abschnitt "Nächste Schritte" am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="e2c22-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="e2c22-114">Erstellen Sie ein ASP.NET Core-Webanwendungsprojekt mit einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="e2c22-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2c22-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2c22-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="e2c22-116">Wählen Sie in Visual Studio **Datei** -> **neu** -> **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="e2c22-117">Wählen Sie die **ASP.NET-Webanwendung** aus der **neues Projekt** (Dialogfeld).</span><span class="sxs-lookup"><span data-stu-id="e2c22-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="e2c22-118">Auswählen einer ASP.NET Core **Webanwendung** mit **einzelne Benutzerkonten** als Authentifizierungsmethode.</span><span class="sxs-lookup"><span data-stu-id="e2c22-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="e2c22-119">Hinweis: Sie müssen auswählen, **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Dialogfeld "Neues Projekt"](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2c22-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e2c22-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="e2c22-122">Wenn Sie die .NET Core-Befehlszeilenschnittstelle verwenden zu können, erstellen Sie das neue Projekt mit ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="e2c22-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="e2c22-123">Dadurch wird ein neues Projekt mit der gleichen Identität Vorlagencode erstellt, die Visual Studio erstellt.</span><span class="sxs-lookup"><span data-stu-id="e2c22-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="e2c22-124">Das Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Paket, d. h. die Identitätsdaten und das Schema zur Verwendung von SQL Server beibehalten wird [Entity Framework Core](https://docs.efproject.net).</span><span class="sxs-lookup"><span data-stu-id="e2c22-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.efproject.net).</span></span>
    
    ---
 
2.  <span data-ttu-id="e2c22-125">Konfigurieren von Identitätsdiensten und Hinzufügen von Middleware in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e2c22-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="e2c22-126">Die Identity-Dienste werden hinzugefügt, an die Anwendung in der `ConfigureServices` Methode in der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e2c22-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>
 
    <span data-ttu-id="e2c22-127">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=7-9,13-34)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-127">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=7-9,13-34)]</span></span>
    
    <span data-ttu-id="e2c22-128">Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e2c22-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
 
    <span data-ttu-id="e2c22-129">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="e2c22-129">Identity is enabled for the application by calling  `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="e2c22-130">`UseIdentity`Fügt der Cookie-basierte Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="e2c22-130">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
 
    <span data-ttu-id="e2c22-131">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configure&highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-131">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configure&highlight=21)]</span></span>
 
    <span data-ttu-id="e2c22-132">Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e2c22-132">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="e2c22-133">Erstellen Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="e2c22-133">Create a user.</span></span>
 
    <span data-ttu-id="e2c22-134">Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.</span><span class="sxs-lookup"><span data-stu-id="e2c22-134">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="e2c22-135">Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie für die Ausführung von Migrationen erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="e2c22-135">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="e2c22-136">Die Anwendung aufgefordert, **gelten Migrationen**:</span><span class="sxs-lookup"><span data-stu-id="e2c22-136">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="e2c22-138">Alternativ können Sie testen, mithilfe von ASP.NET Core Identity mit Ihrer app ohne einen persistenten Datenbank mithilfe einer in-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e2c22-138">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="e2c22-139">Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die ``Microsoft.EntityFrameworkCore.InMemory`` auf Ihre app Packen und ändern Sie Ihre app Aufruf ``AddDbContext`` in ``ConfigureServices`` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e2c22-139">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="e2c22-140">Wenn der Benutzer klickt der **registrieren** Link, der ``Register`` Aktion wird aufgerufen, auf ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="e2c22-140">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="e2c22-141">Die ``Register`` Aktion erstellt Benutzer durch Aufrufen von `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion):</span><span class="sxs-lookup"><span data-stu-id="e2c22-141">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    <span data-ttu-id="e2c22-142">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=register&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-142">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=register&highlight=11)]</span></span>

    <span data-ttu-id="e2c22-143">Wenn der Benutzer erfolgreich erstellt wurde, wird der Anmeldung des Benutzers durch den Aufruf von ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="e2c22-143">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="e2c22-144">**Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte zum sofortigen Anmeldung bei der Registrierung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="e2c22-144">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="e2c22-145">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="e2c22-145">Log in.</span></span>
 
    <span data-ttu-id="e2c22-146">Benutzer können sich anmelden, indem Sie auf die **melden Sie sich** Link am oberen Rand der Website oder möglicherweise der Anmeldeseite navigiert werden, wenn Benutzer versuchen, ein Teil des Standorts, auf dem Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e2c22-146">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="e2c22-147">Wenn der Benutzer das Formular auf der Anmeldeseite sendet die ``AccountController`` ``Login`` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e2c22-147">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="e2c22-148">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=login&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-148">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=login&highlight=13-14)]</span></span>
 
    <span data-ttu-id="e2c22-149">Die ``Login`` Aktion Aufrufe ``PasswordSignInAsync`` auf die ``_signInManager`` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion).</span><span class="sxs-lookup"><span data-stu-id="e2c22-149">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="e2c22-150">Die Basis ``Controller`` Klasse macht ein ``User`` -Eigenschaft, die Sie über Controllermethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="e2c22-150">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="e2c22-151">Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen.</span><span class="sxs-lookup"><span data-stu-id="e2c22-151">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="e2c22-152">Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="e2c22-152">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="e2c22-153">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="e2c22-153">Log out.</span></span>
 
    <span data-ttu-id="e2c22-154">Klicken auf die **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.</span><span class="sxs-lookup"><span data-stu-id="e2c22-154">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    <span data-ttu-id="e2c22-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=logout&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=logout&highlight=7)]</span></span>
 
    <span data-ttu-id="e2c22-156">Der vorangehende Code oben Aufrufe der `_signInManager.SignOutAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="e2c22-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="e2c22-157">Die `SignOutAsync` Methode löscht den Ansprüchen des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e2c22-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="e2c22-158">Die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e2c22-158">Configuration.</span></span>

    <span data-ttu-id="e2c22-159">Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="e2c22-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="e2c22-160">Sie müssen nicht so konfigurieren Sie ``IdentityOptions`` Wenn Sie die Standardverhalten verwenden.</span><span class="sxs-lookup"><span data-stu-id="e2c22-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>
 
    <span data-ttu-id="e2c22-161">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=13-34)]</span><span class="sxs-lookup"><span data-stu-id="e2c22-161">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=13-34)]</span></span>
    
    <span data-ttu-id="e2c22-162">Weitere Informationen zum Konfigurieren von Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c22-162">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="e2c22-163">Sie können zudem konfigurieren den Datentyp des Primärschlüssels, finden Sie unter [konfigurieren Identität Primärschlüssel-Datentyp](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="e2c22-163">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="e2c22-164">Zeigen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="e2c22-164">View the database.</span></span>

    <span data-ttu-id="e2c22-165">Wenn Ihre app auf eine SQL Server-Datenbank (Standard unter Windows und Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank der app erstellt anzeigen.</span><span class="sxs-lookup"><span data-stu-id="e2c22-165">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="e2c22-166">Sie können **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-166">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="e2c22-167">Wählen Sie alternativ in Visual Studio **Ansicht** -> **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-167">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="e2c22-168">Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-168">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="e2c22-169">Die Datenbank mit dem Namen  **Aspnet - <*Name Ihres Projekts*>-<*Datumszeichenfolge*> ** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e2c22-169">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Kontextmenü für AspNetUsers-Datenbanktabelle](identity/_static/04-db.png)
    
    <span data-ttu-id="e2c22-171">Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. AspNetUsers** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="e2c22-171">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="e2c22-172">Identity-Komponenten</span><span class="sxs-lookup"><span data-stu-id="e2c22-172">Identity Components</span></span>

<span data-ttu-id="e2c22-173">Die primäre Verweisassembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="e2c22-173">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="e2c22-174">Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identität und ist durch enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e2c22-174">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="e2c22-175">Diese Abhängigkeiten sind erforderlich, um das Identitätssystem in ASP.NET Core-Anwendungen verwenden:</span><span class="sxs-lookup"><span data-stu-id="e2c22-175">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="e2c22-176">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Enthält die erforderlichen Typen um Identität mit Entity Framework Core verwenden.</span><span class="sxs-lookup"><span data-stu-id="e2c22-176">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="e2c22-177">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core ist Microsofts empfohlene datenzugriffstechnologie für relationale Datenbanken, wie z. B. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e2c22-177">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="e2c22-178">Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="e2c22-178">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="e2c22-179">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="e2c22-179">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="e2c22-180">Migrieren zu ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="e2c22-180">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="e2c22-181">Weitere Informationen und Anweisungen zur Migration Ihrer vorhandenen Identität Filiale finden Sie unter [Migrieren von Authentifizierung und Identität](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="e2c22-181">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2c22-182">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e2c22-182">Next Steps</span></span>

* [<span data-ttu-id="e2c22-183">Migrieren von Authentifizierung und Identität</span><span class="sxs-lookup"><span data-stu-id="e2c22-183">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="e2c22-184">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="e2c22-184">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e2c22-185">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="e2c22-185">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="e2c22-186">Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern</span><span class="sxs-lookup"><span data-stu-id="e2c22-186">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
