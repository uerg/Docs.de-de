---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana-Beispiele | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034489"
---
<a name="katana-samples"></a><span data-ttu-id="ad4a1-102">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="ad4a1-102">Katana Samples</span></span>
====================
<span data-ttu-id="ad4a1-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="ad4a1-104">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="ad4a1-104">Katana Samples</span></span>

<span data-ttu-id="ad4a1-105">**ASP.NET leitet Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="ad4a1-106">In einigen Anwendungen sollten Sie die owin-Komponenten in der Routingtabelle Asp.Net parallel mit nicht-owin-Komponenten einbinden.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="ad4a1-107">In diesem Beispiel wird gezeigt, wie die RouteCollection-Erweiterungsmethoden MapOwinPath und MapOwinRoute gebotenen "Microsoft.owin.Host.systemweb" verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="ad4a1-108">**Verzweigen von Pipelines Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="ad4a1-109">Owin-Anforderung Verarbeitungspipelines müssen nicht lineare sein, sie verzweigt werden können, um Anforderungen auf unterschiedliche Weise verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="ad4a1-110">In diesem Beispiel wird gezeigt, wie erstellt anforderungspfade oder andere Anforderungsdaten wie Header entsprechend eine Verzweigung Pipeline.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="ad4a1-111">Diese Komponenten sind im Microsoft.Owin.Mapping NuGet-Paket verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="ad4a1-112">**Benutzerdefinierten Server-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="ad4a1-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="ad4a1-113">Zeigt, wie einen benutzerdefinierten OWIN-Server beim Selbsthosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="ad4a1-114">**Eingebettete Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="ad4a1-115">Einige OWIN-Server innerhalb des eigenen Prozesses ausgeführt werden können (&quot;selbstgehosteten&quot;).</span><span class="sxs-lookup"><span data-stu-id="ad4a1-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="ad4a1-116">In diesem Beispiel wird gezeigt, wie zum Starten einer OWIN-Anwendung, die mit den Tools, die von der Nuget-Paket "Microsoft.owin.Hosting" bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="ad4a1-117">**HelloWorld-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="ad4a1-118">OWIN ist ein HTTP-Server-API-Abstraktionsschicht bereit, die auf verschiedenen Servern Anwendungsportabilität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="ad4a1-119">Dieses Beispiel veranschaulicht das Schreiben eine Hello World-Anwendung mithilfe einiger **einfache Wrapper** um die unformatierten OWIN-Abstraktion und führen Sie es auf einem Webserver wie z. B. ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="ad4a1-120">**Hello World unformatierten OWIN-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="ad4a1-121">Dieses Beispiel veranschaulicht das Schreiben einer Hello World-Anwendung mithilfe der **unformatierten** OWIN-Abstraktion und führen Sie es auf einem Webserver wie Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="ad4a1-122">**SignalR-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="ad4a1-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="ad4a1-123">Zeigt, wie mithilfe von OWIN SignalR Selbsthosting / Katana.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="ad4a1-124">Weitere Informationen zu Selbsthosting SignalR, finden Sie unter [Lernprogramm: SignalR Selbsthosting](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="ad4a1-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="ad4a1-125">**Statische Dateien Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="ad4a1-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="ad4a1-126">Zeigt, wie zur Unterstützung von HTTP-Anforderungen für statische Dateien, die mithilfe von OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="ad4a1-127">**Web-API** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="ad4a1-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="ad4a1-128">Dieses Beispiel zeigt, wie eine OWIN in IIS hosten, und die OWIN-Pipeline Web-API hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="ad4a1-129">**Web-Socket-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="ad4a1-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="ad4a1-130">Zeigt, wie mithilfe von WebSockets in OWIN unterstützen die [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) Klasse.</span><span class="sxs-lookup"><span data-stu-id="ad4a1-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
