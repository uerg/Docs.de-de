---
title: Google externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Google externe Anmeldung Setup in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: d7c5e1f1564c1bbf345e28fa76ce0b11ae65b0d4
ms.sourcegitcommit: 93785b6b29a4996066fba05149348f1bdf881263
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="4e3f0-104">Konfigurieren von Google-Authentifizierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e3f0-104">Configuring Google authentication in ASP.NET Core</span></span>

<span data-ttu-id="4e3f0-105">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4e3f0-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4e3f0-106">Dieses Lernprogramm veranschaulicht das Ihren Benutzern zur Anmeldung mit ihrem Google +-Kontos mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="4e3f0-106">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="4e3f0-107">Wir beginnen, indem Sie die folgenden der [offizielle Schritte](https://developers.google.com/identity/sign-in/web/devconsole-project) zum Erstellen einer neuen app im Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-107">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="4e3f0-108">Erstellen der app im Google-API-Konsole</span><span class="sxs-lookup"><span data-stu-id="4e3f0-108">Create the app in Google API Console</span></span>

* <span data-ttu-id="4e3f0-109">Navigieren Sie zu [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-109">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="4e3f0-110">Wenn Sie eine Google-Konto noch nicht haben, verwenden **Weitere Optionen** > **[Konto erstellen](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  Link, um eines zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-110">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google-API-Konsole](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="4e3f0-112">Sie werden zur umgeleitet **API-Manager-Bibliothek** Seite:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-112">You are redirected to **API Manager Library** page:</span></span>

![Seite "API-Manager-Bibliothek"](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="4e3f0-114">Tippen Sie auf **erstellen** , und geben Sie Ihre **Projektname**:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-114">Tap **Create** and enter your **Project name**:</span></span>

![Dialogfeld "Neues Projekt"](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="4e3f0-116">Nach dem Akzeptieren des Dialogfeld ", werden Sie wieder auf der Seite" Bibliothek "können Sie Funktionen für Ihre neue app auswählen weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-116">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="4e3f0-117">Suchen **Google + API** in der Liste aus und klicken Sie auf den zugehörigen Link der API-Funktion hinzu:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-117">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Seite "API-Manager-Bibliothek"](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="4e3f0-119">Die Seite für die neu hinzugefügte-API wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-119">The page for the newly added API is displayed.</span></span> <span data-ttu-id="4e3f0-120">Tippen Sie auf **aktivieren** Google + Anmeldung Funktion in Ihrer app hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-120">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API-Manager Google + API-Seite](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="4e3f0-122">Nach dem Aktivieren der API, tippen Sie auf **Anmeldeinformationen erstellen** so konfigurieren Sie die geheimen Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-122">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API-Manager Google + API-Seite](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="4e3f0-124">Wählen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-124">Choose:</span></span>
   * <span data-ttu-id="4e3f0-125">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="4e3f0-125">**Google+ API**</span></span>
   * <span data-ttu-id="4e3f0-126">**Webserver (z. B. node.js, Tomcat)**, und</span><span class="sxs-lookup"><span data-stu-id="4e3f0-126">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="4e3f0-127">**Benutzerdaten**:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-127">**User data**:</span></span>

![API-Manager anmeldeinformationsseite: Informieren Sie sich über welche Art von Anmeldeinformationen benötigen Bereich](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="4e3f0-129">Tippen Sie auf **welche Anmeldeinformationen benötige ich?** die gelangen Sie zu der zweite Schritt des app-Konfiguration **Erstellen einer OAuth 2.0-Client-ID**:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-129">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API-Manager anmeldeinformationsseite: Erstellen einer OAuth 2.0-Client-ID](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="4e3f0-131">Da wir eine Google +-Projekt, mit nur einem Feature (Anmelden) erstellen, können wir geben Sie den gleichen **Namen** für die OAuth 2.0-Client-ID als diejenige, die wir für das Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-131">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="4e3f0-132">Geben Sie die Entwicklung URI mit */signin-google* angefügt, die in der **autorisierte umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="4e3f0-132">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="4e3f0-133">Die Google-Authentifizierung konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei */signin-google* Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-133">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="4e3f0-134">Drücken Sie TAB, um das Hinzufügen der **autorisierte umleitungs-URIs** Eintrag.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-134">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="4e3f0-135">Tippen Sie auf **Client-ID erstellen**, dem gelangen Sie mit dem dritten Schritt **richten Sie den Bildschirm der OAuth 2.0-Zustimmung**:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-135">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API-Manager anmeldeinformationsseite: Richten Sie den Bildschirm der OAuth 2.0-Zustimmung](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="4e3f0-137">Geben Sie Ihre öffentlich **e-Mail-Adresse** und **Produktname** für Ihre app angezeigt wird, wenn Google + melden Sie sich der Benutzer aufgefordert wird.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-137">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="4e3f0-138">Weitere Optionen sind verfügbar unter **Weitere Optionen zur Anpassung der**.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-138">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="4e3f0-139">Tippen Sie auf **Fortfahren** bis zum letzten Schritt fortfahren **Anmeldeinformationen herunterladen**:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-139">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API-Manager anmeldeinformationsseite: Herunterladen von Anmeldeinformationen](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="4e3f0-141">Tippen Sie auf **herunterladen** eine JSON-Datei mit der Anwendung vertrauliche Informationen gespeichert und **Fertig** um die Erstellung der neuen app abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-141">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="4e3f0-142">Bei der Bereitstellung der Website müssen Sie rufen die **Konsole der Google-** und eine neue öffentliche Url zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-142">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="4e3f0-143">Store Google ClientID und ClientSecret</span><span class="sxs-lookup"><span data-stu-id="4e3f0-143">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="4e3f0-144">Verknüpfen Sie die sensiblen Einstellungen wie Google `Client ID` und `Client Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="4e3f0-144">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="4e3f0-145">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-145">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="4e3f0-146">Die Werte für diese Token finden Sie in der JSON-Datei heruntergeladen, die im vorherigen Schritt unter `web.client_id` und `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-146">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="4e3f0-147">Konfigurieren von Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="4e3f0-147">Configure Google Authentication</span></span>

<span data-ttu-id="4e3f0-148">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) Paket installiert ist.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-148">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="4e3f0-149">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-149">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="4e3f0-150">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-150">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e3f0-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e3f0-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e3f0-152">Fügen Sie den Google-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-152">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e3f0-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e3f0-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e3f0-154">Fügen Sie die Google-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-154">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="4e3f0-155">Finden Sie unter der [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Google-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-155">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="4e3f0-156">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-156">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="4e3f0-157">Melden Sie sich mit Google</span><span class="sxs-lookup"><span data-stu-id="4e3f0-157">Sign in with Google</span></span>

<span data-ttu-id="4e3f0-158">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-158">Run your application and click **Log in**.</span></span> <span data-ttu-id="4e3f0-159">Eine Option zur Anmeldung mit Google angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-159">An option to sign in with Google appears:</span></span>

![In Microsoft Edge ausgeführte Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneGoogle.png)

<span data-ttu-id="4e3f0-161">Wenn Sie die Google klicken, werden Sie Google für die Authentifizierung weitergeleitet:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-161">When you click on Google, you are redirected to Google for authentication:</span></span>

![Dialogfeld für Google-Authentifizierung](index/_static/GoogleLogin.png)

<span data-ttu-id="4e3f0-163">Nach dem Eingeben Ihrer Google-Anmeldeinformationen, werden dann Sie zurück zur Website umgeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-163">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="4e3f0-164">Sie sind jetzt angemeldet mithilfe Ihrer Google-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="4e3f0-164">You are now logged in using your Google credentials:</span></span>

![In Microsoft Edge ausgeführte Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="4e3f0-166">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="4e3f0-166">Troubleshooting</span></span>

* <span data-ttu-id="4e3f0-167">Erhalten Sie eine `403 (Forbidden)` Fehlerseite aus Ihrer eigenen app, die beim Ausführen in den Entwicklungsmodus (oder Unterbrechung des Debuggers mit den gleichen Fehler) sicher, dass **Google + API** aktiviert wurde, der **-API-Manager-Bibliothek** anhand der aufgeführten Schritte [weiter oben auf dieser Seite](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="4e3f0-167">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="4e3f0-168">Wenn die Anmeldung funktioniert nicht, und Sie werden keine Fehler erhalten, wechseln Sie zu den Entwicklungsmodus, um das Problem Debuggen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-168">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="4e3f0-169">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-169">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="4e3f0-170">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-170">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="4e3f0-171">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-171">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="4e3f0-172">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-172">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e3f0-173">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4e3f0-173">Next steps</span></span>

* <span data-ttu-id="4e3f0-174">In diesem Artikel wurde gezeigt, wie Sie mit Google authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-174">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="4e3f0-175">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="4e3f0-175">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="4e3f0-176">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ClientSecret` in der Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-176">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="4e3f0-177">Legen Sie die `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-177">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="4e3f0-178">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="4e3f0-178">The configuration system is set up to read keys from environment variables.</span></span>
