---
title: -Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core
author: steve-smith
description: Erfahren Sie, wie ein, um Angriffe auf Web-apps zu verhindern, in denen eine bösartige Website die Interaktion zwischen einem Clientbrowser und die app auswirken kann.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="afdbe-103">-Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="afdbe-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="afdbe-104">Durch [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="afdbe-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="afdbe-105">Websiteübergreifende anforderungsfälschung (auch bekannt als XSRF oder CSRF, Aussprache *Siehe Surfen*) ist ein Angriff gegen Web gehostete apps, die bei dem eine böswillige Web-app die Interaktion zwischen einem Clientbrowser und eine WebApp, die Vertrauensstellungen, die beeinflussen kann Browser.</span><span class="sxs-lookup"><span data-stu-id="afdbe-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="afdbe-106">Diese Angriffe sind möglich, da Webbrowsern einige Arten von Authentifizierungstoken automatisch mit jeder Anforderung an eine Website gesendete.</span><span class="sxs-lookup"><span data-stu-id="afdbe-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="afdbe-107">Diese Form von Angriff ist auch bekannt als ein *einmalklick-Angriff* oder *Sitzung riding* , da das Risiko eines Angriffs nutzt die der Benutzer zuvor Sitzung authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="afdbe-108">Ein Beispiel von CSRF-Angriffen:</span><span class="sxs-lookup"><span data-stu-id="afdbe-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="afdbe-109">Ein Benutzer meldet sich in `www.good-banking-site.com` mit Formularauthentifizierung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="afdbe-110">Der Server authentifiziert den Benutzer und gibt eine Antwort, die ein Authentifizierungscookie enthält.</span><span class="sxs-lookup"><span data-stu-id="afdbe-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="afdbe-111">Der Standort ist anfällig für Angriffe, da sie alle Anforderungen vertraut, die sie mit einem gültigen Authentifizierungscookie erhält.</span><span class="sxs-lookup"><span data-stu-id="afdbe-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="afdbe-112">Der Benutzer besucht eine bösartige Website `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="afdbe-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="afdbe-113">Die bösartige Website `www.bad-crook-site.com`, enthält ein HTML-Formular, das etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="afdbe-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="afdbe-114">Beachten Sie, dass des Formulars `action` Beiträge, die an die anfällig für Website und nicht an die bösartige Website.</span><span class="sxs-lookup"><span data-stu-id="afdbe-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="afdbe-115">Dies ist der "Cross-Site" Teil CSRF.</span><span class="sxs-lookup"><span data-stu-id="afdbe-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="afdbe-116">Der Benutzer wählt die Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="afdbe-116">The user selects the submit button.</span></span> <span data-ttu-id="afdbe-117">Der Browser sendet die Anforderung und schließt automatisch das Authentifizierungscookie für die angeforderte Domäne `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="afdbe-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="afdbe-118">Die Anforderung ausgeführt wird, auf die `www.good-banking-site.com` Server mit dem Kontext des Benutzers Authentifizierung und alle Aktionen, die ein authentifizierter Benutzer ausführen darf ausführen können.</span><span class="sxs-lookup"><span data-stu-id="afdbe-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="afdbe-119">Wenn der Benutzer die Schaltfläche zum Senden des Formulars auswählt, kann die bösartige Website:</span><span class="sxs-lookup"><span data-stu-id="afdbe-119">When the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="afdbe-120">Führen Sie ein Skript, das automatisch auf das Formular sendet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="afdbe-121">Sendet eine Formularübermittlung als eine AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-121">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="afdbe-122">Verwenden Sie ein ausgeblendetes Formular mit CSS ein.</span><span class="sxs-lookup"><span data-stu-id="afdbe-122">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="afdbe-123">Verwendung von HTTPS nicht zu verhindern, dass eine CSRF-Angriffen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-123">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="afdbe-124">Kann die bösartige Website senden ein `https://www.good-banking-site.com/` anfordern genauso einfach wie eine unsichere Anforderung senden können.</span><span class="sxs-lookup"><span data-stu-id="afdbe-124">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="afdbe-125">Einige Angriffe als Ziel Endpunkte, auf die GET-Anforderungen reagiert. in diesem Fall einem Bildtag dienen zum Ausführen der Aktion.</span><span class="sxs-lookup"><span data-stu-id="afdbe-125">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="afdbe-126">Diese Form von Angriff ist üblich, auf der Forum-Standorte, die Bilder ermöglichen jedoch JavaScript blockiert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-126">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="afdbe-127">Apps, die Status für GET-Anforderungen ändern, in denen Variablen oder Ressourcen geändert werden, sind anfällig für Angriffe.</span><span class="sxs-lookup"><span data-stu-id="afdbe-127">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="afdbe-128">**GET-Anforderungen, die Status geändert sind unsicher. Eine bewährte Methode besteht darin nie Zustand für eine GET-Anforderung ändern.**</span><span class="sxs-lookup"><span data-stu-id="afdbe-128">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="afdbe-129">CSRF-Angriffen sind für Web-apps, die Cookies für die Authentifizierung zu verwenden, da möglich:</span><span class="sxs-lookup"><span data-stu-id="afdbe-129">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="afdbe-130">Browser Speichern von Cookies, die von einer Web-app ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="afdbe-130">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="afdbe-131">Gespeicherte Cookies enthalten die Sitzungscookies für authentifizierte Benutzer.</span><span class="sxs-lookup"><span data-stu-id="afdbe-131">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="afdbe-132">Browser senden alle Cookies einer Domäne in der Web-app zugeordnete jede Anforderung, unabhängig davon, wie die Anforderung an die app innerhalb des Browsers generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="afdbe-132">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="afdbe-133">Allerdings CSRF-Angriffen sind nicht begrenzt auf Cookies ausnutzen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-133">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="afdbe-134">Beispielsweise Basis-und Digestauthentifizierung sind auch anfällig.</span><span class="sxs-lookup"><span data-stu-id="afdbe-134">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="afdbe-135">Nachdem ein Benutzer mit Standard oder Digest-Authentifizierung anmeldet, sendet der Browser die Anmeldeinformationen automatisch bis die Sitzung&dagger; endet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-135">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="afdbe-136">&dagger;In diesem Kontext *Sitzung* bezieht sich auf die clientseitige-Sitzung, die während der Authentifizierung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="afdbe-136">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="afdbe-137">Es ist unabhängig vom stagingstatus serverseitige Sitzungen oder [ASP.NET Core Sitzung Middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="afdbe-137">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="afdbe-138">Benutzer können CSRF Sicherheitsrisiken erraten, durch Vorsichtsmaßnahmen:</span><span class="sxs-lookup"><span data-stu-id="afdbe-138">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="afdbe-139">Melden Sie sich außerhalb der Web-apps, die nach Abschluss ihrer Verwendung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-139">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="afdbe-140">Deaktivieren der Browsercookies in regelmäßigen Abständen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-140">Clear browser cookies periodically.</span></span>

<span data-ttu-id="afdbe-141">Allerdings sind CSRF Sicherheitsrisiken im Grunde ein Problem mit der Web-app, nicht für den Endbenutzer.</span><span class="sxs-lookup"><span data-stu-id="afdbe-141">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="afdbe-142">Grundlagen der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="afdbe-142">Authentication fundamentals</span></span>

<span data-ttu-id="afdbe-143">Cookie-basierte Authentifizierung ist eine gängige Form der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-143">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="afdbe-144">Token-basierter Authentifizierungssysteme wachsen in Beliebtheit, insbesondere für Single Page Applications (SPAs).</span><span class="sxs-lookup"><span data-stu-id="afdbe-144">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="afdbe-145">Cookie-basierte Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="afdbe-145">Cookie-based authentication</span></span>

<span data-ttu-id="afdbe-146">Wenn ein Benutzer authentifiziert hat, mithilfe seines Benutzernamens und Kennworts, sind sie ein Token, das ein Authentifizierungsticket für die Authentifizierung und Autorisierung verwendeten enthält ausgestellt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-146">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="afdbe-147">Das Token wird als ein Cookie, die mit jeder Anforderung der Client stellt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-147">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="afdbe-148">Generieren und überprüfen dieses Cookie wird von der Cookieauthentifizierungsmiddleware ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-148">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="afdbe-149">Die [Middleware](xref:fundamentals/middleware/index) einen Benutzerprinzipal in einem verschlüsselten Cookie serialisiert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-149">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="afdbe-150">Bei nachfolgenden Anforderungen die Middleware überprüft das Cookie, den Prinzipal erstellt, und weist den Prinzipal, der [Benutzer](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) Eigenschaft [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="afdbe-150">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="afdbe-151">Token-basierte Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="afdbe-151">Token-based authentication</span></span>

<span data-ttu-id="afdbe-152">Wenn ein Benutzer authentifiziert wurde, sind sie ein Token (nicht antiforgery Token) ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="afdbe-152">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="afdbe-153">Das Token enthält Benutzerinformationen in Form von [Ansprüche](/dotnet/framework/security/claims-based-identity-model) oder ein Verweistoken, die die app auf Benutzerzustand verwaltet in der app verweist.</span><span class="sxs-lookup"><span data-stu-id="afdbe-153">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="afdbe-154">Wenn ein Benutzer versucht, Zugriff auf eine Ressource, die eine Authentifizierung erforderlich ist, wird das Token an die app mit einer zusätzlichen Authorization-Header in Form von Bearer-Token gesendet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-154">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="afdbe-155">Dadurch wird die app zustandslos.</span><span class="sxs-lookup"><span data-stu-id="afdbe-155">This makes the app stateless.</span></span> <span data-ttu-id="afdbe-156">Jede nachfolgende Anforderung ist das Token für die serverseitige Validierung in der Anforderung übergeben.</span><span class="sxs-lookup"><span data-stu-id="afdbe-156">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="afdbe-157">Dieses Token wird nicht *verschlüsselte*; es hat *codiert*.</span><span class="sxs-lookup"><span data-stu-id="afdbe-157">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="afdbe-158">Auf dem Server wird das Token für den Zugriff auf seine Informationen decodiert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-158">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="afdbe-159">Um das Token bei nachfolgenden Anforderungen zu senden, speichern Sie das Token im lokalen Speicher des Browsers.</span><span class="sxs-lookup"><span data-stu-id="afdbe-159">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="afdbe-160">Werden Sie nicht Sicherheitsrisiko FORGERY besorgt, wenn das Token im lokalen Speicher des Browsers gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-160">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="afdbe-161">CSRF ist relevant, wenn das Token in einem Cookie gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="afdbe-161">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="afdbe-162">Mehrere apps, die an einer Domäne gehostet</span><span class="sxs-lookup"><span data-stu-id="afdbe-162">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="afdbe-163">Freigegebene Hostingumgebungen sind anfällig für Sitzungsübernahme CSRF-Anmeldung und andere Angriffe.</span><span class="sxs-lookup"><span data-stu-id="afdbe-163">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="afdbe-164">Obwohl `example1.contoso.net` und `example2.contoso.net` sind verschiedene Hosts besteht ein implizites Vertrauensverhältnis zwischen Hosts unter der `*.contoso.net` Domäne.</span><span class="sxs-lookup"><span data-stu-id="afdbe-164">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="afdbe-165">Diese implizite Vertrauensstellung kann potenziell nicht vertrauenswürdige Hosts Cookies gegenseitig beeinträchtigen (die gleichen-Origin-Richtlinien, die zum Steuern der AJAX-Anforderungen sind nicht unbedingt auf HTTP-Cookies anwendbar).</span><span class="sxs-lookup"><span data-stu-id="afdbe-165">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="afdbe-166">Angriffe, die vertrauenswürdigen Cookies zwischen apps, die auf der gleichen Domäne gehostet ausnutzen können verhindert werden, indem Sie Domänen nicht freigeben.</span><span class="sxs-lookup"><span data-stu-id="afdbe-166">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="afdbe-167">Wenn jede app in einer eigenen Domäne gehostet wird, besteht keine Vertrauensstellung implizite Cookie, um diese auszunutzen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-167">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="afdbe-168">ASP.NET Core antiforgery-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="afdbe-168">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="afdbe-169">ASP.NET Core implementiert antiforgery mit [ASP.NET Core Datenschutz](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="afdbe-169">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="afdbe-170">Data Protection Stapel muss konfiguriert werden, um in einer Serverfarm zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="afdbe-170">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="afdbe-171">Finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-171">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="afdbe-172">In ASP.NET Core 2.0 oder höher der [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery Token in HTML-Formularelemente injiziert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-172">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="afdbe-173">Das folgende Markup in einer Razor-Datei wird automatisch antiforgery Token generiert:</span><span class="sxs-lookup"><span data-stu-id="afdbe-173">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="afdbe-174">Mithilfe von Pull, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) antiforgery Token standardmäßig generiert, wenn die-Methode des Formulars GET nicht ist.</span><span class="sxs-lookup"><span data-stu-id="afdbe-174">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="afdbe-175">Die automatische Generierung von antiforgery Token für HTML-Formularelemente geschieht bei der `<form>` -Tag enthält die `method="post"` Attribut und eine der folgenden sind "true":</span><span class="sxs-lookup"><span data-stu-id="afdbe-175">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="afdbe-176">Das Action-Attribut ist leer (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="afdbe-176">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="afdbe-177">Das Action-Attribut nicht angegeben (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="afdbe-177">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="afdbe-178">Automatische Generierung von antiforgery Token für HTML-Formularelemente kann deaktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="afdbe-178">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="afdbe-179">Explizit deaktivieren antiforgery-Token mit den `asp-antiforgery` Attribut:</span><span class="sxs-lookup"><span data-stu-id="afdbe-179">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="afdbe-180">Form-Elements ist entschieden-Out-of-Tag-Hilfsprogramme von unter Verwendung des Hilfsprogramms Tag [! Ausschlussverfahren Symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="afdbe-180">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="afdbe-181">Entfernen Sie die `FormTagHelper` aus der Sicht.</span><span class="sxs-lookup"><span data-stu-id="afdbe-181">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="afdbe-182">Die `FormTagHelper` kann aus einer Sicht entfernt werden, indem Sie die folgende Anweisung an die Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="afdbe-182">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="afdbe-183">[Razor-Seiten](xref:mvc/razor-pages/index) vor XSRF/CSRF automatisch geschützt sind.</span><span class="sxs-lookup"><span data-stu-id="afdbe-183">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="afdbe-184">Weitere Informationen finden Sie unter [XSRF/CSRF und Razor-Seiten](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="afdbe-184">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="afdbe-185">Die am häufigsten verwendete Ansatz zum Schutz vor CSRF-Angriffen ist die Verwendung der *für die Domänensynchronisierung Tokenmuster* (STP).</span><span class="sxs-lookup"><span data-stu-id="afdbe-185">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="afdbe-186">STP wird verwendet, wenn der Benutzer eine Seite mit den Formulardaten anfordert:</span><span class="sxs-lookup"><span data-stu-id="afdbe-186">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="afdbe-187">Der Server sendet ein Token, das die Identität des aktuellen Benutzers an den Client zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-187">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="afdbe-188">Der Client sendet das Token wieder an den Server für die Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-188">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="afdbe-189">Wenn der Server ein Token, die Identität des authentifizierten Benutzers übereinstimmt empfängt, wird die Anforderung abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-189">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="afdbe-190">Das Token ist eindeutig und unvorhersehbar.</span><span class="sxs-lookup"><span data-stu-id="afdbe-190">The token is unique and unpredictable.</span></span> <span data-ttu-id="afdbe-191">Das Token kann auch verwendet werden, um sicherzustellen, dass ordnungsgemäße Sequenzierung einer Reihe von Anforderungen (z. B. Sicherstellen der anforderungssequenz des: Seite 1 &ndash; Seite 2 &ndash; Seite 3).</span><span class="sxs-lookup"><span data-stu-id="afdbe-191">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="afdbe-192">Alle Formulare in den Razor-Seiten und ASP.NET Core MVC-Vorlagen generiert antiforgery-Token.</span><span class="sxs-lookup"><span data-stu-id="afdbe-192">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="afdbe-193">Die folgenden zwei Beispiele anzeigen generieren antiforgery Token:</span><span class="sxs-lookup"><span data-stu-id="afdbe-193">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="afdbe-194">Explizit ein antiforgery Token zum Hinzufügen einer `<form>` Element ohne das HTML-Hilfsobjekt Tag Hilfsprogramme mit [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="afdbe-194">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="afdbe-195">In jedem der vorangegangenen Fälle fügt ASP.NET Core ein ausgeblendetes Formularfeld folgt oder ähnlich:</span><span class="sxs-lookup"><span data-stu-id="afdbe-195">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="afdbe-196">ASP.NET Core umfasst drei [Filter](xref:mvc/controllers/filters) für die Arbeit mit antiforgery Token:</span><span class="sxs-lookup"><span data-stu-id="afdbe-196">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="afdbe-197">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="afdbe-197">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="afdbe-198">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="afdbe-198">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="afdbe-199">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="afdbe-199">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="afdbe-200">Antiforgery Optionen</span><span class="sxs-lookup"><span data-stu-id="afdbe-200">Antiforgery options</span></span>

<span data-ttu-id="afdbe-201">Anpassen [antiforgery Optionen](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="afdbe-201">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="afdbe-202">Option</span><span class="sxs-lookup"><span data-stu-id="afdbe-202">Option</span></span> | <span data-ttu-id="afdbe-203">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="afdbe-203">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="afdbe-204">Cookie</span><span class="sxs-lookup"><span data-stu-id="afdbe-204">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="afdbe-205">Hängen die Einstellungen, die zum Erstellen der antiforgery Cookies verwendet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-205">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="afdbe-206">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="afdbe-206">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="afdbe-207">Die Domäne des Cookies.</span><span class="sxs-lookup"><span data-stu-id="afdbe-207">The domain of the cookie.</span></span> <span data-ttu-id="afdbe-208">Wird standardmäßig auf `null` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-208">Defaults to `null`.</span></span> <span data-ttu-id="afdbe-209">Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-209">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="afdbe-210">Die empfohlene Alternative ist Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="afdbe-210">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="afdbe-211">CookieName</span><span class="sxs-lookup"><span data-stu-id="afdbe-211">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="afdbe-212">Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="afdbe-212">The name of the cookie.</span></span> <span data-ttu-id="afdbe-213">Wenn nicht festgelegt, der vom System eine eindeutige Namen, die mit beginnt generiert die [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="afdbe-213">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="afdbe-214">Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-214">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="afdbe-215">Die empfohlene Alternative ist Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="afdbe-215">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="afdbe-216">CookiePath</span><span class="sxs-lookup"><span data-stu-id="afdbe-216">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="afdbe-217">Der Pfad für das Cookie festgelegt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-217">The path set on the cookie.</span></span> <span data-ttu-id="afdbe-218">Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-218">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="afdbe-219">Die empfohlene Alternative ist Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="afdbe-219">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="afdbe-220">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="afdbe-220">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="afdbe-221">Der Name des Felds verborgene vom antiforgery System zum Rendern von antiforgery Token in Ansichten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="afdbe-221">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="afdbe-222">HeaderName</span><span class="sxs-lookup"><span data-stu-id="afdbe-222">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="afdbe-223">Der Name des Headers vom antiforgery System verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="afdbe-223">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="afdbe-224">Wenn `null`, das System berücksichtigt nur Formulardaten.</span><span class="sxs-lookup"><span data-stu-id="afdbe-224">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="afdbe-225">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="afdbe-225">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="afdbe-226">Gibt an, ob SSL vom antiforgery System erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="afdbe-226">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="afdbe-227">Wenn `true`, nicht-SSL-Anforderungen fehl.</span><span class="sxs-lookup"><span data-stu-id="afdbe-227">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="afdbe-228">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-228">Defaults to `false`.</span></span> <span data-ttu-id="afdbe-229">Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="afdbe-230">Die empfohlene Alternative ist die Cookie.SecurePolicy festgelegt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-230">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="afdbe-231">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="afdbe-231">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="afdbe-232">Gibt an, ob die Generierung von Unterdrücken der `X-Frame-Options` Header.</span><span class="sxs-lookup"><span data-stu-id="afdbe-232">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="afdbe-233">Standardmäßig wird der Header mit einem Wert von "SAMEORIGIN" generiert.</span><span class="sxs-lookup"><span data-stu-id="afdbe-233">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="afdbe-234">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-234">Defaults to `false`.</span></span> |

<span data-ttu-id="afdbe-235">Weitere Informationen finden Sie unter [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="afdbe-235">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="afdbe-236">Konfigurieren von antiforgery Funktionen mit IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="afdbe-236">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="afdbe-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) stellt die API zum Konfigurieren von antiforgery Funktionen bereit.</span><span class="sxs-lookup"><span data-stu-id="afdbe-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="afdbe-238">`IAntiforgery` angefordert werden können, der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="afdbe-238">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="afdbe-239">Im folgenden Beispiel wird die Middleware auf der Startseite der app antiforgery-Token generieren und senden es in der Antwort als Cookie (mithilfe der Angular Dateinamenskonvention weiter unten in diesem Thema beschrieben):</span><span class="sxs-lookup"><span data-stu-id="afdbe-239">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="afdbe-240">Antiforgery-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="afdbe-240">Require antiforgery validation</span></span>

<span data-ttu-id="afdbe-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) ist ein Aktionsfilter, die auf einer einzelnen Aktion, die einen Controller angewendet werden kann oder Global.</span><span class="sxs-lookup"><span data-stu-id="afdbe-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="afdbe-242">Anforderungen für Aktionen, die dieser Filter angewendet haben werden blockiert, es sei denn, die Anforderung ein gültiges antiforgery Token enthält.</span><span class="sxs-lookup"><span data-stu-id="afdbe-242">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="afdbe-243">Die `ValidateAntiForgeryToken` Attribut erfordert ein Token für Anforderungen an die Aktionsmethoden, die es ausstattet, einschließlich HTTP GET-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-243">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="afdbe-244">Wenn die `ValidateAntiForgeryToken` Attribut über die app Controller angewendet wird, können sie mit außer Kraft gesetzt werden die `IgnoreAntiforgeryToken` Attribut.</span><span class="sxs-lookup"><span data-stu-id="afdbe-244">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="afdbe-245">ASP.NET Core unterstützt keine GET-Anforderungen automatisch antiforgery Token hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-245">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="afdbe-246">Überprüfen Sie automatisch antiforgery Token für unsichere nur HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="afdbe-246">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="afdbe-247">ASP.NET Core apps erstellen keine antiforgery Token für die sichere HTTP-Methoden (GET, HEAD, Optionen und TRACE).</span><span class="sxs-lookup"><span data-stu-id="afdbe-247">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="afdbe-248">Anstelle von Allgemein Anwenden der `ValidateAntiForgeryToken` Attribut und überschreiben diese mit `IgnoreAntiforgeryToken` Attribute, die [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) Attribut kann verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-248">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="afdbe-249">Dieses Attribut funktioniert genauso wie die `ValidateAntiForgeryToken` Attribut, außer dass es keine Token für Anforderungen, die mit den folgenden HTTP-Methoden erforderlich:</span><span class="sxs-lookup"><span data-stu-id="afdbe-249">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="afdbe-250">GET</span><span class="sxs-lookup"><span data-stu-id="afdbe-250">GET</span></span>
* <span data-ttu-id="afdbe-251">HEAD-,</span><span class="sxs-lookup"><span data-stu-id="afdbe-251">HEAD</span></span>
* <span data-ttu-id="afdbe-252">OPTIONEN</span><span class="sxs-lookup"><span data-stu-id="afdbe-252">OPTIONS</span></span>
* <span data-ttu-id="afdbe-253">TRACE</span><span class="sxs-lookup"><span data-stu-id="afdbe-253">TRACE</span></span>

<span data-ttu-id="afdbe-254">Wir empfehlen die Verwendung von `AutoValidateAntiforgeryToken` weitgefasst gilt dies für nicht-API-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="afdbe-254">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="afdbe-255">Dadurch wird sichergestellt, dass Aktionen nach standardmäßig geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-255">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="afdbe-256">Die Alternative besteht darin, antiforgery Token standardmäßig ignoriert, es sei denn, `ValidateAntiForgeryToken` auf einzelner Aktionsmethoden angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="afdbe-256">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="afdbe-257">Es wahrscheinlicher ist in diesem Szenario für eine POST-Aktionsmethode, verbleiben soll, versehentlich ungeschützten Ort verlassen der app CSRF-Angriffe anfällig.</span><span class="sxs-lookup"><span data-stu-id="afdbe-257">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="afdbe-258">Alle Einträge sollten das antiforgery Token gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-258">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="afdbe-259">APIs haben keinen automatischen Mechanismus zum Senden des nicht-Cookie-Teils des Tokens.</span><span class="sxs-lookup"><span data-stu-id="afdbe-259">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="afdbe-260">Die Implementierung hängt wahrscheinlich die Client-Code-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="afdbe-260">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="afdbe-261">Einige Beispiele werden unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="afdbe-261">Some examples are shown below:</span></span>

<span data-ttu-id="afdbe-262">Auf Klassenebene-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="afdbe-262">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="afdbe-263">Globale Beispiel:</span><span class="sxs-lookup"><span data-stu-id="afdbe-263">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="afdbe-264">Überschreiben globaler oder antiforgery Attributen für controller</span><span class="sxs-lookup"><span data-stu-id="afdbe-264">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="afdbe-265">Die [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) Filter wird verwendet, um ein antiforgery Token für eine bestimmte Aktion (oder Domänencontroller) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="afdbe-265">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="afdbe-266">Beim Anwenden dieses Filters überschreibt `ValidateAntiForgeryToken` und `AutoValidateAntiforgeryToken` Filter, die auf einer höheren Ebene angegeben ist (Global oder auf einem Domänencontroller).</span><span class="sxs-lookup"><span data-stu-id="afdbe-266">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="afdbe-267">Aktualisierungstoken nach der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="afdbe-267">Refresh tokens after authentication</span></span>

<span data-ttu-id="afdbe-268">Token sollte aktualisiert werden, nachdem der Benutzer authentifiziert ist, durch Weiterleiten des Benutzers auf eine Sicht oder eine Seite mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="afdbe-268">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="afdbe-269">JavaScript, AJAX- und SPAs</span><span class="sxs-lookup"><span data-stu-id="afdbe-269">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="afdbe-270">In herkömmlichen HTML-basierte apps werden antiforgery Token an den Server mithilfe von ausgeblendeten Formularfeldern übergeben.</span><span class="sxs-lookup"><span data-stu-id="afdbe-270">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="afdbe-271">Viele Anforderungen werden in modernen JavaScript-basierten apps und SPAs programmgesteuert vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-271">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="afdbe-272">Diese AJAX-Anforderungen möglicherweise andere Techniken (z. B. Anforderungsheader oder Cookies) verwenden, um das Token zu senden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-272">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="afdbe-273">Wenn Cookies zum Speichern von Authentifizierungstoken und zum Authentifizieren von API-Anforderungen auf dem Server verwendet werden, ist CSRF ein potenzielles Problem hin.</span><span class="sxs-lookup"><span data-stu-id="afdbe-273">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="afdbe-274">Lokaler Speicher zum Speichern von Token verwendet wird, möglicherweise Sicherheitsrisiko FORGERY verringert werden, da die Werte aus dem lokalen Speicher automatisch mit jeder Anforderung an den Server gesendet werden nicht.</span><span class="sxs-lookup"><span data-stu-id="afdbe-274">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="afdbe-275">Daher mit dem lokalen Speicher zum Speichern der antiforgery-Token auf dem Client und Senden des Tokens als ein Anforderungsheader und eine empfohlene Vorgehensweise ist.</span><span class="sxs-lookup"><span data-stu-id="afdbe-275">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="afdbe-276">JavaScript</span><span class="sxs-lookup"><span data-stu-id="afdbe-276">JavaScript</span></span>

<span data-ttu-id="afdbe-277">Verwenden von JavaScript mit Sichten, kann das Token erstellt werden mithilfe eines Diensts aus innerhalb der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="afdbe-277">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="afdbe-278">Einfügen der [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) -Dienst in der Ansicht, und rufen [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="afdbe-278">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="afdbe-279">Dadurch entfällt die Notwendigkeit so behandeln Sie direkt mit dem Festlegen von Cookies auf dem Server, oder sie vom Client gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-279">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="afdbe-280">Das obige Beispiel nutzt JavaScript zur Festlegung um den Wert des ausgeblendeten Felds für die AJAX-POST-Header zu lesen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-280">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="afdbe-281">JavaScript kann auch Zugriffstoken in Cookies und das Cookie Inhalt zum Erstellen eines Headers mit dem Wert des Tokens verwenden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-281">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="afdbe-282">Vorausgesetzt, das Skript zum Senden von Token in einer Kopfzeile namens fordert `X-CSRF-TOKEN`, konfigurieren Sie den antiforgery Dienst gesucht werden soll, für die `X-CSRF-TOKEN` Header:</span><span class="sxs-lookup"><span data-stu-id="afdbe-282">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="afdbe-283">Im folgende Beispiel nutzt JavaScript zur Festlegung eine AJAX-Anforderung mit den entsprechenden Header vornehmen:</span><span class="sxs-lookup"><span data-stu-id="afdbe-283">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="afdbe-284">AngularJS</span><span class="sxs-lookup"><span data-stu-id="afdbe-284">AngularJS</span></span>

<span data-ttu-id="afdbe-285">AngularJS verwendet stattdessen eine Konvention CSRF-Adresse.</span><span class="sxs-lookup"><span data-stu-id="afdbe-285">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="afdbe-286">Wenn der Server ein Cookie mit dem Namen sendet `XSRF-TOKEN`, die AngularJS `$http` Dienst einen Header den Cookiewert hinzugefügt, wenn er eine Anforderung an den Server sendet.</span><span class="sxs-lookup"><span data-stu-id="afdbe-286">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="afdbe-287">Dieser Prozess erfolgt automatisch.</span><span class="sxs-lookup"><span data-stu-id="afdbe-287">This process is automatic.</span></span> <span data-ttu-id="afdbe-288">Der Header muss nicht explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="afdbe-288">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="afdbe-289">Der Headername wird `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="afdbe-289">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="afdbe-290">Der Server sollte dieser Header zu erkennen und seinen Inhalt zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-290">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="afdbe-291">Arbeiten Sie mit dieser Konvention für ASP.NET Haupt-API:</span><span class="sxs-lookup"><span data-stu-id="afdbe-291">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="afdbe-292">Ihre app so konfigurieren, geben Sie ein Token in einem Cookie aufgerufen `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="afdbe-292">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="afdbe-293">Konfigurieren Sie den antiforgery Dienst zum Suchen nach einem Header mit dem Namen `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="afdbe-293">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="afdbe-294">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="afdbe-294">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="afdbe-295">Erweitern Sie antiforgery</span><span class="sxs-lookup"><span data-stu-id="afdbe-295">Extend antiforgery</span></span>

<span data-ttu-id="afdbe-296">Die [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) Typ ermöglicht Entwicklern das Verhalten des Anti-CSRF-Systems durch zusätzliche Round-Tripping-Daten in einzelnen Token zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="afdbe-296">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="afdbe-297">Die [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) Methode wird jedes Mal aufgerufen, wenn ein Token wird generiert, und der Rückgabewert in das generierte Token eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="afdbe-297">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="afdbe-298">Eine Implementierung konnte einen Zeitstempel, eine Nonce oder ein anderer Wert zurückgeben, und rufen dann [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) dieser Daten validieren, wenn das Token überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="afdbe-298">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="afdbe-299">Der Client Benutzernamen ist bereits in die generierten Token eingebettet, daher keine Notwendigkeit besteht, diese Informationen umfassen.</span><span class="sxs-lookup"><span data-stu-id="afdbe-299">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="afdbe-300">Wenn ein Token ergänzenden Daten, aber keine enthält `IAntiForgeryAdditionalDataProvider` wird konfiguriert, die zusätzlichen Daten wird nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="afdbe-300">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="afdbe-301">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="afdbe-301">Additional resources</span></span>

* <span data-ttu-id="afdbe-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) auf [öffnen Webanwendungsprojekt Sicherheit](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="afdbe-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
