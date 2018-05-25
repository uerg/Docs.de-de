---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR-Hubs-API-Handbuch - JavaScript-Client | Microsoft Docs
author: pfletcher
description: Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 im JavaScript-Clients, z. B. Browsern und Windows Store (WinJS) Application...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="30548-103">ASP.NET SignalR-Hubs-API-Handbuch - JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="30548-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="30548-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="30548-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="30548-105">Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 im JavaScript-Clients, z. B. Browsern und Anwendungen für Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="30548-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="30548-106">Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen.</span><span class="sxs-lookup"><span data-stu-id="30548-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="30548-107">Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="30548-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="30548-108">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="30548-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="30548-109">SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.</span><span class="sxs-lookup"><span data-stu-id="30548-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="30548-110">SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="30548-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="30548-111">Eine Einführung in SignalR-Hubs und dauerhafte Verbindungen erhalten finden Sie unter [Einführung in SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="30548-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="30548-112">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="30548-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="30548-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30548-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="30548-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="30548-114">.NET 4.5</span></span>
> - <span data-ttu-id="30548-115">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="30548-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="30548-116">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="30548-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="30548-117">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="30548-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="30548-118">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="30548-118">Questions and comments</span></span>
> 
> <span data-ttu-id="30548-119">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="30548-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="30548-120">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="30548-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="30548-121">Übersicht</span><span class="sxs-lookup"><span data-stu-id="30548-121">Overview</span></span>

