---
title: Einführung in die Identität in ASP.NET Core
author: rick-anderson
description: Verwenden Sie Identität mit einer ASP.NET Core-app. Erfahren Sie, wie Anforderungen für Kennwörter (RequireDigit, RequiredLength, RequiredUniqueChars usw.) festgelegt.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: bc69b1db56361dc185f582032148a4fb8078fdda
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893232"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a7e44-104">Einführung in die Identität in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7e44-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="a7e44-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7e44-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a7e44-106">ASP.NET Core Identity handelt es sich um ein Mitgliedschaftssystem, das ASP.NET Core-apps Anmeldefunktionalität hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="a7e44-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="a7e44-107">Benutzer können ein Konto erstellen, mit der Anmeldung Informationen in die Identität oder einem externen Anmeldeanbieter können.</span><span class="sxs-lookup"><span data-stu-id="a7e44-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a7e44-108">Einschließen von unterstützten externen anmeldungsanbietern [Facebook, Google, Twitter und Microsoft Account](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a7e44-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a7e44-109">Identität kann mithilfe einer SQL Server-Datenbank zum Speichern von Benutzernamen und Kennwörter, Profildaten konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a7e44-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a7e44-110">Alternativ kann einem anderen permanenten Speicher verwendet werden, z. B. Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="a7e44-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="a7e44-111">Anzeigen oder Herunterladen von Beispielcode.</span><span class="sxs-lookup"><span data-stu-id="a7e44-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="a7e44-112">(Gewusst wie: herunterladen)</span><span class="sxs-lookup"><span data-stu-id="a7e44-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="a7e44-113">In diesem Thema erfahren Sie, wie Identität verwenden, um zu registrieren, melden Sie sich, und melden Sie sich ein Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a7e44-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a7e44-114">Ausführlichere Anweisungen zum Erstellen von apps, die Identität verwenden, finden Sie im Abschnitt "Nächste Schritte" am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="a7e44-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a7e44-115">Erstellen einer Web-app mit Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="a7e44-115">Create a Web app with authentication</span></span>

<span data-ttu-id="a7e44-116">Erstellen Sie ein Projekt für die ASP.NET Core-Webanwendung mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="a7e44-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a7e44-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7e44-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a7e44-118">Wählen Sie **Datei** > **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="a7e44-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="a7e44-119">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="a7e44-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a7e44-120">Nennen Sie das Projekt **"WebApp1"** haben denselben Namespace aufweist wie das Projekt zum Herunterladen.</span><span class="sxs-lookup"><span data-stu-id="a7e44-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a7e44-121">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7e44-121">Click **OK**.</span></span>
* <span data-ttu-id="a7e44-122">Wählen Sie eine ASP.NET Core **Webanwendung** für ASP.NET Core 2.1, wählen Sie dann **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="a7e44-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a7e44-123">Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7e44-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a7e44-124">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a7e44-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="a7e44-125">Enthält das generierte Projekt [ASP.NET Core Identity](xref:security/authentication/identity) als eine [Razor-Klassenbibliothek](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a7e44-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="a7e44-126">Test-Registrierung und Anmeldung</span><span class="sxs-lookup"><span data-stu-id="a7e44-126">Test Register and Login</span></span>

