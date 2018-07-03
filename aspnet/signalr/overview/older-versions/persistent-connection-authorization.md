---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentifizierung und Autorisierung für permanente SignalR-Verbindungen (SignalR 1.x) | Microsoft-Dokumentation
author: pfletcher
description: In diesem Thema wird beschrieben, wie Autorisierung auf eine permanente Verbindung erzwungen wird. Allgemeine Informationen zum Integrieren von Sicherheit in einer SignalR-Anwendung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365109"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Authentifizierung und Autorisierung für permanente SignalR-Verbindungen (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Thema wird beschrieben, wie Autorisierung auf eine permanente Verbindung erzwungen wird. Allgemeine Informationen über die Sicherheit in einer SignalR-Anwendung integrieren, finden Sie unter [Einführung zur Sicherheit](index.md).


## <a name="enforce-authorization"></a>Erzwingen der Autorisierung

Autorisierungsregeln zu erzwingen, wenn es sich bei Verwendung einer [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode. Sie können keine der `Authorize` Attribut mit permanenten Verbindungen. Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer zum Ausführen der angeforderten Aktion berechtigt ist. Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Standardauthentifizierungsmechanismus Ihrer Anwendung.

Das folgende Beispiel zeigt, wie Sie Anforderungen an authentifizierte Benutzer beschränken.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen; Es wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.
