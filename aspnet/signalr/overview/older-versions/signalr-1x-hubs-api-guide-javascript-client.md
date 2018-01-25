---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x Hubs-API-Handbuch - JavaScript-Client | Microsoft Docs
author: pfletcher
description: "Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 1.1 im JavaScript-Clients, z. B. Browsern und Windows Store (WinJS) applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="fde5c-103">SignalR 1.x Hubs-API-Handbuch - JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="fde5c-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="fde5c-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fde5c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="fde5c-105">Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR JavaScript-Clients, z. B. Browsern und Anwendungen für Windows Store (WinJS) Version 1.1.</span><span class="sxs-lookup"><span data-stu-id="fde5c-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="fde5c-106">Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="fde5c-107">Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="fde5c-108">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="fde5c-109">SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="fde5c-110">SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="fde5c-111">Eine Einführung in die SignalR-Hubs und dauerhafte Verbindungen oder ein Lernprogramm, das zeigt, wie Sie eine vollständige SignalR-Anwendung erstellen, finden Sie in [SignalR - erste Schritte](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="fde5c-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="fde5c-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="fde5c-112">Overview</span></span>

<span data-ttu-id="fde5c-113">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="fde5c-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="fde5c-114">Den generierten Proxy und was es für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="fde5c-115">Verwenden Sie den generierten proxy</span><span class="sxs-lookup"><span data-stu-id="fde5c-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="fde5c-116">Client-Setup</span><span class="sxs-lookup"><span data-stu-id="fde5c-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="fde5c-117">Gewusst wie: den dynamisch generierten Proxy verweisen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="fde5c-118">Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy</span><span class="sxs-lookup"><span data-stu-id="fde5c-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="fde5c-119">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="fde5c-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="fde5c-120">$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="fde5c-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="fde5c-121">Asynchrone Ausführung der Start-Methode</span><span class="sxs-lookup"><span data-stu-id="fde5c-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="fde5c-122">Gewusst wie: Herstellen einer Verbindung domänenübergreifende</span><span class="sxs-lookup"><span data-stu-id="fde5c-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="fde5c-123">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="fde5c-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="fde5c-124">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="fde5c-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="fde5c-125">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="fde5c-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="fde5c-126">Gewusst wie: Abrufen ein Proxys für eine hubklasse</span><span class="sxs-lookup"><span data-stu-id="fde5c-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="fde5c-127">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="fde5c-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="fde5c-128">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="fde5c-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="fde5c-129">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="fde5c-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="fde5c-130">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="fde5c-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="fde5c-131">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="fde5c-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="fde5c-132">Dokumentation über die Programmierung des Servers oder Clients .NET finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="fde5c-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="fde5c-133">SignalR-Hubs-API-Leitfaden – Server</span><span class="sxs-lookup"><span data-stu-id="fde5c-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="fde5c-134">SignalR-Hubs-API-Leitfaden – .NET Client</span><span class="sxs-lookup"><span data-stu-id="fde5c-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="fde5c-135">Links zu API-Referenzthemen sind auf die .NET 4.5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="fde5c-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="fde5c-136">Wenn Sie .NET 4 verwenden, finden Sie unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="fde5c-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="fde5c-137">Den generierten Proxy und was es für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="fde5c-138">Sie können einen JavaScript-Client für die Kommunikation mit einer SignalR-Dienst mit oder ohne einen Proxy aus, dem SignalR für Sie generiert programmieren.</span><span class="sxs-lookup"><span data-stu-id="fde5c-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="fde5c-139">Leistungsumfang der Proxy für Sie ist die Syntax des Codes zu vereinfachen, die Verbindung verwenden, Write-Methoden, die der Server aufgerufen wird, und rufen Methoden auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="fde5c-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="fde5c-140">Beim Schreiben von Code zum Aufrufen von Servermethoden, der generierte Proxy können Sie Syntax zu verwenden, das aussieht, als wären Sie eine lokale Funktion ausgeführt wurden: Sie können schreiben `serverMethod(arg1, arg2)` anstelle von `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="fde5c-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="fde5c-141">Die generierten Proxy-Syntax kann auch Fehler clientseitige sofortigen und verständlicher, wenn Sie einen Server-Methodennamen richtig eingegeben.</span><span class="sxs-lookup"><span data-stu-id="fde5c-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="fde5c-142">Und wenn Sie manuell die Datei erstellen, die die Proxys definiert außerdem erhalten Sie IntelliSense-Unterstützung für das Schreiben von Code, der Servermethoden aufruft.</span><span class="sxs-lookup"><span data-stu-id="fde5c-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="fde5c-143">Nehmen wir beispielsweise an, dass Sie die folgenden hubklasse auf dem Server verfügen:</span><span class="sxs-lookup"><span data-stu-id="fde5c-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="fde5c-144">Die folgenden Codebeispiele zeigen, wie sieht der JavaScript-Code zum Aufrufen der `NewContosoChatMessage` Methode auf dem Server und die Aufrufe der Empfang der `addContosoChatMessageToPage` Methode vom Server.</span><span class="sxs-lookup"><span data-stu-id="fde5c-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="fde5c-145">**Mit den generierten proxy**</span><span class="sxs-lookup"><span data-stu-id="fde5c-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="fde5c-146">**Ohne den generierten proxy**</span><span class="sxs-lookup"><span data-stu-id="fde5c-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="fde5c-147">Verwenden Sie den generierten proxy</span><span class="sxs-lookup"><span data-stu-id="fde5c-147">When to use the generated proxy</span></span>

<span data-ttu-id="fde5c-148">Wenn Sie mehrere Ereignishandler für eine Clientmethode zu registrieren, die der Server ruft möchten, können nicht Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="fde5c-149">Andernfalls können auch den generierten Proxy verwenden oder nicht auf die Einstellung für die Codierung basieren.</span><span class="sxs-lookup"><span data-stu-id="fde5c-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="fde5c-150">Wenn Sie nicht verwenden, müssen Sie keine URL / "Signalr-Hubs" in verweisen eine `script` Element im Clientcode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="fde5c-151">Client-setup</span><span class="sxs-lookup"><span data-stu-id="fde5c-151">Client setup</span></span>

<span data-ttu-id="fde5c-152">Ein JavaScript-Client erfordert Verweise auf jQuery und der SignalR Core JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="fde5c-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="fde5c-153">Die jQuery-Version muss es sich um 1.6.4 oder höher Hauptversionen, z. B. 1.7.2, 1.8.2 oder 1.9.1 sein.</span><span class="sxs-lookup"><span data-stu-id="fde5c-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="fde5c-154">Wenn Sie den generierten Proxy verwenden möchten, benötigen Sie auch einen Verweis auf den Proxy generiert SignalR JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="fde5c-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="fde5c-155">Das folgende Beispiel zeigt, wie die Verweise in einer HTML-Seite aussehen könnte, die den generierten Proxy verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde5c-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="fde5c-156">Diese Verweise in dieser Reihenfolge enthalten sein müssen: jQuery SignalR Core danach und SignalR-Proxys First-und last.</span><span class="sxs-lookup"><span data-stu-id="fde5c-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="fde5c-157">Gewusst wie: den dynamisch generierten Proxy verweisen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="fde5c-158">Im vorherigen Beispiel wird der Verweis auf den Proxy SignalR generiert für dynamisch generierte JavaScript-Code, nicht zu einer physischen Datei.</span><span class="sxs-lookup"><span data-stu-id="fde5c-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="fde5c-159">SignalR JavaScript-Code für den Proxy bei Bedarf erstellt und an dem Client als Antwort auf die URL "/ Signalr/Hubs" dient.</span><span class="sxs-lookup"><span data-stu-id="fde5c-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="fde5c-160">Bei Angabe eine andere base-URL für SignalR-Verbindungen auf dem Server in Ihrer `MapHubs` -Methode, die URL für die dynamisch generierte Proxydatei ist Ihre benutzerdefinierte URL mit "/ Hubs" angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="fde5c-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="fde5c-161">Verwenden Sie für Windows 8 (Windows Store) JavaScript-Clients die physische Proxydatei statt der dynamisch generierten.</span><span class="sxs-lookup"><span data-stu-id="fde5c-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="fde5c-162">Weitere Informationen finden Sie unter [zum Erstellen einer physischen Datei für SignalR generierten Proxy](#manualproxy) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="fde5c-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="fde5c-163">Verwenden Sie die Tilde zum Verweisen auf das Stammverzeichnis der Anwendung in Ihrem Proxy Dateiverweis, in einer ASP.NET MVC 4-Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="fde5c-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="fde5c-164">Weitere Informationen zur Verwendung von SignalR in MVC 4 finden Sie unter [erste Schritte mit SignalR und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="fde5c-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="fde5c-165">Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Verweis der Proxy-Datei:</span><span class="sxs-lookup"><span data-stu-id="fde5c-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="fde5c-166">Verwenden Sie in einer ASP.NET Web Forms-Anwendung `ResolveClientUrl` für die Proxys Verweis der Datei, oder registrieren Sie ihn über das ScriptManager mit einem relativen Pfad Stamm app (beginnend mit einer Tilde):</span><span class="sxs-lookup"><span data-stu-id="fde5c-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="fde5c-167">Verwenden Sie als allgemeine Regel die gleiche Methode zum Angeben der URL "/ Signalr/Hubs", die Sie für CSS oder JavaScript-Dateien verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="fde5c-168">Wenn Sie eine URL angeben, ohne mit einer Tilde, wird in einigen Szenarien Ihrer Anwendung fehlerfrei, wenn Sie in Visual Studio mit IIS Express testen schlägt jedoch fehl mit Fehler 404 während der Bereitstellung auf vollständige IIS.</span><span class="sxs-lookup"><span data-stu-id="fde5c-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="fde5c-169">Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen auf der Stammebene** in [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="fde5c-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="fde5c-170">Wenn Sie ein Webprojekt in Visual Studio 2012 im Debugmodus ausgeführt, und wenn Sie Internet Explorer als Ihren Browser verwenden, Sie die Proxydatei in sehen **Projektmappen-Explorer** unter **Skriptdokumente**, entsprechend der folgende Abbildung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![JavaScript generiert eine Proxyklasse im Projektmappen-Explorer](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="fde5c-172">Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="fde5c-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="fde5c-173">Wenn Sie Visual Studio 2012 und Internet Explorer nicht verwenden oder wenn Sie nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei zu suchen und die URL "/ SignalR/Hubs" abrufen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="fde5c-174">Beispielsweise wird bei der Ausführung des Standorts auf `http://localhost:56699`, wechseln Sie zu `http://localhost:56699/SignalR/hubs` in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="fde5c-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="fde5c-175">Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy</span><span class="sxs-lookup"><span data-stu-id="fde5c-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="fde5c-176">Als Alternative zum dynamisch generierten Proxy können Sie erstellen eine physische Datei, die den Proxycode verfügt und diese Datei verweisen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="fde5c-177">Sie sollten sich zur Steuerung von Inhalte zwischengespeichert oder bundling Verhalten dazu oder IntelliSense abzurufen, bei der Codierung Aufrufe von Servermethoden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="fde5c-178">Um eine Proxydatei zu erstellen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="fde5c-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="fde5c-179">Installieren der [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="fde5c-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="fde5c-180">Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu der *Tools* Ordner, der die SignalR.exe-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="fde5c-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="fde5c-181">Ordner "Tools" wird am folgenden Speicherort:</span><span class="sxs-lookup"><span data-stu-id="fde5c-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="fde5c-182">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="fde5c-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="fde5c-183">Der Pfad zu Ihrer *DLL* ist in der Regel die *"bin"* Ordner im Projektordner.</span><span class="sxs-lookup"><span data-stu-id="fde5c-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="fde5c-184">Dieser Befehl erstellt eine Datei namens *server.js* im gleichen Ordner wie *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="fde5c-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="fde5c-185">Versetzen der *server.js* Datei in eine entsprechende Ordner im Projekt, benennen Sie sie nach Bedarf für Ihre Anwendung und einen Verweis darauf anstelle der / "Signalr-Hubs" Verweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="fde5c-186">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="fde5c-186">How to establish a connection</span></span>

<span data-ttu-id="fde5c-187">Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungsobjekt zu erstellen, erstellen einen Proxy und registrieren Sie Ereignishandler für Methoden, die vom Server aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="fde5c-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="fde5c-188">Wenn Sie den Proxy und Ereignishandler eingerichtet wurden, Herstellung einer Verbindung durch Aufrufen der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="fde5c-189">Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungsobjekt in Ihrem eigenen Code erstellt werden, da der generierte Proxycode für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="fde5c-190">**Herstellen einer Verbindungs (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="fde5c-191">**Herstellen einer Verbindungs (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="fde5c-192">Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst.</span><span class="sxs-lookup"><span data-stu-id="fde5c-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fde5c-193">Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fde5c-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="fde5c-194">Normalerweise registrieren Sie Ereignishandler vor dem Aufruf der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="fde5c-195">Wenn einige Ereignishandler zu registrieren, nach dem Herstellen der Verbindung werden sollen, können Sie dies tun, aber Sie müssen mindestens eine Ihrer Ereignis einer oder mehrere Ereignishandler vor dem Aufruf Registrieren der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="fde5c-196">Ein Grund dafür ist, dass in einer Anwendung können viele Hubs vorhanden sein, aber nicht empfehlenswert Auslösen der `OnConnected` Ereignis für jedes Hub, wenn Sie nur eines davon verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="fde5c-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="fde5c-197">Wenn die Verbindung hergestellt wird, das Vorhandensein einer Client-Methode auf einem Hub-Proxy ist womit SignalR zum Auslösen der `OnConnected` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="fde5c-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="fde5c-198">Alle Ereignishandler vor dem Aufruf registriert die `start` -Methode, Sie werden zum Aufrufen von Methoden der Hub, aber auf des Hubs `OnConnected` Methode wird nicht aufgerufen werden und keine Clientmethoden werden vom Server aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="fde5c-199">$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="fde5c-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="fde5c-200">Wie Sie aus den Beispielen sehen können, wenn Sie den generierten Proxy verwenden `$.connection.hub` bezieht sich auf das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="fde5c-201">Dies ist das gleiche Objekt, die Sie durch den Aufruf erhalten `$.hubConnection()` Sie sind nicht bei den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="fde5c-202">Der generierte Proxycode erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:</span><span class="sxs-lookup"><span data-stu-id="fde5c-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Erstellen eine Verbindung in die generierte Proxydatei](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="fde5c-204">Wenn Sie den generierten Proxy verwenden, können Sie alle mit Aktionen `$.connection.hub` , dass Sie mit einem Verbindungsobjekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="fde5c-205">Asynchrone Ausführung der Start-Methode</span><span class="sxs-lookup"><span data-stu-id="fde5c-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="fde5c-206">Die `start` Methode asynchron ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fde5c-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="fde5c-207">Gibt eine [jQuery zurückgestellt Objekt](http://api.jquery.com/category/deferred-object/), was bedeutet, dass Sie Rückruffunktionen hinzufügen können, durch Aufrufen von Methoden wie z. B. `pipe`, `done`, und `fail`.</span><span class="sxs-lookup"><span data-stu-id="fde5c-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="fde5c-208">Wenn Sie über Code, die verfügen nach dem Herstellen der Verbindung ausgeführt werden soll, z. B. einen Aufruf einer Servermethode, legen Sie diesen Code in eine Rückruffunktion oder aus eine Rückruffunktion aufgerufen Sie wird.</span><span class="sxs-lookup"><span data-stu-id="fde5c-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="fde5c-209">Die `.done` Rückrufmethode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach der code Ihrer `OnConnected` Ereignishandlermethode auf dem Server die Ausführung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="fde5c-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="fde5c-210">Wenn Sie die Anweisung "Jetzt verbunden" aus dem vorherigen Beispiel als die nächste Zeile des Codes nach dem Einfügen der `start` Methodenaufruf (nicht in einer `.done` Rückruf), die `console.log` Zeile ausgeführt, bevor die Verbindung hergestellt wird, wie das folgende Beispiel zeigt Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fde5c-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Falsches vorgehen, Code zu schreiben, der ausgeführt wird, nachdem die Verbindung hergestellt wird](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="fde5c-212">Gewusst wie: Herstellen einer Verbindung domänenübergreifende</span><span class="sxs-lookup"><span data-stu-id="fde5c-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="fde5c-213">In der Regel, wenn der Browser eine Seite aus lädt `http://contoso.com`, die SignalR-Verbindung ist auf der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="fde5c-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="fde5c-214">Wenn die Seite `http://contoso.com` stellt eine Verbindung mit `http://fabrikam.com/signalr`, d. h. eine domänenübergreifende-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="fde5c-215">Domänenübergreifende Verbindungen werden aus Gründen der Sicherheit standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="fde5c-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="fde5c-216">Um eine Verbindung mit der domänenübergreifende, stellen Sie sicher, dass domänenübergreifende Verbindungen auf dem Server aktiviert sind, und geben Sie den Verbindungs-URL ein, wenn Sie das Verbindungsobjekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="fde5c-217">SignalR verwendet die geeignete Technologie für domänenübergreifende Verbindungen, z. B. [JSONP](http://en.wikipedia.org/wiki/JSONP) oder [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="fde5c-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="fde5c-218">Aktivieren Sie auf dem Server domänenübergreifende Verbindungen durch Auswählen dieser Option beim Aufrufen der `MapHubs` Methode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="fde5c-219">Geben Sie die URL auf dem Client Wenn Sie das Verbindungsobjekt (ohne den generierten Proxy) erstellen oder vor dem Aufrufen der Start-Methode (mit den generierten Proxy).</span><span class="sxs-lookup"><span data-stu-id="fde5c-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="fde5c-220">**Clientcode, der angibt, eine domänenübergreifende-Verbindung (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="fde5c-221">**Clientcode, der angibt, eine domänenübergreifende-Verbindung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="fde5c-222">Bei Verwendung der `$.hubConnection` Konstruktor verfügen Sie nicht einschließen `signalr` in der URL wird automatisch hinzugefügt (es sei denn, Sie geben `useDefaultUrl` als `false`).</span><span class="sxs-lookup"><span data-stu-id="fde5c-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="fde5c-223">Sie können mehrere Verbindungen an verschiedene Endpunkte erstellen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="fde5c-224">Legen Sie die keine `jQuery.support.cors` auf "true" in Ihrem Code.</span><span class="sxs-lookup"><span data-stu-id="fde5c-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Setzen Sie jQuery.support.cors nicht auf "true"](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="fde5c-226">SignalR behandelt die Verwendung von JSONP oder CORS.</span><span class="sxs-lookup"><span data-stu-id="fde5c-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="fde5c-227">Festlegen von `jQuery.support.cors` auf "true" JSONP deaktiviert, da er bewirkt, dass SignalR anzunehmen, der Browser unterstützt CORS.</span><span class="sxs-lookup"><span data-stu-id="fde5c-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="fde5c-228">Wenn Sie eine Verbindung zu einer URL "localhost" herstellen, wird nicht Internet Explorer 10 eine Verbindung domänenübergreifende beachtet werden, damit die Anwendung lokal mit Internet Explorer 10 verwendet werden kann, selbst wenn Sie die domänenübergreifende Verbindungen auf dem Server aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="fde5c-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="fde5c-229">Weitere Informationen zur Verwendung von domänenübergreifende Verbindungen mit Internet Explorer 9, finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="fde5c-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="fde5c-230">Informationen zur Verwendung von domänenübergreifende Verbindungen mit Chrome finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="fde5c-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="fde5c-231">Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst.</span><span class="sxs-lookup"><span data-stu-id="fde5c-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fde5c-232">Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fde5c-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="fde5c-233">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="fde5c-233">How to configure the connection</span></span>

<span data-ttu-id="fde5c-234">Bevor Sie eine Verbindung herstellen, können Sie Abfragezeichenfolgen-Parameter angeben oder geben Sie die Transportmethode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="fde5c-235">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="fde5c-235">How to specify query string parameters</span></span>

<span data-ttu-id="fde5c-236">Wenn Sie möchten Daten an den Server senden, wenn der Client eine Verbindung herstellt, können Sie Abfragezeichenfolgen-Parameter an das Verbindungsobjekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="fde5c-237">Die folgenden Beispiele zeigen, wie einen Abfragezeichenfolgenparameter in Clientcode festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="fde5c-238">**Einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (mit den generierten Proxy) festgelegt**</span><span class="sxs-lookup"><span data-stu-id="fde5c-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="fde5c-239">**Legen Sie einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="fde5c-240">Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgenparameter in Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="fde5c-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="fde5c-241">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="fde5c-241">How to specify the transport method</span></span>

<span data-ttu-id="fde5c-242">Im Rahmen des Prozesses eine Verbindung herstellen verhandelt SignalR-Client normalerweise mit dem Server zu den bewährten Transport ermitteln kann, der unterstützt wird, indem sowohl Server-als auch.</span><span class="sxs-lookup"><span data-stu-id="fde5c-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="fde5c-243">Wenn Sie bereits wissen, dass der Transportmethode verwenden möchten, können Sie die dabei Aushandlung umgehen, indem die Transportmethode angeben, beim Aufrufen der `start` Methode.</span><span class="sxs-lookup"><span data-stu-id="fde5c-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="fde5c-244">**Clientcode, der angibt, die Transportmethode (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="fde5c-245">**Clientcode, der angibt, die Transportmethode (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="fde5c-246">Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in denen Sie diese ausprobieren SignalR werden soll:</span><span class="sxs-lookup"><span data-stu-id="fde5c-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="fde5c-247">**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="fde5c-248">**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="fde5c-249">Sie können die folgenden Werte zur Angabe der Transportmethode verwenden:</span><span class="sxs-lookup"><span data-stu-id="fde5c-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="fde5c-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="fde5c-250">"webSockets"</span></span>
- <span data-ttu-id="fde5c-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="fde5c-251">"foreverFrame"</span></span>
- <span data-ttu-id="fde5c-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="fde5c-252">"serverSentEvents"</span></span>
- <span data-ttu-id="fde5c-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="fde5c-253">"longPolling"</span></span>

<span data-ttu-id="fde5c-254">Die folgenden Beispiele zeigen, wie Sie herausfinden, welche Transportmethode von eine Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fde5c-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="fde5c-255">**Clientcode, der die Transportmethode, die von einer Verbindung (mit den generierten Proxy) verwendete zeigt**</span><span class="sxs-lookup"><span data-stu-id="fde5c-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="fde5c-256">**Clientcode, der die Transportmethode, die von einer Verbindung (ohne den generierten Proxy) verwendete zeigt**</span><span class="sxs-lookup"><span data-stu-id="fde5c-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="fde5c-257">Informationen zum Überprüfen der Transportmethode in Servercode finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - zum Abrufen von Informationen über den Client aus der Kontexteigenschaft](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="fde5c-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="fde5c-258">Weitere Informationen zu Transporten und Zugriffe, finden Sie unter [Einführung in die SignalR - Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fde5c-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="fde5c-259">Gewusst wie: Abrufen ein Proxys für eine hubklasse</span><span class="sxs-lookup"><span data-stu-id="fde5c-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="fde5c-260">Jedes Verbindungsobjekt, das Sie erstellen kapselt Informationen über eine Verbindung mit einer SignalR-Dienst, der eine oder mehrere Hub-Klassen enthält.</span><span class="sxs-lookup"><span data-stu-id="fde5c-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="fde5c-261">Verwenden Sie für die Kommunikation mit einem hubklasse ein Proxyobjekt aus, die Sie erstellen selbst (sofern Sie nicht den generierten Proxy verwenden) oder der für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="fde5c-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="fde5c-262">Auf dem Client ist der Proxyname einer Version in Kamel-Schreibweise des Klassennamens Hub.</span><span class="sxs-lookup"><span data-stu-id="fde5c-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="fde5c-263">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="fde5c-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fde5c-264">**Hubklasse auf server**</span><span class="sxs-lookup"><span data-stu-id="fde5c-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="fde5c-265">**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="fde5c-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="fde5c-266">**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="fde5c-267">Wenn Sie ergänzen die Hub-Klasse mit einem `HubName` -Attribut angegeben wird, verwenden Sie den genauen Namen ohne die Groß-/Kleinschreibung ändern.</span><span class="sxs-lookup"><span data-stu-id="fde5c-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="fde5c-268">**Hubklasse auf Server mit HubName-Attribut**</span><span class="sxs-lookup"><span data-stu-id="fde5c-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="fde5c-269">**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="fde5c-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="fde5c-270">**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="fde5c-271">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="fde5c-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="fde5c-272">Um eine Methode definieren, die der Server mit einem Hub aufrufen können, fügen Sie einen Ereignishandler auf den Hub-Proxy mithilfe der `client` Eigenschaft des generierten Proxys oder der Aufruf der `on` Methode, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="fde5c-273">Die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="fde5c-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="fde5c-274">Fügen Sie dem Ereignishandler hinzu, vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="fde5c-275">(Wenn Sie nach dem Aufruf Ereignishandler hinzufügen möchten die `start` -Methode finden Sie im Hinweis in [zum Herstellen eine Verbindung](#establishconnection) weiter oben in diesem Dokument, und verwenden Sie die Syntax für die Definition einer Methode ohne Verwendung des generierten Proxys veranschaulicht.)</span><span class="sxs-lookup"><span data-stu-id="fde5c-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="fde5c-276">Methode-namenszuordnung wird Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="fde5c-277">Beispielsweise `Clients.All.addContosoChatMessageToPage` auf dem Server führt `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, oder `addcontosochatmessagetopage` auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="fde5c-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="fde5c-278">**Definieren Sie die Methode auf dem Client (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="fde5c-279">**Alternative Möglichkeit zum Definieren der Methode auf dem Client (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="fde5c-280">**Definieren Sie die Methode auf dem Client (ohne den generierten Proxy oder beim Hinzufügen von nach dem Aufrufen der Start-Methode)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="fde5c-281">**Server-Code, der die Methode aufruft.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="fde5c-282">Im folgenden Beispiel werden ein komplexes Objekt als Parameter einer Methode enthalten.</span><span class="sxs-lookup"><span data-stu-id="fde5c-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="fde5c-283">**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (mit den generierten Proxy) akzeptiert.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="fde5c-284">**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (ohne den generierten Proxy) akzeptiert.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="fde5c-285">**Servercode, die das komplexe Objekt definiert.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="fde5c-286">**Server-Code, der die Methode mithilfe eines komplexen Objekts aufruft.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="fde5c-287">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="fde5c-287">How to call server methods from the client</span></span>

<span data-ttu-id="fde5c-288">Verwenden Sie zum Aufrufen einer Servermethode vom Client die `server` Eigenschaft des generierten Proxys oder `invoke` Methode für die hubproxy, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="fde5c-289">Der Rückgabewert oder Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="fde5c-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="fde5c-290">Eine Camel-Case-Version des Methodennamens auf dem Hub übergeben.</span><span class="sxs-lookup"><span data-stu-id="fde5c-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="fde5c-291">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="fde5c-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fde5c-292">Den folgenden Beispielen wird veranschaulicht, wie eine Servermethode aufrufen, die nicht über einen Rückgabewert verfügt und eine Servermethode aufrufen, die einen Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="fde5c-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="fde5c-293">**Server-Methode ohne HubMethodName-Attribut**</span><span class="sxs-lookup"><span data-stu-id="fde5c-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="fde5c-294">**Servercode, der definiert, das komplexe Objekt in einen Parameter übergeben**</span><span class="sxs-lookup"><span data-stu-id="fde5c-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="fde5c-295">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="fde5c-296">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="fde5c-297">Wenn Sie ergänzt die Hub-Methode mit einem `HubMethodName` -Attribut angegeben wird, verwenden Sie diesen Namen, ohne die Groß-/Kleinschreibung ändern.</span><span class="sxs-lookup"><span data-stu-id="fde5c-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="fde5c-298">**Servermethode** mit einem HubMethodName-Attribut</span><span class="sxs-lookup"><span data-stu-id="fde5c-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="fde5c-299">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="fde5c-300">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="fde5c-301">Siehe vorstehende Beispiele zeigen, wie eine Servermethode aufrufen, die keinen zurückgibt Wert.</span><span class="sxs-lookup"><span data-stu-id="fde5c-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="fde5c-302">Die folgenden Beispiele zeigen, wie eine Servermethode aufrufen, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="fde5c-303">**Servercode für eine Methode, die über einen Rückgabewert verfügt.**</span><span class="sxs-lookup"><span data-stu-id="fde5c-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="fde5c-304">**Die Stock-Klasse, die verwendet werden, für die** Rückgabewert</span><span class="sxs-lookup"><span data-stu-id="fde5c-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="fde5c-305">**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="fde5c-306">**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="fde5c-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="fde5c-307">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="fde5c-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="fde5c-308">SignalR bietet die folgenden Verbindung Lebensdauerereignisse, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="fde5c-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="fde5c-309">`starting`: Wird ausgelöst, bevor alle Daten über die Verbindung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="fde5c-310">`received`: Wird ausgelöst, wenn keine Daten für die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="fde5c-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="fde5c-311">Stellt die empfangenen Daten.</span><span class="sxs-lookup"><span data-stu-id="fde5c-311">Provides the received data.</span></span>
- <span data-ttu-id="fde5c-312">`connectionSlow`: Wird ausgelöst, wenn der Client eine Verbindung langsame oder häufig löschen erkennt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="fde5c-313">`reconnecting`: Wird ausgelöst, wenn die zugrunde liegenden Transport erneut zu verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="fde5c-314">`reconnected`: Wird ausgelöst, wenn die zugrunde liegenden Transport die Verbindung wiederhergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="fde5c-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="fde5c-315">`stateChanged`: Wird ausgelöst, wenn der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="fde5c-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="fde5c-316">Enthält der alte Status und den neuen Zustand (Herstellen einer Verbindung, verbunden, Reconnecting oder Disconnected).</span><span class="sxs-lookup"><span data-stu-id="fde5c-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="fde5c-317">`disconnected`: Wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="fde5c-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="fde5c-318">Z. B. wenn sollen Warnmeldungen angezeigt wird, wenn Verbindungsprobleme, die merkliche Verzögerungen verursachen können, behandelt der `connectionSlow` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="fde5c-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="fde5c-319">**Behandeln des Ereignisses von ConnectionSlow (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="fde5c-320">**Behandeln des Ereignisses ConnectionSlow (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="fde5c-321">Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="fde5c-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="fde5c-322">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="fde5c-322">How to handle errors</span></span>

<span data-ttu-id="fde5c-323">Der SignalR JavaScript-Client stellt eine `error` Ereignis, das Sie einen Handler für hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="fde5c-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="fde5c-324">Sie können auch die Fail-Methode verwenden, einen Handler für Fehler hinzufügen, die aus einem Methodenaufruf Server resultieren.</span><span class="sxs-lookup"><span data-stu-id="fde5c-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="fde5c-325">Wenn Sie ausführliche Fehlermeldungen auf dem Server nicht explizit aktivieren, enthält das Ausnahmeobjekt, das SignalR nach einem Fehler gibt wenige Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="fde5c-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="fde5c-326">Angenommen, wenn ein Aufruf von `newContosoChatMessage` ein Fehler auftritt, der die Fehlermeldung in die Error-Objekt enthält "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" senden, detaillierte Fehlermeldungen für Clients in einer produktionsumgebung wird nicht empfohlen aus Sicherheitsgründen aber wenn Sie möchten detaillierte Fehlermeldungen für aktivieren Problembehandlung, verwenden Sie folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="fde5c-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="fde5c-327">Das folgende Beispiel zeigt, wie einen Handler für das Fehlerereignis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fde5c-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="fde5c-328">**Fügen Sie einen Fehlerhandler (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="fde5c-329">**Hinzufügen eines fehlerhandlers (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="fde5c-330">Im folgende Beispiel wird gezeigt, wie einen Fehler von einem Methodenaufruf behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="fde5c-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="fde5c-331">**Behandeln Sie einen Fehler aus einem Methodenaufruf (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="fde5c-332">**Behandeln Sie einen Fehler aus einem Methodenaufruf (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="fde5c-333">Ausfall ein Methodenaufrufs der `error` Ereignis wird auch ausgelöst, sodass Code in der `error` Methodenhandler und klicken Sie in der `.fail` würde dieser Rückruf Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="fde5c-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="fde5c-334">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="fde5c-334">How to enable client-side logging</span></span>

<span data-ttu-id="fde5c-335">Um die clientseitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging` Eigenschaft für das Verbindungsobjekt vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="fde5c-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="fde5c-336">**Aktivieren Sie die Protokollierung (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="fde5c-337">**Aktivieren Sie die Protokollierung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="fde5c-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="fde5c-338">Um die Protokolle anzuzeigen, öffnen Sie Entwicklertools von Ihrem Browser, und wechseln Sie zur Registerkarte "Console". Ein Lernprogramm, schrittweise Anweisungen und Bildschirm Screenshots enthält, die Vorgehensweise veranschaulichen, finden Sie unter [Broadcast-Server mit ASP.NET Signalr - Enable Logging](index.md).</span><span class="sxs-lookup"><span data-stu-id="fde5c-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
