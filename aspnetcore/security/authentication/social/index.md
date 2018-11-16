---
title: Authentifizierung über Facebook, Google und externe Anbieter in ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht, wie Sie eine ASP.NET Core 2.x-App mithilfe von OAuth 2.0 und externen Authentifizierungsanbietern entwickeln.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 19074d5014a09446ceec1b89449e78760fc8e7cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708373"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="a9096-103">Authentifizierung über Facebook, Google und externe Anbieter in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9096-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="a9096-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9096-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9096-105">Dieses Tutorial veranschaulicht, wie Sie eine ASP.NET Core 2.x-App erstellen, die Benutzern ermöglicht, sich mithilfe von OAuth 2.0 und Anmeldeinformationen von externen Authentifizierungsanbietern anzumelden.</span><span class="sxs-lookup"><span data-stu-id="a9096-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="a9096-106">Die Anbieter [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) und [Microsoft](xref:security/authentication/microsoft-logins) werden in den folgenden Abschnitten behandelt.</span><span class="sxs-lookup"><span data-stu-id="a9096-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="a9096-107">Andere Anbieter stehen in Paketen von Drittanbietern wie z.B. [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) und [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="a9096-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Symbole sozialer Medien wie Facebook, Twitter, Google Plus und Windows](index/_static/social.png)

