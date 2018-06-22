---
title: Google externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Google-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.date: 08/02/2017
uid: security/authentication/google-logins
ms.openlocfilehash: c5b6c992e134a2c4f0314d9d6e0465e6228c54ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274910"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="e9628-103">Google externe Anmeldung Setup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9628-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="e9628-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9628-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e9628-105">Dieses Lernprogramm veranschaulicht das Ihren Benutzern zur Anmeldung mit ihrem Google +-Kontos mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e9628-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="e9628-106">Wir beginnen, indem Sie die folgenden der [offizielle Schritte](https://developers.google.com/identity/sign-in/web/devconsole-project) zum Erstellen einer neuen app im Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="e9628-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="e9628-107">Erstellen der app im Google-API-Konsole</span><span class="sxs-lookup"><span data-stu-id="e9628-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="e9628-108">Navigieren Sie zu [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="e9628-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="e9628-109">Wenn Sie eine Google-Konto noch nicht haben, verwenden **Weitere Optionen** > **[Konto erstellen](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  Link, um eines zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e9628-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google-API-Konsole](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="e9628-111">Sie werden zur umgeleitet **API-Manager-Bibliothek** Seite:</span><span class="sxs-lookup"><span data-stu-id="e9628-111">You are redirected to **API Manager Library** page:</span></span>

![Seite "API-Manager-Bibliothek"](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="e9628-113">Tippen Sie auf **erstellen** , und geben Sie Ihre **Projektname**:</span><span class="sxs-lookup"><span data-stu-id="e9628-113">Tap **Create** and enter your **Project name**:</span></span>

![Dialogfeld "Neues Projekt"](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="e9628-115">Nach dem Akzeptieren des Dialogfeld ", werden Sie wieder auf der Seite" Bibliothek "können Sie Funktionen für Ihre neue app auswählen weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="e9628-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="e9628-116">Suchen **Google + API** in der Liste aus und klicken Sie auf den zugehörigen Link der API-Funktion hinzu:</span><span class="sxs-lookup"><span data-stu-id="e9628-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Seite "API-Manager-Bibliothek"](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="e9628-118">Die Seite für die neu hinzugefügte-API wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e9628-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="e9628-119">Tippen Sie auf **aktivieren** Google + Anmeldung Funktion in Ihrer app hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="e9628-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API-Manager Google + API-Seite](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="e9628-121">Nach dem Aktivieren der API, tippen Sie auf **Anmeldeinformationen erstellen** so konfigurieren Sie die geheimen Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="e9628-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API-Manager Google + API-Seite](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="e9628-123">Wählen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="e9628-123">Choose:</span></span>
   * <span data-ttu-id="e9628-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="e9628-124">**Google+ API**</span></span>
   * <span data-ttu-id="e9628-125">**Webserver (z. B. node.js, Tomcat)**, und</span><span class="sxs-lookup"><span data-stu-id="e9628-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="e9628-126">**Benutzerdaten**:</span><span class="sxs-lookup"><span data-stu-id="e9628-126">**User data**:</span></span>

![API-Manager anmeldeinformationsseite: Informieren Sie sich über welche Art von Anmeldeinformationen benötigen Bereich](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="e9628-128">Tippen Sie auf **welche Anmeldeinformationen benötige ich?** die gelangen Sie zu der zweite Schritt des app-Konfiguration **Erstellen einer OAuth 2.0-Client-ID**:</span><span class="sxs-lookup"><span data-stu-id="e9628-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API-Manager anmeldeinformationsseite: Erstellen einer OAuth 2.0-Client-ID](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="e9628-130">Da wir eine Google +-Projekt, mit nur einem Feature (Anmelden) erstellen, können wir geben Sie den gleichen **Namen** für die OAuth 2.0-Client-ID als diejenige, die wir für das Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9628-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="e9628-131">Geben Sie die Entwicklung URI mit `/signin-google` angefügt, die in der **autorisierte umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="e9628-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="e9628-132">Die Google-Authentifizierung konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei `/signin-google` Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="e9628-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="e9628-133">Das URI-Segment `/signin-google` als den standardrückruf des Google-Authentifizierungsanbieter festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e9628-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="e9628-134">Sie können den standardrückruf-URI ändern, während der Konfiguration der Google-Authentifizierung-Middleware über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft von der [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Klasse.</span><span class="sxs-lookup"><span data-stu-id="e9628-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="e9628-135">Drücken Sie TAB, um das Hinzufügen der **autorisierte umleitungs-URIs** Eintrag.</span><span class="sxs-lookup"><span data-stu-id="e9628-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="e9628-136">Tippen Sie auf **Client-ID erstellen**, dem gelangen Sie mit dem dritten Schritt **richten Sie den Bildschirm der OAuth 2.0-Zustimmung**:</span><span class="sxs-lookup"><span data-stu-id="e9628-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API-Manager anmeldeinformationsseite: Richten Sie den Bildschirm der OAuth 2.0-Zustimmung](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="e9628-138">Geben Sie Ihre öffentlich **e-Mail-Adresse** und **Produktname** für Ihre app angezeigt wird, wenn Google + melden Sie sich der Benutzer aufgefordert wird.</span><span class="sxs-lookup"><span data-stu-id="e9628-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="e9628-139">Weitere Optionen sind verfügbar unter **Weitere Optionen zur Anpassung der**.</span><span class="sxs-lookup"><span data-stu-id="e9628-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="e9628-140">Tippen Sie auf **Fortfahren** bis zum letzten Schritt fortfahren **Anmeldeinformationen herunterladen**:</span><span class="sxs-lookup"><span data-stu-id="e9628-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API-Manager anmeldeinformationsseite: Herunterladen von Anmeldeinformationen](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="e9628-142">Tippen Sie auf **herunterladen** eine JSON-Datei mit der Anwendung vertrauliche Informationen gespeichert und **Fertig** um die Erstellung der neuen app abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="e9628-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="e9628-143">Bei der Bereitstellung der Website müssen Sie rufen die **Konsole der Google-** und eine neue öffentliche Url zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="e9628-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="e9628-144">Store Google ClientID und ClientSecret</span><span class="sxs-lookup"><span data-stu-id="e9628-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="e9628-145">Verknüpfen Sie die sensiblen Einstellungen wie Google `Client ID` und `Client Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e9628-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="e9628-146">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="e9628-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="e9628-147">Die Werte für diese Token finden Sie in der JSON-Datei heruntergeladen, die im vorherigen Schritt unter `web.client_id` und `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="e9628-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="e9628-148">Konfigurieren von Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e9628-148">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9628-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9628-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e9628-150">Fügen Sie den Google-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="e9628-150">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9628-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9628-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e9628-152">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) Paket installiert ist.</span><span class="sxs-lookup"><span data-stu-id="e9628-152">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="e9628-153">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="e9628-153">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="e9628-154">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="e9628-154">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="e9628-155">Fügen Sie die Google-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="e9628-155">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="e9628-156">Finden Sie unter der [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Google-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e9628-156">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="e9628-157">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="e9628-157">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="e9628-158">Melden Sie sich mit Google</span><span class="sxs-lookup"><span data-stu-id="e9628-158">Sign in with Google</span></span>

<span data-ttu-id="e9628-159">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="e9628-159">Run your application and click **Log in**.</span></span> <span data-ttu-id="e9628-160">Eine Option zur Anmeldung mit Google angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="e9628-160">An option to sign in with Google appears:</span></span>

![In Microsoft Edge ausgeführte Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneGoogle.png)

<span data-ttu-id="e9628-162">Wenn Sie die Google klicken, werden Sie Google für die Authentifizierung weitergeleitet:</span><span class="sxs-lookup"><span data-stu-id="e9628-162">When you click on Google, you are redirected to Google for authentication:</span></span>

![Dialogfeld für Google-Authentifizierung](index/_static/GoogleLogin.png)

<span data-ttu-id="e9628-164">Nach dem Eingeben Ihrer Google-Anmeldeinformationen, werden dann Sie zurück zur Website umgeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="e9628-164">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="e9628-165">Sie sind jetzt angemeldet mithilfe Ihrer Google-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="e9628-165">You are now logged in using your Google credentials:</span></span>

![In Microsoft Edge ausgeführte Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="e9628-167">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="e9628-167">Troubleshooting</span></span>

* <span data-ttu-id="e9628-168">Erhalten Sie eine `403 (Forbidden)` Fehlerseite aus Ihrer eigenen app, die beim Ausführen in den Entwicklungsmodus (oder Unterbrechung des Debuggers mit den gleichen Fehler) sicher, dass **Google + API** aktiviert wurde, der **-API-Manager-Bibliothek** anhand der aufgeführten Schritte [weiter oben auf dieser Seite](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="e9628-168">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="e9628-169">Wenn die Anmeldung funktioniert nicht, und Sie werden keine Fehler erhalten, wechseln Sie zu den Entwicklungsmodus, um das Problem Debuggen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="e9628-169">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="e9628-170">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="e9628-170">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="e9628-171">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="e9628-171">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="e9628-172">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="e9628-172">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="e9628-173">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="e9628-173">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9628-174">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e9628-174">Next steps</span></span>

* <span data-ttu-id="e9628-175">In diesem Artikel wurde gezeigt, wie Sie mit Google authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="e9628-175">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="e9628-176">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e9628-176">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="e9628-177">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ClientSecret` in der Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="e9628-177">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="e9628-178">Legen Sie die `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="e9628-178">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="e9628-179">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="e9628-179">The configuration system is set up to read keys from environment variables.</span></span>
