---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana-Beispiele | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393318"
---
<a name="katana-samples"></a><span data-ttu-id="457ee-102">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="457ee-102">Katana Samples</span></span>
====================
<span data-ttu-id="457ee-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="457ee-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="457ee-104">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="457ee-104">Katana Samples</span></span>

<span data-ttu-id="457ee-105">**ASP.NET leitet Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="457ee-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="457ee-106">In einigen Anwendungen möchten Sie die OWIN-Komponenten in der Routentabelle für Asp.Net parallel mit nicht-OWIN-Komponenten einbinden.</span><span class="sxs-lookup"><span data-stu-id="457ee-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="457ee-107">Dieses Beispiel zeigt, wie Sie mit die RouteCollection-Erweiterungsmethoden MapOwinPath und MapOwinRoute von "Microsoft.owin.Host.systemweb" bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="457ee-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="457ee-108">**Beispiel-Pipelines Verzweigungen** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="457ee-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="457ee-109">OWIN-Anforderung Verarbeitungspipelines müssen nicht linear sein, diese zum Verarbeiten von Anforderungen auf unterschiedliche Weise gebrancht werden können.</span><span class="sxs-lookup"><span data-stu-id="457ee-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="457ee-110">Dieses Beispiel zeigt, wie Sie eine verzweigte Pipeline basierend auf anforderungspfade oder andere Anforderungsdaten wie Header zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="457ee-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="457ee-111">Diese Komponenten sind im Microsoft.Owin.Mapping Nuget-Paket verfügbar.</span><span class="sxs-lookup"><span data-stu-id="457ee-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="457ee-112">**Benutzerdefinierte Server-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="457ee-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="457ee-113">Zeigt, wie Sie einen benutzerdefinierten OWIN-Server verwenden, wenn Sie Selbsthosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="457ee-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="457ee-114">**Eingebettete Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="457ee-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="457ee-115">Einige OWIN-Server in Ihrem eigenen Prozess ausgeführt werden können (&quot;selbstgehostet&quot;).</span><span class="sxs-lookup"><span data-stu-id="457ee-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="457ee-116">Dieses Beispiel zeigt, wie Sie zum Starten einer OWIN-Anwendung, die mit den Tools, die von der Microsoft.Owin.Hosting-Nuget-Paket bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="457ee-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="457ee-117">**HelloWorld-Beispiels** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="457ee-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="457ee-118">OWIN ist ein HTTP-Server-API-Abstraktion, das auf verschiedenen Servern Anwendungsportabilität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="457ee-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="457ee-119">Dieses Beispiel veranschaulicht das Schreiben eine Hello World-Anwendung mit einigen **einfache Wrapper** rund um den unformatierten OWIN-Abstraktion, und führen sie auf einem Webserver, z.B. ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="457ee-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="457ee-120">**Hello World-Rohdaten OWIN-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="457ee-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="457ee-121">In diesem Beispiel wird veranschaulicht, wie zum Schreiben einer Hello World-Anwendung mithilfe der **unformatierten** OWIN-Abstraktion, und führen Sie es auf einem Webserver wie Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="457ee-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="457ee-122">**SignalR-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="457ee-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="457ee-123">Zeigt, wie selfhosten von SignalR mit OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="457ee-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="457ee-124">Weitere Informationen zum Self-hosting SignalR finden Sie unter [Tutorial: Selfhosten von SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="457ee-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="457ee-125">**Beispiel für statische Dateien** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="457ee-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="457ee-126">Zeigt, wie zur Unterstützung von HTTP-Anforderungen für statische Dateien, die mithilfe von OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="457ee-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="457ee-127">**Web-API** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="457ee-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="457ee-128">Dieses Beispiel zeigt, wie OWIN in IIS zu hosten und Web-API für die OWIN-Pipeline hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="457ee-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="457ee-129">**Web-Socket-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="457ee-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="457ee-130">Zeigt, wie Web Sockets in OWIN mithilfe von unterstützen die [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) Klasse.</span><span class="sxs-lookup"><span data-stu-id="457ee-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
