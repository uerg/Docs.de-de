---
title: "Einführung in die Identität auf ASP.NET Core"
author: rick-anderson
description: "Verwenden Sie die Identität mit einer ASP.NET Core-app"
keywords: "ASP.NET Core, Identität, Autorisierung, Sicherheit"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: fc8e076af92bd8f9a95e73abb66ce32cae8ab9cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8c4d6-104">Einführung in die Identität auf ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c4d6-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="8c4d6-105">Durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8c4d6-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8c4d6-106">ASP.NET Core Identität ist eine Mitgliedschaftssystem Anmeldefunktionalität für Ihre Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="8c4d6-107">Benutzer können ein Konto und die Anmeldung mit einem Benutzernamen erstellen und Kennwort, oder sie können einen externen Anmeldeanbieter wie Facebook, Google, Microsoft Account, Twitter oder anderen verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="8c4d6-108">Sie können ASP.NET Core Identity Verwendung eine SQL Server-Datenbank zum Speichern von Benutzernamen, Kennwörtern und Profildaten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8c4d6-109">Alternativ können Sie eigene permanenten Speicher zu, z. B. Azure-Tabellenspeicher verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="8c4d6-110">Dieses Dokument enthält Anleitungen für Visual Studio und mithilfe der CLI.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="8c4d6-111">Anzeigen oder den Beispielcode herunter.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="8c4d6-112">(Gewusst wie: herunterladen)</span><span class="sxs-lookup"><span data-stu-id="8c4d6-112">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="8c4d6-113">Übersicht über die Identität</span><span class="sxs-lookup"><span data-stu-id="8c4d6-113">Overview of Identity</span></span>

