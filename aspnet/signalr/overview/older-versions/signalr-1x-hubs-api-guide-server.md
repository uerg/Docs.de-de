---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR-Hubs-API-Handbuch - Server (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: "Dieses Dokument enthält eine Einführung in die Programmierung von der Serverseite der ASP.NET SignalR-Hubs-API für SignalR Version 1.1, mit Code Samples Demonstratin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="f19fc-103">ASP.NET SignalR-Hubs-API-Handbuch - Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f19fc-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f19fc-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f19fc-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="f19fc-105">Dieses Dokument enthält eine Einführung in das Programmieren von der Serverseite der ASP.NET SignalR-Hubs-API für SignalR Version 1.1, mit Codebeispiele zur allgemeine Optionen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="f19fc-106">Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f19fc-107">Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f19fc-108">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f19fc-109">SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="f19fc-110">SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f19fc-111">Eine Einführung in die SignalR-Hubs und dauerhafte Verbindungen oder ein Lernprogramm, das zeigt, wie Sie eine vollständige SignalR-Anwendung erstellen, finden Sie in [SignalR - erste Schritte](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f19fc-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f19fc-112">Overview</span></span>

<span data-ttu-id="f19fc-113">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="f19fc-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="f19fc-114">Die SignalR-Route zu registrieren und Konfigurieren von SignalR-Optionen</span><span class="sxs-lookup"><span data-stu-id="f19fc-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="f19fc-115">Die /signalr-URL</span><span class="sxs-lookup"><span data-stu-id="f19fc-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="f19fc-116">Konfigurieren von SignalR-Optionen</span><span class="sxs-lookup"><span data-stu-id="f19fc-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="f19fc-117">Das Erstellen und Verwenden der Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="f19fc-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="f19fc-118">Lebensdauer eines Objekts "Hub"</span><span class="sxs-lookup"><span data-stu-id="f19fc-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="f19fc-119">In Kamel-Schreibweise des Hub-Namen in der JavaScript-clients</span><span class="sxs-lookup"><span data-stu-id="f19fc-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="f19fc-120">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="f19fc-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="f19fc-121">Zum Definieren von Methoden in der Hub-Klasse, die Clients aufrufen können</span><span class="sxs-lookup"><span data-stu-id="f19fc-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="f19fc-122">Binnenmajuskel Methodennamen in JavaScript-clients</span><span class="sxs-lookup"><span data-stu-id="f19fc-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="f19fc-123">Beim asynchron ausführen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="f19fc-124">Definieren von Überladungen</span><span class="sxs-lookup"><span data-stu-id="f19fc-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="f19fc-125">Gewusst wie: Client Methoden aufrufen, von der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="f19fc-126">Auswählen von welchen Clients erhalten die RPC</span><span class="sxs-lookup"><span data-stu-id="f19fc-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="f19fc-127">Keine Validierung während der Kompilierung für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="f19fc-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="f19fc-128">Namensübereinstimmung Groß-/Kleinschreibung-Methode</span><span class="sxs-lookup"><span data-stu-id="f19fc-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="f19fc-129">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="f19fc-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="f19fc-130">Zum Verwalten von Gruppenmitgliedschaften aus der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="f19fc-131">Asynchrone Ausführung der Add- und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="f19fc-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="f19fc-132">Gruppenmitgliedschaft Persistenz</span><span class="sxs-lookup"><span data-stu-id="f19fc-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="f19fc-133">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="f19fc-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="f19fc-134">Behandeln von Lebensdauer Verbindungsereignisse in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="f19fc-135">Wenn OnConnected OnDisconnected und OnReconnected aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="f19fc-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="f19fc-136">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="f19fc-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="f19fc-137">Gewusst wie: Abrufen von Informationen über den Client aus der Kontexteigenschaft</span><span class="sxs-lookup"><span data-stu-id="f19fc-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="f19fc-138">Gewusst wie: Zustand zwischen Clients und der Hub-Klasse übergeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="f19fc-139">Gewusst wie: Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="f19fc-140">Das Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="f19fc-141">Aufrufen von Clientmethoden</span><span class="sxs-lookup"><span data-stu-id="f19fc-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="f19fc-142">Verwalten von Gruppenmitgliedschaften</span><span class="sxs-lookup"><span data-stu-id="f19fc-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="f19fc-143">Gewusst wie: Aktivieren der Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="f19fc-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="f19fc-144">Gewusst wie: Anpassen die Pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="f19fc-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="f19fc-145">Dokumentation zum Programm Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="f19fc-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="f19fc-146">SignalR-Hubs-API-Handbuch - JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="f19fc-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="f19fc-147">SignalR-Hubs-API-Leitfaden – .NET Client</span><span class="sxs-lookup"><span data-stu-id="f19fc-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="f19fc-148">Links zu API-Referenzthemen sind auf die .NET 4.5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="f19fc-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="f19fc-149">Wenn Sie .NET 4 verwenden, finden Sie unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="f19fc-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="f19fc-150">Die SignalR-Route zu registrieren und Konfigurieren von SignalR-Optionen</span><span class="sxs-lookup"><span data-stu-id="f19fc-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="f19fc-151">Aufrufen, um die Route definieren, die Clients zur Verbindung mit Ihren Hubs verwendet wird, die [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="f19fc-152">`MapHubs`ist ein [Erweiterungsmethode](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) für die `System.Web.Routing.RouteCollection` Klasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="f19fc-153">Im folgende Beispiel wird gezeigt, wie die SignalR-Hubs Route im Definieren der *"Global.asax"* Datei.</span><span class="sxs-lookup"><span data-stu-id="f19fc-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="f19fc-154">Wenn Sie SignalR-Funktionen zu einer ASP.NET MVC-Anwendung hinzugefügt werden, stellen Sie sicher, dass die SignalR-Route, bevor die anderen Routen hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="f19fc-155">Weitere Informationen finden Sie unter [Lernprogramm: Erste Schritte mit SignalR und MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="f19fc-156">Die /signalr-URL</span><span class="sxs-lookup"><span data-stu-id="f19fc-156">The /signalr URL</span></span>

<span data-ttu-id="f19fc-157">Standardmäßig ist die Routen-URL, die von Clients verwendet werden, für die Verbindung mit Ihrem Hub "/ Signalr".</span><span class="sxs-lookup"><span data-stu-id="f19fc-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="f19fc-158">(Verwechseln Sie nicht diese URL durch die URL "/ Signalr/Hubs", der für die automatisch generierte JavaScript-Datei ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="f19fc-159">Weitere Informationen zu den generierten Proxy, finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - generierte Proxy und wozu Sie](index.md).)</span><span class="sxs-lookup"><span data-stu-id="f19fc-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="f19fc-160">Es gibt möglicherweise Ausnahmesituationen, die diese Basis-URL für SignalR nicht verwendbar machen; Angenommen, Sie verfügen über einen Ordner in Ihrem Projekt mit dem Namen *Signalr* und nicht den Namen ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="f19fc-161">In diesem Fall können Sie die base-URL ändern, wie in den folgenden Beispielen gezeigt (ersetzen Sie "/ Signalr" im Code durch die URL Ihres gewünschten).</span><span class="sxs-lookup"><span data-stu-id="f19fc-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="f19fc-162">**Servercode, der die URL angibt.**</span><span class="sxs-lookup"><span data-stu-id="f19fc-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="f19fc-163">**JavaScript-Clientcode, der angibt, die URL (mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="f19fc-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="f19fc-164">**JavaScript-Clientcode, der angibt, die URL (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="f19fc-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="f19fc-165">**.NET Clientcode, der die URL angibt.**</span><span class="sxs-lookup"><span data-stu-id="f19fc-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="f19fc-166">Konfigurieren von SignalR-Optionen</span><span class="sxs-lookup"><span data-stu-id="f19fc-166">Configuring SignalR Options</span></span>

<span data-ttu-id="f19fc-167">Der Überladungen der `MapHubs` Methode ermöglichen es Ihnen, eine benutzerdefinierte URL, ein benutzerdefiniertes Abhängigkeitskonfliktlöser und die folgenden Optionen angeben:</span><span class="sxs-lookup"><span data-stu-id="f19fc-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="f19fc-168">Domänenübergreifende Aufrufe aus Browser-Clients zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="f19fc-169">In der Regel, wenn der Browser eine Seite aus lädt `http://contoso.com`, die SignalR-Verbindung ist auf der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="f19fc-170">Wenn die Seite `http://contoso.com` stellt eine Verbindung mit `http://fabrikam.com/signalr`, d. h. eine domänenübergreifende-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="f19fc-171">Domänenübergreifende Verbindungen werden aus Gründen der Sicherheit standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="f19fc-172">Weitere Informationen finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - JavaScript-Client - Gewusst wie: Herstellen einer Verbindung domänenübergreifende](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="f19fc-173">Detaillierte Fehlermeldungen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="f19fc-174">Wenn Fehler auftreten, ist das Standardverhalten des SignalR an Clients senden eine Benachrichtigung ohne Details, was passiert ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="f19fc-175">Ausführliche Fehlerinformationen an Clients gesendet wird in der Produktion nicht empfohlen, da böswillige Benutzer die Informationen in Angriffe auf Ihre Anwendung verwenden werden können.</span><span class="sxs-lookup"><span data-stu-id="f19fc-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="f19fc-176">Diese Option können Sie für die Problembehandlung um vorübergehend ausführlichere-Fehlerberichterstattung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="f19fc-177">Deaktivieren Sie die automatisch generierte Dateien der JavaScript-Proxy.</span><span class="sxs-lookup"><span data-stu-id="f19fc-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="f19fc-178">Standardmäßig wird eine JavaScript-Datei mit Proxys für den Hub Klassen als Antwort auf die URL "/ Signalr/Hubs" generiert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="f19fc-179">Wenn Sie nicht die JavaScript-Proxys verwenden möchten oder wenn Sie diese Datei manuell zu generieren und zu einer physischen Datei in Ihre Clients verweisen möchten, können Sie diese Option, um Proxygenerierung zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="f19fc-180">Weitere Informationen finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - Gewusst wie: erstellen eine physische Datei für SignalR generierten Proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="f19fc-181">Im folgende Beispiel wird gezeigt, wie der SignalR-Verbindungs-URL und an diese Optionen in einem Aufruf der `MapHubs` Methode.</span><span class="sxs-lookup"><span data-stu-id="f19fc-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="f19fc-182">Um eine benutzerdefinierte URL anzugeben, ersetzen Sie "/ Signalr" im Beispiel durch die URL, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="f19fc-183">Das Erstellen und Verwenden der Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="f19fc-183">How to create and use Hub classes</span></span>

<span data-ttu-id="f19fc-184">Um einen Hub zu erstellen, erstellen Sie eine Klasse, die abgeleitet [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f19fc-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="f19fc-185">Das folgende Beispiel zeigt eine einfache hubklasse für eine Chat-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="f19fc-186">In diesem Beispiel wird ein verbundener Client aufrufen kann die `NewContosoChatMessage` -Methode, und wenn dies der Fall ist, wird die empfangenen Daten für alle verbundenen Clients weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="f19fc-187">Lebensdauer eines Objekts "Hub"</span><span class="sxs-lookup"><span data-stu-id="f19fc-187">Hub object lifetime</span></span>

<span data-ttu-id="f19fc-188">Sie nicht die Hub-Klasse instanziieren oder aus Ihrem eigenen Code auf dem Server ihre Methoden aufrufen; Alle, die Sie von der Pipeline SignalR-Hubs erfolgt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="f19fc-189">SignalR erstellt eine neue Instanz der Klasse Hub jedes Mal, es muss sich um eine Hub-Vorgang, z. B. wenn ein Client eine Verbindung herstellt, trennt die Verbindung oder bewirkt, dass eine Methode aufrufen, mit dem Server zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="f19fc-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="f19fc-190">Da die Instanzen der Klasse Hub vorübergehend sind, können nicht Sie sie zur Beibehaltung des Zustands in einem Methodenaufruf an den nächsten verwenden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="f19fc-191">Jedes Mal erhält der Server einem Methodenaufruf von einem Client, eine neue Instanz Ihrer Klasse-Prozesse Hub die Nachricht.</span><span class="sxs-lookup"><span data-stu-id="f19fc-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="f19fc-192">Um über mehrere Verbindungen und Methodenaufrufe Zustand beibehalten, verwenden Sie eine andere Methode, z. B. eine Datenbank oder eine statische Variable für den Hub-Klasse oder einer anderen Klasse, die nicht von abgeleitet ist `Hub`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="f19fc-193">Wenn Sie Daten im Arbeitsspeicher beibehalten, mithilfe einer Methode wie z. B. eine statische Variable in der Hub-Klasse, die Daten verloren, wenn die Anwendungsdomäne wiederverwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="f19fc-194">Wenn Sie möchten Clients aus Ihrem eigenen Code Senden von Nachrichten an, die außerhalb der Hub-Klasse ausgeführt wird, dies nicht möglich, durch die Instanziierung einer Hub-Klasseninstanz, jedoch können Sie dies tun, durch einen Verweis auf das Kontextobjekt SignalR für die Klasse Hub abrufen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="f19fc-195">Weitere Informationen finden Sie unter [wie Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Klasse Hub](#callfromoutsidehub) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="f19fc-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="f19fc-196">In Kamel-Schreibweise des Hub-Namen in der JavaScript-clients</span><span class="sxs-lookup"><span data-stu-id="f19fc-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="f19fc-197">Standardmäßig finden Sie JavaScript-Clients mit einer Version in Kamel-Schreibweise des Klassennamens für Hubs.</span><span class="sxs-lookup"><span data-stu-id="f19fc-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="f19fc-198">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="f19fc-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="f19fc-199">Im vorherige Beispiel würde, die als bezeichnet `contosoChatHub` im JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="f19fc-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="f19fc-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="f19fc-201">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="f19fc-202">Wenn Sie für Clients verwenden, fügen Sie einen anderen Namen geben möchten die `HubName` Attribut.</span><span class="sxs-lookup"><span data-stu-id="f19fc-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="f19fc-203">Bei Verwendung einer `HubName` -Attribut angegeben wird, erfolgt keine Namensänderung in Camel-Case für JavaScript-Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="f19fc-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f19fc-205">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="f19fc-206">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="f19fc-206">Multiple Hubs</span></span>

<span data-ttu-id="f19fc-207">Sie können mehrere Hub-Klassen in einer Anwendung definieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="f19fc-208">Wenn Sie dies tun, die Verbindung freigegeben, jedoch Gruppen getrennt sind:</span><span class="sxs-lookup"><span data-stu-id="f19fc-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="f19fc-209">Alle Clients verwenden die gleiche URL zum Herstellen einer SignalR-Verbindung mit Ihrem Dienst ("/ Signalr" oder die benutzerdefinierte URL, wenn Sie eine angegeben), und dass für sämtliche Hubs im aktuellen Verbindung verwendet wird, die vom Dienst definierten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="f19fc-210">Es gibt keine Leistungsunterschiede für mehrere Hubs im Vergleich zu allen Hub-Funktionalität in einer einzelnen Klasse definieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="f19fc-211">Sämtliche Hubs im aktuellen erhalten die gleiche Informationen der HTTP-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="f19fc-212">Da sämtliche Hubs im aktuellen auf die gleiche Verbindung verwenden, ist nur HTTP-Anforderungsinformationen, die der Server ruft an, was in der ursprünglichen HTTP-Anforderung stammt, die die SignalR-Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="f19fc-213">Wenn Sie die verbindungsanforderung verwenden, um Informationen vom Client an den Server zu übergeben, indem Sie eine Abfragezeichenfolge angeben, können nicht Sie verschiedene Abfragezeichenfolgen an andere Hubs bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="f19fc-214">Sämtliche Hubs im aktuellen erhalten die gleiche Informationen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="f19fc-215">Die generierte Datei des JavaScript-Proxys wird Proxys für sämtliche Hubs in einer Datei enthalten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="f19fc-216">Weitere Informationen zu JavaScript-Proxys, finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - generierte Proxy und wozu Sie](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="f19fc-217">In den Hubs sind Gruppen definiert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="f19fc-218">Mit dem Namen Gruppen aus, um Teilmengen der verbundenen Clients zu senden, in SignalR beziehen, die Sie definieren können.</span><span class="sxs-lookup"><span data-stu-id="f19fc-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="f19fc-219">Gruppen werden separat für jede Hub verwaltet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="f19fc-220">Beispielsweise würde eine Gruppe namens "Administratoren" enthalten einen Satz von Clients für Ihre `ContosoChatHub` -Klasse, und demselben Gruppennamen würde, beziehen sich auf einen anderen Satz von Clients für Ihre `StockTickerHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="f19fc-221">Zum Definieren von Methoden in der Hub-Klasse, die Clients aufrufen können</span><span class="sxs-lookup"><span data-stu-id="f19fc-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="f19fc-222">Um eine Methode auf dem Hub verfügbar machen, die vom Client aufgerufen werden soll, deklarieren Sie eine öffentliche Methode, wie in den folgenden Beispielen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="f19fc-223">Sie können angeben, ein Rückgabetyp und Parameter, einschließlich Arrays und komplexe Typen, wie in einer C#-Methode.</span><span class="sxs-lookup"><span data-stu-id="f19fc-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="f19fc-224">Alle Daten, die Sie in Parametern empfangen oder an den Aufrufer zurückgegeben werden zwischen dem Client und dem Server kommuniziert, mithilfe von JSON und SignalR behandelt automatisch die Bindung von komplexen Objekten und Arrays von Objekten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="f19fc-225">Binnenmajuskel Methodennamen in JavaScript-clients</span><span class="sxs-lookup"><span data-stu-id="f19fc-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="f19fc-226">Standardmäßig finden Sie JavaScript-Clients in hubmethoden mithilfe einer Camel-Case-Version des Methodennamens ein.</span><span class="sxs-lookup"><span data-stu-id="f19fc-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="f19fc-227">SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="f19fc-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="f19fc-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="f19fc-229">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="f19fc-230">Wenn Sie für Clients verwenden, fügen Sie einen anderen Namen geben möchten die `HubMethodName` Attribut.</span><span class="sxs-lookup"><span data-stu-id="f19fc-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="f19fc-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="f19fc-232">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="f19fc-233">Beim asynchron ausführen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-233">When to execute asynchronously</span></span>

<span data-ttu-id="f19fc-234">Wenn die Methode wird werden lang andauernde oder muss funktionieren würde, die warten, z. B. einer Datenbanksuche oder einem Webdienstaufruf umfassen, stellen Sie die Hub-Methode asynchrone, wird durch Zurückgeben einer [Aufgabe](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (anstelle von `void` zurückgeben) oder [ Aufgabe&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) Objekt (anstelle von `T` Rückgabetyp).</span><span class="sxs-lookup"><span data-stu-id="f19fc-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/en-us/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="f19fc-235">Wenn Sie zurückkehren, eine `Task` Objekt aus der SignalR-Methode wartet der `Task` abgeschlossen ist, und sendet es die entpackte Ergebnis zurück an den Client, damit kein Unterschied besteht in wie der Aufruf der Methode im Client code.</span><span class="sxs-lookup"><span data-stu-id="f19fc-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="f19fc-236">Erstellen einer Hub-Methode wird vermieden, asynchrone die Verbindung blockiert, wenn die WebSocket-Transport verwendet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="f19fc-237">Wenn eine hubmethode synchron wird und WebSocket als Transport verwendet wird, werden nachfolgende Aufrufe von Methoden auf dem Hub vom gleichen Client blockiert, bis zum Abschluss der Hub-Methode.</span><span class="sxs-lookup"><span data-stu-id="f19fc-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="f19fc-238">Das folgende Beispiel zeigt die gleiche Methode codiert, um eine synchrone Ausführung oder asynchron ausgeführt wird, gefolgt von JavaScript-Clientcode, der zum Aufrufen von entweder Version funktioniert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="f19fc-239">**Synchrone**</span><span class="sxs-lookup"><span data-stu-id="f19fc-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="f19fc-240">**Asynchrone - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="f19fc-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="f19fc-241">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="f19fc-242">Weitere Informationen zur Verwendung von asynchronen Methoden in ASP.NET 4.5 finden Sie unter [Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="f19fc-243">Definieren von Überladungen</span><span class="sxs-lookup"><span data-stu-id="f19fc-243">Defining Overloads</span></span>

<span data-ttu-id="f19fc-244">Wenn Sie die Überladungen für eine Methode definieren möchten, muss die Anzahl von Parametern in jede Überladung sich unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="f19fc-245">Wenn Sie eine Überladung unterscheiden, indem Sie verschiedene Parametertypen angeben, die Hub-Klasse kompiliert, aber der SignalR-Dienst löst eine Ausnahme zur Laufzeit, wenn Clients versuchen, auf eine der Überladungen aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="f19fc-246">Gewusst wie: Client Methoden aufrufen, von der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="f19fc-247">Um Client vom Server Methoden aufzurufen, verwenden Sie die `Clients` Eigenschaft in einer Methode in der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="f19fc-248">Das folgende Beispiel zeigt die Servercode, der Aufrufe `addNewMessageToPage` auf allen verbundenen Clients, und Clientcode, der die Methode in einem JavaScript-Client definiert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="f19fc-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="f19fc-250">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="f19fc-251">Einen Rückgabewert kann nicht von einer Clientmethode abgerufen werden; Diese Syntax sind `int x = Clients.All.add(1,1)` funktioniert nicht.</span><span class="sxs-lookup"><span data-stu-id="f19fc-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="f19fc-252">Sie können komplexe Typen und -Arrays für die Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="f19fc-253">Im folgende Beispiel wird einen komplexen Typ an dem Client in einem Methodenparameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="f19fc-254">**Servercode, der eine Clientmethode, die mithilfe eines komplexen Objekts aufruft**</span><span class="sxs-lookup"><span data-stu-id="f19fc-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="f19fc-255">**Servercode, die das komplexe Objekt definiert.**</span><span class="sxs-lookup"><span data-stu-id="f19fc-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="f19fc-256">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="f19fc-257">Auswählen von welchen Clients erhalten die RPC</span><span class="sxs-lookup"><span data-stu-id="f19fc-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="f19fc-258">Gibt die Clients-Eigenschaft einer [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) -Objekt, das bietet mehrere Optionen zum angeben, welche Clients die RPC empfangen werden:</span><span class="sxs-lookup"><span data-stu-id="f19fc-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="f19fc-259">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="f19fc-260">Nur der aufrufende Client.</span><span class="sxs-lookup"><span data-stu-id="f19fc-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="f19fc-261">Alle Clients mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="f19fc-262">Um einen spezifischen Client identifizierte Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="f19fc-263">Dieses Beispiel ruft `addContosoChatMessageToPage` auf dem aufrufenden Client und hat dieselbe Wirkung wie das Verwenden von `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="f19fc-264">Alle verbundenen Clients mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="f19fc-265">Alle verbundenen Clients in einer angegebenen Gruppe.</span><span class="sxs-lookup"><span data-stu-id="f19fc-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="f19fc-266">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="f19fc-267">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="f19fc-268">Keine Validierung während der Kompilierung für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="f19fc-268">No compile-time validation for method names</span></span>

<span data-ttu-id="f19fc-269">Der Methodenname, die Sie angeben, wird als ein dynamisches Objekt interpretiert dies bedeutet, dass es gibt keine IntelliSense oder Überprüfung der Kompilierzeit dafür.</span><span class="sxs-lookup"><span data-stu-id="f19fc-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="f19fc-270">Der Ausdruck wird zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="f19fc-271">Wenn der Methodenaufruf ausgeführt wird, werden SignalR den Methodennamen und die Parameterwerte an den Client gesendet, und wenn der Client eine Methode verfügt, die mit dem Namen übereinstimmt, die-Methode aufgerufen wird und die Parameterwerte zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="f19fc-272">Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="f19fc-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="f19fc-273">Informationen zum Format der Daten, die SignalR überträgt an den Client im Hintergrund auf, wenn Sie eine Clientmethode aufrufen, finden Sie unter [Einführung in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="f19fc-274">Namensübereinstimmung Groß-/Kleinschreibung-Methode</span><span class="sxs-lookup"><span data-stu-id="f19fc-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="f19fc-275">Methode-namenszuordnung wird Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f19fc-276">Beispielsweise `Clients.All.addContosoChatMessageToPage` auf dem Server führt `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, oder `addContosoChatMessageToPage` auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="f19fc-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="f19fc-277">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="f19fc-277">Asynchronous execution</span></span>

<span data-ttu-id="f19fc-278">Die Methode, die Sie aufrufen führt asynchron aus.</span><span class="sxs-lookup"><span data-stu-id="f19fc-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="f19fc-279">Jeglicher Code, der nach dem Aufruf einer Methode an einen Client sofort ausgeführt wird, ohne zu warten, für SignalR Daten an Clients übertragen, es sei denn, die Sie angeben, dass die nachfolgenden Zeilen des Codes, auf den Abschluss der Methode warten soll abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="f19fc-280">Die folgenden Codebeispiele zeigen, wie zwei Clientmethoden sequenziell ausführen, eine mit Funktionsfähigkeit in .NET 4.5 code und eine mit code Funktionsfähigkeit in .NET 4.</span><span class="sxs-lookup"><span data-stu-id="f19fc-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="f19fc-281">**.NET 4.5-Beispiel**</span><span class="sxs-lookup"><span data-stu-id="f19fc-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="f19fc-282">**.NET 4-Beispiel**</span><span class="sxs-lookup"><span data-stu-id="f19fc-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="f19fc-283">Bei Verwendung von `await` oder `ContinueWith` warten, bis eine Clientmethode abgeschlossen ist, bevor die nächste Zeile des Codes ausgeführt wird, dies bedeutet nicht, dass Clients die Nachricht tatsächlich empfangen werden, bevor die nächste Zeile des Codes ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="f19fc-284">"Abschluss", der einen clientmethodenaufruf bedeutet lediglich, dass SignalR alles, was Sie zum Senden der Nachricht durchgeführt hat.</span><span class="sxs-lookup"><span data-stu-id="f19fc-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="f19fc-285">Wenn Sie Überprüfung, dass Clients die Nachricht empfangen möchten, müssen Sie diesen Mechanismus selbst programmieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="f19fc-286">Sie können z. B. code eine `MessageReceived` -Methode auf dem Hub, und klicken Sie in der `addContosoChatMessageToPage` Methode auf dem Client, Sie rufen `MessageReceived` danach nach Belieben Sie arbeiten, müssen auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="f19fc-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="f19fc-287">In `MessageReceived` im Hub erreichen Sie tatsächliche Client-Empfang und die Verarbeitung des ursprünglichen Methodenaufrufs Arbeit abhängt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="f19fc-288">Verwenden Sie eine Zeichenfolgenvariable als Name der Methode</span><span class="sxs-lookup"><span data-stu-id="f19fc-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="f19fc-289">Wenn Sie eine Clientmethode aufrufen, indem Sie eine Zeichenfolgenvariable verwenden, als der Methodenname, wandeln Sie möchten `Clients.All` (oder `Clients.Others`, `Clients.Caller`usw.) zu `IClientProxy` und rufen Sie anschließend [Invoke (MethodName, Args...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f19fc-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="f19fc-290">Zum Verwalten von Gruppenmitgliedschaften aus der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="f19fc-291">Gruppen in SignalR bieten eine Methode zum Übertragen von Nachrichten an den angegebenen Teilmengen von verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="f19fc-292">Eine Gruppe kann eine beliebige Anzahl von Clients umfassen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="f19fc-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="f19fc-293">Verwenden Sie zum Verwalten der Gruppenmitgliedschaft die [hinzufügen](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) und [entfernen](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) bereitgestellten Methoden die `Groups` Eigenschaft der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-293">To manage group membership, use the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="f19fc-294">Das folgende Beispiel zeigt die `Groups.Add` und `Groups.Remove` Methoden, die in hubmethoden, die vom Client-Code aufgerufen werden, gefolgt von JavaScript-Clientcode, der sie aufruft.</span><span class="sxs-lookup"><span data-stu-id="f19fc-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="f19fc-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="f19fc-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="f19fc-296">**JavaScript-Client generierte Proxy zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="f19fc-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="f19fc-297">Sie müssen nicht explizit Gruppen erstellen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="f19fc-298">Faktisch ist eine Gruppe automatisch beim ersten Geben Sie den Namen in einem Aufruf erstellt `Groups.Add`, und es wird gelöscht, wenn Sie aus der Mitgliedschaft in der sie in der letzten Verbindung entfernen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="f19fc-299">Es ist keine API zum Abrufen der Mitgliederliste einer Gruppe oder eine Liste der Gruppen ein.</span><span class="sxs-lookup"><span data-stu-id="f19fc-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="f19fc-300">SignalR sendet Nachrichten an Clients und-Gruppen auf Grundlage einer [und Abonnementmodell](http://en.wikipedia.org/wiki/Publish/subscribe), und der Server behält keine Listen von Gruppen und Gruppenmitgliedschaften.</span><span class="sxs-lookup"><span data-stu-id="f19fc-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="f19fc-301">Dadurch Maximieren der Skalierbarkeit, da Wenn Sie einen Knoten zu einer Webfarm hinzufügen, muss einem beliebigen Zustand, in der SignalR verwaltet an den neuen Knoten weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="f19fc-302">Asynchrone Ausführung der Add- und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="f19fc-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="f19fc-303">Die `Groups.Add` und `Groups.Remove` Methoden asynchron auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="f19fc-304">Wenn Sie einen Client zu einer Gruppe hinzufügen und eine Nachricht sofort an den Client senden, mit dem Group möchten, müssen Sie sicherstellen, dass die `Groups.Add` Methode zuerst beendet ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="f19fc-305">Die folgenden Codebeispiele zeigen, wie diese mithilfe von Code, der in .NET 4.5 und mithilfe von Code, der in .NET 4 funktioniert funktioniert</span><span class="sxs-lookup"><span data-stu-id="f19fc-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="f19fc-306">**.NET 4.5-Beispiel**</span><span class="sxs-lookup"><span data-stu-id="f19fc-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="f19fc-307">**.NET 4-Beispiel**</span><span class="sxs-lookup"><span data-stu-id="f19fc-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="f19fc-308">Gruppenmitgliedschaft Persistenz</span><span class="sxs-lookup"><span data-stu-id="f19fc-308">Group membership persistence</span></span>

<span data-ttu-id="f19fc-309">SignalR verfolgt Verbindungen, nicht von Benutzern, wenn also einen Benutzer in der gleichen Gruppe werden jedes Mal, wenn der Benutzer richtet eine Verbindung soll, müssen Sie anrufen `Groups.Add` jedes Mal, wenn der Benutzer eine neue Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="f19fc-310">Nach einem vorübergehenden Dienstausfall Konnektivität kann manchmal SignalR die Verbindung wiederherzustellen automatisch.</span><span class="sxs-lookup"><span data-stu-id="f19fc-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="f19fc-311">In diesem Fall SignalR ist die gleiche Verbindung wiederherstellen nicht Herstellen einer neuen Verbindung und Gruppenmitgliedschaft für den Client wird daher automatisch wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="f19fc-312">Dies wird dadurch auch, wenn die temporären Unterbrechung einen Neustart des Servers oder einen Fehler, das Ergebnis ist Verbindungsstatus für jeden Client, Gruppenmitgliedschaften, einschließlich Roundtrip an den Client ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="f19fc-313">Wenn ein Server ausfällt und durch einen neuen Server ersetzt wird, bevor die Verbindung ein Timeout eintritt, kann ein Client automatische Wiederherstellen der Verbindung mit dem neuen Server und in Ihnen Mitglied der ist Gruppen erneut registrieren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="f19fc-314">Wenn eine Verbindung nicht automatisch nach einem Verlust der Verbindung, wiederhergestellt werden kann oder wenn die Verbindung ein Timeout auftritt oder wenn der Client die Verbindung trennt (z. B. bei ein Browser zu einer neuen Seite navigiert) sind Gruppenmitgliedschaften verloren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="f19fc-315">Das nächste Mal die Benutzer eine Verbindung herstellt, wird eine neue Verbindung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="f19fc-316">Um die Gruppenmitgliedschaften beizubehalten, wenn derselbe Benutzer eine neue Verbindung hergestellt wird, muss die Anwendung so verfolgen die Zuordnungen zwischen Benutzern und Gruppen und Gruppenmitgliedschaften wiederherstellen, jedes Mal ein Benutzer eine neue Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="f19fc-317">Weitere Informationen zu Verbindungen und erneute Verbindungen finden Sie unter [wie Lebensdauer Verbindungsereignisse in der Hub-Klasse behandelt](#connectionlifetime) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="f19fc-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="f19fc-318">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="f19fc-318">Single-user groups</span></span>

<span data-ttu-id="f19fc-319">Anwendungen, die in der Regel verwenden Sie SignalR haben zum Nachverfolgen von die Zuordnungen zwischen Benutzern und Verbindungen, damit Sie wissen, welche Benutzer eine Nachricht gesendet hat und welche Benutzer eine Nachricht empfangen werden soll.</span><span class="sxs-lookup"><span data-stu-id="f19fc-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="f19fc-320">Gruppen werden in einem der zwei häufig verwendete Muster für auf diese Weise verwendet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="f19fc-321">Einzelbenutzer-Gruppen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-321">Single-user groups.</span></span>

    <span data-ttu-id="f19fc-322">Sie können Geben Sie den Benutzernamen als der Gruppenname und die aktuelle Verbindungs-ID der Gruppe hinzufügen, jedes Mal, wenn der Benutzer eine Verbindung herstellt, oder die Verbindung wiederherstellt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="f19fc-323">Zum Senden von Nachrichten an den Benutzer, die Sie der Gruppe gesendet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="f19fc-324">Ein Nachteil dieser Methode ist, dass die Gruppe Sie eine Möglichkeit bietet, um festzustellen, ob der Benutzer online oder offline ist.</span><span class="sxs-lookup"><span data-stu-id="f19fc-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="f19fc-325">Nachverfolgen von Zuordnungen zwischen den Benutzernamen und Verbindungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="f19fc-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="f19fc-326">Sie können eine Zuordnung zwischen jeden Benutzernamen und ein oder mehrere Verbindungs-IDs in einem Wörterbuch oder einer Datenbank speichern und aktualisieren Sie die gespeicherten Daten jedes Mal, die der Benutzer eine Verbindung herstellt, oder die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="f19fc-327">Geben Sie zum Senden von Nachrichten an den Benutzer der Verbindungs-IDs an.</span><span class="sxs-lookup"><span data-stu-id="f19fc-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="f19fc-328">Ein Nachteil dieser Methode ist, dass mehr Arbeitsspeicher benötigt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="f19fc-329">Behandeln von Lebensdauer Verbindungsereignisse in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="f19fc-330">Gründe für die Behandlung Lebensdauer Verbindungsereignisse werden zum Nachverfolgen, ob ein Benutzer oder nicht verbunden ist, und klicken Sie zum Nachverfolgen der Zuordnung zwischen den Benutzernamen und Verbindungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="f19fc-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="f19fc-331">Um Ihren eigenen Code auszuführen, wenn Clients eine Verbindung herstellen oder trennen, überschreiben die `OnConnected`, `OnDisconnected`, und `OnReconnected` virtuelle Methoden des Hubs-Klasse, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="f19fc-332">Wenn OnConnected OnDisconnected und OnReconnected aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="f19fc-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="f19fc-333">Jedes Mal ein Browser zu einer neuen Seite navigiert eine neue Verbindung muss hergestellt werden, d. h. SignalR führt die `OnDisconnected` Methode gefolgt von der `OnConnected` Methode.</span><span class="sxs-lookup"><span data-stu-id="f19fc-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="f19fc-334">SignalR erstellt eine neuen Verbindungs-ID immer, wenn eine neue Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="f19fc-335">Die `OnReconnected` Methode wird aufgerufen, wenn es eine temporäre Unterbrechung in Verbindung, die automatisch wurde von SignalR wiederherstellen können, z. B. wenn ein Kabel ist vorübergehend getrennt und wiederhergestellt werden, bevor die Verbindung ein Timeout eintritt. Die `OnDisconnected` Methode wird aufgerufen, wenn der Client getrennt ist und SignalR kann nicht automatisch erneut eine Verbindung herstellen, z. B. bei ein Browser zu einer neuen Seite navigiert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="f19fc-336">Eine mögliche Folge von Ereignissen für einen bestimmten Client also `OnConnected`, `OnReconnected`, `OnDisconnected`; oder `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="f19fc-337">Die Sequenz nicht angezeigt `OnConnected`, `OnDisconnected`, `OnReconnected` für eine bestimmte Verbindung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="f19fc-338">Die `OnDisconnected` Methode nicht in einigen Szenarien, z. B. beim Ausfall eines Servers aufgerufen wird, oder die Anwendungsdomäne ruft wiederverwendet.</span><span class="sxs-lookup"><span data-stu-id="f19fc-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="f19fc-339">Wenn einem anderen Server, auf die Zeile stammen oder der App-Domäne abgeschlossen, die Wiederverwendung ist, einige Clients möglicherweise erneut eine Verbindung herstellen und das Auslösen der `OnReconnected` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="f19fc-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="f19fc-340">Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="f19fc-341">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="f19fc-341">Caller state not populated</span></span>

<span data-ttu-id="f19fc-342">Die Verbindung für die Lebensdauer Ereignishandlermethoden heißen vom Server, d. einem beliebigen Zustand, die Sie aufnehmen h., in der `state` Objekt auf dem Client wird nicht aufgefüllt werden, der `Caller` Eigenschaft auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="f19fc-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="f19fc-343">Informationen zu den `state` Objekt und die `Caller` Eigenschaft finden Sie unter [zum Zustand zwischen Clients und der Hub-Klasse übergeben](#passstate) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="f19fc-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="f19fc-344">Gewusst wie: Abrufen von Informationen über den Client aus der Kontexteigenschaft</span><span class="sxs-lookup"><span data-stu-id="f19fc-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="f19fc-345">Verwenden Sie zum Abrufen von Informationen über den Client die `Context` Eigenschaft der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="f19fc-346">Die `Context` Eigenschaft gibt eine [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) Objekt, das Zugriff auf die folgenden Informationen:</span><span class="sxs-lookup"><span data-stu-id="f19fc-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="f19fc-347">Der Verbindungs-ID des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="f19fc-348">Die Verbindungs-ID ist eine GUID, die von SignalR zugewiesen wird (Sie können nicht den Wert in Ihrem eigenen Code angeben).</span><span class="sxs-lookup"><span data-stu-id="f19fc-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="f19fc-349">Es ist ein Verbindungs-ID für jede Verbindung, und die gleiche Verbindung, die ID von sämtliche Hubs im aktuellen verwendet wird, wenn Sie mehrere Hubs in Ihre Anwendung haben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="f19fc-350">HTTP-Header-Daten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="f19fc-351">Sie können auch HTTP-Header aus abrufen `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="f19fc-352">Der Grund für mehrere Verweise auf dasselbe ist, dass `Context.Headers` wurde zunächst erstellt der `Context.Request` Eigenschaft später hinzugefügt wurde und `Context.Headers` wurde für Abwärtskompatibilität beibehalten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="f19fc-353">Abfragen von Zeichenfolgendaten.</span><span class="sxs-lookup"><span data-stu-id="f19fc-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="f19fc-354">Sie können auch Zeichenfolgendaten aus Abfrage abrufen `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="f19fc-355">Die Abfragezeichenfolge, die Sie in dieser Eigenschaft abgerufen wird, die mit der HTTP-Anforderung verwendet wurde, die die SignalR-Verbindung hergestellt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="f19fc-356">Sie können Abfragezeichenfolgen-Parameter im Client hinzufügen, konfigurieren Sie die Verbindung, die wodurch eine einfache Möglichkeit, Daten über den Client vom Client an den Server übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="f19fc-357">Das folgende Beispiel zeigt eine Möglichkeit, eine Abfragezeichenfolge in einem JavaScript-Client hinzufügen, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="f19fc-358">Weitere Informationen zum Festlegen von Abfragezeichenfolgen-Parameter, finden Sie unter den API-Handbüchern für die [JavaScript](index.md) und [.NET](index.md) Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="f19fc-359">Sie finden die Transportmethode verwendet für die Verbindung in der Abfrage Zeichenfolgendaten, zusammen mit einigen anderen Werten, die intern vom SignalR verwendet:</span><span class="sxs-lookup"><span data-stu-id="f19fc-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="f19fc-360">Der Wert des `transportMethod` werden "WebSockets", "ServerSentEvents", "ForeverFrame" oder "LongPolling".</span><span class="sxs-lookup"><span data-stu-id="f19fc-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="f19fc-361">Beachten Sie, dass, wenn Sie diesen Wert, in Prüfen der `OnConnected` Ereignishandlermethode ist in einigen Szenarien möglicherweise zunächst einen Transportwert, der nicht der endgültigen ausgehandelte Transportmethode für die Verbindung ist abrufen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="f19fc-362">In diesem Fall wird die Methode löst eine Ausnahme aus, und wird später erneut aufgerufen werden, wenn die letzte Transportmethode hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f19fc-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="f19fc-363">Cookies.</span><span class="sxs-lookup"><span data-stu-id="f19fc-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="f19fc-364">Außerdem erhalten Sie, dass Cookies von `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="f19fc-365">Benutzerinformationen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="f19fc-366">Das HttpContext-Objekt für die Anforderung:</span><span class="sxs-lookup"><span data-stu-id="f19fc-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="f19fc-367">Verwenden Sie diese Methode anstelle `HttpContext.Current` zum Abrufen der `HttpContext` Objekt für die SignalR-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="f19fc-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="f19fc-368">Gewusst wie: Zustand zwischen Clients und der Hub-Klasse übergeben.</span><span class="sxs-lookup"><span data-stu-id="f19fc-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="f19fc-369">Der Clientproxy bietet eine `state` Objekt, in dem Sie Daten, die mit jedem Methodenaufruf an den Server übertragen werden sollen speichern können.</span><span class="sxs-lookup"><span data-stu-id="f19fc-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="f19fc-370">Auf dem Server können Sie zugreifen, diese Daten in der `Clients.Caller` Eigenschaft, die in hubmethoden, die von Clients aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="f19fc-371">Die `Clients.Caller` Eigenschaft wird nicht angegeben, für die Verbindung für die Lebensdauer Ereignishandlermethoden `OnConnected`, `OnDisconnected`, und `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="f19fc-372">Erstellen oder Aktualisieren von Daten in der `state` Objekt und die `Clients.Caller` Eigenschaft funktioniert in beide Richtungen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="f19fc-373">Sie können die Werte auf dem Server aktualisieren, und sie zurück an den Client übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="f19fc-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="f19fc-374">Das folgende Beispiel zeigt die JavaScript-Clientcode, mit dem Status für die Übertragung an den Server mit jedem Methodenaufruf speichert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="f19fc-375">Im folgende Beispiel wird der entsprechenden Code in einer .NET Client.</span><span class="sxs-lookup"><span data-stu-id="f19fc-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="f19fc-376">In der Hub-Klasse, erreichen Sie, diese Daten in der `Clients.Caller` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f19fc-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="f19fc-377">Im folgende Beispiel wird gezeigt, Code, der im vorherigen Beispiel genannten Zustand abruft.</span><span class="sxs-lookup"><span data-stu-id="f19fc-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="f19fc-378">Dieser Mechanismus für den persistenten Zustand dient nicht zur große Mengen von Daten, da alles, was Sie gelagerte der `state` oder `Clients.Caller` Eigenschaft ist mit jedem Methodenaufruf Roundtrip ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="f19fc-379">Es eignet sich für kleinere Elemente wie Benutzernamen oder Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="f19fc-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="f19fc-380">Gewusst wie: Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="f19fc-381">Verwenden Sie zum Behandeln von Fehlern, die in Ihrer Klasse hubmethoden auftreten, eine oder beide der folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="f19fc-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="f19fc-382">Umschließen Sie Ihrem Code im Try / Catch-Blocks, und melden Sie das Ausnahmeobjekt.</span><span class="sxs-lookup"><span data-stu-id="f19fc-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="f19fc-383">Zu Debugzwecken können Sie die Ausnahme an den Client senden, aber aus Sicherheitsgründen Gründe, senden detaillierte Informationen für Clients in der Produktion nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="f19fc-384">Erstellen Sie ein Hubs Pipeline-Modul, das verarbeitet die [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) Methode.</span><span class="sxs-lookup"><span data-stu-id="f19fc-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="f19fc-385">Das folgende Beispiel zeigt eine Pipeline-Modul, das protokolliert Fehler, gefolgt vom Code in "Global.asax", in den das Modul in die Pipeline Hubs injiziert.</span><span class="sxs-lookup"><span data-stu-id="f19fc-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="f19fc-386">Weitere Informationen zu Hub Pipeline Modulen finden Sie unter [zum Anpassen der Pipeline Hubs](#hubpipeline) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="f19fc-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="f19fc-387">Gewusst wie: Aktivieren der Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="f19fc-387">How to enable tracing</span></span>

<span data-ttu-id="f19fc-388">Fügen Sie zum Aktivieren der serverseitigen Ablaufverfolgung system.diagnostics-Element der Datei "Web.config" hinzu, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f19fc-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="f19fc-389">Wenn Sie die Anwendung in Visual Studio ausführen, sehen Sie die Protokolle in der **Ausgabe** Fenster.</span><span class="sxs-lookup"><span data-stu-id="f19fc-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="f19fc-390">Das Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="f19fc-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="f19fc-391">Um den Client von einer anderen Klasse als der Hub-Klasse Methoden aufrufen, rufen Sie einen Verweis auf das Kontextobjekt SignalR für den Hub und verwenden Sie, die zum Aufrufen von Methoden auf dem Client oder Verwalten von Gruppen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="f19fc-392">Im folgenden Beispiel `StockTicker` Klasse das Context-Objekt abruft, speichert es in einer Instanz der Klasse, speichert die Klasseninstanz in eine statische Eigenschaft und verwendet Sie den Kontext aus der Singleton-Klasse-Instanz zum Aufrufen der `updateStockPrice` Methode auf, die Clients verbunden mit einem Hub, mit dem Namen `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="f19fc-393">Wenn Sie den Kontext mehrere-Male in einem langlebige Objekt verwenden möchten, rufen Sie den Verweis einmal, und speichern Sie, anstatt eine Vorgang jedes Mal des Projekts.</span><span class="sxs-lookup"><span data-stu-id="f19fc-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="f19fc-394">Einmal Abrufen des Kontexts wird sichergestellt, dass SignalR für Clients in derselben Reihenfolge Senden von Nachrichten in der Ihre hubmethoden Client Methodenaufrufe vornehmen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="f19fc-395">Ein Lernprogramm, das veranschaulicht, wie die SignalR-Kontext für einen Hub verwenden, finden Sie unter [Broadcast-Server mit ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="f19fc-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="f19fc-396">Aufrufen von Clientmethoden</span><span class="sxs-lookup"><span data-stu-id="f19fc-396">Calling client methods</span></span>

<span data-ttu-id="f19fc-397">Können Sie angeben, welche Clients die RPC empfangen werden, aber Sie haben weniger Optionen als beim Aufrufen von einer hubklasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="f19fc-398">Der Grund dafür ist, dass der Kontext keinen bestimmten Aufruf von einem Client zugeordnet ist, damit keine Methoden, die die aktuelle Verbindungs-ID, wie z. B. bekannt sein `Clients.Others`, oder `Clients.Caller`, oder `Clients.OthersInGroup`, sind nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f19fc-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="f19fc-399">Die folgenden Optionen sind verfügbar:</span><span class="sxs-lookup"><span data-stu-id="f19fc-399">The following options are available:</span></span>

- <span data-ttu-id="f19fc-400">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="f19fc-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="f19fc-401">Um einen spezifischen Client identifizierte Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="f19fc-402">Alle verbundenen Clients mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="f19fc-403">Alle verbundenen Clients in einer angegebenen Gruppe.</span><span class="sxs-lookup"><span data-stu-id="f19fc-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="f19fc-404">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="f19fc-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="f19fc-405">Wenn Sie Ihre hubklasse von Methoden in der Hub-Klasse aufgerufen werden, können Sie in der aktuellen Verbindungs­id übergeben und verwenden, die mit `Clients.Client`, `Clients.AllExcept`, oder `Clients.Group` simulieren `Clients.Caller`, `Clients.Others`, oder `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="f19fc-406">Im folgenden Beispiel die `MoveShapeHub` Klasse übergibt die Verbindungs-ID, die `Broadcaster` Klasse, damit die `Broadcaster` Klasse kann simulieren `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="f19fc-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="f19fc-407">Verwalten von Gruppenmitgliedschaften</span><span class="sxs-lookup"><span data-stu-id="f19fc-407">Managing group membership</span></span>

<span data-ttu-id="f19fc-408">Verwalten von Gruppen müssen Sie die gleichen Optionen wie in einer hubklasse.</span><span class="sxs-lookup"><span data-stu-id="f19fc-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="f19fc-409">Einen Client zu einer Gruppe hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f19fc-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="f19fc-410">Entfernen Sie einen Client aus einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="f19fc-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="f19fc-411">Gewusst wie: Anpassen die Pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="f19fc-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="f19fc-412">SignalR ermöglicht es Ihnen, Ihren eigenen Code in der Hubpipeline einzufügen.</span><span class="sxs-lookup"><span data-stu-id="f19fc-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="f19fc-413">Das folgende Beispiel zeigt ein benutzerdefiniertes Hub-Pipeline-Modul, das von jedem eingehenden Methodenaufruf empfangen, die vom Client und vom ausgehenden Aufruf der Methode, die aufgerufen wird, auf dem Client protokolliert:</span><span class="sxs-lookup"><span data-stu-id="f19fc-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="f19fc-414">Der folgende code in der *"Global.asax"* Datei registriert das Modul, in der Hub-Pipeline ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="f19fc-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="f19fc-415">Es gibt viele verschiedene Methoden, die Sie überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="f19fc-415">There are many different methods that you can override.</span></span> <span data-ttu-id="f19fc-416">Eine vollständige Liste finden Sie unter [HubPipelineModule Methoden](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f19fc-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span></span>
