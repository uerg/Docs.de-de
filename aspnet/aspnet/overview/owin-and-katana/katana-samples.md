---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana-Beispiele | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a>Katana-Beispiele
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana-Beispiele

**ASP.NET leitet Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
In einigen Anwendungen sollten Sie die owin-Komponenten in der Routingtabelle Asp.Net parallel mit nicht-owin-Komponenten einbinden. In diesem Beispiel wird gezeigt, wie die RouteCollection-Erweiterungsmethoden MapOwinPath und MapOwinRoute gebotenen "Microsoft.owin.Host.systemweb" verwendet wird.

**Verzweigen von Pipelines Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Owin-Anforderung Verarbeitungspipelines müssen nicht lineare sein, sie verzweigt werden können, um Anforderungen auf unterschiedliche Weise verarbeiten. In diesem Beispiel wird gezeigt, wie erstellt anforderungspfade oder andere Anforderungsdaten wie Header entsprechend eine Verzweigung Pipeline. Diese Komponenten sind im Microsoft.Owin.Mapping NuGet-Paket verfügbar.

**Benutzerdefinierten Server-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Zeigt, wie einen benutzerdefinierten OWIN-Server beim Selbsthosting OWIN.

**Eingebettete Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Einige OWIN-Server innerhalb des eigenen Prozesses ausgeführt werden können (&quot;selbstgehosteten&quot;). In diesem Beispiel wird gezeigt, wie zum Starten einer OWIN-Anwendung, die mit den Tools, die von der Nuget-Paket "Microsoft.owin.Hosting" bereitgestellt wird.

**HelloWorld-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN ist ein HTTP-Server-API-Abstraktionsschicht bereit, die auf verschiedenen Servern Anwendungsportabilität ermöglicht. Dieses Beispiel veranschaulicht das Schreiben eine Hello World-Anwendung mithilfe einiger **einfache Wrapper** um die unformatierten OWIN-Abstraktion und führen Sie es auf einem Webserver wie z. B. ASP.NET.

**Hello World unformatierten OWIN-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Dieses Beispiel veranschaulicht das Schreiben einer Hello World-Anwendung mithilfe der **unformatierten** OWIN-Abstraktion und führen Sie es auf einem Webserver wie Asp.Net.

**SignalR-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Zeigt, wie mithilfe von OWIN SignalR Selbsthosting / Katana. Weitere Informationen zu Selbsthosting SignalR, finden Sie unter [Lernprogramm: SignalR Selbsthosting](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statische Dateien Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Zeigt, wie zur Unterstützung von HTTP-Anforderungen für statische Dateien, die mithilfe von OWIN / Katana.

**Web-API** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Dieses Beispiel zeigt, wie eine OWIN in IIS hosten, und die OWIN-Pipeline Web-API hinzuzufügen.

**Web-Socket-Beispiel** | [Quellcode](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Zeigt, wie mithilfe von WebSockets in OWIN unterstützen die [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) Klasse.
