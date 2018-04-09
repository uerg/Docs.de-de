---
title: Open Umleitung Angriffe in ASP.NET Core
author: ardalis
description: Zeigt, wie open umleitungs-Angriffe gegen eine ASP.NET Core-app zu verhindern
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: 4a210b8bb8091e7c036d4bc98306e3b3f90d7d46
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="e50dd-103">Open Umleitung Angriffe in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e50dd-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="e50dd-104">Eine Web-app, die an eine URL umleitet, die über die Anforderung, z. B. die Abfragezeichenfolge oder Formular Daten angegeben wird kann potenziell mit manipuliert werden, um Benutzer an eine externe, böswillige URL weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="e50dd-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="e50dd-105">Diese Manipulation wird einen öffnen-Redirect-Angriff bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="e50dd-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="e50dd-106">Wenn Sie die Anwendungslogik zu einer angegebenen URL umleitet, müssen Sie sicherstellen, dass die umleitungs-URL nicht manipuliert wurde.</span><span class="sxs-lookup"><span data-stu-id="e50dd-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="e50dd-107">ASP.NET Core verfügt über integrierte Funktionalität zu apps öffnen Umleitung (auch bekannt als open-Umleitung) Angriffen zu schützen.</span><span class="sxs-lookup"><span data-stu-id="e50dd-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="e50dd-108">Was ist ein Angriff öffnen Umleitung?</span><span class="sxs-lookup"><span data-stu-id="e50dd-108">What is an open redirect attack?</span></span>

<span data-ttu-id="e50dd-109">Webanwendungen Umleiten von Benutzern häufig zu einer Anmeldeseite beim Zugriff auf Ressourcen, die eine Authentifizierung erfordern.</span><span class="sxs-lookup"><span data-stu-id="e50dd-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="e50dd-110">Die Umleitung Typlically umfasst eine `returnUrl` Querystring-Parameter, damit der Benutzer an die ursprünglich angeforderte URL zurückgegeben werden kann, nachdem sie erfolgreich angemeldet haben.</span><span class="sxs-lookup"><span data-stu-id="e50dd-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="e50dd-111">Nachdem der Benutzer authentifiziert hat, sind sie mit der URL umgeleitet, die sie ursprünglich angefordert hatten.</span><span class="sxs-lookup"><span data-stu-id="e50dd-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="e50dd-112">Da der Ziel-URL in die Abfragezeichenfolge der Anforderung angegeben ist, kann ein böswilliger Benutzer Querystring manipulieren.</span><span class="sxs-lookup"><span data-stu-id="e50dd-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="e50dd-113">Ein manipuliertes Querystring kann die Website zum Umleiten des Benutzers auf eine externe, schädlichen Website ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e50dd-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="e50dd-114">Diese Technik ist einen open Umleitung (oder der Umleitung)-Angriff bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="e50dd-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="e50dd-115">Ein Beispiel-Angriff</span><span class="sxs-lookup"><span data-stu-id="e50dd-115">An example attack</span></span>

<span data-ttu-id="e50dd-116">Ein böswilliger Benutzer könnte einen Angriff den böswilliger Benutzerzugriff auf eines Benutzers Anmeldeinformationen oder vertrauliche Informationen in Ihrer app zu ermöglichen entwickeln.</span><span class="sxs-lookup"><span data-stu-id="e50dd-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="e50dd-117">Um das Risiko eines Angriffs zu beginnen, ermöglichen sie den Benutzer einen Link zur Anmeldeseite für Ihre Website, und klicken Sie auf eine `returnUrl` Querystring-Wert zur URL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e50dd-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="e50dd-118">Z. B. die [NerdDinner.com](http://nerddinner.com) beispielanwendung (für ASP.NET MVC geschrieben) schließt solche eine Anmeldeseite hier: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="e50dd-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="e50dd-119">Der Angriff führt dann folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="e50dd-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="e50dd-120">Benutzer klickt auf einen Link zur ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Beachten Sie, dass zweiten URL ist Nerddi**n**mputerkonto, nicht Nerddi**Nn**mputerkonto).</span><span class="sxs-lookup"><span data-stu-id="e50dd-120">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="e50dd-121">Der Benutzer anmeldet erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="e50dd-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="e50dd-122">Der Benutzer umgeleitet wird (durch den Standort) ``http://nerddiner.com/Account/LogOn`` (bösartige Website, die echte Website aussieht).</span><span class="sxs-lookup"><span data-stu-id="e50dd-122">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="e50dd-123">Der Benutzer wieder anmeldet (Vergabe böswillige Standort ihrer Anmeldeinformationen) und wieder an die wirkliche Website umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="e50dd-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="e50dd-124">Der Benutzer wird wahrscheinlich glauben, dass ihre ersten Versuch, melden Sie sich Fehler, und die zweiten Ausdruck erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="e50dd-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="e50dd-125">Sehr wahrscheinlich werden Sie unabhängig von deren bleiben ihre Anmeldeinformationen beschädigt wurden.</span><span class="sxs-lookup"><span data-stu-id="e50dd-125">They will most likely remain unaware their credentials have been compromised.</span></span>

