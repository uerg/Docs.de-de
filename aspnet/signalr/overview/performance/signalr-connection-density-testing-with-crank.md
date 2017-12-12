---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: "SignalR-Verbindung Dichte Testen mit tatsächlich | Microsoft Docs"
author: tfitzmac
description: "SignalR-Verbindung Dichte Testen mit tatsächlich"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2017
---
<a name="signalr-connection-density-testing-with-crank"></a>SignalR-Verbindung Dichte Testen mit tatsächlich
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie das kommt als Tool zum Testen einer Anwendung mit mehreren simulierten Clients.


Sobald die Anwendung in seiner Hostingumgebung (entweder eine Azure Webrolle, IIS oder selbstgehosteten mit Owin) ausgeführt wird, können Sie die Anwendung als Antwort auf eine hohe Verbindung Dichte mit dem Tool tatsächlich testen. Die Hostingumgebung kann ein Server Internet Information Services (IIS), eine Owin-Host oder ein Azure-Webrolle. (Hinweis: Leistungsindikatoren sind nicht auf Azure App Service Web-Apps verfügbar, damit Sie keine Dichte Verbindungstest Leistungsdaten entnommen werden können.)

Verbindung Dichte bezieht sich auf die Anzahl gleichzeitiger TCP-Verbindungen, die auf einem Server hergestellt werden kann. Jeder TCP-Verbindung eine eigene Rechenaufwand, und öffnen eine große Anzahl von Verbindungen im Leerlauf werden einen Speicherengpass schließlich zu erstellen.

[SignalR codebase](https://github.com/signalr/signalr) enthält ein Auslastungstests Tool namens **bereitstellen**. Die neueste Version der tatsächlich verwendbaren [der Verzweigung Dev](https://github.com/SignalR/signalr/tree/dev) auf GitHub. Sie können eine ZIP-Datei Archivieren von der Verzweigung Dev von SignalR codebase herunterladen [hier](https://github.com/SignalR/SignalR/archive/dev.zip).

Tatsächlich kann zum vollständig der Arbeitsspeicher des Servers stark beanspruchen verwendet werden, um die Gesamtzahl von Verbindungen im Leerlauf auf die Serverhardware möglich zu berechnen. Alternativ können Sie auch tatsächlich für den Auslastungstest den Server unter eine bestimmte Menge an Speicher aufweist, durch verwenden Verbindungen Planungsansicht, bis eine bestimmte Anzahl oder einen bestimmten speicherschwellenwert erreicht ist.

Beim Testen, ist es wichtig, um remote-Clients zu verwenden, um alle Wettbewerb um Ressourcen (z. B. TCP-Verbindungen und Arbeitsspeicher) zu vermeiden. Überwachen Sie die Clients, um sicherzustellen, dass sie Engpässe nicht aktiviert werden, die den Server erreichen der vollen Kapazität (Arbeitsspeicher oder CPU) verhindern. Sie müssen möglicherweise die Anzahl der Clients zu erhöhen, um vollständig vom Server geladen werden.

### <a name="running-a-connection-density-test"></a>Ausführen eines Tests der Verbindung Dichte

In diesem Abschnitt werden die Schritte zum Ausführen einer Verbindung Dichte-Tests auf einer SignalR-Anwendung beschrieben.

1. Herunterladen und erstellen Sie die [Verzweigung Dev von SignalR codebase](https://github.com/SignalR/SignalR/archive/dev.zip). Wechseln Sie in der Eingabeaufforderung zu &lt;Projektverzeichnis&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Bereitstellen Sie Ihre Anwendung in der vorgesehenen Hostingumgebung. Notieren Sie den Endpunkt, den von der Anwendung verwendeten; Beispielsweise ist in der Anwendung in der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), der Endpunkt ist `http://<yourhost>:8080/signalr`.
3. Installieren Sie [SignalR-Leistungsindikatoren](signalr-performance.md#perfcounters) auf dem Server. Wenn Ihre Anwendung in Azure ausgeführt wird, finden Sie unter [SignalR-Leistungsindikatoren in einer Azure-Webrolle mithilfe von](using-signalr-performance-counters-in-an-azure-web-role.md).

Sobald heruntergeladen und erstellt die Codebasis und Leistungsindikatoren auf dem Host installiert haben, kann das Befehlszeilentool "tatsächlich" gefunden werden, der `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` Ordner.

Optionen für das Tool tatsächlich sind verfügbar:

- **/?** : Zeigt die Hilfe an. Die verfügbaren Optionen werden auch angezeigt, wenn die **Url** Parameter ausgelassen wird.
- **/ Url**: die URL für SignalR-Verbindungen. Dieser Parameter ist erforderlich. Der Pfad endet bei Verwendung der standardzuordnung SignalR-Anwendungen "/ Signalr".
- **/ Transport**: der Name des Transports verwendet. Die Standardeinstellung ist `auto`, werden die verfügbare Protokoll auswählen. Zu den Optionen gehören `WebSockets`, `ServerSentEvents`, und `LongPolling` (`ForeverFrame` ist keine Option für tatsächlich, seit der .NET Client statt Internet Explorer verwendet wird). Weitere Informationen wie SignalR Transporte auswählt, finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: die Anzahl der Clients, die in jedem Batch hinzugefügt. Der Standardwert ist 50.
- **/ ConnectInterval**: das Intervall in Millisekunden zwischen dem Hinzufügen von Verbindungen. Der Standard ist 500.
- **/ Connections**: die Anzahl der Verbindungen verwendet, um die Anwendung Auslastungstest. Der Standardwert ist 100.000.
- **/ ConnectTimeout**: das Timeout in Sekunden, bevor der Test wird abgebrochen. Der Standardwert ist 300.
- **MinServerMBytes**: das Minimum (MB) erreicht. Der Standard ist 500.
- **SendBytes**: die Größe der Nutzlast in Bytes an den Server gesendet. Der Standard ist 0.
- **SendInterval**: die Verzögerung in Millisekunden zwischen Nachrichten an den Server. Der Standard ist 500.
- **SendTimeout**: das Timeout in Millisekunden für Nachrichten an den Server. Der Standardwert ist 300.
- **ControllerUrl**: die Url, in denen ein Client einen Controller-Hub gehostet wird. Der Standardwert ist null (kein Domänencontroller-Hub). Die Controller-Hub wird gestartet, wenn die Sitzung tatsächlich gestartet wird; keine weiteren erfolgt Kontakt zwischen dem Controller Hub und tatsächlich.
- **NumClients**: die Anzahl der simulierten Clients auf die Anwendung zugreifen. Der Standardwert ist 1.
- **LogFile**: der Dateiname für die Protokolldatei für den Testlauf. Die Standardeinstellung ist `crank.csv`.
- **SampleInterval**: die Zeit in Millisekunden zwischen Leistungsindikatorsamplings. Der Standard ist 1000.
- **SignalRInstance**: den Instanznamen für die Leistungsindikatoren auf dem Server. Die Standardeinstellung ist die Verwendung den Client-Verbindungsstatus.

### <a name="example"></a>Beispiel

Der folgende Befehl wird eine Site mit dem Testen `pfsignalr` in Azure, die eine Anwendung über Port 8080 mit einem Hub, mit dem Namen "ControllerHub" hostet, unter Verwendung von 100 Verbindungen.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
