---
uid: signalr/overview/performance/signalr-performance
title: SignalR-Leistung | Microsoft Docs
author: pfletcher
description: SignalR-Leistung
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a><span data-ttu-id="8bf8a-103">SignalR-Leistung</span><span class="sxs-lookup"><span data-stu-id="8bf8a-103">SignalR Performance</span></span>
====================
<span data-ttu-id="8bf8a-104">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="8bf8a-105">Dieses Thema beschreibt, wie für entwerfen, messen und Verbessern der Leistung in einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8bf8a-106">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="8bf8a-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="8bf8a-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8bf8a-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8bf8a-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8bf8a-108">.NET 4.5</span></span>
> - <span data-ttu-id="8bf8a-109">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="8bf8a-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8bf8a-110">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="8bf8a-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="8bf8a-111">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8bf8a-112">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="8bf8a-112">Questions and comments</span></span>
> 
> <span data-ttu-id="8bf8a-113">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8bf8a-114">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="8bf8a-115">Eine aktuelle Präsentation auf SignalR-Leistung und Skalierung finden Sie unter [im Web in Echtzeit mit ASP.NET SignalR-Skalierung](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="8bf8a-116">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8bf8a-117">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="8bf8a-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="8bf8a-118">Optimieren des SignalR-Servers für die Leistung</span><span class="sxs-lookup"><span data-stu-id="8bf8a-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="8bf8a-119">Problembehandlung bei Performanceproblemen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="8bf8a-120">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="8bf8a-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="8bf8a-121">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="8bf8a-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="8bf8a-122">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="8bf8a-123">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="8bf8a-123">Design considerations</span></span>

<span data-ttu-id="8bf8a-124">Dieser Abschnitt beschreibt die Muster, die während des Entwurfs einer SignalR-Anwendung, um sicherzustellen, dass die Leistung nicht beeinträchtigt werden wird, generieren Sie unnötigen Netzwerkdatenverkehr implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="8bf8a-125">Einschränkung der Häufigkeit der Nachricht</span><span class="sxs-lookup"><span data-stu-id="8bf8a-125">Throttling message frequency</span></span>

