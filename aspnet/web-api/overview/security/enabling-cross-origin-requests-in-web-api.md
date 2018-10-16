---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Aktivieren von ursprungsübergreifenden Anforderungen in ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie zur Unterstützung von Cross-Origin Resource Sharing (CORS) in ASP.NET Web-API.
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348519"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="1858f-103">Aktivieren Sie ursprungsübergreifender Anforderungen in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1858f-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1858f-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1858f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1858f-105">Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen in eine andere Domäne.</span><span class="sxs-lookup"><span data-stu-id="1858f-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="1858f-106">Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*, und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest.</span><span class="sxs-lookup"><span data-stu-id="1858f-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="1858f-107">Jedoch sollten Sie manchmal auf andere Standorte Ihrer Web-API aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="1858f-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="1858f-108">[Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1858f-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="1858f-109">Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.</span><span class="sxs-lookup"><span data-stu-id="1858f-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="1858f-110">CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="1858f-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="1858f-111">In diesem Tutorial veranschaulicht das Aktivieren von CORS in Ihrer Web-API-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1858f-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="1858f-112">In diesem Tutorial verwendete Software</span><span class="sxs-lookup"><span data-stu-id="1858f-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="1858f-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1858f-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="1858f-114">Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="1858f-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="1858f-115">Einführung</span><span class="sxs-lookup"><span data-stu-id="1858f-115">Introduction</span></span>

<span data-ttu-id="1858f-116">In diesem Tutorial wird veranschaulicht, dass CORS-Unterstützung in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="1858f-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="1858f-117">Wir beginnen, erstellen Sie zwei ASP.NET-Projekte – einen namens "WebService", hostet einen Web-API-Controller, und die anderen namens "" Webclient "" der Webdienst aufruft.</span><span class="sxs-lookup"><span data-stu-id="1858f-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="1858f-118">Da die beiden Anwendungen auf verschiedenen Domänen gehostet werden, ist eine AJAX-Anforderung von "Webclient" auf den Webdienst eine cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="1858f-119">Was ist "denselben Ursprung"?</span><span class="sxs-lookup"><span data-stu-id="1858f-119">What is "same origin"?</span></span>

