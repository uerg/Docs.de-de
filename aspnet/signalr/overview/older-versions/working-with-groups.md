---
uid: signalr/overview/older-versions/working-with-groups
title: Arbeiten mit Gruppen in SignalR 1.x | Microsoft Docs
author: pfletcher
description: In diesem Thema wird beschrieben, wie Gruppenmitgliedschaftsinformationen mit der Hub-API beibehalten wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036543"
---
<a name="working-with-groups-in-signalr-1x"></a>Arbeiten mit Gruppen in SignalR 1.x
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Thema wird beschrieben, wie zum Hinzufügen von Benutzern zu Gruppen und Gruppenmitgliedschaftsinformationen beibehalten wird.


## <a name="overview"></a>Übersicht

Gruppen in SignalR bieten eine Methode zum Übertragen von Nachrichten an den angegebenen Teilmengen von verbundenen Clients. Eine Gruppe kann eine beliebige Anzahl von Clients umfassen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein. Sie müssen nicht explizit Gruppen erstellen. Tatsächlich eine Gruppe erstellt wird automatisch beim ersten Sie seinen Namen in einem Aufruf von Groups.Add angeben, und er wird gelöscht, wenn Sie aus der Mitgliedschaft in der sie in der letzten Verbindung entfernen. Eine Einführung zum Verwenden von Gruppen finden Sie unter [zum Verwalten von Gruppenmitgliedschaften aus die hubklasse](index.md) in der Hubs-API - Server-Handbuch.

Es ist keine API zum Abrufen der Mitgliederliste einer Gruppe oder eine Liste der Gruppen ein. SignalR sendet Nachrichten an Clients und Gruppen, die auf einem Pub/Sub-Modell basiert, und der Server behält keine Listen von Gruppen und Gruppenmitgliedschaften. Dadurch Maximieren der Skalierbarkeit, da Wenn Sie einen Knoten zu einer Webfarm hinzufügen, muss einem beliebigen Zustand, in der SignalR verwaltet an den neuen Knoten weitergegeben werden.

Beim Hinzufügen eines Benutzers zu einer Gruppe mithilfe der `Groups.Add` -Methode, die der Benutzer erhält Nachrichten, die zu dieser Gruppe für die Dauer der aktuellen Verbindung geleitet, aber die Mitgliedschaft des Benutzers in dieser Gruppe wird nicht über die aktuelle Verbindung beibehalten. Wenn Sie Informationen zu Gruppen und Gruppenmitgliedschaften dauerhaft beibehalten möchten, müssen Sie diese Daten in einem Repository wie z. B. einer Datenbank oder Azure-Tabellenspeicher speichern. Klicken Sie dann jedes Mal, wenn ein Benutzer der Anwendung herstellt Sie aus dem Repository abrufen, welchen Gruppen der Benutzer angehört, und manuell zu diesen Gruppen Benutzer hinzufügen.

Beim Wiederherstellen nach einer temporären Unterbrechung der Verbindung Beitritt erneut der Benutzer automatisch die zuvor zugewiesenen Gruppen. Eine Gruppe automatisch ein gilt nur beim Wiederherstellen der Verbindung nicht verwendet werden, wenn eine neue Verbindung herstellen. Eine digital signierte Token wird vom Client übergeben, die die Liste der zuvor zugewiesenen Gruppen enthält. Wenn Sie möchten überprüfen, ob der Benutzer die angeforderte Gruppen angehört, können Sie das Standardverhalten überschreiben.

Dieses Thema enthält die folgenden Abschnitte:

