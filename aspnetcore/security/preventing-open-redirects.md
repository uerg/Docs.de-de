---
title: Verhindern von Angriffen von Open Umleitung in einer ASP.NET Core-app | Microsoft Docs
author: ardalis
description: Zeigt, wie open umleitungs-Angriffe gegen eine ASP.NET Core-app zu verhindern
keywords: ASP.NET Core, Sicherheit, Open Umleitung Angriff
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: f5cf472d49c2475e4d57654efd5fc0a4ccecba4c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="d6bde-104">Verhindern von Angriffen von Open Umleitung in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="d6bde-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="d6bde-105">Eine Web-app, die an eine URL, die über die Anforderung angegeben wird umleitet, wie z. B. die Abfragezeichenfolge oder Formular Daten möglicherweise manipuliert werden können Benutzer an eine externe, böswillige URL umleiten.</span><span class="sxs-lookup"><span data-stu-id="d6bde-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="d6bde-106">Diese Manipulation wird einen öffnen-Redirect-Angriff bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="d6bde-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="d6bde-107">Wenn Sie die Anwendungslogik zu einer angegebenen URL umleitet, müssen Sie sicherstellen, dass die umleitungs-URL nicht manipuliert wurde.</span><span class="sxs-lookup"><span data-stu-id="d6bde-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="d6bde-108">ASP.NET Core verfügt über integrierte Funktionalität zu apps öffnen Umleitung (auch bekannt als open-Umleitung) Angriffen zu schützen.</span><span class="sxs-lookup"><span data-stu-id="d6bde-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="d6bde-109">Was ist ein Angriff öffnen Umleitung?</span><span class="sxs-lookup"><span data-stu-id="d6bde-109">What is an open redirect attack?</span></span>

<span data-ttu-id="d6bde-110">Webanwendungen Umleiten von Benutzern häufig zu einer Anmeldeseite beim Zugriff auf Ressourcen, die eine Authentifizierung erfordern.</span><span class="sxs-lookup"><span data-stu-id="d6bde-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="d6bde-111">Die Umleitung Typlically umfasst eine `returnUrl` Querystring-Parameter, damit der Benutzer an die ursprünglich angeforderte URL zurückgegeben werden kann, nachdem sie erfolgreich angemeldet haben.</span><span class="sxs-lookup"><span data-stu-id="d6bde-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="d6bde-112">Nachdem der Benutzer authentifiziert hat, wird er an die URL umgeleitet, die sie ursprünglich angefordert hatten.</span><span class="sxs-lookup"><span data-stu-id="d6bde-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="d6bde-113">Da der Ziel-URL in die Abfragezeichenfolge der Anforderung angegeben ist, kann ein böswilliger Benutzer Querystring manipulieren.</span><span class="sxs-lookup"><span data-stu-id="d6bde-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="d6bde-114">Ein manipuliertes Querystring kann die Website zum Umleiten des Benutzers auf eine externe, schädlichen Website ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="d6bde-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="d6bde-115">Diese Technik ist einen open Umleitung (oder der Umleitung)-Angriff bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="d6bde-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="d6bde-116">Ein Beispiel-Angriff</span><span class="sxs-lookup"><span data-stu-id="d6bde-116">An example attack</span></span>

