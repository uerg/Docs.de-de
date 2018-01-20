---
title: Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern
author: rick-anderson
description: Dieses Tutorial veranschaulicht, wie Sie eine ASP.NET Core 2.x-App mithilfe von OAuth 2.0 und externen Authentifizierungsanbietern entwickeln.
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 7d03998c82bf13976ec6157acb5c56c28e5c0d52
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="12471-103">Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern</span><span class="sxs-lookup"><span data-stu-id="12471-103">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="12471-104">Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="12471-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="12471-105">Dieses Tutorial veranschaulicht, wie Sie eine ASP.NET Core 2.x-App erstellen, die Benutzern ermöglicht, sich mithilfe von OAuth 2.0 und Anmeldeinformationen von externen Authentifizierungsanbietern anzumelden.</span><span class="sxs-lookup"><span data-stu-id="12471-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="12471-106">Die Anbieter [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) und [Microsoft](microsoft-logins.md) werden in den folgenden Abschnitten behandelt.</span><span class="sxs-lookup"><span data-stu-id="12471-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="12471-107">Andere Anbieter stehen in Paketen von Drittanbietern wie z.B. [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) und [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="12471-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Symbole sozialer Medien wie Facebook, Twitter, Google Plus und Windows](index/_static/social.png)

<span data-ttu-id="12471-109">Den Benutzern zu ermöglichen, sich mit ihren vorhandenen Anmeldeinformationen anzumelden, ist für diese komfortabel und verlagert einen Großteil der Komplexität der Verwaltung des Anmeldevorgangs an einen Drittanbieter.</span><span class="sxs-lookup"><span data-stu-id="12471-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="12471-110">Beispiel dazu, wie Anmeldungen bei sozialen Medien für mehr Datenverkehr und gewonnene Kunden sorgen, finden Sie in Fallstudien von [Facebook](https://www.facebook.com/unsupportedbrowser) und [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="12471-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="12471-111">Hinweis: Hier vorgestellte Pakete abstrahieren einen Großteil der Komplexität des OAuth-Authentifizierungsflusses, doch ein Verständnis der Details kann bei der Problembehandlung erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="12471-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="12471-112">Viele Ressourcen sind verfügbar. Siehe z.B. [Einführung in OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) oder [Grundlegendes zu OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="12471-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="12471-113">Einige Probleme können mithilfe einer Untersuchung des [ASP.NET Core-Quellcodes für die Anbieterpakete](https://github.com/aspnet/Security/tree/dev/src) behoben werden.</span><span class="sxs-lookup"><span data-stu-id="12471-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="12471-114">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="12471-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="12471-115">Erstellen Sie in Visual Studio 2017 ein neues Projekt auf der Startseite oder über **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="12471-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="12471-116">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus, die in der Kategorie **Visual C# > .NET Core** verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="12471-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Dialogfeld "Neues Projekt"](index/_static/new-project.png)

* <span data-ttu-id="12471-118">Tippen Sie auf **Webanwendung**, und vergewissern Sie sich, dass **Authentifizierung** auf **Einzelne Benutzerkonten** festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="12471-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Dialogfeld „Neue Webanwendung“](index/_static/select-project.png)

<span data-ttu-id="12471-120">Hinweis: Dieses Tutorial bezieht sich auf die ASP.NET Core 2.0 SDK-Version, die oben im Assistenten ausgewählt werden kann.</span><span class="sxs-lookup"><span data-stu-id="12471-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="12471-121">Anwenden von Migrationen</span><span class="sxs-lookup"><span data-stu-id="12471-121">Apply migrations</span></span>

* <span data-ttu-id="12471-122">Führen Sie die App aus, und klicken Sie auf den Link **Anmelden**.</span><span class="sxs-lookup"><span data-stu-id="12471-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="12471-123">Klicken Sie auf den Link **Als neuer Benutzer registrieren**.</span><span class="sxs-lookup"><span data-stu-id="12471-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="12471-124">Geben Sie die E-Mail-Adresse und das Kennwort für das neue Konto ein, und wählen Sie dann **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="12471-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="12471-125">Befolgen Sie die Anweisungen zum Anwenden von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="12471-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="12471-126">Anfordern von SSL</span><span class="sxs-lookup"><span data-stu-id="12471-126">Require SSL</span></span>

<span data-ttu-id="12471-127">OAuth 2.0 erfordert die Verwendung von SSL für die Authentifizierung über das HTTPS-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="12471-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="12471-128">Hinweis: Mithilfe der Projektvorlagen **Webanwendung** oder **Web-API** für ASP.NET Core 2.x erstellte Projekte sind automatisch für das Aktivieren von SSL konfiguriert und werden mit einer HTTPS-URL gestartet, wenn **Einzelne Benutzerkonten** im Dialogfeld **Authentifizierung ändern** im Projekt-Assistenten, wie oben gezeigt, ausgewählt war.</span><span class="sxs-lookup"><span data-stu-id="12471-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="12471-129">Machen Sie SSL für Ihre Website erforderlich, indem Sie die Schritte im Thema [Erzwingen von SSL in einer ASP.NET Core-App](xref:security/enforcing-ssl) befolgen.</span><span class="sxs-lookup"><span data-stu-id="12471-129">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="12471-130">Verwenden von SecretManager zum Speichern von Token, die von Anmeldeanbietern zugewiesen wurden</span><span class="sxs-lookup"><span data-stu-id="12471-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="12471-131">Anmeldeanbieter aus dem Bereich der sozialen Medien weisen während des Registrierungsvorgangs Token des Typs **Anwendungs-ID** und **Anwendungsgeheimnis** (die genaue Bezeichnung hängt vom Anbieter ab) zu.</span><span class="sxs-lookup"><span data-stu-id="12471-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="12471-132">Diese Werte sind gewissermaßen der *Benutzername* und das *Kennwort*, den/das Ihre Anwendung für den Zugriff auf die API des Anbieters verwendet. Diese Anmeldeinformationen bilden die „Geheimnisse“, die mithilfe von **Secret Manager** mit Ihrer Anwendungskonfiguration verknüpft werden können, anstatt diese direkt in Konfigurationsdateien zu speichern oder hartzucodieren.</span><span class="sxs-lookup"><span data-stu-id="12471-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="12471-133">Führen Sie die Schritte im Thema [Sichere Speicherung von App-Geheimnissen während der Entwicklung in ASP.NET Core](xref:security/app-secrets) aus, damit Sie Token speichern können, die von den nachstehenden Anmeldeanbietern zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="12471-133">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="12471-134">Einrichten der für Ihre Anwendung erforderlichen Anmeldeanbietern</span><span class="sxs-lookup"><span data-stu-id="12471-134">Setup login providers required by your application</span></span>

<span data-ttu-id="12471-135">In den folgenden Themen erfahren Sie, wie Sie Ihre Anwendung für die entsprechenden Anbieter konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="12471-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="12471-136">Anweisungen für [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="12471-136">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="12471-137">Anweisungen für [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="12471-137">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="12471-138">Anweisungen für [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="12471-138">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="12471-139">Anweisungen für [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="12471-139">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="12471-140">Anweisungen für [andere Anbieter](other-logins.md)</span><span class="sxs-lookup"><span data-stu-id="12471-140">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="12471-141">Optionales Festlegen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="12471-141">Optionally set password</span></span>

<span data-ttu-id="12471-142">Bei der Registrierung bei einem externen Anmeldeanbieter wird kein Kennwort mit der App registriert.</span><span class="sxs-lookup"><span data-stu-id="12471-142">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="12471-143">Dadurch entfällt für Sie Erstellen und Merken eines Kennworts für die Website. Sie werden allerdings auch vom externen Anmeldeanbieter abhängig.</span><span class="sxs-lookup"><span data-stu-id="12471-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="12471-144">Wenn der externe Anmeldeanbieter nicht verfügbar ist, können Sie sich nicht bei der Website anmelden.</span><span class="sxs-lookup"><span data-stu-id="12471-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="12471-145">So erstellen Sie ein Kennwort und melden sich mithilfe Ihrer E-Mail-Adresse an, die Sie während des Anmeldevorgangs bei externen Anbietern festgelegt haben:</span><span class="sxs-lookup"><span data-stu-id="12471-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="12471-146">Tippen Sie rechts oben auf den Link **Hallo<email alias>**, navigieren Sie zur Ansicht **Verwalten**.</span><span class="sxs-lookup"><span data-stu-id="12471-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Ansicht „Verwalten“ der Webanwendung](index/_static/pass1a.png)

* <span data-ttu-id="12471-148">Tippen Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="12471-148">Tap **Create**</span></span>

![Seite zum Festlegen Ihres Kennworts](index/_static/pass2a.png)

* <span data-ttu-id="12471-150">Legen Sie ein gültiges Kennwort fest, das Sie anschließend mit Ihrer E-Mail-Adresse zum Anmelden nutzen können.</span><span class="sxs-lookup"><span data-stu-id="12471-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12471-151">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="12471-151">Next steps</span></span>

* <span data-ttu-id="12471-152">In diesem Artikel wurde die externe Authentifizierung behandelt. Ferner wurden die Voraussetzungen für das Hinzufügen externer Anmeldungen zu Ihrer ASP.NET Core-App erläutert.</span><span class="sxs-lookup"><span data-stu-id="12471-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="12471-153">Konsultieren Sie anbieterspezifische Seiten zum Konfigurieren von Anmeldungen für die von Ihrer App angeforderten Anbieter.</span><span class="sxs-lookup"><span data-stu-id="12471-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
