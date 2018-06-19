---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Unter Verwendung von externen Websites in einer ASP.NET-Webanwendung angemeldet Pages (Razor) Standort | Microsoft Docs
author: tfitzmac
description: In diesem Artikel wird erläutert, wie der ASP.NET Web Pages (Razor)-Site, die mithilfe von Google, Facebook, Twitter, Yahoo und andere Standorte anmelden – d. h. wie unterstützt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530169"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="24794-103">Protokollierung bei der Verwendung von externen Sites in einem Standort der ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="24794-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="24794-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24794-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24794-105">In diesem Artikel wird erläutert, wie der ASP.NET Web Pages (Razor)-Site, die mithilfe von Google, Facebook, Twitter, Yahoo und andere Standorte anmelden – d. h. wie OAuth- und OpenID an Ihrem Standort unterstützt.</span><span class="sxs-lookup"><span data-stu-id="24794-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="24794-106">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="24794-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="24794-107">Wie die Anmeldung von anderen Standorten aktiviert, bei der Verwendung der Vorlage Starter Site von WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="24794-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="24794-108">Dies ist die ASP.NET-Funktion im Artikel eingeführt:</span><span class="sxs-lookup"><span data-stu-id="24794-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="24794-109">Die `OAuthWebSecurity` Helper.</span><span class="sxs-lookup"><span data-stu-id="24794-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="24794-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="24794-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="24794-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="24794-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="24794-112">WebMatrix-3</span><span class="sxs-lookup"><span data-stu-id="24794-112">WebMatrix 3</span></span>