<span data-ttu-id="d6bde-117">Ein böswilliger Benutzer könnte einen Angriff den böswilliger Benutzerzugriff auf eines Benutzers Anmeldeinformationen oder vertrauliche Informationen in Ihrer app zu ermöglichen entwickeln.</span><span class="sxs-lookup"><span data-stu-id="d6bde-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="d6bde-118">Um das Risiko eines Angriffs zu beginnen, ermöglichen sie den Benutzer einen Link zur Anmeldeseite für Ihre Website, und klicken Sie auf eine `returnUrl` Querystring-Wert zur URL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d6bde-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="d6bde-119">Z. B. die [NerdDinner.com](http://nerddinner.com) beispielanwendung (für ASP.NET MVC geschrieben) schließt solche eine Anmeldeseite hier: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="d6bde-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="d6bde-120">Der Angriff führt dann folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="d6bde-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="d6bde-121">Benutzer klickt auf einen Link zur ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Beachten Sie, dass zweiten URL ist Nerddi**n**mputerkonto, nicht Nerddi**nn**mputerkonto).</span><span class="sxs-lookup"><span data-stu-id="d6bde-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="d6bde-122">Der Benutzer anmeldet erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="d6bde-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="d6bde-123">Der Benutzer umgeleitet wird (durch den Standort) ``http://nerddiner.com/Account/LogOn`` (bösartige Website, die echte Website aussieht).</span><span class="sxs-lookup"><span data-stu-id="d6bde-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="d6bde-124">Der Benutzer wieder anmeldet (Vergabe böswillige Standort ihrer Anmeldeinformationen) und wieder an die wirkliche Website umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="d6bde-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="d6bde-125">Der Benutzer wird wahrscheinlich glauben, dass ihre ersten Versuch, melden Sie sich Fehler, und die zweiten Ausdruck erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="d6bde-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="d6bde-126">Wahrscheinlich müssen Sie unabhängig von deren bleiben ihre Anmeldeinformationen beschädigt wurden.</span><span class="sxs-lookup"><span data-stu-id="d6bde-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Open-Angriff-Weiterleitung](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="d6bde-128">Geben Sie zusätzlich zu den Anmeldungsseiten einige Websites Umleitung Seiten oder Endpunkte.</span><span class="sxs-lookup"><span data-stu-id="d6bde-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="d6bde-129">Stellen Sie sich vor Ihrer app hat eine Seite mit einer geöffneten Umleitung ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="d6bde-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="d6bde-130">Ein Angreifer könnte erstellen, z. B. einen Link in einer e-Mail, die auf geht ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="d6bde-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="d6bde-131">Ein normaler Benutzer wird an der URL suchen und feststellen, dass er mit dem Standortnamen Ihres beginnt.</span><span class="sxs-lookup"><span data-stu-id="d6bde-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="d6bde-132">Sie werden als vertrauenswürdig festlegen, die, auf den Link klicken.</span><span class="sxs-lookup"><span data-stu-id="d6bde-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="d6bde-133">Öffnen umleiten den Benutzer klicken Sie dann auf die Phishingwebsite identisch mit Ihren Instanzen-aussieht, senden und der Benutzer wahrscheinlich würden, was sie meinen Anmeldung ist Ihr Standort.</span><span class="sxs-lookup"><span data-stu-id="d6bde-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="d6bde-134">Schutz vor Angriffen öffnen umleiten</span><span class="sxs-lookup"><span data-stu-id="d6bde-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="d6bde-135">Wenn Sie Webanwendungen zu entwickeln, behandeln Sie alle Benutzer bereitgestellten Daten als nicht vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="d6bde-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="d6bde-136">Wenn die Anwendung Funktionen, die den Benutzer basierend auf den Inhalt der URL umleitet verfügt, stellen Sie sicher, dass solche leitet nur lokal ausgeführt werden, innerhalb Ihrer app (oder eine bekannte URL, aber keine URL, die in der Abfragezeichenfolge angegeben werden kann).</span><span class="sxs-lookup"><span data-stu-id="d6bde-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="d6bde-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="d6bde-137">LocalRedirect</span></span>

<span data-ttu-id="d6bde-138">Verwenden der ``LocalRedirect`` Hilfsmethode, die von der Basisklasse `Controller` Klasse:</span><span class="sxs-lookup"><span data-stu-id="d6bde-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="d6bde-139">``LocalRedirect``Wenn eine nicht lokale-URL angegeben ist, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="d6bde-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="d6bde-140">Andernfalls verhält sich ebenso wie die ``Redirect`` Methode.</span><span class="sxs-lookup"><span data-stu-id="d6bde-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="d6bde-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="d6bde-141">IsLocalUrl</span></span>

<span data-ttu-id="d6bde-142">Verwenden der [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) Methode zum Testen von URLs vor der Umleitung:</span><span class="sxs-lookup"><span data-stu-id="d6bde-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="d6bde-143">Das folgende Beispiel zeigt, wie zu überprüfen, ob vor der Umleitung für eine URL lokal ist.</span><span class="sxs-lookup"><span data-stu-id="d6bde-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="d6bde-144">Die `IsLocalUrl` Methode verhindert, dass Benutzer versehentlich an eine bösartige Website umgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="d6bde-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="d6bde-145">Sie können die Details der URL protokollieren, die bereitgestellt wurde, wird eine nicht lokale-URL in einer Situation bereitgestellt, in dem Sie eine lokale URL erwartet.</span><span class="sxs-lookup"><span data-stu-id="d6bde-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="d6bde-146">Protokollierung Umleitung können URLs bei der Diagnose von Umleitung Angriffe.</span><span class="sxs-lookup"><span data-stu-id="d6bde-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
