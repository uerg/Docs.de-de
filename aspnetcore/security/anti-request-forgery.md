---
title: "-Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core"
author: steve-smith
description: "Erfahren Sie, wie ein, um Angriffe auf Web-apps zu verhindern, in denen eine bösartige Website die Interaktion zwischen einem Clientbrowser und die app auswirken kann."
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="5221b-103">-Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5221b-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="5221b-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5221b-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="5221b-105">Welche Angriff verhindert das fälschungssicherheitssystem auf einen?</span><span class="sxs-lookup"><span data-stu-id="5221b-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="5221b-106">Websiteübergreifende anforderungsfälschung (XSRF oder CSRF, auch bekannt als Aussprache *Siehe Surfen*) ist ein Angriff gegen Web gehostete Anwendungen, bei dem eine bösartige Website die Interaktion zwischen einem Clientbrowser und eine Website, die Vertrauensstellungen beeinflussen können, Dieser Browser.</span><span class="sxs-lookup"><span data-stu-id="5221b-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="5221b-107">Diese Angriffe sind möglich, da Webbrowsern einige Arten von Authentifizierungstoken automatisch mit jeder Anforderung an eine Website zu senden.</span><span class="sxs-lookup"><span data-stu-id="5221b-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="5221b-108">Diese Form der Exploit des auch bekannt als ein *einmalklick-Angriff* oder als *Sitzung riding*, da der Angriff nutzt des Benutzers zuvor den authentifizierten Sitzung.</span><span class="sxs-lookup"><span data-stu-id="5221b-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="5221b-109">Ein Beispiel von CSRF-Angriffen:</span><span class="sxs-lookup"><span data-stu-id="5221b-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="5221b-110">Ein Benutzer meldet sich bei `www.example.com`, mit Formularauthentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5221b-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="5221b-111">Der Server authentifiziert den Benutzer und gibt eine Antwort, die ein Authentifizierungscookie enthält.</span><span class="sxs-lookup"><span data-stu-id="5221b-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="5221b-112">Der Benutzer besucht eine bösartige Website.</span><span class="sxs-lookup"><span data-stu-id="5221b-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="5221b-113">Die bösartige Website enthält ein HTML-Formular, das etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5221b-113">The malicious site contains an HTML form similar to the following:</span></span>

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

<span data-ttu-id="5221b-114">Beachten Sie, dass die formaktion an den Standort anfällig für nicht auf die bösartige Website sendet.</span><span class="sxs-lookup"><span data-stu-id="5221b-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="5221b-115">Dies ist der "Cross-Site" Teil CSRF.</span><span class="sxs-lookup"><span data-stu-id="5221b-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="5221b-116">Der Benutzer klickt auf die Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="5221b-116">The user clicks the submit button.</span></span> <span data-ttu-id="5221b-117">Der Browser schließt automatisch das Authentifizierungscookie für die angeforderte Domäne (in diesem Fall die anfällig für Standort) mit der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5221b-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="5221b-118">Die Anforderung auf dem Server mit dem Kontext des Benutzers Authentifizierung ausgeführt und kann Ausführen aller Aktionen, die ein authentifizierter Benutzer tun darf.</span><span class="sxs-lookup"><span data-stu-id="5221b-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="5221b-119">In diesem Beispiel wird der Benutzer auf die Schaltfläche klicken muss.</span><span class="sxs-lookup"><span data-stu-id="5221b-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="5221b-120">Böswillige Seite konnte:</span><span class="sxs-lookup"><span data-stu-id="5221b-120">The malicious page could:</span></span>

* <span data-ttu-id="5221b-121">Führen Sie ein Skript, das automatisch auf das Formular sendet.</span><span class="sxs-lookup"><span data-stu-id="5221b-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="5221b-122">Sendet eine Formularübermittlung als eine AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5221b-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="5221b-123">Verwenden Sie ein ausgeblendetes Formular mit CSS ein.</span><span class="sxs-lookup"><span data-stu-id="5221b-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="5221b-124">Verwendung von SSL nicht zu verhindern, dass eine CSRF-Angriffen, kann die bösartige Website senden eine `https://` Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5221b-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="5221b-125">Einige Angriffe abzielen, Standort-Endpunkte, auf die reagiert `GET` Anforderungen, in dem Fall Bildtag zum Ausführen der Aktion (diese Form des Angriffs befindet sich häufig auf Forum-Standorte, die Bilder ermöglichen jedoch blockieren JavaScript) verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5221b-125">Some attacks target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="5221b-126">Anwendungen, die mit Statuswechsel `GET` Anforderungen vor böswilligen Angriffen anfällig sind.</span><span class="sxs-lookup"><span data-stu-id="5221b-126">Applications that change state with `GET` requests are vulnerable from malicious attacks.</span></span>