<span data-ttu-id="30548-122">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="30548-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="30548-123">Den generierten Proxy und was es für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="30548-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="30548-124">Verwenden Sie den generierten proxy</span><span class="sxs-lookup"><span data-stu-id="30548-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="30548-125">Client-Setup</span><span class="sxs-lookup"><span data-stu-id="30548-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="30548-126">Gewusst wie: den dynamisch generierten Proxy verweisen.</span><span class="sxs-lookup"><span data-stu-id="30548-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="30548-127">Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy</span><span class="sxs-lookup"><span data-stu-id="30548-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="30548-128">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="30548-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="30548-129">$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="30548-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="30548-130">Asynchrone Ausführung der Start-Methode</span><span class="sxs-lookup"><span data-stu-id="30548-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="30548-131">Gewusst wie: Herstellen einer Verbindung domänenübergreifende</span><span class="sxs-lookup"><span data-stu-id="30548-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="30548-132">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="30548-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="30548-133">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="30548-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="30548-134">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="30548-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="30548-135">Gewusst wie: Abrufen ein Proxys für eine hubklasse</span><span class="sxs-lookup"><span data-stu-id="30548-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="30548-136">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="30548-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="30548-137">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="30548-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="30548-138">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="30548-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="30548-139">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="30548-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="30548-140">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="30548-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="30548-141">Dokumentation über die Programmierung des Servers oder Clients .NET finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="30548-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="30548-142">SignalR-Hubs-API-Leitfaden – Server</span><span class="sxs-lookup"><span data-stu-id="30548-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="30548-143">SignalR-Hubs-API-Leitfaden – .NET Client</span><span class="sxs-lookup"><span data-stu-id="30548-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="30548-144">Die Serverkomponente von SignalR-2 ist nur verfügbar, auf .NET 4.5, (obwohl es ein .NET Client für SignalR-2 unter .NET 4.0 ist).</span><span class="sxs-lookup"><span data-stu-id="30548-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="30548-145">Den generierten Proxy und was es für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="30548-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="30548-146">Sie können einen JavaScript-Client für die Kommunikation mit einer SignalR-Dienst mit oder ohne einen Proxy aus, dem SignalR für Sie generiert programmieren.</span><span class="sxs-lookup"><span data-stu-id="30548-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="30548-147">Leistungsumfang der Proxy für Sie ist die Syntax des Codes zu vereinfachen, die Verbindung verwenden, Write-Methoden, die der Server aufgerufen wird, und rufen Methoden auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="30548-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="30548-148">Beim Schreiben von Code zum Aufrufen von Servermethoden, der generierte Proxy können Sie Syntax zu verwenden, das aussieht, als wären Sie eine lokale Funktion ausgeführt wurden: Sie können schreiben `serverMethod(arg1, arg2)` anstelle von `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="30548-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="30548-149">Die generierten Proxy-Syntax kann auch Fehler clientseitige sofortigen und verständlicher, wenn Sie einen Server-Methodennamen richtig eingegeben.</span><span class="sxs-lookup"><span data-stu-id="30548-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="30548-150">Und wenn Sie manuell die Datei erstellen, die die Proxys definiert außerdem erhalten Sie IntelliSense-Unterstützung für das Schreiben von Code, der Servermethoden aufruft.</span><span class="sxs-lookup"><span data-stu-id="30548-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="30548-151">Nehmen wir beispielsweise an, dass Sie die folgenden hubklasse auf dem Server verfügen:</span><span class="sxs-lookup"><span data-stu-id="30548-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="30548-152">Die folgenden Codebeispiele zeigen, wie sieht der JavaScript-Code zum Aufrufen der `NewContosoChatMessage` Methode auf dem Server und die Aufrufe der Empfang der `addContosoChatMessageToPage` Methode vom Server.</span><span class="sxs-lookup"><span data-stu-id="30548-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="30548-153">**Mit den generierten proxy**</span><span class="sxs-lookup"><span data-stu-id="30548-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="30548-154">**Ohne den generierten proxy**</span><span class="sxs-lookup"><span data-stu-id="30548-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="30548-155">Verwenden Sie den generierten proxy</span><span class="sxs-lookup"><span data-stu-id="30548-155">When to use the generated proxy</span></span>

<span data-ttu-id="30548-156">Wenn Sie mehrere Ereignishandler für eine Clientmethode zu registrieren, die der Server ruft möchten, können nicht Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="30548-157">Andernfalls können auch den generierten Proxy verwenden oder nicht auf die Einstellung für die Codierung basieren.</span><span class="sxs-lookup"><span data-stu-id="30548-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="30548-158">Wenn Sie nicht verwenden, müssen Sie keine URL / "Signalr-Hubs" in verweisen eine `script` Element im Clientcode.</span><span class="sxs-lookup"><span data-stu-id="30548-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="30548-159">Client-setup</span><span class="sxs-lookup"><span data-stu-id="30548-159">Client setup</span></span>

<span data-ttu-id="30548-160">Ein JavaScript-Client erfordert Verweise auf jQuery und der SignalR Core JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="30548-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="30548-161">Die jQuery-Version muss es sich um 1.6.4 oder höher Hauptversionen, z. B. 1.7.2, 1.8.2 oder 1.9.1 sein.</span><span class="sxs-lookup"><span data-stu-id="30548-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="30548-162">Wenn Sie den generierten Proxy verwenden möchten, benötigen Sie auch einen Verweis auf den Proxy generiert SignalR JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="30548-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="30548-163">Das folgende Beispiel zeigt, wie die Verweise in einer HTML-Seite aussehen könnte, die den generierten Proxy verwendet.</span><span class="sxs-lookup"><span data-stu-id="30548-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="30548-164">Diese Verweise in dieser Reihenfolge enthalten sein müssen: jQuery SignalR Core danach und SignalR-Proxys First-und last.</span><span class="sxs-lookup"><span data-stu-id="30548-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="30548-165">Gewusst wie: den dynamisch generierten Proxy verweisen.</span><span class="sxs-lookup"><span data-stu-id="30548-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="30548-166">Im vorherigen Beispiel wird der Verweis auf den Proxy SignalR generiert für dynamisch generierte JavaScript-Code, nicht zu einer physischen Datei.</span><span class="sxs-lookup"><span data-stu-id="30548-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="30548-167">SignalR JavaScript-Code für den Proxy bei Bedarf erstellt und an dem Client als Antwort auf die URL "/ Signalr/Hubs" dient.</span><span class="sxs-lookup"><span data-stu-id="30548-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="30548-168">Bei Angabe eine andere base-URL für SignalR-Verbindungen auf dem Server in Ihrer `MapSignalR` -Methode, die URL für die dynamisch generierte Proxydatei ist Ihre benutzerdefinierte URL mit "/ Hubs" angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="30548-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="30548-169">Verwenden Sie für Windows 8 (Windows Store) JavaScript-Clients die physische Proxydatei statt der dynamisch generierten.</span><span class="sxs-lookup"><span data-stu-id="30548-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="30548-170">Weitere Informationen finden Sie unter [zum Erstellen einer physischen Datei für SignalR generierten Proxy](#manualproxy) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="30548-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="30548-171">Verwenden Sie die Tilde zum Verweisen auf das Stammverzeichnis der Anwendung in Ihrem Proxy Dateiverweis, in einer ASP.NET MVC 4 oder 5 Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="30548-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="30548-172">Weitere Informationen zur Verwendung von SignalR in MVC 5 finden Sie unter [erste Schritte mit SignalR und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="30548-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="30548-173">Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Verweis der Proxy-Datei:</span><span class="sxs-lookup"><span data-stu-id="30548-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="30548-174">Verwenden Sie in einer ASP.NET Web Forms-Anwendung `ResolveClientUrl` für die Proxys Verweis der Datei, oder registrieren Sie ihn über das ScriptManager mit einem relativen Pfad Stamm app (beginnend mit einer Tilde):</span><span class="sxs-lookup"><span data-stu-id="30548-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="30548-175">Verwenden Sie als allgemeine Regel die gleiche Methode zum Angeben der URL "/ Signalr/Hubs", die Sie für CSS oder JavaScript-Dateien verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="30548-176">Wenn Sie eine URL angeben, ohne mit einer Tilde, wird in einigen Szenarien Ihrer Anwendung fehlerfrei, wenn Sie in Visual Studio mit IIS Express testen schlägt jedoch fehl mit Fehler 404 während der Bereitstellung auf vollständige IIS.</span><span class="sxs-lookup"><span data-stu-id="30548-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="30548-177">Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen auf der Stammebene** in [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="30548-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="30548-178">Wenn Sie ein Webprojekt in Visual Studio 2013 im Debugmodus ausgeführt, und wenn Sie Internet Explorer als Ihren Browser verwenden, Sie die Proxydatei in sehen **Projektmappen-Explorer** unter **Skriptdokumente**, entsprechend der folgende Abbildung.</span><span class="sxs-lookup"><span data-stu-id="30548-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![JavaScript generiert eine Proxyklasse im Projektmappen-Explorer](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="30548-180">Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="30548-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="30548-181">Wenn Sie Visual Studio 2012 oder 2013 und Internet Explorer nicht verwenden oder wenn Sie nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei zu suchen und die URL "/ SignalR/Hubs" abrufen.</span><span class="sxs-lookup"><span data-stu-id="30548-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="30548-182">Beispielsweise wird bei der Ausführung des Standorts auf `http://localhost:56699`, wechseln Sie zu `http://localhost:56699/SignalR/hubs` in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="30548-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="30548-183">Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy</span><span class="sxs-lookup"><span data-stu-id="30548-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="30548-184">Als Alternative zum dynamisch generierten Proxy können Sie erstellen eine physische Datei, die den Proxycode verfügt und diese Datei verweisen.</span><span class="sxs-lookup"><span data-stu-id="30548-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="30548-185">Sie sollten sich zur Steuerung von Inhalte zwischengespeichert oder bundling Verhalten dazu oder IntelliSense abzurufen, bei der Codierung Aufrufe von Servermethoden.</span><span class="sxs-lookup"><span data-stu-id="30548-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="30548-186">Um eine Proxydatei zu erstellen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="30548-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="30548-187">Installieren der [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="30548-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="30548-188">Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu der *Tools* Ordner, der die SignalR.exe-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="30548-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="30548-189">Ordner "Tools" wird am folgenden Speicherort:</span><span class="sxs-lookup"><span data-stu-id="30548-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="30548-190">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="30548-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="30548-191">Der Pfad zu Ihrer *DLL* ist in der Regel die *"bin"* Ordner im Projektordner.</span><span class="sxs-lookup"><span data-stu-id="30548-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="30548-192">Dieser Befehl erstellt eine Datei namens *server.js* im gleichen Ordner wie *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="30548-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="30548-193">Versetzen der *server.js* Datei in eine entsprechende Ordner im Projekt, benennen Sie sie nach Bedarf für Ihre Anwendung und einen Verweis darauf anstelle der / "Signalr-Hubs" Verweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="30548-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="30548-194">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="30548-194">How to establish a connection</span></span>

