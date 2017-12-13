---
title: "Überschreiben von URLs in ASP.NET Core Middleware"
author: guardrex
description: Informationen Sie zur URL umschreiben und Umleitung mit URL umschreiben Middleware in ASP.NET Core-Anwendungen.
keywords: ASP.NET Core, umschreiben, URL-Rewrite URL umleiten, URL-Umleitung, Middleware, Apache_mod URL
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="6af50-104">Überschreiben von URLs in ASP.NET Core Middleware</span><span class="sxs-lookup"><span data-stu-id="6af50-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="6af50-105">Durch [Luke Latham](https://github.com/guardrex) und [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="6af50-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="6af50-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6af50-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6af50-107">URLs dient der Anforderung, die eine oder mehrere vordefinierte Regeln URLs anhand ändern.</span><span class="sxs-lookup"><span data-stu-id="6af50-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="6af50-108">Eine Abstraktion zwischen Ressourcenpfade und ihre Adressen URLs erstellt werden, sodass die Standorte und die Adressen nicht eng miteinander verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="6af50-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="6af50-109">Es gibt mehrere Szenarien, in denen URLs nützlich ist:</span><span class="sxs-lookup"><span data-stu-id="6af50-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="6af50-110">Verschieben oder ersetzen Serverressourcen temporär oder dauerhaft Beibehaltung stabil Locators für diese Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6af50-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="6af50-111">Teilen bei der anforderungsverarbeitung für andere apps oder Bereiche von einer app</span><span class="sxs-lookup"><span data-stu-id="6af50-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="6af50-112">Entfernen von, hinzufügen oder Neuorganisieren von URL-Segmente für eingehende Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6af50-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="6af50-113">Optimieren der öffentlichen URLs für Search Engine Optimization (SEO)</span><span class="sxs-lookup"><span data-stu-id="6af50-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="6af50-114">Die Verwendung von benutzerfreundlichen Öffentliche URLs können Personen, die den Inhalt Vorhersagen sie erwarten können, indem Sie einen Link zulassen</span><span class="sxs-lookup"><span data-stu-id="6af50-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="6af50-115">Umleiten von unsichere Anforderungen zum Sichern von Endpunkten</span><span class="sxs-lookup"><span data-stu-id="6af50-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="6af50-116">Verhindern von Image hotlinking</span><span class="sxs-lookup"><span data-stu-id="6af50-116">Preventing image hotlinking</span></span>

<span data-ttu-id="6af50-117">Sie können Regeln für die Änderung der URL auf verschiedene Arten, einschließlich Regex, Apache Mod_rewrite Modul Regeln, Rewrite-Modul für IIS-Regeln und verwenden benutzerdefinierte Logik definieren.</span><span class="sxs-lookup"><span data-stu-id="6af50-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="6af50-118">Dieses Dokument führt URLs mit Anweisungen zum Neuerstellen von URL-Middleware in ASP.NET Core-apps verwenden.</span><span class="sxs-lookup"><span data-stu-id="6af50-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="6af50-119">URLs kann die Leistung einer App reduzieren.</span><span class="sxs-lookup"><span data-stu-id="6af50-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="6af50-120">Sofern möglich, sollten Sie die Anzahl und Komplexität der Regeln beschränken.</span><span class="sxs-lookup"><span data-stu-id="6af50-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="6af50-121">Schreiben Sie die URL-Umleitung "und"-URL</span><span class="sxs-lookup"><span data-stu-id="6af50-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="6af50-122">Der Unterschied in die Formulierung "zwischen" *URL-Umleitung* und *URL Rewrite* mag feine am ersten hat wichtige Auswirkungen auf die Ressourcen für Clients bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6af50-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="6af50-123">ASP.NET Core URL umschreiben Middleware ist in der Besprechung müssen für beide.</span><span class="sxs-lookup"><span data-stu-id="6af50-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="6af50-124">Ein *URL-Umleitung* ist ein Vorgang die clientseitige, in dem der Client den Zugriff auf eine Ressource an eine andere Adresse angewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="6af50-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="6af50-125">Dies ist einen Roundtrip zum Server erforderlich, und die umleitungs-URL an den Client zurückgegeben wird in der Adressleiste des Browsers angezeigt, wenn der Client eine neue Anforderung für die Ressource stellt.</span><span class="sxs-lookup"><span data-stu-id="6af50-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="6af50-126">Wenn `/resource` ist *umgeleitet* auf `/different-resource`, der Clientanforderungen `/resource`, und der Server antwortet, dass der Client die Ressource unter Abrufen sollte `/different-resource` mit einem Status Code gibt an, dass die Umleitung ist temporär oder dauerhaft ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6af50-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="6af50-127">Der Client führt eine neue Anforderung für die Ressource an die umleitungs-URL.</span><span class="sxs-lookup"><span data-stu-id="6af50-127">The client executes a new request for the resource at the redirect URL.</span></span>

