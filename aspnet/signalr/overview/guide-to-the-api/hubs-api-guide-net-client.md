---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR-Hubs-API-Guide – .NET Client (c#) | Microsoft Docs
author: pfletcher
description: Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 in .NET Clients, z. B. Windows Store (WinRT), WPF, Silverlight und Nachteile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043937"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="917fb-103">ASP.NET SignalR-Hubs-API-Guide – .NET Client (c#)</span><span class="sxs-lookup"><span data-stu-id="917fb-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="917fb-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="917fb-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="917fb-105">Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 in .NET Clients, z. B. Windows Store (WinRT), WPF, Silverlight und konsolenanwendungen.</span><span class="sxs-lookup"><span data-stu-id="917fb-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="917fb-106">Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen.</span><span class="sxs-lookup"><span data-stu-id="917fb-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="917fb-107">Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="917fb-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="917fb-108">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="917fb-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="917fb-109">SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.</span><span class="sxs-lookup"><span data-stu-id="917fb-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="917fb-110">SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="917fb-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="917fb-111">Eine Einführung in die SignalR-Hubs und dauerhafte Verbindungen oder ein Lernprogramm, das zeigt, wie Sie eine vollständige SignalR-Anwendung erstellen, finden Sie in [SignalR - erste Schritte](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="917fb-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="917fb-112">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="917fb-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="917fb-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="917fb-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="917fb-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="917fb-114">.NET 4.5</span></span>
> - <span data-ttu-id="917fb-115">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="917fb-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="917fb-116">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="917fb-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="917fb-117">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="917fb-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="917fb-118">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="917fb-118">Questions and comments</span></span>
> 
> <span data-ttu-id="917fb-119">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="917fb-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="917fb-120">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="917fb-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="917fb-121">Übersicht</span><span class="sxs-lookup"><span data-stu-id="917fb-121">Overview</span></span>

<span data-ttu-id="917fb-122">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="917fb-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="917fb-123">Client-Setup</span><span class="sxs-lookup"><span data-stu-id="917fb-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="917fb-124">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="917fb-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="917fb-125">Domänenübergreifende Verbindungen von Silverlight-clients</span><span class="sxs-lookup"><span data-stu-id="917fb-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="917fb-126">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="917fb-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="917fb-127">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-clients</span><span class="sxs-lookup"><span data-stu-id="917fb-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="917fb-128">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="917fb-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="917fb-129">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="917fb-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="917fb-130">Zum Angeben der HTTP-Header</span><span class="sxs-lookup"><span data-stu-id="917fb-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="917fb-131">Gewusst wie: Geben Sie Clientzertifikate</span><span class="sxs-lookup"><span data-stu-id="917fb-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="917fb-132">Vorgehensweise: Erstellen Sie die hubproxy</span><span class="sxs-lookup"><span data-stu-id="917fb-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="917fb-133">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="917fb-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="917fb-134">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="917fb-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="917fb-135">Methoden mit Parametern, Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="917fb-136">Methoden mit Parametern, dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="917fb-137">Gewusst wie: einen Handler zu entfernen</span><span class="sxs-lookup"><span data-stu-id="917fb-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="917fb-138">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="917fb-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="917fb-139">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="917fb-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="917fb-140">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="917fb-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="917fb-141">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="917fb-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="917fb-142">WPF und Silverlight Konsolenanwendung Codebeispiele für Clientmethoden, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="917fb-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="917fb-143">Ein Beispiel .NET Client-Projekten finden Sie unter den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="917fb-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="917fb-144">[Gustavo Armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) auf "github.com" (WinRT, Silverlight, Konsole app-Beispiele).</span><span class="sxs-lookup"><span data-stu-id="917fb-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="917fb-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) auf "github.com" (WPF-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="917fb-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="917fb-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) auf "github.com" (Konsole-app-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="917fb-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="917fb-147">Dokumentation über die Programmierung des Servers oder JavaScript-Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="917fb-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="917fb-148">SignalR-Hubs-API-Leitfaden – Server</span><span class="sxs-lookup"><span data-stu-id="917fb-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="917fb-149">SignalR-Hubs-API-Handbuch - JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="917fb-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="917fb-150">Links zu API-Referenzthemen sind auf die .NET 4.5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="917fb-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="917fb-151">Wenn Sie .NET 4 verwenden, finden Sie unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="917fb-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="917fb-152">Client-setup</span><span class="sxs-lookup"><span data-stu-id="917fb-152">Client setup</span></span>

<span data-ttu-id="917fb-153">Installieren der [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet-Paket (nicht die [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) Paket).</span><span class="sxs-lookup"><span data-stu-id="917fb-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="917fb-154">Dieses Paket unterstützt WinRT, Silverlight, WPF, Konsolenanwendung und Windows Phone-Clients für .NET 4 und .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="917fb-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="917fb-155">Wenn die Version des SignalR, die Sie auf dem Client müssen von der Version, die Sie auf dem Server verfügen unterscheidet, kann SignalR häufig der Differenz angepasst werden kann.</span><span class="sxs-lookup"><span data-stu-id="917fb-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="917fb-156">Ein Server mit SignalR Version 2 unterstützt z. B. Clients, die 1.1.x installiert haben, sowie Clients, die Version 2 installiert haben.</span><span class="sxs-lookup"><span data-stu-id="917fb-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="917fb-157">Wenn der Unterschied zwischen der Version auf dem Server und die Version auf dem Client zu groß ist oder SignalR löst aus, wenn der Client neuer als der Server befindet, eine `InvalidOperationException` -Ausnahme aus, wenn der Client versucht, eine Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="917fb-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="917fb-158">Die Fehlermeldung lautet "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="917fb-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="917fb-159">Gewusst wie: Herstellen einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="917fb-159">How to establish a connection</span></span>

<span data-ttu-id="917fb-160">Bevor Sie eine Verbindung herstellen können, müssen Sie erstellen eine `HubConnection` Objekt, und erstellen Sie ein Proxykonto.</span><span class="sxs-lookup"><span data-stu-id="917fb-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="917fb-161">Rufen Sie zum Herstellen einer Verbindung, die `Start` Methode für die `HubConnection` Objekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="917fb-162">Bei JavaScript-Clients müssen Sie mindestens ein Ereignishandler vor dem Aufruf registriert die `Start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="917fb-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="917fb-163">Dies ist nicht für .NET-Clients erforderlich.</span><span class="sxs-lookup"><span data-stu-id="917fb-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="917fb-164">Für JavaScript-Clients, erstellt des generierte Proxycodes Proxys für sämtliche Hubs, die vorhanden sind automatisch auf dem Server und registrieren einen Ereignishandler ist wie die Hubs angeben möchte, dass der Client verwenden.</span><span class="sxs-lookup"><span data-stu-id="917fb-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="917fb-165">Aber für einen Client .NET Hub Proxys manuell erstellt, damit SignalR wird davon ausgegangen, dass Sie alle Hub verwendet werden, die Sie einen Proxy für erstellen.</span><span class="sxs-lookup"><span data-stu-id="917fb-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="917fb-166">Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst.</span><span class="sxs-lookup"><span data-stu-id="917fb-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="917fb-167">Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="917fb-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="917fb-168">Die `Start` Methode asynchron ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="917fb-169">Um sicherzustellen, dass nachfolgende Codezeilen nicht erst ausführen, nachdem die Verbindung hergestellt wird, verwenden Sie `await` in einer asynchronen Methode von ASP.NET 4.5 oder `.Wait()` in eine synchrone Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="917fb-170">Verwenden Sie keine `.Wait()` in einem WinRT-Client.</span><span class="sxs-lookup"><span data-stu-id="917fb-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="917fb-171">Domänenübergreifende Verbindungen von Silverlight-clients</span><span class="sxs-lookup"><span data-stu-id="917fb-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="917fb-172">Informationen zum Aktivieren der domänenübergreifende Verbindungen von Silverlight-Clients finden Sie unter [machen einen Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="917fb-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="917fb-173">Gewusst wie: Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="917fb-173">How to configure the connection</span></span>

<span data-ttu-id="917fb-174">Bevor Sie eine Verbindung herstellen, können Sie eine der folgenden Optionen angeben:</span><span class="sxs-lookup"><span data-stu-id="917fb-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="917fb-175">Gleichzeitige Verbindungen einschränken.</span><span class="sxs-lookup"><span data-stu-id="917fb-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="917fb-176">Abfragezeichenfolgenparameter.</span><span class="sxs-lookup"><span data-stu-id="917fb-176">Query string parameters.</span></span>
- <span data-ttu-id="917fb-177">Die Transportmethode.</span><span class="sxs-lookup"><span data-stu-id="917fb-177">The transport method.</span></span>
- <span data-ttu-id="917fb-178">HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="917fb-178">HTTP headers.</span></span>
- <span data-ttu-id="917fb-179">Clientzertifikate.</span><span class="sxs-lookup"><span data-stu-id="917fb-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="917fb-180">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-clients</span><span class="sxs-lookup"><span data-stu-id="917fb-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="917fb-181">In WPF-Clients müssen Sie möglicherweise die maximale Anzahl gleichzeitiger Verbindungen von seinem Standardwert 2 erhöhen.</span><span class="sxs-lookup"><span data-stu-id="917fb-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="917fb-182">Der empfohlene Wert ist 10.</span><span class="sxs-lookup"><span data-stu-id="917fb-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="917fb-183">Weitere Informationen finden Sie unter [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="917fb-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="917fb-184">Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter</span><span class="sxs-lookup"><span data-stu-id="917fb-184">How to specify query string parameters</span></span>

<span data-ttu-id="917fb-185">Wenn Sie möchten Daten an den Server senden, wenn der Client eine Verbindung herstellt, können Sie Abfragezeichenfolgen-Parameter an das Verbindungsobjekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="917fb-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="917fb-186">Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgen-Parameter im Clientcode festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="917fb-187">Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgenparameter in Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="917fb-188">Zum Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="917fb-188">How to specify the transport method</span></span>

<span data-ttu-id="917fb-189">Im Rahmen des Prozesses eine Verbindung herstellen verhandelt SignalR-Client normalerweise mit dem Server zu den bewährten Transport ermitteln kann, der unterstützt wird, indem sowohl Server-als auch.</span><span class="sxs-lookup"><span data-stu-id="917fb-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="917fb-190">Wenn Sie bereits wissen, dass der Transportmethode verwenden möchten, können Sie diese Aushandlung-Prozess umgehen.</span><span class="sxs-lookup"><span data-stu-id="917fb-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="917fb-191">Zum Angeben der Transportmethode übergeben Sie einen Transport-Objekt an die Start-Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="917fb-192">Das folgende Beispiel zeigt, wie die Transportmethode im Clientcode an.</span><span class="sxs-lookup"><span data-stu-id="917fb-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="917fb-193">Die [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) Namespace enthält die folgenden Klassen, die Sie verwenden können, um die Übertragung angegeben.</span><span class="sxs-lookup"><span data-stu-id="917fb-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="917fb-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="917fb-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="917fb-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="917fb-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="917fb-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (nur verfügbar, wenn sowohl Server-als auch .NET 4.5 verwenden.)</span><span class="sxs-lookup"><span data-stu-id="917fb-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="917fb-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (wählt automatisch den besten Transport, der vom Client und dem Server unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="917fb-198">Dies ist der Standardtransport.</span><span class="sxs-lookup"><span data-stu-id="917fb-198">This is the default transport.</span></span> <span data-ttu-id="917fb-199">Übergeben dies in der `Start` Methode hat denselben Effekt wie die Elemente nicht erfolgreich abgeschlossen.)</span><span class="sxs-lookup"><span data-stu-id="917fb-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="917fb-200">Der Transport ForeverFrame ist nicht in dieser Liste enthalten, da er nur von Browsern verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="917fb-201">Informationen zum Überprüfen der Transportmethode in Servercode finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - zum Abrufen von Informationen über den Client aus der Kontexteigenschaft](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="917fb-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="917fb-202">Weitere Informationen zu Transporten und Zugriffe, finden Sie unter [Einführung in die SignalR - Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="917fb-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="917fb-203">Zum Angeben der HTTP-Header</span><span class="sxs-lookup"><span data-stu-id="917fb-203">How to specify HTTP headers</span></span>

<span data-ttu-id="917fb-204">Verwenden Sie zum Festlegen von HTTP-Header der `Headers` Eigenschaft für das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="917fb-205">Im folgende Beispiel wird gezeigt, wie einen HTTP-Header hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="917fb-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="917fb-206">Gewusst wie: Geben Sie Clientzertifikate</span><span class="sxs-lookup"><span data-stu-id="917fb-206">How to specify client certificates</span></span>

<span data-ttu-id="917fb-207">Verwenden Sie zum Hinzufügen von Clientzertifikaten die `AddClientCertificate` Methode für das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="917fb-208">Vorgehensweise: Erstellen Sie die hubproxy</span><span class="sxs-lookup"><span data-stu-id="917fb-208">How to create the Hub proxy</span></span>

<span data-ttu-id="917fb-209">Um Methoden auf dem Client zu definieren, die ein Hub vom Server aufgerufen werden kann, und zum Aufrufen von Methoden für einen Hub auf dem Server, einen Proxy für den Hub erstellen, durch den Aufruf `CreateHubProxy` für das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="917fb-210">Die Zeichenfolge übergebenen `CreateHubProxy` ist der Name der Klasse Hub oder indem der angegebene Name der `HubName` Attribut, wenn eine auf dem Server verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="917fb-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="917fb-211">Die namenszuordnung wird Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="917fb-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="917fb-212">**Hubklasse auf server**</span><span class="sxs-lookup"><span data-stu-id="917fb-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="917fb-213">**Erstellen des Clientproxys für den Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="917fb-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="917fb-214">Wenn Sie ergänzen die Hub-Klasse mit einem `HubName` -Attribut angegeben wird, verwenden Sie diesen Namen.</span><span class="sxs-lookup"><span data-stu-id="917fb-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="917fb-215">**Hubklasse auf server**</span><span class="sxs-lookup"><span data-stu-id="917fb-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="917fb-216">**Erstellen des Clientproxys für den Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="917fb-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="917fb-217">Beim Aufrufen `HubConnection.CreateHubProxy` mehrmals mit dem gleichen `hubName`, erhalten Sie dieselbe zwischengespeichert `IHubProxy` Objekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="917fb-218">Zum Definieren von Methoden auf dem Client, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="917fb-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="917fb-219">Um eine Methode definieren, die der Server aufrufen können, verwenden Sie des Proxys `On` Methode, um einen Ereignishandler zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="917fb-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="917fb-220">Methode-namenszuordnung wird Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="917fb-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="917fb-221">Beispielsweise `Clients.All.UpdateStockPrice` auf dem Server führt `updateStockPrice`, `updatestockprice`, oder `UpdateStockPrice` auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="917fb-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="917fb-222">Andere Client-Plattformen haben unterschiedliche Anforderungen an wie Sie den Code schreiben, die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="917fb-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="917fb-223">Die dargestellten Beispiele gelten für WinRT (.NET für Windows Store)-Clients.</span><span class="sxs-lookup"><span data-stu-id="917fb-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="917fb-224">WPF und Silverlight sowie Konsole Anwendungsbeispiele dienen [einem separaten Abschnitt weiter unten in diesem Thema](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="917fb-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="917fb-225">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="917fb-225">Methods without parameters</span></span>

<span data-ttu-id="917fb-226">Wenn die Methode, die Sie behandeln können nicht über Parameter verfügt, verwenden Sie die nicht generische Überladung von der `On` Methode:</span><span class="sxs-lookup"><span data-stu-id="917fb-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="917fb-227">**Servercode Clientmethode ohne Parameter aufrufen**</span><span class="sxs-lookup"><span data-stu-id="917fb-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="917fb-228">**WinRT-Clientcode für die Methode, die vom Server ohne Parameter aufgerufen ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="917fb-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="917fb-229">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="917fb-230">Wenn die Methode, die Sie behandeln können Parameter verfügt, geben Sie die Typen der Parameter als generischen Typen von der `On` Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="917fb-231">Es gibt generische Überladung von der `On` Methode zum Aktivieren von bis zu 8 Parameter (4 unter Windows Phone 7) angeben.</span><span class="sxs-lookup"><span data-stu-id="917fb-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="917fb-232">Im folgenden Beispiel einen Parameter gesendet wird, zu der `UpdateStockPrice` Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="917fb-233">**Servercode-Methode mit einem Parameter aufrufen**</span><span class="sxs-lookup"><span data-stu-id="917fb-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="917fb-234">**Der Stock-Klasse, die für den Parameter verwendet**</span><span class="sxs-lookup"><span data-stu-id="917fb-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="917fb-235">**WinRT-Clientcode für eine Methode aufgerufen wird, vom Server mit einem Parameter ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="917fb-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="917fb-236">Methoden mit Parametern, dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="917fb-237">Als Alternative zum Angeben von Parametern als generischen Typen von der `On` -Methode, können Sie Parameter als dynamische Objekte angeben:</span><span class="sxs-lookup"><span data-stu-id="917fb-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="917fb-238">**Servercode-Methode mit einem Parameter aufrufen**</span><span class="sxs-lookup"><span data-stu-id="917fb-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="917fb-239">**Der Stock-Klasse, die für den Parameter verwendet**</span><span class="sxs-lookup"><span data-stu-id="917fb-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="917fb-240">**WinRT-Clientcode für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den Parameter ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="917fb-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="917fb-241">Gewusst wie: einen Handler zu entfernen</span><span class="sxs-lookup"><span data-stu-id="917fb-241">How to remove a handler</span></span>

<span data-ttu-id="917fb-242">Um einen Handler zu entfernen, rufen Sie die `Dispose` Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="917fb-243">**Clientcode für einen vom Server aufgerufene Methode**</span><span class="sxs-lookup"><span data-stu-id="917fb-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="917fb-244">**Client-Code, um den Ereignishandler zu entfernen**</span><span class="sxs-lookup"><span data-stu-id="917fb-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="917fb-245">Gewusst wie: Aufrufen von Servermethoden vom client</span><span class="sxs-lookup"><span data-stu-id="917fb-245">How to call server methods from the client</span></span>

<span data-ttu-id="917fb-246">Verwenden Sie zum Aufrufen einer Methode auf dem Server die `Invoke` Methode für die hubproxy.</span><span class="sxs-lookup"><span data-stu-id="917fb-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="917fb-247">Verfügt die Server-Methode keinen Wert zurückgibt, verwenden Sie die nicht generische Überladung von der `Invoke` Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="917fb-248">**Servercode für eine Methode, die keinen zurückgibt Wert**</span><span class="sxs-lookup"><span data-stu-id="917fb-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="917fb-249">**Clientcode Aufrufen einer Methode, die keinen zurückgibt Wert**</span><span class="sxs-lookup"><span data-stu-id="917fb-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="917fb-250">Wenn die Server-Methode einen Rückgabewert verfügt, den Rückgabetyp angeben, wie der generische Typ, der die `Invoke` Methode.</span><span class="sxs-lookup"><span data-stu-id="917fb-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="917fb-251">**Servercode für eine Methode, die einen Wert zurückgibt und nimmt den Parameter ein komplexer Typ**</span><span class="sxs-lookup"><span data-stu-id="917fb-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="917fb-252">**Der Stock-Klasse, die für den Parameter und Rückgabewert verwendet**</span><span class="sxs-lookup"><span data-stu-id="917fb-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="917fb-253">**Clientcode Aufrufen einer Methode, die einen Wert zurückgibt und akzeptiert einen Parameter mit komplexen Typ in einer ASP.NET 4.5 Async-Methode**</span><span class="sxs-lookup"><span data-stu-id="917fb-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="917fb-254">**Aufrufen einer Methode, die einen Wert zurückgibt und nimmt einen Parameter mit komplexen Typ in eine synchrone Methode Clientcode**</span><span class="sxs-lookup"><span data-stu-id="917fb-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="917fb-255">Die `Invoke` Methode asynchron ausgeführt wird, und gibt eine `Task` Objekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="917fb-256">Wenn Sie keinen `await` oder `.Wait()`, die nächste Codezeile ausgeführt, bevor die Methode, die Sie aufrufen Ausführung beendet wurde.</span><span class="sxs-lookup"><span data-stu-id="917fb-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="917fb-257">Behandeln von Lebensdauer Verbindungsereignisse</span><span class="sxs-lookup"><span data-stu-id="917fb-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="917fb-258">SignalR bietet die folgenden Verbindung Lebensdauerereignisse, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="917fb-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="917fb-259">`Received`: Wird ausgelöst, wenn keine Daten für die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="917fb-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="917fb-260">Stellt die empfangenen Daten.</span><span class="sxs-lookup"><span data-stu-id="917fb-260">Provides the received data.</span></span>
- <span data-ttu-id="917fb-261">`ConnectionSlow`: Wird ausgelöst, wenn der Client eine Verbindung langsame oder häufig löschen erkennt.</span><span class="sxs-lookup"><span data-stu-id="917fb-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="917fb-262">`Reconnecting`: Wird ausgelöst, wenn die zugrunde liegenden Transport erneut zu verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="917fb-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="917fb-263">`Reconnected`: Wird ausgelöst, wenn die zugrunde liegenden Transport die Verbindung wiederhergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="917fb-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="917fb-264">`StateChanged`: Wird ausgelöst, wenn der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="917fb-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="917fb-265">Enthält der alte Status und den Zustand "Neu".</span><span class="sxs-lookup"><span data-stu-id="917fb-265">Provides the old state and the new state.</span></span> <span data-ttu-id="917fb-266">Informationen zur Verbindung Zustandswerte finden Sie unter [ConnectionState-Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="917fb-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="917fb-267">`Closed`: Wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="917fb-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="917fb-268">Z. B. Wenn Sie Warnmeldungen für Fehler anzeigen, die nicht schwerwiegend sind, aber dazu führen, dass vorübergehende Verbindungsprobleme möchten, wie dies darauf oder häufige Löschen der Verbindung, behandeln die `ConnectionSlow` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="917fb-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="917fb-269">Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="917fb-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="917fb-270">Gewusst wie: Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="917fb-270">How to handle errors</span></span>

<span data-ttu-id="917fb-271">Wenn Sie ausführliche Fehlermeldungen auf dem Server nicht explizit aktivieren, enthält das Ausnahmeobjekt, das SignalR nach einem Fehler gibt wenige Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="917fb-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="917fb-272">Angenommen, wenn ein Aufruf von `newContosoChatMessage` ein Fehler auftritt, der die Fehlermeldung in die Error-Objekt enthält "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" senden, detaillierte Fehlermeldungen für Clients in einer produktionsumgebung wird nicht empfohlen aus Sicherheitsgründen aber wenn Sie möchten detaillierte Fehlermeldungen für aktivieren Problembehandlung, verwenden Sie folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="917fb-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="917fb-273">Zum Behandeln von Fehlern, die SignalR auslöst, können Sie hinzufügen, einen Handler für das `Error` Ereignis für das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="917fb-274">Umschließen Sie den Code in einem Try / Catch-Block, um Fehler von Methodenaufrufen zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="917fb-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="917fb-275">Clientseitige Protokollierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="917fb-275">How to enable client-side logging</span></span>

<span data-ttu-id="917fb-276">Um die clientseitige Protokollierung zu aktivieren, legen die `TraceLevel` und `TraceWriter` Eigenschaften für das Verbindungsobjekt.</span><span class="sxs-lookup"><span data-stu-id="917fb-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="917fb-277">WPF und Silverlight Konsolenanwendung Codebeispiele für Clientmethoden, die der Server aufrufen können</span><span class="sxs-lookup"><span data-stu-id="917fb-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="917fb-278">Die Codebeispiele, die weiter oben dargestellten zum Definieren von Clientmethoden, die der Server aufrufen können, gelten für WinRT-Clients.</span><span class="sxs-lookup"><span data-stu-id="917fb-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="917fb-279">Den folgenden Beispielen wird den entsprechenden Code für WPF und Silverlight webanwendungsclients Konsole.</span><span class="sxs-lookup"><span data-stu-id="917fb-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="917fb-280">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="917fb-280">Methods without parameters</span></span>

<span data-ttu-id="917fb-281">**WPF-Clientcode für die Methode, die vom Server ohne Parameter aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="917fb-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="917fb-282">**Silverlight-Client-Code für die Methode, die vom Server ohne Parameter aufgerufen**</span><span class="sxs-lookup"><span data-stu-id="917fb-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="917fb-283">**Client-Anwendungscode für die Methode, die vom Server ohne Parameter aufgerufen-Konsole**</span><span class="sxs-lookup"><span data-stu-id="917fb-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="917fb-284">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="917fb-285">**WPF-Clientcode für eine Methode vom Server mit einem Parameter namens**</span><span class="sxs-lookup"><span data-stu-id="917fb-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="917fb-286">**Silverlight-Client-Code für eine Methode aufgerufen wird, vom Server mit einem parameter**</span><span class="sxs-lookup"><span data-stu-id="917fb-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="917fb-287">**Client-Anwendungscode für eine Methode vom Server mit einem Parameter namens-Konsole**</span><span class="sxs-lookup"><span data-stu-id="917fb-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="917fb-288">Methoden mit Parametern, dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="917fb-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="917fb-289">**WPF-Clientcode für eine Methode vom Server mit einem Parameter, verwenden für den Parameter ein dynamisches Objekt namens**</span><span class="sxs-lookup"><span data-stu-id="917fb-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="917fb-290">**Silverlight-Client-Code für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den parameter**</span><span class="sxs-lookup"><span data-stu-id="917fb-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="917fb-291">**Client-Anwendungscode für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den Parameter-Konsole**</span><span class="sxs-lookup"><span data-stu-id="917fb-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
