---
title: "Setup für Microsoft-Account-externe Anmeldung"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 25ada568a8629b82eb55ca27a55a9ac0c5fb38b2
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="63801-103">Konfigurieren von Microsoft-Account-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="63801-103">Configuring Microsoft Account authentication</span></span>

<a name=security-authentication-microsoft-logins></a>

<span data-ttu-id="63801-104">Durch [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63801-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63801-105">Dieses Lernprogramm veranschaulicht das Aktivieren von Benutzern die Anmeldung mit seinem Microsoft-Konto mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="63801-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="63801-106">Erstellen Sie die app im Microsoft Developer Portal</span><span class="sxs-lookup"><span data-stu-id="63801-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="63801-107">Navigieren Sie zu [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) und erstellen oder ein Microsoft-Konto anmelden:</span><span class="sxs-lookup"><span data-stu-id="63801-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Melden Sie sich im Dialogfeld "](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="63801-109">Wenn Sie ein Microsoft-Konto noch nicht haben, tippen Sie auf  **[erstellen!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="63801-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="63801-110">Nach der Anmeldung werden Sie umgeleitet **Meine Anwendungen** Seite:</span><span class="sxs-lookup"><span data-stu-id="63801-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft Developer Portal in Microsoft Edge öffnen](index/_static/MicrosoftDev.png)

* <span data-ttu-id="63801-112">Tippen Sie auf **app hinzufügen** in der oberen rechten Ecke, und geben Sie Ihre **Anwendungsname** und **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="63801-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Dialogfeld "Neues Anwendungsregistrierung"](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="63801-114">Deaktivieren Sie für die Zwecke dieses Lernprogramms können die **geführtes Setup** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="63801-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="63801-115">Tippen Sie auf **erstellen** zur Weiterleitung an die **Registrierung** Seite:</span><span class="sxs-lookup"><span data-stu-id="63801-115">Tap **Create** to continue to the **Registration** page:</span></span>

![Seite "Registrierung"](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="63801-117">Tippen Sie auf **Plattform hinzufügen** in der **Plattformen** Abschnitt, und wählen Sie die **Web** Plattform:</span><span class="sxs-lookup"><span data-stu-id="63801-117">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Fügen Sie die Zielplattform (Dialogfeld)](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="63801-119">In der neuen **Web** Plattform Abschnitt, geben Sie Ihre Entwicklung URL mit */signin-microsoft* angefügt, die in der **Umleitungs-URLs** Feld (z. B.: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="63801-119">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="63801-120">Das Microsoft-Authentifizierungsschema konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei */signin-microsoft* Route zum Implementieren des OAuth-Fluss:</span><span class="sxs-lookup"><span data-stu-id="63801-120">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Webabschnitt-Plattform](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="63801-122">Tippen Sie auf **URL hinzufügen** um sicherzustellen, dass die URL wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="63801-122">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="63801-123">Füllen Sie ggf. Weitere Anwendungseinstellungen vor bei Bedarf, und tippen Sie auf **speichern** am unteren Rand der Seite, um Änderungen an der app-Konfiguration zu speichern.</span><span class="sxs-lookup"><span data-stu-id="63801-123">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="63801-124">Bei der Bereitstellung der Website müssen Sie rufen die **Registrierung** Seite, und legen Sie eine neue Öffentliche URL.</span><span class="sxs-lookup"><span data-stu-id="63801-124">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="63801-125">Microsoft Anwendungs-Id und Kennwort speichern</span><span class="sxs-lookup"><span data-stu-id="63801-125">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="63801-126">Beachten Sie die `Application Id` angezeigt, auf die **Registrierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="63801-126">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="63801-127">Tippen Sie auf **neues Kennwort generieren** in der **Anwendung geheime Schlüssel** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="63801-127">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="63801-128">Dies zeigt ein Feld, in dem Sie das anwendungskennwort kopieren können:</span><span class="sxs-lookup"><span data-stu-id="63801-128">This displays a box where you can copy the application password:</span></span>

![Dialogfeld "Neues Kennwort generiert"](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="63801-130">Verknüpfen Sie die sensiblen Einstellungen wie Microsoft `Application ID` und `Password` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="63801-130">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="63801-131">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Microsoft:ApplicationId` und `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="63801-131">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="63801-132">Konfigurieren Sie die Microsoft-Konto-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="63801-132">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="63801-133">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Microsoft) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="63801-133">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Microsoft) package is already installed.</span></span>

* <span data-ttu-id="63801-134">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="63801-134">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="63801-135">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="63801-135">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Microsoft`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63801-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63801-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63801-137">Fügen Sie den Microsoft-Account-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="63801-137">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

<span data-ttu-id="63801-138">Die `AddAuthentication` Methode sollte nur einmal beim Hinzufügen mehrerer Authentifizierungsanbieter aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="63801-138">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="63801-139">Nachfolgende Aufrufe haben das volle Potenzial von überschreiben alle zuvor konfigurierten [authenticationoptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="63801-139">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63801-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63801-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63801-141">Fügen Sie die Microsoft-Account-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="63801-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="63801-142">Obwohl die Terminologie, die auf Microsoft-Entwicklerportal diese Token benennt `ApplicationId` und `Password`, als verfügbar gemacht werden `ClientId` und `ClientSecret` an der API-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="63801-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="63801-143">Finden Sie unter der [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Microsoft-Account-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="63801-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="63801-144">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="63801-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="63801-145">Melden Sie sich mit Microsoft-Konto</span><span class="sxs-lookup"><span data-stu-id="63801-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="63801-146">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="63801-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="63801-147">Eine Option aus, um sich mit Microsoft anmelden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="63801-147">An option to sign in with Microsoft appears:</span></span>

![-Webanwendung Protokoll auf der Seite: nicht authentifizierte Benutzer](index/_static/DoneMicrosoft.png)

<span data-ttu-id="63801-149">Wenn Sie auf Microsoft klicken, werden Sie für die Authentifizierung an Microsoft weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="63801-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="63801-150">Nachdem Sie sich mit Ihrem Microsoft-Account (sofern nicht bereits angemeldet) werden Sie aufgefordert, um die app Zugriff auf Ihre Infos zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="63801-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Dialogfeld für Microsoft-Authentifizierung](index/_static/MicrosoftLogin.png)

<span data-ttu-id="63801-152">Tippen Sie auf **Ja** und Sie zurück zur Website, wo Sie Ihre e-Mail-Adresse festlegen können, umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="63801-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="63801-153">Sie sind jetzt mit Ihrem Microsoft-Anmeldeinformationen angemeldet:</span><span class="sxs-lookup"><span data-stu-id="63801-153">You are now logged in using your Microsoft credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="63801-155">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="63801-155">Troubleshooting</span></span>

* <span data-ttu-id="63801-156">Wenn der Microsoft-Account-Anbieter auf eine Fehler-Anmeldeseite umgeleitet, beachten Sie die Fehler Titel und Beschreibung Abfragezeichenfolgen-Parameter direkt nach der `#` (Hashtag) im Uri.</span><span class="sxs-lookup"><span data-stu-id="63801-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="63801-157">Obwohl die Fehlermeldung an, dass ein Problem mit dem Microsoft-Authentifizierung scheint, ist die häufigste Ursache Ihrer Anwendung mit keiner der Uri der **Weiterleitungs-URIs** angegeben für die **Web** Plattform .</span><span class="sxs-lookup"><span data-stu-id="63801-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="63801-158">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="63801-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="63801-159">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="63801-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="63801-160">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="63801-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="63801-161">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="63801-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63801-162">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="63801-162">Next steps</span></span>

* <span data-ttu-id="63801-163">In diesem Artikel wurde gezeigt, wie Sie mit Microsoft authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="63801-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="63801-164">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="63801-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="63801-165">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie ein neues erstellen `Password` im Microsoft-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="63801-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="63801-166">Legen Sie die `Authentication:Microsoft:ApplicationId` und `Authentication:Microsoft:Password` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="63801-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="63801-167">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="63801-167">The configuration system is set up to read keys from environment variables.</span></span>
