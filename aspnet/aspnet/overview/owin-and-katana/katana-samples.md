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
<a name="katana-samples"></a>Katana-Beispiele
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana-Beispiele

**ASP.NET leitet Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
In einigen Anwendungen möchten Sie die OWIN-Komponenten in der Routentabelle für Asp.Net parallel mit nicht-OWIN-Komponenten einbinden. Dieses Beispiel zeigt, wie Sie mit die RouteCollection-Erweiterungsmethoden MapOwinPath und MapOwinRoute von "Microsoft.owin.Host.systemweb" bereitgestellt wird.

**Beispiel-Pipelines Verzweigungen** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN-Anforderung Verarbeitungspipelines müssen nicht linear sein, diese zum Verarbeiten von Anforderungen auf unterschiedliche Weise gebrancht werden können. Dieses Beispiel zeigt, wie Sie eine verzweigte Pipeline basierend auf anforderungspfade oder andere Anforderungsdaten wie Header zu erstellen. Diese Komponenten sind im Microsoft.Owin.Mapping Nuget-Paket verfügbar.

**Benutzerdefinierte Server-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Zeigt, wie Sie einen benutzerdefinierten OWIN-Server verwenden, wenn Sie Selbsthosting OWIN.

**Eingebettete Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Einige OWIN-Server in Ihrem eigenen Prozess ausgeführt werden können (&quot;selbstgehostet&quot;). Dieses Beispiel zeigt, wie Sie zum Starten einer OWIN-Anwendung, die mit den Tools, die von der Microsoft.Owin.Hosting-Nuget-Paket bereitgestellt wird.

**HelloWorld-Beispiels** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN ist ein HTTP-Server-API-Abstraktion, das auf verschiedenen Servern Anwendungsportabilität ermöglicht. Dieses Beispiel veranschaulicht das Schreiben eine Hello World-Anwendung mit einigen **einfache Wrapper** rund um den unformatierten OWIN-Abstraktion, und führen sie auf einem Webserver, z.B. ASP.NET.

**Hello World-Rohdaten OWIN-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
In diesem Beispiel wird veranschaulicht, wie zum Schreiben einer Hello World-Anwendung mithilfe der **unformatierten** OWIN-Abstraktion, und führen Sie es auf einem Webserver wie Asp.Net.

**SignalR-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Zeigt, wie selfhosten von SignalR mit OWIN / Katana. Weitere Informationen zum Self-hosting SignalR finden Sie unter [Tutorial: Selfhosten von SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Beispiel für statische Dateien** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Zeigt, wie zur Unterstützung von HTTP-Anforderungen für statische Dateien, die mithilfe von OWIN / Katana.

**Web-API** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Dieses Beispiel zeigt, wie OWIN in IIS zu hosten und Web-API für die OWIN-Pipeline hinzufügen.

**Web-Socket-Beispiel** | [Quellcode:](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Zeigt, wie Web Sockets in OWIN mithilfe von unterstützen die [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) Klasse.
