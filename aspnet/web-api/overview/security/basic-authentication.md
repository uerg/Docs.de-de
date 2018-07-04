---
uid: web-api/overview/security/basic-authentication
title: Standardauthentifizierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Standardauthentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9d5610eb61088a8e7573ba6399c771f0957ff437
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395330"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="b04ae-103">Standardauthentifizierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="b04ae-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b04ae-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b04ae-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b04ae-105">Die Standardauthentifizierung ist in definiert [RFC 2617, HTTP-Authentifizierung: Standard- und Hashwertauthentifizierung für den Zugriff](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="b04ae-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="b04ae-106">Nachteile</span><span class="sxs-lookup"><span data-stu-id="b04ae-106">Disadvantages</span></span>

- <span data-ttu-id="b04ae-107">In der Anforderung werden die Anmeldeinformationen des Benutzers gesendet.</span><span class="sxs-lookup"><span data-stu-id="b04ae-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="b04ae-108">Anmeldeinformationen werden als Klartext gesendet.</span><span class="sxs-lookup"><span data-stu-id="b04ae-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="b04ae-109">Anmeldeinformationen werden bei jeder Anforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="b04ae-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="b04ae-110">So melden Sie sich, mit Ausnahme der durch die Browsersitzung beenden.</span><span class="sxs-lookup"><span data-stu-id="b04ae-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="b04ae-111">Anfällig für websiteübergreifende anforderungsfälschung (CSRF); erfordert, dass Anti-CSRF-Measures.</span><span class="sxs-lookup"><span data-stu-id="b04ae-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="b04ae-112">Vorteile</span><span class="sxs-lookup"><span data-stu-id="b04ae-112">Advantages</span></span>

- <span data-ttu-id="b04ae-113">Internet-Standard.</span><span class="sxs-lookup"><span data-stu-id="b04ae-113">Internet standard.</span></span>
- <span data-ttu-id="b04ae-114">Von allen wichtigen Browsern unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b04ae-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="b04ae-115">Relativ einfaches Protokoll.</span><span class="sxs-lookup"><span data-stu-id="b04ae-115">Relatively simple protocol.</span></span>