<span data-ttu-id="a9096-109">Den Benutzern zu ermöglichen, sich mit ihren vorhandenen Anmeldeinformationen anzumelden, ist für diese komfortabel und verlagert einen Großteil der Komplexität der Verwaltung des Anmeldevorgangs an einen Drittanbieter.</span><span class="sxs-lookup"><span data-stu-id="a9096-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="a9096-110">Beispiel dazu, wie Anmeldungen bei sozialen Medien für mehr Datenverkehr und gewonnene Kunden sorgen, finden Sie in Fallstudien von [Facebook](https://www.facebook.com/unsupportedbrowser) und [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="a9096-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="a9096-111">Hinweis: Hier vorgestellte Pakete abstrahieren einen Großteil der Komplexität des OAuth-Authentifizierungsflusses, doch ein Verständnis der Details kann bei der Problembehandlung erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="a9096-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="a9096-112">Viele Ressourcen sind verfügbar. Siehe z.B. [Einführung in OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) oder [Grundlegendes zu OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="a9096-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="a9096-113">Einige Probleme können mithilfe einer Untersuchung des [ASP.NET Core-Quellcodes für die Anbieterpakete](https://github.com/aspnet/Security/tree/master/src) behoben werden.</span><span class="sxs-lookup"><span data-stu-id="a9096-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="a9096-114">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="a9096-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="a9096-115">Erstellen Sie in Visual Studio 2017 ein neues Projekt auf der Startseite oder über **Date** > **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a9096-115">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="a9096-116">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus, die in der Kategorie **Visual C#** > **.NET Core** verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="a9096-116">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>

![Dialogfeld "Neues Projekt"](index/_static/new-project.png)

* <span data-ttu-id="a9096-118">Tippen Sie auf **Webanwendung**, und vergewissern Sie sich, dass **Authentifizierung** auf **Einzelne Benutzerkonten** festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="a9096-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Dialogfeld „Neue Webanwendung“](index/_static/select-project.png)

<span data-ttu-id="a9096-120">Hinweis: Dieses Tutorial bezieht sich auf die ASP.NET Core 2.0 SDK-Version, die oben im Assistenten ausgewählt werden kann.</span><span class="sxs-lookup"><span data-stu-id="a9096-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="a9096-121">Anwenden von Migrationen</span><span class="sxs-lookup"><span data-stu-id="a9096-121">Apply migrations</span></span>

* <span data-ttu-id="a9096-122">Führen Sie die App aus, und klicken Sie auf den Link **Anmelden**.</span><span class="sxs-lookup"><span data-stu-id="a9096-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="a9096-123">Klicken Sie auf den Link **Als neuer Benutzer registrieren**.</span><span class="sxs-lookup"><span data-stu-id="a9096-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="a9096-124">Geben Sie die E-Mail-Adresse und das Kennwort für das neue Konto ein, und wählen Sie dann **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="a9096-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="a9096-125">Befolgen Sie die Anweisungen zum Anwenden von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="a9096-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="a9096-126">Anfordern von SSL</span><span class="sxs-lookup"><span data-stu-id="a9096-126">Require SSL</span></span>

<span data-ttu-id="a9096-127">OAuth 2.0 erfordert die Verwendung von SSL für die Authentifizierung über das HTTPS-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="a9096-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="a9096-128">Projekte, die mithilfe der Projektvorlagen **Webanwendung** oder **Web-API** mit ASP.NET Core 2.1 oder höher erstellt werden, werden automatisch so konfiguriert, dass SSL aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="a9096-128">Projects created using the **Web Application** or **Web API** project templates with ASP.NET Core 2.1 or later are automatically configured to enable SSL.</span></span> <span data-ttu-id="a9096-129">Die App wird mit einem sicheren Standardendpunkt gestartet, wenn die Option **Einzelne Benutzerkonten** im Dialogfeld **Authentifizierung ändern** des Projekt-Assistenten ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="a9096-129">The app launches with a secure default endpoint if the **Individual User Accounts** option is selected in the **Change Authentication dialog** of the project wizard.</span></span>

<span data-ttu-id="a9096-130">Weitere Informationen finden Sie unter <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="a9096-130">For more information, see <xref:security/enforcing-ssl>.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="a9096-131">Verwenden von SecretManager zum Speichern von Token, die von Anmeldeanbietern zugewiesen wurden</span><span class="sxs-lookup"><span data-stu-id="a9096-131">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="a9096-132">Anmeldeanbieter aus dem Bereich der sozialen Medien weisen während des Registrierungsvorgangs Token des Typs **Anwendungs-ID** und **Anwendungsgeheimnis** zu.</span><span class="sxs-lookup"><span data-stu-id="a9096-132">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="a9096-133">Die genauen Tokennamen unterscheiden sich je nach Anbieter.</span><span class="sxs-lookup"><span data-stu-id="a9096-133">The exact token names vary by provider.</span></span> <span data-ttu-id="a9096-134">Diese Token stellen die Anmeldeinformationen dar, mit denen Ihre App auf ihre API zugreift.</span><span class="sxs-lookup"><span data-stu-id="a9096-134">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="a9096-135">Diese Token setzen sich zu „Geheimnissen“ zusammen, die mithilfe von [Secret Manager](xref:security/app-secrets#secret-manager) mit Ihrer App-Konfiguration verknüpft werden können.</span><span class="sxs-lookup"><span data-stu-id="a9096-135">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="a9096-136">Secret Manager ist eine sicherere Alternative zum Speichern von Token in einer Konfigurationsdatei wie z.B. *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a9096-136">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9096-137">Secret Manager ist nur zur Entwicklung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="a9096-137">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="a9096-138">Sie können Azure-Test- und -Produktionsgeheimnisse mit dem [Konfigurationsanbieter Azure Key Vault](xref:security/key-vault-configuration) speichern und schützen.</span><span class="sxs-lookup"><span data-stu-id="a9096-138">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="a9096-139">Führen Sie die im Artikel [Sichere Speicherung von App-Geheimnissen bei einer Entwicklung in ASP.NET Core](xref:security/app-secrets) beschriebenen Schritte durch, um Token zu speichern, die von den folgenden Anmeldeanbietern zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="a9096-139">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="a9096-140">Einrichten der für Ihre Anwendung erforderlichen Anmeldeanbietern</span><span class="sxs-lookup"><span data-stu-id="a9096-140">Setup login providers required by your application</span></span>

<span data-ttu-id="a9096-141">In den folgenden Themen erfahren Sie, wie Sie Ihre Anwendung für die entsprechenden Anbieter konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="a9096-141">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="a9096-142">Anweisungen für [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="a9096-142">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="a9096-143">Anweisungen für [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="a9096-143">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="a9096-144">Anweisungen für [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="a9096-144">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="a9096-145">Anweisungen für [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="a9096-145">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="a9096-146">Anweisungen für [andere Anbieter](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="a9096-146">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="a9096-147">Optionales Festlegen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="a9096-147">Optionally set password</span></span>

<span data-ttu-id="a9096-148">Wenn Sie sich bei einem externen Anmeldeanbieter registrieren, wird kein Kennwort bei der App registriert.</span><span class="sxs-lookup"><span data-stu-id="a9096-148">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="a9096-149">Dadurch entfällt für Sie Erstellen und Merken eines Kennworts für die Website. Sie werden allerdings auch vom externen Anmeldeanbieter abhängig.</span><span class="sxs-lookup"><span data-stu-id="a9096-149">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="a9096-150">Wenn der externe Anmeldeanbieter nicht verfügbar ist, können Sie sich nicht bei der Website anmelden.</span><span class="sxs-lookup"><span data-stu-id="a9096-150">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="a9096-151">So erstellen Sie ein Kennwort und melden sich mithilfe Ihrer E-Mail-Adresse an, die Sie während des Anmeldevorgangs bei externen Anbietern festgelegt haben:</span><span class="sxs-lookup"><span data-stu-id="a9096-151">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="a9096-152">Tippen Sie rechts oben auf den Link **Hallo &lt;E-Mail-Alias&gt;**, um zur Ansicht **Verwalten** zu gelangen.</span><span class="sxs-lookup"><span data-stu-id="a9096-152">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Ansicht „Verwalten“ der Webanwendung](index/_static/pass1a.png)

* <span data-ttu-id="a9096-154">Tippen Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="a9096-154">Tap **Create**</span></span>

![Seite zum Festlegen Ihres Kennworts](index/_static/pass2a.png)

* <span data-ttu-id="a9096-156">Legen Sie ein gültiges Kennwort fest, das Sie anschließend mit Ihrer E-Mail-Adresse zum Anmelden nutzen können.</span><span class="sxs-lookup"><span data-stu-id="a9096-156">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9096-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a9096-157">Next steps</span></span>

* <span data-ttu-id="a9096-158">In diesem Artikel wurde die externe Authentifizierung behandelt. Ferner wurden die Voraussetzungen für das Hinzufügen externer Anmeldungen zu Ihrer ASP.NET Core-App erläutert.</span><span class="sxs-lookup"><span data-stu-id="a9096-158">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="a9096-159">Konsultieren Sie anbieterspezifische Seiten zum Konfigurieren von Anmeldungen für die von Ihrer App angeforderten Anbieter.</span><span class="sxs-lookup"><span data-stu-id="a9096-159">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="a9096-160">Möglicherweise möchten Sie zusätzliche Daten zum Benutzer und seinen Zugriffs- und Aktualisierungstoken persistent speichern.</span><span class="sxs-lookup"><span data-stu-id="a9096-160">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="a9096-161">Weitere Informationen finden Sie unter <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="a9096-161">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
