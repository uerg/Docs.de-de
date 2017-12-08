---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Arbeiten mit Gruppen in SignalR | Microsoft Docs
author: pfletcher
description: In diesem Thema wird beschrieben, wie Gruppenmitgliedschaftsinformationen mit der Hub-API beibehalten wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 3befcdbbc735dc4f64c714ba583e026c0c19465d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="d6f94-103">Arbeiten mit Gruppen in SignalR</span><span class="sxs-lookup"><span data-stu-id="d6f94-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="d6f94-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d6f94-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d6f94-105">In diesem Thema wird beschrieben, wie zum Hinzufügen von Benutzern zu Gruppen und Gruppenmitgliedschaftsinformationen beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="d6f94-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d6f94-106">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="d6f94-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="d6f94-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d6f94-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d6f94-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d6f94-108">.NET 4.5</span></span>
> - <span data-ttu-id="d6f94-109">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="d6f94-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d6f94-110">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="d6f94-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="d6f94-111">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d6f94-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d6f94-112">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="d6f94-112">Questions and comments</span></span>
> 
> <span data-ttu-id="d6f94-113">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="d6f94-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d6f94-114">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d6f94-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="d6f94-115">Übersicht</span><span class="sxs-lookup"><span data-stu-id="d6f94-115">Overview</span></span>

