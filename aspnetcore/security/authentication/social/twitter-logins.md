---
title: Twitter-Einrichtung der externen Anmeldung mit ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Twitter-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735803"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="73c0a-103">Twitter-Einrichtung der externen Anmeldung mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73c0a-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="73c0a-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73c0a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="73c0a-105">In diesem Tutorial erfahren Sie, wie Sie es Benutzern ermöglichen [melden Sie sich mit ihrem Twitter-Konto](https://dev.twitter.com/web/sign-in/desktop-browser) erstellt mit einem ASP.NET Core 2.0-Beispielprojekt die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="73c0a-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="73c0a-106">Erstellen einer app in Twitter</span><span class="sxs-lookup"><span data-stu-id="73c0a-106">Create the app in Twitter</span></span>

* <span data-ttu-id="73c0a-107">Navigieren Sie zu [ https://apps.twitter.com/ ](https://apps.twitter.com/) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="73c0a-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="73c0a-108">Wenn Sie noch kein Twitter-Konto besitzen, verwenden Sie die **[registrieren Sie sich jetzt](https://twitter.com/signup)** Link zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="73c0a-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="73c0a-109">Nach der Anmeldung die **Anwendungsverwaltung** Seite wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="73c0a-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Twitter Application Management in Microsoft Edge öffnen](index/_static/TwitterAppManage.png)

* <span data-ttu-id="73c0a-111">Tippen Sie auf **Create New App** , und füllen Sie die Anwendung **Namen**, **Beschreibung** und öffentliche **Website** URI (Dies kann temporär sein bis Registrieren Sie den Domänennamen):</span><span class="sxs-lookup"><span data-stu-id="73c0a-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Erstellen einer Anwendungsseite](index/_static/TwitterCreate.png)

* <span data-ttu-id="73c0a-113">Geben Sie Ihre Entwicklung URI mit `/signin-twitter` angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="73c0a-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="73c0a-114">Das Twitter-Authentifizierungsschema, das später in diesem Tutorial konfiguriert automatisch behandelt Anforderungen an `/signin-twitter` Route zum Implementieren von OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="73c0a-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="73c0a-115">Der URI-Segment `/signin-twitter` ist als der standardrückruf des Authentifizierungsanbieters Twitter festgelegt.</span><span class="sxs-lookup"><span data-stu-id="73c0a-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="73c0a-116">Sie können den standardrückruf-URI ändern, während der Konfiguration der Twitter-Authentifizierung-Middleware über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) -Klasse.</span><span class="sxs-lookup"><span data-stu-id="73c0a-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="73c0a-117">Geben Sie im Rest des Formulars, und tippen Sie auf **Erstellen Ihrer Twitter-Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="73c0a-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="73c0a-118">Die Anwendungsdetails der neuen werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="73c0a-118">New application details are displayed:</span></span>

  ![Registerkarte "Details" auf der Seite "Anwendung"](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="73c0a-120">Bei der Bereitstellung der Website müssen Sie noch einmal die **Anwendungsverwaltung** Seite, und registrieren Sie einen neuen öffentlichen URI.</span><span class="sxs-lookup"><span data-stu-id="73c0a-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="73c0a-121">Das Speichern von Twitter-Consumerkey- und ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="73c0a-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="73c0a-122">Verknüpfen Sie sensible Einstellungen wie Twitter `Consumer Key` und `Consumer Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="73c0a-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="73c0a-123">Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="73c0a-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="73c0a-124">Diese Token finden Sie auf die **Keys and Access Tokens** Registerkarte nach der Erstellung Ihrer neuen Twitter-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="73c0a-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Registerkarte "Schlüssel und Zugriffstoken"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="73c0a-126">Konfigurieren der Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="73c0a-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="73c0a-127">Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="73c0a-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="73c0a-128">Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="73c0a-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="73c0a-129">Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:</span><span class="sxs-lookup"><span data-stu-id="73c0a-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73c0a-130">Fügen Sie den Twitter-Dienst in der `ConfigureServices` -Methode in der *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="73c0a-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73c0a-131">Hinzufügen die Twitter-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="73c0a-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="73c0a-132">Finden Sie unter den [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Twitter-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="73c0a-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="73c0a-133">Dies kann verwendet werden, um verschiedene Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="73c0a-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="73c0a-134">Melden Sie sich mit Twitter</span><span class="sxs-lookup"><span data-stu-id="73c0a-134">Sign in with Twitter</span></span>

<span data-ttu-id="73c0a-135">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**.</span><span class="sxs-lookup"><span data-stu-id="73c0a-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="73c0a-136">Eine Option aus, um sich mit Twitter anmelden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="73c0a-136">An option to sign in with Twitter appears:</span></span>

![-Webanwendung: Benutzer nicht authentifiziert.](index/_static/DoneTwitter.png)

<span data-ttu-id="73c0a-138">Durch Klicken auf **Twitter** an Twitter umgeleitet wird, für die Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="73c0a-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter-Authentifizierungsseite](index/_static/TwitterLogin.png)

<span data-ttu-id="73c0a-140">Geben Sie Ihre Twitter-Anmeldeinformationen, werden Sie zurück zur Website weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="73c0a-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="73c0a-141">Sie sind jetzt angemeldet, sich mit Ihren Twitter-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="73c0a-141">You are now logged in using your Twitter credentials:</span></span>

![-Webanwendung: Benutzerauthentifizierung](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="73c0a-143">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="73c0a-143">Troubleshooting</span></span>

* <span data-ttu-id="73c0a-144">**ASP.NET Core 2.x nur:** Wenn Identität nicht, durch den Aufruf konfiguriert ist `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: Die Option 'SignInScheme' muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="73c0a-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="73c0a-145">Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="73c0a-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="73c0a-146">Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="73c0a-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="73c0a-147">Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="73c0a-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73c0a-148">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="73c0a-148">Next steps</span></span>

* <span data-ttu-id="73c0a-149">In diesem Artikel wurde gezeigt, wie Sie Twitter authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="73c0a-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="73c0a-150">Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="73c0a-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="73c0a-151">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ConsumerSecret` im Twitter-Entwicklerportal.</span><span class="sxs-lookup"><span data-stu-id="73c0a-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="73c0a-152">Legen Sie die `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret` Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="73c0a-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="73c0a-153">Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="73c0a-153">The configuration system is set up to read keys from environment variables.</span></span>
