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
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Authentifizierung und Autorisierung für SignalR-Hubs
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema beschreibt, wie festzulegen, welche Benutzer oder Rollen hubmethoden zugreifen können. 
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

SignalR stellt die [autorisieren](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder -Methode haben. Dieses Attribut befindet sich der `Microsoft.AspNet.SignalR` Namespace. Sie wenden die `Authorize` -Attribut auf einen Hub oder bestimmte Methoden in einem Hub. Beim Anwenden der `Authorize` -Attribut auf eine hubklasse, die angegebene autorisierungsanforderung auf alle Methoden in den Hub angewendet wird. Dieses Thema enthält Beispiele für die verschiedenen Typen von autorisierungsanforderungen, die Sie anwenden können. Ohne die `Authorize` -Attribut mit einer verbundenen Client keine öffentliche Methode auf dem Hub zugegriffen werden kann.

Wenn Sie eine Rolle mit dem Namen "Admin" in der Webanwendung definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle einen Hub mit den folgenden Code zugreifen können.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Oder Sie können angeben, dass ein Hub enthält eine Methode, die allen Benutzern zur Verfügung steht und eine zweite Methode, die nur für authentifizierte Benutzer verfügbar ist, wie unten dargestellt.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Die folgenden Beispiele behandeln verschiedene autorisierungsszenarien:

- `[Authorize]` – nur authentifizierte Benutzer
- `[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen
- `[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit dem angegebenen Benutzernamen
- `[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server an Clients keine Autorisierung, z. B., Beschränkung Wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen können die Nachricht empfangen. Die RequireOutgoing-Eigenschaft kann nur auf den gesamten Hub, nicht für Einzelpersonen-Methoden in den Hub angewendet werden. Wenn RequireOutgoing nicht auf "false" festgelegt ist, werden nur Benutzer, die die autorisierungsanforderung erfüllen vom Server aufgerufen.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Authentifizierung für sämtliche hubs

Sie können die Authentifizierung anfordern für alle Hubs und hubmethoden in Ihrer Anwendung durch Aufrufen der [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) Methode, wenn die Anwendung gestartet wird. Sie können diese Methode verwenden, wenn Sie mehrere Hubs verfügen und eine Anforderung einer Authentifizierung für alle von ihnen erzwungen werden sollen. Mit dieser Methode angeben nicht die Anforderungen für die Serverrolle, Benutzer oder ausgehende Autorisierung. Sie können nur angeben, dass der Zugriff auf die hubmethoden für authentifizierte Benutzer beschränkt ist. Allerdings können Sie weiterhin Authorize-Attribut auf Hubs oder zusätzliche Anforderungen an Methoden anwenden. Jede Anforderung, die Sie in einem Attribut angeben, wird der wesentlichen Grundlagen der Authentifizierung hinzugefügt.

Das folgende Beispiel zeigt eine Startdatei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Beim Aufrufen der `RequireAuthentication()` -Methode auf, nachdem eine SignalR-Anforderung verarbeitet wurde, SignalR löst eine `InvalidOperationException` Ausnahme. SignalR löst diese Ausnahme aus, da Sie ein Modul, die "Hubpipeline" hinzufügen können, nachdem die Pipeline aufgerufen wurde. Im vorherigen Beispiel ist der Aufruf der `RequireAuthentication` Methode in der `Configuration` Methode, die einmal vor der Behandlung der ersten Anforderung ausgeführt wird.

<a id="custom"></a>

## <a name="customized-authorization"></a>Benutzerdefinierte Autorisierung

Wenn Sie anpassen, wie die Autorisierung bestimmt wird, können Sie eine Klasse, die abgeleitet erstellen `AuthorizeAttribute` und überschreiben die [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) Methode. Für jede Anforderung ruft SignalR diese Methode, um zu bestimmen, ob der Benutzer autorisiert ist, um die Anforderung abzuschließen. In der überschriebenen Methode geben Sie die erforderliche Logik für Ihr Szenario Autorisierung an. Im folgende Beispiel veranschaulicht die Autorisierung über anspruchsbasierte Identität zu erzwingen.

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

Wenn der .NET Client mit einem Hub, der ASP.NET-Formularauthentifizierung verwendet interagiert, müssen Sie manuell das Authentifizierungscookie für die Verbindung festgelegt. Sie fügen das Cookie an den `CookieContainer` Eigenschaft auf die [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) Objekt. Das folgende Beispiel zeigt eine Konsolen-app, die ein Authentifizierungscookie aus einer Webseite abgerufen und die Verbindung das Cookie hinzugefügt.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Die Konsolen-app sendet die Anmeldeinformationen für <strong>www.contoso.com/RemoteLogin</strong> konnte die finden Sie in eine leere Seite, die den folgenden Code-Behind-Datei enthält.

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
