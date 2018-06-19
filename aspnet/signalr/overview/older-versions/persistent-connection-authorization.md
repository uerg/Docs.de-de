---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen. Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036101"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen. Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung finden Sie unter [Einführung in die Sicherheit](index.md).


## <a name="enforce-authorization"></a>Erzwingen der Autorisierung

Autorisierungsregeln zu erzwingen, bei Verwendung einer ["persistentconnection"](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode. Sie können keine der `Authorize` Attribut mit permanenten Verbindungen. Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer autorisiert ist, um die angeforderte Aktion auszuführen. Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Ihre Anwendung-Authentifizierungsmechanismus.

Im folgenden Beispiel wird gezeigt, wie Anforderungen für authentifizierte Benutzer beschränkt.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen. wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.