- [Hinzufügen und Entfernen von Benutzern](#add)
- [Aufrufen von Membern einer Gruppe](#call)
- [Mitgliedschaft in einer Datenbank gespeichert.](#storedatabase)
- [Das Speichern von Gruppenmitgliedschaften in Azure-Tabellenspeicher](#storeazuretable)
- [Überprüfen der Gruppenmitgliedschaft bei einer erneuten Verbindung](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Hinzufügen und Entfernen von Benutzern

Rufen Sie zum Hinzufügen oder Entfernen von Benutzern aus einer Gruppe, die [hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) oder [entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) Methoden und Verbindungs-Id des Benutzers und Gruppennamen als Parameter übergeben. Sie müssen nicht manuell einen Benutzer aus einer Gruppe entfernen, wenn eine Verbindung beendet.

Das folgende Beispiel zeigt die `Groups.Add` und `Groups.Remove` Methoden, die in hubmethoden verwendet.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Die `Groups.Add` und `Groups.Remove` Methoden asynchron auszuführen.

Wenn Sie einen Client zu einer Gruppe hinzufügen und eine Nachricht sofort an den Client senden, mit dem Group möchten, müssen Sie sicherstellen, dass die Methode Groups.Add zuerst beendet wird. Die folgenden Codebeispiele zeigen, wie diese mithilfe von Code, der in .NET 4.5 und mithilfe von Code, der in .NET 4 funktioniert funktioniert.

#### <a name="asynchronous-net-45-example"></a>Asynchrone .NET 4.5 Beispiel

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchrone .NET 4 Beispiel

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Im Allgemeinen sollten Sie nicht einschließen `await` beim Aufrufen der `Groups.Remove` Methode, da die Verbindungs-Id, die Sie entfernen möchten, sind möglicherweise nicht mehr verfügbar. In diesem Fall `TaskCanceledException` wird ausgelöst, nachdem die Anforderung ein Timeout eintritt. Wenn Ihre Anwendung sicherstellen muss, dass der Benutzer vor dem Senden einer Nachricht an die Gruppe aus der Gruppe entfernt wurde, können Sie hinzufügen `await` vor Groups.Remove, und klicken Sie dann Abfangen der `TaskCanceledException` Ausnahme, die möglicherweise ausgelöst werden.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Aufrufen von Membern einer Gruppe

Sie können Nachrichten an alle in der die Mitglieder einer Gruppe oder nur angegebene Mitglieder der Gruppe senden, wie in den folgenden Beispielen gezeigt.

- **Alle** verbundene Clients in einer angegebenen Gruppe. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme der angegebenen Clients**identifizierten von Verbindungs-ID. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme des aufrufenden Clients**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Mitgliedschaft in einer Datenbank gespeichert.

Die folgenden Beispiele zeigen, wie Gruppen- und Informationen in einer Datenbank beibehalten werden sollen. Sie können alle datenzugriffstechnologie verwenden. Das folgende Beispiel zeigt jedoch wie Modelle, die Verwendung von Entity Framework definiert. Diese Entitätsmodelle, die Datenbanktabellen und Felder entsprechen. Die Datenstruktur kann je nach den Anforderungen Ihrer Anwendung unterschiedlich. In diesem Beispiel enthält eine Klasse namens `ConversationRoom` die würde eindeutig sein, um eine Anwendung, die Benutzern ermöglicht, Konversationen zu verschiedenen Themen, z. B. Sport oder Gartenbau verknüpfen. In diesem Beispiel enthält auch eine Klasse für die Verbindungen. Connection-Klasse ist nicht unbedingt erforderlich, zum Nachverfolgen von Gruppenmitgliedschaft, jedoch ist häufig Teil robuste Lösung zum Nachverfolgen von Benutzern.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Anschließend können Sie auf dem Hub die Gruppen- und Informationen aus der Datenbank abrufen und den Benutzer den entsprechenden Gruppen manuell hinzufügen. Das Beispiel enthält Code zum Nachverfolgen der benutzerverbindungen keine. In diesem Beispiel wird die `await` Schlüsselwort wird nicht angewendet, bevor `Groups.Add` , da eine Nachricht nicht sofort an Mitglieder der Gruppe gesendet werden. Wenn Sie eine Nachricht an alle Mitglieder der Gruppe zu senden, sofort nach dem das neue Element hinzufügen möchten, möchten Sie würden gelten die `await` Schlüsselwort, um sicherzustellen, dass der asynchrone Vorgang abgeschlossen wurde.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Das Speichern von Gruppenmitgliedschaften in Azure-Tabellenspeicher

Mithilfe von Azure-Tabellenspeicher zum Speichern von Informationen von Gruppen- und ähnelt der Verwendung einer Datenbank. Das folgende Beispiel zeigt eine Tabellenentität, die den Benutzernamen und der Gruppenname speichert.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Rufen Sie auf dem Hub zugewiesenen Gruppen aus, wenn der Benutzer eine Verbindung herstellt.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Überprüfen der Gruppenmitgliedschaft bei einer erneuten Verbindung

Standardmäßig weist erneut SignalR automatisch einen Benutzer den entsprechenden Gruppen bei einer erneuten Verbindung aus einer temporären Unterbrechung, z. B. wenn eine Verbindung gelöscht und neu eingerichtet werden, bevor die Verbindung ein Timeout eintritt. Gruppeninformationen des Benutzers wird in einem Token übergeben, bei einer erneuten Verbindung, und dieses Token wird auf dem Server überprüft. Informationen zu den Überprüfungsprozess für die ein Benutzer, Gruppen, finden Sie unter [Gruppen ein, bei einer erneuten Verbindung](index.md).

Im Allgemeinen sollten Sie das Standardverhalten des tritt automatisch wieder, dass die Gruppen auf Verbindung verwenden. SignalR Gruppen dürfen nicht als Sicherheitsmechanismus für den Zugriff auf vertrauliche Daten einzuschränken. Wenn Ihre Anwendung bei einer erneuten Verbindung Gruppenmitgliedschaft eines Benutzers überprüfen muss, können Sie das Standardverhalten überschreiben. Ändern des Standardverhaltens kann der Last mit Ihrer Datenbank hinzufügen, da die Gruppenmitgliedschaft des Benutzers abgerufen werden muss, für jedes erneuten Herstellen einer Verbindung, statt nur, wenn der Benutzer eine Verbindung herstellt.

Wenn Sie auf der Gruppenmitgliedschaft überprüfen müssen erneut eine Verbindung herzustellen Sie, erstellen Sie ein neues Hub-Pipeline-Modul, das eine Liste der zugewiesenen Gruppen zurückgibt, wie unten dargestellt.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Anschließend fügen Sie, dass das Modul die Hub-Pipeline, wie unten markiert.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
