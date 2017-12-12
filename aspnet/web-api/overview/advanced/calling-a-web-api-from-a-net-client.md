---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Rufen Sie eine Web-API aus einem .NET-Client (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8fcc5e7c6bc39f961931589128a7a5863482aa4e
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="cd3c0-102">Rufen Sie eine Web-API aus einem .NET-Client (c#)</span><span class="sxs-lookup"><span data-stu-id="cd3c0-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="cd3c0-103">durch [Mike Wasson](https://github.com/MikeWasson) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cd3c0-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="cd3c0-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="cd3c0-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="cd3c0-105">Dieses Lernprogramm zeigt, wie eine Web-API in einer .NET-Anwendung mit [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="cd3c0-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="cd3c0-106">In diesem Lernprogramm wird eine Client-app, die die folgenden Web-API nutzt geschrieben:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="cd3c0-107">Aktion</span><span class="sxs-lookup"><span data-stu-id="cd3c0-107">Action</span></span> | <span data-ttu-id="cd3c0-108">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="cd3c0-108">HTTP method</span></span> | <span data-ttu-id="cd3c0-109">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="cd3c0-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd3c0-110">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="cd3c0-110">Get a product by ID</span></span> | <span data-ttu-id="cd3c0-111">GET</span><span class="sxs-lookup"><span data-stu-id="cd3c0-111">GET</span></span> | <span data-ttu-id="cd3c0-112">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="cd3c0-112">/api/products/*id*</span></span> |
| <span data-ttu-id="cd3c0-113">Erstellen eines neuen Produkts</span><span class="sxs-lookup"><span data-stu-id="cd3c0-113">Create a new product</span></span> | <span data-ttu-id="cd3c0-114">BEREITSTELLEN</span><span class="sxs-lookup"><span data-stu-id="cd3c0-114">POST</span></span> | <span data-ttu-id="cd3c0-115">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="cd3c0-115">/api/products</span></span> |
| <span data-ttu-id="cd3c0-116">Aktualisieren eines Produkts</span><span class="sxs-lookup"><span data-stu-id="cd3c0-116">Update a product</span></span> | <span data-ttu-id="cd3c0-117">PUT</span><span class="sxs-lookup"><span data-stu-id="cd3c0-117">PUT</span></span> | <span data-ttu-id="cd3c0-118">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="cd3c0-118">/api/products/*id*</span></span> |
| <span data-ttu-id="cd3c0-119">Löschen eines Produkts</span><span class="sxs-lookup"><span data-stu-id="cd3c0-119">Delete a product</span></span> | <span data-ttu-id="cd3c0-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="cd3c0-120">DELETE</span></span> | <span data-ttu-id="cd3c0-121">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="cd3c0-121">/api/products/*id*</span></span> |

<span data-ttu-id="cd3c0-122">Zum Implementieren dieser API mit ASP.NET Web-API finden Sie unter [erstellen eine Web-API, CRUD-Vorgänge unterstützt](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="cd3c0-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="cd3c0-123">Der Einfachheit halber wird die Clientanwendung in diesem Lernprogramm eine Windows-Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="cd3c0-124">**"HttpClient"** wird auch für Windows Phone und Windows Store-apps unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="cd3c0-125">Weitere Informationen finden Sie unter [Schreiben von Web-API-Clientcode für mehrere Plattformen mithilfe von portablen Bibliotheken](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="cd3c0-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="cd3c0-126">Erstellen der Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="cd3c0-126">Create the Console Application</span></span>

<span data-ttu-id="cd3c0-127">In Visual Studio, erstellen Sie eine neue Windows-Konsolen-app mit dem Namen **HttpClientSample** und fügen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="cd3c0-128">Der vorhergehende Code ist die vollständige Client-app.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="cd3c0-129">`RunAsync`ausgeführt wird, und blockiert, bis er abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="cd3c0-130">Die meisten **"HttpClient"** Methoden sind asynchrone, daran, dass sie die Netzwerk-e/a ausführen.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="cd3c0-131">Alle asynchronen Vorgänge erfolgen in `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="cd3c0-132">Normalerweise keine app den Haupt-Thread blockieren, aber diese app lässt nicht die ein Eingreifen.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="cd3c0-133">Installieren Sie die Web-API-Client-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="cd3c0-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="cd3c0-134">Verwenden Sie NuGet Package Manager zum Installieren des Pakets Web API Client Libraries.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="cd3c0-135">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="cd3c0-136">Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="cd3c0-137">Dieser Befehl wird dem Projekt die folgenden NuGet-Pakete hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="cd3c0-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="cd3c0-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="cd3c0-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="cd3c0-139">Newtonsoft.Json</span></span>

<span data-ttu-id="cd3c0-140">Json.NET ist eine beliebte hohe Leistung JSON-Framework für .NET.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="cd3c0-141">Fügen Sie eine Modellklasse hinzu</span><span class="sxs-lookup"><span data-stu-id="cd3c0-141">Add a Model Class</span></span>

<span data-ttu-id="cd3c0-142">Überprüfen Sie die `Product` Klasse:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="cd3c0-143">Diese Klasse entspricht das Datenmodell, das von der Web-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="cd3c0-144">Können Sie eine app **"HttpClient"** zum Lesen einer `Product` -Instanz anhand einer HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="cd3c0-145">Die app keine Deserialisierungscode schreiben.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="cd3c0-146">Erstellen und initialisieren Sie "HttpClient"</span><span class="sxs-lookup"><span data-stu-id="cd3c0-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="cd3c0-147">Überprüfen Sie die statische **"HttpClient"** Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="cd3c0-148">**"HttpClient"** einmal instanziiert werden soll und die Lebensdauer einer Anwendung wiederverwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="cd3c0-149">Die folgenden Bedingungen können dazu führen, **SocketException** Fehler:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="cd3c0-150">Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="cd3c0-151">Der Server stark ausgelastet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-151">Server under heavy load.</span></span>

<span data-ttu-id="cd3c0-152">Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung kann die verfügbaren Sockets ausgeschöpft.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="cd3c0-153">Der folgende code initialisiert das **"HttpClient"** Instanz:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="cd3c0-154">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-154">The preceding code:</span></span>

* <span data-ttu-id="cd3c0-155">Definiert die Basis-URI für HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="cd3c0-156">Ändern Sie die Portnummer an den Port in der Server-app verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="cd3c0-157">Die app funktioniert nicht, es sei denn, der Port für die Server-app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="cd3c0-158">Legt den Accept-Header auf "Application/Json" fest.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="cd3c0-159">Dieser Header festlegen, weist den Server zum Senden von Daten im JSON-Format.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="cd3c0-160">Senden einer GET-Anforderung zum Abrufen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="cd3c0-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="cd3c0-161">Der folgende Code sendet eine GET-Anforderung für ein Produkt:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="cd3c0-162">Die **GetAsync** Methode sendet die HTTP-GET-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="cd3c0-163">Wenn die Methode abgeschlossen wird, gibt es eine **HttpResponseMessage** , enthält die HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="cd3c0-164">Wenn der Statuscode in der Antwort einen Erfolgscode ist, enthält der Antworttext die JSON-Darstellung eines Produkts.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="cd3c0-165">Rufen Sie **ReadAsAsync** zu deserialisierende JSON-Nutzlast an einen `Product` Instanz.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="cd3c0-166">Die **ReadAsAsync** Methode ist asynchron, da der Antworttext beliebig groß sein kann.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="cd3c0-167">**"HttpClient"** löst keine Ausnahme aus, wenn die HTTP-Antwort einen Fehlercode enthält.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="cd3c0-168">Stattdessen die **IsSuccessStatusCode** Eigenschaft **"false"** lautet der Status ein Fehlercode.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="cd3c0-169">Wenn Sie es vorziehen, die HTTP-Fehlercodes als Ausnahmen behandelt werden sollen, rufen Sie [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) auf dem Antwortobjekt.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="cd3c0-170">`EnsureSuccessStatusCode`löst eine Ausnahme aus, wenn der Statuscode außerhalb des Bereichs 200 liegt&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="cd3c0-171">Beachten Sie, dass **"HttpClient"** kann Ausnahmen auslösen, aus anderen Gründen &mdash; z. B., wenn die Anforderung ein Timeout eintritt.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="cd3c0-172">Medientypformatierer deserialisiert</span><span class="sxs-lookup"><span data-stu-id="cd3c0-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="cd3c0-173">Wenn **ReadAsAsync** wird aufgerufen, ohne Parameter verwendet eine Reihe von *medienformatierer* zum Lesen des Antworttexts.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="cd3c0-174">Die Standard-Formatierer unterstützt JSON und XML-Formular-Url-codierte Daten.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="cd3c0-175">Anstatt die Standard-Formatierer verwenden, können Sie eine Liste der Formatierer zum Bereitstellen der **ReadAsAsync** Methode.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="cd3c0-176">Mit einem eine Liste der Formatierer ist nützlich, wenn Sie eine benutzerdefinierte medientypformatierer haben:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="cd3c0-177">Weitere Informationen finden Sie unter [Medienformatierer in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="cd3c0-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="cd3c0-178">Sendet eine POST-Anforderung zum Erstellen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="cd3c0-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="cd3c0-179">Der folgende Code sendet eine POST-Anforderung, die enthält eine `Product` Instanz im JSON-Format:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="cd3c0-180">Die **PostAsJsonAsync** Methode:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="cd3c0-181">Serialisiert ein Objekt in JSON.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="cd3c0-182">Sendet die JSON-Nutzlast in einer POST-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="cd3c0-183">Wenn die Anforderung erfolgreich ist:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-183">If the request succeeds:</span></span>

* <span data-ttu-id="cd3c0-184">Es sollte eine 201 (erstellt) Antwort zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="cd3c0-185">Die Antwort sollte die URL der erstellten Ressourcen im Location-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="cd3c0-186">Sendet eine PUT-Anforderung zum Aktualisieren einer Ressourcenpools</span><span class="sxs-lookup"><span data-stu-id="cd3c0-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="cd3c0-187">Der folgende Code sendet eine PUT-Anforderung, um ein Produkt zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="cd3c0-188">Die **PutAsJsonAsync** Methode funktioniert wie **PostAsJsonAsync**, außer dass eine PUT-Anforderung anstelle von POST sendet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="cd3c0-189">Senden eine DELETE-Anforderung zum Löschen eines Ressourcenpools</span><span class="sxs-lookup"><span data-stu-id="cd3c0-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="cd3c0-190">Der folgende Code sendet eine DELETE-Anforderung zum Löschen eines Produkts:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="cd3c0-191">Wie "GET" muss eine DELETE-Anforderung keinen Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="cd3c0-192">Sie müssen nicht JSON oder XML-Format mit DELETE angeben.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="cd3c0-193">Testen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="cd3c0-193">Test the sample</span></span>

<span data-ttu-id="cd3c0-194">So testen Sie die Client-app:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-194">To test the client app:</span></span>

1. <span data-ttu-id="cd3c0-195">[Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) und führen Sie die Server-app.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) and run the server app.</span></span> <span data-ttu-id="cd3c0-196">[Anweisungen zum Herunterladen](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="cd3c0-196">[Download instructions](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="cd3c0-197">Stellen Sie sicher, dass die Server-app funktioniert.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-197">Verify the server app is working.</span></span> <span data-ttu-id="cd3c0-198">Für Exaxmple `http://localhost:64195/api/products` sollte eine Liste der Produkte zurück.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="cd3c0-199">Legen Sie die Basis-URI für HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="cd3c0-200">Ändern Sie die Portnummer an den Port in der Server-app verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="cd3c0-201">Führen Sie die Client-app.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-201">Run the client app.</span></span> <span data-ttu-id="cd3c0-202">Es wird die folgende Ausgabe generiert:</span><span class="sxs-lookup"><span data-stu-id="cd3c0-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
