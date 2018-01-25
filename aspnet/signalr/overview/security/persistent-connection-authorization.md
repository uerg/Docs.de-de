---
uid: signalr/overview/security/persistent-connection-authorization
title: "Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen | Microsoft Docs"
author: pfletcher
description: Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen. Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen. Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung finden Sie unter [Einführung in die Sicherheit](introduction-to-security.md). 
> 
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR Version 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Frühere Versionen dieses Themas
> 
> Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="enforce-authorization"></a>Erzwingen der Autorisierung

Autorisierungsregeln zu erzwingen, bei Verwendung einer ["persistentconnection"](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode. Sie können keine der `Authorize` Attribut mit permanenten Verbindungen. Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer autorisiert ist, um die angeforderte Aktion auszuführen. Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Ihre Anwendung-Authentifizierungsmechanismus.

Im folgenden Beispiel wird gezeigt, wie Anforderungen für authentifizierte Benutzer beschränkt.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen. wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.
