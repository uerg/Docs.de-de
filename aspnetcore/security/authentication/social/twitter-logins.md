---
title: Externe Anmeldung Setup Twitter
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Twitter-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4fd1c2d59cc277ef3781df5e306e4a5e6064520a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="2065f-103">Konfigurieren von Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="2065f-103">Configuring Twitter authentication</span></span>

<span data-ttu-id="2065f-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2065f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2065f-105">Diesem Lernprogramm erfahren Sie, wie Sie Ihren Benutzern ermöglichen [melden Sie sich mit ihren Twitter-Konto](https://dev.twitter.com/web/sign-in/desktop-browser) ein Beispielprojekt für ASP.NET Core 2.0 erstellt wurde, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="2065f-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="2065f-106">Erstellen Sie die app in Twitter</span><span class="sxs-lookup"><span data-stu-id="2065f-106">Create the app in Twitter</span></span>

* <span data-ttu-id="2065f-107">Navigieren Sie zu [https://apps.twitter.com/](https://apps.twitter.com/) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="2065f-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="2065f-108">Wenn Sie einen Twitter-Konto noch nicht haben, verwenden die  **[jetzt registrieren](https://twitter.com/signup)**  Link, um eines zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2065f-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="2065f-109">Nach der Anmeldung, die **Anwendungsverwaltung** Seite wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2065f-109">After signing in, the **Application Management** page is shown:</span></span>

![Twitter Application Management in Microsoft Edge öffnen](index/_static/TwitterAppManage.png)

* <span data-ttu-id="2065f-111">Tippen Sie auf **neue App** , und füllen Sie die Anwendung **Namen**, **Beschreibung** und öffentlichen **Website** URI (Dies kann temporär sein bis Registrieren Sie den Domänennamen):</span><span class="sxs-lookup"><span data-stu-id="2065f-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Erstellen einer Anwendungsseite](index/_static/TwitterCreate.png)

* <span data-ttu-id="2065f-113">Geben Sie die Entwicklung URI mit */signin-twitter* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="2065f-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="2065f-114">Verarbeiten der Twitter-Authentifizierungsschema, die weiter unten in diesem Lernprogramm konfiguriert automatisch Anforderungen, die bei */signin-twitter* Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="2065f-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="2065f-115">Füllen Sie den Rest des Formulars, und tippen Sie auf **die Twitter-Anwendung erstellen**.</span><span class="sxs-lookup"><span data-stu-id="2065f-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="2065f-116">Anwendungsdetails des neuen werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2065f-116">New application details are displayed:</span></span>

![Registerkarte "Details" auf der Seite "Anwendung"](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="2065f-118">Bei der Bereitstellung der Website müssen Sie rufen die **Anwendungsverwaltung** Seite, und registrieren Sie einen neuen öffentlichen URI.</span><span class="sxs-lookup"><span data-stu-id="2065f-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="2065f-119">Speichern von Twitter ConsumerKey und ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="2065f-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="2065f-120">Verknüpfen Sie die sensiblen Einstellungen wie Twitter `Consumer Key` und `Consumer Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="2065f-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="2065f-121">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="2065f-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="2065f-122">Diese Token befinden sich auf die **Schlüssel und Zugriffstoken** Registerkarte nach dem Erstellen einer neuen Twitter-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="2065f-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Registerkarte "Schlüssel und Zugriffstoken"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="2065f-124">Twitter-Authentifizierung konfigurieren</span><span class="sxs-lookup"><span data-stu-id="2065f-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="2065f-125">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="2065f-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="2065f-126">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="2065f-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2065f-127">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="2065f-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2065f-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2065f-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2065f-129">Hinzufügen den Twitter-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="2065f-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2065f-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2065f-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2065f-131">Hinzufügen die Twitter-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="2065f-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="2065f-132">Finden Sie unter der [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen von Twitter-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2065f-132">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="2065f-133">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="2065f-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="2065f-134">Melden Sie sich mit Twitter</span><span class="sxs-lookup"><span data-stu-id="2065f-134">Sign in with Twitter</span></span>

<span data-ttu-id="2065f-135">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="2065f-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="2065f-136">Eine Option zur Anmeldung mit Twitter angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="2065f-136">An option to sign in with Twitter appears:</span></span>

![-Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneTwitter.png)

<span data-ttu-id="2065f-138">Durch Klicken auf **Twitter** leitet an Twitter für die Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="2065f-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter-Seite "Authentifizierung"](index/_static/TwitterLogin.png)

<span data-ttu-id="2065f-140">Nach der Eingabe der Anmeldeinformationen für Twitter, werden Sie zurück zur Website umgeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="2065f-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="2065f-141">Sie sind jetzt angemeldet mit Ihren Twitter-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="2065f-141">You are now logged in using your Twitter credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="2065f-143">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="2065f-143">Troubleshooting</span></span>

* <span data-ttu-id="2065f-144">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="2065f-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2065f-145">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="2065f-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2065f-146">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="2065f-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2065f-147">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="2065f-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2065f-148">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2065f-148">Next steps</span></span>

* <span data-ttu-id="2065f-149">In diesem Artikel wurde gezeigt, wie Sie mit Twitter authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="2065f-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="2065f-150">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="2065f-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="2065f-151">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ConsumerSecret` im Entwicklerportal Twitter.</span><span class="sxs-lookup"><span data-stu-id="2065f-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="2065f-152">Legen Sie die `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="2065f-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2065f-153">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="2065f-153">The configuration system is set up to read keys from environment variables.</span></span>
