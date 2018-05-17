---
title: Microsoft-Account externe Anmeldung Setup mit ASP.NET Core
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Microsoft-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 7244f7a808899a2846bb8b40e626208f168d40b8
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="f67be-103">Microsoft-Account externe Anmeldung Setup mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f67be-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="f67be-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f67be-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f67be-105">Dieses Lernprogramm veranschaulicht das Aktivieren von Benutzern die Anmeldung mit seinem Microsoft-Konto mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f67be-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="f67be-106">Erstellen Sie die app im Microsoft Developer Portal</span><span class="sxs-lookup"><span data-stu-id="f67be-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="f67be-107">Navigieren Sie zu [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) und erstellen oder ein Microsoft-Konto anmelden:</span><span class="sxs-lookup"><span data-stu-id="f67be-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Melden Sie sich im Dialogfeld "](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="f67be-109">Wenn Sie ein Microsoft-Konto noch nicht haben, tippen Sie auf  **[erstellen!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="f67be-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="f67be-110">Nach der Anmeldung werden Sie umgeleitet **Meine Anwendungen** Seite:</span><span class="sxs-lookup"><span data-stu-id="f67be-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft Developer Portal in Microsoft Edge öffnen](index/_static/MicrosoftDev.png)

* <span data-ttu-id="f67be-112">Tippen Sie auf **app hinzufügen** in der oberen rechten Ecke, und geben Sie Ihre **Anwendungsname** und **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="f67be-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Dialogfeld "Neues Anwendungsregistrierung"](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="f67be-114">Deaktivieren Sie für die Zwecke dieses Lernprogramms können die **geführtes Setup** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="f67be-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="f67be-115">Tippen Sie auf **erstellen** zur Weiterleitung an die **Registrierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="f67be-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="f67be-116">Geben Sie eine **Name** und notieren Sie den Wert der der **Anwendungs-Id**, die Verwendung als `ClientId` später im Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="f67be-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Seite "Registrierung"](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="f67be-118">Tippen Sie auf **Plattform hinzufügen** in der **Plattformen** Abschnitt, und wählen Sie die **Web** Plattform:</span><span class="sxs-lookup"><span data-stu-id="f67be-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Fügen Sie die Zielplattform (Dialogfeld)](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="f67be-120">In der neuen **Web** Plattform Abschnitt, geben Sie Ihre Entwicklung URL mit */signin-microsoft* angefügt, die in der **Umleitungs-URLs** Feld (z. B.: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="f67be-120">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="f67be-121">Das Microsoft-Authentifizierungsschema konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei */signin-microsoft* Route zum Implementieren des OAuth-Fluss:</span><span class="sxs-lookup"><span data-stu-id="f67be-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Webabschnitt-Plattform](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="f67be-123">Tippen Sie auf **URL hinzufügen** um sicherzustellen, dass die URL wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f67be-123">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="f67be-124">Füllen Sie ggf. Weitere Anwendungseinstellungen vor bei Bedarf, und tippen Sie auf **speichern** am unteren Rand der Seite, um Änderungen an der app-Konfiguration zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f67be-124">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="f67be-125">Bei der Bereitstellung der Website müssen Sie rufen die **Registrierung** Seite, und legen Sie eine neue Öffentliche URL.</span><span class="sxs-lookup"><span data-stu-id="f67be-125">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="f67be-126">Microsoft Anwendungs-Id und Kennwort speichern</span><span class="sxs-lookup"><span data-stu-id="f67be-126">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="f67be-127">Beachten Sie die `Application Id` angezeigt, auf die **Registrierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="f67be-127">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="f67be-128">Tippen Sie auf **neues Kennwort generieren** in der **Anwendung geheime Schlüssel** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="f67be-128">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="f67be-129">Dies zeigt ein Feld, in dem Sie das anwendungskennwort kopieren können:</span><span class="sxs-lookup"><span data-stu-id="f67be-129">This displays a box where you can copy the application password:</span></span>

![Dialogfeld "Neues Kennwort generiert"](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="f67be-131">Verknüpfen Sie die sensiblen Einstellungen wie Microsoft `Application ID` und `Password` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f67be-131">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="f67be-132">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Microsoft:ApplicationId` und `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="f67be-132">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="f67be-133">Konfigurieren Sie die Microsoft-Konto-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f67be-133">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="f67be-134">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="f67be-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="f67be-135">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="f67be-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="f67be-136">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="f67be-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f67be-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f67be-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f67be-138">Fügen Sie den Microsoft-Account-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="f67be-138">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f67be-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f67be-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f67be-140">Fügen Sie die Microsoft-Account-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="f67be-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="f67be-141">Obwohl die Terminologie, die auf Microsoft-Entwicklerportal diese Token benennt `ApplicationId` und `Password`, sie sind als verfügbar gemacht `ClientId` und `ClientSecret` an der API-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f67be-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="f67be-142">Finden Sie unter der [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Microsoft-Account-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f67be-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="f67be-143">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="f67be-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="f67be-144">Melden Sie sich mit Microsoft-Konto</span><span class="sxs-lookup"><span data-stu-id="f67be-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="f67be-145">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="f67be-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="f67be-146">Eine Option aus, um sich mit Microsoft anmelden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f67be-146">An option to sign in with Microsoft appears:</span></span>

![-Webanwendung Protokoll auf der Seite: nicht authentifizierte Benutzer](index/_static/DoneMicrosoft.png)

<span data-ttu-id="f67be-148">Wenn Sie auf Microsoft klicken, werden Sie für die Authentifizierung an Microsoft weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="f67be-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="f67be-149">Nachdem Sie sich mit Ihrem Microsoft-Account (sofern nicht bereits angemeldet) werden Sie aufgefordert, um die app Zugriff auf Ihre Infos zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="f67be-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Dialogfeld für Microsoft-Authentifizierung](index/_static/MicrosoftLogin.png)

<span data-ttu-id="f67be-151">Tippen Sie auf **Ja** und Sie zurück zur Website, wo Sie Ihre e-Mail-Adresse festlegen können, umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="f67be-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="f67be-152">Sie sind jetzt mit Ihrem Microsoft-Anmeldeinformationen angemeldet:</span><span class="sxs-lookup"><span data-stu-id="f67be-152">You are now logged in using your Microsoft credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="f67be-154">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f67be-154">Troubleshooting</span></span>

* <span data-ttu-id="f67be-155">Wenn der Microsoft-Account-Anbieter auf eine Fehler-Anmeldeseite umgeleitet, beachten Sie die Fehler Titel und Beschreibung Abfragezeichenfolgen-Parameter direkt nach der `#` (Hashtag) im Uri.</span><span class="sxs-lookup"><span data-stu-id="f67be-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="f67be-156">Obwohl die Fehlermeldung an, dass ein Problem mit dem Microsoft-Authentifizierung scheint, ist die häufigste Ursache Ihrer Anwendung mit keiner der Uri der **Weiterleitungs-URIs** angegeben für die **Web** Plattform .</span><span class="sxs-lookup"><span data-stu-id="f67be-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="f67be-157">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="f67be-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f67be-158">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="f67be-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="f67be-159">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="f67be-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f67be-160">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="f67be-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f67be-161">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f67be-161">Next steps</span></span>

* <span data-ttu-id="f67be-162">In diesem Artikel wurde gezeigt, wie Sie mit Microsoft authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="f67be-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="f67be-163">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f67be-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="f67be-164">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie ein neues erstellen `Password` im Microsoft-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="f67be-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="f67be-165">Legen Sie die `Authentication:Microsoft:ApplicationId` und `Authentication:Microsoft:Password` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="f67be-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="f67be-166">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="f67be-166">The configuration system is set up to read keys from environment variables.</span></span>
