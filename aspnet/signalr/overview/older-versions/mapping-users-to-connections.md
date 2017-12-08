---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Zuordnen von SignalR-Benutzern zu Verbindungen in SignalR 1.x | Microsoft Docs
author: pfletcher
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="7ca9d-103">Zuordnen von SignalR-Benutzern zu Verbindungen in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="7ca9d-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="7ca9d-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7ca9d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7ca9d-105">In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="7ca9d-106">Einführung</span><span class="sxs-lookup"><span data-stu-id="7ca9d-106">Introduction</span></span>

<span data-ttu-id="7ca9d-107">Jeder Client, der eine Verbindung mit einem Hub übergibt eine eindeutige Verbindungs-Id an. Sie können diesen Wert in Abrufen der `Context.ConnectionId` Eigenschaft des Hub-Kontexts.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="7ca9d-108">Wenn Ihre Anwendung muss einen Benutzer die Verbindungs-Id zuordnen und die Zuordnung beizubehalten, können Sie eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="7ca9d-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="7ca9d-109">[Arbeitsspeicherinterne Speicherung](#inmemory), z. B. ein Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="7ca9d-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="7ca9d-110">SignalR-Gruppe für jeden Benutzer</span><span class="sxs-lookup"><span data-stu-id="7ca9d-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="7ca9d-111">[Permanente, externe Speichergeräte](#database), z. B. eine Datenbanktabelle oder Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="7ca9d-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="7ca9d-112">Jede dieser Implementierungen wird in diesem Thema dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="7ca9d-113">Verwenden Sie die `OnConnected`, `OnDisconnected`, und `OnReconnected` Methoden die `Hub` Klasse, um den Verbindungsstatus für Benutzer zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="7ca9d-114">Der beste Ansatz für die Anwendung abhängig:</span><span class="sxs-lookup"><span data-stu-id="7ca9d-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="7ca9d-115">Die Anzahl der Webserver, die Ihre Anwendung hostet.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="7ca9d-116">Gibt an, ob Sie eine Liste der derzeit verbundene Benutzer abrufen müssen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="7ca9d-117">Gibt an, ob Sie persistent zu speichernde Gruppe und die Benutzerinformationen beim Neustart der Anwendung oder dem Server.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="7ca9d-118">Gibt an, ob die Wartezeiten bei der Aufruf von einem externen Server ein Problem ist.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="7ca9d-119">Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="7ca9d-120">Mehrere server</span><span class="sxs-lookup"><span data-stu-id="7ca9d-120">More than one server</span></span> | <span data-ttu-id="7ca9d-121">Abrufen der Liste der derzeit verbundene Benutzer</span><span class="sxs-lookup"><span data-stu-id="7ca9d-121">Get list of currently connected users</span></span> | <span data-ttu-id="7ca9d-122">Beibehalten der Informationen nach dem Neustart</span><span class="sxs-lookup"><span data-stu-id="7ca9d-122">Persist information after restarts</span></span> | <span data-ttu-id="7ca9d-123">Eine optimale Leistung</span><span class="sxs-lookup"><span data-stu-id="7ca9d-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="7ca9d-124">Im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="7ca9d-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="7ca9d-125">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="7ca9d-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="7ca9d-126">Externe permanent</span><span class="sxs-lookup"><span data-stu-id="7ca9d-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="7ca9d-127">In-Memory-Speicher</span><span class="sxs-lookup"><span data-stu-id="7ca9d-127">In-memory storage</span></span>

<span data-ttu-id="7ca9d-128">Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einem Wörterbuch beibehalten werden sollen, die im Arbeitsspeicher gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="7ca9d-129">Das Wörterbuch verwendet eine `HashSet` in die Verbindungs­ID gespeichert. Zu einem beliebigen Zeitpunkt kann ein Benutzer mehr als eine Verbindung mit dem SignalR-Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="7ca9d-130">Beispielsweise müsste ein Benutzer, die über mehrere Geräte oder mehrere Browserregisterkarte verbunden ist mehr als ein Verbindungs-Id ab.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="7ca9d-131">Wenn die Anwendung heruntergefahren wird, alle Informationen, die verloren gegangen ist jedoch erneut aufgefüllt wird, wie die Benutzer ihre Verbindungen erneut herstellen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="7ca9d-132">Arbeitsspeicherinterne Speicherung ist nicht möglich, wenn Ihre Umgebung mehr als einen Webserver enthält, da jeder Server eine eigene Auflistung von Verbindungen verfügen würde.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="7ca9d-133">Das erste Beispiel zeigt eine Klasse, die die Zuordnung von Benutzern für Verbindungen verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="7ca9d-134">Der Schlüssel für das HashSet werden der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="7ca9d-135">Im nächste Beispiel wird gezeigt, wie der Klasse des Verbindungs-Zuordnung mit einem Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="7ca9d-136">Die Instanz der Klasse befindet sich in ein Variablenname `_connections`.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="7ca9d-137">Einzelbenutzer-Gruppen</span><span class="sxs-lookup"><span data-stu-id="7ca9d-137">Single-user groups</span></span>