<span data-ttu-id="b04ae-116">Die Standardauthentifizierung funktioniert wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="b04ae-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="b04ae-117">Wenn eine Anforderung eine Authentifizierung erfordert, gibt der Server 401 (nicht autorisiert) zurück.</span><span class="sxs-lookup"><span data-stu-id="b04ae-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="b04ae-118">Die Antwort enthält einen WWW-Authenticate-Header, der angibt, dass der Server die Standardauthentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b04ae-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="b04ae-119">Der Client sendet eine weitere Anforderung, mit die Anmeldeinformationen des Clients in der Authorization-Header.</span><span class="sxs-lookup"><span data-stu-id="b04ae-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="b04ae-120">Die Anmeldeinformationen werden als "Name: Password" als base64-codierte Zeichenfolge formatiert.</span><span class="sxs-lookup"><span data-stu-id="b04ae-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="b04ae-121">Die Anmeldeinformationen werden nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="b04ae-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="b04ae-122">Standardauthentifizierung erfolgt im Rahmen einer "Realm".</span><span class="sxs-lookup"><span data-stu-id="b04ae-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="b04ae-123">Der Server enthält den Namen des Bereichs im WWW-Authenticate-Header.</span><span class="sxs-lookup"><span data-stu-id="b04ae-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="b04ae-124">Die Anmeldeinformationen des Benutzers sind innerhalb dieses Bereichs gültig.</span><span class="sxs-lookup"><span data-stu-id="b04ae-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="b04ae-125">Der genauen Geltungsbereich ein Bereich wird durch den Server definiert.</span><span class="sxs-lookup"><span data-stu-id="b04ae-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="b04ae-126">Beispielsweise können Sie mehrere Bereiche in der Reihenfolge nach Partition Ressourcen definieren.</span><span class="sxs-lookup"><span data-stu-id="b04ae-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="b04ae-127">Da die Anmeldeinformationen unverschlüsselt gesendet werden, ist die Standardauthentifizierung nur sichere über HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b04ae-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="b04ae-128">Finden Sie unter [arbeiten mit SSL in Web-API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b04ae-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="b04ae-129">Standardauthentifizierung ist auch anfällig für CSRF-Angriffe.</span><span class="sxs-lookup"><span data-stu-id="b04ae-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="b04ae-130">Nachdem der Benutzer Anmeldeinformationen eingegeben hat, sendet der Browser automatisch diese bei nachfolgenden Anforderungen der gleichen Domäne an, für die Dauer der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="b04ae-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="b04ae-131">Dies schließt die AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="b04ae-131">This includes AJAX requests.</span></span> <span data-ttu-id="b04ae-132">Finden Sie unter [verhindern von Angriffen Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="b04ae-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="b04ae-133">Standardauthentifizierung mit IIS</span><span class="sxs-lookup"><span data-stu-id="b04ae-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="b04ae-134">IIS unterstützt die Standardauthentifizierung, es gibt jedoch ein Nachteil: der Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="b04ae-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="b04ae-135">Das bedeutet, dass der Benutzer ein Konto für die Domäne des Servers verfügen muss.</span><span class="sxs-lookup"><span data-stu-id="b04ae-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="b04ae-136">Für eine öffentliche Website möchten Sie in der Regel ein ASP.NET-Mitgliedschaftsanbieter authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="b04ae-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="b04ae-137">Um grundlegende Authentifizierung mit IIS zu aktivieren, legen Sie den Authentifizierungsmodus auf "Windows" in "Web.config" von Ihrem ASP.NET-Projekt aus:</span><span class="sxs-lookup"><span data-stu-id="b04ae-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="b04ae-138">In diesem Modus verwendet IIS Windows-Anmeldeinformationen zum Authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="b04ae-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="b04ae-139">Darüber hinaus müssen Sie die grundlegenden Authentifizierung in IIS aktivieren.</span><span class="sxs-lookup"><span data-stu-id="b04ae-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="b04ae-140">Im IIS-Manager, wechseln Sie zur Ansicht "Features", wählen Sie die Authentifizierung und aktivieren Sie der Standardauthentifizierung.</span><span class="sxs-lookup"><span data-stu-id="b04ae-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="b04ae-141">Fügen Sie in Ihrem Web-API-Projekt, das `[Authorize]` -Attribut für alle Controlleraktionen, die eine Authentifizierung benötigen.</span><span class="sxs-lookup"><span data-stu-id="b04ae-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="b04ae-142">Ein Client authentifiziert sich durch Festlegen des Authorization-Headers in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="b04ae-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="b04ae-143">Browser-Clients führen Sie diesen Schritt automatisch.</span><span class="sxs-lookup"><span data-stu-id="b04ae-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="b04ae-144">Nichtsuchdienst Clients müssen die Header festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b04ae-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="b04ae-145">Standardauthentifizierung mit benutzerdefinierten Mitgliedschaftsanbieter</span><span class="sxs-lookup"><span data-stu-id="b04ae-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="b04ae-146">Wie bereits erwähnt, verwendet die Standardauthentifizierung verwendet, die in IIS integrierte Windows-Anmeldeinformationen an.</span><span class="sxs-lookup"><span data-stu-id="b04ae-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="b04ae-147">Das bedeutet, dass Sie Konten für die Benutzer auf dem Hostserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="b04ae-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="b04ae-148">Aber für eine internetanwendung, Benutzerkonten in der Regel in einer externen Datenbank gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="b04ae-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="b04ae-149">Im folgenden code, wie ein HTTP-Modul, das Standardauthentifizierung ausführt.</span><span class="sxs-lookup"><span data-stu-id="b04ae-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="b04ae-150">Sie können ganz einfach in ein ASP.NET-Mitgliedschaftsanbieter einbinden, durch Ersetzen der `CheckPassword` -Methode, die eine dummy-Methode in diesem Beispiel ist.</span><span class="sxs-lookup"><span data-stu-id="b04ae-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="b04ae-151">In Web-API 2, sollten Sie das Schreiben einer [Authentifizierungsfilter](authentication-filters.md) oder [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/index.md), anstatt ein HTTP-Modul.</span><span class="sxs-lookup"><span data-stu-id="b04ae-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="b04ae-152">Um das HTTP-Modul zu aktivieren, fügen Sie Folgendes in die Datei "web.config" in der **"System.Webserver"** Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="b04ae-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="b04ae-153">Ersetzen Sie "YourAssemblyName", durch den Namen der Assembly (nicht einschließlich der Erweiterungs "Dll").</span><span class="sxs-lookup"><span data-stu-id="b04ae-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="b04ae-154">Sie sollten die anderen Authentifizierungsschemas, z. B. Forms oder Windows-Authentifizierung deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="b04ae-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
