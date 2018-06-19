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
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037011"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Zuordnen von SignalR-Benutzern zu Verbindungen in SignalR 1.x
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten wird.


## <a name="introduction"></a>Einführung

Jeder Client, der eine Verbindung mit einem Hub übergibt eine eindeutige Verbindungs-Id an. Sie können diesen Wert in Abrufen der `Context.ConnectionId` Eigenschaft des Hub-Kontexts. Wenn Ihre Anwendung muss einen Benutzer die Verbindungs-Id zuordnen und die Zuordnung beizubehalten, können Sie eine der folgenden:

- [Arbeitsspeicherinterne Speicherung](#inmemory), z. B. ein Wörterbuch
- [SignalR-Gruppe für jeden Benutzer](#groups)
- [Permanente, externe Speichergeräte](#database), z. B. eine Datenbanktabelle oder Azure-Tabellenspeicher

Jede dieser Implementierungen wird in diesem Thema dargestellt. Verwenden Sie die `OnConnected`, `OnDisconnected`, und `OnReconnected` Methoden die `Hub` Klasse, um den Verbindungsstatus für Benutzer zu verfolgen.

Der beste Ansatz für die Anwendung abhängig:

- Die Anzahl der Webserver, die Ihre Anwendung hostet.
- Gibt an, ob Sie eine Liste der derzeit verbundene Benutzer abrufen müssen.
- Gibt an, ob Sie persistent zu speichernde Gruppe und die Benutzerinformationen beim Neustart der Anwendung oder dem Server.
- Gibt an, ob die Wartezeiten bei der Aufruf von einem externen Server ein Problem ist.

Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.

|  | Mehrere server | Abrufen der Liste der derzeit verbundene Benutzer | Beibehalten der Informationen nach dem Neustart | Eine optimale Leistung |
| --- | --- | --- | --- | --- |
| Im Arbeitsspeicher |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Einzelbenutzer-Gruppen | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Externe permanent | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>In-Memory-Speicher

Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einem Wörterbuch beibehalten werden sollen, die im Arbeitsspeicher gespeichert sind. Das Wörterbuch verwendet eine `HashSet` in die Verbindungs­ID gespeichert. Zu einem beliebigen Zeitpunkt kann ein Benutzer mehr als eine Verbindung mit dem SignalR-Anwendung verfügen. Beispielsweise müsste ein Benutzer, die über mehrere Geräte oder mehrere Browserregisterkarte verbunden ist mehr als ein Verbindungs-Id ab.

Wenn die Anwendung heruntergefahren wird, alle Informationen, die verloren gegangen ist jedoch erneut aufgefüllt wird, wie die Benutzer ihre Verbindungen erneut herstellen. Arbeitsspeicherinterne Speicherung ist nicht möglich, wenn Ihre Umgebung mehr als einen Webserver enthält, da jeder Server eine eigene Auflistung von Verbindungen verfügen würde.

Das erste Beispiel zeigt eine Klasse, die die Zuordnung von Benutzern für Verbindungen verwaltet werden. Der Schlüssel für das HashSet werden der Name des Benutzers.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Im nächste Beispiel wird gezeigt, wie der Klasse des Verbindungs-Zuordnung mit einem Hub verwendet wird. Die Instanz der Klasse befindet sich in ein Variablenname `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Einzelbenutzer-Gruppen

Sie können eine Gruppe für jeden Benutzer erstellen und senden eine Nachricht zu dieser Gruppe, wenn Sie nur für diesen Benutzer erreichen möchten. Der Name der Gruppe "Jeder" ist der Name des Benutzers. Wenn ein Benutzer mehrere Verbindungen verfügt, wird jeder Verbindungs-Id Gruppe des Benutzers hinzugefügt.

Sie sollten nicht manuell entfernen den Benutzer aus der Gruppe "Wenn der Benutzer die Verbindung trennt. Diese Aktion wird vom Framework SignalR automatisch ausgeführt.

Im folgende Beispiel wird gezeigt, wie Einzelbenutzer-Gruppen implementiert wird.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Permanente, externen Speicher

In diesem Thema wird gezeigt, wie einer Datenbank oder die Azure-Tabellenspeicher zum Speichern von Verbindungsinformationen. Dieser Ansatz funktioniert, wenn Sie mehrere Webserver verfügen, da jeder Webserver mit der gleichen Datenrepository interagieren kann. Wenn Ihr Webserver arbeiten oder die Anwendung neu gestartet wird, beendet die `OnDisconnected` Methode wird nicht aufgerufen. Aus diesem Grund ist es möglich, dass Ihre Datenrepository Datensätze für Verbindungs-Ids haben, die nicht mehr gültig sind. Um diese verwaiste Einträge zu bereinigen, möchten Sie möglicherweise Verbindung mit Sicherheit ungültig, die außerhalb eines Zeitraums erstellt wurde, die relevant für Ihre Anwendung ist. Die Beispiele in diesem Abschnitt enthalten einen Wert für die Verfolgung, wenn die Verbindung erstellt wurde, aber nicht mehr anzeigen Bereinigen von alte Datensätze, da Sie als Hintergrundprozess nachholen möchten.

### <a name="database"></a>Datenbank

Die folgenden Beispiele zeigen, wie Verbindungs- und Benutzer Informationen in einer Datenbank beibehalten werden sollen. Sie können alle datenzugriffstechnologie verwenden. Das folgende Beispiel zeigt jedoch wie Modelle, die Verwendung von Entity Framework definiert. Diese Entitätsmodelle, die Datenbanktabellen und Felder entsprechen. Die Datenstruktur kann je nach den Anforderungen Ihrer Anwendung unterschiedlich.

Das erste Beispiel zeigt, wie eine Benutzerentität definieren, die viele Entitäten der Verbindung zugeordnet werden können.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure-Tabellenspeicher

Das folgende Beispiel der Azure-Tabelle-Speicher ist ähnlich wie im Beispiel für die Datenbank. Sie schließt nicht alle Informationen ein, die Sie zum Einstieg in Azure-Tabellenspeicherdienst benötigen würde. Informationen finden Sie unter [zum Tabellenspeicher aus .NET verwenden](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Das folgende Beispiel zeigt eine Tabellenentität zum Speichern von Verbindungsinformationen. Er nach Benutzername werden die Daten partitioniert und jede Entität durch die Verbindungs-Id identifiziert werden, damit ein Benutzer mehrere Verbindungen zu einem beliebigen Zeitpunkt verfügen kann.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

In der Hub verfolgen Sie den Status der Verbindung des Benutzers.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
