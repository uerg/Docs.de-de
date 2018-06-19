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
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Authentifizierung und Autorisierung für SignalR-Hubs (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können.


## <a name="overview"></a>Übersicht

Dieses Thema enthält folgende Abschnitte:

- [Autorisieren Attribut](#authorizeattribute)
- [Authentifizierung für sämtliche hubs](#requireauth)
- [Benutzerdefinierte Autorisierung](#custom)
- [Übergeben Sie Authentifizierungsinformationen für clients](#passauth)
- [Authentifizierungsoptionen für .NET-Clients](#authoptions)

    - [Cookie, mit der Formularauthentifizierung](#cookie)
    - [Windows-Authentifizierung](#windows)
    - [Connection-header](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorisieren Attribut

SignalR stellt die [autorisieren](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder -Methode haben. Dieses Attribut befindet sich der `Microsoft.AspNet.SignalR` Namespace. Sie wenden die `Authorize` -Attribut auf einen Hub oder bestimmte Methoden in einem Hub. Beim Anwenden der `Authorize` -Attribut auf eine hubklasse, die angegebene autorisierungsanforderung auf alle Methoden in den Hub angewendet wird. Nachfolgend finden Sie die verschiedenen Typen von autorisierungsanforderungen, die Sie anwenden können. Ohne die `Authorize` -Attribut, auf dem Hub alle öffentliche Methoden stehen für einen Client, der mit dem Hub verbunden ist.

Wenn Sie eine Rolle mit dem Namen "Admin" in der Webanwendung definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle einen Hub mit den folgenden Code zugreifen können.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Oder Sie können angeben, dass ein Hub enthält eine Methode, die allen Benutzern zur Verfügung steht und eine zweite Methode, die nur für authentifizierte Benutzer verfügbar ist, wie unten dargestellt.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Die folgenden Beispiele behandeln verschiedene autorisierungsszenarien:

- `[Authorize]`– nur authentifizierte Benutzer
- `[Authorize(Roles = "Admin,Manager")]`– nur authentifizierte Benutzer in den angegebenen Rollen
- `[Authorize(Users = "user1,user2")]`– nur authentifizierte Benutzer mit dem angegebenen Benutzernamen
- `[Authorize(RequireOutgoing=false)]`– nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server an Clients keine Autorisierung, z. B., Beschränkung Wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen können die Nachricht empfangen. Die RequireOutgoing-Eigenschaft kann nur auf den gesamten Hub, nicht für Einzelpersonen-Methoden in den Hub angewendet werden. Wenn RequireOutgoing nicht auf "false" festgelegt ist, werden nur Benutzer, die die autorisierungsanforderung erfüllen vom Server aufgerufen.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Authentifizierung für sämtliche hubs

Sie können die Authentifizierung anfordern für alle Hubs und hubmethoden in Ihrer Anwendung durch Aufrufen der [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird. Sie können diese Methode verwenden, wenn Sie mehrere Hubs verfügen und eine Anforderung einer Authentifizierung für alle von ihnen erzwungen werden sollen. Mit dieser Methode können keine Sie-Serverrolle, Benutzer oder ausgehende Autorisierung angeben. Sie können nur angeben, dass der Zugriff auf die hubmethoden für authentifizierte Benutzer beschränkt ist. Allerdings können Sie weiterhin Authorize-Attribut auf Hubs oder zusätzliche Anforderungen an Methoden anwenden. Jede Anforderung, die Sie in Attributen angeben, die zusätzlich zu den Grundlagen der Authentifizierung angewendet wird.

Das folgende Beispiel zeigt eine Datei "Global.asax" das alle hubmethoden auf authentifizierte Benutzer beschränkt.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Beim Aufrufen der `RequireAuthentication()` -Methode auf, nachdem eine SignalR-Anforderung verarbeitet wurde, SignalR löst eine `InvalidOperationException` Ausnahme. Diese Ausnahme wird ausgelöst, da Sie ein Modul, die "Hubpipeline" hinzufügen können, nachdem die Pipeline aufgerufen wurde. Im vorherigen Beispiel ist der Aufruf der `RequireAuthentication` Methode in der `Application_Start` Methode, die einmal vor der Behandlung der ersten Anforderung ausgeführt wird.

<a id="custom"></a>

## <a name="customized-authorization"></a>Benutzerdefinierte Autorisierung

Wenn Sie anpassen, wie die Autorisierung bestimmt wird, können Sie eine Klasse, die abgeleitet erstellen `AuthorizeAttribute` und überschreiben die [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) Methode. Diese Methode wird aufgerufen, für jede Anforderung zu bestimmen, ob der Benutzer autorisiert ist, um die Anforderung abzuschließen. In der überschriebenen Methode geben Sie die erforderliche Logik für Ihr Szenario Autorisierung an. Im folgende Beispiel veranschaulicht die Autorisierung über anspruchsbasierte Identität zu erzwingen.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Übergeben Sie Authentifizierungsinformationen für clients

Sie müssen möglicherweise Authentifizierungsinformationen im Code verwenden, die auf dem Client ausgeführt wird. Beim Aufrufen der Methoden auf dem Client, übergeben Sie die erforderliche Informationen. Z. B. übergeben eine Chat-Application-Methode als Parameter der Benutzername der Person Bereitstellen einer Nachricht wie unten dargestellt.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Oder Sie können ein Objekt, um die Authentifizierungsinformationen darstellen, und übergeben Sie dieses Objekt als Parameter verwendet, erstellen, wie unten dargestellt.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Sie sollten nie eine Clientverbindungs-Id für andere Clients übergeben, wie ein böswilliger Benutzer um zu imitieren, die eine Anforderung von diesem Client verwendet werden konnte.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Authentifizierungsoptionen für .NET-Clients

Wenn Sie einen .NET Client, z. B. eine Konsolen-app, die mit einem Hub, der verfügen interagiert auf authentifizierte Benutzer beschränkt ist, können Sie die Anmeldeinformationen in ein Cookie, das Connection-Header oder ein Zertifikat für die Authentifizierung übergeben. In den Beispielen in diesem Abschnitt zeigen, wie die verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet wird. Sie sind nicht voll funktionsfähig SignalR-apps. Weitere Informationen zu Clients mit SignalR .NET finden Sie unter [Hubs-API - Handbuchs, .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Wenn der .NET Client mit einem Hub, der ASP.NET-Formularauthentifizierung verwendet interagiert, müssen Sie manuell das Authentifizierungscookie für die Verbindung festgelegt. Sie fügen das Cookie an den `CookieContainer` Eigenschaft auf die [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) Objekt. Das folgende Beispiel zeigt eine Konsolen-app, die ein Authentifizierungscookie aus einer Webseite abgerufen und die Verbindung das Cookie hinzugefügt. Die URL `https://www.contoso.com/RemoteLogin` im Beispiel zeigt zu einer Webseite, die Sie erstellen müssen. Die Seite abzurufen, der bereitgestellte Benutzername und das Kennwort, und versuchen, in der Benutzer mit den Anmeldeinformationen anzumelden.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Die Konsolen-app, stellt die Anmeldeinformationen an, die auf eine leere Seite verweisen kann, die den folgenden Code-Behind-Datei enthält www.contoso.com/RemoteLogin bereit.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows-Authentifizierung

Bei Verwendung der Windows-Authentifizierung können Sie die Anmeldeinformationen des aktuellen Benutzers übergeben, indem die [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Eigenschaft. Festlegen der Anmeldeinformationen für die Verbindung auf den Wert, der die DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Connection-header

Wenn Ihre Anwendung keine Cookies verwendet wird, können Sie Benutzerinformationen in der Connection-Header übergeben. Beispielsweise können Sie ein Token in der Connection-Header übergeben.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

In der Hub, dann überprüfen Sie das Zugriffstoken des Benutzers.

<a id="certificate"></a>

### <a name="certificate"></a>Zertifikat

Sie können ein Clientzertifikat, um zu überprüfen, ob den Benutzer übergeben. Das Zertifikat wird beim Erstellen der Verbindung hinzufügen. Im folgende Beispiel wird gezeigt, wie nur die Verbindung ein Clientzertifikat hinzugefügt: die vollständige Konsolen-app werden nicht angezeigt. Er verwendet die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) Klasse die bietet verschiedene Möglichkeiten, das Zertifikat zu erstellen.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
