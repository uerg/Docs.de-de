---
uid: signalr/overview/security/hub-authorization
title: Authentifizierung und Autorisierung für SignalR-Hubs | Microsoft Docs
author: pfletcher
description: Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können. Versionen der Software, die in diesem Thema verwendet Visual Studio 2013 .NET 4.5 SignalR Ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8e3bc8889efb1be80c57084fb04dc8030b386601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872753"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="6bae2-104">Authentifizierung und Autorisierung für SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="6bae2-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="6bae2-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6bae2-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6bae2-106">Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="6bae2-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6bae2-107">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="6bae2-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6bae2-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6bae2-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6bae2-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6bae2-109">.NET 4.5</span></span>
> - <span data-ttu-id="6bae2-110">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="6bae2-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6bae2-111">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="6bae2-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6bae2-112">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6bae2-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6bae2-113">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="6bae2-113">Questions and comments</span></span>
> 
> <span data-ttu-id="6bae2-114">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="6bae2-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6bae2-115">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6bae2-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="6bae2-116">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6bae2-116">Overview</span></span>

<span data-ttu-id="6bae2-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="6bae2-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6bae2-118">Autorisieren Attribut</span><span class="sxs-lookup"><span data-stu-id="6bae2-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="6bae2-119">Authentifizierung für sämtliche hubs</span><span class="sxs-lookup"><span data-stu-id="6bae2-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="6bae2-120">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="6bae2-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="6bae2-121">Übergeben Sie Authentifizierungsinformationen für clients</span><span class="sxs-lookup"><span data-stu-id="6bae2-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="6bae2-122">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="6bae2-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="6bae2-123">Cookie, mit der Formularauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="6bae2-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="6bae2-124">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="6bae2-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="6bae2-125">Connection-header</span><span class="sxs-lookup"><span data-stu-id="6bae2-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="6bae2-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="6bae2-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="6bae2-127">Autorisieren Attribut</span><span class="sxs-lookup"><span data-stu-id="6bae2-127">Authorize attribute</span></span>

