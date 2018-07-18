---
title: Einführung in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie die ASP.NET Core SignalR-Bibliothek vereinfacht das Hinzufügen von Echtzeitfunktionalität für apps.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095388"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Einführung in ASP.NET Core SignalR

Von [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Was ist SignalR?

ASP.NET Core SignalR handelt es sich um eine Bibliothek, die Hinzufügen von echtzeitwebfunktionalität zu apps vereinfacht. Echtzeit-Webfunktionen ermöglicht serverseitiger Code Inhalt per Pushvorgang an Clients sofort an.

Gute Kandidaten für SignalR:

* Apps, die eine hohe Frequenz von Updates auf dem Server erfordern. Beispiele sind Gaming, soziale Netzwerke, voting, Auktion, Karten und GPS-apps.
* Dashboards und überwachungs-apps. Beispiele sind Unternehmensdashboards, sofortupdates von Verkaufszahlen oder reisehinweise.
* Apps für die Zusammenarbeit. Whiteboard-apps und Software für teambesprechungen sind Beispiele von apps für die Zusammenarbeit.
* Apps, die Benachrichtigungen benötigt. Verwenden Benachrichtigungen, soziale Netzwerke, e-Mail, Chat, Games, reisehinweise und viele andere apps.

SignalR stellt eine API zum Erstellen von Server-zu-Client [Remoteprozeduraufrufe (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Die RPCs rufen JavaScript-Funktionen auf Clients von serverseitigen .NET Core-Code.

SignalR für ASP.NET Core:

* Verbindungsverwaltung behandelt automatisch.
* Ermöglicht das Senden von Nachrichten an alle verbundenen Clients gleichzeitig über. Z. B. einem Chatraum.
* Ermöglicht das Senden von Nachrichten an bestimmte Clients oder Clientgruppen.
* Ist auf Open Source [GitHub](https://github.com/aspnet/signalr).
* Skalierbar.

Die Verbindung zwischen Client und Server ist persistent, im Gegensatz zu einer HTTP-Verbindung.

## <a name="transports"></a>Transportprotokolle

SignalR-Abstracts über eine Reihe von Techniken zum Erstellen von Echtzeit-Webanwendungen. [WebSockets](https://tools.ietf.org/html/rfc7118) optimalen Transport, aber andere Techniken wie Server-Sent-Ereignisse "und" Long Polling können verwendet werden, wenn diese nicht zur Verfügung stehen. SignalR erkennt automatisch und initialisieren das richtige Transportverfahren basierend auf Funktionen, die auf dem Server und Client unterstützt.

## <a name="hubs"></a>Hubs

SignalR verwendet Hubs für die Kommunikation zwischen Clients und Servern.

Ein Hub ist eine allgemeine Pipeline, die sich Client und Server, zum Aufrufen von Methoden voneinander ermöglicht. SignalR übernimmt die Verteilung über Computergrenzen hinweg automatisch, sodass Clients zum Aufrufen von Methoden auf dem Server als einfach als lokalen Methoden (und umgekehrt). Hubs ermöglichen es übergeben von stark typisierten Parametern an Methoden, die modellbindung aktiviert. SignalR enthält zwei integrierte Hub-Protokolle: ein Text-Protokoll, anhand von JSON- und ein binäres Protokoll, das basierend auf [MessagePack](https://msgpack.org/).  MessagePack wird in der Regel kleinere Nachrichten als bei der Verwendung von JSON erstellt. Müssen ältere Browser unterstützen [XHR-Ebene 2](https://caniuse.com/#feat=xhr2) MessagePack protokollunterstützung bereitstellen.

Hubs Aufrufen der clientseitige Code Senden von Nachrichten mithilfe der aktive Transport. Die Nachrichten enthalten den Namen und Parameter, der eine clientseitige Methode. Objekte, die als Parameter gesendet werden deserialisiert das konfigurierte Protokoll verwenden. Der Client versucht, die mit dem Namen einer Methode im clientseitigen Code übereinstimmen. Wenn eine Übereinstimmung vorliegt, führt die Methode verwendet die deserialisierte Parameterdaten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Erste Schritte mit SignalR für ASP.NET Core](xref:tutorials/signalr)
* [Unterstützte Plattformen](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [JavaScript-Client](xref:signalr/javascript-client)
