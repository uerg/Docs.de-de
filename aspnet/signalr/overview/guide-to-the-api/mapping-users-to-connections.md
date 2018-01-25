---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Zuordnen von SignalR-Benutzern zu Verbindungen | Microsoft Docs
author: tfitzmac
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten wird. Patrick Fletcher-Abgleich dabei behilflich waren, Schreiben in diesem Thema. In diesem Thema verwendeten Versionen der Software...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="7eb4c-105">Zuordnen von SignalR-Benutzern zu Verbindungen</span><span class="sxs-lookup"><span data-stu-id="7eb4c-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="7eb4c-106">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7eb4c-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7eb4c-107">In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="7eb4c-108">Patrick Fletcher-Abgleich dabei behilflich waren, Schreiben in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7eb4c-109">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="7eb4c-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="7eb4c-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7eb4c-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7eb4c-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7eb4c-111">.NET 4.5</span></span>
> - <span data-ttu-id="7eb4c-112">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="7eb4c-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7eb4c-113">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="7eb4c-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="7eb4c-114">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7eb4c-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="7eb4c-115">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="7eb4c-115">Questions and comments</span></span>
> 
> <span data-ttu-id="7eb4c-116">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7eb4c-117">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7eb4c-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="7eb4c-118">Einführung</span><span class="sxs-lookup"><span data-stu-id="7eb4c-118">Introduction</span></span>