<span data-ttu-id="6bae2-128">SignalR stellt die [autorisieren](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder -Methode haben.</span><span class="sxs-lookup"><span data-stu-id="6bae2-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="6bae2-129">Dieses Attribut befindet sich der `Microsoft.AspNet.SignalR` Namespace.</span><span class="sxs-lookup"><span data-stu-id="6bae2-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="6bae2-130">Sie wenden die `Authorize` -Attribut auf einen Hub oder bestimmte Methoden in einem Hub.</span><span class="sxs-lookup"><span data-stu-id="6bae2-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="6bae2-131">Beim Anwenden der `Authorize` -Attribut auf eine hubklasse, die angegebene autorisierungsanforderung auf alle Methoden in den Hub angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="6bae2-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="6bae2-132">Dieses Thema enthält Beispiele für die verschiedenen Typen von autorisierungsanforderungen, die Sie anwenden können.</span><span class="sxs-lookup"><span data-stu-id="6bae2-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="6bae2-133">Ohne die `Authorize` -Attribut mit einer verbundenen Client keine öffentliche Methode auf dem Hub zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="6bae2-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="6bae2-134">Wenn Sie eine Rolle mit dem Namen "Admin" in der Webanwendung definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle einen Hub mit den folgenden Code zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="6bae2-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="6bae2-135">Oder Sie können angeben, dass ein Hub enthält eine Methode, die allen Benutzern zur Verfügung steht und eine zweite Methode, die nur für authentifizierte Benutzer verfügbar ist, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="6bae2-136">Die folgenden Beispiele behandeln verschiedene autorisierungsszenarien:</span><span class="sxs-lookup"><span data-stu-id="6bae2-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="6bae2-137">`[Authorize]` – nur authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="6bae2-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="6bae2-138">`[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen</span><span class="sxs-lookup"><span data-stu-id="6bae2-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="6bae2-139">`[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit dem angegebenen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="6bae2-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="6bae2-140">`[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server an Clients keine Autorisierung, z. B., Beschränkung Wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen können die Nachricht empfangen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="6bae2-141">Die RequireOutgoing-Eigenschaft kann nur auf den gesamten Hub, nicht für Einzelpersonen-Methoden in den Hub angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="6bae2-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="6bae2-142">Wenn RequireOutgoing nicht auf "false" festgelegt ist, werden nur Benutzer, die die autorisierungsanforderung erfüllen vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="6bae2-143">Authentifizierung für sämtliche hubs</span><span class="sxs-lookup"><span data-stu-id="6bae2-143">Require authentication for all hubs</span></span>

<span data-ttu-id="6bae2-144">Sie können die Authentifizierung anfordern für alle Hubs und hubmethoden in Ihrer Anwendung durch Aufrufen der [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="6bae2-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="6bae2-145">Sie können diese Methode verwenden, wenn Sie mehrere Hubs verfügen und eine Anforderung einer Authentifizierung für alle von ihnen erzwungen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="6bae2-146">Mit dieser Methode angeben nicht die Anforderungen für die Serverrolle, Benutzer oder ausgehende Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="6bae2-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="6bae2-147">Sie können nur angeben, dass der Zugriff auf die hubmethoden für authentifizierte Benutzer beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="6bae2-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="6bae2-148">Allerdings können Sie weiterhin Authorize-Attribut auf Hubs oder zusätzliche Anforderungen an Methoden anwenden.</span><span class="sxs-lookup"><span data-stu-id="6bae2-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="6bae2-149">Jede Anforderung, die Sie in einem Attribut angeben, wird der wesentlichen Grundlagen der Authentifizierung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="6bae2-150">Das folgende Beispiel zeigt eine Startdatei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="6bae2-151">Beim Aufrufen der `RequireAuthentication()` -Methode auf, nachdem eine SignalR-Anforderung verarbeitet wurde, SignalR löst eine `InvalidOperationException` Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="6bae2-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="6bae2-152">SignalR löst diese Ausnahme aus, da Sie ein Modul, die "Hubpipeline" hinzufügen können, nachdem die Pipeline aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="6bae2-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="6bae2-153">Im vorherigen Beispiel ist der Aufruf der `RequireAuthentication` Methode in der `Configuration` Methode, die einmal vor der Behandlung der ersten Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6bae2-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="6bae2-154">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="6bae2-154">Customized authorization</span></span>

<span data-ttu-id="6bae2-155">Wenn Sie anpassen, wie die Autorisierung bestimmt wird, können Sie eine Klasse, die abgeleitet erstellen `AuthorizeAttribute` und überschreiben die [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) Methode.</span><span class="sxs-lookup"><span data-stu-id="6bae2-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="6bae2-156">Für jede Anforderung ruft SignalR diese Methode, um zu bestimmen, ob der Benutzer autorisiert ist, um die Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="6bae2-157">In der überschriebenen Methode geben Sie die erforderliche Logik für Ihr Szenario Autorisierung an.</span><span class="sxs-lookup"><span data-stu-id="6bae2-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="6bae2-158">Im folgende Beispiel veranschaulicht die Autorisierung über anspruchsbasierte Identität zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="6bae2-159">Übergeben Sie Authentifizierungsinformationen für clients</span><span class="sxs-lookup"><span data-stu-id="6bae2-159">Pass authentication information to clients</span></span>

<span data-ttu-id="6bae2-160">Sie müssen möglicherweise Authentifizierungsinformationen im Code verwenden, die auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6bae2-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="6bae2-161">Beim Aufrufen der Methoden auf dem Client, übergeben Sie die erforderliche Informationen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="6bae2-162">Z. B. übergeben eine Chat-Application-Methode als Parameter der Benutzername der Person Bereitstellen einer Nachricht wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="6bae2-163">Oder Sie können ein Objekt, um die Authentifizierungsinformationen darstellen, und übergeben Sie dieses Objekt als Parameter verwendet, erstellen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="6bae2-164">Sie sollten nie eine Clientverbindungs-Id für andere Clients übergeben, wie ein böswilliger Benutzer um zu imitieren, die eine Anforderung von diesem Client verwendet werden konnte.</span><span class="sxs-lookup"><span data-stu-id="6bae2-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="6bae2-165">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="6bae2-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="6bae2-166">Wenn Sie einen .NET Client, z. B. eine Konsolen-app, die mit einem Hub, der verfügen interagiert auf authentifizierte Benutzer beschränkt ist, können Sie die Anmeldeinformationen in ein Cookie, das Connection-Header oder ein Zertifikat für die Authentifizierung übergeben.</span><span class="sxs-lookup"><span data-stu-id="6bae2-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="6bae2-167">In den Beispielen in diesem Abschnitt zeigen, wie die verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6bae2-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="6bae2-168">Sie sind nicht voll funktionsfähig SignalR-apps.</span><span class="sxs-lookup"><span data-stu-id="6bae2-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="6bae2-169">Weitere Informationen zu Clients mit SignalR .NET finden Sie unter [Hubs-API - Handbuchs, .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="6bae2-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="6bae2-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="6bae2-170">Cookie</span></span>

<span data-ttu-id="6bae2-171">Wenn der .NET Client mit einem Hub, der ASP.NET-Formularauthentifizierung verwendet interagiert, müssen Sie manuell das Authentifizierungscookie für die Verbindung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="6bae2-172">Sie fügen das Cookie an den `CookieContainer` Eigenschaft auf die [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) Objekt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="6bae2-173">Das folgende Beispiel zeigt eine Konsolen-app, die ein Authentifizierungscookie aus einer Webseite abgerufen und die Verbindung das Cookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="6bae2-174">Die Konsolen-app sendet die Anmeldeinformationen für <strong>www.contoso.com/RemoteLogin</strong> konnte die finden Sie in eine leere Seite, die den folgenden Code-Behind-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="6bae2-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="6bae2-175">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="6bae2-175">Windows authentication</span></span>

<span data-ttu-id="6bae2-176">Bei Verwendung der Windows-Authentifizierung können Sie die Anmeldeinformationen des aktuellen Benutzers übergeben, indem die [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6bae2-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="6bae2-177">Festlegen der Anmeldeinformationen für die Verbindung auf den Wert, der die DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="6bae2-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="6bae2-178">Connection-header</span><span class="sxs-lookup"><span data-stu-id="6bae2-178">Connection header</span></span>

<span data-ttu-id="6bae2-179">Wenn Ihre Anwendung keine Cookies verwendet wird, können Sie Benutzerinformationen in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="6bae2-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="6bae2-180">Beispielsweise können Sie ein Token in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="6bae2-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="6bae2-181">In der Hub, dann überprüfen Sie das Zugriffstoken des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="6bae2-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="6bae2-182">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="6bae2-182">Certificate</span></span>

<span data-ttu-id="6bae2-183">Sie können ein Clientzertifikat, um zu überprüfen, ob den Benutzer übergeben.</span><span class="sxs-lookup"><span data-stu-id="6bae2-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="6bae2-184">Das Zertifikat wird beim Erstellen der Verbindung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="6bae2-185">Im folgende Beispiel wird gezeigt, wie nur die Verbindung ein Clientzertifikat hinzugefügt: die vollständige Konsolen-app werden nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6bae2-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="6bae2-186">Er verwendet die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) Klasse die bietet verschiedene Möglichkeiten, das Zertifikat zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6bae2-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