<span data-ttu-id="1858f-120">Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports.</span><span class="sxs-lookup"><span data-stu-id="1858f-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="1858f-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="1858f-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="1858f-122">Diese zwei URLs haben denselben Ursprung an:</span><span class="sxs-lookup"><span data-stu-id="1858f-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="1858f-123">Diese URLs haben verschiedene Ursprünge als die vorherige zwei:</span><span class="sxs-lookup"><span data-stu-id="1858f-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="1858f-124">`http://example.net` -Andere Domäne</span><span class="sxs-lookup"><span data-stu-id="1858f-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="1858f-125">`http://example.com:9000/foo.html` -Portnummer</span><span class="sxs-lookup"><span data-stu-id="1858f-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="1858f-126">`https://example.com/foo.html` -Andere Partitionsschema</span><span class="sxs-lookup"><span data-stu-id="1858f-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="1858f-127">`http://www.example.com/foo.html` -Andere Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="1858f-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="1858f-128">Internet Explorer wird den Port nicht berücksichtigt, für den Vergleich Ursprünge.</span><span class="sxs-lookup"><span data-stu-id="1858f-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="1858f-129">Erstellen Sie das WebService-Projekt</span><span class="sxs-lookup"><span data-stu-id="1858f-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="1858f-130">In diesem Abschnitt wird davon ausgegangen, dass Sie bereits wissen, wie Web-API-Projekte erstellen.</span><span class="sxs-lookup"><span data-stu-id="1858f-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="1858f-131">Falls nicht, siehe [erste Schritte mit ASP.NET Web-API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1858f-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="1858f-132">Starten Sie Visual Studio, und erstellen Sie ein neues **ASP.NET-Webanwendung ((.NET Framework)** Projekt.</span><span class="sxs-lookup"><span data-stu-id="1858f-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="1858f-133">In der **neue ASP.NET-Webanwendung** wählen Sie im Dialogfeld die **leere** Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="1858f-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="1858f-134">Klicken Sie unter **fügen Sie Ordner und kernreferenzen für**, wählen die **Web-API-** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="1858f-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Dialogfeld "neue ASP.NET Projekt" in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="1858f-136">Fügen Sie einen Web-API-Controller mit dem Namen `TestController` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1858f-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="1858f-137">Sie können die Anwendung lokal ausführen oder in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1858f-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="1858f-138">(Die Screenshots in diesem Tutorial wird die app in Azure App Service Web Apps bereitgestellt.) Navigieren Sie zu, um sicherzustellen, dass die Web-API arbeitet, `http://hostname/api/test/`, wobei *Hostname* ist die Domäne, in dem Sie die Anwendung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1858f-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="1858f-139">Daraufhin sollte der Antworttext, &quot;abrufen: Testnachricht&quot;.</span><span class="sxs-lookup"><span data-stu-id="1858f-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Internet-Browser mit Test-Nachricht](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="1858f-141">Erstellen Sie das Projekt "Webclient"</span><span class="sxs-lookup"><span data-stu-id="1858f-141">Create the WebClient project</span></span>

1. <span data-ttu-id="1858f-142">Erstellen Sie eine weitere **ASP.NET-Webanwendung ((.NET Framework)** Projekt, und wählen die **MVC** Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="1858f-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="1858f-143">Wählen Sie optional **Authentifizierung ändern** > **keine Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="1858f-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="1858f-144">Für dieses Tutorial benötigen Sie keine Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="1858f-144">You don't need authentication for this tutorial.</span></span>

   ![MVC-Vorlage im Dialogfeld für neues ASP.NET-Projekt in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="1858f-146">In **Projektmappen-Explorer**, öffnen Sie die Datei *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1858f-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="1858f-147">Ersetzen Sie den Code in dieser Datei durch Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="1858f-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="1858f-148">Für die *ServiceUrl* Variable, verwenden Sie den URI der WebService-app.</span><span class="sxs-lookup"><span data-stu-id="1858f-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="1858f-149">Führen Sie den WebClient-app lokal oder in einer anderen Website veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1858f-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="1858f-150">Wenn Sie die Schaltfläche "Ausprobieren" klicken, wird eine AJAX-Anforderung übermittelt, für die WebService-app, die mit der HTTP-Methode aufgeführt, die der Dropdownliste (GET, POST oder PUT).</span><span class="sxs-lookup"><span data-stu-id="1858f-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="1858f-151">Dadurch können Sie die verschiedene Cross-Origin-Anforderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="1858f-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="1858f-152">Die WebService-app unterstützt derzeit keine CORS daher, wenn Sie auf die Schaltfläche klicken müssen Sie eine Fehlermeldung erhalten werden.</span><span class="sxs-lookup"><span data-stu-id="1858f-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Fehler "Ausprobieren", im browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="1858f-154">Wenn Sie sehen Sie sich den HTTP-Datenverkehr in einem Tool wie [Fiddler](http://www.telerik.com/fiddler), sehen Sie, dass der Browser die GET-Anforderung sendet, und die Anforderung erfolgreich ist, aber der AJAX-Aufruf gibt einen Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="1858f-154">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="1858f-155">Es ist wichtig zu verstehen, dass die Richtlinie desselben Ursprungs nicht durch den Browser verhindert *senden* der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="1858f-156">Stattdessen es verhindert, dass die Anwendung sehen die *Antwort*.</span><span class="sxs-lookup"><span data-stu-id="1858f-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Fiddler-webdebugger, die webanforderungen anzeigen](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="1858f-158">Aktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="1858f-158">Enable CORS</span></span>

<span data-ttu-id="1858f-159">Jetzt aktivieren wir CORS in der WebService-app.</span><span class="sxs-lookup"><span data-stu-id="1858f-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="1858f-160">Fügen Sie zunächst das CORS-NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="1858f-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="1858f-161">In Visual Studio aus der **Tools** die Option **NuGet Paket-Manager**, wählen Sie **Paket-Manager Konsole**.</span><span class="sxs-lookup"><span data-stu-id="1858f-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1858f-162">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1858f-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="1858f-163">Dieser Befehl installiert das neueste Paket und alle Abhängigkeiten, einschließlich der Core-Web-API-Bibliotheken aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1858f-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="1858f-164">Verwenden der `-Version` Flag, um eine bestimmte Version abzielen.</span><span class="sxs-lookup"><span data-stu-id="1858f-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="1858f-165">Das CORS-Paket ist die Web-API 2.0 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1858f-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="1858f-166">Öffnen Sie die Datei *App\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="1858f-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="1858f-167">Fügen Sie den folgenden Code der **WebApiConfig.Register** Methode:</span><span class="sxs-lookup"><span data-stu-id="1858f-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="1858f-168">Fügen Sie als Nächstes die **[EnableCors]** -Attribut auf die `TestController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="1858f-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="1858f-169">Für die *Ursprünge* -Parameter verwenden Sie den URI, wo Sie die Anwendung "Webclient" bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1858f-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="1858f-170">Dies erlaubt Cross-Origin-Anfragen von "Webclient" untersagen weiterhin auf alle anderen domänenübergreifende Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="1858f-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="1858f-171">Ich beschreibe später die Parameter für **[EnableCors]** im Detail.</span><span class="sxs-lookup"><span data-stu-id="1858f-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="1858f-172">Schließen Sie keinen Schrägstrich am Ende der *Ursprünge* URL.</span><span class="sxs-lookup"><span data-stu-id="1858f-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="1858f-173">Bereitstellen Sie die aktualisierte WebService-Anwendung erneut.</span><span class="sxs-lookup"><span data-stu-id="1858f-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="1858f-174">Sie müssen nicht "Webclient" zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="1858f-174">You don't need to update WebClient.</span></span> <span data-ttu-id="1858f-175">Die AJAX-Anforderung von "Webclient" sollte jetzt erfolgreich sein.</span><span class="sxs-lookup"><span data-stu-id="1858f-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="1858f-176">Die GET, PUT und POST-Methoden sind alle zulässig.</span><span class="sxs-lookup"><span data-stu-id="1858f-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Browser mit erfolgreichen Test Webnachricht](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="1858f-178">Funktionsweise von CORS</span><span class="sxs-lookup"><span data-stu-id="1858f-178">How CORS Works</span></span>

<span data-ttu-id="1858f-179">In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="1858f-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="1858f-180">Es ist wichtig zum Verständnis der Funktionsweise von CORS, sodass Sie konfigurieren können die **[EnableCors]** ordnungsgemäß Attribut, und beheben, wenn etwas nicht wie erwartet funktioniert.</span><span class="sxs-lookup"><span data-stu-id="1858f-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="1858f-181">CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1858f-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="1858f-182">Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen; Sie müssen gar nichts Besonderes im JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="1858f-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="1858f-183">Hier ist ein Beispiel für eine cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="1858f-184">Der "Origin"-Header gibt die Domäne der Website, die die Anforderung stammt.</span><span class="sxs-lookup"><span data-stu-id="1858f-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="1858f-185">Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header.</span><span class="sxs-lookup"><span data-stu-id="1858f-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="1858f-186">Der Wert dieses Headers entspricht den Origin-Header oder ist Sie den Platzhalterwert "\*", Bedeutung, die einen beliebigen Ursprung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="1858f-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="1858f-187">Wenn es sich bei den Access-Control-Allow-Origin-Header in die Antwort nicht enthalten ist, schlägt die AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="1858f-188">Der Browser lässt, die die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="1858f-189">Selbst wenn der Server eine erfolgreiche Antwort zurückgibt, ist der Browser nicht die Antwort an die Clientanwendung zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1858f-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="1858f-190">**Preflight-Anforderungen**</span><span class="sxs-lookup"><span data-stu-id="1858f-190">**Preflight Requests**</span></span>

<span data-ttu-id="1858f-191">Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, die eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet.</span><span class="sxs-lookup"><span data-stu-id="1858f-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="1858f-192">Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="1858f-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="1858f-193">Die Anforderungsmethode ist GET, HEAD oder POST, *und*</span><span class="sxs-lookup"><span data-stu-id="1858f-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="1858f-194">Die Anwendung ist nicht festgelegt Anforderungsheader als Accept, Accept-Language, Inhaltssprache, Content-Type oder letzten-Ereignis-ID, *und*</span><span class="sxs-lookup"><span data-stu-id="1858f-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="1858f-195">Der Content-Type-Header (falls festgelegt) ist eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="1858f-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="1858f-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="1858f-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="1858f-197">Multipart/Form-data</span><span class="sxs-lookup"><span data-stu-id="1858f-197">multipart/form-data</span></span>
    - <span data-ttu-id="1858f-198">Text/plain</span><span class="sxs-lookup"><span data-stu-id="1858f-198">text/plain</span></span>

<span data-ttu-id="1858f-199">Die Regel zu Anforderungsheader gilt für Header, die die Anwendung durch Aufrufen von festlegt **SetRequestHeader** auf die **XMLHttpRequest** Objekt.</span><span class="sxs-lookup"><span data-stu-id="1858f-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="1858f-200">(Die CORS-Spezifikation als diese "Author-Anforderungsheader" bezeichnet). Die Regel gilt nicht für Header der *Browser* festlegen können, z. B. Benutzer-Agent, Host und Content-Length.</span><span class="sxs-lookup"><span data-stu-id="1858f-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="1858f-201">Hier ist ein Beispiel für eine preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="1858f-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="1858f-202">Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="1858f-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="1858f-203">Es enthält zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="1858f-203">It includes two special headers:</span></span>

- <span data-ttu-id="1858f-204">Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1858f-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="1858f-205">Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die *Anwendung* legen Sie für die tatsächliche Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="1858f-206">(In diesem Fall ist dies nicht-Header, die der Browser legt diese fest enthalten.)</span><span class="sxs-lookup"><span data-stu-id="1858f-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="1858f-207">Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:</span><span class="sxs-lookup"><span data-stu-id="1858f-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="1858f-208">Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgeführt, und optional eine Access-Control-Allow-Headers-Header, der die erlaubten Header aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="1858f-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="1858f-209">Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="1858f-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="1858f-210">Bereichsregeln für [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="1858f-210">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="1858f-211">Sie können CORS pro Aktion, pro Controller oder global für alle Web-API-Controller in Ihrer Anwendung ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1858f-211">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="1858f-212">**Pro Aktion**</span><span class="sxs-lookup"><span data-stu-id="1858f-212">**Per Action**</span></span>

<span data-ttu-id="1858f-213">Legen Sie zum Aktivieren von CORS für eine einzelne Aktion die **[EnableCors]** Attribut für die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="1858f-213">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="1858f-214">Im folgenden Beispiel wird CORS für den `GetItem` nur Methode.</span><span class="sxs-lookup"><span data-stu-id="1858f-214">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="1858f-215">**Pro Controller**</span><span class="sxs-lookup"><span data-stu-id="1858f-215">**Per Controller**</span></span>

<span data-ttu-id="1858f-216">Setzen Sie **[EnableCors]** auf die Controllerklasse, gilt für alle Aktionen auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="1858f-216">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="1858f-217">Wenn Sie CORS für eine Aktion deaktivieren möchten, fügen die **[DisableCors]** -Attribut auf die Aktion.</span><span class="sxs-lookup"><span data-stu-id="1858f-217">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="1858f-218">Im folgenden Beispiel wird CORS für jede Methode, mit Ausnahme von `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="1858f-218">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="1858f-219">**Global**</span><span class="sxs-lookup"><span data-stu-id="1858f-219">**Globally**</span></span>

<span data-ttu-id="1858f-220">Übergeben Sie zum Aktivieren von CORS für alle Web-API-Controller in Ihrer Anwendung eine **EnableCorsAttribute** -Instanz, auf die **EnableCors** Methode:</span><span class="sxs-lookup"><span data-stu-id="1858f-220">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="1858f-221">Wenn Sie das Attribut auf mehr als einen Bereich festlegen, ist die Reihenfolge auf:</span><span class="sxs-lookup"><span data-stu-id="1858f-221">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="1858f-222">Aktion</span><span class="sxs-lookup"><span data-stu-id="1858f-222">Action</span></span>
2. <span data-ttu-id="1858f-223">Controller</span><span class="sxs-lookup"><span data-stu-id="1858f-223">Controller</span></span>
3. <span data-ttu-id="1858f-224">Global</span><span class="sxs-lookup"><span data-stu-id="1858f-224">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="1858f-225">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="1858f-225">Set the allowed origins</span></span>

<span data-ttu-id="1858f-226">Die *Ursprünge* Parameter, der die **[EnableCors]** Attribut gibt an, welche Ursprünge zulässig sind, auf die Ressource zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1858f-226">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="1858f-227">Der Wert ist eine durch Trennzeichen getrennte Liste der zulässigen Ursprünge.</span><span class="sxs-lookup"><span data-stu-id="1858f-227">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="1858f-228">Sie können auch den Platzhalterwert "\*" um Anforderungen von der alle Ursprünge zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="1858f-228">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="1858f-229">Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen.</span><span class="sxs-lookup"><span data-stu-id="1858f-229">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="1858f-230">Es bedeutet, dass praktisch jeder Website AJAX-Aufrufe Ihrer Web-API durchführen kann.</span><span class="sxs-lookup"><span data-stu-id="1858f-230">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="1858f-231">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="1858f-231">Set the allowed HTTP methods</span></span>

<span data-ttu-id="1858f-232">Die *Methoden* Parameter, der die **[EnableCors]** Attribut gibt an, welche HTTP-Methoden zulässig sind, auf die Ressource zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1858f-232">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="1858f-233">Um alle Methoden zu ermöglichen, verwenden Sie den Platzhalterwert "\*".</span><span class="sxs-lookup"><span data-stu-id="1858f-233">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="1858f-234">Im folgende Beispiel kann nur Get- und POST-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="1858f-234">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="1858f-235">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="1858f-235">Set the allowed request headers</span></span>

<span data-ttu-id="1858f-236">Dieser Artikel beschreibt, wie zuvor eine preflight-Anforderung auf einen Access-Control-Request-Headers-Header, die HTTP-Header, die von der Anwendung festgelegte auflisten umfassen kann (die so genannte "author-Anforderungsheader").</span><span class="sxs-lookup"><span data-stu-id="1858f-236">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="1858f-237">Die *Header* Parameter, der die **[EnableCors]** Attribut gibt an, welche Autor Anforderungsheader zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="1858f-237">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="1858f-238">Um alle Header zuzulassen, legen *Header* auf "\*".</span><span class="sxs-lookup"><span data-stu-id="1858f-238">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="1858f-239">Legen Sie auf eine Whitelist bestimmte Header, *Header* auf eine durch Trennzeichen getrennte Liste der zulässigen Header:</span><span class="sxs-lookup"><span data-stu-id="1858f-239">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="1858f-240">Browser sind jedoch nicht vollständig konsistent wie sie Access-Control-Request-Headers festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1858f-240">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="1858f-241">So enthält beispielsweise Chrome derzeit "Origin".</span><span class="sxs-lookup"><span data-stu-id="1858f-241">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="1858f-242">FireFox enthält keine Standardheader wie z. B. "Accept", auch wenn die Anwendung im Skript festlegt.</span><span class="sxs-lookup"><span data-stu-id="1858f-242">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="1858f-243">Setzen Sie *Header* auf irgendetwas außer "\*", aufzunehmen mindestens "accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="1858f-243">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="1858f-244">Legen Sie die zulässigen Antwortheader</span><span class="sxs-lookup"><span data-stu-id="1858f-244">Set the allowed response headers</span></span>

<span data-ttu-id="1858f-245">Standardmäßig ist der Browser nicht alle Antwortheader für die Anwendung verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="1858f-245">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="1858f-246">Die Antwortheader, die standardmäßig verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="1858f-246">The response headers that are available by default are:</span></span>

- <span data-ttu-id="1858f-247">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="1858f-247">Cache-Control</span></span>
- <span data-ttu-id="1858f-248">Content-Language</span><span class="sxs-lookup"><span data-stu-id="1858f-248">Content-Language</span></span>
- <span data-ttu-id="1858f-249">Inhaltstyp</span><span class="sxs-lookup"><span data-stu-id="1858f-249">Content-Type</span></span>
- <span data-ttu-id="1858f-250">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="1858f-250">Expires</span></span>
- <span data-ttu-id="1858f-251">Zuletzt geändert</span><span class="sxs-lookup"><span data-stu-id="1858f-251">Last-Modified</span></span>
- <span data-ttu-id="1858f-252">Pragma</span><span class="sxs-lookup"><span data-stu-id="1858f-252">Pragma</span></span>

<span data-ttu-id="1858f-253">Die CORS-Spezifikation ruft diese [Header für die einfache Antwort](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="1858f-253">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="1858f-254">Um weitere Header für die Anwendung verfügbar machen, legen die *ExposedHeaders* Parameter **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="1858f-254">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="1858f-255">Im folgenden Beispiel ist der Controller die `Get` Methode legt einen benutzerdefinierten Header mit dem Namen "X-Custom-Header" fest.</span><span class="sxs-lookup"><span data-stu-id="1858f-255">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="1858f-256">Standardmäßig wird der Browser diesen Header in einer cors-Anforderung nicht verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="1858f-256">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="1858f-257">Um den Header verfügbar zu machen, schließen Sie "X-Custom-Header" in *ExposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="1858f-257">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="1858f-258">Übergeben von Anmeldeinformationen in Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="1858f-258">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="1858f-259">Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-259">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="1858f-260">Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1858f-260">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="1858f-261">Anmeldeinformationen enthalten, Cookies sowie HTTP-Authentifizierungsschemas.</span><span class="sxs-lookup"><span data-stu-id="1858f-261">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="1858f-262">Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client festgelegt **XMLHttpRequest.withCredentials** auf "true".</span><span class="sxs-lookup"><span data-stu-id="1858f-262">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="1858f-263">Mithilfe von **XMLHttpRequest** direkt:</span><span class="sxs-lookup"><span data-stu-id="1858f-263">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="1858f-264">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="1858f-264">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="1858f-265">Darüber hinaus muss der Server die Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="1858f-265">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="1858f-266">Um ursprungsübergreifende Anmeldeinformationen in Web-API zu ermöglichen, legen die **"supportscredentials"** Eigenschaft auf "true", auf die **[EnableCors]** Attribut:</span><span class="sxs-lookup"><span data-stu-id="1858f-266">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="1858f-267">Wenn diese Eigenschaft auf "true" festgelegt ist, wird die HTTP-Antwort einen Access-Control-Allow-Credentials-Header einfügen.</span><span class="sxs-lookup"><span data-stu-id="1858f-267">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="1858f-268">Dieser Header informiert den Browser, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.</span><span class="sxs-lookup"><span data-stu-id="1858f-268">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="1858f-269">Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort enthält keines gültigen Access-Control-Allow-Credentials-Headers, der Browser wird die Antwort an die Anwendung nicht verfügbar, und die AJAX-Anforderung ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="1858f-269">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="1858f-270">Achten Sie darauf, dass Sie zur Einstellung **"supportscredentials"** auf "true", da es bedeutet, dass eine Website in einer anderen Domäne kann Ihrer Web-API im Auftrag des Benutzers, eines angemeldeten Benutzers Anmeldeinformationen senden, ohne dass der Benutzer an, dass Sie sich,.</span><span class="sxs-lookup"><span data-stu-id="1858f-270">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="1858f-271">Die CORS-Spezifikation gibt auch an dieser Einstellung *Ursprünge* zu &quot; \* &quot; ist ungültig Wenn **"supportscredentials"** ist "true".</span><span class="sxs-lookup"><span data-stu-id="1858f-271">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="1858f-272">Benutzerdefinierte Anbieter von CORS-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="1858f-272">Custom CORS policy providers</span></span>

<span data-ttu-id="1858f-273">Die **[EnableCors]** Attribut implementiert die **ICorsPolicyProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1858f-273">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="1858f-274">Sie können Ihre eigene Implementierung bereitstellen, indem Sie eine abgeleitete Klasse erstellen **Attribut** und implementiert **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="1858f-274">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="1858f-275">Nachdem Sie das Attribut anwenden können, beliebiger Stelle, platzieren Sie würde **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="1858f-275">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="1858f-276">Beispielsweise kann ein benutzerdefiniertes CORS-Richtlinienanbieter die Einstellungen aus einer Konfigurationsdatei lesen.</span><span class="sxs-lookup"><span data-stu-id="1858f-276">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="1858f-277">Als Alternative zur Verwendung von Attributen, können Sie registrieren einen **ICorsPolicyProviderFactory** Objekt, das erstellt **ICorsPolicyProvider** Objekte.</span><span class="sxs-lookup"><span data-stu-id="1858f-277">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="1858f-278">Festlegen der **ICorsPolicyProviderFactory**, rufen Sie die **SetCorsPolicyProviderFactory** Erweiterungsmethode beim Start wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1858f-278">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="1858f-279">Browserunterstützung</span><span class="sxs-lookup"><span data-stu-id="1858f-279">Browser support</span></span>

<span data-ttu-id="1858f-280">Die Web-API-CORS-Paket ist eine serverseitige Technologie.</span><span class="sxs-lookup"><span data-stu-id="1858f-280">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="1858f-281">Außerdem muss der Browser des Benutzers CORS-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="1858f-281">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="1858f-282">Die aktuellen Versionen von allen wichtigen Browsern zum Glück enthalten [Unterstützung für CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="1858f-282">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
