---
uid: signalr/overview/security/hub-authorization
title: Authentifizierung und Autorisierung für SignalR-Hubs | Microsoft-Dokumentation
author: pfletcher
description: Dieses Thema beschreibt, wie Sie einschränken, welche Benutzer oder Rollen hubmethoden zugreifen können. Softwareversionen, die in diesem Thema verwendeten Visual Studio 2013 .NET 4.5 SignalR speichern...
ms.author: aspnetcontent
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6f69ffc31f048c8a962547da37f65b63e2193f5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819035"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="15217-104">Authentifizierung und Autorisierung für SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="15217-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="15217-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="15217-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="15217-106">Dieses Thema beschreibt, wie Sie einschränken, welche Benutzer oder Rollen hubmethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="15217-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="15217-107">In diesem Thema verwendeten Softwareversionen</span><span class="sxs-lookup"><span data-stu-id="15217-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="15217-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="15217-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="15217-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="15217-109">.NET 4.5</span></span>
> - <span data-ttu-id="15217-110">SignalR-Version 2</span><span class="sxs-lookup"><span data-stu-id="15217-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="15217-111">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="15217-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="15217-112">Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="15217-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="15217-113">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="15217-113">Questions and comments</span></span>
> 
> <span data-ttu-id="15217-114">Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="15217-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="15217-115">Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="15217-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="15217-116">Übersicht</span><span class="sxs-lookup"><span data-stu-id="15217-116">Overview</span></span>

<span data-ttu-id="15217-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="15217-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="15217-118">Autorisieren von Attribut</span><span class="sxs-lookup"><span data-stu-id="15217-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="15217-119">Erzwingen der Authentifizierung für alle hubs</span><span class="sxs-lookup"><span data-stu-id="15217-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="15217-120">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="15217-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="15217-121">Übergeben von Informationen für die Authentifizierung für clients</span><span class="sxs-lookup"><span data-stu-id="15217-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="15217-122">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="15217-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="15217-123">Cookie mit der Formularauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="15217-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="15217-124">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="15217-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="15217-125">Connection-header</span><span class="sxs-lookup"><span data-stu-id="15217-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="15217-126">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="15217-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="15217-127">Autorisieren von Attribut</span><span class="sxs-lookup"><span data-stu-id="15217-127">Authorize attribute</span></span>

