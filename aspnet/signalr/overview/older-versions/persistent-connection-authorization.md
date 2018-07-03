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
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="2eac7-104">Authentifizierung und Autorisierung für permanente SignalR-Verbindungen (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="2eac7-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="2eac7-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2eac7-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2eac7-106">In diesem Thema wird beschrieben, wie Autorisierung auf eine permanente Verbindung erzwungen wird.</span><span class="sxs-lookup"><span data-stu-id="2eac7-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="2eac7-107">Allgemeine Informationen über die Sicherheit in einer SignalR-Anwendung integrieren, finden Sie unter [Einführung zur Sicherheit](index.md).</span><span class="sxs-lookup"><span data-stu-id="2eac7-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="2eac7-108">Erzwingen der Autorisierung</span><span class="sxs-lookup"><span data-stu-id="2eac7-108">Enforce authorization</span></span>

<span data-ttu-id="2eac7-109">Autorisierungsregeln zu erzwingen, wenn es sich bei Verwendung einer [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode.</span><span class="sxs-lookup"><span data-stu-id="2eac7-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="2eac7-110">Sie können keine der `Authorize` Attribut mit permanenten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="2eac7-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="2eac7-111">Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer zum Ausführen der angeforderten Aktion berechtigt ist.</span><span class="sxs-lookup"><span data-stu-id="2eac7-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="2eac7-112">Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Standardauthentifizierungsmechanismus Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2eac7-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="2eac7-113">Das folgende Beispiel zeigt, wie Sie Anforderungen an authentifizierte Benutzer beschränken.</span><span class="sxs-lookup"><span data-stu-id="2eac7-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="2eac7-114">Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen; Es wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.</span><span class="sxs-lookup"><span data-stu-id="2eac7-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
