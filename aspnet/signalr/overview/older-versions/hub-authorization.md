---
uid: signalr/overview/older-versions/hub-authorization
title: Authentifizierung und Autorisierung für SignalR-Hubs (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042874"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="34191-103">Authentifizierung und Autorisierung für SignalR-Hubs (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="34191-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="34191-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="34191-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="34191-105">Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="34191-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="34191-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="34191-106">Overview</span></span>

<span data-ttu-id="34191-107">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="34191-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="34191-108">Autorisieren Attribut</span><span class="sxs-lookup"><span data-stu-id="34191-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="34191-109">Authentifizierung für sämtliche hubs</span><span class="sxs-lookup"><span data-stu-id="34191-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="34191-110">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="34191-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="34191-111">Übergeben Sie Authentifizierungsinformationen für clients</span><span class="sxs-lookup"><span data-stu-id="34191-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="34191-112">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="34191-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="34191-113">Cookie, mit der Formularauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="34191-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="34191-114">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34191-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="34191-115">Connection-header</span><span class="sxs-lookup"><span data-stu-id="34191-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="34191-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="34191-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="34191-117">Autorisieren Attribut</span><span class="sxs-lookup"><span data-stu-id="34191-117">Authorize attribute</span></span>

<span data-ttu-id="34191-118">SignalR stellt die [autorisieren](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder -Methode haben.</span><span class="sxs-lookup"><span data-stu-id="34191-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="34191-119">Dieses Attribut befindet sich der `Microsoft.AspNet.SignalR` Namespace.</span><span class="sxs-lookup"><span data-stu-id="34191-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="34191-120">Sie wenden die `Authorize` -Attribut auf einen Hub oder bestimmte Methoden in einem Hub.</span><span class="sxs-lookup"><span data-stu-id="34191-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="34191-121">Beim Anwenden der `Authorize` -Attribut auf eine hubklasse, die angegebene autorisierungsanforderung auf alle Methoden in den Hub angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="34191-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="34191-122">Nachfolgend finden Sie die verschiedenen Typen von autorisierungsanforderungen, die Sie anwenden können.</span><span class="sxs-lookup"><span data-stu-id="34191-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="34191-123">Ohne die `Authorize` -Attribut, auf dem Hub alle öffentliche Methoden stehen für einen Client, der mit dem Hub verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="34191-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="34191-124">Wenn Sie eine Rolle mit dem Namen "Admin" in der Webanwendung definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle einen Hub mit den folgenden Code zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="34191-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="34191-125">Oder Sie können angeben, dass ein Hub enthält eine Methode, die allen Benutzern zur Verfügung steht und eine zweite Methode, die nur für authentifizierte Benutzer verfügbar ist, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="34191-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="34191-126">Die folgenden Beispiele behandeln verschiedene autorisierungsszenarien:</span><span class="sxs-lookup"><span data-stu-id="34191-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="34191-127">`[Authorize]`– nur authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="34191-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="34191-128">`[Authorize(Roles = "Admin,Manager")]`– nur authentifizierte Benutzer in den angegebenen Rollen</span><span class="sxs-lookup"><span data-stu-id="34191-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="34191-129">`[Authorize(Users = "user1,user2")]`– nur authentifizierte Benutzer mit dem angegebenen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="34191-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="34191-130">`[Authorize(RequireOutgoing=false)]`– nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server an Clients keine Autorisierung, z. B., Beschränkung Wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen können die Nachricht empfangen.</span><span class="sxs-lookup"><span data-stu-id="34191-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="34191-131">Die RequireOutgoing-Eigenschaft kann nur auf den gesamten Hub, nicht für Einzelpersonen-Methoden in den Hub angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="34191-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="34191-132">Wenn RequireOutgoing nicht auf "false" festgelegt ist, werden nur Benutzer, die die autorisierungsanforderung erfüllen vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="34191-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="34191-133">Authentifizierung für sämtliche hubs</span><span class="sxs-lookup"><span data-stu-id="34191-133">Require authentication for all hubs</span></span>

<span data-ttu-id="34191-134">Sie können die Authentifizierung anfordern für alle Hubs und hubmethoden in Ihrer Anwendung durch Aufrufen der [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="34191-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="34191-135">Sie können diese Methode verwenden, wenn Sie mehrere Hubs verfügen und eine Anforderung einer Authentifizierung für alle von ihnen erzwungen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="34191-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="34191-136">Mit dieser Methode können keine Sie-Serverrolle, Benutzer oder ausgehende Autorisierung angeben.</span><span class="sxs-lookup"><span data-stu-id="34191-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="34191-137">Sie können nur angeben, dass der Zugriff auf die hubmethoden für authentifizierte Benutzer beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="34191-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="34191-138">Allerdings können Sie weiterhin Authorize-Attribut auf Hubs oder zusätzliche Anforderungen an Methoden anwenden.</span><span class="sxs-lookup"><span data-stu-id="34191-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="34191-139">Jede Anforderung, die Sie in Attributen angeben, die zusätzlich zu den Grundlagen der Authentifizierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="34191-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="34191-140">Das folgende Beispiel zeigt eine Datei "Global.asax" das alle hubmethoden auf authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="34191-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="34191-141">Beim Aufrufen der `RequireAuthentication()` -Methode auf, nachdem eine SignalR-Anforderung verarbeitet wurde, SignalR löst eine `InvalidOperationException` Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="34191-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="34191-142">Diese Ausnahme wird ausgelöst, da Sie ein Modul, die "Hubpipeline" hinzufügen können, nachdem die Pipeline aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="34191-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="34191-143">Im vorherigen Beispiel ist der Aufruf der `RequireAuthentication` Methode in der `Application_Start` Methode, die einmal vor der Behandlung der ersten Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="34191-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="34191-144">Benutzerdefinierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="34191-144">Customized authorization</span></span>

<span data-ttu-id="34191-145">Wenn Sie anpassen, wie die Autorisierung bestimmt wird, können Sie eine Klasse, die abgeleitet erstellen `AuthorizeAttribute` und überschreiben die [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) Methode.</span><span class="sxs-lookup"><span data-stu-id="34191-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="34191-146">Diese Methode wird aufgerufen, für jede Anforderung zu bestimmen, ob der Benutzer autorisiert ist, um die Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="34191-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="34191-147">In der überschriebenen Methode geben Sie die erforderliche Logik für Ihr Szenario Autorisierung an.</span><span class="sxs-lookup"><span data-stu-id="34191-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="34191-148">Im folgende Beispiel veranschaulicht die Autorisierung über anspruchsbasierte Identität zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="34191-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="34191-149">Übergeben Sie Authentifizierungsinformationen für clients</span><span class="sxs-lookup"><span data-stu-id="34191-149">Pass authentication information to clients</span></span>

<span data-ttu-id="34191-150">Sie müssen möglicherweise Authentifizierungsinformationen im Code verwenden, die auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="34191-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="34191-151">Beim Aufrufen der Methoden auf dem Client, übergeben Sie die erforderliche Informationen.</span><span class="sxs-lookup"><span data-stu-id="34191-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="34191-152">Z. B. übergeben eine Chat-Application-Methode als Parameter der Benutzername der Person Bereitstellen einer Nachricht wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="34191-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="34191-153">Oder Sie können ein Objekt, um die Authentifizierungsinformationen darstellen, und übergeben Sie dieses Objekt als Parameter verwendet, erstellen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="34191-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="34191-154">Sie sollten nie eine Clientverbindungs-Id für andere Clients übergeben, wie ein böswilliger Benutzer um zu imitieren, die eine Anforderung von diesem Client verwendet werden konnte.</span><span class="sxs-lookup"><span data-stu-id="34191-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="34191-155">Authentifizierungsoptionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="34191-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="34191-156">Wenn Sie einen .NET Client, z. B. eine Konsolen-app, die mit einem Hub, der verfügen interagiert auf authentifizierte Benutzer beschränkt ist, können Sie die Anmeldeinformationen in ein Cookie, das Connection-Header oder ein Zertifikat für die Authentifizierung übergeben.</span><span class="sxs-lookup"><span data-stu-id="34191-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="34191-157">In den Beispielen in diesem Abschnitt zeigen, wie die verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="34191-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="34191-158">Sie sind nicht voll funktionsfähig SignalR-apps.</span><span class="sxs-lookup"><span data-stu-id="34191-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="34191-159">Weitere Informationen zu Clients mit SignalR .NET finden Sie unter [Hubs-API - Handbuchs, .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="34191-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="34191-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="34191-160">Cookie</span></span>

<span data-ttu-id="34191-161">Wenn der .NET Client mit einem Hub, der ASP.NET-Formularauthentifizierung verwendet interagiert, müssen Sie manuell das Authentifizierungscookie für die Verbindung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="34191-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="34191-162">Sie fügen das Cookie an den `CookieContainer` Eigenschaft auf die [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) Objekt.</span><span class="sxs-lookup"><span data-stu-id="34191-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="34191-163">Das folgende Beispiel zeigt eine Konsolen-app, die ein Authentifizierungscookie aus einer Webseite abgerufen und die Verbindung das Cookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="34191-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="34191-164">Die URL `https://www.contoso.com/RemoteLogin` im Beispiel zeigt zu einer Webseite, die Sie erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="34191-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="34191-165">Die Seite abzurufen, der bereitgestellte Benutzername und das Kennwort, und versuchen, in der Benutzer mit den Anmeldeinformationen anzumelden.</span><span class="sxs-lookup"><span data-stu-id="34191-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="34191-166">Die Konsolen-app, stellt die Anmeldeinformationen an, die auf eine leere Seite verweisen kann, die den folgenden Code-Behind-Datei enthält www.contoso.com/RemoteLogin bereit.</span><span class="sxs-lookup"><span data-stu-id="34191-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="34191-167">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34191-167">Windows authentication</span></span>

<span data-ttu-id="34191-168">Bei Verwendung der Windows-Authentifizierung können Sie die Anmeldeinformationen des aktuellen Benutzers übergeben, indem die [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="34191-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="34191-169">Festlegen der Anmeldeinformationen für die Verbindung auf den Wert, der die DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="34191-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="34191-170">Connection-header</span><span class="sxs-lookup"><span data-stu-id="34191-170">Connection header</span></span>

<span data-ttu-id="34191-171">Wenn Ihre Anwendung keine Cookies verwendet wird, können Sie Benutzerinformationen in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="34191-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="34191-172">Beispielsweise können Sie ein Token in der Connection-Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="34191-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="34191-173">In der Hub, dann überprüfen Sie das Zugriffstoken des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="34191-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="34191-174">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="34191-174">Certificate</span></span>

<span data-ttu-id="34191-175">Sie können ein Clientzertifikat, um zu überprüfen, ob den Benutzer übergeben.</span><span class="sxs-lookup"><span data-stu-id="34191-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="34191-176">Das Zertifikat wird beim Erstellen der Verbindung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="34191-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="34191-177">Im folgende Beispiel wird gezeigt, wie nur die Verbindung ein Clientzertifikat hinzugefügt: die vollständige Konsolen-app werden nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34191-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="34191-178">Er verwendet die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) Klasse die bietet verschiedene Möglichkeiten, das Zertifikat zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="34191-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
