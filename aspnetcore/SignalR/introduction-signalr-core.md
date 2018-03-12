---
title: "Einführung in SignalR für ASP.NET Core"
author: rachelappel
description: "Erfahren Sie, wie die SignalR für ASP.NET Core-Bibliothek vereinfacht die Echtzeit-Webfunktionen um apps hinzuzufügen."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: d4ad9bb1910a3339ac8d0d8ff740417f4e7262b7
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-signalr"></a><span data-ttu-id="2e0ea-103">Einführung in SignalR</span><span class="sxs-lookup"><span data-stu-id="2e0ea-103">Introduction to SignalR</span></span>

<span data-ttu-id="2e0ea-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="2e0ea-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="2e0ea-105">Was eine SignalR ist?</span><span class="sxs-lookup"><span data-stu-id="2e0ea-105">What is SignalR?</span></span>

<span data-ttu-id="2e0ea-106">SignalR für ASP.NET Core ist eine Bibliothek, die das Hinzufügen von Echtzeit-Webfunktionen Apps vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="2e0ea-107">Echtzeit-Webfunktionen kann serverseitigen Code Push-Inhalte für Clients sofort an.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="2e0ea-108">Gute Kandidaten für SignalR:</span><span class="sxs-lookup"><span data-stu-id="2e0ea-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="2e0ea-109">Apps, die sehr häufig Updates vom Server erfordern.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="2e0ea-110">Beispiele sind Spiele, soziale Netzwerke abstimmen, Auktion, Zuordnungen und GPS-apps.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="2e0ea-111">Dashboards und Überwachung apps.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="2e0ea-112">Beispiele umfassen Unternehmensdashboards, instant sales Updates, oder die Warnungen übertragen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="2e0ea-113">Apps für die Zusammenarbeit.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-113">Collaborative apps.</span></span> <span data-ttu-id="2e0ea-114">Whiteboard-apps und Software erfüllen Team sind Beispiele für apps für die Zusammenarbeit auf.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="2e0ea-115">Apps, die benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-115">Apps that require notifications.</span></span> <span data-ttu-id="2e0ea-116">Verwenden Benachrichtigungen, soziale Netzwerke, e-Mail, Chat, Spiele, Reisen Warnungen und viele andere apps.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="2e0ea-117">SignalR stellt eine API zum Erstellen von Server-zu-Client [von Remoteprozeduraufrufen (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="2e0ea-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="2e0ea-118">Die RPCs rufen Sie JavaScript-Funktionen auf Clients aus serverseitigem .NET Core-Code.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="2e0ea-119">SignalR für ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2e0ea-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="2e0ea-120">Verbindungsverwaltung behandelt automatisch.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="2e0ea-121">Ermöglicht das Senden von Nachrichten für alle verbundenen Clients gleichzeitig über.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="2e0ea-122">Z. B. einem Chatraum.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-122">For example, a chat room.</span></span>
* <span data-ttu-id="2e0ea-123">Ermöglicht das Senden von Nachrichten an bestimmte Clients oder Clientgruppen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="2e0ea-124">Wird auf Open Source [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="2e0ea-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="2e0ea-125">Gut skaliert werden.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-125">Scales nicely.</span></span>

<span data-ttu-id="2e0ea-126">Die Verbindung zwischen Client und Server ist persistent, im Gegensatz zu einer HTTP-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="2e0ea-127">Transportprotokolle</span><span class="sxs-lookup"><span data-stu-id="2e0ea-127">Transports</span></span>

<span data-ttu-id="2e0ea-128">SignalR abstrahiert über eine Reihe von Techniken zum Erstellen von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="2e0ea-129">[WebSockets](https://tools.ietf.org/html/rfc7118) ist der optimale Transport, aber andere Techniken wie Server-Sent Ereignisse und lange Abfragen können verwendet werden, wenn diese nicht zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="2e0ea-130">SignalR automatisch erkennt, und initialisieren den entsprechenden Transport basierend auf den Funktionen, die auf dem Client und Server unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="2e0ea-131">Hubs und Endpunkte</span><span class="sxs-lookup"><span data-stu-id="2e0ea-131">Hubs and Endpoints</span></span>

<span data-ttu-id="2e0ea-132">SignalR verwendet Hubs und Endpunkte für die Kommunikation zwischen Clients und Servern.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="2e0ea-133">Die Hubs-API werden die meisten Szenarien behandelt.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="2e0ea-134">Ein Hub ist eine allgemeine Pipeline, die auf die Endpunkt-API, die der Client-als auch zum Aufrufen von Methoden voneinander ermöglicht basiert.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="2e0ea-135">SignalR übernimmt die Verteilung über Computergrenzen hinweg automatisch, sodass Clients zum Aufrufen von Methoden auf dem Server als einfach als lokalen Methoden (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="2e0ea-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="2e0ea-136">Hubs ermöglichen übergeben von Parametern stark typisierte Methoden, wodurch wurden die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="2e0ea-137">SignalR enthält zwei integrierte Hub-Protokolle: ein Text-Protokoll, anhand von JSON und einer binären Protokolls basierend auf [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="2e0ea-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="2e0ea-138">MessagePack erstellt in der Regel über kleinere Nachrichten als bei der Verwendung von JSON.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="2e0ea-139">Ältere Browser unterstützen müssen [XHR Ebene 2](https://caniuse.com/#feat=xhr2) MessagePack-Protokoll unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="2e0ea-140">Senden von Nachrichten mithilfe der aktive Transport aufrufen Hubs clientseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="2e0ea-141">Die Nachrichten enthalten den Namen und die Parameter der Methode die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="2e0ea-142">Objekte, die als Methodenparameter gesendet werden mit dem konfigurierten Protokoll deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="2e0ea-143">Der Client versucht, mit dem Namen einer Methode im clientseitigen Code übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="2e0ea-144">Wenn eine Übereinstimmung vorliegt, führt die Methode verwendet die deserialisierte Parameterdaten.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="2e0ea-145">Endpunkte ermöglichen raw-Socket-ähnliche API, aktivieren sie zum Lesen und schreiben, die vom Client an.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="2e0ea-146">Es liegt an der Entwickler, Gruppierung, senden und andere Funktionen zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="2e0ea-147">Die Hubs-API baut auf der Ebene der Endpunkte.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="2e0ea-148">Das folgende Diagramm zeigt die Beziehung zwischen Hubs, Endpunkte und Clients.</span><span class="sxs-lookup"><span data-stu-id="2e0ea-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![SignalR-Karte](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="2e0ea-150">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="2e0ea-150">Related resources</span></span>

[<span data-ttu-id="2e0ea-151">Erste Schritte mit SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e0ea-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started-signalr-core)