<span data-ttu-id="8c4d6-114">In diesem Thema werden Sie erfahren, wie ASP.NET Core Identity zu verwenden, um Funktionen zu registrieren, melden Sie sich hinzufügen und Abmelden eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="8c4d6-115">Ausführlichere Anweisungen zum Erstellen von apps mithilfe von ASP.NET Core Identity finden Sie unter dem Abschnitt "Nächste Schritte" am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="8c4d6-116">Erstellen Sie ein ASP.NET Core-Webanwendungsprojekt mit einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c4d6-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c4d6-117">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="8c4d6-118">Wählen Sie in Visual Studio **Datei** > **neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="8c4d6-119">Wählen Sie **ASP.NET-Webanwendung für Core** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Dialogfeld "Neues Projekt"](identity/_static/01-new-project.png)

    <span data-ttu-id="8c4d6-121">Wählen Sie eine ASP.NET Core **Webanwendung (Model-View-Controller)** für ASP.NET Core 2.x, und wählen Sie dann **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Dialogfeld "Neues Projekt"](identity/_static/02-new-project.png)

    <span data-ttu-id="8c4d6-123">Ein Dialogfeld wird angezeigt, Angebot Authentifizierungsoptionen.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="8c4d6-124">Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK** um zum vorherigen Dialogfeld zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Dialogfeld "Neues Projekt"](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="8c4d6-126">Auswählen von **einzelne Benutzerkonten** leitet Visual Studio und Erstellen von Modellen, ViewModels, Ansichten, Controller und anderen Ressourcen, die im Rahmen der Projektvorlage für die Authentifizierung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c4d6-127">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="8c4d6-127">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="8c4d6-128">Wenn Sie die .NET Core-Befehlszeilenschnittstelle verwenden zu können, erstellen Sie das neue Projekt mit ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="8c4d6-129">Dieser Befehl erstellt ein neues Projekt mit der gleichen Identität Vorlagencode, die, den Visual Studio erstellt.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="8c4d6-130">Das Projekt enthält die `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Paket, d. h. die Identitätsdaten und das Schema mit SQL Server weiterhin [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="8c4d6-131">Konfigurieren von Identitätsdiensten und Hinzufügen von Middleware in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-131">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="8c4d6-132">Die Identity-Dienste werden hinzugefügt, an die Anwendung in der `ConfigureServices` Methode in der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="8c4d6-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c4d6-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c4d6-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="8c4d6-134">Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="8c4d6-135">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="8c4d6-136">`UseAuthentication`Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c4d6-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c4d6-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="8c4d6-138">Diese Dienste werden durch an die Anwendung zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="8c4d6-139">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="8c4d6-140">`UseIdentity`Fügt der Cookie-basierte Authentifizierung [Middleware](xref:fundamentals/middleware) an die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="8c4d6-141">Weitere Informationen zu den Prozess starten der Anwendung, finden Sie unter [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="8c4d6-142">Erstellen Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-142">Create a user.</span></span>

    <span data-ttu-id="8c4d6-143">Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-143">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="8c4d6-144">Ist dies beim ersten Verwenden Sie diese Aktion durchführen, können Sie für die Ausführung von Migrationen erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="8c4d6-145">Die Anwendung aufgefordert, **gelten Migrationen**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="8c4d6-146">Die Seite "bei Bedarf zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-146">Refresh the page if needed.</span></span>
    
    ![Anwenden von Migrationen-Webseite](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="8c4d6-148">Alternativ können Sie testen, mithilfe von ASP.NET Core Identity mit Ihrer app ohne einen persistenten Datenbank mithilfe einer in-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="8c4d6-149">Um eine Datenbank im Arbeitsspeicher zu verwenden, fügen die ``Microsoft.EntityFrameworkCore.InMemory`` auf Ihre app Packen und ändern Sie Ihre app Aufruf ``AddDbContext`` in ``ConfigureServices`` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="8c4d6-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="8c4d6-150">Wenn der Benutzer klickt der **registrieren** Link, der ``Register`` Aktion wird aufgerufen, auf ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="8c4d6-151">Die ``Register`` Aktion erstellt Benutzer durch Aufrufen von `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion):</span><span class="sxs-lookup"><span data-stu-id="8c4d6-151">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="8c4d6-152">Wenn der Benutzer erfolgreich erstellt wurde, wird der Anmeldung des Benutzers durch den Aufruf von ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="8c4d6-153">**Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte zum sofortigen Anmeldung bei der Registrierung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="8c4d6-154">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-154">Log in.</span></span>
 
    <span data-ttu-id="8c4d6-155">Benutzer können sich anmelden, indem Sie auf die **melden Sie sich** Link am oberen Rand der Website oder möglicherweise der Anmeldeseite navigiert werden, wenn Benutzer versuchen, ein Teil des Standorts, auf dem Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="8c4d6-156">Wenn der Benutzer das Formular auf der Anmeldeseite sendet die ``AccountController`` ``Login`` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="8c4d6-157">Die ``Login`` Aktion Aufrufe ``PasswordSignInAsync`` auf die ``_signInManager`` Objekt (bereitgestellt, um ``AccountController`` von Abhängigkeitsinjektion).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="8c4d6-158">Die Basis ``Controller`` Klasse macht ein ``User`` -Eigenschaft, die Sie über Controllermethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="8c4d6-159">Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="8c4d6-160">Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="8c4d6-161">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-161">Log out.</span></span>
 
    <span data-ttu-id="8c4d6-162">Klicken auf die **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="8c4d6-163">Der vorangehende Code oben Aufrufe der `_signInManager.SignOutAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="8c4d6-164">Die `SignOutAsync` Methode löscht den Ansprüchen des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="8c4d6-165">Die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-165">Configuration.</span></span>

    <span data-ttu-id="8c4d6-166">Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-166">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="8c4d6-167">Sie müssen nicht so konfigurieren Sie ``IdentityOptions`` Wenn Sie die Standardverhalten verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-167">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c4d6-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c4d6-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c4d6-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c4d6-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="8c4d6-170">Weitere Informationen zum Konfigurieren von Identität finden Sie unter [konfigurieren Identität](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-170">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="8c4d6-171">Sie können zudem konfigurieren den Datentyp des Primärschlüssels, finden Sie unter [konfigurieren Identität Primärschlüssel-Datentyp](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-171">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="8c4d6-172">Zeigen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-172">View the database.</span></span>

    <span data-ttu-id="8c4d6-173">Wenn Ihre app auf eine SQL Server-Datenbank (Standard unter Windows und Visual Studio-Benutzer) verwendet wird, können Sie die Datenbank der app erstellt anzeigen.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-173">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="8c4d6-174">Sie können **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-174">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="8c4d6-175">Wählen Sie alternativ in Visual Studio **Ansicht** > **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-175">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="8c4d6-176">Herstellen einer Verbindung mit **(Localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-176">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="8c4d6-177">Die Datenbank mit dem Namen  **Aspnet - <*Name Ihres Projekts*>-<*Datumszeichenfolge*> ** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-177">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Kontextmenü für AspNetUsers-Datenbanktabelle](identity/_static/04-db.png)
    
    <span data-ttu-id="8c4d6-179">Erweitern Sie die Datenbank und die zugehörige **Tabellen**, klicken Sie dann mit der rechten Maustaste die **Dbo. AspNetUsers** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-179">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="8c4d6-180">Überprüfen der Identität works</span><span class="sxs-lookup"><span data-stu-id="8c4d6-180">Verify Identity works</span></span>

    <span data-ttu-id="8c4d6-181">Die Standardeinstellung *ASP.NET-Webanwendung für Core* -Projektvorlage kann Benutzer eine Aktion in der Anwendung ohne zugreifen, die Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-181">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="8c4d6-182">Um sicherzustellen, dass ASP.NET Identity funktioniert, fügen eine`[Authorize]` -Attribut auf die `About` Aktion von der `Home` Controller.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-182">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c4d6-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c4d6-183">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="8c4d6-184">Führen Sie das Projekt mit **STRG** + **F5** und navigieren Sie zu der **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-184">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="8c4d6-185">Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt, damit ASP.NET zur Anmeldeseite anmelden oder registrieren Sie weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-185">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c4d6-186">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="8c4d6-186">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="8c4d6-187">Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Stamm des Projekts, das Verzeichnis enthält die `.csproj` Datei.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-187">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="8c4d6-188">Führen Sie die `dotnet run` Befehl aus, um die app ausführen:</span><span class="sxs-lookup"><span data-stu-id="8c4d6-188">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="8c4d6-189">Navigieren Sie in der Ausgabe von angegebenen URL der `dotnet run` Befehl.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-189">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="8c4d6-190">Zeigen Sie die URL sollte auf `localhost` mit einem generierte Portnummer aus.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-190">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="8c4d6-191">Navigieren Sie zu der **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-191">Navigate to the **About** page.</span></span> <span data-ttu-id="8c4d6-192">Nur authentifizierte Benutzer möglicherweise Zugriff auf die **zu** Seite jetzt, damit ASP.NET zur Anmeldeseite anmelden oder registrieren Sie weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-192">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="8c4d6-193">Identity-Komponenten</span><span class="sxs-lookup"><span data-stu-id="8c4d6-193">Identity Components</span></span>

<span data-ttu-id="8c4d6-194">Die primäre Verweisassembly für das Identitätssystem ist `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-194">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="8c4d6-195">Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identität und ist durch enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="8c4d6-196">Diese Abhängigkeiten sind erforderlich, um das Identitätssystem in ASP.NET Core-Anwendungen verwenden:</span><span class="sxs-lookup"><span data-stu-id="8c4d6-196">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="8c4d6-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Enthält die erforderlichen Typen um Identität mit Entity Framework Core verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="8c4d6-198">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core ist Microsofts empfohlene datenzugriffstechnologie für relationale Datenbanken, wie z. B. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-198">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="8c4d6-199">Zu Testzwecken können Sie `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-199">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="8c4d6-200">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware, die eine app für die Verwendung von Cookie-basierte Authentifizierung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="8c4d6-200">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8c4d6-201">Migrieren zu ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="8c4d6-201">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8c4d6-202">Weitere Informationen und Anweisungen zur Migration Ihrer vorhandenen Identität Filiale finden Sie unter [Migrieren von Authentifizierung und Identität](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="8c4d6-202">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c4d6-203">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="8c4d6-203">Next Steps</span></span>

* [<span data-ttu-id="8c4d6-204">Migrating Authentication and Identity (Migration von Authentifizierungs- und Identitätseinstellungen)</span><span class="sxs-lookup"><span data-stu-id="8c4d6-204">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="8c4d6-205">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="8c4d6-205">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="8c4d6-206">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="8c4d6-206">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="8c4d6-207">Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern</span><span class="sxs-lookup"><span data-stu-id="8c4d6-207">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