<span data-ttu-id="8bf8a-126">Die meisten Anwendungen müssen nicht auch in eine Anwendung, die eine hohe Frequenz (z. B. eine Anwendung für den Echtzeit-Spiele zulassen) Nachrichten sendet, mehrere Nachrichten ein zweites zu senden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="8bf8a-127">Um den Umfang des Datenverkehrs zu reduzieren, die jedem Client generiert, eine Nachrichtenschleife, Warteschlangen und sendet Nachrichten nicht mehr als einer festen Rate häufig implementiert werden kann (d. h. bis zu einer bestimmten Anzahl von Nachrichten wird gesendet, pro Sekunde, wenn in diesem Zeitraum in Nachrichten vorhanden sind Terval gesendet werden).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="8bf8a-128">Eine beispielanwendung, die Nachrichten zu einer bestimmten Rate (von Client und Server) schränkt, finden Sie unter [hochfrequente Realtime mit SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="8bf8a-129">Verringern der Größe</span><span class="sxs-lookup"><span data-stu-id="8bf8a-129">Reducing message size</span></span>

<span data-ttu-id="8bf8a-130">Sie können die Größe einer SignalR-Nachricht durch Reduzierung der Größe der serialisierten Objekte reduzieren.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="8bf8a-131">Im Servercode, wenn Sie ein Objekt gesendet, die Eigenschaften, die nicht enthält übertragen werden müssen, verhindern, dass diese Eigenschaften serialisiert wird, mithilfe der `JsonIgnore` Attribut.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="8bf8a-132">Die Namen von Eigenschaften werden auch in der Nachricht gespeichert. die Namen von Eigenschaften über verkürzt die `JsonProperty` Attribut.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="8bf8a-133">Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft ausschließen, die an den Client gesendet werden, und wie Eigenschaftennamen zu verkürzen:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="8bf8a-134">**.NET Servercode, die das JsonIgnore-Attribut, um Daten auszuschließen, die an den Client gesendet werden und JsonProperty Attribut zum Reduzieren der Größe der Nachricht veranschaulicht**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="8bf8a-135">Um die Lesbarkeit beibehalten / bessere Verwaltbarkeit im Clientcode die abgekürzten Namen kann neu zugeordnete auf einem von Menschen benutzerfreundliche Namen nach dem Empfang der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="8bf8a-136">Das folgende Codebeispiel veranschaulicht eine Möglichkeit der neuzuordnung verkürzter Namen länger Vorgängen, durch die Definition eines Nachrichtenvertrags (Zuordnung), und Verwenden der `reMap` Funktion, die den Vertrag der optimierte Nachrichtenklasse angewendet:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="8bf8a-137">**Ordnet die clientseitige-JavaScript-Code verkürzt Eigenschaftennamen, die einem von Menschen lesbaren Namen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="8bf8a-138">Namen können in Nachrichten vom Client an den Server auch verkürzt werden mit derselben Methode.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="8bf8a-139">Reduzierung des Platzbedarfs Speicher (d. h. die Menge des Arbeitsspeichers, die für die Nachricht verwendet) der Nachricht Objekt kann auch die Leistung verbessern.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="8bf8a-140">Angenommen, wenn das gesamte Spektrum der ein `int` ist nicht erforderlich, eine `short` oder `byte` kann stattdessen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="8bf8a-141">Da Nachrichten im Nachrichtenbus im Arbeitsspeicher des Servers, reduzieren die Größe der Nachrichten gespeichert werden können Sie auch Adresse Server von Arbeitsspeicherproblemen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="8bf8a-142">Optimieren des SignalR-Servers für die Leistung</span><span class="sxs-lookup"><span data-stu-id="8bf8a-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="8bf8a-143">Die folgenden Konfigurationseinstellungen können verwendet werden, um den Server für eine bessere Leistung in einer SignalR-Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="8bf8a-144">Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter [verbessern die Leistung von ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="8bf8a-145">**SignalR-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="8bf8a-146">**DefaultMessageBufferSize**: Standardmäßig werden SignalR 1000 Nachrichten im Arbeitsspeicher pro Hub pro Verbindung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="8bf8a-147">Wenn große Nachrichten verwendet werden, kann dies Speicherprobleme zu erstellen, die behoben werden können, durch Verringern dieses Werts.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="8bf8a-148">Diese Einstellung kann festgelegt werden, der `Application_Start` -Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration` Methode einer OWIN-Start-Klasse in einer selbst gehosteten Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="8bf8a-149">Im folgende Beispiel wird veranschaulicht, wie Sie diesen Wert verringern, um Ihrer Anwendung, um die Menge des belegten Server zu reduzieren den Speicherbedarf zu reduzieren:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="8bf8a-150">**.NET Servercode in Startup.cs zum Verringern der Größe des Nachrichtenpuffers Standard**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="8bf8a-151">**IIS-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="8bf8a-152">**Max. Anzahl gleichzeitiger Anforderungen pro Anwendung**: die Anzahl der gleichzeitigen IIS Anforderungen steigt auch die Serverressourcen verfügbar sind, Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="8bf8a-153">Der Standardwert ist 5000. Um diese Einstellung erhöhen möchten, führen Sie die folgenden Befehle in einem Eingabeaufforderungsfenster mit erhöhten Rechten aus:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="8bf8a-154">**ApplicationPool QueueLength**: Dies ist die maximale Anzahl von Anforderungen, die Warteschlangen von "http.sys" für den Anwendungspool.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="8bf8a-155">Wenn die Warteschlange voll ist, erhalten neue Anforderungen Antworten 503 "Dienst nicht verfügbar".</span><span class="sxs-lookup"><span data-stu-id="8bf8a-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="8bf8a-156">Der Standardwert ist 1000.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-156">The default value is 1000.</span></span>

    <span data-ttu-id="8bf8a-157">Verkürzen Sie die Warteschlangenlänge für den Arbeitsprozess im Anwendungspool Hosten der Anwendung wird die Speicher-Ressourcen freizugeben.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="8bf8a-158">Weitere Informationen finden Sie unter [verwalten, Optimierung und Konfigurieren von Anwendungspools](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span></span>

<span data-ttu-id="8bf8a-159">**Konfiguration der ASP.NET-Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="8bf8a-160">Dieser Abschnitt enthält Konfigurationseinstellungen, die festgelegt werden können, in der `aspnet.config` Datei.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="8bf8a-161">Diese Datei befindet sich in einem von zwei Speicherorten, je nach Plattform:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="8bf8a-162">Die folgenden: ASP.NET-Einstellungen, die SignalR-Leistung verbessern können</span><span class="sxs-lookup"><span data-stu-id="8bf8a-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="8bf8a-163">**Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: erhöhen diese Einstellung möglicherweise Leistungsengpässe verringern.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="8bf8a-164">Um diese Einstellung erhöhen möchten, fügen Sie die folgende Konfigurationseinstellung der `aspnet.config` Datei:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="8bf8a-165">**Anfordern der Begrenzung der Warteschlange**: Wenn die Gesamtzahl der Verbindungen überschreitet die `maxConcurrentRequestsPerCPU` festlegen, ASP.NET startet Einschränkung Anforderungen, die über eine Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="8bf8a-166">Um die Größe der Warteschlange zu erhöhen, erhöhen Sie die `requestQueueLimit` Einstellung.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="8bf8a-167">Fügen Sie hierzu die folgende Konfigurationseinstellung der `processModel` Knoten `config/machine.config` (statt `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="8bf8a-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="8bf8a-168">Problembehandlung bei Performanceproblemen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="8bf8a-169">Dieser Abschnitt beschreibt die Möglichkeiten, um Leistungsengpässe in der Anwendung zu finden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="8bf8a-170">Überprüfen, dass ein WebSocket verwendet wird</span><span class="sxs-lookup"><span data-stu-id="8bf8a-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="8bf8a-171">Obgleich SignalR eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, WebSocket bietet einen deutlichen Leistungsvorteil und sollte verwendet werden, wenn Client und Server unterstützen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="8bf8a-172">Um festzustellen, ob der Client-als auch die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="8bf8a-173">Um zu bestimmen, welcher Transport in Ihrer Anwendung verwendet wird, können Sie die Browser-Entwicklertools verwenden, und überprüfen die Protokolle, um festzustellen, welcher Transport für die Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="8bf8a-174">Informationen zur Verwendung der Browser-Entwicklungstools in Internet Explorer und Chrome finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="8bf8a-175">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="8bf8a-175">Using SignalR performance counters</span></span>

<span data-ttu-id="8bf8a-176">In diesem Abschnitt wird beschrieben, wie aktivieren und Verwenden von SignalR-Leistungsindikatoren finden der `Microsoft.AspNet.SignalR.Utils` Paket.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="8bf8a-177">Installieren von signalr.exe</span><span class="sxs-lookup"><span data-stu-id="8bf8a-177">Installing signalr.exe</span></span>

<span data-ttu-id="8bf8a-178">Leistungsindikatoren können mit dem Server mithilfe eines Dienstprogramms namens SignalR.exe hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="8bf8a-179">Um dieses Programm zu installieren, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="8bf8a-180">Wählen Sie in der Visual Studio-Anwendung **Tools**, **Bibliothekspaket-Manager**, **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="8bf8a-181">Suchen Sie nach **signalr.utils**, und wählen Sie installieren.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="8bf8a-182">Stimmen Sie dem Lizenzvertrag zum Installieren des Pakets an.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="8bf8a-183">SignalR.exe installiert werden soll `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="8bf8a-184">Installieren von Leistungsindikatoren mit SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="8bf8a-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="8bf8a-185">Führen Sie zum Installieren von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="8bf8a-186">Führen Sie zum Entfernen von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="8bf8a-187">SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="8bf8a-187">SignalR Performance counters</span></span>

<span data-ttu-id="8bf8a-188">Das Hilfsprogramme installiert der folgenden Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="8bf8a-189">Die Leistungsindikatoren "Total" messen die Anzahl der Ereignisse an, seit der letzten Anwendungspool oder den Server neu starten.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="8bf8a-190">**Verbindung Metriken**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-190">**Connection metrics**</span></span>

<span data-ttu-id="8bf8a-191">Die folgenden Metriken messen die Lebensdauer-Verbindungsereignisse, die auftreten.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="8bf8a-192">Weitere Informationen finden Sie unter [verstehen und Behandeln von Verbindungsereignisse Lebensdauer](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="8bf8a-193">**Verbindungen verbunden**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-193">**Connections Connected**</span></span>
- <span data-ttu-id="8bf8a-194">**Verbindungen, die die Verbindung wiederhergestellt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="8bf8a-195">**Verbindungen getrennt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="8bf8a-196">**Aktuelle Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-196">**Connections Current**</span></span>

<span data-ttu-id="8bf8a-197">**Nachrichtenmetrik**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-197">**Message metrics**</span></span>

<span data-ttu-id="8bf8a-198">Die folgenden Metriken messen dem Volumen des Nachrichtenverkehrs von SignalR generiert.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="8bf8a-199">**Gesamtanzahl der empfangenen Nachrichten Verbindung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="8bf8a-200">**Gesamtanzahl der gesendeten Nachrichten Verbindung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="8bf8a-201">**Verbindung empfangene Nachrichten/s**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="8bf8a-202">**Verbindung gesendete Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="8bf8a-203">**Bus Nachrichtenmetrik**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-203">**Message bus metrics**</span></span>

<span data-ttu-id="8bf8a-204">Die folgenden Metriken messen Datenverkehr über den internen SignalR-Nachrichtenbus der Warteschlange, in der alle eingehende und ausgehende SignalR-Nachrichten eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="8bf8a-205">Eine Nachricht ist **veröffentlichte** wann sie gesendet oder übertragen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="8bf8a-206">Ein **Abonnenten** in diesem Kontext wird ein Abonnement für den Nachrichtenbus; Dies sollte die Anzahl der Clients und dem Server selbst entsprechen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="8bf8a-207">Ein **reservierten Worker** ist eine Komponente, die Daten an die aktiven Verbindungen, sendet eine **ausgelasteten Worker** ist eine, die aktiv eine Nachricht sendet.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="8bf8a-208">**Nachrichtenbus Nachrichten empfangene gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="8bf8a-209">**Nachrichtenbus Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="8bf8a-210">**Nachricht Bus Nachrichten veröffentlicht gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="8bf8a-211">**Nachrichtenbus Nachrichten veröffentlicht/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="8bf8a-212">**Nachricht Bus Abonnenten aktuelle**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="8bf8a-213">**Bus Abonnenten gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="8bf8a-214">**Nachricht Bus Abonnenten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="8bf8a-215">**Nachrichtenbus reservierten Worker**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="8bf8a-216">**Nachricht Bus ausgelasteter Worker**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="8bf8a-217">**Nachricht Bus Themen aktuelle**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="8bf8a-218">**Fehler-Metriken**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-218">**Error metrics**</span></span>

<span data-ttu-id="8bf8a-219">Die folgenden Metriken Messen von SignalR Volumen des Nachrichtenverkehrs generierten Fehler.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="8bf8a-220">**Hub-Auflösung** Fehler auftreten, wenn Sie einen Hub oder hubmethode nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="8bf8a-221">**Hubaufruf** Fehler sind beim Aufrufen einer hubmethode ausgelösten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="8bf8a-222">**Transport** Fehler sind Verbindungsfehler während einer HTTP-Anforderung oder Antwort ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="8bf8a-223">**Fehler: Alle gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-223">**Errors: All Total**</span></span>
- <span data-ttu-id="8bf8a-224">**Fehler: Alle/Sek.**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="8bf8a-225">**Fehler: Gesamtanzahl der Hub-Lösung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="8bf8a-226">**Fehler: Hub Auflösung/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="8bf8a-227">**Fehler: Gesamtanzahl der Hub-Aufruf**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="8bf8a-228">**Fehler: Hub Aufruf/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="8bf8a-229">**Fehler: Transport gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="8bf8a-230">**Fehler: Transport/Sek.**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="8bf8a-231">**Metriken mit horizontaler Skalierung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-231">**Scaleout metrics**</span></span>

<span data-ttu-id="8bf8a-232">Die folgenden Metriken Measuregruppen Datenverkehr und Fehler, die vom Anbieter mit horizontaler Skalierung.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="8bf8a-233">Ein **Stream** in diesem Kontext ist eine Skalierungseinheit vom Anbieter mit horizontaler Skalierung verwendet; dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird und ein Abonnement, wenn Redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="8bf8a-234">Jeder Datenstrom wird sichergestellt, dass geordnete Lese- und Schreibvorgänge; ein einzigen Datenstrom ist ein potenzieller Engpass für die Skalierung, damit die Anzahl der Datenströme vergrößert werden kann, um diesen Engpass verringern.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="8bf8a-235">Wenn mehrere Datenströme verwendet werden, wird SignalR automatisch (Shard) Nachrichten auf diese Streams auf eine Weise zu verteilen, der sicherstellt, dass von pro Verbindung gesendeten Nachrichten in der Reihenfolge sind.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="8bf8a-236">Die [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) Einstellung steuert die Länge der Sendewarteschlange mit horizontaler Skalierung von SignalR verwaltet.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-236">The [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="8bf8a-237">Festlegen auf einen Wert größer als 0 alle Nachrichten in einer Sendewarteschlange jeweils einzeln an die konfigurierten messagingbackplane gesendet werden abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="8bf8a-238">Wenn die Größe der Warteschlange für die konfigurierte Länge überschreitet, nachfolgende Aufrufe zum Senden von wird sofort mit Fehlschlagen einer [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) bis die Anzahl der Nachrichten in der Warteschlange kleiner als die Einstellung ist erneut.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="8bf8a-239">Message Queueing ist standardmäßig deaktiviert, da der implementierten Backplanes in der Regel eigene queuing oder datenflusssteuerung erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="8bf8a-240">Im Fall von SQL Server schränkt das Verbindungspooling effektiv die Anzahl Sendevorgänge jederzeit erfolgen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="8bf8a-241">Standardmäßig nur einen Stream für SQL Server und Redis verwendet wird, werden fünf Streams für Service Bus verwendet und warteschlangenanwendungen ist deaktiviert, aber diese Einstellungen können über die Konfiguration für SQL Server und Service Bus geändert werden:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="8bf8a-242">**.NET Servercode zur Konfiguration der Tabelle die Anzahl und die Warteschlange Länge für SQL Server-Rückwandplatine**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="8bf8a-243">**Servercode für .NET zum Konfigurieren von Thema Count und Warteschlange Länge für Service Bus-Rückwandplatine**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="8bf8a-244">Ein **Pufferung** ist eine, die einen Fehlerzustand eingegeben hat; wird der Stream in den Fehlerzustand, alle Nachrichten, die an die Backplane gesendet werden erst, wenn sofort nicht mehr Fault.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="8bf8a-245">Die **Länge der Sendewarteschlange** ist die Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="8bf8a-246">**Nachrichtenbus mit horizontaler Skalierung Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="8bf8a-247">**Warteschlangen für horizontale Skalierung Streams gesamt**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="8bf8a-248">**Warteschlangen für horizontale Skalierung streamt öffnen**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="8bf8a-249">**Warteschlangen für horizontale Skalierung Streams Pufferung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="8bf8a-250">**Gesamtanzahl der Fehler mit horizontaler Skalierung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="8bf8a-251">**Warteschlangen für horizontale Skalierung/Sek.**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="8bf8a-252">**Länge der Sendewarteschlange für horizontale Skalierung**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="8bf8a-253">Weitere Informationen, was diese Indikatoren gemessen werden, finden Sie unter [SignalR Scaleout mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="8bf8a-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="8bf8a-254">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="8bf8a-254">Using other performance counters</span></span>

<span data-ttu-id="8bf8a-255">Die folgenden Leistungsindikatoren möglicherweise auch bei der Überwachung der Leistung Ihrer Anwendung nützlich.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="8bf8a-256">**Arbeitsspeicher**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-256">**Memory**</span></span>

- <span data-ttu-id="8bf8a-257">.NET CLR-Speicher\\# Bytes in allen Heaps (für w3wp)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="8bf8a-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-258">**ASP.NET**</span></span>

- <span data-ttu-id="8bf8a-259">Aktuelle "ASP.net\aktuelle Anforderungen"</span><span class="sxs-lookup"><span data-stu-id="8bf8a-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="8bf8a-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="8bf8a-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="8bf8a-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="8bf8a-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="8bf8a-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-262">**CPU**</span></span>

- <span data-ttu-id="8bf8a-263">Information\Processor Prozessorzeit</span><span class="sxs-lookup"><span data-stu-id="8bf8a-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="8bf8a-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-264">**TCP/IP**</span></span>

- <span data-ttu-id="8bf8a-265">TCPv6/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="8bf8a-266">TCPv4/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="8bf8a-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="8bf8a-267">**Webdienst**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-267">**Web Service**</span></span>

- <span data-ttu-id="8bf8a-268">Web-Dienst\Aktuelle Verbindungen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="8bf8a-269">Service\Maximum Webverbindungen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="8bf8a-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-270">**Threading**</span></span>

- <span data-ttu-id="8bf8a-271">.NET CLR-Sperren und Threads\\Anzahl aktuelle logische Threads</span><span class="sxs-lookup"><span data-stu-id="8bf8a-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="8bf8a-272">.NET CLR-Sperren und Threads\\Anzahl aktuelle physische Threads</span><span class="sxs-lookup"><span data-stu-id="8bf8a-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="8bf8a-273">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8bf8a-273">Other Resources</span></span>

<span data-ttu-id="8bf8a-274">Weitere Informationen zu ASP.NET-Leistung überwachen und optimieren finden Sie unter den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="8bf8a-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="8bf8a-275">[ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="8bf8a-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0 und IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="8bf8a-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="8bf8a-277">&lt;ApplicationPool&gt; Element (Webeinstellungen)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