<span data-ttu-id="7eb4c-119">Jeder Client, der eine Verbindung mit einem Hub übergibt eine eindeutige Verbindungs-Id an. Sie können diesen Wert in Abrufen der `Context.ConnectionId` Eigenschaft des Hub-Kontexts.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="7eb4c-120">Wenn Ihre Anwendung muss einen Benutzer die Verbindungs-Id zuordnen und die Zuordnung beizubehalten, können Sie eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="7eb4c-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="7eb4c-121">Die Benutzer-ID-Anbieter (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="7eb4c-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="7eb4c-122">[Arbeitsspeicherinterne Speicherung](#inmemory), z. B. ein Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="7eb4c-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="7eb4c-123">SignalR-Gruppe für jeden Benutzer</span><span class="sxs-lookup"><span data-stu-id="7eb4c-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="7eb4c-124">[Permanente, externe Speichergeräte](#database), z. B. eine Datenbanktabelle oder Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="7eb4c-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="7eb4c-125">Jede dieser Implementierungen wird in diesem Thema dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="7eb4c-126">Verwenden Sie die `OnConnected`, `OnDisconnected`, und `OnReconnected` Methoden die `Hub` Klasse, um den Verbindungsstatus für Benutzer zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="7eb4c-127">Der beste Ansatz für die Anwendung abhängig:</span><span class="sxs-lookup"><span data-stu-id="7eb4c-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="7eb4c-128">Die Anzahl der Webserver, die Ihre Anwendung hostet.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="7eb4c-129">Gibt an, ob Sie eine Liste der derzeit verbundene Benutzer abrufen müssen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="7eb4c-130">Gibt an, ob Sie persistent zu speichernde Gruppe und die Benutzerinformationen beim Neustart der Anwendung oder dem Server.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="7eb4c-131">Gibt an, ob die Wartezeiten bei der Aufruf von einem externen Server ein Problem ist.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="7eb4c-132">Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="7eb4c-133">Mehrere server</span><span class="sxs-lookup"><span data-stu-id="7eb4c-133">More than one server</span></span> | <span data-ttu-id="7eb4c-134">Abrufen der Liste der derzeit verbundene Benutzer</span><span class="sxs-lookup"><span data-stu-id="7eb4c-134">Get list of currently connected users</span></span> | <span data-ttu-id="7eb4c-135">Beibehalten der Informationen nach dem Neustart</span><span class="sxs-lookup"><span data-stu-id="7eb4c-135">Persist information after restarts</span></span> | <span data-ttu-id="7eb4c-136">Eine optimale Leistung</span><span class="sxs-lookup"><span data-stu-id="7eb4c-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="7eb4c-137">Benutzer-ID-Anbieter</span><span class="sxs-lookup"><span data-stu-id="7eb4c-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="7eb4c-138">Im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="7eb4c-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="7eb4c-139">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="7eb4c-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="7eb4c-140">Externe permanent</span><span class="sxs-lookup"><span data-stu-id="7eb4c-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="7eb4c-141">IUserID-Anbieter</span><span class="sxs-lookup"><span data-stu-id="7eb4c-141">IUserID provider</span></span>

<span data-ttu-id="7eb4c-142">Diese Funktion ermöglicht Benutzern die Angabe, was die Benutzer-ID wird basierend auf einer IRequest über eine neue Schnittstelle IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="7eb4c-143">**The IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="7eb4c-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="7eb4c-144">Standardmäßig wird eine Implementierung, die der Benutzer verwendet `IPrincipal.Identity.Name` als Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="7eb4c-145">Um dies zu ändern, registrieren Sie Ihre Implementierung von `IUserIdProvider` mit dem globalen Host beim Start der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="7eb4c-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="7eb4c-146">Von innerhalb eines Hubs werden Sie Nachrichten an diese Benutzer über die folgende API gesendet:</span><span class="sxs-lookup"><span data-stu-id="7eb4c-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="7eb4c-147">**Senden einer Nachricht an einen bestimmten Benutzer**</span><span class="sxs-lookup"><span data-stu-id="7eb4c-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="7eb4c-148">In-Memory-Speicher</span><span class="sxs-lookup"><span data-stu-id="7eb4c-148">In-memory storage</span></span>

<span data-ttu-id="7eb4c-149">Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einem Wörterbuch beibehalten werden sollen, die im Arbeitsspeicher gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="7eb4c-150">Das Wörterbuch verwendet eine `HashSet` in die Verbindungs­ID gespeichert. Zu einem beliebigen Zeitpunkt kann ein Benutzer mehr als eine Verbindung mit dem SignalR-Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="7eb4c-151">Beispielsweise müsste ein Benutzer, die über mehrere Geräte oder mehrere Browserregisterkarte verbunden ist mehr als ein Verbindungs-Id ab.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="7eb4c-152">Wenn die Anwendung heruntergefahren wird, alle Informationen, die verloren gegangen ist jedoch erneut aufgefüllt wird, wie die Benutzer ihre Verbindungen erneut herstellen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="7eb4c-153">Arbeitsspeicherinterne Speicherung ist nicht möglich, wenn Ihre Umgebung mehr als einen Webserver enthält, da jeder Server eine eigene Auflistung von Verbindungen verfügen würde.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="7eb4c-154">Das erste Beispiel zeigt eine Klasse, die die Zuordnung von Benutzern für Verbindungen verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="7eb4c-155">Der Schlüssel für das HashSet werden der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="7eb4c-156">Im nächste Beispiel wird gezeigt, wie der Klasse des Verbindungs-Zuordnung mit einem Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="7eb4c-157">Die Instanz der Klasse befindet sich in ein Variablenname `_connections`.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="7eb4c-158">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="7eb4c-158">Single-user groups</span></span>

<span data-ttu-id="7eb4c-159">Sie können eine Gruppe für jeden Benutzer erstellen und senden eine Nachricht zu dieser Gruppe, wenn Sie nur für diesen Benutzer erreichen möchten.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="7eb4c-160">Der Name der Gruppe "Jeder" ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="7eb4c-161">Wenn ein Benutzer mehrere Verbindungen verfügt, wird jeder Verbindungs-Id Gruppe des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="7eb4c-162">Sie sollten nicht manuell entfernen den Benutzer aus der Gruppe "Wenn der Benutzer die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="7eb4c-163">Diese Aktion wird vom Framework SignalR automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="7eb4c-164">Im folgende Beispiel wird gezeigt, wie Einzelbenutzer-Gruppen implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="7eb4c-165">Permanente, externen Speicher</span><span class="sxs-lookup"><span data-stu-id="7eb4c-165">Permanent, external storage</span></span>

<span data-ttu-id="7eb4c-166">In diesem Thema wird gezeigt, wie einer Datenbank oder die Azure-Tabellenspeicher zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="7eb4c-167">Dieser Ansatz funktioniert, wenn Sie mehrere Webserver verfügen, da jeder Webserver mit der gleichen Datenrepository interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="7eb4c-168">Wenn Ihr Webserver arbeiten oder die Anwendung neu gestartet wird, beendet die `OnDisconnected` Methode wird nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="7eb4c-169">Aus diesem Grund ist es möglich, dass Ihre Datenrepository Datensätze für Verbindungs-Ids haben, die nicht mehr gültig sind.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="7eb4c-170">Um diese verwaiste Einträge zu bereinigen, möchten Sie möglicherweise Verbindung mit Sicherheit ungültig, die außerhalb eines Zeitraums erstellt wurde, die relevant für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="7eb4c-171">Die Beispiele in diesem Abschnitt enthalten einen Wert für die Verfolgung, wenn die Verbindung erstellt wurde, aber nicht mehr anzeigen Bereinigen von alte Datensätze, da Sie als Hintergrundprozess nachholen möchten.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="7eb4c-172">Datenbank</span><span class="sxs-lookup"><span data-stu-id="7eb4c-172">Database</span></span>

<span data-ttu-id="7eb4c-173">Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einer Datenbank beibehalten werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="7eb4c-174">Sie können alle datenzugriffstechnologie verwenden. Das folgende Beispiel zeigt jedoch wie Modelle, die Verwendung von Entity Framework definiert.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="7eb4c-175">Diese Entitätsmodelle, die Datenbanktabellen und Felder entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="7eb4c-176">Die Datenstruktur kann je nach den Anforderungen Ihrer Anwendung unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="7eb4c-177">Das erste Beispiel zeigt, wie eine Benutzerentität definieren, die viele Entitäten der Verbindung zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="7eb4c-178">Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="7eb4c-179">Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="7eb4c-179">Azure table storage</span></span>

<span data-ttu-id="7eb4c-180">Das folgende Beispiel der Azure-Tabelle-Speicher ist ähnlich wie im Beispiel für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="7eb4c-181">Sie schließt nicht alle Informationen ein, die Sie zum Einstieg in Azure-Tabellenspeicherdienst benötigen würde.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="7eb4c-182">Informationen finden Sie unter [zum Tabellenspeicher aus .NET verwenden](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="7eb4c-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="7eb4c-183">Das folgende Beispiel zeigt eine Tabellenentität zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="7eb4c-184">Er nach Benutzername werden die Daten partitioniert und jede Entität durch die Verbindungs-Id identifiziert werden, damit ein Benutzer mehrere Verbindungen zu einem beliebigen Zeitpunkt verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="7eb4c-185">In der Hub verfolgen Sie den Status der Verbindung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7eb4c-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