![Ein Dienstendpunkt WebAPI wurde vorübergehend von Version 1 (v1) auf Version 2 (v2) auf dem Server geändert.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="6af50-133">Wenn Anforderungen zu einer anderen URL umgeleitet, geben Sie an, ob die Umleitung permanente oder temporäre ist.</span><span class="sxs-lookup"><span data-stu-id="6af50-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="6af50-134">Der 301 (Permanent verschoben) Statuscode wird, in dem die Ressource besitzt, eine neue, permanente-URL, und Sie möchten verwendet, um dem Client anzuweisen, dass alle zukünftige Anforderungen für die Ressource mit die neue URL verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="6af50-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="6af50-135">*Der Client möglicherweise die Antwort zwischenspeichern, wenn ein Statuscode "301" empfangen wird.*</span><span class="sxs-lookup"><span data-stu-id="6af50-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="6af50-136">Der Statuscode "302 (gefunden)" wird verwendet, in denen die Umleitung temporäre oder in der Regel der Betreffzeile um zu ändern, dass der Client sollte nicht speichern und, die umleitungs-URL in der Zukunft wiederverwenden.</span><span class="sxs-lookup"><span data-stu-id="6af50-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="6af50-137">Weitere Informationen finden Sie unter [RFC 2616: Statuscodedefinitionen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6af50-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="6af50-138">Ein *URL Rewrite* ist ein serverseitiger Vorgang, um eine Ressource von einer anderen Ressource Adresse bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="6af50-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="6af50-139">Eine URL umschreiben erfordern keinen Roundtrip zum Server.</span><span class="sxs-lookup"><span data-stu-id="6af50-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="6af50-140">Die umgeschriebene URL ist nicht an den Client zurückgegeben und wird nicht in der Adressleiste des Browsers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6af50-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="6af50-141">Wenn `/resource` ist *umgeschrieben* auf `/different-resource`, der Clientanforderungen `/resource`, und der Server *intern* Ruft die Ressource auf `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="6af50-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="6af50-142">Obwohl der Client die Ressource auf die umgeschriebene URL abrufen sein kann, wird nicht der Client informiert werden, dass die Ressource an der umgeschriebene URL vorhanden ist, wenn er die Anforderung sendet und die Antwort empfängt.</span><span class="sxs-lookup"><span data-stu-id="6af50-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Ein Dienstendpunkt WebAPI wurde von Version 1 (v1) auf Version 2 (v2) auf dem Server geändert.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="6af50-147">Umschreiben von URL-Beispiel-app</span><span class="sxs-lookup"><span data-stu-id="6af50-147">URL rewriting sample app</span></span>
<span data-ttu-id="6af50-148">Untersuchen Sie die Funktionen von der URL umschreiben Middleware mit der [Umschreiben von URL-Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="6af50-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="6af50-149">Die app gilt, schreiben und Umleitung Regeln und zeigt die URL umgeschriebene oder umgeleitet erfolgen.</span><span class="sxs-lookup"><span data-stu-id="6af50-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="6af50-150">URL umschreiben Middleware verwenden</span><span class="sxs-lookup"><span data-stu-id="6af50-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="6af50-151">URL umschreiben Middleware verwenden, wenn Sie nicht verwenden, können die [URL Rewrite-Modul](https://www.iis.net/downloads/microsoft/url-rewrite) mit IIS unter Windows Server, die [Apache Mod_rewrite-Modul](https://httpd.apache.org/docs/2.4/rewrite/) auf Apache-Server [URLs auf Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), oder auf Ihrer app gehostet wird [HTTP.sys Server](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="6af50-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="6af50-152">Der Hauptgründe, warum die Server-basierte URL Umschreiben von Technologien in IIS, Apache oder Nginx verwendet werden, dass die Middleware nicht die vollständigen Funktionen von diesen Modulen nicht unterstützt und die Leistung der Middleware wahrscheinlich nicht, die von den Modulen überein.</span><span class="sxs-lookup"><span data-stu-id="6af50-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="6af50-153">Es gibt jedoch einige Funktionen von Servermodule, die mit ASP.NET Core-Projekten, wie z. B. nicht die `IsFile` und `IsDirectory` Einschränkungen des IIS-Rewrite-Modul.</span><span class="sxs-lookup"><span data-stu-id="6af50-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="6af50-154">Verwenden Sie in diesen Szenarien stattdessen die Middleware.</span><span class="sxs-lookup"><span data-stu-id="6af50-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="6af50-155">Package</span><span class="sxs-lookup"><span data-stu-id="6af50-155">Package</span></span>
<span data-ttu-id="6af50-156">Um die Middleware in Ihrem Projekt einzuschließen, fügen Sie einen Verweis auf die [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) Paket.</span><span class="sxs-lookup"><span data-stu-id="6af50-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="6af50-157">Diese Funktion ist für apps für ASP.NET Core 1.1 oder höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="6af50-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="6af50-158">Erweiterung und Optionen</span><span class="sxs-lookup"><span data-stu-id="6af50-158">Extension and options</span></span>
<span data-ttu-id="6af50-159">Die URL-Rewrite herzustellen und leiten Sie Regeln durch das Erstellen einer Instanz von der `RewriteOptions` Klasse mit Erweiterungsmethoden für die einzelnen Regeln.</span><span class="sxs-lookup"><span data-stu-id="6af50-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="6af50-160">Verkettet mehrere Regeln in der Reihenfolge, in der Sie verarbeitet sollen.</span><span class="sxs-lookup"><span data-stu-id="6af50-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="6af50-161">Die `RewriteOptions` in der URL umschreiben Middleware übergeben werden, während sie mit der Anforderungspipeline hinzugefügt werden `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="6af50-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="6af50-164">URL-Umleitung</span><span class="sxs-lookup"><span data-stu-id="6af50-164">URL redirect</span></span>
<span data-ttu-id="6af50-165">Verwendung `AddRedirect` Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="6af50-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="6af50-166">Der erste Parameter enthält die Regex für den Abgleich für den Pfad der eingehenden URL an.</span><span class="sxs-lookup"><span data-stu-id="6af50-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="6af50-167">Der zweite Parameter ist die Ersatzzeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="6af50-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="6af50-168">Der dritte Parameter, gibt falls vorhanden, den Statuscode.</span><span class="sxs-lookup"><span data-stu-id="6af50-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="6af50-169">Wenn Sie den Statuscode nicht angeben, wird standardmäßig 302 (Found), der angibt, dass die Ressource vorübergehend verschoben oder ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="6af50-172">In einem Browser mit den Entwicklertools aktiviert, nehmen Sie eine Anforderung an die Beispiel-app mit dem Pfad `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6af50-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="6af50-173">Der Regex-Ausdruck entspricht den Anforderungspfad auf `redirect-rule/(.*)`, und der Pfad wird durch ersetzt `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6af50-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="6af50-174">Die Umleitung wird zurück an den Client mit Statuscode 302 (gefunden) URL gesendet.</span><span class="sxs-lookup"><span data-stu-id="6af50-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="6af50-175">Der Browser sendet eine neue Anforderung an die umleitungs-URL, die in der Adressleiste des Browsers angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="6af50-176">Da keine Regeln in der Beispiel-app auf der umleitungs-URL übereinstimmen, die zweite Anforderung empfängt eine 200 (OK)-Antwort von der app, und der Text der Antwort zeigt der umleitungs-URL.</span><span class="sxs-lookup"><span data-stu-id="6af50-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="6af50-177">Ein Roundtrip wird mit dem Server ausgelöst, wenn eine URL ist *umgeleitet*.</span><span class="sxs-lookup"><span data-stu-id="6af50-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="6af50-178">Seien Sie vorsichtig beim Einrichten der umleitungs-Regeln.</span><span class="sxs-lookup"><span data-stu-id="6af50-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="6af50-179">Die umleitungs-Regeln werden bei jeder Anforderung an die app, die nach einer Umleitung einschließlich ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="6af50-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="6af50-180">Es ist einfach, versehentlich eine Schleife von unendliche umleitungen erstellen.</span><span class="sxs-lookup"><span data-stu-id="6af50-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="6af50-181">Ursprüngliche Anforderung:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6af50-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="6af50-183">Wird aufgerufen, der Teil des Ausdrucks innerhalb der Klammern eine *Erfassungsgruppe*.</span><span class="sxs-lookup"><span data-stu-id="6af50-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="6af50-184">Der Punkt (`.`) des Ausdrucks bedeutet *Übereinstimmung mit beliebigem Zeichen*.</span><span class="sxs-lookup"><span data-stu-id="6af50-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="6af50-185">Das Sternchen (`*`) gibt an, *entspricht dem vorangehende Zeichen keine oder mehrmalige*.</span><span class="sxs-lookup"><span data-stu-id="6af50-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="6af50-186">Aus diesem Grund die letzten beiden Pfadsegmente der URL, `1234/5678`, werden von der Erfassungsgruppe erfasst `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="6af50-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="6af50-187">Jeder Wert, der Sie in der Anforderungs-URL nach dem Bereitstellen `redirect-rule/` als dieser einzelnen Erfassungsgruppe erfasst wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="6af50-188">In der Ersetzungszeichenfolge erfasste Gruppen werden in der Zeichenfolge mit einem Dollarzeichen eingefügt (`$`) gefolgt von die Sequenznummer der Erfassung.</span><span class="sxs-lookup"><span data-stu-id="6af50-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="6af50-189">Der erste Wert der Capture-Gruppe mit abgerufen wird `$1`, wobei der zweite mit `$2`, in Reihenfolge für die Erfassungsgruppen in Ihre Regex weiter.</span><span class="sxs-lookup"><span data-stu-id="6af50-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="6af50-190">Ist nur bei einer erfasste Gruppe der Umleitung Regel Regex in der Beispiel-app, daher besteht nur eine eingefügte Gruppe in der Ersetzungszeichenfolge ein, also `$1`.</span><span class="sxs-lookup"><span data-stu-id="6af50-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="6af50-191">Wenn die Regel angewendet wird, wird die URL `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6af50-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="6af50-192">URL-Umleitung an einen sicheren Endpunkt</span><span class="sxs-lookup"><span data-stu-id="6af50-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="6af50-193">Verwendung `AddRedirectToHttps` zum Umleiten von HTTP-Anforderungen auf dem gleichen Host und Pfad, die über HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="6af50-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="6af50-194">Wenn der Statuscode ist nicht angegeben wird, standardmäßig die Middleware 302 (gefunden).</span><span class="sxs-lookup"><span data-stu-id="6af50-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="6af50-195">Wenn der Port angegeben ist, wird die Middleware standardmäßig `null`, was bedeutet, dass das Protokoll ändert sich in `https://` und der Client greift auf die Ressource über Port 443.</span><span class="sxs-lookup"><span data-stu-id="6af50-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="6af50-196">Im Beispiel veranschaulicht das Festlegen des Statuscodes 301 (Permanent verschoben) und den Port zu 5001 ändern.</span><span class="sxs-lookup"><span data-stu-id="6af50-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="6af50-197">Verwendung `AddRedirectToHttpsPermanent` umleiten unsichere Anforderungen auf dem Host und Pfad mit sicheren HTTPS-Protokoll (`https://` über Port 443).</span><span class="sxs-lookup"><span data-stu-id="6af50-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="6af50-198">Die Middleware legt den Statuscode 301 (Permanent verschoben).</span><span class="sxs-lookup"><span data-stu-id="6af50-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="6af50-199">Die Beispiel-app ist in der Lage ist, demonstrieren, wie Sie `AddRedirectToHttps` oder `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="6af50-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="6af50-200">Fügen Sie die Erweiterungsmethode, um die `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="6af50-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="6af50-201">Stellen Sie eine unsichere Anforderung an die app auf eine beliebige URL ein.</span><span class="sxs-lookup"><span data-stu-id="6af50-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="6af50-202">Schließen Sie den Browser "sicherheitswarnung", dass das selbstsignierte Zertifikat nicht vertrauenswürdig ist.</span><span class="sxs-lookup"><span data-stu-id="6af50-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="6af50-203">Ursprüngliche Anforderung mit `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="6af50-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="6af50-205">Ursprüngliche Anforderung mit `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="6af50-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="6af50-207">URL rewrite</span><span class="sxs-lookup"><span data-stu-id="6af50-207">URL rewrite</span></span>
<span data-ttu-id="6af50-208">Verwendung `AddRewrite` ein Regeln zum Umschreiben von URLs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6af50-208">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="6af50-209">Der erste Parameter enthält die Regex für den Abgleich für die eingehende URL-Pfad.</span><span class="sxs-lookup"><span data-stu-id="6af50-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="6af50-210">Der zweite Parameter ist die Ersatzzeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="6af50-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="6af50-211">Der dritte Parameter `skipRemainingRules: {true|false}`, gibt an, um die Middleware ob zusätzliche neuschreibungsregeln überspringen, wenn die aktuelle Regel angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="6af50-214">Ursprüngliche Anforderung:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6af50-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Browser-Fenster mit den Entwicklertools nachverfolgen, die Anforderung und Antwort](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="6af50-216">Beachten Sie in den Regex als Erstes wird das Caretzeichen (`^`) am Anfang des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="6af50-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="6af50-217">Dies bedeutet, dass die Übereinstimmung am Anfang des URL-Pfads beginnt.</span><span class="sxs-lookup"><span data-stu-id="6af50-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="6af50-218">Im vorangehenden Beispiel mit der umleitungsregel `redirect-rule/(.*)`, gibt es keine Caretzeichen am Anfang der Regex; deshalb keine Zeichen vorangestellt möglicherweise `redirect-rule/` in den Pfad für eine erfolgreiche Übereinstimmung.</span><span class="sxs-lookup"><span data-stu-id="6af50-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="6af50-219">Pfad</span><span class="sxs-lookup"><span data-stu-id="6af50-219">Path</span></span>                               | <span data-ttu-id="6af50-220">Match</span><span class="sxs-lookup"><span data-stu-id="6af50-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="6af50-221">Ja</span><span class="sxs-lookup"><span data-stu-id="6af50-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="6af50-222">Ja</span><span class="sxs-lookup"><span data-stu-id="6af50-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="6af50-223">Ja</span><span class="sxs-lookup"><span data-stu-id="6af50-223">Yes</span></span>   |

<span data-ttu-id="6af50-224">Die neuschreibungsregel `^rewrite-rule/(\d+)/(\d+)`, nur Pfade entspricht, wenn sie mit dem `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="6af50-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="6af50-225">Beachten Sie den Unterschied zwischen den folgenden neuschreibungsregel und der oben genannten umleitungsregel abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="6af50-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="6af50-226">Pfad</span><span class="sxs-lookup"><span data-stu-id="6af50-226">Path</span></span>                              | <span data-ttu-id="6af50-227">Match</span><span class="sxs-lookup"><span data-stu-id="6af50-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="6af50-228">Ja</span><span class="sxs-lookup"><span data-stu-id="6af50-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="6af50-229">Nein</span><span class="sxs-lookup"><span data-stu-id="6af50-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="6af50-230">Nein</span><span class="sxs-lookup"><span data-stu-id="6af50-230">No</span></span>    |

<span data-ttu-id="6af50-231">Nach der `^rewrite-rule/` Teil des Ausdrucks, es gibt zwei Erfassungsgruppen, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="6af50-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="6af50-232">Die `\d` , bedeutet dies *eine Ziffer (Anzahl) entsprechen*.</span><span class="sxs-lookup"><span data-stu-id="6af50-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="6af50-233">Das Pluszeichen (`+`) bedeutet, dass *abgleichen mit einem oder mehreren das vorherige Zeichen*.</span><span class="sxs-lookup"><span data-stu-id="6af50-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="6af50-234">Aus diesem Grund muss die URL eine Zahl, gefolgt von einem Schrägstrich, gefolgt von einer anderen Zahl enthalten.</span><span class="sxs-lookup"><span data-stu-id="6af50-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="6af50-235">Diese Gruppen werden in der umgeschriebene URL eingefügt Capture `$1` und `$2`.</span><span class="sxs-lookup"><span data-stu-id="6af50-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="6af50-236">Die Ersetzungszeichenfolge der Rewrite-Regel platziert der erfassten Gruppe in die Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="6af50-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="6af50-237">Der angeforderte Pfad der `/rewrite-rule/1234/5678` wird erneut geschrieben, um die Ressource auf erhalten `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="6af50-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="6af50-238">Wenn eine Abfragezeichenfolge für die ursprüngliche Anforderung vorhanden ist, wird es beim URL umgeschrieben wird beibehalten.</span><span class="sxs-lookup"><span data-stu-id="6af50-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="6af50-239">Es gibt keine Roundtrip zum Server, der die Ressource zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="6af50-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="6af50-240">Wenn die Ressource vorhanden ist, hat es abgerufen und an den Client mit einem Statuscode "200 (OK)" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6af50-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="6af50-241">Da der Client umgeleitet wird, wird die URL in die Adressleiste des Browsers nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="6af50-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="6af50-242">Soweit der Client ist, ist der URL-Rewrite Vorgang nie aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="6af50-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="6af50-243">Verwendung `skipRemainingRules: true` wann immer möglich, da Abgleichsregeln ein kostengünstiger Prozess ist und reduziert die Antwortzeit für die app.</span><span class="sxs-lookup"><span data-stu-id="6af50-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="6af50-244">Für die schnellste app-Antwort:</span><span class="sxs-lookup"><span data-stu-id="6af50-244">For the fastest app response:</span></span>
> * <span data-ttu-id="6af50-245">Sortieren Sie die neuschreibungsregeln am häufigsten übereinstimmende Regel aus, die am seltensten übereinstimmende Regel.</span><span class="sxs-lookup"><span data-stu-id="6af50-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="6af50-246">Überspringen Sie die Verarbeitung der verbleibenden Regeln aus, wenn eine Übereinstimmung auftritt und keine weitere regelverarbeitung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="6af50-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="6af50-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6af50-247">Apache mod_rewrite</span></span>
<span data-ttu-id="6af50-248">Wenden Sie Apache Mod_rewrite Regeln mit `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="6af50-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="6af50-249">Stellen Sie sicher, dass die Rules-Datei mit der app bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6af50-250">Weitere Informationen und Beispiele für Mod_rewrite-Regeln finden Sie unter [Apache Mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="6af50-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6af50-252">Ein `StreamReader` wird verwendet, lesen Sie die Regeln aus der *ApacheModRewrite.txt* Rules-Datei.</span><span class="sxs-lookup"><span data-stu-id="6af50-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6af50-254">Der erste Parameter erwartet ein `IFileProvider`, weiter über [Abhängigkeitsinjektion](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6af50-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="6af50-255">Die `IHostingEnvironment` eingefügt wird, zum Bereitstellen der `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6af50-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="6af50-256">Der zweite Parameter ist der Pfad zu der Rules-Datei, also *ApacheModRewrite.txt* in der Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="6af50-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="6af50-257">Die Beispiel-app leitet Anforderungen von `/apache-mod-rules-redirect/(.\*)` auf `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6af50-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="6af50-258">Statuscode der Antwort ist 302 (gefunden).</span><span class="sxs-lookup"><span data-stu-id="6af50-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="6af50-259">Ursprüngliche Anforderung:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="6af50-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="6af50-261">Unterstützte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="6af50-261">Supported server variables</span></span>
<span data-ttu-id="6af50-262">Die Middleware unterstützt die folgenden Apache Mod_rewrite Servervariablen:</span><span class="sxs-lookup"><span data-stu-id="6af50-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="6af50-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6af50-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="6af50-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6af50-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6af50-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6af50-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6af50-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6af50-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="6af50-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="6af50-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="6af50-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6af50-268">HTTP_HOST</span></span>
* <span data-ttu-id="6af50-269">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6af50-269">HTTP_REFERER</span></span>
* <span data-ttu-id="6af50-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6af50-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6af50-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6af50-271">HTTPS</span></span>
* <span data-ttu-id="6af50-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="6af50-272">IPV6</span></span>
* <span data-ttu-id="6af50-273">ABFRAGEZEICHENFOLGE</span><span class="sxs-lookup"><span data-stu-id="6af50-273">QUERY_STRING</span></span>
* <span data-ttu-id="6af50-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6af50-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="6af50-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6af50-275">REMOTE_PORT</span></span>
* <span data-ttu-id="6af50-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6af50-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6af50-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="6af50-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="6af50-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="6af50-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="6af50-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6af50-279">REQUEST_URI</span></span>
* <span data-ttu-id="6af50-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6af50-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="6af50-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="6af50-281">SERVER_ADDR</span></span>
* <span data-ttu-id="6af50-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="6af50-282">SERVER_PORT</span></span>
* <span data-ttu-id="6af50-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="6af50-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="6af50-284">ZEIT</span><span class="sxs-lookup"><span data-stu-id="6af50-284">TIME</span></span>
* <span data-ttu-id="6af50-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="6af50-285">TIME_DAY</span></span>
* <span data-ttu-id="6af50-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="6af50-286">TIME_HOUR</span></span>
* <span data-ttu-id="6af50-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="6af50-287">TIME_MIN</span></span>
* <span data-ttu-id="6af50-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="6af50-288">TIME_MON</span></span>
* <span data-ttu-id="6af50-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="6af50-289">TIME_SEC</span></span>
* <span data-ttu-id="6af50-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="6af50-290">TIME_WDAY</span></span>
* <span data-ttu-id="6af50-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="6af50-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="6af50-292">IIS URL Rewrite-Modul-Regeln</span><span class="sxs-lookup"><span data-stu-id="6af50-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="6af50-293">Verwenden, um Regeln zu verwenden, die für das IIS URL Rewrite-Modul gelten `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="6af50-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="6af50-294">Stellen Sie sicher, dass die Rules-Datei mit der app bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6af50-295">Nicht weiterleiten die Middleware verwenden Ihre *"Web.config"* bei Ausführung auf Windows Server IIS-Datei.</span><span class="sxs-lookup"><span data-stu-id="6af50-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="6af50-296">Bei IIS diese Regeln gespeichert werden sollen, außerhalb von Ihrem *"Web.config"* zur Vermeidung von Konflikten mit der IIS-Rewrite-Modul.</span><span class="sxs-lookup"><span data-stu-id="6af50-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="6af50-297">Weitere Informationen und Beispiele für IIS URL Rewrite-Modul-Regeln finden Sie unter [mithilfe von Url Rewrite-Modul 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) und [Konfiguration des URL schreiben Modulverweis](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="6af50-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6af50-299">Ein `StreamReader` wird verwendet, lesen Sie die Regeln aus der *IISUrlRewrite.xml* Rules-Datei.</span><span class="sxs-lookup"><span data-stu-id="6af50-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-300">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6af50-301">Der erste Parameter erwartet ein `IFileProvider`, während der zweite Parameter den Pfad zu den Regeln der XML-Datei wird also *IISUrlRewrite.xml* in der Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="6af50-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="6af50-302">Die Beispiel-app ändert die Anforderungen von `/iis-rules-rewrite/(.*)` auf `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6af50-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="6af50-303">Die Antwort wird an den Client mit Statuscode 200 (OK) gesendet.</span><span class="sxs-lookup"><span data-stu-id="6af50-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="6af50-304">Ursprüngliche Anforderung:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="6af50-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Browser-Fenster mit den Entwicklertools nachverfolgen, die Anforderung und Antwort](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="6af50-306">Wenn Sie eine aktive IIS Rewrite-Modul auf Serverebene Regeln konfiguriert, die Ihre app auf unerwünschten Weise beeinträchtigen würde haben, können Sie das IIS Rewrite-Modul für eine app deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="6af50-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="6af50-307">Weitere Informationen finden Sie unter [Deaktivieren von IIS-Module](xref:hosting/iis-modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6af50-307">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="6af50-308">Nicht unterstützte Funktionen</span><span class="sxs-lookup"><span data-stu-id="6af50-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6af50-310">Die Middleware freigegeben mit ASP.NET Core 2.x unterstützt die folgenden Funktionen von IIS URL Rewrite-Modul:</span><span class="sxs-lookup"><span data-stu-id="6af50-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="6af50-311">Ausgehende Regeln</span><span class="sxs-lookup"><span data-stu-id="6af50-311">Outbound Rules</span></span>
* <span data-ttu-id="6af50-312">Benutzerdefinierte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="6af50-312">Custom Server Variables</span></span>
* <span data-ttu-id="6af50-313">Platzhalter</span><span class="sxs-lookup"><span data-stu-id="6af50-313">Wildcards</span></span>
* <span data-ttu-id="6af50-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6af50-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6af50-316">Die Middleware freigegeben mit ASP.NET Core 1.x unterstützt die folgenden Funktionen von IIS URL Rewrite-Modul:</span><span class="sxs-lookup"><span data-stu-id="6af50-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="6af50-317">Globale Regeln</span><span class="sxs-lookup"><span data-stu-id="6af50-317">Global Rules</span></span>
* <span data-ttu-id="6af50-318">Ausgehende Regeln</span><span class="sxs-lookup"><span data-stu-id="6af50-318">Outbound Rules</span></span>
* <span data-ttu-id="6af50-319">Schreiben von Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="6af50-319">Rewrite Maps</span></span>
* <span data-ttu-id="6af50-320">CustomResponse Aktion</span><span class="sxs-lookup"><span data-stu-id="6af50-320">CustomResponse Action</span></span>
* <span data-ttu-id="6af50-321">Benutzerdefinierte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="6af50-321">Custom Server Variables</span></span>
* <span data-ttu-id="6af50-322">Platzhalter</span><span class="sxs-lookup"><span data-stu-id="6af50-322">Wildcards</span></span>
* <span data-ttu-id="6af50-323">Aktion: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="6af50-323">Action:CustomResponse</span></span>
* <span data-ttu-id="6af50-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6af50-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="6af50-325">Unterstützte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="6af50-325">Supported server variables</span></span>
<span data-ttu-id="6af50-326">Die Middleware unterstützt die folgenden Servervariablen von IIS URL Rewrite-Modul:</span><span class="sxs-lookup"><span data-stu-id="6af50-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="6af50-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="6af50-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="6af50-328">INHALTSTYP</span><span class="sxs-lookup"><span data-stu-id="6af50-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="6af50-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6af50-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6af50-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6af50-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6af50-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6af50-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="6af50-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6af50-332">HTTP_HOST</span></span>
* <span data-ttu-id="6af50-333">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6af50-333">HTTP_REFERER</span></span>
* <span data-ttu-id="6af50-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="6af50-334">HTTP_URL</span></span>
* <span data-ttu-id="6af50-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6af50-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6af50-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6af50-336">HTTPS</span></span>
* <span data-ttu-id="6af50-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="6af50-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="6af50-338">ABFRAGEZEICHENFOLGE</span><span class="sxs-lookup"><span data-stu-id="6af50-338">QUERY_STRING</span></span>
* <span data-ttu-id="6af50-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6af50-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="6af50-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6af50-340">REMOTE_PORT</span></span>
* <span data-ttu-id="6af50-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6af50-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6af50-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6af50-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="6af50-343">Sie erhalten außerdem einen `IFileProvider` über eine `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6af50-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="6af50-344">Dieser Ansatz kann größere Flexibilität für den Speicherort, der die Umgestaltung Rules-Dateien bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6af50-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="6af50-345">Stellen Sie sicher, dass Ihre Rewrite Rules-Dateien in den Pfad auf dem Server bereitgestellt werden, die Sie bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6af50-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="6af50-346">Methodenbasierte Regel</span><span class="sxs-lookup"><span data-stu-id="6af50-346">Method-based rule</span></span>
<span data-ttu-id="6af50-347">Verwendung `Add(Action<RewriteContext> applyRule)` Ihre eigene Regellogik in einer Methode zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6af50-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="6af50-348">Die `RewriteContext` macht die `HttpContext` für die Verwendung in der Methode.</span><span class="sxs-lookup"><span data-stu-id="6af50-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="6af50-349">Die `context.Result` bestimmt, wie zusätzliche Pipeline Verarbeitung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="6af50-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="6af50-350">Kontext. Ergebnis</span><span class="sxs-lookup"><span data-stu-id="6af50-350">context.Result</span></span>                       | <span data-ttu-id="6af50-351">Aktion</span><span class="sxs-lookup"><span data-stu-id="6af50-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="6af50-352">`RuleResult.ContinueRules` (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="6af50-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="6af50-353">Anwenden von Regeln wird fortgesetzt</span><span class="sxs-lookup"><span data-stu-id="6af50-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="6af50-354">Beenden Sie die Regeln angewendet werden, und Senden der Antwort</span><span class="sxs-lookup"><span data-stu-id="6af50-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="6af50-355">Beenden Sie die Regeln angewendet werden, und senden Sie den Kontext an die nächste middleware</span><span class="sxs-lookup"><span data-stu-id="6af50-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-356">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="6af50-358">Die Beispiel-app zeigt eine Methode, die leitet Anforderungen für Pfade, die mit enden *XML*.</span><span class="sxs-lookup"><span data-stu-id="6af50-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="6af50-359">Wenn Sie eine Anforderung für vornehmen `/file.xml`, wird er an umgeleitet `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="6af50-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="6af50-360">Der Statuscode 301 (Permanent verschoben) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6af50-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="6af50-361">Für eine Umleitung zu können, müssen Sie explizit den Statuscode der Antwort festlegen. Andernfalls wird ein Statuscode "200 (OK)" zurückgegeben, und die Umleitung wird nicht auf dem Client auftreten.</span><span class="sxs-lookup"><span data-stu-id="6af50-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-362">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="6af50-364">Ursprüngliche Anforderung:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="6af50-364">Original Request: `/file.xml`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten für file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="6af50-366">IRule basierende Regel</span><span class="sxs-lookup"><span data-stu-id="6af50-366">IRule-based rule</span></span>
<span data-ttu-id="6af50-367">Verwendung `Add(IRule)` Ihre eigene Regellogik in einer Klasse zu implementieren, die abgeleitet `IRule`.</span><span class="sxs-lookup"><span data-stu-id="6af50-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="6af50-368">Mithilfe einer `IRule` bietet größere Flexibilität gegenüber des methodenbasierten Regel Ansatzes.</span><span class="sxs-lookup"><span data-stu-id="6af50-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="6af50-369">Die abgeleitete Klasse eventuell einen Konstruktor, in dem Sie im Parameter für übergeben können die `ApplyRule` Methode.</span><span class="sxs-lookup"><span data-stu-id="6af50-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="6af50-372">Die Werte der Parameter in der Beispiel-app für die `extension` und `newPath` werden überprüft, um mehrere Bedingungen erfüllen.</span><span class="sxs-lookup"><span data-stu-id="6af50-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="6af50-373">Die `extension` muss einen Wert enthalten, und der Wert muss *PNG*, *jpg*, oder *GIF*.</span><span class="sxs-lookup"><span data-stu-id="6af50-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="6af50-374">Wenn die `newPath` ist ungültig, ein `ArgumentException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="6af50-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="6af50-375">Wenn Sie eine Anforderung für vornehmen *image.png*, wird er an umgeleitet `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="6af50-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="6af50-376">Wenn Sie eine Anforderung für vornehmen *image.jpg*, wird er an umgeleitet `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="6af50-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="6af50-377">Der Statuscode 301 (Permanent verschoben), festgelegt ist und die `context.Result` Regeln für die Verarbeitung zu beenden, und Senden der Antwort festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="6af50-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6af50-378">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6af50-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6af50-379">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6af50-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="6af50-380">Ursprüngliche Anforderung:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="6af50-380">Original Request: `/image.png`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten für image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="6af50-382">Ursprüngliche Anforderung:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="6af50-382">Original Request: `/image.jpg`</span></span>

![Browser-Fenster mit den Entwicklertools Nachverfolgen von Anforderungen und Antworten für image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="6af50-384">Regex-Beispiele</span><span class="sxs-lookup"><span data-stu-id="6af50-384">Regex examples</span></span>

| <span data-ttu-id="6af50-385">Ziel</span><span class="sxs-lookup"><span data-stu-id="6af50-385">Goal</span></span> | <span data-ttu-id="6af50-386">Regex-Zeichenfolge &</span><span class="sxs-lookup"><span data-stu-id="6af50-386">Regex String &</span></span><br><span data-ttu-id="6af50-387">Match-Beispiel</span><span class="sxs-lookup"><span data-stu-id="6af50-387">Match Example</span></span> | <span data-ttu-id="6af50-388">Ersetzungszeichenfolge &</span><span class="sxs-lookup"><span data-stu-id="6af50-388">Replacement String &</span></span><br><span data-ttu-id="6af50-389">Ausgabebeispiel</span><span class="sxs-lookup"><span data-stu-id="6af50-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="6af50-390">Schreiben Sie Pfad in der Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6af50-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="6af50-391">Bereichsstreifen nachgestellten Schrägstrich</span><span class="sxs-lookup"><span data-stu-id="6af50-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="6af50-392">Erzwingen Sie die nachstehenden Schrägstrich</span><span class="sxs-lookup"><span data-stu-id="6af50-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="6af50-393">Vermeiden Sie umschreiben bestimmte Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6af50-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="6af50-394">"Ja":`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="6af50-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="6af50-395">Nein:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="6af50-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="6af50-396">Neuanordnen von URL-Segmente</span><span class="sxs-lookup"><span data-stu-id="6af50-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="6af50-397">Ersetzen Sie ein URL-segment</span><span class="sxs-lookup"><span data-stu-id="6af50-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="6af50-398">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6af50-398">Additional resources</span></span>
* [<span data-ttu-id="6af50-399">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="6af50-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="6af50-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="6af50-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="6af50-401">Reguläre Ausdrücke in .NET</span><span class="sxs-lookup"><span data-stu-id="6af50-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="6af50-402">Sprachelemente für reguläre Ausdrücke – Kurzübersicht</span><span class="sxs-lookup"><span data-stu-id="6af50-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="6af50-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6af50-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="6af50-404">Verwenden von Url-Rewrite-Modul 2.0 (für IIS)</span><span class="sxs-lookup"><span data-stu-id="6af50-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="6af50-405">URL Rewrite-Modul Konfigurationsverweis</span><span class="sxs-lookup"><span data-stu-id="6af50-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="6af50-406">IIS URL Rewrite-Modul-Forum</span><span class="sxs-lookup"><span data-stu-id="6af50-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="6af50-407">Behalten Sie eine einfache URL-Struktur</span><span class="sxs-lookup"><span data-stu-id="6af50-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="6af50-408">Tipps und Tricks 10 URLs</span><span class="sxs-lookup"><span data-stu-id="6af50-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="6af50-409">Schrägstrich oder nicht Schrägstrich</span><span class="sxs-lookup"><span data-stu-id="6af50-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