<span data-ttu-id="d6f94-116">Gruppen in SignalR bieten eine Methode zum Übertragen von Nachrichten an den angegebenen Teilmengen von verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="d6f94-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="d6f94-117">Eine Gruppe kann eine beliebige Anzahl von Clients umfassen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="d6f94-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="d6f94-118">Sie müssen nicht explizit Gruppen erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="d6f94-119">Tatsächlich eine Gruppe erstellt wird automatisch beim ersten Sie seinen Namen in einem Aufruf von Groups.Add angeben, und er wird gelöscht, wenn Sie aus der Mitgliedschaft in der sie in der letzten Verbindung entfernen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="d6f94-120">Eine Einführung zum Verwenden von Gruppen finden Sie unter [zum Verwalten von Gruppenmitgliedschaften aus die hubklasse](hubs-api-guide-server.md#groupsfromhub) in der Hubs-API - Server-Handbuch.</span><span class="sxs-lookup"><span data-stu-id="d6f94-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="d6f94-121">Es ist keine API zum Abrufen der Mitgliederliste einer Gruppe oder eine Liste der Gruppen ein.</span><span class="sxs-lookup"><span data-stu-id="d6f94-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="d6f94-122">SignalR sendet Nachrichten an Clients und Gruppen, die auf einem Pub/Sub-Modell basiert, und der Server behält keine Listen von Gruppen und Gruppenmitgliedschaften.</span><span class="sxs-lookup"><span data-stu-id="d6f94-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="d6f94-123">Dadurch Maximieren der Skalierbarkeit, da Wenn Sie einen Knoten zu einer Webfarm hinzufügen, muss einem beliebigen Zustand, in der SignalR verwaltet an den neuen Knoten weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="d6f94-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="d6f94-124">Beim Hinzufügen eines Benutzers zu einer Gruppe mithilfe der `Groups.Add` -Methode, die der Benutzer erhält Nachrichten, die zu dieser Gruppe für die Dauer der aktuellen Verbindung geleitet, aber die Mitgliedschaft des Benutzers in dieser Gruppe wird nicht über die aktuelle Verbindung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="d6f94-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="d6f94-125">Wenn Sie Informationen zu Gruppen und Gruppenmitgliedschaften dauerhaft beibehalten möchten, müssen Sie diese Daten in einem Repository wie z. B. einer Datenbank oder Azure-Tabellenspeicher speichern.</span><span class="sxs-lookup"><span data-stu-id="d6f94-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="d6f94-126">Klicken Sie dann jedes Mal, wenn ein Benutzer der Anwendung herstellt Sie aus dem Repository abrufen, welchen Gruppen der Benutzer angehört, und manuell zu diesen Gruppen Benutzer hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="d6f94-127">Beim Wiederherstellen nach einer temporären Unterbrechung der Verbindung Beitritt erneut der Benutzer automatisch die zuvor zugewiesenen Gruppen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="d6f94-128">Eine Gruppe automatisch ein gilt nur beim Wiederherstellen der Verbindung nicht verwendet werden, wenn eine neue Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="d6f94-129">Eine digital signierte Token wird vom Client übergeben, die die Liste der zuvor zugewiesenen Gruppen enthält.</span><span class="sxs-lookup"><span data-stu-id="d6f94-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="d6f94-130">Wenn Sie möchten überprüfen, ob der Benutzer die angeforderte Gruppen angehört, können Sie das Standardverhalten überschreiben.</span><span class="sxs-lookup"><span data-stu-id="d6f94-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="d6f94-131">Dieses Thema enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="d6f94-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="d6f94-132">Hinzufügen und Entfernen von Benutzern</span><span class="sxs-lookup"><span data-stu-id="d6f94-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="d6f94-133">Aufrufen von Membern einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="d6f94-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="d6f94-134">Mitgliedschaft in einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d6f94-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="d6f94-135">Das Speichern von Gruppenmitgliedschaften in Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="d6f94-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="d6f94-136">Überprüfen der Gruppenmitgliedschaft bei einer erneuten Verbindung</span><span class="sxs-lookup"><span data-stu-id="d6f94-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="d6f94-137">Hinzufügen und Entfernen von Benutzern</span><span class="sxs-lookup"><span data-stu-id="d6f94-137">Adding and removing users</span></span>

<span data-ttu-id="d6f94-138">Rufen Sie zum Hinzufügen oder Entfernen von Benutzern aus einer Gruppe, die [hinzufügen](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) oder [entfernen](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) Methoden und Verbindungs-Id des Benutzers und Gruppennamen als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="d6f94-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="d6f94-139">Sie müssen nicht manuell einen Benutzer aus einer Gruppe entfernen, wenn eine Verbindung beendet.</span><span class="sxs-lookup"><span data-stu-id="d6f94-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="d6f94-140">Das folgende Beispiel zeigt die `Groups.Add` und `Groups.Remove` Methoden, die in hubmethoden verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6f94-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="d6f94-141">Die `Groups.Add` und `Groups.Remove` Methoden asynchron auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="d6f94-142">Wenn Sie einen Client zu einer Gruppe hinzufügen und eine Nachricht sofort an den Client senden, mit dem Group möchten, müssen Sie sicherstellen, dass die Methode Groups.Add zuerst beendet wird.</span><span class="sxs-lookup"><span data-stu-id="d6f94-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="d6f94-143">Die folgenden Codebeispiele zeigen, wie nachholen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="d6f94-144">Im Allgemeinen sollten Sie nicht einschließen `await` beim Aufrufen der `Groups.Remove` Methode, da die Verbindungs-Id, die Sie entfernen möchten, sind möglicherweise nicht mehr verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d6f94-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="d6f94-145">In diesem Fall `TaskCanceledException` wird ausgelöst, nachdem die Anforderung ein Timeout eintritt. Wenn Ihre Anwendung sicherstellen muss, dass der Benutzer vor dem Senden einer Nachricht an die Gruppe aus der Gruppe entfernt wurde, können Sie hinzufügen `await` vor Groups.Remove, und klicken Sie dann Abfangen der `TaskCanceledException` Ausnahme, die möglicherweise ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="d6f94-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="d6f94-146">Aufrufen von Membern einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="d6f94-146">Calling members of a group</span></span>

<span data-ttu-id="d6f94-147">Sie können Nachrichten an alle in der die Mitglieder einer Gruppe oder nur angegebene Mitglieder der Gruppe senden, wie in den folgenden Beispielen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d6f94-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="d6f94-148">**Alle** verbundene Clients in einer angegebenen Gruppe.</span><span class="sxs-lookup"><span data-stu-id="d6f94-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="d6f94-149">Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme der angegebenen Clients**identifizierten von Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="d6f94-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="d6f94-150">Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme des aufrufenden Clients**.</span><span class="sxs-lookup"><span data-stu-id="d6f94-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="d6f94-151">Mitgliedschaft in einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d6f94-151">Storing group membership in a database</span></span>

<span data-ttu-id="d6f94-152">Die folgenden Beispiele zeigen, wie Gruppen- und Informationen in einer Datenbank beibehalten werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="d6f94-153">Sie können alle datenzugriffstechnologie verwenden. Das folgende Beispiel zeigt jedoch wie Modelle, die Verwendung von Entity Framework definiert.</span><span class="sxs-lookup"><span data-stu-id="d6f94-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d6f94-154">Diese Entitätsmodelle, die Datenbanktabellen und Felder entsprechen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d6f94-155">Die Datenstruktur kann je nach den Anforderungen Ihrer Anwendung unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="d6f94-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="d6f94-156">In diesem Beispiel enthält eine Klasse namens `ConversationRoom` die würde eindeutig sein, um eine Anwendung, die Benutzern ermöglicht, Konversationen zu verschiedenen Themen, z. B. Sport oder Gartenbau verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="d6f94-157">In diesem Beispiel enthält auch eine Klasse für die Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="d6f94-158">Connection-Klasse ist nicht unbedingt erforderlich, zum Nachverfolgen von Gruppenmitgliedschaft, jedoch ist häufig Teil robuste Lösung zum Nachverfolgen von Benutzern.</span><span class="sxs-lookup"><span data-stu-id="d6f94-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="d6f94-159">Anschließend können Sie auf dem Hub die Gruppen- und Informationen aus der Datenbank abrufen und den Benutzer den entsprechenden Gruppen manuell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d6f94-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="d6f94-160">Das Beispiel enthält Code zum Nachverfolgen der benutzerverbindungen keine.</span><span class="sxs-lookup"><span data-stu-id="d6f94-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="d6f94-161">In diesem Beispiel wird die `await` Schlüsselwort wird nicht angewendet, bevor `Groups.Add` , da eine Nachricht nicht sofort an Mitglieder der Gruppe gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="d6f94-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="d6f94-162">Wenn Sie eine Nachricht an alle Mitglieder der Gruppe zu senden, sofort nach dem das neue Element hinzufügen möchten, möchten Sie würden gelten die `await` Schlüsselwort, um sicherzustellen, dass der asynchrone Vorgang abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="d6f94-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="d6f94-163">Das Speichern von Gruppenmitgliedschaften in Azure-Tabellenspeicher</span><span class="sxs-lookup"><span data-stu-id="d6f94-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="d6f94-164">Mithilfe von Azure-Tabellenspeicher zum Speichern von Informationen von Gruppen- und ähnelt der Verwendung einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d6f94-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="d6f94-165">Das folgende Beispiel zeigt eine Tabellenentität, die den Benutzernamen und der Gruppenname speichert.</span><span class="sxs-lookup"><span data-stu-id="d6f94-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="d6f94-166">Rufen Sie auf dem Hub zugewiesenen Gruppen aus, wenn der Benutzer eine Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="d6f94-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="d6f94-167">Überprüfen der Gruppenmitgliedschaft bei einer erneuten Verbindung</span><span class="sxs-lookup"><span data-stu-id="d6f94-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="d6f94-168">Standardmäßig weist erneut SignalR automatisch einen Benutzer den entsprechenden Gruppen bei einer erneuten Verbindung aus einer temporären Unterbrechung, z. B. wenn eine Verbindung gelöscht und neu eingerichtet werden, bevor die Verbindung ein Timeout eintritt. Gruppeninformationen des Benutzers wird in einem Token übergeben, bei einer erneuten Verbindung, und dieses Token wird auf dem Server überprüft.</span><span class="sxs-lookup"><span data-stu-id="d6f94-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="d6f94-169">Informationen zu den Überprüfungsprozess für die ein Benutzer, Gruppen, finden Sie unter [Gruppen ein, bei einer erneuten Verbindung](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="d6f94-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="d6f94-170">Im Allgemeinen sollten Sie das Standardverhalten des tritt automatisch wieder, dass die Gruppen auf Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d6f94-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="d6f94-171">SignalR Gruppen dürfen nicht als Sicherheitsmechanismus für den Zugriff auf vertrauliche Daten einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="d6f94-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="d6f94-172">Wenn Ihre Anwendung bei einer erneuten Verbindung Gruppenmitgliedschaft eines Benutzers überprüfen muss, können Sie das Standardverhalten überschreiben.</span><span class="sxs-lookup"><span data-stu-id="d6f94-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="d6f94-173">Ändern des Standardverhaltens kann der Last mit Ihrer Datenbank hinzufügen, da die Gruppenmitgliedschaft des Benutzers abgerufen werden muss, für jedes erneuten Herstellen einer Verbindung, statt nur, wenn der Benutzer eine Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="d6f94-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="d6f94-174">Wenn Sie auf der Gruppenmitgliedschaft überprüfen müssen erneut eine Verbindung herzustellen Sie, erstellen Sie ein neues Hub-Pipeline-Modul, das eine Liste der zugewiesenen Gruppen zurückgibt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d6f94-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="d6f94-175">Anschließend fügen Sie, dass das Modul die Hub-Pipeline, wie unten markiert.</span><span class="sxs-lookup"><span data-stu-id="d6f94-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
