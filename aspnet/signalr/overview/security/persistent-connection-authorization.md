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
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="40e1d-104">Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen</span><span class="sxs-lookup"><span data-stu-id="40e1d-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="40e1d-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40e1d-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40e1d-106">Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="40e1d-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="40e1d-107">Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung finden Sie unter [Einführung in die Sicherheit](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="40e1d-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="40e1d-108">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="40e1d-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="40e1d-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="40e1d-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="40e1d-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="40e1d-110">.NET 4.5</span></span>
> - <span data-ttu-id="40e1d-111">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="40e1d-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="40e1d-112">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="40e1d-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="40e1d-113">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="40e1d-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="40e1d-114">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="40e1d-114">Questions and comments</span></span>
> 
> <span data-ttu-id="40e1d-115">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="40e1d-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="40e1d-116">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="40e1d-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="40e1d-117">Erzwingen der Autorisierung</span><span class="sxs-lookup"><span data-stu-id="40e1d-117">Enforce authorization</span></span>

<span data-ttu-id="40e1d-118">Autorisierungsregeln zu erzwingen, bei Verwendung einer ["persistentconnection"](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode.</span><span class="sxs-lookup"><span data-stu-id="40e1d-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="40e1d-119">Sie können keine der `Authorize` Attribut mit permanenten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="40e1d-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="40e1d-120">Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer autorisiert ist, um die angeforderte Aktion auszuführen.</span><span class="sxs-lookup"><span data-stu-id="40e1d-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="40e1d-121">Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Ihre Anwendung-Authentifizierungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="40e1d-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="40e1d-122">Im folgenden Beispiel wird gezeigt, wie Anforderungen für authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="40e1d-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="40e1d-123">Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen. wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.</span><span class="sxs-lookup"><span data-stu-id="40e1d-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
