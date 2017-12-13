---
title: Facebook externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Facebook-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
keywords: ASP.NET Core, Facebook, Anmeldung, Authentifizierung
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 826ac826c22dae81e5dbea08a11a62cac0b1068a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="2e295-104">Konfigurieren von Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="2e295-104">Configuring Facebook authentication</span></span>

<span data-ttu-id="2e295-105">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e295-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2e295-106">Dieses Lernprogramm veranschaulicht das Ihren Benutzern zur Anmeldung mit ihrem Facebook-Kontos mit einem Beispiel ASP.NET Core 2.0-Projekt erstellt haben, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="2e295-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="2e295-107">Erstellen eine Facebook-App-ID gemäß wir zunächst die [offizielle Schritte](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="2e295-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="2e295-108">Erstellen Sie die app in Facebook</span><span class="sxs-lookup"><span data-stu-id="2e295-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="2e295-109">Navigieren Sie zu der [Facebook für Entwickler](https://developers.facebook.com) Seite, und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="2e295-109">Navigate to the [Facebook for Developers](https://developers.facebook.com) page and sign in.</span></span> <span data-ttu-id="2e295-110">Wenn Sie eine Facebook-Konto noch nicht haben, verwenden Sie die **registrieren Sie sich für Facebook** Link auf der Anmeldeseite um eines zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2e295-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="2e295-111">Tippen Sie auf die **erstellen App** Schaltfläche in der oberen rechten Ecke zum Erstellen einer neuen App-ID</span><span class="sxs-lookup"><span data-stu-id="2e295-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook für Entwicklerportal öffnen, in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="2e295-113">Das Formular auszufüllen, und tippen Sie auf die **Erstellen von App-ID** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="2e295-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Erstellen Sie ein Formular neue App-ID](index/_static/FBNewAppId.png)

* <span data-ttu-id="2e295-115">Bei **Produkt auswählen** aufzufordern, klicken Sie auf **Set Up** auf die **Facebook-Anmeldung** Karte.</span><span class="sxs-lookup"><span data-stu-id="2e295-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![Setup-Produktseite](index/_static/FBProductSetup.png)

* <span data-ttu-id="2e295-117">Die **Schnellstart** Assistent startet mit **wählen Sie eine Plattform** als erste Seite.</span><span class="sxs-lookup"><span data-stu-id="2e295-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="2e295-118">Der Assistent fürs zu umgehen, indem Sie auf die **Einstellungen** Link im Menü auf der linken Seite:</span><span class="sxs-lookup"><span data-stu-id="2e295-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Skip-Schnellstart](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="2e295-120">Clientanzahl der **OAuth-Clienteinstellungen** Seite:</span><span class="sxs-lookup"><span data-stu-id="2e295-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Seite "Client-OAuth-Einstellungen"](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="2e295-122">Geben Sie die Entwicklung URI mit */signin-facebook* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="2e295-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="2e295-123">Die Facebook-Authentifizierung konfiguriert, die weiter unten in diesem Lernprogramm behandelt automatisch Anforderungen, die bei */signin-facebook* Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="2e295-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="2e295-124">Klicken Sie auf **Änderungen speichern**.</span><span class="sxs-lookup"><span data-stu-id="2e295-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="2e295-125">Klicken Sie auf die **Dashboard** Link im linken Navigationsbereich.</span><span class="sxs-lookup"><span data-stu-id="2e295-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="2e295-126">Auf dieser Seite, notieren Sie sich Ihre `App ID` und Ihre `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="2e295-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="2e295-127">Sie werden sowohl in Ihre ASP.NET Core-Anwendung im nächsten Abschnitt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="2e295-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Facebook-Entwickler-Dashboard](index/_static/FBDashboard.png)

* <span data-ttu-id="2e295-129">Wenn Sie den Standort bereitstellen, müssen Sie wiederholen, die **Facebook-Anmeldung** Setupseite, und registrieren Sie einen neuen öffentliche URI.</span><span class="sxs-lookup"><span data-stu-id="2e295-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="2e295-130">Facebook-App-ID und den geheimen speichern</span><span class="sxs-lookup"><span data-stu-id="2e295-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="2e295-131">Verknüpfen Sie die sensiblen Einstellungen wie Facebook `App ID` und `App Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2e295-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2e295-132">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="2e295-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="2e295-133">Führen Sie die folgenden Befehle zum sicheren Speichern `App ID` und `App Secret` mit dem geheimen Schlüssel-Manager:</span><span class="sxs-lookup"><span data-stu-id="2e295-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="2e295-134">Konfigurieren von Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="2e295-134">Configure Facebook Authentication</span></span>

<span data-ttu-id="2e295-135">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="2e295-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="2e295-136">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="2e295-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2e295-137">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="2e295-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e295-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e295-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e295-139">Hinzufügen den Facebook-Dienst in der `ConfigureServices` Methode in der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="2e295-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e295-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e295-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e295-141">Hinzufügen die Facebook-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="2e295-141">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="2e295-142">Finden Sie unter der [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen von Facebook-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2e295-142">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="2e295-143">Konfigurationsoptionen verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="2e295-143">Configuration options can be used to:</span></span>

* <span data-ttu-id="2e295-144">Fordern Sie unterschiedliche Informationen über den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="2e295-144">Request different information about the user.</span></span>
* <span data-ttu-id="2e295-145">Fügen Sie Abfragezeichenfolgenargumente, um das Anmeldeverfahren anzupassen.</span><span class="sxs-lookup"><span data-stu-id="2e295-145">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="2e295-146">Mit Facebook anmelden</span><span class="sxs-lookup"><span data-stu-id="2e295-146">Sign in with Facebook</span></span>

<span data-ttu-id="2e295-147">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="2e295-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="2e295-148">Sie sehen eine Option zur Anmeldung mit Facebook.</span><span class="sxs-lookup"><span data-stu-id="2e295-148">You see an option to sign in with Facebook.</span></span>

![-Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneFacebook.png)

<span data-ttu-id="2e295-150">Wenn Sie klicken auf **Facebook**, Sie werden für die Authentifizierung mit Facebook weitergeleitet:</span><span class="sxs-lookup"><span data-stu-id="2e295-150">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Seite "Facebook-Authentifizierung"](index/_static/FBLogin.png)

<span data-ttu-id="2e295-152">Facebook-Authentifizierung anfordert öffentliche Profile und e-Mail-Adresse wird standardmäßig an:</span><span class="sxs-lookup"><span data-stu-id="2e295-152">Facebook authentication requests public profile and email address by default:</span></span>

![Seite "Facebook-Authentifizierung"](index/_static/FBLoginDone.png)

<span data-ttu-id="2e295-154">Sobald Sie Ihre Facebook-Anmeldeinformationen eingeben, werden Sie wieder auf Ihrer Website weitergeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="2e295-154">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="2e295-155">Sie sind jetzt angemeldet mit Ihren Facebook-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="2e295-155">You are now logged in using your Facebook credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="2e295-157">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="2e295-157">Troubleshooting</span></span>

* <span data-ttu-id="2e295-158">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="2e295-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2e295-159">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="2e295-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2e295-160">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="2e295-160">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2e295-161">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="2e295-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e295-162">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2e295-162">Next steps</span></span>

* <span data-ttu-id="2e295-163">In diesem Artikel wurde gezeigt, wie Sie mit Facebook authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="2e295-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="2e295-164">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="2e295-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="2e295-165">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `AppSecret` im Facebook-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="2e295-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="2e295-166">Legen Sie die `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="2e295-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2e295-167">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="2e295-167">The configuration system is set up to read keys from environment variables.</span></span>