---
title: Facebook externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Facebook-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: 70a4b2e53be335b8854b0aef3cfbf8f4e21e6ebe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="853a4-103">Facebook externe Anmeldung Setup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="853a4-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="853a4-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="853a4-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="853a4-105">Dieses Lernprogramm veranschaulicht das Ihren Benutzern zur Anmeldung mit ihrem Facebook-Kontos mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="853a4-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="853a4-106">Erstellen eine Facebook-App-ID gemäß wir zunächst die [offizielle Schritte](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="853a4-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="853a4-107">Erstellen Sie die app in Facebook</span><span class="sxs-lookup"><span data-stu-id="853a4-107">Create the app in Facebook</span></span>

*  <span data-ttu-id="853a4-108">Navigieren Sie zu der [app von Facebook | Entwickler](https://developers.facebook.com/apps/) Seite, und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="853a4-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="853a4-109">Wenn Sie eine Facebook-Konto noch nicht haben, verwenden Sie die **registrieren Sie sich für Facebook** Link auf der Anmeldeseite um eines zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="853a4-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="853a4-110">Tippen Sie auf die **Hinzufügen einer neuen App** Schaltfläche in der oberen rechten Ecke zum Erstellen einer neuen App-ID</span><span class="sxs-lookup"><span data-stu-id="853a4-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook für Entwicklerportal öffnen, in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="853a4-112">Das Formular auszufüllen, und tippen Sie auf die **Erstellen von App-ID** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="853a4-112">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Erstellen Sie ein Formular neue App-ID](index/_static/FBNewAppId.png)

* <span data-ttu-id="853a4-114">Auf der **Produkt auswählen** auf **Set Up** auf die **Facebook-Anmeldung** Karte.</span><span class="sxs-lookup"><span data-stu-id="853a4-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Setup-Produktseite](index/_static/FBProductSetup.png)

* <span data-ttu-id="853a4-116">Die **Schnellstart** Assistent startet mit **wählen Sie eine Plattform** als erste Seite.</span><span class="sxs-lookup"><span data-stu-id="853a4-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="853a4-117">Der Assistent fürs zu umgehen, indem Sie auf die **Einstellungen** Link im Menü auf der linken Seite:</span><span class="sxs-lookup"><span data-stu-id="853a4-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Skip-Schnellstart](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="853a4-119">Clientanzahl der **OAuth-Clienteinstellungen** Seite:</span><span class="sxs-lookup"><span data-stu-id="853a4-119">You are presented with the **Client OAuth Settings** page:</span></span>

![Seite "Client-OAuth-Einstellungen"](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="853a4-121">Geben Sie die Entwicklung URI mit */signin-facebook* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="853a4-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="853a4-122">Die Facebook-Authentifizierung konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei */signin-facebook* Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="853a4-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="853a4-123">Klicken Sie auf **Änderungen speichern**.</span><span class="sxs-lookup"><span data-stu-id="853a4-123">Click **Save Changes**.</span></span>

* <span data-ttu-id="853a4-124">Klicken Sie auf die **Dashboard** Link im linken Navigationsbereich.</span><span class="sxs-lookup"><span data-stu-id="853a4-124">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="853a4-125">Auf dieser Seite, notieren Sie sich Ihre `App ID` und Ihre `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="853a4-125">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="853a4-126">Sie werden sowohl in Ihre ASP.NET Core-Anwendung im nächsten Abschnitt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="853a4-126">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Facebook-Entwickler-Dashboard](index/_static/FBDashboard.png)

* <span data-ttu-id="853a4-128">Wenn Sie den Standort bereitstellen, müssen Sie wiederholen, die **Facebook-Anmeldung** Setupseite, und registrieren Sie einen neuen öffentliche URI.</span><span class="sxs-lookup"><span data-stu-id="853a4-128">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="853a4-129">Facebook-App-ID und den geheimen speichern</span><span class="sxs-lookup"><span data-stu-id="853a4-129">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="853a4-130">Verknüpfen Sie die sensiblen Einstellungen wie Facebook `App ID` und `App Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="853a4-130">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="853a4-131">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="853a4-131">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="853a4-132">Führen Sie die folgenden Befehle zum sicheren Speichern `App ID` und `App Secret` mit dem geheimen Schlüssel-Manager:</span><span class="sxs-lookup"><span data-stu-id="853a4-132">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="853a4-133">Konfigurieren von Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="853a4-133">Configure Facebook Authentication</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="853a4-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="853a4-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="853a4-135">Hinzufügen den Facebook-Dienst in der `ConfigureServices` Methode in der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="853a4-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="853a4-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="853a4-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="853a4-137">Installieren der [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) Paket.</span><span class="sxs-lookup"><span data-stu-id="853a4-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="853a4-138">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="853a4-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="853a4-139">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="853a4-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="853a4-140">Hinzufügen die Facebook-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="853a4-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

* * *
<span data-ttu-id="853a4-141">Finden Sie unter der [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen von Facebook-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="853a4-141">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="853a4-142">Konfigurationsoptionen verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="853a4-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="853a4-143">Fordern Sie unterschiedliche Informationen über den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="853a4-143">Request different information about the user.</span></span>
* <span data-ttu-id="853a4-144">Fügen Sie Abfragezeichenfolgenargumente, um das Anmeldeverfahren anzupassen.</span><span class="sxs-lookup"><span data-stu-id="853a4-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="853a4-145">Mit Facebook anmelden</span><span class="sxs-lookup"><span data-stu-id="853a4-145">Sign in with Facebook</span></span>

<span data-ttu-id="853a4-146">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="853a4-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="853a4-147">Sie sehen eine Option zur Anmeldung mit Facebook.</span><span class="sxs-lookup"><span data-stu-id="853a4-147">You see an option to sign in with Facebook.</span></span>

![-Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneFacebook.png)

<span data-ttu-id="853a4-149">Wenn Sie klicken auf **Facebook**, Sie werden für die Authentifizierung mit Facebook weitergeleitet:</span><span class="sxs-lookup"><span data-stu-id="853a4-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Seite "Facebook-Authentifizierung"](index/_static/FBLogin.png)

<span data-ttu-id="853a4-151">Facebook-Authentifizierung anfordert öffentliche Profile und e-Mail-Adresse wird standardmäßig an:</span><span class="sxs-lookup"><span data-stu-id="853a4-151">Facebook authentication requests public profile and email address by default:</span></span>

![Seite "Facebook-Authentifizierung"](index/_static/FBLoginDone.png)

<span data-ttu-id="853a4-153">Sobald Sie Ihre Facebook-Anmeldeinformationen eingeben, werden Sie wieder auf Ihrer Website weitergeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="853a4-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="853a4-154">Sie sind jetzt angemeldet mit Ihren Facebook-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="853a4-154">You are now logged in using your Facebook credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="853a4-156">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="853a4-156">Troubleshooting</span></span>

* <span data-ttu-id="853a4-157">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="853a4-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="853a4-158">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="853a4-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="853a4-159">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="853a4-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="853a4-160">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="853a4-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="853a4-161">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="853a4-161">Next steps</span></span>

* <span data-ttu-id="853a4-162">In diesem Artikel wurde gezeigt, wie Sie mit Facebook authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="853a4-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="853a4-163">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="853a4-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="853a4-164">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `AppSecret` im Facebook-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="853a4-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="853a4-165">Legen Sie die `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="853a4-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="853a4-166">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="853a4-166">The configuration system is set up to read keys from environment variables.</span></span>