![Open-Angriff-Weiterleitung](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="e50dd-127">Geben Sie zusätzlich zu den Anmeldungsseiten einige Websites Umleitung Seiten oder Endpunkte.</span><span class="sxs-lookup"><span data-stu-id="e50dd-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="e50dd-128">Stellen Sie sich vor Ihrer app hat eine Seite mit einer geöffneten Umleitung ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="e50dd-128">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="e50dd-129">Ein Angreifer könnte erstellen, z. B. einen Link in einer e-Mail, die auf geht ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="e50dd-129">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="e50dd-130">Ein normaler Benutzer wird an der URL suchen und feststellen, dass er mit dem Standortnamen Ihres beginnt.</span><span class="sxs-lookup"><span data-stu-id="e50dd-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="e50dd-131">Sie werden als vertrauenswürdig festlegen, die, auf den Link klicken.</span><span class="sxs-lookup"><span data-stu-id="e50dd-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="e50dd-132">Öffnen umleiten den Benutzer klicken Sie dann auf die Phishingwebsite identisch mit Ihren Instanzen-aussieht, senden und der Benutzer wahrscheinlich würden, was sie meinen Anmeldung ist Ihr Standort.</span><span class="sxs-lookup"><span data-stu-id="e50dd-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="e50dd-133">Schutz vor Angriffen öffnen umleiten</span><span class="sxs-lookup"><span data-stu-id="e50dd-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="e50dd-134">Wenn Sie Webanwendungen zu entwickeln, behandeln Sie alle Benutzer bereitgestellten Daten als nicht vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="e50dd-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="e50dd-135">Wenn die Anwendung Funktionen, die den Benutzer basierend auf den Inhalt der URL umleitet verfügt, stellen Sie sicher, dass solche leitet nur lokal ausgeführt werden, innerhalb Ihrer app (oder eine bekannte URL, aber keine URL, die in der Abfragezeichenfolge angegeben werden kann).</span><span class="sxs-lookup"><span data-stu-id="e50dd-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="e50dd-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="e50dd-136">LocalRedirect</span></span>

<span data-ttu-id="e50dd-137">Verwenden der ``LocalRedirect`` Hilfsmethode, die von der Basisklasse `Controller` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e50dd-137">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="e50dd-138">``LocalRedirect`` Wenn eine nicht lokale-URL angegeben ist, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e50dd-138">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="e50dd-139">Andernfalls verhält sich ebenso wie die ``Redirect`` Methode.</span><span class="sxs-lookup"><span data-stu-id="e50dd-139">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="e50dd-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="e50dd-140">IsLocalUrl</span></span>

<span data-ttu-id="e50dd-141">Verwenden der [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) Methode zum Testen von URLs vor der Umleitung:</span><span class="sxs-lookup"><span data-stu-id="e50dd-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="e50dd-142">Das folgende Beispiel zeigt, wie zu überprüfen, ob vor der Umleitung für eine URL lokal ist.</span><span class="sxs-lookup"><span data-stu-id="e50dd-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="e50dd-143">Die `IsLocalUrl` Methode verhindert, dass Benutzer versehentlich an eine bösartige Website umgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="e50dd-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="e50dd-144">Sie können die Details der URL protokollieren, die bereitgestellt wurde, wird eine nicht lokale-URL in einer Situation bereitgestellt, in dem Sie eine lokale URL erwartet.</span><span class="sxs-lookup"><span data-stu-id="e50dd-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="e50dd-145">Protokollierung Umleitung können URLs bei der Diagnose von Umleitung Angriffe.</span><span class="sxs-lookup"><span data-stu-id="e50dd-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
