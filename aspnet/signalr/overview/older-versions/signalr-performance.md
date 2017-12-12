---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR-Leistung (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: SignalR-Leistung
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 52052ad202958eb5d648ceb64d9f06fb86ef3777
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="f7371-103">SignalR-Leistung (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f7371-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f7371-104">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f7371-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f7371-105">Dieses Thema beschreibt, wie für entwerfen, messen und Verbessern der Leistung in einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f7371-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="f7371-106">Eine aktuelle Präsentation auf SignalR-Leistung und Skalierung finden Sie unter [im Web in Echtzeit mit ASP.NET SignalR-Skalierung](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="f7371-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="f7371-107">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="f7371-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f7371-108">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="f7371-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="f7371-109">Optimieren des SignalR-Servers für die Leistung</span><span class="sxs-lookup"><span data-stu-id="f7371-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="f7371-110">Problembehandlung bei Performanceproblemen</span><span class="sxs-lookup"><span data-stu-id="f7371-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="f7371-111">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="f7371-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="f7371-112">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="f7371-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="f7371-113">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f7371-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="f7371-114">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="f7371-114">Design considerations</span></span>

<span data-ttu-id="f7371-115">Dieser Abschnitt beschreibt die Muster, die während des Entwurfs einer SignalR-Anwendung, um sicherzustellen, dass die Leistung nicht beeinträchtigt werden wird, generieren Sie unnötigen Netzwerkdatenverkehr implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="f7371-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="f7371-116">Einschränkung der Häufigkeit der Nachricht</span><span class="sxs-lookup"><span data-stu-id="f7371-116">Throttling message frequency</span></span>

<span data-ttu-id="f7371-117">Die meisten Anwendungen müssen nicht auch in eine Anwendung, die eine hohe Frequenz (z. B. eine Anwendung für den Echtzeit-Spiele zulassen) Nachrichten sendet, mehrere Nachrichten ein zweites zu senden.</span><span class="sxs-lookup"><span data-stu-id="f7371-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="f7371-118">Um den Umfang des Datenverkehrs zu reduzieren, die jedem Client generiert, eine Nachrichtenschleife, Warteschlangen und sendet Nachrichten nicht mehr als einer festen Rate häufig implementiert werden kann (d. h. bis zu einer bestimmten Anzahl von Nachrichten wird gesendet, pro Sekunde, wenn in diesem Zeitraum in Nachrichten vorhanden sind Terval gesendet werden).</span><span class="sxs-lookup"><span data-stu-id="f7371-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="f7371-119">Eine beispielanwendung, die Nachrichten zu einer bestimmten Rate (von Client und Server) schränkt, finden Sie unter [hochfrequente Realtime mit SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f7371-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="f7371-120">Verringern der Größe</span><span class="sxs-lookup"><span data-stu-id="f7371-120">Reducing message size</span></span>

<span data-ttu-id="f7371-121">Sie können die Größe einer SignalR-Nachricht durch Reduzierung der Größe der serialisierten Objekte reduzieren.</span><span class="sxs-lookup"><span data-stu-id="f7371-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="f7371-122">Im Servercode, wenn Sie ein Objekt gesendet, die Eigenschaften, die nicht enthält übertragen werden müssen, verhindern, dass diese Eigenschaften serialisiert wird, mithilfe der `JsonIgnore` Attribut.</span><span class="sxs-lookup"><span data-stu-id="f7371-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="f7371-123">Die Namen von Eigenschaften werden auch in der Nachricht gespeichert. die Namen von Eigenschaften über verkürzt die `JsonProperty` Attribut.</span><span class="sxs-lookup"><span data-stu-id="f7371-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="f7371-124">Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft ausschließen, die an den Client gesendet werden, und wie Eigenschaftennamen zu verkürzen:</span><span class="sxs-lookup"><span data-stu-id="f7371-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="f7371-125">**.NET Servercode, die das JsonIgnore-Attribut, um Daten auszuschließen, die an den Client gesendet werden und JsonProperty Attribut zum Reduzieren der Größe der Nachricht veranschaulicht**</span><span class="sxs-lookup"><span data-stu-id="f7371-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="f7371-126">Um die Lesbarkeit beibehalten / bessere Verwaltbarkeit im Clientcode die abgekürzten Namen kann neu zugeordnete auf einem von Menschen benutzerfreundliche Namen nach dem Empfang der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="f7371-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="f7371-127">Das folgende Codebeispiel veranschaulicht eine Möglichkeit der neuzuordnung verkürzter Namen länger Vorgängen, durch die Definition eines Nachrichtenvertrags (Zuordnung), und Verwenden der `reMap` Funktion, die den Vertrag der optimierte Nachrichtenklasse angewendet:</span><span class="sxs-lookup"><span data-stu-id="f7371-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="f7371-128">**Ordnet die clientseitige-JavaScript-Code verkürzt Eigenschaftennamen, die einem von Menschen lesbaren Namen**</span><span class="sxs-lookup"><span data-stu-id="f7371-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="f7371-129">Namen können in Nachrichten vom Client an den Server auch verkürzt werden mit derselben Methode.</span><span class="sxs-lookup"><span data-stu-id="f7371-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="f7371-130">Reduzierung des Platzbedarfs Speicher (d. h. die Menge des Arbeitsspeichers, die für die Nachricht verwendet) der Nachricht Objekt kann auch die Leistung verbessern.</span><span class="sxs-lookup"><span data-stu-id="f7371-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="f7371-131">Angenommen, wenn das gesamte Spektrum der ein `int` ist nicht erforderlich, eine `short` oder `byte` kann stattdessen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7371-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="f7371-132">Da Nachrichten im Nachrichtenbus im Arbeitsspeicher des Servers, reduzieren die Größe der Nachrichten gespeichert werden können Sie auch Adresse Server von Arbeitsspeicherproblemen.</span><span class="sxs-lookup"><span data-stu-id="f7371-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="f7371-133">Optimieren des SignalR-Servers für die Leistung</span><span class="sxs-lookup"><span data-stu-id="f7371-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="f7371-134">Die folgenden Konfigurationseinstellungen können verwendet werden, um den Server für eine bessere Leistung in einer SignalR-Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="f7371-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="f7371-135">Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter [verbessern die Leistung von ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7371-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="f7371-136">**SignalR-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="f7371-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="f7371-137">**DefaultMessageBufferSize**: Standardmäßig werden SignalR 1000 Nachrichten im Arbeitsspeicher pro Hub pro Verbindung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="f7371-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="f7371-138">Wenn große Nachrichten verwendet werden, kann dies Speicherprobleme zu erstellen, die behoben werden können, durch Verringern dieses Werts.</span><span class="sxs-lookup"><span data-stu-id="f7371-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="f7371-139">Diese Einstellung kann festgelegt werden, der `Application_Start` -Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration` Methode einer OWIN-Start-Klasse in einer selbst gehosteten Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f7371-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="f7371-140">Im folgende Beispiel wird veranschaulicht, wie Sie diesen Wert verringern, um Ihrer Anwendung, um die Menge des belegten Server zu reduzieren den Speicherbedarf zu reduzieren:</span><span class="sxs-lookup"><span data-stu-id="f7371-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="f7371-141">**.NET Servercode in Global.asax zum Verringern der Größe des Nachrichtenpuffers Standard**</span><span class="sxs-lookup"><span data-stu-id="f7371-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="f7371-142">**IIS-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="f7371-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="f7371-143">**Max. Anzahl gleichzeitiger Anforderungen pro Anwendung**: die Anzahl der gleichzeitigen IIS Anforderungen steigt auch die Serverressourcen verfügbar sind, Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="f7371-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="f7371-144">Der Standardwert ist 5000. Um diese Einstellung erhöhen möchten, führen Sie die folgenden Befehle in einem Eingabeaufforderungsfenster mit erhöhten Rechten aus:</span><span class="sxs-lookup"><span data-stu-id="f7371-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="f7371-145">**Konfiguration der ASP.NET-Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="f7371-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="f7371-146">Dieser Abschnitt enthält Konfigurationseinstellungen, die festgelegt werden können, in der `aspnet.config` Datei.</span><span class="sxs-lookup"><span data-stu-id="f7371-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="f7371-147">Diese Datei befindet sich in einem von zwei Speicherorten, je nach Plattform:</span><span class="sxs-lookup"><span data-stu-id="f7371-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="f7371-148">Die folgenden: ASP.NET-Einstellungen, die SignalR-Leistung verbessern können</span><span class="sxs-lookup"><span data-stu-id="f7371-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="f7371-149">**Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: erhöhen diese Einstellung möglicherweise Leistungsengpässe verringern.</span><span class="sxs-lookup"><span data-stu-id="f7371-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="f7371-150">Um diese Einstellung erhöhen möchten, fügen Sie die folgende Konfigurationseinstellung der `aspnet.config` Datei:</span><span class="sxs-lookup"><span data-stu-id="f7371-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="f7371-151">**Anfordern der Begrenzung der Warteschlange**: Wenn die Gesamtzahl der Verbindungen überschreitet die `maxConcurrentRequestsPerCPU` festlegen, ASP.NET startet Einschränkung Anforderungen, die über eine Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="f7371-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="f7371-152">Um die Größe der Warteschlange zu erhöhen, erhöhen Sie die `requestQueueLimit` Einstellung.</span><span class="sxs-lookup"><span data-stu-id="f7371-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="f7371-153">Fügen Sie hierzu die folgende Konfigurationseinstellung der `processModel` Knoten `config/machine.config` (statt `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="f7371-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="f7371-154">Problembehandlung bei Performanceproblemen</span><span class="sxs-lookup"><span data-stu-id="f7371-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="f7371-155">Dieser Abschnitt beschreibt die Möglichkeiten, um Leistungsengpässe in der Anwendung zu finden.</span><span class="sxs-lookup"><span data-stu-id="f7371-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="f7371-156">Überprüfen, dass ein WebSocket verwendet wird</span><span class="sxs-lookup"><span data-stu-id="f7371-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="f7371-157">Obgleich SignalR eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, WebSocket bietet einen deutlichen Leistungsvorteil und sollte verwendet werden, wenn Client und Server unterstützen.</span><span class="sxs-lookup"><span data-stu-id="f7371-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="f7371-158">Um festzustellen, ob der Client-als auch die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f7371-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="f7371-159">Um zu bestimmen, welcher Transport in Ihrer Anwendung verwendet wird, können Sie die Browser-Entwicklertools verwenden, und überprüfen die Protokolle, um festzustellen, welcher Transport für die Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f7371-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="f7371-160">Informationen zur Verwendung der Browser-Entwicklungstools in Internet Explorer und Chrome finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f7371-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="f7371-161">Verwenden von SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="f7371-161">Using SignalR performance counters</span></span>

<span data-ttu-id="f7371-162">In diesem Abschnitt wird beschrieben, wie aktivieren und Verwenden von SignalR-Leistungsindikatoren finden der `Microsoft.AspNet.SignalR.Utils` Paket.</span><span class="sxs-lookup"><span data-stu-id="f7371-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="f7371-163">Installieren von signalr.exe</span><span class="sxs-lookup"><span data-stu-id="f7371-163">Installing signalr.exe</span></span>

<span data-ttu-id="f7371-164">Leistung-Leistungsindikatoren können mit dem Server mithilfe eines Dienstprogramms namens SignalR.exe hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="f7371-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="f7371-165">Um dieses Programm zu installieren, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="f7371-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="f7371-166">Wählen Sie in der Visual Studio-Anwendung **Tools**, **Bibliothekspaket-Manager**, **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="f7371-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="f7371-167">Suchen Sie nach **signalr.utils**, und wählen Sie installieren.</span><span class="sxs-lookup"><span data-stu-id="f7371-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="f7371-168">Stimmen Sie dem Lizenzvertrag zum Installieren des Pakets an.</span><span class="sxs-lookup"><span data-stu-id="f7371-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="f7371-169">SignalR.exe installiert werden soll `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="f7371-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="f7371-170">Installieren von Leistungsindikatoren mit SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="f7371-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="f7371-171">Führen Sie zum Installieren von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="f7371-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="f7371-172">Führen Sie zum Entfernen von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="f7371-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="f7371-173">SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="f7371-173">SignalR Performance counters</span></span>

<span data-ttu-id="f7371-174">Das Dienstprogramme installiert der folgenden Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="f7371-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="f7371-175">Die Leistungsindikatoren "Total" messen die Anzahl der Ereignisse an, seit der letzten Anwendungspool oder den Server neu starten.</span><span class="sxs-lookup"><span data-stu-id="f7371-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="f7371-176">**Verbindung Metriken**</span><span class="sxs-lookup"><span data-stu-id="f7371-176">**Connection metrics**</span></span>

<span data-ttu-id="f7371-177">Die folgenden Metriken messen die Lebensdauer-Verbindungsereignisse, die auftreten.</span><span class="sxs-lookup"><span data-stu-id="f7371-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="f7371-178">Weitere Informationen finden Sie unter [verstehen und Behandeln von Verbindungsereignisse Lebensdauer](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f7371-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="f7371-179">**Verbindungen verbunden**</span><span class="sxs-lookup"><span data-stu-id="f7371-179">**Connections Connected**</span></span>
- <span data-ttu-id="f7371-180">**Verbindungen, die die Verbindung wiederhergestellt**</span><span class="sxs-lookup"><span data-stu-id="f7371-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="f7371-181">**Verbindungen Disonnected**</span><span class="sxs-lookup"><span data-stu-id="f7371-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="f7371-182">**Aktuelle Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="f7371-182">**Connections Current**</span></span>

<span data-ttu-id="f7371-183">**Nachrichtenmetrik**</span><span class="sxs-lookup"><span data-stu-id="f7371-183">**Message metrics**</span></span>

<span data-ttu-id="f7371-184">Die folgenden Metriken messen dem Volumen des Nachrichtenverkehrs von SignalR generiert.</span><span class="sxs-lookup"><span data-stu-id="f7371-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="f7371-185">**Gesamtanzahl der empfangenen Nachrichten Verbindung**</span><span class="sxs-lookup"><span data-stu-id="f7371-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="f7371-186">**Gesamtanzahl der gesendeten Nachrichten Verbindung**</span><span class="sxs-lookup"><span data-stu-id="f7371-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="f7371-187">**Verbindung empfangene Nachrichten/s**</span><span class="sxs-lookup"><span data-stu-id="f7371-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="f7371-188">**Verbindung gesendete Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="f7371-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="f7371-189">**Bus Nachrichtenmetrik**</span><span class="sxs-lookup"><span data-stu-id="f7371-189">**Message bus metrics**</span></span>

<span data-ttu-id="f7371-190">Die folgenden Metriken messen Datenverkehr über den internen SignalR-Nachrichtenbus der Warteschlange, in der alle eingehende und ausgehende SignalR-Nachrichten eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="f7371-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="f7371-191">Eine Nachricht ist **veröffentlichte** wann sie gesendet oder übertragen.</span><span class="sxs-lookup"><span data-stu-id="f7371-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="f7371-192">Ein **Abonnenten** in diesem Kontext wird ein Abonnement für den Nachrichtenbus; Dies sollte die Anzahl der Clients und dem Server selbst entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f7371-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="f7371-193">Ein **reservierten Worker** ist eine Komponente, die Daten an die aktiven Verbindungen, sendet eine **ausgelasteten Worker** ist eine, die aktiv eine Nachricht sendet.</span><span class="sxs-lookup"><span data-stu-id="f7371-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="f7371-194">**Nachrichtenbus Nachrichten empfangene gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="f7371-195">**Nachrichtenbus Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="f7371-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f7371-196">**Nachricht Bus Nachrichten veröffentlicht gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="f7371-197">**Nachrichtenbus Nachrichten veröffentlicht/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="f7371-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="f7371-198">**Nachricht Bus Abonnenten aktuelle**</span><span class="sxs-lookup"><span data-stu-id="f7371-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="f7371-199">**Bus Abonnenten gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="f7371-200">**Nachricht Bus Abonnenten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="f7371-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="f7371-201">**Nachrichtenbus reservierten Worker**</span><span class="sxs-lookup"><span data-stu-id="f7371-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="f7371-202">**Nachricht Bus ausgelasteter Worker**</span><span class="sxs-lookup"><span data-stu-id="f7371-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="f7371-203">**Nachricht Bus Themen aktuelle**</span><span class="sxs-lookup"><span data-stu-id="f7371-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="f7371-204">**Fehler-Metriken**</span><span class="sxs-lookup"><span data-stu-id="f7371-204">**Error metrics**</span></span>

<span data-ttu-id="f7371-205">Die folgenden Metriken Messen von SignalR Volumen des Nachrichtenverkehrs generierten Fehler.</span><span class="sxs-lookup"><span data-stu-id="f7371-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="f7371-206">**Hub-Auflösung** Fehler auftreten, wenn Sie einen Hub oder hubmethode nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="f7371-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="f7371-207">**Hubaufruf** Fehler sind beim Aufrufen einer hubmethode ausgelösten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="f7371-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="f7371-208">**Transport** Fehler sind Verbindungsfehler während einer HTTP-Anforderung oder Antwort ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="f7371-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="f7371-209">**Fehler: Alle gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-209">**Errors: All Total**</span></span>
- <span data-ttu-id="f7371-210">**Fehler: Alle/Sek.**</span><span class="sxs-lookup"><span data-stu-id="f7371-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="f7371-211">**Fehler: Gesamtanzahl der Hub-Lösung**</span><span class="sxs-lookup"><span data-stu-id="f7371-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="f7371-212">**Fehler: Hub Auflösung/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="f7371-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="f7371-213">**Fehler: Gesamtanzahl der Hub-Aufruf**</span><span class="sxs-lookup"><span data-stu-id="f7371-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="f7371-214">**Fehler: Hub Aufruf/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="f7371-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="f7371-215">**Fehler: Transport gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="f7371-216">**Fehler: Transport/Sek.**</span><span class="sxs-lookup"><span data-stu-id="f7371-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="f7371-217">**Metriken mit horizontaler Skalierung**</span><span class="sxs-lookup"><span data-stu-id="f7371-217">**Scaleout metrics**</span></span>

<span data-ttu-id="f7371-218">Die folgenden Metriken Measuregruppen Datenverkehr und Fehler, die vom Anbieter mit horizontaler Skalierung.</span><span class="sxs-lookup"><span data-stu-id="f7371-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="f7371-219">Ein **Stream** in diesem Kontext ist eine Skalierungseinheit vom Anbieter mit horizontaler Skalierung verwendet; dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird und ein Abonnement, wenn Redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f7371-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="f7371-220">Standardmäßig wird nur ein Datenstrom verwendet, aber dies kann über die Konfiguration für SQL Server und Service Bus erhöht werden.</span><span class="sxs-lookup"><span data-stu-id="f7371-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="f7371-221">Ein **Pufferung** ist eine, die einen Fehlerzustand eingegeben hat; wird der Stream in den Fehlerzustand, alle Nachrichten, die an die Backplane gesendet werden erst, wenn sofort nicht mehr Fault.</span><span class="sxs-lookup"><span data-stu-id="f7371-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="f7371-222">Die **Länge der Sendewarteschlange** ist die Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="f7371-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="f7371-223">**Nachrichtenbus mit horizontaler Skalierung Nachrichten empfangen/Sekunde**</span><span class="sxs-lookup"><span data-stu-id="f7371-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f7371-224">**Warteschlangen für horizontale Skalierung Streams gesamt**</span><span class="sxs-lookup"><span data-stu-id="f7371-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="f7371-225">**Warteschlangen für horizontale Skalierung streamt öffnen**</span><span class="sxs-lookup"><span data-stu-id="f7371-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="f7371-226">**Warteschlangen für horizontale Skalierung Streams Pufferung**</span><span class="sxs-lookup"><span data-stu-id="f7371-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="f7371-227">**Gesamtanzahl der Fehler mit horizontaler Skalierung**</span><span class="sxs-lookup"><span data-stu-id="f7371-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="f7371-228">**Warteschlangen für horizontale Skalierung/Sek.**</span><span class="sxs-lookup"><span data-stu-id="f7371-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="f7371-229">**Länge der Sendewarteschlange für horizontale Skalierung**</span><span class="sxs-lookup"><span data-stu-id="f7371-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="f7371-230">Weitere Informationen, was diese Indikatoren gemessen werden, finden Sie unter [SignalR Scaleout mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="f7371-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="f7371-231">Verwenden die übrigen Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="f7371-231">Using other performance counters</span></span>

<span data-ttu-id="f7371-232">Die folgenden Leistungsindikatoren möglicherweise auch bei der Überwachung der Leistung Ihrer Anwendung nützlich.</span><span class="sxs-lookup"><span data-stu-id="f7371-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="f7371-233">**Arbeitsspeicher**</span><span class="sxs-lookup"><span data-stu-id="f7371-233">**Memory**</span></span>

- <span data-ttu-id="f7371-234">.NET CLR Memory-Bytes in allen Heaps (für w3wp)</span><span class="sxs-lookup"><span data-stu-id="f7371-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="f7371-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f7371-235">**ASP.NET**</span></span>

- <span data-ttu-id="f7371-236">Aktuelle "ASP.net\aktuelle Anforderungen"</span><span class="sxs-lookup"><span data-stu-id="f7371-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="f7371-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="f7371-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="f7371-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="f7371-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="f7371-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="f7371-239">**CPU**</span></span>

- <span data-ttu-id="f7371-240">Information\Processor Prozessorzeit</span><span class="sxs-lookup"><span data-stu-id="f7371-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="f7371-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="f7371-241">**TCP/IP**</span></span>

- <span data-ttu-id="f7371-242">TCPv6/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="f7371-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="f7371-243">TCPv4/hergestellten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="f7371-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="f7371-244">**Webdienst**</span><span class="sxs-lookup"><span data-stu-id="f7371-244">**Web Service**</span></span>

- <span data-ttu-id="f7371-245">Web-Dienst\Aktuelle Verbindungen</span><span class="sxs-lookup"><span data-stu-id="f7371-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="f7371-246">Service\Maximum Webverbindungen</span><span class="sxs-lookup"><span data-stu-id="f7371-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="f7371-247">**Threading**</span><span class="sxs-lookup"><span data-stu-id="f7371-247">**Threading**</span></span>

- <span data-ttu-id="f7371-248">.NET CLR-Sperren\# aktuelle logische Threads</span><span class="sxs-lookup"><span data-stu-id="f7371-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="f7371-249">.NET CLR LocksAnd Threads\# aktuelle physische Threads</span><span class="sxs-lookup"><span data-stu-id="f7371-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="f7371-250">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f7371-250">Other Resources</span></span>

<span data-ttu-id="f7371-251">Weitere Informationen zu ASP.NET-Leistung überwachen und optimieren finden Sie unter den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="f7371-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="f7371-252">[ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="f7371-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="f7371-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0 und IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="f7371-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="f7371-254">&lt;ApplicationPool&gt; Element (Webeinstellungen)</span><span class="sxs-lookup"><span data-stu-id="f7371-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
