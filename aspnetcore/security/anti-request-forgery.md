---
title: "Verhindern von Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung-Angriffen in ASP.NET Core"
author: steve-smith
ms.author: riande
description: "Verhindern von Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung-Angriffen in ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: 3c0f90dd9894c362c0d7fef5d1f1da076991605c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="5cf99-103">Verhindern von Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung-Angriffen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cf99-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="5cf99-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5cf99-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="5cf99-105">Welche Angriff verhindert das fälschungssicherheitssystem auf einen?</span><span class="sxs-lookup"><span data-stu-id="5cf99-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="5cf99-106">Websiteübergreifende anforderungsfälschung (XSRF oder CSRF, auch bekannt als Aussprache *Siehe Surfen*) ist ein Angriff gegen Web gehostete Anwendungen, bei dem eine bösartige Website die Interaktion zwischen einem Clientbrowser und eine Website, die Vertrauensstellungen beeinflussen können, Dieser Browser.</span><span class="sxs-lookup"><span data-stu-id="5cf99-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="5cf99-107">Diese Angriffe sind möglich, da Webbrowsern einige Arten von Authentifizierungstoken automatisch mit jeder Anforderung an eine Website zu senden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="5cf99-108">Diese Form von Angriff ist auch bekannt als ein *einmalklick-Angriff* oder als *Sitzung riding*, da der Angriff nutzt des Benutzers zuvor den authentifizierten Sitzung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-108">This form of exploit is also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="5cf99-109">Ein Beispiel von CSRF-Angriffen:</span><span class="sxs-lookup"><span data-stu-id="5cf99-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="5cf99-110">Ein Benutzer meldet sich bei `www.example.com`, mit Formularauthentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="5cf99-111">Der Server authentifiziert den Benutzer und gibt eine Antwort, die ein Authentifizierungscookie enthält.</span><span class="sxs-lookup"><span data-stu-id="5cf99-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="5cf99-112">Der Benutzer besucht eine bösartige Website.</span><span class="sxs-lookup"><span data-stu-id="5cf99-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="5cf99-113">Die bösartige Website enthält ein HTML-Formular, das etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5cf99-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="5cf99-114">Beachten Sie, dass die formaktion an den Standort anfällig für nicht auf die bösartige Website sendet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="5cf99-115">Dies ist der "Cross-Site" Teil CSRF.</span><span class="sxs-lookup"><span data-stu-id="5cf99-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="5cf99-116">Der Benutzer klickt auf die Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="5cf99-116">The user clicks the submit button.</span></span> <span data-ttu-id="5cf99-117">Der Browser schließt automatisch das Authentifizierungscookie für die angeforderte Domäne (in diesem Fall die anfällig für Standort) mit der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="5cf99-118">Die Anforderung auf dem Server mit dem Kontext des Benutzers Authentifizierung ausgeführt und Aktionen möglich, die ein authentifizierter Benutzer tun darf.</span><span class="sxs-lookup"><span data-stu-id="5cf99-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="5cf99-119">In diesem Beispiel wird der Benutzer auf die Schaltfläche klicken muss.</span><span class="sxs-lookup"><span data-stu-id="5cf99-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="5cf99-120">Böswillige Seite konnte:</span><span class="sxs-lookup"><span data-stu-id="5cf99-120">The malicious page could:</span></span>