<span data-ttu-id="24794-113">ASP.NET Web Pages bietet Unterstützung für [OAuth](http://oauth.net/) und [OpenID](http://openid.net/) Anbieter.</span><span class="sxs-lookup"><span data-stu-id="24794-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="24794-114">Diese Anbieter können Sie Benutzern die Anmeldung in Ihre Website mit ihren vorhandenen Anmeldeinformationen von Facebook, Twitter, Microsoft und Google lassen.</span><span class="sxs-lookup"><span data-stu-id="24794-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="24794-115">Beispielsweise können zur Anmeldung mit Facebook-Konto Benutzer eine Facebook-Symbol, wählen Sie dem diese an die Facebook-Anmeldeseite weitergeleitet, wo er ihre Benutzerinformationen ein.</span><span class="sxs-lookup"><span data-stu-id="24794-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="24794-116">Sie können dann die Facebook-Anmeldung mit ihrem Konto auf der Website zuordnen.</span><span class="sxs-lookup"><span data-stu-id="24794-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="24794-117">Eine verwandte-Erweiterung für die Web Pages-Mitgliedschaft-Funktionen ist, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen über social Network-Sites) zuordnen, können mit einem einzelnen Konto auf Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="24794-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="24794-118">Dieses Bild zeigt die Anmeldeseite von der **Starter Site** Vorlage, in dem ein Benutzer eine Symbol für Facebook, Twitter, Google oder Microsoft zum Aktivieren der Protokollierung mit einem externen Konto auswählen kann:</span><span class="sxs-lookup"><span data-stu-id="24794-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![externen Anbietern](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="24794-120">Sie können OAuth- und OpenID-Mitgliedschaft aktivieren, indem Sie einige Codezeilen in Auskommentieren der **Starter Site** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="24794-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="24794-121">Die Methoden und Eigenschaften verwenden Sie zum Arbeiten mit dem OAuth und OpenID-Anbieter sind in der `WebMatrix.Security.OAuthWebSecurity` Klasse.</span><span class="sxs-lookup"><span data-stu-id="24794-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="24794-122">Die **Starter Site** eine vollständige Mitgliedschaft-Infrastruktur umfasst die Vorlage, mit einer Anmeldeseite, eine Mitgliedschaftsdatenbank und der gesamte Code müssen Sie Benutzern die Anmeldung in Ihre Website mithilfe von lokalen Anmeldeinformationen oder die von einem anderen Standort zu ermöglichen .</span><span class="sxs-lookup"><span data-stu-id="24794-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="24794-123">Dieser Abschnitt enthält ein Beispiel, damit Benutzer von externen Standorten auf einer Website anzumelden, die basierend auf den **Starter Site** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="24794-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="24794-124">Nach dem Erstellen einer Startwebsite, führen Sie diese (Details folgen):</span><span class="sxs-lookup"><span data-stu-id="24794-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="24794-125">Für die Standorte, die einen OAuth-Anbieter (Facebook, Twitter und Microsoft) verwenden, erstellen Sie eine Anwendung auf der externen Website an.</span><span class="sxs-lookup"><span data-stu-id="24794-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="24794-126">Dies gibt Ihnen die Anwendungsschlüssel, die Sie benötigen, um die Login-Funktion für diese Standorte aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="24794-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="24794-127">Für Standorte, die einen OpenID-Anbieter (Google) verwenden, müssen Sie keine Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="24794-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="24794-128">Für alle diese Standorte müssen Sie ein Konto verfügen, um anzumelden und entwickleranwendungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="24794-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="24794-129">Microsoft-Anwendungen akzeptieren nur eine live-URL für eine Website verwenden, damit Sie eine lokale Website-URL für das Testen von Benutzernamen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="24794-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="24794-130">Bearbeiten Sie einige Dateien in Ihrer Website, um den entsprechenden Authentifizierungsanbieter anzugeben und eine Anmeldung auf der Website übermitteln, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="24794-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="24794-131">Dieser Artikel enthält separate Anweisungen für die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="24794-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="24794-132">Aktivieren des Google-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="24794-133">Aktivieren der Facebook-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="24794-134">Aktivieren der Twitter-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="24794-135">Aktivieren des Google-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="24794-136">Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.</span><span class="sxs-lookup"><span data-stu-id="24794-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="24794-137">Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung der folgenden Codezeile.</span><span class="sxs-lookup"><span data-stu-id="24794-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="24794-138">Testen von Google-Anmeldung</span><span class="sxs-lookup"><span data-stu-id="24794-138">Testing Google login</span></span>

1. <span data-ttu-id="24794-139">Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **melden Sie sich** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="24794-140">Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt, und wählen Sie entweder die **Google** oder **Yahoo** Schaltfläche zum Absenden.</span><span class="sxs-lookup"><span data-stu-id="24794-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="24794-141">Dieses Beispiel verwendet die Google-Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="24794-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="24794-142">Die Webseite leitet die Anforderung an der Google-Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="24794-142">The web page redirects the request to the Google login page.</span></span>

    ![Google-Anmeldung](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="24794-144">Geben Sie Anmeldeinformationen für ein vorhandenes Google-Konto ein.</span><span class="sxs-lookup"><span data-stu-id="24794-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="24794-145">Wenn Sie Google Sie gefragt werden, ob Sie zulassen möchten *"localhost"* um Informationen aus dem Konto zu verwenden, klicken Sie auf **zulassen**.</span><span class="sxs-lookup"><span data-stu-id="24794-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="24794-146">Der Code verwendet das Google-Token zur Authentifizierung des Benutzers, und klicken Sie dann auf Ihrer Website auf diese Seite gibt.</span><span class="sxs-lookup"><span data-stu-id="24794-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="24794-147">Auf dieser Seite können Benutzer ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website zu verknüpfen, oder sie können ein neues Konto an Ihrem Standort ordnen Sie die externe Anmeldung mit registrieren.</span><span class="sxs-lookup"><span data-stu-id="24794-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="24794-149">Wählen Sie die **zuordnen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-149">Choose the **Associate** button.</span></span> <span data-ttu-id="24794-150">Der Browser gibt zur Startseite der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="24794-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="24794-151">Aktivieren der Facebook-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="24794-152">Wechseln Sie zu der [Facebook-Entwickler-Website](https://developers.facebook.com/apps) (Melden Sie sich, wenn Sie noch nicht angemeldet sind).</span><span class="sxs-lookup"><span data-stu-id="24794-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="24794-153">Wählen Sie die **neue App** Schaltfläche und befolgen Sie dann die Anweisungen, die neue Anwendung erstellen und benennen.</span><span class="sxs-lookup"><span data-stu-id="24794-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="24794-154">Im Abschnitt **wählen Sie die Integration Ihrer app mit Facebook**, wählen Sie die **Website** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="24794-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="24794-155">Füllen Sie die **Website-URL** Feld mit der URL Ihrer Website (z. B. `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="24794-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="24794-156">Die **Domäne** Feld ist optional; Sie können Hiermit können Sie die Bereitstellung der Authentifizierung für eine gesamte Domäne (z. B. *example.com*).</span><span class="sxs-lookup"><span data-stu-id="24794-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="24794-157">Wenn Sie einen Standort, auf dem lokalen Computer mit einer URL ausgeführt werden wie `http://localhost:12345` (wobei die Anzahl ist eine lokale Portnummer), können Sie diesen Wert zum Hinzufügen der **Website-URL** bei Testen Ihrer Website im Feld.</span><span class="sxs-lookup"><span data-stu-id="24794-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="24794-158">Jedoch jedes Mal, wenn die Portnummer des lokalen standortänderungen, müssen Sie beim Aktualisieren der **Website-URL** Feld Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="24794-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="24794-159">Wählen Sie die **Änderungen speichern** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="24794-160">Wählen Sie die **Apps** Registerkarte erneut aus, und zeigen Sie dann auf die Startseite für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="24794-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="24794-161">Kopieren der **App-ID** und **App-Geheimnis** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen.</span><span class="sxs-lookup"><span data-stu-id="24794-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="24794-162">Diese Werte werden an dem Facebook-Anbieter in Ihrem Websitecode übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="24794-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="24794-163">Beenden Sie die Facebook-Entwicklerwebsite ein.</span><span class="sxs-lookup"><span data-stu-id="24794-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="24794-164">Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer können sich bei der Website mithilfe ihrer Facebook-Konten anmelden können.</span><span class="sxs-lookup"><span data-stu-id="24794-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="24794-165">Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.</span><span class="sxs-lookup"><span data-stu-id="24794-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="24794-166">Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für die Facebook-OAuth-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="24794-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="24794-167">Der unkommentierten Codeblock sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="24794-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="24794-168">Kopieren der **App-ID** Wert aus der Facebook-Anwendung als Wert für die `appId` Parameter (in Anführungszeichen).</span><span class="sxs-lookup"><span data-stu-id="24794-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="24794-169">Kopie **App-Geheimnis** Wert aus der Facebook-Anwendung als der `appSecret` Parameterwert.</span><span class="sxs-lookup"><span data-stu-id="24794-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="24794-170">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="24794-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="24794-171">Testen der Facebook-Anmeldung</span><span class="sxs-lookup"><span data-stu-id="24794-171">Testing Facebook login</span></span>

1. <span data-ttu-id="24794-172">Führen Sie des Standorts *default.cshtml* Seite, und wählen Sie die **Anmeldung** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="24794-173">Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Facebook** Symbol.</span><span class="sxs-lookup"><span data-stu-id="24794-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="24794-174">Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="24794-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="24794-176">Melden Sie sich eine Facebook-Konto.</span><span class="sxs-lookup"><span data-stu-id="24794-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="24794-177">Der Code verwendet das Facebook-Token, um Sie zu authentifizieren und gibt dann zurück auf eine Seite in dem Sie Ihre Facebook-Anmeldung mit Ihrer Website Anmeldung zuordnen.</span><span class="sxs-lookup"><span data-stu-id="24794-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="24794-178">Der Name oder e-Mail-Adresse eines Benutzers in gefüllt wird die **E-Mail** Feld im Formular.</span><span class="sxs-lookup"><span data-stu-id="24794-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="24794-180">Wählen Sie die **zuordnen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="24794-181">Der Browser gibt, um zur Startseite, und Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="24794-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="24794-182">Aktivieren der Twitter-Anmeldungen</span><span class="sxs-lookup"><span data-stu-id="24794-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="24794-183">Navigieren Sie zu der [Twitter-Entwickler-Website](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="24794-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="24794-184">Wählen Sie die **Erstellen einer App** verknüpfen, und melden Sie sich dann bei der Website.</span><span class="sxs-lookup"><span data-stu-id="24794-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="24794-185">Auf der **erstellen Sie eine Anwendung** bilden, "füllen" der **Namen** und **Beschreibung** Felder.</span><span class="sxs-lookup"><span data-stu-id="24794-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="24794-186">In der **WebSite** Feld, geben Sie die URL Ihrer Website (z. B. `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="24794-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="24794-187">Wenn Sie Ihre Website lokal testen (über eine URL wie `http://localhost:12345`), ist die URL, Twitter nicht akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="24794-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="24794-188">Allerdings können Sie möglicherweise die lokalen Loopback-IP-Adresse verwenden (z. B. `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="24794-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="24794-189">Dies vereinfacht das Testen Ihrer Anwendung lokal.</span><span class="sxs-lookup"><span data-stu-id="24794-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="24794-190">Jedoch jedes Mal, wenn die Portnummer Ihres lokalen Standorts geändert wird, Sie müssen zum Aktualisieren der **WebSite** Feld Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="24794-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="24794-191">In der **Rückruf-URL** Feld, geben Sie eine URL für die Seite in Ihrer Website, die Benutzern, nach der Anmeldung in Twitter zurückgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="24794-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="24794-192">Geben Sie beispielsweise die gleiche URL, die Sie in eingegeben haben, um Benutzer zur Startseite des Starter-Site zu senden (die ihre angemeldeten Status erkannt wird), die **WebSite** Feld.</span><span class="sxs-lookup"><span data-stu-id="24794-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="24794-193">Akzeptieren Sie die Bestimmungen ein, und wählen Sie die **die Twitter-Anwendung erstellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="24794-194">Auf der **Meine Anwendungen** Angebotsseite, wählen Sie die Anwendung, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="24794-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="24794-195">Auf der **Details** Registerkarte, führen Sie einen Bildlauf nach unten aus, und wählen Sie die **erstellen Sie eigene Zugriffstoken** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="24794-196">Auf der **Details** Registerkarte, kopieren Sie die **Consumerschlüssel** und **Consumerschlüssel** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen.</span><span class="sxs-lookup"><span data-stu-id="24794-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="24794-197">Sie müssen diese Werte in Ihrem Websitecode auf Twitter-Anbieter übergeben.</span><span class="sxs-lookup"><span data-stu-id="24794-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="24794-198">Die Twitter-Website zu beenden.</span><span class="sxs-lookup"><span data-stu-id="24794-198">Exit the Twitter site.</span></span>

<span data-ttu-id="24794-199">Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer werden bei der Website mithilfe ihrer Konten Twitter anmelden können.</span><span class="sxs-lookup"><span data-stu-id="24794-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="24794-200">Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.</span><span class="sxs-lookup"><span data-stu-id="24794-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="24794-201">Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für den Twitter-OAuth-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="24794-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="24794-202">Der unkommentierten Codeblock sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="24794-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="24794-203">Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerKey` Parameter (in Anführungszeichen).</span><span class="sxs-lookup"><span data-stu-id="24794-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="24794-204">Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerSecret` Parameter.</span><span class="sxs-lookup"><span data-stu-id="24794-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="24794-205">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="24794-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="24794-206">Testen der Twitter-Anmeldung</span><span class="sxs-lookup"><span data-stu-id="24794-206">Testing Twitter login</span></span>

1. <span data-ttu-id="24794-207">Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **Anmeldung** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="24794-208">Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Twitter** Symbol.</span><span class="sxs-lookup"><span data-stu-id="24794-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="24794-209">Die Webseite leitet die Anforderung an einen Twitter-Anmeldeseite für die Anwendung, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="24794-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="24794-211">Melden Sie sich ein Twitter-Konto.</span><span class="sxs-lookup"><span data-stu-id="24794-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="24794-212">Der Code verwendet das Twitter-Token zur Authentifizierung des Benutzers und wechselt zurück zu einer Seite, in dem Sie zuordnen können Ihre Anmeldung mit Ihrem Konto für die Website.</span><span class="sxs-lookup"><span data-stu-id="24794-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="24794-213">Der Name oder e-Mail-Adresse in gefüllt wird die **E-Mail** Feld im Formular.</span><span class="sxs-lookup"><span data-stu-id="24794-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="24794-215">Wählen Sie die **zuordnen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="24794-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="24794-216">Der Browser gibt, um zur Startseite, und Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="24794-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="24794-217">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="24794-217">Additional Resources</span></span>


- [<span data-ttu-id="24794-218">Anpassen des Verhaltens der standortweiten</span><span class="sxs-lookup"><span data-stu-id="24794-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="24794-219">Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website</span><span class="sxs-lookup"><span data-stu-id="24794-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