<span data-ttu-id="a7e44-127">Führen Sie die app aus, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a7e44-127">Run the app and register a user.</span></span> <span data-ttu-id="a7e44-128">Je nach Bildschirmgröße, Sie müssen ggf. auf die Navigationsschaltfläche für die ein-/ausschalten, finden Sie unter den **registrieren** und **Anmeldung** Links.</span><span class="sxs-lookup"><span data-stu-id="a7e44-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![Umschaltfläche für die Navigationsleiste](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="a7e44-130">Konfigurieren der Identitätsdienste</span><span class="sxs-lookup"><span data-stu-id="a7e44-130">Configure Identity services</span></span>

<span data-ttu-id="a7e44-131">Dienste hinzugefügt werden, `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a7e44-131">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a7e44-132">Der folgende Code umfasst nicht die generierte Vorlage `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="a7e44-132">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="a7e44-133">Der vorangehende Code konfiguriert Identität mit Standardwerten für die Option.</span><span class="sxs-lookup"><span data-stu-id="a7e44-133">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="a7e44-134">Dienste, die app über den zur Verfügung gestellt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a7e44-134">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a7e44-135">Identität ist aktiviert, durch den Aufruf [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a7e44-135">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="a7e44-136">`UseAuthentication` Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="a7e44-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="a7e44-137">Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a7e44-137">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a7e44-138">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseAuthentication` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="a7e44-138">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="a7e44-139">`UseAuthentication` Fügt der Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="a7e44-139">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="a7e44-140">Diese Dienste stehen der Anwendung über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a7e44-140">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a7e44-141">Identität für die Anwendung aktiviert ist, durch den Aufruf `UseIdentity` in die `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="a7e44-141">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="a7e44-142">`UseIdentity` Fügt der cookiebasierte Authentifizierung [Middleware](xref:fundamentals/middleware/index) zu der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="a7e44-142">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="a7e44-143">Weitere Informationen finden Sie unter den [IdentityOptions Klasse](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) und [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a7e44-143">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a7e44-144">Erstellen des Gerüsts für registrieren, Anmeldung und Abmeldung</span><span class="sxs-lookup"><span data-stu-id="a7e44-144">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="a7e44-145">Führen Sie die [Gerüst Identity in einer Razor-Projekt mit Autorisierung](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="a7e44-145">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a7e44-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7e44-146">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7e44-147">Fügen Sie die Dateien registrieren, Anmeldung und Abmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="a7e44-147">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a7e44-148">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a7e44-148">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a7e44-149">Wenn Sie das Projekt mit dem Namen erstellt **"WebApp1"**, führen Sie die folgenden Befehle.</span><span class="sxs-lookup"><span data-stu-id="a7e44-149">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a7e44-150">Verwenden Sie andernfalls den richtigen Namespace für die `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a7e44-150">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="a7e44-151">PowerShell arbeitet mit Semikolon als Befehlstrennzeichen.</span><span class="sxs-lookup"><span data-stu-id="a7e44-151">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a7e44-152">Wenn Sie PowerShell verwenden, versehen Sie die Semikolons aus der Datei mit Escapezeichen, oder fügen Sie die Liste der Dateien in doppelte Anführungszeichen, wie im vorherigen Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="a7e44-152">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a7e44-153">Überprüfen Sie die Registrierung</span><span class="sxs-lookup"><span data-stu-id="a7e44-153">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="a7e44-154">Wenn ein Benutzer klickt der **registrieren** Link die `RegisterModel.OnPostAsync` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a7e44-154">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a7e44-155">Der Benutzer wird erstellt, indem [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) auf die `_userManager` Objekt.</span><span class="sxs-lookup"><span data-stu-id="a7e44-155">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a7e44-156">`_userManager` wird durch Dependency Injection bereitgestellt):</span><span class="sxs-lookup"><span data-stu-id="a7e44-156">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="a7e44-157">Wenn ein Benutzer klickt der **registrieren** Link die `Register` Aktion wird aufgerufen, auf `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="a7e44-157">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="a7e44-158">Die `Register` Aktion wird den Benutzer erstellt, durch den Aufruf `CreateAsync` auf die `_userManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection):</span><span class="sxs-lookup"><span data-stu-id="a7e44-158">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="a7e44-159">Wenn der Benutzer erfolgreich erstellt wurde, der angemeldete Benutzer wird durch den Aufruf von `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7e44-159">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="a7e44-160">**Hinweis:** finden Sie unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) Schritte, um sofortige Anmeldung bei der Registrierung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="a7e44-160">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a7e44-161">Anmelden</span><span class="sxs-lookup"><span data-stu-id="a7e44-161">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7e44-162">Das Anmeldeformular wird angezeigt, wenn:</span><span class="sxs-lookup"><span data-stu-id="a7e44-162">The Login form is displayed when:</span></span>

* <span data-ttu-id="a7e44-163">Die **melden Sie sich bei** Link ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="a7e44-163">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="a7e44-164">Wenn ein Benutzer greift auf eine Seite, in denen sie nicht authentifiziert **oder** autorisiert ist, werden sie zur Anmeldeseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a7e44-164">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="a7e44-165">Wenn das Formular auf der Anmeldeseite gesendet wird, die `OnPostAsync` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a7e44-165">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a7e44-166">`PasswordSignInAsync` wird aufgerufen, auf die `_signInManager` Objekt (bereitgestellt durch Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="a7e44-166">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="a7e44-167">Die Basis `Controller` -Klasse macht eine `User` -Eigenschaft, die Sie von Controllermethoden aus zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="a7e44-167">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a7e44-168">Sie können z. B. auflisten `User.Claims` und autorisierungsentscheidungen treffen.</span><span class="sxs-lookup"><span data-stu-id="a7e44-168">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a7e44-169">Weitere Informationen finden Sie unter [Autorisierung](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="a7e44-169">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7e44-170">Das Anmeldeformular wird angezeigt, wenn ein Benutzer auf die **melden Sie sich bei** verknüpfen oder umgeleitet werden, wenn eine Seite zugreifen, die eine Authentifizierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a7e44-170">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="a7e44-171">Wenn der Benutzer das Formular auf die Anmeldeseite, sendet der `AccountController` `Login` Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a7e44-171">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="a7e44-172">Die `Login` Aktionsaufrufen `PasswordSignInAsync` auf die `_signInManager` Objekt (bereitgestellt, um `AccountController` durch Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="a7e44-172">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="a7e44-173">Die Basis (`Controller` oder `PageModel`)-Klasse macht eine `User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a7e44-173">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="a7e44-174">Z. B. `User.Claims` aufgelistet werden kann, um autorisierungsentscheidungen zu treffen.</span><span class="sxs-lookup"><span data-stu-id="a7e44-174">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="a7e44-175">Melden Sie sich ab</span><span class="sxs-lookup"><span data-stu-id="a7e44-175">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7e44-176">Die **Abmelden** Link Ruft die `LogoutModel.OnPost` Aktion.</span><span class="sxs-lookup"><span data-stu-id="a7e44-176">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="a7e44-177">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) löscht der Ansprüche des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a7e44-177">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="a7e44-178">Nicht nach dem Aufruf umleiten `SignOutAsync` oder der Benutzer wird **nicht** abgemeldet werden.</span><span class="sxs-lookup"><span data-stu-id="a7e44-178">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="a7e44-179">POST wird angegeben, der *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7e44-179">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="a7e44-180">Auf der **Abmelden** verknüpfen Aufrufe der `LogOut` Aktion.</span><span class="sxs-lookup"><span data-stu-id="a7e44-180">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="a7e44-181">Der vorangehende Code Ruft die `_signInManager.SignOutAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="a7e44-181">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="a7e44-182">Die `SignOutAsync` Methode löscht der Ansprüche des Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a7e44-182">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="a7e44-183">Testidentität</span><span class="sxs-lookup"><span data-stu-id="a7e44-183">Test Identity</span></span>

<span data-ttu-id="a7e44-184">Die Standardprojektvorlagen für Web zulassen anonymen Zugriff auf den Startseiten.</span><span class="sxs-lookup"><span data-stu-id="a7e44-184">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a7e44-185">Um die Identität zu testen, fügen [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) auf der Seite "Info".</span><span class="sxs-lookup"><span data-stu-id="a7e44-185">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="a7e44-186">Wenn Sie angemeldet sind, melden Sie sich ab. Führen Sie die app, und wählen Sie die **zu** Link.</span><span class="sxs-lookup"><span data-stu-id="a7e44-186">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="a7e44-187">Sie werden zur Anmeldeseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a7e44-187">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="a7e44-188">Untersuchen Sie die Identität</span><span class="sxs-lookup"><span data-stu-id="a7e44-188">Explore Identity</span></span>

<span data-ttu-id="a7e44-189">Identität noch ausführlicher untersuchen zu können:</span><span class="sxs-lookup"><span data-stu-id="a7e44-189">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a7e44-190">Erstellen Sie die vollständige Benutzeroberfläche identitätsquelle</span><span class="sxs-lookup"><span data-stu-id="a7e44-190">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a7e44-191">Überprüfen Sie die Quelle der einzelnen Seiten und durchlaufen Sie den Debugger an.</span><span class="sxs-lookup"><span data-stu-id="a7e44-191">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="a7e44-192">Identitätskomponenten</span><span class="sxs-lookup"><span data-stu-id="a7e44-192">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7e44-193">Alle Identity abhängigen NuGet-Pakete sind in enthalten die [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a7e44-193">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="a7e44-194">Das primäre Paket für die Identität ist [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a7e44-194">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a7e44-195">Dieses Paket enthält die Kerngruppe von Schnittstellen für ASP.NET Core Identity und befindet sich durch `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a7e44-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a7e44-196">Migrieren zu ASP.NET Core-Identität</span><span class="sxs-lookup"><span data-stu-id="a7e44-196">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a7e44-197">Weitere Informationen und Anleitungen zum Migrieren von vorhandenen Identitätsspeicher, finden Sie unter [Migrieren von Authentifizierungs- und Identitätseinstellungen](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a7e44-197">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a7e44-198">Festlegen der kennwortsicherheit</span><span class="sxs-lookup"><span data-stu-id="a7e44-198">Setting password strength</span></span>

<span data-ttu-id="a7e44-199">Finden Sie unter [Konfiguration](#pw) für ein Beispiel, das die Anforderungen für die Mindestlänge für Kennwörter legt diese fest.</span><span class="sxs-lookup"><span data-stu-id="a7e44-199">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7e44-200">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a7e44-200">Next Steps</span></span>

* [<span data-ttu-id="a7e44-201">Konfigurieren von Identity</span><span class="sxs-lookup"><span data-stu-id="a7e44-201">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="a7e44-202">[Konfigurieren Sie die Identität, Primärschlüssel-Datentyp](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="a7e44-202">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