<span data-ttu-id="5221b-127">CSRF-Angriffen sind möglich für Websites, die Cookies für die Authentifizierung verwenden, da der Browser alle relevanten Cookies an die Ziel-Website senden.</span><span class="sxs-lookup"><span data-stu-id="5221b-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="5221b-128">CSRF-Angriffen sind jedoch nicht beschränkt auf Cookies ausnutzen.</span><span class="sxs-lookup"><span data-stu-id="5221b-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="5221b-129">Beispielsweise Basis-und Digestauthentifizierung sind auch anfällig.</span><span class="sxs-lookup"><span data-stu-id="5221b-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="5221b-130">Nachdem ein Benutzer mit Standard oder Digest-Authentifizierung anmeldet, sendet der Browser automatisch die Anmeldeinformationen an, bis die Sitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="5221b-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="5221b-131">Hinweis: In diesem Kontext *Sitzung* bezieht sich auf die clientseitige-Sitzung, die während der Authentifizierung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="5221b-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="5221b-132">Es ist unabhängig vom stagingstatus serverseitige Sitzungen oder [Sitzung Middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="5221b-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="5221b-133">Benutzer können CSRF Schwachstellen durch erraten:</span><span class="sxs-lookup"><span data-stu-id="5221b-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="5221b-134">Protokollierung von Websites, nach deren Verwendung.</span><span class="sxs-lookup"><span data-stu-id="5221b-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="5221b-135">Löschen Sie ihren Browser-Cookies in regelmäßigen Abständen.</span><span class="sxs-lookup"><span data-stu-id="5221b-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="5221b-136">Allerdings sind CSRF Sicherheitsrisiken im Grunde ein Problem mit der Web-app, nicht für den Endbenutzer.</span><span class="sxs-lookup"><span data-stu-id="5221b-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="5221b-137">Wie befasst Core ASP.NET-MVC CSRF?</span><span class="sxs-lookup"><span data-stu-id="5221b-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="5221b-138">ASP.NET Core implementiert anti-request-Fälschung mithilfe der [ASP.NET Core Data Protection Stack](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="5221b-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5221b-139">ASP.NET Core Datenschutz muss konfiguriert werden, um in einer Serverfarm zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5221b-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="5221b-140">Finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="5221b-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="5221b-141">ASP.NET Core anti-request-Fälschung Data Protection Standardkonfiguration</span><span class="sxs-lookup"><span data-stu-id="5221b-141">ASP.NET Core anti-request-forgery default data protection configuration</span></span> 

<span data-ttu-id="5221b-142">In ASP.NET Core MVC 2.0 die [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) fälschungssicherheitstoken für HTML-Formularelemente injiziert.</span><span class="sxs-lookup"><span data-stu-id="5221b-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="5221b-143">Beispielsweise wird das folgende Markup in einer Razor-Datei automatisch fälschungssicherheitstoken generiert:</span><span class="sxs-lookup"><span data-stu-id="5221b-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="5221b-144">Die automatische Generierung von fälschungssicherheitstoken für HTML-Formularelemente geschieht, wenn:</span><span class="sxs-lookup"><span data-stu-id="5221b-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="5221b-145">Die `form` -Tag enthält die `method="post"` Attribut AND</span><span class="sxs-lookup"><span data-stu-id="5221b-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="5221b-146">Das Action-Attribut ist leer.</span><span class="sxs-lookup"><span data-stu-id="5221b-146">The action attribute is empty.</span></span> <span data-ttu-id="5221b-147">( `action=""`) OR</span><span class="sxs-lookup"><span data-stu-id="5221b-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="5221b-148">Das Action-Attribut nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="5221b-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="5221b-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="5221b-149">(`<form method="post">`)</span></span>