* <span data-ttu-id="5cf99-121">Führen Sie ein Skript, das automatisch auf das Formular sendet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="5cf99-122">Sendet eine Formularübermittlung als eine AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="5cf99-123">Verwenden Sie ein ausgeblendetes Formular mit CSS ein.</span><span class="sxs-lookup"><span data-stu-id="5cf99-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="5cf99-124">Verwendung von SSL verhindert nicht, dass eine CSRF-Angriffen, kann die bösartige Website senden eine `https://` Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-124">Using SSL does not prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="5cf99-125">Einige Angriffe abzielen, Standort-Endpunkte, auf die reagiert `GET` Anforderungen, in dem Fall Bildtag zum Ausführen der Aktion (diese Form des Angriffs befindet sich häufig auf Forum-Standorte, die Bilder ermöglichen jedoch blockieren JavaScript) verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5cf99-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="5cf99-126">Anwendungen, die mit Statuswechsel `GET` Anforderungen vor böswilligen Angriffen anfällig sind.</span><span class="sxs-lookup"><span data-stu-id="5cf99-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="5cf99-127">CSRF-Angriffen sind möglich für Websites, die Cookies für die Authentifizierung verwenden, da der Browser alle relevanten Cookies an die Ziel-Website senden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="5cf99-128">CSRF-Angriffen sind jedoch nicht beschränkt auf Cookies ausnutzen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="5cf99-129">Beispielsweise Basis-und Digestauthentifizierung sind auch anfällig.</span><span class="sxs-lookup"><span data-stu-id="5cf99-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="5cf99-130">Nachdem ein Benutzer mit Standard oder Digest-Authentifizierung anmeldet, sendet der Browser automatisch die Anmeldeinformationen an, bis die Sitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="5cf99-131">Hinweis: In diesem Kontext *Sitzung* bezieht sich auf die clientseitige-Sitzung, die während der Authentifizierung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="5cf99-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="5cf99-132">Es ist nicht mit serverseitigen Sitzungen oder [Sitzung Middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="5cf99-132">It is unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="5cf99-133">Benutzer können CSRF Schwachstellen durch erraten:</span><span class="sxs-lookup"><span data-stu-id="5cf99-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="5cf99-134">Protokollierung von Websites, nach deren Verwendung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="5cf99-135">Löschen Sie ihren Browser-Cookies in regelmäßigen Abständen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="5cf99-136">Allerdings sind CSRF Sicherheitsrisiken im Grunde ein Problem mit der Web-app, nicht für den Endbenutzer.</span><span class="sxs-lookup"><span data-stu-id="5cf99-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="5cf99-137">Wie befasst Core ASP.NET-MVC CSRF?</span><span class="sxs-lookup"><span data-stu-id="5cf99-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="5cf99-138">ASP.NET Core implementiert anti-request-Fälschung mithilfe der [ASP.NET Core Data Protection Stack](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="5cf99-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5cf99-139">ASP.NET Core Datenschutz muss konfiguriert werden, um in einer Serverfarm zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5cf99-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="5cf99-140">Finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="5cf99-141">ASP.NET Core anti-request-Fälschung Data Protection Standardkonfiguration</span><span class="sxs-lookup"><span data-stu-id="5cf99-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="5cf99-142">In ASP.NET Core MVC 2.0 die [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) fälschungssicherheitstoken für HTML-Formularelemente injiziert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="5cf99-143">Beispielsweise wird das folgende Markup in einer Razor-Datei automatisch fälschungssicherheitstoken generiert:</span><span class="sxs-lookup"><span data-stu-id="5cf99-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="5cf99-144">Die automatische Generierung von fälschungssicherheitstoken für HTML-Formularelemente geschieht, wenn:</span><span class="sxs-lookup"><span data-stu-id="5cf99-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="5cf99-145">Die `form` -Tag enthält die `method="post"` Attribut AND</span><span class="sxs-lookup"><span data-stu-id="5cf99-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="5cf99-146">Das Action-Attribut ist leer.</span><span class="sxs-lookup"><span data-stu-id="5cf99-146">The action attribute is empty.</span></span> <span data-ttu-id="5cf99-147">( `action=""`) ODER</span><span class="sxs-lookup"><span data-stu-id="5cf99-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="5cf99-148">Das Action-Attribut wird nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="5cf99-148">The action attribute is not supplied.</span></span> <span data-ttu-id="5cf99-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="5cf99-149">(`<form method="post">`)</span></span>

<span data-ttu-id="5cf99-150">Sie können die automatische Generierung von fälschungssicherheitstoken für HTML-Formularelemente durch deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="5cf99-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="5cf99-151">Explizit deaktiviert `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="5cf99-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="5cf99-152">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5cf99-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="5cf99-153">Entscheiden Sie außerhalb des gültigen Tag Hilfsprogramme Form-Elements, indem Sie unter Verwendung des Hilfsprogramms Tag [! Ausschlussverfahren Symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="5cf99-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="5cf99-154">Entfernen Sie die `FormTagHelper` aus der Sicht.</span><span class="sxs-lookup"><span data-stu-id="5cf99-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="5cf99-155">Entfernen Sie die `FormTagHelper` aus einer Sicht, indem Sie die folgende Anweisung an die Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="5cf99-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="5cf99-156">[Razor-Seiten](xref:mvc/razor-pages/index) vor XSRF/CSRF automatisch geschützt sind.</span><span class="sxs-lookup"><span data-stu-id="5cf99-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="5cf99-157">Sie müssen keinen zusätzlichen Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="5cf99-157">You don't have to write any additional code.</span></span> <span data-ttu-id="5cf99-158">Finden Sie unter [XSRF/CSRF und Razor-Seiten](xref:mvc/razor-pages/index#xsrf) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="5cf99-159">Die am häufigsten verwendete Ansatz zum Schutz vor CSRF-Angriffen wird das token für die domänensynchronisierung-Muster (STP).</span><span class="sxs-lookup"><span data-stu-id="5cf99-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="5cf99-160">STP ist eine Technik, die verwendet werden, wenn der Benutzer eine Seite mit Daten anfordert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="5cf99-161">Der Server sendet ein Token, das die Identität des aktuellen Benutzers an den Client zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="5cf99-162">Der Client sendet das Token wieder an den Server für die Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="5cf99-163">Wenn der Server ein Token, die Identität des authentifizierten Benutzers übereinstimmt empfängt, wird die Anforderung abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="5cf99-164">Das Token ist eindeutig und unvorhersehbar.</span><span class="sxs-lookup"><span data-stu-id="5cf99-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="5cf99-165">Das Token kann auch verwendet werden, um sicherzustellen, dass ordnungsgemäße Sequenzierung einer Reihe von Anforderungen (sicherstellen, dass Seite 1 vorausgeht Seite 2 vor der Seite "3").</span><span class="sxs-lookup"><span data-stu-id="5cf99-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="5cf99-166">Alle Formulare in ASP.NET Core MVC-Vorlagen generiert antiforgery-Token.</span><span class="sxs-lookup"><span data-stu-id="5cf99-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="5cf99-167">Die folgenden zwei Beispiele für ansichtslogik antiforgery Token zu generieren:</span><span class="sxs-lookup"><span data-stu-id="5cf99-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="5cf99-168">Sie können eine antiforgery Token explizit hinzufügen eine ``<form>`` Element ohne das HTML-Hilfsobjekt Tag Hilfsprogramme mit ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="5cf99-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

```html
In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:

<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="5cf99-169">ASP.NET Core umfasst drei [Filter](xref:mvc/controllers/filters) für die Arbeit mit antiforgery Token: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, und ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="5cf99-169">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="5cf99-170">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cf99-170">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="5cf99-171">Die ``ValidateAntiForgeryToken`` ist ein Aktionsfilter, die auf einer einzelnen Aktion, die einen Controller angewendet werden kann oder Global.</span><span class="sxs-lookup"><span data-stu-id="5cf99-171">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="5cf99-172">Anforderungen für Aktionen, die dieser Filter angewendet werden, wenn die Anforderung ein gültiges antiforgery Token enthält blockiert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-172">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="5cf99-173">Die ``ValidateAntiForgeryToken`` Attribut erfordert ein Token für Anforderungen an Aktionsmethoden, die es ausstattet, einschließlich `HTTP GET` Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-173">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="5cf99-174">Allgemein gelten, überschreiben Sie diese mit der ``IgnoreAntiforgeryToken`` Attribut.</span><span class="sxs-lookup"><span data-stu-id="5cf99-174">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="5cf99-175">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cf99-175">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="5cf99-176">ASP.NET Core apps generieren antiforgery Token für die sichere HTTP-Methoden (GET, HEAD, Optionen und TRACE) in der Regel nicht.</span><span class="sxs-lookup"><span data-stu-id="5cf99-176">ASP.NET Core apps generally do not generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="5cf99-177">Statt Allgemein anzuwenden der ``ValidateAntiForgeryToken`` -Attribut, und überschreiben diese mit ``IgnoreAntiforgeryToken`` Attribute, die Sie verwenden die ``AutoValidateAntiforgeryToken`` Attribut.</span><span class="sxs-lookup"><span data-stu-id="5cf99-177">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="5cf99-178">Dieses Attribut funktioniert genauso wie die ``ValidateAntiForgeryToken`` Attribut, außer dass es keine Token für Anforderungen, die mit den folgenden HTTP-Methoden erforderlich:</span><span class="sxs-lookup"><span data-stu-id="5cf99-178">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="5cf99-179">GET</span><span class="sxs-lookup"><span data-stu-id="5cf99-179">GET</span></span>
* <span data-ttu-id="5cf99-180">HEAD-,</span><span class="sxs-lookup"><span data-stu-id="5cf99-180">HEAD</span></span>
* <span data-ttu-id="5cf99-181">OPTIONEN</span><span class="sxs-lookup"><span data-stu-id="5cf99-181">OPTIONS</span></span>
* <span data-ttu-id="5cf99-182">TRACE</span><span class="sxs-lookup"><span data-stu-id="5cf99-182">TRACE</span></span>

<span data-ttu-id="5cf99-183">Es wird empfohlen, ``AutoValidateAntiforgeryToken`` weitgefasst gilt dies für nicht-API-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="5cf99-183">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="5cf99-184">Dadurch wird sichergestellt, dass Ihre Aktionen POST standardmäßig geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-184">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="5cf99-185">Die Alternative besteht darin, antiforgery Token standardmäßig ignoriert, es sei denn, ``ValidateAntiForgeryToken`` auf die einzelnen Aktionsmethode angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="5cf99-185">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="5cf99-186">Es ist wahrscheinlicher in diesem Szenario für eine POST-Aktionsmethode, werden Links nicht geschützt ist, lassen Ihre app CSRF-Angriffe anfällig.</span><span class="sxs-lookup"><span data-stu-id="5cf99-186">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="5cf99-187">Sogar anonyme Beiträge sollten antiforgery Token gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-187">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="5cf99-188">Hinweis: Die APIs nicht automatischen Mechanismus zum Senden des nicht-Cookie-Teils des Tokens verfügen; Ihre Implementierung hängen Ihre Client-Code-Implementierung wahrscheinlich.</span><span class="sxs-lookup"><span data-stu-id="5cf99-188">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="5cf99-189">Einige Beispiele werden unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-189">Some examples are shown below.</span></span>


<span data-ttu-id="5cf99-190">Beispiel (auf Klassenebene):</span><span class="sxs-lookup"><span data-stu-id="5cf99-190">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="5cf99-191">Beispiel (global):</span><span class="sxs-lookup"><span data-stu-id="5cf99-191">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="5cf99-192">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5cf99-192">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="5cf99-193">Die ``IgnoreAntiforgeryToken`` Filter wird verwendet, um eine antiforgery Token für eine bestimmte Aktion (oder Domänencontroller) vorhanden ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5cf99-193">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="5cf99-194">Beim Anwenden dieses Filters wird überschreiben ``ValidateAntiForgeryToken`` und/oder ``AutoValidateAntiforgeryToken`` Filter, die auf einer höheren Ebene angegeben ist (Global oder auf einem Domänencontroller).</span><span class="sxs-lookup"><span data-stu-id="5cf99-194">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="5cf99-195">JavaScript, AJAX- und SPAs</span><span class="sxs-lookup"><span data-stu-id="5cf99-195">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="5cf99-196">In herkömmlichen HTML-basierten Anwendungen werden die antiforgery Token an den Server mithilfe von ausgeblendeten Formularfeldern übergeben.</span><span class="sxs-lookup"><span data-stu-id="5cf99-196">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="5cf99-197">In modernen JavaScript-basierten apps und einseitige Anwendungen (SPAs) werden viele Anforderungen programmgesteuert vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-197">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="5cf99-198">Diese AJAX-Anforderungen möglicherweise andere Techniken (z. B. Anforderungsheader oder Cookies) verwenden, um das Token zu senden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-198">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="5cf99-199">Wenn Cookies zum Speichern von Authentifizierungstoken und zum Authentifizieren von API-Anforderungen auf dem Server verwendet werden, werden CSRF ein potenzielles Problem hin.</span><span class="sxs-lookup"><span data-stu-id="5cf99-199">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="5cf99-200">Jedoch wenn lokaler Speicher zum Speichern von Token verwendet wird, möglicherweise Sicherheitsrisiko FORGERY verringert werden, da die Werte aus dem lokalen Speicher nicht automatisch mit der jede neue Anforderung an den Server gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-200">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="5cf99-201">Daher mit dem lokalen Speicher zum Speichern der antiforgery-Token auf dem Client und Senden des Tokens als ein Anforderungsheader und eine empfohlene Vorgehensweise ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-201">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="5cf99-202">AngularJS</span><span class="sxs-lookup"><span data-stu-id="5cf99-202">AngularJS</span></span>

<span data-ttu-id="5cf99-203">AngularJS verwendet stattdessen eine Konvention CSRF-Adresse.</span><span class="sxs-lookup"><span data-stu-id="5cf99-203">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="5cf99-204">Wenn der Server ein Cookie mit dem Namen sendet ``XSRF-TOKEN``, das Drehmoment ``$http`` Dienst wird der Wert hinzufügen dieses Cookie einen Header beim Senden einer Anforderungs auf diesem Server.</span><span class="sxs-lookup"><span data-stu-id="5cf99-204">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="5cf99-205">Dieser Prozess erfolgt automatisch; Sie müssen nicht explizit der Header festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-205">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="5cf99-206">Der Headername wird ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="5cf99-206">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="5cf99-207">Der Server sollte dieser Header zu erkennen und seinen Inhalt zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-207">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="5cf99-208">Arbeiten Sie mit dieser Konvention für ASP.NET Haupt-API:</span><span class="sxs-lookup"><span data-stu-id="5cf99-208">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="5cf99-209">Konfigurieren Sie Ihre app so, geben Sie ein Token in einem Cookie wird aufgerufen``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="5cf99-209">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="5cf99-210">Konfigurieren Sie den antiforgery Dienst für einen Header mit dem Namen gesucht werden soll.``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="5cf99-210">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="5cf99-211">[Viewer-Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="5cf99-211">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="5cf99-212">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5cf99-212">JavaScript</span></span>

<span data-ttu-id="5cf99-213">Verwenden von JavaScript mit Sichten, können Sie das Token mit einem Dienst in Ihrer Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-213">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="5cf99-214">Zu diesem Zweck fügen Sie der `Microsoft.AspNetCore.Antiforgery.IAntiforgery` -Dienst in der Ansicht, und rufen `GetAndStoreTokens`, wie dargestellt:</span><span class="sxs-lookup"><span data-stu-id="5cf99-214">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

<span data-ttu-id="5cf99-215">[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]</span><span class="sxs-lookup"><span data-stu-id="5cf99-215">[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]</span></span>

<span data-ttu-id="5cf99-216">Dadurch entfällt die Notwendigkeit so behandeln Sie direkt mit dem Festlegen von Cookies auf dem Server, oder sie vom Client gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="5cf99-217">JavaScript kann auch Zugriffstoken in Cookies bereitgestellt, und verwenden Sie das Cookie Inhalt zum Erstellen eines Headers mit dem Wert für das Token, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="5cf99-218">Klicken Sie dann zum Senden von Token in einer Kopfzeile namens fordert vorausgesetzt, Sie erstellen das Skript ``X-CSRF-TOKEN``, konfigurieren Sie den antiforgery Dienst gesucht werden soll, für die ``X-CSRF-TOKEN`` Header:</span><span class="sxs-lookup"><span data-stu-id="5cf99-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="5cf99-219">Im folgenden Beispiel wird jQuery, um eine AJAX-Anforderung mit den entsprechenden Header zu machen:</span><span class="sxs-lookup"><span data-stu-id="5cf99-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="5cf99-220">Konfigurieren von Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5cf99-220">Configuring Antiforgery</span></span>

<span data-ttu-id="5cf99-221">`IAntiforgery`Stellt die API so konfigurieren Sie die antiforgery System bereit.</span><span class="sxs-lookup"><span data-stu-id="5cf99-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="5cf99-222">Es muss angefordert werden der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="5cf99-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="5cf99-223">Im folgenden Beispiel wird die Middleware auf der Startseite der app antiforgery-Token generieren und senden es in der Antwort als Cookie (mithilfe der Angular Dateinamenskonvention oben beschriebene):</span><span class="sxs-lookup"><span data-stu-id="5cf99-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="5cf99-224">Optionen</span><span class="sxs-lookup"><span data-stu-id="5cf99-224">Options</span></span>

<span data-ttu-id="5cf99-225">Sie können anpassen, [antiforgery Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5cf99-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="5cf99-226">Option</span><span class="sxs-lookup"><span data-stu-id="5cf99-226">Option</span></span>        | <span data-ttu-id="5cf99-227">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="5cf99-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="5cf99-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5cf99-228">CookieDomain</span></span>  | <span data-ttu-id="5cf99-229">Die Domäne des Cookies.</span><span class="sxs-lookup"><span data-stu-id="5cf99-229">The domain of the cookie.</span></span> <span data-ttu-id="5cf99-230">Wird standardmäßig auf `null` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="5cf99-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="5cf99-231">CookieName</span></span>    | <span data-ttu-id="5cf99-232">Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="5cf99-232">The name of the cookie.</span></span> <span data-ttu-id="5cf99-233">Wenn nicht festgelegt ist, das System eine eindeutige Namen, die mit beginnt generiert, die `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="5cf99-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="5cf99-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5cf99-234">CookiePath</span></span>    | <span data-ttu-id="5cf99-235">Der Pfad für das Cookie festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="5cf99-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="5cf99-236">FormFieldName</span></span> | <span data-ttu-id="5cf99-237">Der Name des Felds verborgene vom antiforgery System zum Rendern von antiforgery Token in Ansichten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="5cf99-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="5cf99-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="5cf99-238">HeaderName</span></span>    | <span data-ttu-id="5cf99-239">Der Name des Headers vom antiforgery System verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="5cf99-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="5cf99-240">Wenn `null`, das System berücksichtigt nur Formulardaten.</span><span class="sxs-lookup"><span data-stu-id="5cf99-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="5cf99-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="5cf99-241">RequireSsl</span></span>    | <span data-ttu-id="5cf99-242">Gibt an, ob SSL vom antiforgery System erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="5cf99-243">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-243">Defaults to `false`.</span></span> <span data-ttu-id="5cf99-244">Wenn `true`, nicht-SSL-Anforderungen schlagen fehl.</span><span class="sxs-lookup"><span data-stu-id="5cf99-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="5cf99-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="5cf99-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="5cf99-246">Gibt an, ob die Generierung von Unterdrücken der `X-Frame-Options` Header.</span><span class="sxs-lookup"><span data-stu-id="5cf99-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="5cf99-247">Standardmäßig wird der Header mit einem Wert von "SAMEORIGIN" generiert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="5cf99-248">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-248">Defaults to `false`.</span></span> |

<span data-ttu-id="5cf99-249">Finden Sie unter https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="5cf99-250">Erweitern von Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5cf99-250">Extending Antiforgery</span></span>

<span data-ttu-id="5cf99-251">Die [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) Typ ermöglicht Entwicklern das Verhalten des Anti-XSRF-Systems durch zusätzliche Round-Tripping-Daten in einzelnen Token zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="5cf99-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="5cf99-252">Die [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) Methode wird jedes Mal aufgerufen, wenn ein Token wird generiert, und der Rückgabewert in das generierte Token eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="5cf99-253">Eine Implementierung konnte einen Zeitstempel, eine Nonce oder ein anderer Wert zurückgeben, und rufen dann [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) dieser Daten validieren, wenn das Token überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="5cf99-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="5cf99-254">Der Client Benutzernamen ist bereits in die generierten Token eingebettet, daher keine Notwendigkeit besteht, diese Informationen umfassen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-254">The client's username is already embedded in the generated tokens, so there is no need to include this information.</span></span> <span data-ttu-id="5cf99-255">Wenn ein Token ergänzenden Daten, aber keine enthält `IAntiForgeryAdditionalDataProvider` wurde konfiguriert, wird die ergänzenden Daten nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="5cf99-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data is not validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="5cf99-256">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="5cf99-256">Fundamentals</span></span>

<span data-ttu-id="5cf99-257">CSRF-Angriffen basieren auf dem Standardverhalten der Browser Cookies, die mit einer Domäne verknüpft sind, mit jeder Anforderung an, dass diese Domäne zu senden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="5cf99-258">Diese Cookies werden innerhalb des Browsers gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="5cf99-259">Sie enthalten häufig Sitzungscookies für authentifizierte Benutzer.</span><span class="sxs-lookup"><span data-stu-id="5cf99-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="5cf99-260">Cookie-basierte Authentifizierung ist eine gängige Form der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5cf99-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="5cf99-261">Token-basierte Authentifizierungssysteme haben in Beliebtheit, vor allem für SPAs und andere Szenarien "smart Client" zugenommen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="5cf99-262">Cookie-basierte Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5cf99-262">Cookie-based authentication</span></span>

<span data-ttu-id="5cf99-263">Sobald ein Benutzer mithilfe ihres Benutzernamens und Kennworts authentifiziert wurde, sind sie ein Token ausgestellt, die verwendet werden kann, um sie zu identifizieren und überprüfen, dass sie authentifiziert wurden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-263">Once a user has authenticated using their username and password, they are issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="5cf99-264">Das Token wird als ein Cookie, die mit jeder Anforderung der Client stellt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5cf99-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="5cf99-265">Generieren und überprüfen dieses Cookie wird von der cookieauthentifizierungsmiddleware ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="5cf99-266">ASP.NET Core bietet Cookie [Middleware](../fundamentals/middleware.md) der serialisiert eines Benutzerprinzipals in einem verschlüsselten Cookie, und klicken Sie dann bei nachfolgenden Anforderungen überprüft das Cookie wird den Prinzipal erstellt, und weist sie der `User` Eigenschaft `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5cf99-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="5cf99-267">Wenn ein Cookie verwendet wird, ist das Authentifizierungscookie lediglich als Container für das Formularauthentifizierungsticket.</span><span class="sxs-lookup"><span data-stu-id="5cf99-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="5cf99-268">Das Ticket wird als Wert des Formularauthentifizierungscookies mit jeder Anforderung übergeben und dient Formularauthentifizierung auf dem Server beim Identifizieren eines authentifiziertes Benutzers verwendet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="5cf99-269">Wenn ein Benutzer mit einem System angemeldet ist, wird eine Sitzung des Benutzers wird auf der Serverseite erstellt und in einer Datenbank oder einem anderen permanenten Speicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="5cf99-270">Das System generiert einen Sitzungsschlüssel, der auf die aktuelle Sitzung im Datenspeicher verweist, und sie als Client Side Cookie gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5cf99-270">The system generates a session key that points to the actual session in the data store and it is sent as a client side cookie.</span></span> <span data-ttu-id="5cf99-271">Prüft der Webserver diesen Sitzungsschlüssel jedes Mal ein Benutzer eine Ressource anfordert, die Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="5cf99-272">Das System überprüft, ob die Sitzung zugeordneten Benutzer über die Berechtigung zum Zugriff auf die angeforderte Ressource verfügt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="5cf99-273">Wenn dies der Fall ist, wird die Anforderung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="5cf99-273">If so, the request continues.</span></span> <span data-ttu-id="5cf99-274">Andernfalls gibt die Anforderung als nicht autorisierte zurück.</span><span class="sxs-lookup"><span data-stu-id="5cf99-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="5cf99-275">Bei dieser Vorgehensweise Cookies werden verwendet, um die Anwendung angezeigt werden, um statusbehafteten werden, da er "merken" kann der Benutzer wurde zuvor authentifiziert mit dem Server.</span><span class="sxs-lookup"><span data-stu-id="5cf99-275">In this approach, cookies are used to make the application appear to be stateful, since it is able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="5cf99-276">Benutzertoken</span><span class="sxs-lookup"><span data-stu-id="5cf99-276">User tokens</span></span>

<span data-ttu-id="5cf99-277">Token-basierte Authentifizierung speichern keine Sitzung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="5cf99-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="5cf99-278">Wenn ein Benutzer, in angemeldet ist werden sie stattdessen ein Token (nicht antiforgery Token) ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="5cf99-278">Instead, when a user is logged in they are issued a token (not an antiforgery token).</span></span> <span data-ttu-id="5cf99-279">Dieses Token enthält alle Daten, die erforderlich ist, um das Token zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5cf99-279">This token holds all the data that is required to validate the token.</span></span> <span data-ttu-id="5cf99-280">Es enthält auch Benutzerinformationen in Form von [Ansprüche](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="5cf99-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="5cf99-281">Wenn ein Benutzer möchte den Zugriff auf eine Serverressource, die eine Authentifizierung erforderlich ist, wird das Token mit einer zusätzlichen Authorization-Header in Form von Träger {Token} an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="5cf99-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="5cf99-282">Dadurch wird die Anwendung zustandslose, da das Token in jede nachfolgende Anforderung für die serverseitige Validierung in der Anforderung übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="5cf99-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="5cf99-283">Dieses Token ist nicht *verschlüsselte*; es *codiert*.</span><span class="sxs-lookup"><span data-stu-id="5cf99-283">This token is not *encrypted*; rather it is *encoded*.</span></span> <span data-ttu-id="5cf99-284">Auf der Serverseite kann das Token für den Zugriff auf die unformatierten Informationen innerhalb der Token decodiert werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="5cf99-285">Um das Token in nachfolgenden Anforderungen zu senden, können Sie entweder es im lokalen Speicher des Browsers oder in einem Cookie speichern.</span><span class="sxs-lookup"><span data-stu-id="5cf99-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="5cf99-286">Sie müssen nicht kümmern XSRF Sicherheitsrisiko, wenn das Token im lokalen Speicher gespeichert ist, aber es ist ein Problem auf, wenn das Token in einem Cookie gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="5cf99-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it is an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="5cf99-287">Mehrere Anwendungen werden in einer Domäne gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="5cf99-288">Obwohl `example1.cloudapp.net` und `example2.cloudapp.net` sind verschiedene Hosts besteht ein implizites Vertrauensverhältnis zwischen allen Hosts unter der `*.cloudapp.net` Domäne.</span><span class="sxs-lookup"><span data-stu-id="5cf99-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there is an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="5cf99-289">Diese implizite Vertrauensstellung kann potenziell nicht vertrauenswürdige Hosts Cookies gegenseitig beeinträchtigen (die gleichen-Origin-Richtlinien, die AJAX-Anforderungen steuern unbedingt gelten nicht für HTTP-Cookies).</span><span class="sxs-lookup"><span data-stu-id="5cf99-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests do not necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="5cf99-290">Die ASP.NET Core-Laufzeit bietet einige Verringerung, der Benutzernamen in das Feldtoken eingebettet ist, selbst wenn eine böswillige Unterdomäne einen Sitzungstoken überschreiben kann zum Generieren eines gültigen Felds Tokens für den Benutzer werden.</span><span class="sxs-lookup"><span data-stu-id="5cf99-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="5cf99-291">Allerdings können nicht beim Hosten in einer derartigen Umgebung die integrierte Anti-XSRF-Routinen weiterhin Sitzungsübernahme oder Anmeldung CSRF Verteidigung gegen Angriffe.</span><span class="sxs-lookup"><span data-stu-id="5cf99-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="5cf99-292">Freigegebene Hostingumgebungen sind Vunerable Sitzungsübernahme CSRF-Anmeldung und andere Angriffe.</span><span class="sxs-lookup"><span data-stu-id="5cf99-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="5cf99-293">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5cf99-293">Additional Resources</span></span>

* <span data-ttu-id="5cf99-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) auf [öffnen Webanwendungsprojekt Sicherheit](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="5cf99-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
