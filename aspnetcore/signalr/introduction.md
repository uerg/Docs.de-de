---
title: Einführung in ASP.NET Core SignalR
author: rachelappel
description: Erfahren Sie, wie die SignalR für ASP.NET Core-Bibliothek vereinfacht das Hinzufügen von Funktionen in Echtzeit zu apps.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923354"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Einführung in ASP.NET Core SignalR

Von [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Was eine SignalR ist?

SignalR für ASP.NET Core ist eine Bibliothek, die das Hinzufügen von Echtzeit-Webfunktionen Apps vereinfacht. Echtzeit-Webfunktionen kann serverseitigen Code Push-Inhalte für Clients sofort an.

Gute Kandidaten für SignalR:

* Apps, die sehr häufig Updates vom Server erfordern. Beispiele sind Spiele, soziale Netzwerke abstimmen, Auktion, Zuordnungen und GPS-apps.
* Dashboards und Überwachung apps. Beispiele umfassen Unternehmensdashboards, instant sales Updates, oder die Warnungen übertragen.
* Apps für die Zusammenarbeit. Whiteboard-apps und Software erfüllen Team sind Beispiele für apps für die Zusammenarbeit auf.
* Apps, die benötigt wird. Verwenden Benachrichtigungen, soziale Netzwerke, e-Mail, Chat, Spiele, Reisen Warnungen und viele andere apps.

SignalR stellt eine API zum Erstellen von Server-zu-Client [von Remoteprozeduraufrufen (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Die RPCs rufen Sie JavaScript-Funktionen auf Clients aus serverseitigem .NET Core-Code.

SignalR für ASP.NET Core:

* Verbindungsverwaltung behandelt automatisch.
* Ermöglicht das Senden von Nachrichten für alle verbundenen Clients gleichzeitig über. Z. B. einem Chatraum.
* Ermöglicht das Senden von Nachrichten an bestimmte Clients oder Clientgruppen.
* Wird auf Open Source [GitHub](https://github.com/aspnet/signalr).
* Skalierbare.

Die Verbindung zwischen Client und Server ist persistent, im Gegensatz zu einer HTTP-Verbindung.

## <a name="transports"></a>Transportprotokolle

SignalR abstrahiert über eine Reihe von Techniken zum Erstellen von Echtzeit-Webanwendungen. [WebSockets](https://tools.ietf.org/html/rfc7118) ist der optimale Transport, aber andere Techniken wie Server-Sent Ereignisse und lange Abfragen können verwendet werden, wenn diese nicht zur Verfügung stehen. SignalR automatisch erkennt, und initialisieren den entsprechenden Transport basierend auf den Funktionen, die auf dem Client und Server unterstützt.

## <a name="hubs"></a>Hubs

SignalR verwendet Hubs für die Kommunikation zwischen Clients und Servern.

Ein Hub ist eine allgemeine Pipeline, die der Client-als auch voneinander Methoden aufrufen kann. SignalR übernimmt die Verteilung über Computergrenzen hinweg automatisch, sodass Clients zum Aufrufen von Methoden auf dem Server als einfach als lokalen Methoden (und umgekehrt). Hubs ermöglichen übergeben von Parametern stark typisierte Methoden, wodurch wurden die modellbindung. SignalR enthält zwei integrierte Hub-Protokolle: ein Text-Protokoll, anhand von JSON und einer binären Protokolls basierend auf [MessagePack](https://msgpack.org/).  MessagePack erstellt in der Regel über kleinere Nachrichten als bei der Verwendung von JSON. Ältere Browser unterstützen müssen [XHR Ebene 2](https://caniuse.com/#feat=xhr2) MessagePack-Protokoll unterstützen.

Senden von Nachrichten mithilfe der aktive Transport aufrufen Hubs clientseitigen Code. Die Nachrichten enthalten den Namen und die Parameter der Methode die clientseitige. Objekte, die als Methodenparameter gesendet werden mit dem konfigurierten Protokoll deserialisiert. Der Client versucht, mit dem Namen einer Methode im clientseitigen Code übereinstimmen. Wenn eine Übereinstimmung vorliegt, führt die Methode verwendet die deserialisierte Parameterdaten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Erste Schritte mit SignalR für ASP.NET Core](xref:signalr/get-started)
* [Unterstützte Plattformen](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [JavaScript-Client](xref:signalr/javascript-client)
