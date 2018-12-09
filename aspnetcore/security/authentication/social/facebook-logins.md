---
title: Facebook externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Facebook-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 8bb22dc6df9879e827ff9a5ac11e9e3ad5346dc2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121504"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="3455a-103">Facebook externe Anmeldung Setup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3455a-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="3455a-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3455a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3455a-105">In diesem Tutorial erfahren Sie, wie Sie Benutzern die Anmeldung mit ihren Facebook-Kontos mit einem ASP.NET Core 2.0-Beispielprojekt auf erstellt ermöglichen, sich die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3455a-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="3455a-106">Facebook-Authentifizierung erfordert die [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="3455a-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="3455a-107">Zunächst erstellen eine Facebook-App-ID gemäß der [offizielle Schritte](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="3455a-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="3455a-108">Erstellen einer app in Facebook</span><span class="sxs-lookup"><span data-stu-id="3455a-108">Create the app in Facebook</span></span>

* <span data-ttu-id="3455a-109">Navigieren Sie zu der [Facebook-Entwickler-app](https://developers.facebook.com/apps/) Seite, und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="3455a-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="3455a-110">Wenn Sie bereits über ein Facebook-Konto haben, verwenden Sie die **für Facebook registrieren** Link auf der Anmeldeseite erstellen.</span><span class="sxs-lookup"><span data-stu-id="3455a-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="3455a-111">Tippen Sie auf die **Hinzufügen einer neuen App** -Schaltfläche in der oberen rechten Ecke, um eine neue App-ID zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3455a-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook Entwicklerportal in Microsoft Edge geöffnete](index/_static/FBMyApps.png)

* <span data-ttu-id="3455a-113">Füllen Sie das Formular, und tippen Sie auf die **Create App ID** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="3455a-113">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Erstellen Sie ein Formular neue App-ID](index/_static/FBNewAppId.png)

* <span data-ttu-id="3455a-115">Auf der **wählen Sie ein Produkt** auf **Set Up** auf die **Facebook-Anmeldung** Karte.</span><span class="sxs-lookup"><span data-stu-id="3455a-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Setup-Produktseite](index/_static/FBProductSetup.png)

* <span data-ttu-id="3455a-117">Die **Schnellstart** der Assistent wird gestartet, mit **wählen Sie eine Plattform** als erste Seite.</span><span class="sxs-lookup"><span data-stu-id="3455a-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="3455a-118">Der Assistent jetzt zu umgehen, indem Sie auf die **Einstellungen** Link im Menü auf der linken Seite:</span><span class="sxs-lookup"><span data-stu-id="3455a-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Skip-Schnellstart](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="3455a-120">Werden Ihnen die **Client OAuth Settings** Seite:</span><span class="sxs-lookup"><span data-stu-id="3455a-120">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Seite "Client-OAuth-Einstellungen"](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="3455a-122">Geben Sie Ihre Entwicklung URI mit */signin-facebook* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="3455a-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="3455a-123">Die Facebook-Authentifizierung konfiguriert, die später in diesem Tutorial behandelt die Anforderungen an automatisch */signin-facebook* Route zum Implementieren von OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="3455a-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="3455a-124">Der URI */signin-facebook* ist als der standardrückruf von der Facebook-Authentifizierungsanbieter festgelegt.</span><span class="sxs-lookup"><span data-stu-id="3455a-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="3455a-125">Sie können den standardrückruf-URI ändern, während der Konfiguration der Facebook-Authentifizierung-Middleware über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) -Klasse.</span><span class="sxs-lookup"><span data-stu-id="3455a-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="3455a-126">Klicken Sie auf **speichern Änderungen**.</span><span class="sxs-lookup"><span data-stu-id="3455a-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="3455a-127">Klicken Sie auf **Einstellungen** > **grundlegende** Link im linken Navigationsbereich.</span><span class="sxs-lookup"><span data-stu-id="3455a-127">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="3455a-128">Notieren Sie sich auf dieser Seite Ihre `App ID` und Ihre `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="3455a-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="3455a-129">Im nächsten Abschnitt fügen Sie beide Zertifikate in Ihrer ASP.NET Core-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="3455a-129">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="3455a-130">Bei der Bereitstellung der Website müssen Sie noch einmal die **Facebook-Anmeldung** Setupseite, und registrieren Sie einen neuen öffentlichen URI.</span><span class="sxs-lookup"><span data-stu-id="3455a-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="3455a-131">Store-Facebook-App-ID und App-Geheimnis</span><span class="sxs-lookup"><span data-stu-id="3455a-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="3455a-132">Verknüpfen Sie die sensible Einstellungen wie etwa Facebook `App ID` und `App Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3455a-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="3455a-133">Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="3455a-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="3455a-134">Führen Sie die folgenden Befehle zum sicheren Speichern von `App ID` und `App Secret` mit Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="3455a-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="3455a-135">Konfigurieren der Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="3455a-135">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3455a-136">Hinzufügen des Facebook-Diensts in der `ConfigureServices` -Methode in der die *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="3455a-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3455a-137">Installieren Sie die [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) Paket.</span><span class="sxs-lookup"><span data-stu-id="3455a-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="3455a-138">Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="3455a-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="3455a-139">Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:</span><span class="sxs-lookup"><span data-stu-id="3455a-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="3455a-140">Hinzufügen die Facebook-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="3455a-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="3455a-141">Finden Sie unter den [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Facebook-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3455a-141">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="3455a-142">Optionen für die Konfiguration können zu verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="3455a-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="3455a-143">Fordern Sie verschiedene Informationen über den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="3455a-143">Request different information about the user.</span></span>
* <span data-ttu-id="3455a-144">Fügen Sie die Abfragezeichenfolge-Argumente zum Anpassen der Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="3455a-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="3455a-145">Melden Sie sich mit Facebook</span><span class="sxs-lookup"><span data-stu-id="3455a-145">Sign in with Facebook</span></span>

<span data-ttu-id="3455a-146">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**.</span><span class="sxs-lookup"><span data-stu-id="3455a-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="3455a-147">Daraufhin wird eine Option aus, um sich mit Facebook anmelden.</span><span class="sxs-lookup"><span data-stu-id="3455a-147">You see an option to sign in with Facebook.</span></span>

![Web-Anwendung: Benutzer nicht authentifiziert.](index/_static/DoneFacebook.png)

<span data-ttu-id="3455a-149">Wenn Sie beim Klicken auf **Facebook**, Sie werden zur Authentifizierung an Facebook umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="3455a-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Seite der Facebook-Authentifizierung](index/_static/FBLogin.png)

<span data-ttu-id="3455a-151">Facebook-Authentifizierung standardmäßige öffentliche Profil und e-Mail-Adresse anfordert:</span><span class="sxs-lookup"><span data-stu-id="3455a-151">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook-Authentifizierung Seite zustimmungsbildschirm](index/_static/FBLoginDone.png)

<span data-ttu-id="3455a-153">Nachdem Sie Ihre Facebook-Anmeldeinformationen eingeben, werden Sie an Ihrem Standort weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="3455a-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="3455a-154">Sie sind jetzt angemeldet, sich mit Ihren Facebook-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="3455a-154">You are now logged in using your Facebook credentials:</span></span>

![Web-Anwendung: Benutzerauthentifizierung](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="3455a-156">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="3455a-156">Troubleshooting</span></span>

* <span data-ttu-id="3455a-157">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option 'SignInScheme' muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="3455a-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="3455a-158">Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="3455a-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="3455a-159">Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="3455a-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="3455a-160">Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="3455a-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3455a-161">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="3455a-161">Next steps</span></span>

* <span data-ttu-id="3455a-162">In diesem Artikel wurde gezeigt, wie Sie mit Facebook authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="3455a-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="3455a-163">Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3455a-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="3455a-164">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `AppSecret` im Facebook-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="3455a-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="3455a-165">Legen Sie die `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret` Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="3455a-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="3455a-166">Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="3455a-166">The configuration system is set up to read keys from environment variables.</span></span>
