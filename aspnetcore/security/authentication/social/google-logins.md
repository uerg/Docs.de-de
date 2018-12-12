---
title: Google externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Google-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284486"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="b9bc1-103">Google externe Anmeldung Setup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9bc1-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="b9bc1-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b9bc1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b9bc1-105">In diesem Tutorial erfahren Sie, wie Sie Benutzern die Anmeldung mit ihrer Google +-Kontos mit einem ASP.NET Core 2.0-Beispielprojekt auf erstellt ermöglichen, sich die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b9bc1-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="b9bc1-106">Befolgen Sie zunächst die [offizielle Schritte](https://developers.google.com/identity/sign-in/web/devconsole-project) zum Erstellen einer neuen app im Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="b9bc1-107">Erstellen einer app in Google-API-Konsole</span><span class="sxs-lookup"><span data-stu-id="b9bc1-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="b9bc1-108">Navigieren Sie zu [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="b9bc1-109">Wenn Sie bereits über ein Google-Konto haben, verwenden Sie **Weitere Optionen** > **[Konto erstellen](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  Link zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google-API-Konsole](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="b9bc1-111">Sie werden zur **-API-Manager-Bibliothek** Seite:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-111">You are redirected to **API Manager Library** page:</span></span>

![Angebotsseite auf der Seite für die API-Manager-Bibliothek](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="b9bc1-113">Tippen Sie auf **erstellen** , und geben Sie Ihre **Projektname**:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-113">Tap **Create** and enter your **Project name**:</span></span>

![Dialogfeld "Neues Projekt"](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="b9bc1-115">Nach dem Akzeptieren des Dialogfelds werden Sie an der Library-Seite können Sie auf die Funktionen für Ihre neue app weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="b9bc1-116">Suchen **Google + API** in der Liste und klicken Sie auf den zugehörigen Link, um die API-Funktion hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Suchen Sie nach "Google +-API" auf der Seite für die API-Manager-Bibliothek](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="b9bc1-118">Die Seite für die neu hinzugefügte-API wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="b9bc1-119">Tippen Sie auf **aktivieren** um Google +-Anmeldung in der Funktion Ihrer app hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Angebotsseite auf der Seite für die API-Manager Google +-API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="b9bc1-121">Nach der Aktivierung der API, tippen Sie auf **Anmeldeinformationen erstellen** so konfigurieren Sie die geheimen Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Erstellen Sie Schaltfläche "Anmeldeinformationen" auf API-Manager Google +-API-Seite](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="b9bc1-123">Wählen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-123">Choose:</span></span>
  * <span data-ttu-id="b9bc1-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="b9bc1-124">**Google+ API**</span></span>
  * <span data-ttu-id="b9bc1-125">**Webserver (z. B. node.js, Tomcat)**, und</span><span class="sxs-lookup"><span data-stu-id="b9bc1-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="b9bc1-126">**Benutzerdaten**:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-126">**User data**:</span></span>

![Seite für die API-Manager-Anmeldeinformationen: Erfahren Sie, welche Art von Anmeldeinformationen benötigen Bereich](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="b9bc1-128">Tippen Sie auf **welche Anmeldeinformationen benötige ich?** die gelangen Sie zum zweiten Schritt des app-Konfiguration, **erstellen Sie eine OAuth 2.0-Client-ID**:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Seite für die API-Manager-Anmeldeinformationen: Erstellen Sie eine OAuth 2.0-Client-ID](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="b9bc1-130">Da wir ein Google +-Projekt, mit nur einem Merkmal (Anmelden) erstellen, können wir geben Sie den gleichen **Namen** für die OAuth 2.0-Client-ID, das wir für das Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="b9bc1-131">Geben Sie Ihre Entwicklung URI mit `/signin-google` angefügt, die in der **autorisierte weiterleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="b9bc1-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="b9bc1-132">Die Google-Authentifizierung konfiguriert, die später in diesem Tutorial behandelt die Anforderungen an automatisch `/signin-google` Route zum Implementieren von OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="b9bc1-133">Der URI-Segment `/signin-google` als den standardrückruf des Google-Authentifizierungsanbieter festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="b9bc1-134">Sie können den standardrückruf-URI ändern, während der Konfiguration die Middleware für die Google-Authentifizierung über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Klasse.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="b9bc1-135">Tabstopp drücken, um das Hinzufügen der **autorisierte weiterleitungs-URIs** Eintrag.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="b9bc1-136">Tippen Sie auf **Client-ID erstellen**, die mit dem dritten Schritt gelangen Sie **richten Sie die OAuth 2.0-Zustimmungsdialogfeld**:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Seite für die API-Manager-Anmeldeinformationen: Einrichten der OAuth 2.0-zustimmungsbildschirm](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="b9bc1-138">Geben Sie Ihre öffentlich **e-Mail-Adresse** und **Produktname** für Ihre app angezeigt wird, wenn es sich bei Google + der Benutzer zur Anmeldung aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="b9bc1-139">Weitere Optionen sind verfügbar unter **Weitere Anpassungsoptionen**.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="b9bc1-140">Tippen Sie auf **Weiter** bis zum letzten Schritt fortfahren **Anmeldeinformationen herunterladen**:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Seite für die API-Manager-Anmeldeinformationen: Anmeldeinformationen herunterladen](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="b9bc1-142">Tippen Sie auf **herunterladen** um eine JSON-Datei mit Geheimnissen aus Anwendungen, zu speichern und **Fertig** um die Erstellung der neuen app abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="b9bc1-143">Bei der Bereitstellung der Website müssen Sie noch einmal die **Webkonsole von Google** und registrieren Sie eine neue öffentliche Url.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="b9bc1-144">Store-Google-ClientID und ClientSecret</span><span class="sxs-lookup"><span data-stu-id="b9bc1-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="b9bc1-145">Verknüpfen Sie sensible Einstellungen wie Google `Client ID` und `Client Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b9bc1-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b9bc1-146">Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="b9bc1-147">Die Werte für diese Token finden Sie in der JSON-Datei heruntergeladen, die im vorherigen Schritt unter `web.client_id` und `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="b9bc1-148">Konfigurieren der Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b9bc1-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b9bc1-149">Fügen Sie den Google-Dienst in der `ConfigureServices` -Methode in der *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b9bc1-150">Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) -Paket ist installiert.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="b9bc1-151">Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b9bc1-152">Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="b9bc1-153">Fügen Sie die Google-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="b9bc1-154">Finden Sie unter den [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Google-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="b9bc1-155">Dies kann verwendet werden, um verschiedene Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="b9bc1-156">Mit Google anmelden</span><span class="sxs-lookup"><span data-stu-id="b9bc1-156">Sign in with Google</span></span>

<span data-ttu-id="b9bc1-157">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="b9bc1-158">Eine Option aus, um sich mit Google anmelden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-158">An option to sign in with Google appears:</span></span>

![Webanwendung, die in Microsoft Edge ausgeführt wird: Benutzer nicht authentifiziert.](index/_static/DoneGoogle.png)

<span data-ttu-id="b9bc1-160">Wenn Sie Google klicken, werden Sie für die Authentifizierung an Google umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Dialogfeld "Google-Authentifizierung"](index/_static/GoogleLogin.png)

<span data-ttu-id="b9bc1-162">Nach dem Eingeben Ihrer Google-Anmeldeinformationen an, werden dann Sie zurück zur Website weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b9bc1-163">Sie sind jetzt angemeldet, sich mit Ihren Google-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="b9bc1-163">You are now logged in using your Google credentials:</span></span>

![Webanwendung, die in Microsoft Edge ausgeführt wird: Benutzerauthentifizierung](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b9bc1-165">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="b9bc1-165">Troubleshooting</span></span>

* <span data-ttu-id="b9bc1-166">Wenn Sie erhalten eine `403 (Forbidden)` Fehlerseite aus Ihrer eigenen app beim Ausführen im Entwicklungsmodus (oder unterbrechen im Debugger mit dem gleichen Fehler), sicher, dass **Google + API** in aktiviert wurde die **-API-Manager-Bibliothek** durch die folgenden Schritte [weiter oben auf dieser Seite](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="b9bc1-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="b9bc1-167">Wenn die Anmeldung funktioniert nicht, und Sie sind keine Fehler erhalten, wechseln Sie in Development-Modus, um das Problem einfacher Debuggen lässt.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="b9bc1-168">**ASP.NET Core 2.x nur:** Wenn Identität nicht, durch den Aufruf konfiguriert ist `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: Die Option 'SignInScheme' muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b9bc1-169">Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b9bc1-170">Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b9bc1-171">Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9bc1-172">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b9bc1-172">Next steps</span></span>

* <span data-ttu-id="b9bc1-173">In diesem Artikel wurde gezeigt, wie Sie mit Google authentifiziert werden können.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="b9bc1-174">Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b9bc1-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b9bc1-175">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ClientSecret` in der Google-API-Konsole.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="b9bc1-176">Legen Sie die `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret` Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="b9bc1-177">Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="b9bc1-177">The configuration system is set up to read keys from environment variables.</span></span>