<span data-ttu-id="15217-128">SignalR stellt die [autorisieren](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer oder Rollen mit einem Hub oder die Methode zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="15217-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="15217-129">Dieses Attribut befindet sich in der `Microsoft.AspNet.SignalR` Namespace.</span><span class="sxs-lookup"><span data-stu-id="15217-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="15217-130">Sie wenden die `Authorize` Attribut entweder einen Hub oder bestimmte Methoden in einem Hub.</span><span class="sxs-lookup"><span data-stu-id="15217-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="15217-131">Beim Anwenden der `Authorize` Attribut, um eine hubklasse, die angegebene autorisierungsanforderung gilt für alle Methoden in den Hub.</span><span class="sxs-lookup"><span data-stu-id="15217-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="15217-132">Dieses Thema enthält Beispiele für die verschiedenen Typen von autorisierungsanforderungen, die Sie anwenden können.</span><span class="sxs-lookup"><span data-stu-id="15217-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="15217-133">Ohne die `Authorize` Attribut einer verbundenen Client kann jede öffentliche Methode auf dem Hub zugreifen.</span><span class="sxs-lookup"><span data-stu-id="15217-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="15217-134">Wenn Sie eine Rolle mit dem Namen "Administrator" in Ihrer Webanwendung definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle einen Hub mit den folgenden Code zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="15217-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="15217-135">Sie können auch angeben, dass ein Hub enthält eine Methode, die für alle Benutzer verfügbar ist, und eine zweite Methode, die nur für authentifizierte Benutzer verfügbar ist, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15217-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="15217-136">Die folgenden Beispiele beziehen sich auf verschiedenen autorisierungsszenarien:</span><span class="sxs-lookup"><span data-stu-id="15217-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="15217-137">`[Authorize]` – nur authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="15217-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="15217-138">`[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen</span><span class="sxs-lookup"><span data-stu-id="15217-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="15217-139">`[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit den angegebenen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="15217-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="15217-140">`[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber die Aufrufe vom Server an Clients keine Autorisierung, z. B., Beschränkung Wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen können die Nachricht empfangen.</span><span class="sxs-lookup"><span data-stu-id="15217-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="15217-141">Die RequireOutgoing-Eigenschaft kann nur für die gesamte Hub-Instanz nicht für Einzelpersonen-Methoden in den Hub angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="15217-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="15217-142">Wenn es sich bei RequireOutgoing nicht auf "false" festgelegt ist, werden nur Benutzer, die die autorisierungsanforderung erfüllen vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="15217-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="15217-143">Erzwingen der Authentifizierung für alle hubs</span><span class="sxs-lookup"><span data-stu-id="15217-143">Require authentication for all hubs</span></span>

<span data-ttu-id="15217-144">Sie können erzwingen der Authentifizierung für alle Hubs und hubmethoden in Ihrer Anwendung durch Aufrufen der [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="15217-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="15217-145">Sie können diese Methode verwenden, wenn Sie mehrere Hubs haben und eine Anforderung zur Authentifizierung für alle erzwungen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="15217-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="15217-146">Mit dieser Methode können keine Sie Anforderungen für die Rolle, die Benutzer oder die ausgehende Autorisierung angeben.</span><span class="sxs-lookup"><span data-stu-id="15217-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="15217-147">Sie können nur angeben, dass der Zugriff auf die hubmethoden auf authentifizierte Benutzer beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="15217-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="15217-148">Allerdings können Sie weiterhin das Authorize-Attribut, Hubs oder Methoden ein, geben Sie zusätzliche Anforderungen anwenden.</span><span class="sxs-lookup"><span data-stu-id="15217-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="15217-149">Alle Anforderungen, die Sie in einem Attribut angeben, wird die grundlegende Anforderung der Authentifizierung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="15217-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="15217-150">Das folgende Beispiel zeigt eine Startdatei, die alle Methoden des Hubs an authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="15217-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="15217-151">Aufrufen der `RequireAuthentication()` -Methode, nach der Verarbeitung einer Anforderung SignalR, SignalR löst eine `InvalidOperationException` Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="15217-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="15217-152">SignalR löst diese Ausnahme aus, da Sie ein Modul auf dem Hubpipeline-Objekt hinzufügen können, nachdem die Pipeline aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="15217-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="15217-153">Im vorherige Beispiel zeigt den Aufruf der `RequireAuthentication` -Methode in der die `Configuration` Methode, die einmal vor der Verarbeitung der ersten Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="15217-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="15217-154">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="15217-154">Customized authorization</span></span>

<span data-ttu-id="15217-155">Bei Bedarf anpassen, wie die Autorisierung bestimmt wird, können Sie eine abgeleitete Klasse erstellen `AuthorizeAttribute` und überschreiben die [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) Methode.</span><span class="sxs-lookup"><span data-stu-id="15217-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="15217-156">Bei jeder Anforderung ruft SignalR für diese Methode, um zu bestimmen, ob der Benutzer autorisiert ist, um die Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="15217-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="15217-157">In der überschriebenen Methode geben Sie die erforderliche Logik an, für Ihr Szenario für die Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="15217-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="15217-158">Das folgende Beispiel zeigt, wie Sie das Erzwingen der Autorisierung über anspruchsbasierte Identität.</span><span class="sxs-lookup"><span data-stu-id="15217-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="15217-159">Übergeben von Informationen für die Authentifizierung für clients</span><span class="sxs-lookup"><span data-stu-id="15217-159">Pass authentication information to clients</span></span>

<span data-ttu-id="15217-160">Sie müssen möglicherweise Informationen für die Authentifizierung im Code verwenden, die auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="15217-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="15217-161">Sie übergeben die erforderliche Informationen beim Aufruf der Methoden auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="15217-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="15217-162">Beispielsweise könnte eine Chat-Anwendung-Methode als Parameter der Benutzername der Person, die eine Nachricht, übergeben wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15217-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="15217-163">Oder Sie können ein Objekt, um die Authentifizierungsinformationen darstellen, und übermitteln Sie dieses Objekt als Parameter erstellen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15217-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="15217-164">Sie sollten niemals eine Clientverbindungs-Id für andere Clients übergeben, wie ein böswilliger Benutzer es verwenden kann, um eine Anforderung von diesem Client imitieren.</span><span class="sxs-lookup"><span data-stu-id="15217-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="15217-165">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="15217-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="15217-166">Wenn Sie einen .NET Client, z. B. eine Konsolen-app, die mit einem Hub interagiert, die verfügen authentifizierte Benutzer beschränkt ist. folgende Aktionen ausführen, können Sie die Anmeldeinformationen in ein Cookie, der Connection-Header oder ein Zertifikat für die Authentifizierung übergeben.</span><span class="sxs-lookup"><span data-stu-id="15217-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="15217-167">In die Beispielen in diesem Abschnitt zeigen, wie die verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="15217-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="15217-168">Sie sind nicht voll funktionsfähigen SignalR-apps.</span><span class="sxs-lookup"><span data-stu-id="15217-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="15217-169">Weitere Informationen zu mit SignalR .NET-Client zu erhalten, finden Sie unter [-API-Leitfaden für die Hubs - Client für .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="15217-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="15217-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="15217-170">Cookie</span></span>

<span data-ttu-id="15217-171">Wenn der .NET Client mit einem Hub, der ASP.NET-Formularauthentifizierung verwendet interagiert, müssen Sie manuell das Authentifizierungscookie für die Verbindung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="15217-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="15217-172">Sie Cookies, das Hinzufügen der `CookieContainer` Eigenschaft für die [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) Objekt.</span><span class="sxs-lookup"><span data-stu-id="15217-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="15217-173">Das folgende Beispiel zeigt eine Konsolen-app, die Ruft ein Authentifizierungscookie auf einer Webseite ab und fügt das Cookie für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="15217-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="15217-174">Die Konsolen-app sendet die Anmeldeinformationen für <strong>www.contoso.com/RemoteLogin</strong> konnte beziehen sich auf eine leere Seite, die den folgenden Code-Behind-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="15217-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="15217-175">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="15217-175">Windows authentication</span></span>

<span data-ttu-id="15217-176">Bei Verwendung der Windows-Authentifizierung können Sie die Anmeldeinformationen des aktuellen Benutzers übergeben, indem Sie mit der [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="15217-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="15217-177">Sie haben die Anmeldeinformationen für die Verbindung auf den Wert des der DefaultCredentials festlegen.</span><span class="sxs-lookup"><span data-stu-id="15217-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="15217-178">Connection-header</span><span class="sxs-lookup"><span data-stu-id="15217-178">Connection header</span></span>

<span data-ttu-id="15217-179">Wenn Ihre Anwendung keine Cookies verwendet wird, können Sie Benutzerinformationen in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="15217-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="15217-180">Beispielsweise können Sie ein Token in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="15217-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="15217-181">Klicken Sie dann würden Sie auf dem Hub das Token des Benutzers überprüfen.</span><span class="sxs-lookup"><span data-stu-id="15217-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="15217-182">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="15217-182">Certificate</span></span>

<span data-ttu-id="15217-183">Sie können ein Clientzertifikat, um zu überprüfen, ob den Benutzer übergeben.</span><span class="sxs-lookup"><span data-stu-id="15217-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="15217-184">Sie fügen Sie das Zertifikat beim Erstellen der Verbindung hinzu.</span><span class="sxs-lookup"><span data-stu-id="15217-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="15217-185">Das folgende Beispiel zeigt, wie nur die Verbindung ein Clientzertifikat hinzugefügt wird; die vollständige Konsolen-app werden nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="15217-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="15217-186">Er verwendet den [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) Klasse bietet mehrere Möglichkeiten zum Erstellen des Zertifikats.</span><span class="sxs-lookup"><span data-stu-id="15217-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
