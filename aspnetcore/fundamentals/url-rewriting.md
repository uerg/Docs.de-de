---
title: URL-umschreibende Middleware in ASP.NET Core
author: guardrex
description: Informationen zum Umschreiben und Umleiten von URL mit URL-umschreibender Middleware in ASP.NET Core-Anwendungen
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276346"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="29ccf-103">URL-umschreibende Middleware in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29ccf-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="29ccf-104">Von [Luke Latham](https://github.com/guardrex) und [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="29ccf-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="29ccf-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="29ccf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="29ccf-106">Bei der URL-Umschreibung werden die Anforderungs-URLs verändert, die auf mindesten einer vordefinierten Regel basieren.</span><span class="sxs-lookup"><span data-stu-id="29ccf-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="29ccf-107">Es wird eine Abstraktion zwischen den Speicherorten und Adressen von Ressourcen erstellt, sodass diese nicht eng miteinander verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="29ccf-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="29ccf-108">Für folgende Szenarios kann die URL-Umschreibung angewendet werden:</span><span class="sxs-lookup"><span data-stu-id="29ccf-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="29ccf-109">Temporäres oder permanentes Verschieben oder Ersetzen von Serverressourcen, wobei stabile Locators für diese Ressourcen erhalten bleiben.</span><span class="sxs-lookup"><span data-stu-id="29ccf-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="29ccf-110">Verteilen der Verarbeitung von Anforderungen auf verschiedene Apps oder Bereiche einer App.</span><span class="sxs-lookup"><span data-stu-id="29ccf-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="29ccf-111">Entfernen, Hinzufügen oder Umorganisieren von URL-Segmenten in eingehenden Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="29ccf-112">Optimieren von öffentlichen URLs für die Suchmaschinenoptimierung (Search Engine Optimization, SEO).</span><span class="sxs-lookup"><span data-stu-id="29ccf-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="29ccf-113">Zulassen, dass unterstützte öffentliche URLs verwendet werden, damit die Benutzer den Inhalt einsehen können, der ihnen beim Klicken auf einen Link angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="29ccf-114">Umleiten von unsicheren Anforderungen auf sichere Endpunkte.</span><span class="sxs-lookup"><span data-stu-id="29ccf-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="29ccf-115">Vermeiden von Hotlinking von Bildern.</span><span class="sxs-lookup"><span data-stu-id="29ccf-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="29ccf-116">Es gibt verschiedene Möglichkeiten, Regeln zum Ändern der URL zu definieren. Sie können z.B. RegEx, die Apache-Modulregeln „mod_rewrite“, die Regeln zum IIS-Umschreibungsmodul oder benutzerdefinierte logische Regeln verwenden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="29ccf-117">In diesem Artikel wird das Umschreiben von URLs beschrieben. Außerdem erhalten Sie Anweisungen zur Verwendung der URL-umschreibenden Middleware in ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="29ccf-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="29ccf-118">Wenn Sie URLs umschreiben, kann das negative Auswirkungen auf die Leistung einer App haben.</span><span class="sxs-lookup"><span data-stu-id="29ccf-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="29ccf-119">Wenn möglich, sollten Sie so wenig Regeln wie möglich erstellen und darauf achten, dass sie nicht zu kompliziert sind.</span><span class="sxs-lookup"><span data-stu-id="29ccf-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="29ccf-120">Umleiten und Umschreiben von URLs</span><span class="sxs-lookup"><span data-stu-id="29ccf-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="29ccf-121">Auf den ersten Blick scheint der Unterschied zwischen dem *Umleiten* und *Umschreiben* von URLs eher gering zu sein, allerdings haben die beiden Verfahren sehr unterschiedliche Auswirkungen auf die Bereitstellung von Ressourcen für Clients.</span><span class="sxs-lookup"><span data-stu-id="29ccf-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="29ccf-122">Die URL-umschreibenden Middleware von ASP.NET Core kann für beide Vorgänge verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="29ccf-123">Bei der *Umleitung von URLs* handelt es sich um einen clientseitigen Vorgang, bei dem der Client angewiesen wird, auf eine Ressource unter einer anderen Adresse zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="29ccf-124">Dafür ist ein Roundtrip zum Server erforderlich.</span><span class="sxs-lookup"><span data-stu-id="29ccf-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="29ccf-125">Die Umleitungs-URL, die an den Client zurückgegeben wird, wird in der Adressleiste des Browsers angezeigt, wenn der Client eine neue Anforderung an die Ressource sendet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="29ccf-126">Wenn `/resource` auf `/different-resource` *umgeleitet* wird, fordert der Client `/resource` an.</span><span class="sxs-lookup"><span data-stu-id="29ccf-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="29ccf-127">Der Server sendet dann die Antwort, dass der Client die Ressource unter `/different-resource` abrufen soll. In der Antwort ist außerdem ein Statuscode enthalten, aus dem entnommen werden kann, ob die Umleitung temporär oder permanent ist.</span><span class="sxs-lookup"><span data-stu-id="29ccf-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="29ccf-128">Der Client führt unter der Umleitungs-URL eine neue Anforderung für die Ressource aus.</span><span class="sxs-lookup"><span data-stu-id="29ccf-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Ein Web-API-Dienstendpunkt wurde auf dem Server kurzzeitig von Version 1 (v1) auf Version 2 (v2) geändert.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="29ccf-134">Wenn Anforderungen auf eine andere URL umgeleitet werden, müssen Sie angeben, ob die Umleitung temporär oder permanent sein soll.</span><span class="sxs-lookup"><span data-stu-id="29ccf-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="29ccf-135">Der Statuscode „301 – Permanent verschoben“ wird verwendet, wenn der Ressource eine neue permanente URL zugewiesen wurde, und Sie dem Client die Anweisung geben möchten, dass alle zukünftigen Anforderungen an die Ressource die neue URL verwenden sollen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="29ccf-136">*Der Client kann die Antwort zwischenspeichern, wenn der Statuscode 301 empfangen wird.*</span><span class="sxs-lookup"><span data-stu-id="29ccf-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="29ccf-137">Der Statuscode „302 – Gefunden“ wird verwendet, wenn die Umleitung temporär ist oder häufig verändert wird. Dann speichert der Client die Umleitungs-URL nicht und verwendet sie auch nicht erneut.</span><span class="sxs-lookup"><span data-stu-id="29ccf-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="29ccf-138">Weitere Informationen finden Sie unter [RFC 2616: Status Code Definitions (RFC 2616: Statuscodedefinitionen)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="29ccf-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="29ccf-139">Bei der *Umschreibung einer URL* handelt es sich um einen serverseitigen Vorgang, über den Ressourcen von einer anderen Ressourcenadresse bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="29ccf-140">Wenn eine URL umgeschrieben wird, ist kein Roundtrip zum Server erforderlich.</span><span class="sxs-lookup"><span data-stu-id="29ccf-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="29ccf-141">Die umgeschriebene URL wird nicht an den Server zurückgegeben und nicht in der Adressleiste des Browsers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="29ccf-142">Wenn `/resource` in `/different-resource` *umgeschrieben* wird, sendet der Client die Anforderung `/resource`, und der Server ruft die Ressource *intern* unter `/different-resource` ab.</span><span class="sxs-lookup"><span data-stu-id="29ccf-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="29ccf-143">Auch wenn der Client die Ressource unter der umgeschriebenen URL abrufen kann, erhält er nicht die Information, dass die Ressource unter der umgeschriebenen URL gespeichert ist, wenn er die Anforderung sendet und eine Antwort erhält.</span><span class="sxs-lookup"><span data-stu-id="29ccf-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Ein Web-API-Dienstendpunkt wurde auf dem Server von Version 1 (v1) auf Version 2 (v2) geändert.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="29ccf-148">URL-umschreibende Beispiel-App</span><span class="sxs-lookup"><span data-stu-id="29ccf-148">URL rewriting sample app</span></span>

<span data-ttu-id="29ccf-149">Mit der [URL-umschreibenden Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) können Sie die Features der URL-umschreibenden Middleware testen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="29ccf-150">Die App wendet Regeln zum Umschreiben und Umleiten an und stellt die umgeschriebene oder umgeleitete URL dar.</span><span class="sxs-lookup"><span data-stu-id="29ccf-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="29ccf-151">Empfohlene Verwendung der URL-umschreibenden Middleware</span><span class="sxs-lookup"><span data-stu-id="29ccf-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="29ccf-152">Verwenden Sie die URL-umschreibende Middleware, wenn Sie das [URL-Umschreibungsmodul](https://www.iis.net/downloads/microsoft/url-rewrite) nicht mit IIS unter Windows Server verwenden können, wenn Sie unter Apache-Server nicht mit dem [Apache-Modul „mod_rewrite“](https://httpd.apache.org/docs/2.4/rewrite/) arbeiten können, das [Umschreiben von URLs unter Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) nicht funktioniert oder Ihre App von [HTTP.sys-Server](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)) gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="29ccf-153">Sie sollten mit IIS, Apache oder Nginx serverbasierte Technologien zum Umschreiben von URLs verwenden, da die Middleware nicht alle Features dieser Module unterstützt und die Leistung der Middleware nicht mit der Leistung der Module übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="29ccf-154">Dennoch funktionieren einige Features der Servermodule nicht mit ASP.NET Core-Projekten – z.B. die Einschränkungen `IsFile` und `IsDirectory` des IIS-Umschreibungsmoduls.</span><span class="sxs-lookup"><span data-stu-id="29ccf-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="29ccf-155">Verwenden Sie in diesem Szenario stattdessen die Middleware.</span><span class="sxs-lookup"><span data-stu-id="29ccf-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="29ccf-156">Package</span><span class="sxs-lookup"><span data-stu-id="29ccf-156">Package</span></span>

<span data-ttu-id="29ccf-157">Fügen Sie einen Verweis auf das [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)-Paket hinzu, um die Middleware in Ihrem Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="29ccf-158">Dieses Feature ist für alle Apps verfügbar, die für ASP.NET Core 1.1 oder höher konzipiert sind.</span><span class="sxs-lookup"><span data-stu-id="29ccf-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="29ccf-159">Erweiterung und Optionen</span><span class="sxs-lookup"><span data-stu-id="29ccf-159">Extension and options</span></span>

<span data-ttu-id="29ccf-160">Legen Sie Regeln zum Umschreiben und Umleiten von URLs fest, indem Sie eine Instanz der `RewriteOptions`-Klasse erstellen und Erweiterungsmethoden für jede Regel hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="29ccf-161">Verketten Sie mehrere Regeln in der Reihenfolge miteinander, in der sie verarbeitet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="29ccf-162">Die `RewriteOptions` werden an die URL-umschreibende Middleware übergeben, wenn diese mit `app.UseRewriter(options);` zu der Anforderungspipeline hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a><span data-ttu-id="29ccf-165">Umleitungs-URL</span><span class="sxs-lookup"><span data-stu-id="29ccf-165">URL redirect</span></span>

<span data-ttu-id="29ccf-166">Verwenden Sie `AddRedirect`, um Anforderungen umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="29ccf-167">Der erste Parameter enthält Ihren RegEx, damit dieser dem Pfad der eingehenden URL zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="29ccf-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="29ccf-168">Beim zweiten Parameter handelt es sich um eine Ersatzzeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="29ccf-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="29ccf-169">Der dritte Parameter gibt, falls vorhanden, den Statuscode an.</span><span class="sxs-lookup"><span data-stu-id="29ccf-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="29ccf-170">Wenn Sie den Statuscode nicht angeben, wird standardmäßig „302 – Gefunden“ zurückgegeben, was bedeutet, dass die Ressource kurzzeitig verschoben oder ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="29ccf-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-173">Aktivieren Sie in Ihrem Browser die Entwicklertools, und senden Sie eine Anforderung an die Beispiel-App mit dem Pfad `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="29ccf-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="29ccf-174">Der RegEx stimmt mit dem Anforderungspfad unter `redirect-rule/(.*)` überein, und der Pfad wird durch `/redirected/1234/5678` ersetzt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="29ccf-175">Die Umleitungs-URL wird mit dem Statuscode „302 – Gefunden“ an den Client zurückgesendet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="29ccf-176">Unter der Umleitungs-URL sendet der Browser eine neue Anforderung, die in dessen Adressleiste angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="29ccf-177">Da keine Regeln in der Beispiel-App für die Umleitungs-URL gelten, wird für die zweite Anforderung die Antwort „200 – OK“ von der App zurückgegeben, und im Antworttext wird die Umleitungs-URL angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="29ccf-178">Wenn eine URL *weitergeleitet* wird, wird ein Roundtrip zum Server ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="29ccf-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="29ccf-179">Seien Sie vorsichtig, wenn Sie die Umleitungsregeln einrichten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="29ccf-180">Bei jeder Anforderung an die App werden Ihre Umleitungsregeln überprüft – auch nach einer Umleitung.</span><span class="sxs-lookup"><span data-stu-id="29ccf-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="29ccf-181">Dabei kann schnell aus Versehen eine Dauerschleife von Umleitungen entstehen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="29ccf-182">Ursprüngliche Anforderung: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="29ccf-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="29ccf-184">Der Teil des Ausdruck in Klammern wird als *Erfassungsgruppe* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="29ccf-185">Der Punkt (`.`) im Ausdruck steht für *Übereinstimmung mit beliebigem Zeichen*.</span><span class="sxs-lookup"><span data-stu-id="29ccf-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="29ccf-186">Das Sternchen (`*`) steht für *Übereinstimmung mit dem vorausgehenden Zeichen (keinmal oder mindestens einmal)*.</span><span class="sxs-lookup"><span data-stu-id="29ccf-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="29ccf-187">Daher werden die letzten beiden Pfadsegmente der URL (`1234/5678`) von der Erfassungsgruppe erfasst `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="29ccf-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="29ccf-188">Alle Werte, die Sie in der Anforderungs-URL nach `redirect-rule/` angeben, werden von dieser Erfassungsgruppe erfasst.</span><span class="sxs-lookup"><span data-stu-id="29ccf-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="29ccf-189">Erfassungsgruppen werden in der Ersetzungszeichenfolge mit dem Dollarzeichen (`$`) in die Zeichenfolge eingefügt. Danach folgt die Sequenznummer der Erfassung.</span><span class="sxs-lookup"><span data-stu-id="29ccf-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="29ccf-190">Der erste Wert der Erfassungsgruppe wird mit `$1` abgerufen, der zweite mit `$2`. Dies wird in Sequenzen für die Erfassungsgruppen Ihres RegEx weitergeführt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="29ccf-191">Nur eine Erfassungsgruppe ist in der Beispiel-App im RegEx der Umleitungsregel enthalten. Das bedeutet, dass es in die Ersetzungszeichenfolge nur eine Gruppe eingefügt wird, nämlich `$1`.</span><span class="sxs-lookup"><span data-stu-id="29ccf-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="29ccf-192">Wenn die Regel angewendet wird, ändert sich die URL in `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="29ccf-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="29ccf-193">URL-Umleitung an einen sicheren Endpunkt</span><span class="sxs-lookup"><span data-stu-id="29ccf-193">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="29ccf-194">Verwenden Sie `AddRedirectToHttps`, um HTTP-Anforderungen auf denselben Host und Pfad mithilfe von HTTPS (`https://`) umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="29ccf-195">Wenn kein Statuscode angegeben wird, wird für die Middleware standardmäßig „302 – Gefunden“ zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="29ccf-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="29ccf-196">Wenn kein Port angegeben wird, wird für die Middleware standardmäßig `null` zurückzugeben. Das heißt, das Protokoll ändert sich in `https://`, und der Client greift auf die Ressource auf Port 443 zu.</span><span class="sxs-lookup"><span data-stu-id="29ccf-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="29ccf-197">Im Beispiel wird dargestellt, wie Sie den Statuscode „301 – Permanent verschoben“ festlegen und den Port in 5001 ändern.</span><span class="sxs-lookup"><span data-stu-id="29ccf-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="29ccf-198">Verwenden Sie `AddRedirectToHttpsPermanent`, um unsichere Anforderungen auf denselben Host und Pfad mit einem sicheren HTTPS-Protokoll (`https://` auf Port 443) umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="29ccf-199">Die Middleware legt den Statuscode auf „301 – Permanent verschoben“ fest.</span><span class="sxs-lookup"><span data-stu-id="29ccf-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="29ccf-200">Wenn Sie eine Umleitung auf HTTPS ohne weitere erforderliche Umleitungsregeln durchführen, wird empfohlen, Middleware für HTTPS-Umleitung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-200">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="29ccf-201">Weitere Informationen finden Sie im Artikel zum [Erzwingen von HTTPS](xref:security/enforcing-ssl#require-https).</span><span class="sxs-lookup"><span data-stu-id="29ccf-201">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="29ccf-202">Anhand der Beispiel-App wird veranschaulicht, wie `AddRedirectToHttps` oder `AddRedirectToHttpsPermanent` verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-202">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="29ccf-203">Fügen Sie die Erweiterungsmethode zu den `RewriteOptions` hinzu.</span><span class="sxs-lookup"><span data-stu-id="29ccf-203">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="29ccf-204">Senden Sie eine unsichere Anforderung an die App unter einer beliebigen URL.</span><span class="sxs-lookup"><span data-stu-id="29ccf-204">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="29ccf-205">Schließen Sie die Sicherheitswarnung des Browsers, in der Sie darüber informiert werden, dass das selbstsignierte Zertifikat nicht vertrauenswürdig ist, oder erstellen sie eine Ausnahme, um dem Zertifikat zu vertrauen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-205">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="29ccf-206">Ursprüngliche Anforderung über `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="29ccf-206">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="29ccf-208">Ursprüngliche Anforderung über `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="29ccf-208">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="29ccf-210">Umschreiben einer URL</span><span class="sxs-lookup"><span data-stu-id="29ccf-210">URL rewrite</span></span>

<span data-ttu-id="29ccf-211">Erstellen Sie mithilfe von `AddRewrite` eine Regel zum Umschreiben von URLs.</span><span class="sxs-lookup"><span data-stu-id="29ccf-211">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="29ccf-212">Der erste Parameter enthält Ihren RegEx, damit dieser der eingehenden URL zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="29ccf-212">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="29ccf-213">Beim zweiten Parameter handelt es sich um eine Ersatzzeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="29ccf-213">The second parameter is the replacement string.</span></span> <span data-ttu-id="29ccf-214">Der dritte Parameter (`skipRemainingRules: {true|false}`) teilt der Middleware mit, ob sie zusätzliche Umschreibungsregeln überspringen soll, wenn die aktuelle Regel angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-214">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-217">Ursprüngliche Anforderung: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="29ccf-217">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderung und Antwort nachverfolgen](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="29ccf-219">Im RegEx fällt besonders das Zirkumflexzeichen (`^`) am Anfang des Ausdrucks auf.</span><span class="sxs-lookup"><span data-stu-id="29ccf-219">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="29ccf-220">Das bedeutet, dass die Übereinstimmung schon am Anfang des URL-Pfads beginnt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-220">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="29ccf-221">Im zuvor genannten Beispiel zur Umleitungsregel (`redirect-rule/(.*)`) gibt es kein Zirkumflexzeichen am Anfang des RegEx. Daher kann jedes beliebige Zeichen vor `redirect-rule/` im Pfad stehen, damit es zu einer Übereinstimmung kommt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-221">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="29ccf-222">Pfad</span><span class="sxs-lookup"><span data-stu-id="29ccf-222">Path</span></span>                               | <span data-ttu-id="29ccf-223">Match</span><span class="sxs-lookup"><span data-stu-id="29ccf-223">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="29ccf-224">Ja</span><span class="sxs-lookup"><span data-stu-id="29ccf-224">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="29ccf-225">Ja</span><span class="sxs-lookup"><span data-stu-id="29ccf-225">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="29ccf-226">Ja</span><span class="sxs-lookup"><span data-stu-id="29ccf-226">Yes</span></span>   |

<span data-ttu-id="29ccf-227">Die Umschreibungsregel (`^rewrite-rule/(\d+)/(\d+)`) stimmt nur mit Pfaden überein, wenn sie mit `rewrite-rule/` beginnen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-227">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="29ccf-228">Beachten Sie die unterschiedlichen Übereinstimmungen zwischen der Umschreibungsregel und der Umleitungsregel.</span><span class="sxs-lookup"><span data-stu-id="29ccf-228">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="29ccf-229">Pfad</span><span class="sxs-lookup"><span data-stu-id="29ccf-229">Path</span></span>                              | <span data-ttu-id="29ccf-230">Match</span><span class="sxs-lookup"><span data-stu-id="29ccf-230">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="29ccf-231">Ja</span><span class="sxs-lookup"><span data-stu-id="29ccf-231">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="29ccf-232">Nein</span><span class="sxs-lookup"><span data-stu-id="29ccf-232">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="29ccf-233">Nein</span><span class="sxs-lookup"><span data-stu-id="29ccf-233">No</span></span>    |

<span data-ttu-id="29ccf-234">Auf den `^rewrite-rule/`-Teil des Ausdruck folgen zwei Erfassungsgruppen: `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="29ccf-234">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="29ccf-235">`\d` steht für *Übereinstimmung mit einer Ziffer (Zahl)*.</span><span class="sxs-lookup"><span data-stu-id="29ccf-235">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="29ccf-236">Das Pluszeichen (`+`) steht für *match one or more of the preceding character* (Übereinstimmung mit mindestens einem vorausgehenden Zeichen).</span><span class="sxs-lookup"><span data-stu-id="29ccf-236">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="29ccf-237">Aus diesem Grund muss die URL eine Zahl enthalten, auf die ein Schrägstrich und eine weitere Zahl folgt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-237">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="29ccf-238">Die Erfassungsgruppen werden in die umgeschriebene URL als `$1` und `$2` eingefügt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-238">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="29ccf-239">Über die Ersetzungszeichenfolge der Umschreibungsregel werden die Erfassungsgruppen in die Abfragezeichenfolge eingefügt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-239">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="29ccf-240">Der angeforderte `/rewrite-rule/1234/5678`-Pfad wird umgeschrieben, um eine Ressource unter `/rewritten?var1=1234&var2=5678` abzurufen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-240">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="29ccf-241">Wenn es in der ursprünglichen Abfrage eine Abfragezeichenfolge gibt, bleibt sie erhalten, wenn die URL umgeschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-241">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="29ccf-242">Es gibt keinen Roundtrip zum Server, um die Ressource abzurufen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-242">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="29ccf-243">Wenn es die Ressource gibt, wird sie abgerufen und dem Client mit dem Statuscode „200– OK“ zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="29ccf-243">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="29ccf-244">Da der Client nicht umgeleitet wird, ändert sich die URL in der Adressleiste des Browsers nicht.</span><span class="sxs-lookup"><span data-stu-id="29ccf-244">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="29ccf-245">Für den Client hat der URL-Umschreibungsvorgang nie stattgefunden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-245">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="29ccf-246">Verwenden Sie `skipRemainingRules: true` so häufig wie möglich, da das Abgleichen von Regeln sehr umfangreich ist und die Antwortzeit der App verringert.</span><span class="sxs-lookup"><span data-stu-id="29ccf-246">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="29ccf-247">Führen Sie folgende Schritte aus, um die schnellsten App-Antwortzeiten zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="29ccf-247">For the fastest app response:</span></span>
> * <span data-ttu-id="29ccf-248">Sortieren Sie Ihre Umschreibungsregeln von der am häufigsten abgeglichenen Regel zu der am seltensten abgeglichenen Regel.</span><span class="sxs-lookup"><span data-stu-id="29ccf-248">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="29ccf-249">Überspringen Sie die Verarbeitung der übrigen Regeln, wenn es zu einer Übereinstimmung kommt und keine zusätzlichen Regelverarbeitungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="29ccf-249">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="29ccf-250">Apache: „mod_rewrite“</span><span class="sxs-lookup"><span data-stu-id="29ccf-250">Apache mod_rewrite</span></span>

<span data-ttu-id="29ccf-251">Wenden Sie die Apache-Regeln „mod_rewrite“ mit `AddApacheModRewrite` an.</span><span class="sxs-lookup"><span data-stu-id="29ccf-251">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="29ccf-252">Vergewissern Sie sich, dass die Regeldatei mit der App bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-252">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="29ccf-253">Weitere Informationen zu diesen Regeln finden Sie unter [Apache: „mod_rewrite“](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="29ccf-253">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="29ccf-255">Zum Lesen der Regeldatei *ApacheModRewrite.txt* wird `StreamReader` verwendet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-255">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-256">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-256">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="29ccf-257">Der erste Parameter verwendet eine `IFileProvider`-Schnittstelle, die über [Dependency Injection](dependency-injection.md) bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-257">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="29ccf-258">`IHostingEnvironment` wird eingefügt, um `ContentRootFileProvider` bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-258">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="29ccf-259">Der zweite Parameter steht für den Pfad Ihrer Regeldatei (*ApacheModRewrite.txt* in der Beispiel-App).</span><span class="sxs-lookup"><span data-stu-id="29ccf-259">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-260">Die Beispiel-App leitet Anforderungen von `/apache-mod-rules-redirect/(.\*)` auf `/redirected?id=$1` um.</span><span class="sxs-lookup"><span data-stu-id="29ccf-260">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="29ccf-261">Der Antwortstatuscode lautet „302 – Gefunden“.</span><span class="sxs-lookup"><span data-stu-id="29ccf-261">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="29ccf-262">Ursprüngliche Anforderung: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="29ccf-262">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="29ccf-264">Unterstützte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="29ccf-264">Supported server variables</span></span>

<span data-ttu-id="29ccf-265">Die Middleware unterstützt die folgenden Servervariablen für die Apache „mod_rewrite“:</span><span class="sxs-lookup"><span data-stu-id="29ccf-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="29ccf-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="29ccf-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="29ccf-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="29ccf-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="29ccf-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="29ccf-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="29ccf-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="29ccf-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="29ccf-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="29ccf-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="29ccf-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="29ccf-271">HTTP_HOST</span></span>
* <span data-ttu-id="29ccf-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="29ccf-272">HTTP_REFERER</span></span>
* <span data-ttu-id="29ccf-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="29ccf-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="29ccf-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="29ccf-274">HTTPS</span></span>
* <span data-ttu-id="29ccf-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="29ccf-275">IPV6</span></span>
* <span data-ttu-id="29ccf-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="29ccf-276">QUERY_STRING</span></span>
* <span data-ttu-id="29ccf-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="29ccf-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="29ccf-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="29ccf-278">REMOTE_PORT</span></span>
* <span data-ttu-id="29ccf-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="29ccf-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="29ccf-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="29ccf-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="29ccf-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="29ccf-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="29ccf-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="29ccf-282">REQUEST_URI</span></span>
* <span data-ttu-id="29ccf-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="29ccf-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="29ccf-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="29ccf-284">SERVER_ADDR</span></span>
* <span data-ttu-id="29ccf-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="29ccf-285">SERVER_PORT</span></span>
* <span data-ttu-id="29ccf-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="29ccf-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="29ccf-287">TIME</span><span class="sxs-lookup"><span data-stu-id="29ccf-287">TIME</span></span>
* <span data-ttu-id="29ccf-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="29ccf-288">TIME_DAY</span></span>
* <span data-ttu-id="29ccf-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="29ccf-289">TIME_HOUR</span></span>
* <span data-ttu-id="29ccf-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="29ccf-290">TIME_MIN</span></span>
* <span data-ttu-id="29ccf-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="29ccf-291">TIME_MON</span></span>
* <span data-ttu-id="29ccf-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="29ccf-292">TIME_SEC</span></span>
* <span data-ttu-id="29ccf-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="29ccf-293">TIME_WDAY</span></span>
* <span data-ttu-id="29ccf-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="29ccf-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="29ccf-295">Regeln zum IIS-Umschreibungsmodul</span><span class="sxs-lookup"><span data-stu-id="29ccf-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="29ccf-296">Verwenden Sie `AddIISUrlRewrite`, um die Regeln zu verwenden, die für das IIS-Umschreibungsmodul gelten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="29ccf-297">Vergewissern Sie sich, dass die Regeldatei mit der App bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="29ccf-298">Geben Sie Ihrer Middleware nicht die Anweisung, die *web.config*-Datei zu verwenden, wenn sie auf der Windows Server-IIS ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="29ccf-299">Die Regeln sollten mit IIS außerhalb der *web.config*-Datei gespeichert werden, um Konflikte mit dem IIS-Umschreibungsmodul zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="29ccf-300">Weitere Informationen und Beispiele zu den Regeln zum IIS-Umschreibungsmodul finden Sie unter [Using Url Rewrite Module 2.0 (Verwenden des URL-Umschreibungsmoduls 2.0)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) und [URL Rewrite Module Configuration Reference (Konfigurationsreferenz des URL-Umschreibungsmoduls)](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="29ccf-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="29ccf-302">Zum Lesen der Regeldatei *IISUrlRewrite.xml* wird `StreamReader` verwendet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-302">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="29ccf-304">Der erste Parameter verwendet eine `IFileProvider`-Schnittstelle, wohingegen es sich bei dem zweiten Parameter um einen Pfad zu einer XML-Regeldatei handelt (*IISUrlRewrite.xml* in der Beispiel-App).</span><span class="sxs-lookup"><span data-stu-id="29ccf-304">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-305">Die Beispiel-App schreibt Anforderungen von `/iis-rules-rewrite/(.*)` in `/rewritten?id=$1` um.</span><span class="sxs-lookup"><span data-stu-id="29ccf-305">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="29ccf-306">Die Antwort wird an den Client mit dem Statuscode „200 – OK“ gesendet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-306">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="29ccf-307">Ursprüngliche Anforderung: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="29ccf-307">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderung und Antwort nachverfolgen](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="29ccf-309">Wenn Sie über ein aktives IIS-Umschreibungsmodul verfügen, für das die Regeln auf Serverebene konfiguriert sind, die negative Auswirkungen auf Ihre App hätten, können Sie das IIS-Umschreibungsmodul für die App deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="29ccf-309">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="29ccf-310">Weitere Informationen finden Sie unter [Disabling IIS modules (Deaktivieren von IIS-Modulen)](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="29ccf-310">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="29ccf-311">Nicht unterstützte Features</span><span class="sxs-lookup"><span data-stu-id="29ccf-311">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="29ccf-313">Die im Lieferumfang von ASP.NET Core 2.x enthaltene Middleware unterstützt die folgenden Features des IIS-URL-Umschreibungsmoduls nicht:</span><span class="sxs-lookup"><span data-stu-id="29ccf-313">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="29ccf-314">Ausgehende Regeln</span><span class="sxs-lookup"><span data-stu-id="29ccf-314">Outbound Rules</span></span>
* <span data-ttu-id="29ccf-315">Benutzerdefinierte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="29ccf-315">Custom Server Variables</span></span>
* <span data-ttu-id="29ccf-316">Platzhalter</span><span class="sxs-lookup"><span data-stu-id="29ccf-316">Wildcards</span></span>
* <span data-ttu-id="29ccf-317">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="29ccf-317">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="29ccf-319">Die im Lieferumfang von ASP.NET Core 1.x enthaltene Middleware unterstützt die folgenden Features des IIS-URL-Umschreibungsmoduls nicht:</span><span class="sxs-lookup"><span data-stu-id="29ccf-319">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="29ccf-320">Globale Regeln</span><span class="sxs-lookup"><span data-stu-id="29ccf-320">Global Rules</span></span>
* <span data-ttu-id="29ccf-321">Ausgehende Regeln</span><span class="sxs-lookup"><span data-stu-id="29ccf-321">Outbound Rules</span></span>
* <span data-ttu-id="29ccf-322">Umschreibungszuordnungen</span><span class="sxs-lookup"><span data-stu-id="29ccf-322">Rewrite Maps</span></span>
* <span data-ttu-id="29ccf-323">Benutzerdefinierte Antwortaktionen</span><span class="sxs-lookup"><span data-stu-id="29ccf-323">CustomResponse Action</span></span>
* <span data-ttu-id="29ccf-324">Benutzerdefinierte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="29ccf-324">Custom Server Variables</span></span>
* <span data-ttu-id="29ccf-325">Platzhalter</span><span class="sxs-lookup"><span data-stu-id="29ccf-325">Wildcards</span></span>
* <span data-ttu-id="29ccf-326">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="29ccf-326">Action:CustomResponse</span></span>
* <span data-ttu-id="29ccf-327">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="29ccf-327">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="29ccf-328">Unterstützte Servervariablen</span><span class="sxs-lookup"><span data-stu-id="29ccf-328">Supported server variables</span></span>

<span data-ttu-id="29ccf-329">Die Middleware unterstützt die folgenden Servervariablen für das IIS-URL-Umschreibungsmodul:</span><span class="sxs-lookup"><span data-stu-id="29ccf-329">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="29ccf-330">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="29ccf-330">CONTENT_LENGTH</span></span>
* <span data-ttu-id="29ccf-331">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="29ccf-331">CONTENT_TYPE</span></span>
* <span data-ttu-id="29ccf-332">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="29ccf-332">HTTP_ACCEPT</span></span>
* <span data-ttu-id="29ccf-333">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="29ccf-333">HTTP_CONNECTION</span></span>
* <span data-ttu-id="29ccf-334">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="29ccf-334">HTTP_COOKIE</span></span>
* <span data-ttu-id="29ccf-335">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="29ccf-335">HTTP_HOST</span></span>
* <span data-ttu-id="29ccf-336">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="29ccf-336">HTTP_REFERER</span></span>
* <span data-ttu-id="29ccf-337">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="29ccf-337">HTTP_URL</span></span>
* <span data-ttu-id="29ccf-338">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="29ccf-338">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="29ccf-339">HTTPS</span><span class="sxs-lookup"><span data-stu-id="29ccf-339">HTTPS</span></span>
* <span data-ttu-id="29ccf-340">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="29ccf-340">LOCAL_ADDR</span></span>
* <span data-ttu-id="29ccf-341">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="29ccf-341">QUERY_STRING</span></span>
* <span data-ttu-id="29ccf-342">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="29ccf-342">REMOTE_ADDR</span></span>
* <span data-ttu-id="29ccf-343">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="29ccf-343">REMOTE_PORT</span></span>
* <span data-ttu-id="29ccf-344">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="29ccf-344">REQUEST_FILENAME</span></span>
* <span data-ttu-id="29ccf-345">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="29ccf-345">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="29ccf-346">Sie können `IFileProvider` auch über `PhysicalFileProvider` abrufen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-346">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="29ccf-347">Über diesen Ansatz können Sie flexibler einen Speicherort für die Dateien der Umschreibungsregeln auswählen.</span><span class="sxs-lookup"><span data-stu-id="29ccf-347">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="29ccf-348">Vergewissern Sie sich, dass die Dateien zu den Umschreibungsregeln auf dem Server unter dem von Ihnen angegebenen Pfad bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-348">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="29ccf-349">Methodenbasierte Regel</span><span class="sxs-lookup"><span data-stu-id="29ccf-349">Method-based rule</span></span>

<span data-ttu-id="29ccf-350">Verwenden Sie `Add(Action<RewriteContext> applyRule)`, um Ihre eigene Regellogik in einer Methode zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="29ccf-350">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="29ccf-351">Über `RewriteContext` können Sie `HttpContext` in Ihrer Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-351">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="29ccf-352">`context.Result` bestimmt, wie die zusätzliche Pipelineverarbeitung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-352">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="29ccf-353">context.Result</span><span class="sxs-lookup"><span data-stu-id="29ccf-353">context.Result</span></span>                       | <span data-ttu-id="29ccf-354">Aktion</span><span class="sxs-lookup"><span data-stu-id="29ccf-354">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="29ccf-355">`RuleResult.ContinueRules` (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="29ccf-355">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="29ccf-356">Regeln weiter anwenden</span><span class="sxs-lookup"><span data-stu-id="29ccf-356">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="29ccf-357">Regeln nicht mehr anwenden und Antwort senden</span><span class="sxs-lookup"><span data-stu-id="29ccf-357">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="29ccf-358">Regeln nicht mehr anwenden, und den Kontext an die nächste Middleware senden</span><span class="sxs-lookup"><span data-stu-id="29ccf-358">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-360">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-360">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-361">In der Beispiel-App wird eine Methode dargestellt, die Anforderungen für Pfade weiterleitet, die auf *.xml* enden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-361">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="29ccf-362">Wenn Sie eine Anforderung an `/file.xml` senden, wird diese an `/xmlfiles/file.xml` weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-362">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="29ccf-363">Der Statuscode wird auf „301 – Permanent verschoben“ festgelegt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-363">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="29ccf-364">Sie müssen für eine Umleitung den Statuscode auf eine genaue Antwort festlegen. Ansonsten wird der Statuscode „200 – OK“ zurückgegeben und es wird keine Umleitung zum Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="29ccf-364">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="29ccf-365">Ursprüngliche Anforderung: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="29ccf-365">Original Request: `/file.xml`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der file.xml-Datei nachverfolgen](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="29ccf-367">IRule-basierte Regel</span><span class="sxs-lookup"><span data-stu-id="29ccf-367">IRule-based rule</span></span>

<span data-ttu-id="29ccf-368">Verwenden Sie `Add(IRule)`, um Ihre eigene Regellogik in eine Klasse zu implementieren, die von `IRule` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="29ccf-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="29ccf-369">Wenn Sie `IRule` verwenden, können Sie flexibler entscheiden, ob Sie eine methodenbasierte Regel verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="29ccf-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="29ccf-370">Ihre abgeleitete Klasse kann an den Stellen einen Konstruktor enthalten, an denen Sie Parameter für die `ApplyRule`-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="29ccf-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29ccf-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29ccf-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29ccf-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="29ccf-373">Die Werte für die Parameter in der Beispiel-App für `extension` und `newPath` werden auf verschiedene Bedingungen geprüft.</span><span class="sxs-lookup"><span data-stu-id="29ccf-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="29ccf-374">`extension` muss einen Wert enthalten, der *.png*, *.jpg* oder *.gif* entspricht.</span><span class="sxs-lookup"><span data-stu-id="29ccf-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="29ccf-375">Wenn `newPath` nicht gültig ist, wird `ArgumentException` nicht ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="29ccf-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="29ccf-376">Wenn Sie eine Anforderung an *image.png* senden, wird diese an `/png-images/image.png` weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="29ccf-377">Wenn Sie eine Anforderung an *image.jpg* senden, wird diese an `/jpg-images/image.jpg` weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="29ccf-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="29ccf-378">Der Statuscode ist auf „301 – Permanent verschoben“ festgelegt, und `context.Result` erhält die Anweisung, Verarbeitungsregeln nicht mehr anzuwenden und Antworten zu senden.</span><span class="sxs-lookup"><span data-stu-id="29ccf-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="29ccf-379">Ursprüngliche Anforderung: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="29ccf-379">Original Request: `/image.png`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der image.png-Datei nachverfolgen](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="29ccf-381">Ursprüngliche Anforderung: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="29ccf-381">Original Request: `/image.jpg`</span></span>

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der image.jpg-Datei nachverfolgen](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="29ccf-383">RegEx-Beispiele</span><span class="sxs-lookup"><span data-stu-id="29ccf-383">Regex examples</span></span>

| <span data-ttu-id="29ccf-384">Ziel</span><span class="sxs-lookup"><span data-stu-id="29ccf-384">Goal</span></span> | <span data-ttu-id="29ccf-385">RegEx-Zeichenfolge und</span><span class="sxs-lookup"><span data-stu-id="29ccf-385">Regex String &</span></span><br><span data-ttu-id="29ccf-386">übereinstimmendes Beispiel</span><span class="sxs-lookup"><span data-stu-id="29ccf-386">Match Example</span></span> | <span data-ttu-id="29ccf-387">Ersetzungszeichenfolge und</span><span class="sxs-lookup"><span data-stu-id="29ccf-387">Replacement String &</span></span><br><span data-ttu-id="29ccf-388">Ausgabebeispiel</span><span class="sxs-lookup"><span data-stu-id="29ccf-388">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="29ccf-389">Umschreiben des Pfads in die Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="29ccf-389">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="29ccf-390">Entfernen des nachgestellten Schrägstrichs</span><span class="sxs-lookup"><span data-stu-id="29ccf-390">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="29ccf-391">Erzwingen des nachgestellten Schrägstrichs</span><span class="sxs-lookup"><span data-stu-id="29ccf-391">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="29ccf-392">Vermeiden des Umschreibens von bestimmten Anforderungen</span><span class="sxs-lookup"><span data-stu-id="29ccf-392">Avoid rewriting specific requests</span></span> | <span data-ttu-id="29ccf-393">`^(.*)(?<!\.axd)$` oder `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="29ccf-393">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="29ccf-394">Ja: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="29ccf-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="29ccf-395">Nein: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="29ccf-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="29ccf-396">Ändern der Anordnung von URL-Segmenten</span><span class="sxs-lookup"><span data-stu-id="29ccf-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="29ccf-397">Ersetzen von URL-Segmenten</span><span class="sxs-lookup"><span data-stu-id="29ccf-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="29ccf-398">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="29ccf-398">Additional resources</span></span>

* [<span data-ttu-id="29ccf-399">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="29ccf-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="29ccf-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="29ccf-400">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="29ccf-401">Reguläre Ausdrücke in .NET</span><span class="sxs-lookup"><span data-stu-id="29ccf-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="29ccf-402">Sprachelemente für reguläre Ausdrücke – Kurzübersicht</span><span class="sxs-lookup"><span data-stu-id="29ccf-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="29ccf-403">Apache: „mod_rewrite“</span><span class="sxs-lookup"><span data-stu-id="29ccf-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="29ccf-404">Using Url Rewrite Module 2.0 (for IIS) (Verwenden des URL-Umschreibungsmoduls 2.0 (für IIS))</span><span class="sxs-lookup"><span data-stu-id="29ccf-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="29ccf-405">URL Rewrite Module Configuration Reference (Konfigurationsreferenz für das URL-Umschreibungsmodul)</span><span class="sxs-lookup"><span data-stu-id="29ccf-405">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="29ccf-406">Forum zum IIS-URL-Umschreibungsmodul</span><span class="sxs-lookup"><span data-stu-id="29ccf-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="29ccf-407">Einfache Strukturierung von URLs</span><span class="sxs-lookup"><span data-stu-id="29ccf-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="29ccf-408">10 URL Rewriting Tips and Tricks (10 Tipps zum Umschreiben von URL)</span><span class="sxs-lookup"><span data-stu-id="29ccf-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="29ccf-409">To slash or not to slash (Schrägstriche setzen oder nicht setzen)</span><span class="sxs-lookup"><span data-stu-id="29ccf-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
