---
uid: signalr/overview/performance/signalr-performance
title: SignalR-Leistung | Microsoft-Dokumentation
author: pfletcher
description: SignalR-Leistung
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 9346f0ff9720361f07afe196f59305f0f38ffe8a
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287770"
---
<a name="signalr-performance"></a><span data-ttu-id="6f091-103">SignalR-Leistung</span><span class="sxs-lookup"><span data-stu-id="6f091-103">SignalR Performance</span></span>
====================
<span data-ttu-id="6f091-104">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6f091-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="6f091-105">In diesem Thema wird beschrieben, wie für entwerfen, zu messen und Verbessern der Leistung in einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6f091-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6f091-106">In diesem Thema verwendeten Softwareversionen</span><span class="sxs-lookup"><span data-stu-id="6f091-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="6f091-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6f091-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="6f091-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6f091-108">.NET 4.5</span></span>
> - <span data-ttu-id="6f091-109">SignalR-Version 2</span><span class="sxs-lookup"><span data-stu-id="6f091-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6f091-110">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="6f091-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="6f091-111">Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6f091-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="6f091-112">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="6f091-112">Questions and comments</span></span>
>
> <span data-ttu-id="6f091-113">Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="6f091-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6f091-114">Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6f091-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="6f091-115">Aktuelle Darstellung der SignalR-Leistung und Skalierung, finden Sie unter [Skalierung im Web in Echtzeit mit SignalR für ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="6f091-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="6f091-116">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="6f091-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6f091-117">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="6f091-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="6f091-118">Optimieren der Leistung des Servers SignalR</span><span class="sxs-lookup"><span data-stu-id="6f091-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="6f091-119">Behandeln von Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="6f091-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="6f091-120">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="6f091-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="6f091-121">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="6f091-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="6f091-122">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6f091-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="6f091-123">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="6f091-123">Design considerations</span></span>

<span data-ttu-id="6f091-124">Dieser Abschnitt beschreibt die Muster, die implementiert werden können, während des Entwurfs einer SignalR-Anwendung, um sicherzustellen, dass die Leistung nicht beeinträchtigt wird wird, indem Sie unnötigen Netzwerkdatenverkehr generieren.</span><span class="sxs-lookup"><span data-stu-id="6f091-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="6f091-125">Einschränkung der Häufigkeit der Nachricht</span><span class="sxs-lookup"><span data-stu-id="6f091-125">Throttling message frequency</span></span>

<span data-ttu-id="6f091-126">Die meisten Anwendungen müssen nicht selbst in Anwendungen, die Nachrichten mit hoher Frequenz (z. B. eine Echtzeit-Gaming-Anwendung) sendet, mehr als ein paar Nachrichten pro Sekunde zu senden.</span><span class="sxs-lookup"><span data-stu-id="6f091-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="6f091-127">Um die Menge des Datenverkehrs zu verringern, die von jedem Client generiert, eine Meldungsschleife kann implementiert werden, dass Warteschlangen und sendet Nachrichten häufig nicht mehr als einer festen Rate (d. h. bis zu einer bestimmten Anzahl von Nachrichten gesendet pro Sekunde, wenn sich Nachrichten in dieser Zeit in euerstellungsintervall gesendet werden).</span><span class="sxs-lookup"><span data-stu-id="6f091-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="6f091-128">Eine beispielanwendung, die Nachrichten an eine bestimmte Rate an (von Client und Server) drosselt, finden Sie unter [Echtzeitnachrichten mit SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6f091-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="6f091-129">Verringern der Größe</span><span class="sxs-lookup"><span data-stu-id="6f091-129">Reducing message size</span></span>

<span data-ttu-id="6f091-130">Sie können die Größe einer SignalR-Nachricht reduzieren, durch die Reduzierung der serialisierten Objekte.</span><span class="sxs-lookup"><span data-stu-id="6f091-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="6f091-131">Im Server-Code, wenn Sie ein Objekt senden, das Eigenschaften, die nicht enthält übertragen werden müssen, verhindern, dass diese Eigenschaften serialisiert werden, indem Sie mit der `JsonIgnore` Attribut.</span><span class="sxs-lookup"><span data-stu-id="6f091-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="6f091-132">Die Namen von Eigenschaften werden auch in der Nachricht gespeichert. die Namen der Eigenschaften können verkürzt werden, mit der `JsonProperty` Attribut.</span><span class="sxs-lookup"><span data-stu-id="6f091-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="6f091-133">Im folgenden Codebeispiel wird veranschaulicht, wie Sie ausschließen möchten, dass eine Eigenschaft, die an den Client gesendet werden, und Eigenschaftsnamen zu verkürzen:</span><span class="sxs-lookup"><span data-stu-id="6f091-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="6f091-134">**.NET Server-Code, die zeigt, das JsonIgnore-Attribut, um Daten auszuschließen, die an den Client gesendet werden, und das Attribut "jsonproperty" Reduzieren der Größe**</span><span class="sxs-lookup"><span data-stu-id="6f091-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="6f091-135">Um die Lesbarkeit beibehalten / Verwaltbarkeit im Clientcode, den abgekürzten Namen können neu zugeordnete, Benutzerfreundlicher Namen nach dem die Nachricht empfangen wird.</span><span class="sxs-lookup"><span data-stu-id="6f091-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="6f091-136">Das folgende Codebeispiel veranschaulicht eine Möglichkeit der neuzuordnung verkürzter Namen länger werden, durch die Definition eines Nachrichtenvertrags (Zuordnung), und Verwenden der `reMap` Funktion, die den Vertrag für die optimierte Nachrichtenklasse gelten:</span><span class="sxs-lookup"><span data-stu-id="6f091-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="6f091-137">**Die clientseitige JavaScript-Code, der zugeordnet verkürzt Eigenschaftsnamen für Menschen lesbaren Namen**</span><span class="sxs-lookup"><span data-stu-id="6f091-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="6f091-138">Namen können in den Nachrichten vom Client an den Server auch verkürzt werden mit derselben Methode.</span><span class="sxs-lookup"><span data-stu-id="6f091-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="6f091-139">Reduziert den Speicherbedarf (d. h. die Menge des Arbeitsspeichers, die für die Nachricht verwendet) der Nachricht Objekt kann auch die Leistung verbessern.</span><span class="sxs-lookup"><span data-stu-id="6f091-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="6f091-140">Z. B. wenn das gesamte Spektrum ein `int` ist nicht erforderlich, eine `short` oder `byte` kann stattdessen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6f091-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="6f091-141">Da Nachrichten im Nachrichtenbus im Arbeitsspeicher des Servers, verringern der Größe der Nachrichten gespeichert werden, können Sie auch eine Adresse Server Arbeitsspeicherproblemen.</span><span class="sxs-lookup"><span data-stu-id="6f091-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="6f091-142">Optimieren der Leistung des Servers SignalR</span><span class="sxs-lookup"><span data-stu-id="6f091-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="6f091-143">Die folgenden Konfigurationseinstellungen können verwendet werden, um den Server für eine bessere Leistung in einer SignalR-Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="6f091-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="6f091-144">Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter [Verbessern der Leistung von ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f091-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="6f091-145">**SignalR-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="6f091-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="6f091-146">**DefaultMessageBufferSize**: Standardmäßig behält SignalR 1000-Nachrichten im Arbeitsspeicher pro Hub pro Verbindung.</span><span class="sxs-lookup"><span data-stu-id="6f091-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="6f091-147">Wenn große Nachrichten verwendet werden, kann dies Arbeitsspeicherproblemen erstellen, die durch eine Reduzierung dieses Werts verringert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6f091-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="6f091-148">Diese Einstellung kann festgelegt werden, der `Application_Start` -Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration` Methode eine OWIN-Startup-Klasse in einer selbst gehosteten Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6f091-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="6f091-149">Im folgende Beispiel wird veranschaulicht, wie kann ich diesen Wert verringern, um verringern den Speicherbedarf Ihrer Anwendung, um den Server verwendete Speichermenge zu reduzieren:</span><span class="sxs-lookup"><span data-stu-id="6f091-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="6f091-150">**.NET Server-Code in "Startup.cs" zum Verringern der Größe des Nachrichtenpuffers Standard**</span><span class="sxs-lookup"><span data-stu-id="6f091-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="6f091-151">**IIS-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="6f091-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="6f091-152">**Maximale Anzahl gleichzeitiger Anforderungen pro Anwendung**: Erhöhen der Anzahl von gleichzeitigen IIS erhöht Anforderungen Serverressourcen verfügbar sind, Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="6f091-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="6f091-153">Der Standardwert ist 5000. Um diese Einstellung erhöhen, führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus:</span><span class="sxs-lookup"><span data-stu-id="6f091-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="6f091-154">**ApplicationPool QueueLength**: Dies ist die maximale Anzahl von Anforderungen für Http.sys für den Anwendungspool Warteschlangen.</span><span class="sxs-lookup"><span data-stu-id="6f091-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="6f091-155">Wenn die Warteschlange voll ist, erhalten neue Anforderungen eine Antwort mit 503 "Dienst nicht verfügbar".</span><span class="sxs-lookup"><span data-stu-id="6f091-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="6f091-156">Der Standardwert ist 1000.</span><span class="sxs-lookup"><span data-stu-id="6f091-156">The default value is 1000.</span></span>

    <span data-ttu-id="6f091-157">Verkürzen Sie die Länge der Warteschlange für den Arbeitsprozess im Anwendungspool Hosten der Anwendung wird die Speicher-Ressourcen freizugeben.</span><span class="sxs-lookup"><span data-stu-id="6f091-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="6f091-158">Weitere Informationen finden Sie unter [verwalten, Optimierung und Konfigurieren von Anwendungspools](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f091-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="6f091-159">**ASP.NET-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="6f091-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="6f091-160">Dieser Abschnitt enthält Konfigurationseinstellungen, die festgelegt werden können, in der `aspnet.config` Datei.</span><span class="sxs-lookup"><span data-stu-id="6f091-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="6f091-161">Diese Datei befindet sich in einem von zwei Speicherorten, je nach Plattform:</span><span class="sxs-lookup"><span data-stu-id="6f091-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="6f091-162">Die folgenden: ASP.NET-Einstellungen, die SignalR-Leistung verbessern können</span><span class="sxs-lookup"><span data-stu-id="6f091-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="6f091-163">**Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: Erhöhen diese Einstellung möglicherweise Leistungsengpässe verringern.</span><span class="sxs-lookup"><span data-stu-id="6f091-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="6f091-164">Um diese Einstellung erhöhen, fügen Sie die folgende Konfigurationseinstellung der `aspnet.config` Datei:</span><span class="sxs-lookup"><span data-stu-id="6f091-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="6f091-165">**Begrenzung für Anforderungswarteschlange**: Wenn die Gesamtzahl der Verbindungen überschreitet die `maxConcurrentRequestsPerCPU` festlegen, ASP.NET startet drosselungsanforderungen über eine Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="6f091-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="6f091-166">Um die Größe der Warteschlange zu erhöhen, können Sie erhöhen die `requestQueueLimit` festlegen.</span><span class="sxs-lookup"><span data-stu-id="6f091-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="6f091-167">Zu diesem Zweck fügen Sie die folgende Konfigurationseinstellung der `processModel` Knoten `config/machine.config` (statt `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="6f091-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="6f091-168">Behandeln von Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="6f091-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="6f091-169">Dieser Abschnitt beschreibt die Möglichkeiten, um Leistungsengpässe in Ihrer Anwendung zu finden.</span><span class="sxs-lookup"><span data-stu-id="6f091-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="6f091-170">Überprüfen, dass WebSocket verwendet wird</span><span class="sxs-lookup"><span data-stu-id="6f091-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="6f091-171">Obwohl SignalR eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, werden WebSocket bietet einen deutlichen Leistungsvorteil, und sollte verwendet werden, wenn der Client und Server unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6f091-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="6f091-172">Um zu bestimmen, wenn sich Client und Server die Anforderungen für den WebSocket erfüllt, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6f091-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="6f091-173">Um zu bestimmen, welcher Transport in Ihrer Anwendung verwendet wird, können Sie das browserentwicklertools verwenden, und überprüfen die Protokolle, um festzustellen, welcher Transport für die Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6f091-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="6f091-174">Weitere Informationen finden Sie unter den Browser-Entwicklungstools in Internet Explorer und Chrome, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6f091-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="6f091-175">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="6f091-175">Using SignalR performance counters</span></span>

<span data-ttu-id="6f091-176">In diesem Abschnitt wird beschrieben, wie aktivieren und Verwenden von SignalR-Leistungsindikatoren finden Sie in der `Microsoft.AspNet.SignalR.Utils` Paket.</span><span class="sxs-lookup"><span data-stu-id="6f091-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="6f091-177">Installieren von signalr.exe</span><span class="sxs-lookup"><span data-stu-id="6f091-177">Installing signalr.exe</span></span>

<span data-ttu-id="6f091-178">Leistungsindikatoren können mit dem Server mithilfe eines Dienstprogramms namens SignalR.exe hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6f091-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="6f091-179">Um dieses Hilfsprogramm zu installieren, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6f091-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="6f091-180">Wählen Sie in Visual Studio **Tools** > **NuGet Paket-Manager** > **NuGet-Pakete verwalten Lösung**</span><span class="sxs-lookup"><span data-stu-id="6f091-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="6f091-181">Suchen Sie nach **signalr.utils**, und wählen Sie installieren.</span><span class="sxs-lookup"><span data-stu-id="6f091-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="6f091-182">Akzeptieren des Lizenzvertrags zum Installieren des Pakets an.</span><span class="sxs-lookup"><span data-stu-id="6f091-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="6f091-183">SignalR.exe installiert werden soll `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="6f091-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="6f091-184">Installieren von Leistungsindikatoren mit SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="6f091-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="6f091-185">Führen Sie zum Installieren von SignalR-Leistungsindikatoren, SignalR.exe in einer Eingabeaufforderung mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="6f091-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="6f091-186">Führen Sie zum Entfernen von SignalR-Leistungsindikatoren SignalR.exe in einer Eingabeaufforderung mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="6f091-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="6f091-187">SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="6f091-187">SignalR Performance counters</span></span>

<span data-ttu-id="6f091-188">Die Hilfsprogramme-Paket installiert die folgenden Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="6f091-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="6f091-189">Die "Gesamt" Indikatoren messen die Anzahl der Ereignisse, seit der letzten Anwendungspool oder die Server neu starten.</span><span class="sxs-lookup"><span data-stu-id="6f091-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="6f091-190">**Verbindungsmetriken**</span><span class="sxs-lookup"><span data-stu-id="6f091-190">**Connection metrics**</span></span>

<span data-ttu-id="6f091-191">Die folgenden Metriken messen Sie die Verbindung Objektlebensdauer-Ereignisse, die auftreten.</span><span class="sxs-lookup"><span data-stu-id="6f091-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="6f091-192">Weitere Informationen finden Sie unter [verstehen und Verbindung Objektlebensdauer-Ereignisse behandeln](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6f091-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="6f091-193">**Verbindungen mit verbunden**</span><span class="sxs-lookup"><span data-stu-id="6f091-193">**Connections Connected**</span></span>
- <span data-ttu-id="6f091-194">**Verbindungen, die die Verbindung wiederhergestellt**</span><span class="sxs-lookup"><span data-stu-id="6f091-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="6f091-195">**Verbindungen, die die Verbindung getrennt**</span><span class="sxs-lookup"><span data-stu-id="6f091-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="6f091-196">**Aktuelle für Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="6f091-196">**Connections Current**</span></span>

<span data-ttu-id="6f091-197">**Nachrichtenmetrik**</span><span class="sxs-lookup"><span data-stu-id="6f091-197">**Message metrics**</span></span>

<span data-ttu-id="6f091-198">Die folgenden Metriken messen, den Nachrichtenverkehr, die von SignalR generiert wird.</span><span class="sxs-lookup"><span data-stu-id="6f091-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="6f091-199">**Gesamtanzahl der empfangenen Nachrichten der Verbindung**</span><span class="sxs-lookup"><span data-stu-id="6f091-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="6f091-200">**Verbindung gesendeten Daten insgesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="6f091-201">**Verbindung empfangene Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="6f091-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="6f091-202">**Verbindung gesendete Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="6f091-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="6f091-203">**Message Bus-Metriken**</span><span class="sxs-lookup"><span data-stu-id="6f091-203">**Message bus metrics**</span></span>

<span data-ttu-id="6f091-204">Die folgenden Metriken Messen von Datenverkehr über die interne SignalR-Nachrichtenbus der Warteschlange, in dem alle eingehende und ausgehende SignalR-Nachrichten platziert werden.</span><span class="sxs-lookup"><span data-stu-id="6f091-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="6f091-205">Eine Nachricht ist **veröffentlicht** Wenn gesendet oder übertragen.</span><span class="sxs-lookup"><span data-stu-id="6f091-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="6f091-206">Ein **Abonnenten** in diesem Kontext wird ein Abonnement für den Nachrichtenbus; Dies sollte die Anzahl der Clients und den Server selbst entsprechen.</span><span class="sxs-lookup"><span data-stu-id="6f091-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="6f091-207">Ein **Worker zugewiesen** ist eine Komponente, die Daten an die aktiven Verbindungen, sendet eine **ausgelasteten Worker** ist eine, die aktiv eine Nachricht sendet.</span><span class="sxs-lookup"><span data-stu-id="6f091-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="6f091-208">**Nachrichtenbus Nachrichten empfangen gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="6f091-209">**Nachrichtenbus Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6f091-210">**Message Bus-Nachrichten veröffentlicht gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="6f091-211">**Nachrichtenbus Nachrichten veröffentlicht/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="6f091-212">**Message Bus Abonnenten aktuelle**</span><span class="sxs-lookup"><span data-stu-id="6f091-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="6f091-213">**Message Bus Abonnenten gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="6f091-214">**Message Bus Abonnenten/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="6f091-215">**Nachrichtenbus Worker zugewiesen**</span><span class="sxs-lookup"><span data-stu-id="6f091-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="6f091-216">**Message Bus ausgelasteter Worker**</span><span class="sxs-lookup"><span data-stu-id="6f091-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="6f091-217">**Message Bus-Themen aktuelle**</span><span class="sxs-lookup"><span data-stu-id="6f091-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="6f091-218">**Fehlermetriken**</span><span class="sxs-lookup"><span data-stu-id="6f091-218">**Error metrics**</span></span>

<span data-ttu-id="6f091-219">Die folgenden Metriken Messen von SignalR-Nachrichtenverkehr generierten Fehler.</span><span class="sxs-lookup"><span data-stu-id="6f091-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="6f091-220">**Hub-Lösung** Fehler auftreten, wenn Sie einen Hub oder hubmethode nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="6f091-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="6f091-221">**Hubaufruf** Fehler sind Ausnahmen, die beim Aufruf einer hubmethode.</span><span class="sxs-lookup"><span data-stu-id="6f091-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="6f091-222">**Transport** treten Verbindungsfehler während einer HTTP-Anforderung oder Antwort ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="6f091-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="6f091-223">**Fehler: Alle gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-223">**Errors: All Total**</span></span>
- <span data-ttu-id="6f091-224">**Fehler: Alle/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="6f091-225">**Fehler: Gesamtanzahl der Hub-Lösung**</span><span class="sxs-lookup"><span data-stu-id="6f091-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="6f091-226">**Fehler: Hub-Lösung/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="6f091-227">**Fehler: Hub-aufrufen gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="6f091-228">**Fehler: Hub-aufrufen/Sek.**</span><span class="sxs-lookup"><span data-stu-id="6f091-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="6f091-229">**Fehler: Transport gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="6f091-230">**Fehler: Transport/Sek.**</span><span class="sxs-lookup"><span data-stu-id="6f091-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="6f091-231">**Horizontale Skalierung Metriken**</span><span class="sxs-lookup"><span data-stu-id="6f091-231">**Scaleout metrics**</span></span>

<span data-ttu-id="6f091-232">Die folgenden Metriken messen Datenverkehr und Fehler, die durch den Datenanbieter mit horizontaler Skalierung generiert.</span><span class="sxs-lookup"><span data-stu-id="6f091-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="6f091-233">Ein **Stream** in diesem Kontext ist eine Mengen-Einheit der Anbieter mit horizontaler Skalierung verwendet; dies ist eine Tabelle aus, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird und ein Abonnement, wenn Redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6f091-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="6f091-234">Jeder Stream wird sichergestellt, geordnete Lese- und Schreibvorgänge. ein einzelner Datenstrom ist ein potenzieller Engpass für die Skalierung, damit die Anzahl der Streams erhöht werden kann, um diesen Engpass zu verringern.</span><span class="sxs-lookup"><span data-stu-id="6f091-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="6f091-235">Wenn mehrere Streams verwendet werden, wird SignalR automatisch (Shard) Nachrichten auf diese Datenströme in einer Weise zu verteilen, die sicherstellt, dass von der eine bestimmte Verbindung gesendeten Daten in der Reihenfolge sind.</span><span class="sxs-lookup"><span data-stu-id="6f091-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="6f091-236">Die [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) Einstellung steuert die Länge der Sendewarteschlange mit horizontaler Skalierung von SignalR verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="6f091-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="6f091-237">Festlegen auf einen Wert größer als 0, alle Nachrichten in einer Sendwarteschlange jeweils einzeln an die konfigurierten messagingbackplane gesendet werden abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="6f091-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="6f091-238">Wenn die Größe der Warteschlange für die konfigurierten Länge überschreitet, tritt nachfolgende Aufrufe zum Senden von wird sofort bei einer ["InvalidOperationException"](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) bis die Anzahl der Nachrichten in der Warteschlange kleiner als die Einstellung erneut.</span><span class="sxs-lookup"><span data-stu-id="6f091-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="6f091-239">Einfügen in die Warteschlange ist standardmäßig deaktiviert, da die implementierten Backplanes in der Regel ihre eigenen queuing oder flusssteuerung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6f091-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="6f091-240">Im Fall von SQL Server schränkt das Verbindungspooling effektiv die Anzahl der zu jedem Zeitpunkt statt sendet.</span><span class="sxs-lookup"><span data-stu-id="6f091-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="6f091-241">Standardmäßig nur einen Stream für SQL Server und Redis verwendet wird, werden fünf Streams für Service Bus verwendet und Einfügen in die Warteschlange ist deaktiviert, aber diese Einstellungen können geändert werden, über die Konfiguration auf SQL Server und Service Bus:</span><span class="sxs-lookup"><span data-stu-id="6f091-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="6f091-242">**.NET Server-Code für die Konfiguration der Tabelle und die Anzahl der Warteschlangenlänge für SQL Server-Rückwandplatine**</span><span class="sxs-lookup"><span data-stu-id="6f091-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="6f091-243">**.NET Server-Code für das Thema die Anzahl und der Warteschlangenlänge für Service Bus-Rückwandplatine konfigurieren**</span><span class="sxs-lookup"><span data-stu-id="6f091-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="6f091-244">Ein **Pufferung** Stream ist ein, der einem fehlerhaften Zustand befindet, wenn der Datenstrom in den Fehlerzustand ist, alle Nachrichten, die an die Backplane gesendet funktioniert erst, sofort der Stream nicht mehr fehlschlägt bzw. fehlerhaft ist.</span><span class="sxs-lookup"><span data-stu-id="6f091-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="6f091-245">Die **Länge der Sendewarteschlange** ist die Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="6f091-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="6f091-246">**Nachrichtenbus mit horizontaler Skalierung Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="6f091-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6f091-247">**Horizontale Skalierung in Streams gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="6f091-248">**Horizontale Skalierung in Streams öffnen**</span><span class="sxs-lookup"><span data-stu-id="6f091-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="6f091-249">**Horizontale Skalierung in Streams Pufferung**</span><span class="sxs-lookup"><span data-stu-id="6f091-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="6f091-250">**Horizontale Skalierung gesamt**</span><span class="sxs-lookup"><span data-stu-id="6f091-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="6f091-251">**Horizontale Skalierung/Sek.**</span><span class="sxs-lookup"><span data-stu-id="6f091-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="6f091-252">**Länge der Sendewarteschlange für horizontale Skalierung**</span><span class="sxs-lookup"><span data-stu-id="6f091-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="6f091-253">Weitere Informationen dazu, was diese Indikatoren messen, finden Sie unter [SignalR Scaleout mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="6f091-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="6f091-254">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="6f091-254">Using other performance counters</span></span>

<span data-ttu-id="6f091-255">Die folgenden Leistungsindikatoren können auch bei der Überwachung der Leistung Ihrer Anwendung nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="6f091-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="6f091-256">**Arbeitsspeicher**</span><span class="sxs-lookup"><span data-stu-id="6f091-256">**Memory**</span></span>

- <span data-ttu-id="6f091-257">.NET CLR-Speicher\\Anzahl der Bytes in allen Heaps (für w3wp)</span><span class="sxs-lookup"><span data-stu-id="6f091-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="6f091-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="6f091-258">**ASP.NET**</span></span>

- <span data-ttu-id="6f091-259">Aktuelle für "ASP.net\aktuelle Anforderungen"</span><span class="sxs-lookup"><span data-stu-id="6f091-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="6f091-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="6f091-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="6f091-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="6f091-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="6f091-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="6f091-262">**CPU**</span></span>

- <span data-ttu-id="6f091-263">Information\Processor Prozessorzeit</span><span class="sxs-lookup"><span data-stu-id="6f091-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="6f091-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="6f091-264">**TCP/IP**</span></span>

- <span data-ttu-id="6f091-265">TCPv6/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="6f091-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="6f091-266">TCPv4/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="6f091-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="6f091-267">**Webdienst**</span><span class="sxs-lookup"><span data-stu-id="6f091-267">**Web Service**</span></span>

- <span data-ttu-id="6f091-268">Web-Dienst\Aktuelle Verbindungen</span><span class="sxs-lookup"><span data-stu-id="6f091-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="6f091-269">Service\Maximum Webverbindungen</span><span class="sxs-lookup"><span data-stu-id="6f091-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="6f091-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="6f091-270">**Threading**</span></span>

- <span data-ttu-id="6f091-271">.NET CLR Sperren und Threads\\Anzahl aktuelle logische Threads</span><span class="sxs-lookup"><span data-stu-id="6f091-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="6f091-272">.NET CLR Sperren und Threads\\Anzahl aktuelle physische Threads</span><span class="sxs-lookup"><span data-stu-id="6f091-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="6f091-273">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6f091-273">Other Resources</span></span>

<span data-ttu-id="6f091-274">Weitere Informationen zu ASP.NET zur Leistungsüberwachung und-Optimierung finden Sie unter den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="6f091-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="6f091-275">[ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="6f091-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="6f091-276">ASP.NET-Thread Usage on IIS 7.5, IIS 7.0 und IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="6f091-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="6f091-277">&lt;ApplicationPool&gt; -Element (Webeinstellungen)</span><span class="sxs-lookup"><span data-stu-id="6f091-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