<span data-ttu-id="5221b-150">Sie können die automatische Generierung von fälschungssicherheitstoken für HTML-Formularelemente durch deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="5221b-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="5221b-151">Explizit deaktiviert `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="5221b-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="5221b-152">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5221b-152">For example</span></span>

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="5221b-153">Entscheiden Sie außerhalb des gültigen Tag Hilfsprogramme Form-Elements, indem Sie unter Verwendung des Hilfsprogramms Tag [! Ausschlussverfahren Symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="5221b-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

  ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="5221b-154">Entfernen Sie die `FormTagHelper` aus der Sicht.</span><span class="sxs-lookup"><span data-stu-id="5221b-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="5221b-155">Entfernen Sie die `FormTagHelper` aus einer Sicht, indem Sie die folgende Anweisung an die Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="5221b-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="5221b-156">[Razor-Seiten](xref:mvc/razor-pages/index) vor XSRF/CSRF automatisch geschützt sind.</span><span class="sxs-lookup"><span data-stu-id="5221b-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="5221b-157">Sie müssen keinen zusätzlichen Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="5221b-157">You don't have to write any additional code.</span></span> <span data-ttu-id="5221b-158">Finden Sie unter [XSRF/CSRF und Razor-Seiten](xref:mvc/razor-pages/index#xsrf) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="5221b-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="5221b-159">Die am häufigsten verwendete Ansatz zum Schutz vor CSRF-Angriffen wird das token für die domänensynchronisierung-Muster (STP).</span><span class="sxs-lookup"><span data-stu-id="5221b-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="5221b-160">STP ist eine Technik, die verwendet werden, wenn der Benutzer eine Seite mit Daten anfordert.</span><span class="sxs-lookup"><span data-stu-id="5221b-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="5221b-161">Der Server sendet ein Token, das die Identität des aktuellen Benutzers an den Client zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="5221b-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="5221b-162">Der Client sendet das Token wieder an den Server für die Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="5221b-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="5221b-163">Wenn der Server ein Token, die Identität des authentifizierten Benutzers übereinstimmt empfängt, wird die Anforderung abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="5221b-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="5221b-164">Das Token ist eindeutig und unvorhersehbar.</span><span class="sxs-lookup"><span data-stu-id="5221b-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="5221b-165">Das Token kann auch verwendet werden, um sicherzustellen, dass ordnungsgemäße Sequenzierung einer Reihe von Anforderungen (sicherstellen, dass Seite 1 vorausgeht Seite 2 vor der Seite "3").</span><span class="sxs-lookup"><span data-stu-id="5221b-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="5221b-166">Alle Formulare in ASP.NET Core MVC-Vorlagen generiert antiforgery-Token.</span><span class="sxs-lookup"><span data-stu-id="5221b-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="5221b-167">Die folgenden zwei Beispiele für ansichtslogik antiforgery Token zu generieren:</span><span class="sxs-lookup"><span data-stu-id="5221b-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="5221b-168">Sie können eine antiforgery Token explizit hinzufügen eine `<form>` Element ohne das HTML-Hilfsobjekt Tag Hilfsprogramme mit `@Html.AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="5221b-168">You can explicitly add an antiforgery token to a `<form>` element without using tag helpers with the HTML helper `@Html.AntiForgeryToken`:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="5221b-169">In jedem der vorangegangenen Fälle werden ASP.NET Core ein ausgeblendetes Formularfeld ähnlich der folgenden hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="5221b-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

<span data-ttu-id="5221b-170">ASP.NET Core umfasst drei [Filter](xref:mvc/controllers/filters) für die Arbeit mit antiforgery Token: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, und `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="5221b-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, and `IgnoreAntiforgeryToken`.</span></span>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="5221b-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="5221b-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="5221b-172">Die `ValidateAntiForgeryToken` ist ein Aktionsfilter, die auf einer einzelnen Aktion, die einen Controller angewendet werden kann oder Global.</span><span class="sxs-lookup"><span data-stu-id="5221b-172">The `ValidateAntiForgeryToken` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="5221b-173">Anforderungen für Aktionen, die dieser Filter angewendet werden, wenn die Anforderung ein gültiges antiforgery Token enthält blockiert.</span><span class="sxs-lookup"><span data-stu-id="5221b-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
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

<span data-ttu-id="5221b-174">Die `ValidateAntiForgeryToken` Attribut erfordert ein Token für Anforderungen an Aktionsmethoden, die es ausstattet, einschließlich `HTTP GET` Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="5221b-174">The `ValidateAntiForgeryToken` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="5221b-175">Allgemein gelten, überschreiben Sie diese mit der `IgnoreAntiforgeryToken` Attribut.</span><span class="sxs-lookup"><span data-stu-id="5221b-175">If you apply it broadly, you can override it with the `IgnoreAntiforgeryToken` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="5221b-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5221b-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="5221b-177">ASP.NET Core-apps nicht in der Regel antiforgery Token für die sichere HTTP-Methoden (GET, HEAD, Optionen und TRACE) generieren.</span><span class="sxs-lookup"><span data-stu-id="5221b-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="5221b-178">Statt Allgemein anzuwenden der `ValidateAntiForgeryToken` -Attribut, und überschreiben diese mit `IgnoreAntiforgeryToken` Attribute, die Sie verwenden die ``AutoValidateAntiforgeryToken`` Attribut.</span><span class="sxs-lookup"><span data-stu-id="5221b-178">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="5221b-179">Dieses Attribut funktioniert genauso wie die `ValidateAntiForgeryToken` Attribut, außer dass es keine Token für Anforderungen, die mit den folgenden HTTP-Methoden erforderlich:</span><span class="sxs-lookup"><span data-stu-id="5221b-179">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="5221b-180">GET</span><span class="sxs-lookup"><span data-stu-id="5221b-180">GET</span></span>
* <span data-ttu-id="5221b-181">HEAD-,</span><span class="sxs-lookup"><span data-stu-id="5221b-181">HEAD</span></span>
* <span data-ttu-id="5221b-182">OPTIONEN</span><span class="sxs-lookup"><span data-stu-id="5221b-182">OPTIONS</span></span>
* <span data-ttu-id="5221b-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="5221b-183">TRACE</span></span>

<span data-ttu-id="5221b-184">Es wird empfohlen, `AutoValidateAntiforgeryToken` weitgefasst gilt dies für nicht-API-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="5221b-184">We recommend you use `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="5221b-185">Dadurch wird sichergestellt, dass Ihre Aktionen POST standardmäßig geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="5221b-186">Die Alternative besteht darin, antiforgery Token standardmäßig ignoriert, es sei denn, `ValidateAntiForgeryToken` auf die einzelnen Aktionsmethode angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="5221b-186">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to the individual action method.</span></span> <span data-ttu-id="5221b-187">Es ist wahrscheinlicher in diesem Szenario für eine POST-Aktionsmethode, werden Links nicht geschützt ist, lassen Ihre app CSRF-Angriffe anfällig.</span><span class="sxs-lookup"><span data-stu-id="5221b-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="5221b-188">Sogar anonyme Beiträge sollten antiforgery Token gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="5221b-189">Hinweis: Die APIs nicht automatischen Mechanismus zum Senden des nicht-Cookie-Teils des Tokens verfügen; Ihre Implementierung hängen Ihre Client-Code-Implementierung wahrscheinlich.</span><span class="sxs-lookup"><span data-stu-id="5221b-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="5221b-190">Einige Beispiele werden unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5221b-190">Some examples are shown below.</span></span>

<span data-ttu-id="5221b-191">Beispiel (auf Klassenebene):</span><span class="sxs-lookup"><span data-stu-id="5221b-191">Example (class level):</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="5221b-192">Beispiel (global):</span><span class="sxs-lookup"><span data-stu-id="5221b-192">Example (global):</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="5221b-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="5221b-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="5221b-194">Die `IgnoreAntiforgeryToken` Filter wird verwendet, um eine antiforgery Token für eine bestimmte Aktion (oder Domänencontroller) vorhanden ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5221b-194">The `IgnoreAntiforgeryToken` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="5221b-195">Beim Anwenden dieses Filters wird überschreiben `ValidateAntiForgeryToken` und/oder `AutoValidateAntiforgeryToken` Filter, die auf einer höheren Ebene angegeben ist (Global oder auf einem Domänencontroller).</span><span class="sxs-lookup"><span data-stu-id="5221b-195">When applied, this filter will override `ValidateAntiForgeryToken` and/or `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="5221b-196">JavaScript, AJAX- und SPAs</span><span class="sxs-lookup"><span data-stu-id="5221b-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="5221b-197">In herkömmlichen HTML-basierten Anwendungen werden die antiforgery Token an den Server mithilfe von ausgeblendeten Formularfeldern übergeben.</span><span class="sxs-lookup"><span data-stu-id="5221b-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="5221b-198">In modernen JavaScript-basierten apps und einseitige Anwendungen (SPAs) werden viele Anforderungen programmgesteuert vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="5221b-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="5221b-199">Diese AJAX-Anforderungen möglicherweise andere Techniken (z. B. Anforderungsheader oder Cookies) verwenden, um das Token zu senden.</span><span class="sxs-lookup"><span data-stu-id="5221b-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="5221b-200">Wenn Cookies zum Speichern von Authentifizierungstoken und zum Authentifizieren von API-Anforderungen auf dem Server verwendet werden, werden CSRF ein potenzielles Problem hin.</span><span class="sxs-lookup"><span data-stu-id="5221b-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="5221b-201">Jedoch wenn lokaler Speicher zum Speichern von Token verwendet wird, möglicherweise Sicherheitsrisiko FORGERY verringert werden, da die Werte aus dem lokalen Speicher nicht automatisch mit der jede neue Anforderung an den Server gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="5221b-202">Daher mit dem lokalen Speicher zum Speichern der antiforgery-Token auf dem Client und Senden des Tokens als ein Anforderungsheader und eine empfohlene Vorgehensweise ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="5221b-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="5221b-203">AngularJS</span></span>

<span data-ttu-id="5221b-204">AngularJS verwendet stattdessen eine Konvention CSRF-Adresse.</span><span class="sxs-lookup"><span data-stu-id="5221b-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="5221b-205">Wenn der Server ein Cookie mit dem Namen sendet `XSRF-TOKEN`, das Drehmoment `$http` Dienst wird der Wert hinzufügen dieses Cookie einen Header beim Senden einer Anforderungs auf diesem Server.</span><span class="sxs-lookup"><span data-stu-id="5221b-205">If the server sends a cookie with the name `XSRF-TOKEN`, the Angular `$http` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="5221b-206">Dieser Prozess erfolgt automatisch; Sie müssen nicht explizit der Header festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5221b-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="5221b-207">Der Headername wird `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="5221b-207">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="5221b-208">Der Server sollte dieser Header zu erkennen und seinen Inhalt zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5221b-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="5221b-209">Arbeiten Sie mit dieser Konvention für ASP.NET Haupt-API:</span><span class="sxs-lookup"><span data-stu-id="5221b-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="5221b-210">Konfigurieren Sie Ihre app so, geben Sie ein Token in einem Cookie wird aufgerufen `XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="5221b-210">Configure your app to provide a token in a cookie called `XSRF-TOKEN`</span></span>
* <span data-ttu-id="5221b-211">Konfigurieren Sie den antiforgery Dienst für einen Header mit dem Namen gesucht werden soll. `X-XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="5221b-211">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="5221b-212">[Viewer-Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="5221b-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="5221b-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5221b-213">JavaScript</span></span>

<span data-ttu-id="5221b-214">Verwenden von JavaScript mit Sichten, können Sie das Token mit einem Dienst in Ihrer Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="5221b-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="5221b-215">Zu diesem Zweck fügen Sie der `Microsoft.AspNetCore.Antiforgery.IAntiforgery` -Dienst in der Ansicht, und rufen `GetAndStoreTokens`, wie dargestellt:</span><span class="sxs-lookup"><span data-stu-id="5221b-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

<span data-ttu-id="5221b-216">Dadurch entfällt die Notwendigkeit so behandeln Sie direkt mit dem Festlegen von Cookies auf dem Server, oder sie vom Client gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="5221b-217">Im vorhergehende Beispiel verwendet jQuery, um den Wert des ausgeblendeten Felds für die AJAX-POST-Header zu lesen.</span><span class="sxs-lookup"><span data-stu-id="5221b-217">The preceding example uses jQuery to read the hidden field value for the AJAX POST header.</span></span> <span data-ttu-id="5221b-218">Um die JavaScript verwenden, um das Token Wert abzurufen, verwenden Sie `document.getElementById('RequestVerificationToken').value`.</span><span class="sxs-lookup"><span data-stu-id="5221b-218">To use JavaScript to obtain the token's value, use `document.getElementById('RequestVerificationToken').value`.</span></span>

<span data-ttu-id="5221b-219">JavaScript kann auch Zugriffstoken in Cookies bereitgestellt, und verwenden Sie das Cookie Inhalt zum Erstellen eines Headers mit dem Wert für das Token, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5221b-219">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="5221b-220">Klicken Sie dann zum Senden von Token in einer Kopfzeile namens fordert vorausgesetzt, Sie erstellen das Skript `X-CSRF-TOKEN`, konfigurieren Sie den antiforgery Dienst gesucht werden soll, für die `X-CSRF-TOKEN` Header:</span><span class="sxs-lookup"><span data-stu-id="5221b-220">Then, assuming you construct your script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="5221b-221">Im folgenden Beispiel wird jQuery, um eine AJAX-Anforderung mit den entsprechenden Header zu machen:</span><span class="sxs-lookup"><span data-stu-id="5221b-221">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

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

## <a name="configuring-antiforgery"></a><span data-ttu-id="5221b-222">Konfigurieren von Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5221b-222">Configuring Antiforgery</span></span>

<span data-ttu-id="5221b-223">`IAntiforgery` Stellt die API so konfigurieren Sie die antiforgery System bereit.</span><span class="sxs-lookup"><span data-stu-id="5221b-223">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="5221b-224">Es muss angefordert werden der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="5221b-224">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="5221b-225">Im folgenden Beispiel wird die Middleware auf der Startseite der app antiforgery-Token generieren und senden es in der Antwort als Cookie (mithilfe der Angular Dateinamenskonvention oben beschriebene):</span><span class="sxs-lookup"><span data-stu-id="5221b-225">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```csharp
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

### <a name="options"></a><span data-ttu-id="5221b-226">Optionen</span><span class="sxs-lookup"><span data-stu-id="5221b-226">Options</span></span>

<span data-ttu-id="5221b-227">Sie können anpassen, [antiforgery Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5221b-227">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```csharp
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

|<span data-ttu-id="5221b-228">Option</span><span class="sxs-lookup"><span data-stu-id="5221b-228">Option</span></span>        | <span data-ttu-id="5221b-229">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="5221b-229">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="5221b-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5221b-230">CookieDomain</span></span>  | <span data-ttu-id="5221b-231">Die Domäne des Cookies.</span><span class="sxs-lookup"><span data-stu-id="5221b-231">The domain of the cookie.</span></span> <span data-ttu-id="5221b-232">Wird standardmäßig auf `null` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5221b-232">Defaults to `null`.</span></span> |
|<span data-ttu-id="5221b-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="5221b-233">CookieName</span></span>    | <span data-ttu-id="5221b-234">Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="5221b-234">The name of the cookie.</span></span> <span data-ttu-id="5221b-235">Wenn nicht festgelegt ist, das System eine eindeutige Namen, die mit beginnt generiert, die `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="5221b-235">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="5221b-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5221b-236">CookiePath</span></span>    | <span data-ttu-id="5221b-237">Der Pfad für das Cookie festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5221b-237">The path set on the cookie.</span></span> |
|<span data-ttu-id="5221b-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="5221b-238">FormFieldName</span></span> | <span data-ttu-id="5221b-239">Der Name des Felds verborgene vom antiforgery System zum Rendern von antiforgery Token in Ansichten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="5221b-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="5221b-240">HeaderName</span><span class="sxs-lookup"><span data-stu-id="5221b-240">HeaderName</span></span>    | <span data-ttu-id="5221b-241">Der Name des Headers vom antiforgery System verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="5221b-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="5221b-242">Wenn `null`, das System berücksichtigt nur Formulardaten.</span><span class="sxs-lookup"><span data-stu-id="5221b-242">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="5221b-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="5221b-243">RequireSsl</span></span>    | <span data-ttu-id="5221b-244">Gibt an, ob SSL vom antiforgery System erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-244">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="5221b-245">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5221b-245">Defaults to `false`.</span></span> <span data-ttu-id="5221b-246">Wenn `true`, nicht-SSL-Anforderungen schlagen fehl.</span><span class="sxs-lookup"><span data-stu-id="5221b-246">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="5221b-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="5221b-247">SuppressXFrameOptionsHeader</span></span> | <span data-ttu-id="5221b-248">Gibt an, ob die Generierung von Unterdrücken der `X-Frame-Options` Header.</span><span class="sxs-lookup"><span data-stu-id="5221b-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="5221b-249">Standardmäßig wird der Header mit einem Wert von "SAMEORIGIN" generiert.</span><span class="sxs-lookup"><span data-stu-id="5221b-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="5221b-250">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5221b-250">Defaults to `false`.</span></span> |

<span data-ttu-id="5221b-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span><span class="sxs-lookup"><span data-stu-id="5221b-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="5221b-252">Erweitern von Antiforgery</span><span class="sxs-lookup"><span data-stu-id="5221b-252">Extending Antiforgery</span></span>

<span data-ttu-id="5221b-253">Die [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) Typ ermöglicht Entwicklern das Verhalten des Anti-XSRF-Systems durch zusätzliche Round-Tripping-Daten in einzelnen Token zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="5221b-253">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="5221b-254">Die [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) Methode wird jedes Mal aufgerufen, wenn ein Token wird generiert, und der Rückgabewert in das generierte Token eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-254">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="5221b-255">Eine Implementierung konnte einen Zeitstempel, eine Nonce oder ein anderer Wert zurückgeben, und rufen dann [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) dieser Daten validieren, wenn das Token überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="5221b-255">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="5221b-256">Der Client Benutzernamen ist bereits in die generierten Token eingebettet, daher keine Notwendigkeit besteht, diese Informationen umfassen.</span><span class="sxs-lookup"><span data-stu-id="5221b-256">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="5221b-257">Wenn ein Token ergänzenden Daten, aber keine enthält `IAntiForgeryAdditionalDataProvider` wurde konfiguriert, die zusätzlichen Daten wird nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="5221b-257">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="5221b-258">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="5221b-258">Fundamentals</span></span>

<span data-ttu-id="5221b-259">CSRF-Angriffen basieren auf dem Standardverhalten der Browser Cookies, die mit einer Domäne verknüpft sind, mit jeder Anforderung an, dass diese Domäne zu senden.</span><span class="sxs-lookup"><span data-stu-id="5221b-259">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="5221b-260">Diese Cookies werden innerhalb des Browsers gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5221b-260">These cookies are stored within the browser.</span></span> <span data-ttu-id="5221b-261">Sie enthalten häufig Sitzungscookies für authentifizierte Benutzer.</span><span class="sxs-lookup"><span data-stu-id="5221b-261">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="5221b-262">Cookie-basierte Authentifizierung ist eine gängige Form der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5221b-262">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="5221b-263">Token-basierte Authentifizierungssysteme haben in Beliebtheit, vor allem für SPAs und andere Szenarien "smart Client" zugenommen.</span><span class="sxs-lookup"><span data-stu-id="5221b-263">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="5221b-264">Cookie-basierte Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5221b-264">Cookie-based authentication</span></span>

<span data-ttu-id="5221b-265">Sobald ein Benutzer mithilfe ihres Benutzernamens und Kennworts authentifiziert wurde, können sie ein Token ausgestellt hat, die verwendet werden kann, um sie zu identifizieren und überprüfen, dass sie authentifiziert wurden.</span><span class="sxs-lookup"><span data-stu-id="5221b-265">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="5221b-266">Das Token wird als ein Cookie, die mit jeder Anforderung der Client stellt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5221b-266">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="5221b-267">Generieren und überprüfen dieses Cookie wird von der cookieauthentifizierungsmiddleware ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5221b-267">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="5221b-268">ASP.NET Core bietet Cookie [Middleware](xref:fundamentals/middleware/index) der serialisiert eines Benutzerprinzipals in einem verschlüsselten Cookie, und klicken Sie dann bei nachfolgenden Anforderungen überprüft das Cookie wird den Prinzipal erstellt, und weist sie der `User` Eigenschaft `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5221b-268">ASP.NET Core provides cookie [middleware](xref:fundamentals/middleware/index) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="5221b-269">Wenn ein Cookie verwendet wird, ist das Authentifizierungscookie lediglich als Container für das Formularauthentifizierungsticket.</span><span class="sxs-lookup"><span data-stu-id="5221b-269">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="5221b-270">Das Ticket wird als Wert des Formularauthentifizierungscookies mit jeder Anforderung übergeben und dient Formularauthentifizierung auf dem Server beim Identifizieren eines authentifiziertes Benutzers verwendet.</span><span class="sxs-lookup"><span data-stu-id="5221b-270">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="5221b-271">Wenn ein Benutzer mit einem System angemeldet ist, wird eine Sitzung des Benutzers wird auf der Serverseite erstellt und in einer Datenbank oder einem anderen permanenten Speicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-271">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="5221b-272">Das System generiert einen Sitzungsschlüssel, der auf die aktuelle Sitzung im Datenspeicher verweist, und sie als Client Side Cookie gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5221b-272">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="5221b-273">Prüft der Webserver diesen Sitzungsschlüssel jedes Mal ein Benutzer eine Ressource anfordert, die Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-273">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="5221b-274">Das System überprüft, ob die Sitzung zugeordneten Benutzer über die Berechtigung zum Zugriff auf die angeforderte Ressource verfügt.</span><span class="sxs-lookup"><span data-stu-id="5221b-274">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="5221b-275">Wenn dies der Fall ist, wird die Anforderung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="5221b-275">If so, the request continues.</span></span> <span data-ttu-id="5221b-276">Andernfalls gibt die Anforderung als nicht autorisierte zurück.</span><span class="sxs-lookup"><span data-stu-id="5221b-276">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="5221b-277">Bei dieser Vorgehensweise Cookies werden verwendet, um die Anwendung angezeigt werden, um statusbehafteten werden, da er "merken" kann der Benutzer wurde zuvor authentifiziert mit dem Server.</span><span class="sxs-lookup"><span data-stu-id="5221b-277">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="5221b-278">Benutzertoken</span><span class="sxs-lookup"><span data-stu-id="5221b-278">User tokens</span></span>

<span data-ttu-id="5221b-279">Token-basierte Authentifizierung speichern keine Sitzung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="5221b-279">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="5221b-280">Wenn ein Benutzer angemeldet ist, sind sie ein Token (nicht antiforgery Token) ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="5221b-280">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="5221b-281">Dieses Token enthält die Daten, die erforderlich ist, um das Token zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5221b-281">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="5221b-282">Es enthält auch Benutzerinformationen in Form von [Ansprüche](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="5221b-282">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="5221b-283">Wenn ein Benutzer möchte den Zugriff auf eine Serverressource, die eine Authentifizierung erforderlich ist, wird das Token mit einer zusätzlichen Authorization-Header in Form von Träger {Token} an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="5221b-283">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="5221b-284">Dadurch wird die Anwendung zustandslose, da das Token in jede nachfolgende Anforderung für die serverseitige Validierung in der Anforderung übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="5221b-284">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="5221b-285">Dieses Token wird nicht *verschlüsselte*; es *codiert*.</span><span class="sxs-lookup"><span data-stu-id="5221b-285">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="5221b-286">Auf der Serverseite kann das Token für den Zugriff auf die unformatierten Informationen innerhalb der Token decodiert werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-286">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="5221b-287">Um das Token in nachfolgenden Anforderungen zu senden, muss entweder dieses im lokalen Speicher für den Browser oder in einem Cookie.</span><span class="sxs-lookup"><span data-stu-id="5221b-287">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="5221b-288">Keine Gedanken Sie über XSRF Sicherheitsrisiko, wenn das Token im lokalen Speicher gespeichert ist, aber es ist ein Problem auf, wenn das Token in einem Cookie gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-288">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="5221b-289">Mehrere Anwendungen werden in einer Domäne gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="5221b-289">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="5221b-290">Obwohl `example1.cloudapp.net` und `example2.cloudapp.net` sind verschiedene Hosts besteht ein implizites Vertrauensverhältnis zwischen Hosts unter der `*.cloudapp.net` Domäne.</span><span class="sxs-lookup"><span data-stu-id="5221b-290">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="5221b-291">Diese implizite Vertrauensstellung kann potenziell nicht vertrauenswürdige Hosts Cookies gegenseitig beeinträchtigen (die gleichen-Origin-Richtlinien, die zum Steuern der AJAX-Anforderungen sind nicht unbedingt auf HTTP-Cookies anwendbar).</span><span class="sxs-lookup"><span data-stu-id="5221b-291">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="5221b-292">Die ASP.NET Core-Laufzeit bietet einige Entschärfung, insofern, dass der Benutzername in das Feldtoken eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="5221b-292">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="5221b-293">Auch wenn eine böswillige Unterdomäne einen Sitzungstoken überschrieben werden kann, kann nicht er ein gültiger Token für den Benutzer generieren.</span><span class="sxs-lookup"><span data-stu-id="5221b-293">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="5221b-294">Wenn in einer derartigen Umgebung gehostet wird, können nicht die integrierte Anti-XSRF-Routinen weiterhin Sitzungsübernahme oder Anmeldung CSRF Verteidigung gegen Angriffe.</span><span class="sxs-lookup"><span data-stu-id="5221b-294">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="5221b-295">Freigegebene Hostingumgebungen sind Vunerable Sitzungsübernahme CSRF-Anmeldung und andere Angriffe.</span><span class="sxs-lookup"><span data-stu-id="5221b-295">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="5221b-296">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5221b-296">Additional resources</span></span>

* <span data-ttu-id="5221b-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) auf [öffnen Webanwendungsprojekt Sicherheit](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="5221b-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