<span data-ttu-id="30548-195">Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungsobjekt zu erstellen, erstellen einen Proxy und registrieren Sie Ereignishandler für Methoden, die vom Server aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="30548-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="30548-196">Wenn Sie den Proxy und Ereignishandler eingerichtet wurden, Herstellung einer Verbindung durch Aufrufen der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="30548-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="30548-197">Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungsobjekt in Ihrem eigenen Code erstellt werden, da der generierte Proxycode für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="30548-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="30548-198">**Herstellen einer Verbindungs (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="30548-199">**Herstellen einer Verbindungs (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="30548-200">Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst.</span><span class="sxs-lookup"><span data-stu-id="30548-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="30548-201">Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="30548-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="30548-202">Standardmäßig ist der Hub-Speicherort des aktuellen Servers; Wenn Sie eine Verbindung mit einem anderen Server herstellen, geben Sie die URL vor dem Aufruf der `start` Methode, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="30548-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="30548-203">Normalerweise registrieren Sie Ereignishandler vor dem Aufruf der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="30548-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="30548-204">Wenn einige Ereignishandler zu registrieren, nach dem Herstellen der Verbindung werden sollen, können Sie dies tun, aber Sie müssen mindestens eine Ihrer Ereignis einer oder mehrere Ereignishandler vor dem Aufruf Registrieren der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="30548-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="30548-205">Ein Grund dafür ist, dass in einer Anwendung können viele Hubs vorhanden sein, aber nicht empfehlenswert Auslösen der `OnConnected` Ereignis für jedes Hub, wenn Sie nur eines davon verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="30548-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="30548-206">Wenn die Verbindung hergestellt wird, das Vorhandensein einer Client-Methode auf einem Hub-Proxy ist womit SignalR zum Auslösen der `OnConnected` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="30548-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="30548-207">Alle Ereignishandler vor dem Aufruf registriert die `start` -Methode, Sie werden zum Aufrufen von Methoden der Hub, aber auf des Hubs `OnConnected` Methode wird nicht aufgerufen werden und keine Clientmethoden werden vom Server aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="30548-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="30548-208">$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="30548-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="30548-209">Wie Sie aus den Beispielen sehen können, wenn Sie den generierten Proxy verwenden `$.connection.hub` bezieht sich auf das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="30548-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="30548-210">Dies ist das gleiche Objekt, die Sie durch den Aufruf erhalten `$.hubConnection()` Sie sind nicht bei den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="30548-211">Der generierte Proxycode erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:</span><span class="sxs-lookup"><span data-stu-id="30548-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Erstellen eine Verbindung in die generierte Proxydatei](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="30548-213">Wenn Sie den generierten Proxy verwenden, können Sie alle mit Aktionen `$.connection.hub` , dass Sie mit einem Verbindungsobjekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="30548-214">Asynchrone Ausführung der Start-Methode</span><span class="sxs-lookup"><span data-stu-id="30548-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="30548-215">Die `start` Methode asynchron ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="30548-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="30548-216">Gibt eine [jQuery zurückgestellt Objekt](http://api.jquery.com/category/deferred-object/), was bedeutet, dass Sie Rückruffunktionen hinzufügen können, durch Aufrufen von Methoden wie z. B. `pipe`, `done`, und `fail`.</span><span class="sxs-lookup"><span data-stu-id="30548-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="30548-217">Wenn Sie über Code, die verfügen nach dem Herstellen der Verbindung ausgeführt werden soll, z. B. einen Aufruf einer Servermethode, legen Sie diesen Code in eine Rückruffunktion oder aus eine Rückruffunktion aufgerufen Sie wird.</span><span class="sxs-lookup"><span data-stu-id="30548-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="30548-218">Die `.done` Rückrufmethode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach der code Ihrer `OnConnected` Ereignishandlermethode auf dem Server die Ausführung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="30548-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="30548-219">Wenn Sie die Anweisung "Jetzt verbunden" aus dem vorherigen Beispiel als die nächste Zeile des Codes nach dem Einfügen der `start` Methodenaufruf (nicht in einer `.done` Rückruf), die `console.log` Zeile ausgeführt, bevor die Verbindung hergestellt wird, wie das folgende Beispiel zeigt Beispiel:</span><span class="sxs-lookup"><span data-stu-id="30548-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Falsches vorgehen, Code zu schreiben, der ausgeführt wird, nachdem die Verbindung hergestellt wird](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="30548-221">Gewusst wie: Herstellen einer Verbindung domänenübergreifende</span><span class="sxs-lookup"><span data-stu-id="30548-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="30548-222">In der Regel, wenn der Browser eine Seite aus lädt `http://contoso.com`, die SignalR-Verbindung ist auf der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="30548-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="30548-223">Wenn die Seite `http://contoso.com` stellt eine Verbindung mit `http://fabrikam.com/signalr`, d. h. eine domänenübergreifende-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="30548-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="30548-224">Domänenübergreifende Verbindungen werden aus Gründen der Sicherheit standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="30548-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="30548-225">Im SignalR wurden von einem einzelnen EnableCrossDomain Flag 1.x, domänenübergreifende Anforderungen gesteuert.</span><span class="sxs-lookup"><span data-stu-id="30548-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="30548-226">Dieses Flag gesteuert, JSONP und CORS-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="30548-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="30548-227">Für größere Flexibilität, die alle CORS-Unterstützung die Serverkomponente von SignalR entzogen wurde (JavaScript-Clients weiterhin verwenden CORS normalerweise, wenn es erkannt wird, dass der Browser unterstützt), und neue OWIN-Middleware zur Unterstützung dieser Szenarien verfügbar gemacht wurde.</span><span class="sxs-lookup"><span data-stu-id="30548-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="30548-228">Wenn JSONP auf dem Client (zur Unterstützung von älteren Browsern domänenübergreifende Anforderungen) erforderlich ist, müssen sie explizit aktiviert werden, indem die Einstellung `EnableJSONP` auf die `HubConfiguration` -Objekt `true`, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="30548-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="30548-229">JSONP ist standardmäßig deaktiviert, da es weniger sicher als CORS ist.</span><span class="sxs-lookup"><span data-stu-id="30548-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="30548-230">**Hinzufügen von Microsoft.Owin.Cors zu Ihrem Projekt:** um diese Bibliothek zu installieren, führen Sie den folgenden Befehl in der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="30548-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="30548-231">Mit diesem Befehl wird die 2.1.0 hinzugefügt Version des Pakets zu Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="30548-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="30548-232">UseCors aufrufen</span><span class="sxs-lookup"><span data-stu-id="30548-232">Calling UseCors</span></span>

 <span data-ttu-id="30548-233">Der folgende Codeausschnitt veranschaulicht, wie domänenübergreifende Verbindungen in SignalR 2 implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="30548-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="30548-234">**Implementieren von domänenübergreifende Anforderungen in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="30548-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="30548-235">Der folgende Code veranschaulicht, wie zum Aktivieren von CORS oder JSONP in einer SignalR-2-Projekt.</span><span class="sxs-lookup"><span data-stu-id="30548-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="30548-236">Dieses Codebeispiel verwendet `Map` und `RunSignalR` anstelle von `MapSignalR`, sodass nur für die SignalR-Anforderungen, die CORS-Unterstützung erfordern, die CORS-Middleware ausgeführt wird (und nicht für den gesamten Datenverkehr im angegebenen Pfad `MapSignalR`.) Zuordnung kann auch für andere Middleware, die verwendet werden, die für eine bestimmte URL-Präfix, nicht für die gesamte Anwendung ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="30548-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="30548-237">Legen Sie die keine `jQuery.support.cors` auf "true" in Ihrem Code.</span><span class="sxs-lookup"><span data-stu-id="30548-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Setzen Sie jQuery.support.cors nicht auf "true"](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="30548-239">SignalR behandelt die Verwendung von CORS.</span><span class="sxs-lookup"><span data-stu-id="30548-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="30548-240">Festlegen von `jQuery.support.cors` auf "true" JSONP deaktiviert, da er bewirkt, dass SignalR anzunehmen, der Browser unterstützt CORS.</span><span class="sxs-lookup"><span data-stu-id="30548-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="30548-241">Wenn Sie eine Verbindung zu einer URL "localhost" herstellen, wird nicht Internet Explorer 10 eine Verbindung domänenübergreifende beachtet werden, damit die Anwendung lokal mit Internet Explorer 10 verwendet werden kann, selbst wenn Sie die domänenübergreifende Verbindungen auf dem Server aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="30548-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="30548-242">Weitere Informationen zur Verwendung von domänenübergreifende Verbindungen mit Internet Explorer 9, finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="30548-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="30548-243">Informationen zur Verwendung von domänenübergreifende Verbindungen mit Chrome finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="30548-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="30548-244">Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst.</span><span class="sxs-lookup"><span data-stu-id="30548-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="30548-245">Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="30548-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="30548-246">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="30548-246">How to configure the connection</span></span>

<span data-ttu-id="30548-247">Bevor Sie eine Verbindung herstellen, können Sie Abfragezeichenfolgen-Parameter angeben oder geben Sie die Transportmethode.</span><span class="sxs-lookup"><span data-stu-id="30548-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="30548-248">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="30548-248">How to specify query string parameters</span></span>

<span data-ttu-id="30548-249">Wenn Sie möchten Daten an den Server senden, wenn der Client eine Verbindung herstellt, können Sie Abfragezeichenfolgen-Parameter an das Verbindungsobjekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="30548-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="30548-250">Die folgenden Beispiele zeigen, wie einen Abfragezeichenfolgenparameter in Clientcode festgelegt.</span><span class="sxs-lookup"><span data-stu-id="30548-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="30548-251">**Einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (mit den generierten Proxy) festgelegt**</span><span class="sxs-lookup"><span data-stu-id="30548-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="30548-252">**Legen Sie einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="30548-253">Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgenparameter in Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="30548-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="30548-254">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="30548-254">How to specify the transport method</span></span>

<span data-ttu-id="30548-255">Im Rahmen des Prozesses eine Verbindung herstellen verhandelt SignalR-Client normalerweise mit dem Server zu den bewährten Transport ermitteln kann, der unterstützt wird, indem sowohl Server-als auch.</span><span class="sxs-lookup"><span data-stu-id="30548-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="30548-256">Wenn Sie bereits wissen, dass der Transportmethode verwenden möchten, können Sie die dabei Aushandlung umgehen, indem die Transportmethode angeben, beim Aufrufen der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="30548-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="30548-257">**Clientcode, der angibt, die Transportmethode (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="30548-258">**Clientcode, der angibt, die Transportmethode (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="30548-259">Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in denen Sie diese ausprobieren SignalR werden soll:</span><span class="sxs-lookup"><span data-stu-id="30548-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="30548-260">**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="30548-261">**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="30548-262">Sie können die folgenden Werte zur Angabe der Transportmethode verwenden:</span><span class="sxs-lookup"><span data-stu-id="30548-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="30548-263">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="30548-263">"webSockets"</span></span>
- <span data-ttu-id="30548-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="30548-264">"foreverFrame"</span></span>
- <span data-ttu-id="30548-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="30548-265">"serverSentEvents"</span></span>
- <span data-ttu-id="30548-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="30548-266">"longPolling"</span></span>

<span data-ttu-id="30548-267">Die folgenden Beispiele zeigen, wie Sie herausfinden, welche Transportmethode von eine Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="30548-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="30548-268">**Clientcode, der die Transportmethode, die von einer Verbindung (mit den generierten Proxy) verwendete zeigt**</span><span class="sxs-lookup"><span data-stu-id="30548-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="30548-269">**Clientcode, der die Transportmethode, die von einer Verbindung (ohne den generierten Proxy) verwendete zeigt**</span><span class="sxs-lookup"><span data-stu-id="30548-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="30548-270">Informationen zum Überprüfen der Transportmethode in Servercode finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - zum Abrufen von Informationen über den Client aus der Kontexteigenschaft](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="30548-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="30548-271">Weitere Informationen zu Transporten und Zugriffe, finden Sie unter [Einführung in die SignalR - Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="30548-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="30548-272">Gewusst wie: Abrufen ein Proxys für eine hubklasse</span><span class="sxs-lookup"><span data-stu-id="30548-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="30548-273">Jedes Verbindungsobjekt, das Sie erstellen kapselt Informationen über eine Verbindung mit einer SignalR-Dienst, der eine oder mehrere Hub-Klassen enthält.</span><span class="sxs-lookup"><span data-stu-id="30548-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="30548-274">Verwenden Sie für die Kommunikation mit einem hubklasse ein Proxyobjekt aus, die Sie erstellen selbst (sofern Sie nicht den generierten Proxy verwenden) oder der für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="30548-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="30548-275">Auf dem Client ist der Proxyname einer Version in Kamel-Schreibweise des Klassennamens Hub.</span><span class="sxs-lookup"><span data-stu-id="30548-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="30548-276">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="30548-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="30548-277">**Hubklasse auf server**</span><span class="sxs-lookup"><span data-stu-id="30548-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="30548-278">**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="30548-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="30548-279">**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="30548-280">Wenn Sie ergänzen die Hub-Klasse mit einem `HubName` -Attribut angegeben wird, verwenden Sie den genauen Namen ohne die Groß-/Kleinschreibung ändern.</span><span class="sxs-lookup"><span data-stu-id="30548-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="30548-281">**Hubklasse auf Server mit HubName-Attribut**</span><span class="sxs-lookup"><span data-stu-id="30548-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="30548-282">**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="30548-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="30548-283">**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="30548-284">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="30548-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="30548-285">Um eine Methode definieren, die der Server mit einem Hub aufrufen können, fügen Sie einen Ereignishandler auf den Hub-Proxy mithilfe der `client` Eigenschaft des generierten Proxys oder der Aufruf der `on` Methode, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="30548-286">Die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="30548-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="30548-287">Fügen Sie dem Ereignishandler hinzu, vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="30548-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="30548-288">(Wenn Sie nach dem Aufruf Ereignishandler hinzufügen möchten die `start` -Methode finden Sie im Hinweis in [zum Herstellen eine Verbindung](#establishconnection) weiter oben in diesem Dokument, und verwenden Sie die Syntax für die Definition einer Methode ohne Verwendung des generierten Proxys veranschaulicht.)</span><span class="sxs-lookup"><span data-stu-id="30548-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="30548-289">Methode-namenszuordnung wird Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="30548-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="30548-290">Beispielsweise `Clients.All.addContosoChatMessageToPage` auf dem Server führt `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, oder `addcontosochatmessagetopage` auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="30548-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="30548-291">**Definieren Sie die Methode auf dem Client (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="30548-292">**Alternative Möglichkeit zum Definieren der Methode auf dem Client (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="30548-293">**Definieren Sie die Methode auf dem Client (ohne den generierten Proxy oder beim Hinzufügen von nach dem Aufrufen der Start-Methode)**</span><span class="sxs-lookup"><span data-stu-id="30548-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="30548-294">**Server-Code, der die Methode aufruft.**</span><span class="sxs-lookup"><span data-stu-id="30548-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="30548-295">Im folgenden Beispiel werden ein komplexes Objekt als Parameter einer Methode enthalten.</span><span class="sxs-lookup"><span data-stu-id="30548-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="30548-296">**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (mit den generierten Proxy) akzeptiert.**</span><span class="sxs-lookup"><span data-stu-id="30548-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="30548-297">**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (ohne den generierten Proxy) akzeptiert.**</span><span class="sxs-lookup"><span data-stu-id="30548-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="30548-298">**Servercode, die das komplexe Objekt definiert.**</span><span class="sxs-lookup"><span data-stu-id="30548-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="30548-299">**Server-Code, der die Methode mithilfe eines komplexen Objekts aufruft.**</span><span class="sxs-lookup"><span data-stu-id="30548-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="30548-300">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="30548-300">How to call server methods from the client</span></span>

<span data-ttu-id="30548-301">Verwenden Sie zum Aufrufen einer Servermethode vom Client die `server` Eigenschaft des generierten Proxys oder `invoke` Methode für die hubproxy, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="30548-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="30548-302">Der Rückgabewert oder Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="30548-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="30548-303">Eine Camel-Case-Version des Methodennamens auf dem Hub übergeben.</span><span class="sxs-lookup"><span data-stu-id="30548-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="30548-304">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="30548-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="30548-305">Den folgenden Beispielen wird veranschaulicht, wie eine Servermethode aufrufen, die nicht über einen Rückgabewert verfügt und eine Servermethode aufrufen, die einen Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="30548-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="30548-306">**Server-Methode ohne HubMethodName-Attribut**</span><span class="sxs-lookup"><span data-stu-id="30548-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="30548-307">**Servercode, der definiert, das komplexe Objekt in einen Parameter übergeben**</span><span class="sxs-lookup"><span data-stu-id="30548-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="30548-308">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="30548-309">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="30548-310">Wenn Sie ergänzt die Hub-Methode mit einem `HubMethodName` -Attribut angegeben wird, verwenden Sie diesen Namen, ohne die Groß-/Kleinschreibung ändern.</span><span class="sxs-lookup"><span data-stu-id="30548-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="30548-311">**Servermethode** mit einem HubMethodName-Attribut</span><span class="sxs-lookup"><span data-stu-id="30548-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="30548-312">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="30548-313">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="30548-314">Siehe vorstehende Beispiele zeigen, wie eine Servermethode aufrufen, die keinen zurückgibt Wert.</span><span class="sxs-lookup"><span data-stu-id="30548-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="30548-315">Die folgenden Beispiele zeigen, wie eine Servermethode aufrufen, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="30548-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="30548-316">**Servercode für eine Methode, die über einen Rückgabewert verfügt.**</span><span class="sxs-lookup"><span data-stu-id="30548-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="30548-317">**Die Stock-Klasse, die verwendet werden, für die** Rückgabewert</span><span class="sxs-lookup"><span data-stu-id="30548-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="30548-318">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="30548-319">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="30548-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="30548-320">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="30548-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="30548-321">SignalR bietet die folgenden Verbindung Lebensdauerereignisse, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="30548-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="30548-322">`starting`: Wird ausgelöst, bevor alle Daten über die Verbindung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="30548-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="30548-323">`received`: Wird ausgelöst, wenn keine Daten für die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="30548-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="30548-324">Stellt die empfangenen Daten.</span><span class="sxs-lookup"><span data-stu-id="30548-324">Provides the received data.</span></span>
- <span data-ttu-id="30548-325">`connectionSlow`: Wird ausgelöst, wenn der Client eine Verbindung langsame oder häufig löschen erkennt.</span><span class="sxs-lookup"><span data-stu-id="30548-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="30548-326">`reconnecting`: Wird ausgelöst, wenn die zugrunde liegenden Transport erneut zu verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="30548-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="30548-327">`reconnected`: Wird ausgelöst, wenn die zugrunde liegenden Transport die Verbindung wiederhergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="30548-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="30548-328">`stateChanged`: Wird ausgelöst, wenn der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="30548-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="30548-329">Enthält der alte Status und den neuen Zustand (Herstellen einer Verbindung, verbunden, Reconnecting oder Disconnected).</span><span class="sxs-lookup"><span data-stu-id="30548-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="30548-330">`disconnected`: Wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="30548-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="30548-331">Z. B. wenn sollen Warnmeldungen angezeigt wird, wenn Verbindungsprobleme, die merkliche Verzögerungen verursachen können, behandelt der `connectionSlow` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="30548-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="30548-332">**Behandeln des Ereignisses von ConnectionSlow (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="30548-333">**Behandeln des Ereignisses ConnectionSlow (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="30548-334">Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="30548-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="30548-335">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="30548-335">How to handle errors</span></span>

<span data-ttu-id="30548-336">Der SignalR JavaScript-Client stellt eine `error` Ereignis, das Sie einen Handler für hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="30548-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="30548-337">Sie können auch die Fail-Methode verwenden, einen Handler für Fehler hinzufügen, die aus einem Methodenaufruf Server resultieren.</span><span class="sxs-lookup"><span data-stu-id="30548-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="30548-338">Wenn Sie ausführliche Fehlermeldungen auf dem Server nicht explizit aktivieren, enthält das Ausnahmeobjekt, das SignalR nach einem Fehler gibt wenige Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="30548-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="30548-339">Angenommen, wenn ein Aufruf von `newContosoChatMessage` ein Fehler auftritt, der die Fehlermeldung in die Error-Objekt enthält "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" senden, detaillierte Fehlermeldungen für Clients in einer produktionsumgebung wird nicht empfohlen aus Sicherheitsgründen aber wenn Sie möchten detaillierte Fehlermeldungen für aktivieren Problembehandlung, verwenden Sie folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="30548-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="30548-340">Das folgende Beispiel zeigt, wie einen Handler für das Fehlerereignis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="30548-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="30548-341">**Fügen Sie einen Fehlerhandler (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="30548-342">**Hinzufügen eines fehlerhandlers (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="30548-343">Im folgende Beispiel wird gezeigt, wie einen Fehler von einem Methodenaufruf behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="30548-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="30548-344">**Behandeln Sie einen Fehler aus einem Methodenaufruf (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="30548-345">**Behandeln Sie einen Fehler aus einem Methodenaufruf (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="30548-346">Ausfall ein Methodenaufrufs der `error` Ereignis wird auch ausgelöst, sodass Code in der `error` Methodenhandler und klicken Sie in der `.fail` würde dieser Rückruf Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="30548-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="30548-347">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="30548-347">How to enable client-side logging</span></span>

<span data-ttu-id="30548-348">Um die clientseitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging` Eigenschaft für das Verbindungsobjekt vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="30548-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="30548-349">**Aktivieren Sie die Protokollierung (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="30548-350">**Aktivieren Sie die Protokollierung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="30548-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="30548-351">Um die Protokolle anzuzeigen, öffnen Sie Entwicklertools von Ihrem Browser, und wechseln Sie zur Registerkarte "Console". Ein Lernprogramm, schrittweise Anweisungen und Bildschirm Screenshots enthält, die Vorgehensweise veranschaulichen, finden Sie unter [Broadcast-Server mit ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="30548-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