<span data-ttu-id="7ca9d-138">Sie können eine Gruppe für jeden Benutzer erstellen und senden eine Nachricht zu dieser Gruppe, wenn Sie nur für diesen Benutzer erreichen möchten.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="7ca9d-139">Der Name der Gruppe "Jeder" ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="7ca9d-140">Wenn ein Benutzer mehrere Verbindungen verfügt, wird jeder Verbindungs-Id Gruppe des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="7ca9d-141">Sie sollten nicht manuell entfernen den Benutzer aus der Gruppe "Wenn der Benutzer die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="7ca9d-142">Diese Aktion wird vom Framework SignalR automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="7ca9d-143">Im folgende Beispiel wird gezeigt, wie Einzelbenutzer-Gruppen implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="7ca9d-144">Permanente, externen Speicher</span><span class="sxs-lookup"><span data-stu-id="7ca9d-144">Permanent, external storage</span></span>

<span data-ttu-id="7ca9d-145">In diesem Thema wird gezeigt, wie einer Datenbank oder die Azure-Tabellenspeicher zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="7ca9d-146">Dieser Ansatz funktioniert, wenn Sie mehrere Webserver verfügen, da jeder Webserver mit der gleichen Datenrepository interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="7ca9d-147">Wenn Ihr Webserver arbeiten oder die Anwendung neu gestartet wird, beendet die `OnDisconnected` Methode wird nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="7ca9d-148">Aus diesem Grund ist es möglich, dass Ihre Datenrepository Datensätze für Verbindungs-Ids haben, die nicht mehr gültig sind.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="7ca9d-149">Um diese verwaiste Einträge zu bereinigen, möchten Sie möglicherweise Verbindung mit Sicherheit ungültig, die außerhalb eines Zeitraums erstellt wurde, die relevant für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="7ca9d-150">Die Beispiele in diesem Abschnitt enthalten einen Wert für die Verfolgung, wenn die Verbindung erstellt wurde, aber nicht mehr anzeigen Bereinigen von alte Datensätze, da Sie als Hintergrundprozess nachholen möchten.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="7ca9d-151">Datenbank</span><span class="sxs-lookup"><span data-stu-id="7ca9d-151">Database</span></span>

<span data-ttu-id="7ca9d-152">Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einer Datenbank beibehalten werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="7ca9d-153">Sie können alle datenzugriffstechnologie verwenden. Das folgende Beispiel zeigt jedoch wie Modelle, die Verwendung von Entity Framework definiert.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="7ca9d-154">Diese Entitätsmodelle, die Datenbanktabellen und Felder entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="7ca9d-155">Die Datenstruktur kann je nach den Anforderungen Ihrer Anwendung unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="7ca9d-156">Das erste Beispiel zeigt, wie eine Benutzerentität definieren, die viele Entitäten der Verbindung zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="7ca9d-157">Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="7ca9d-158">Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="7ca9d-158">Azure table storage</span></span>

<span data-ttu-id="7ca9d-159">Das folgende Beispiel der Azure-Tabelle-Speicher ist ähnlich wie im Beispiel für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="7ca9d-160">Sie schließt nicht alle Informationen ein, die Sie zum Einstieg in Azure-Tabellenspeicherdienst benötigen würde.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="7ca9d-161">Informationen finden Sie unter [zum Tabellenspeicher aus .NET verwenden](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="7ca9d-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="7ca9d-162">Das folgende Beispiel zeigt eine Tabellenentität zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="7ca9d-163">Er nach Benutzername werden die Daten partitioniert und jede Entität durch die Verbindungs-Id identifiziert werden, damit ein Benutzer mehrere Verbindungen zu einem beliebigen Zeitpunkt verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="7ca9d-164">In der Hub verfolgen Sie den Status der Verbindung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="7ca9d-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
