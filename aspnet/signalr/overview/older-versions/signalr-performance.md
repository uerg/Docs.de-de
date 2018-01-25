---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR-Leistung (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: SignalR-Leistung
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a>SignalR-Leistung (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> Dieses Thema beschreibt, wie für entwerfen, messen und Verbessern der Leistung in einer SignalR-Anwendung.


Eine aktuelle Präsentation auf SignalR-Leistung und Skalierung finden Sie unter [im Web in Echtzeit mit ASP.NET SignalR-Skalierung](https://channel9.msdn.com/Events/Build/2013/3-502).

Dieses Thema enthält folgende Abschnitte:

- [Überlegungen zum Entwurf](#design)
- [Optimieren des SignalR-Servers für die Leistung](#tuning)
- [Problembehandlung bei Performanceproblemen](#troubleshooting)
- [Verwenden von SignalR-Leistungsindikatoren](#perfcounters)
- [Verwenden die übrigen Leistungsindikatoren](#othercounters)
- [Weitere Ressourcen](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Dieser Abschnitt beschreibt die Muster, die während des Entwurfs einer SignalR-Anwendung, um sicherzustellen, dass die Leistung nicht beeinträchtigt werden wird, generieren Sie unnötigen Netzwerkdatenverkehr implementiert werden können.

### <a name="throttling-message-frequency"></a>Einschränkung der Häufigkeit der Nachricht

Die meisten Anwendungen müssen nicht auch in eine Anwendung, die eine hohe Frequenz (z. B. eine Anwendung für den Echtzeit-Spiele zulassen) Nachrichten sendet, mehrere Nachrichten ein zweites zu senden. Um den Umfang des Datenverkehrs zu reduzieren, die jedem Client generiert, eine Nachrichtenschleife, Warteschlangen und sendet Nachrichten nicht mehr als einer festen Rate häufig implementiert werden kann (d. h. bis zu einer bestimmten Anzahl von Nachrichten wird gesendet, pro Sekunde, wenn in diesem Zeitraum in Nachrichten vorhanden sind Terval gesendet werden). Eine beispielanwendung, die Nachrichten zu einer bestimmten Rate (von Client und Server) schränkt, finden Sie unter [hochfrequente Realtime mit SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Verringern der Größe

Sie können die Größe einer SignalR-Nachricht durch Reduzierung der Größe der serialisierten Objekte reduzieren. Im Servercode, wenn Sie ein Objekt gesendet, die Eigenschaften, die nicht enthält übertragen werden müssen, verhindern, dass diese Eigenschaften serialisiert wird, mithilfe der `JsonIgnore` Attribut. Die Namen von Eigenschaften werden auch in der Nachricht gespeichert. die Namen von Eigenschaften über verkürzt die `JsonProperty` Attribut. Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft ausschließen, die an den Client gesendet werden, und wie Eigenschaftennamen zu verkürzen:

**.NET Servercode, die das JsonIgnore-Attribut, um Daten auszuschließen, die an den Client gesendet werden und JsonProperty Attribut zum Reduzieren der Größe der Nachricht veranschaulicht**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Um die Lesbarkeit beibehalten / bessere Verwaltbarkeit im Clientcode die abgekürzten Namen kann neu zugeordnete auf einem von Menschen benutzerfreundliche Namen nach dem Empfang der Nachricht. Das folgende Codebeispiel veranschaulicht eine Möglichkeit der neuzuordnung verkürzter Namen länger Vorgängen, durch die Definition eines Nachrichtenvertrags (Zuordnung), und Verwenden der `reMap` Funktion, die den Vertrag der optimierte Nachrichtenklasse angewendet:

**Ordnet die clientseitige-JavaScript-Code verkürzt Eigenschaftennamen, die einem von Menschen lesbaren Namen**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Namen können in Nachrichten vom Client an den Server auch verkürzt werden mit derselben Methode.

Reduzierung des Platzbedarfs Speicher (d. h. die Menge des Arbeitsspeichers, die für die Nachricht verwendet) der Nachricht Objekt kann auch die Leistung verbessern. Angenommen, wenn das gesamte Spektrum der ein `int` ist nicht erforderlich, eine `short` oder `byte` kann stattdessen verwendet werden.

Da Nachrichten im Nachrichtenbus im Arbeitsspeicher des Servers, reduzieren die Größe der Nachrichten gespeichert werden können Sie auch Adresse Server von Arbeitsspeicherproblemen.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Optimieren des SignalR-Servers für die Leistung

Die folgenden Konfigurationseinstellungen können verwendet werden, um den Server für eine bessere Leistung in einer SignalR-Anwendung zu optimieren. Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter [verbessern die Leistung von ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**SignalR-Konfigurationseinstellungen**

- **DefaultMessageBufferSize**: Standardmäßig werden SignalR 1000 Nachrichten im Arbeitsspeicher pro Hub pro Verbindung beibehalten. Wenn große Nachrichten verwendet werden, kann dies Speicherprobleme zu erstellen, die behoben werden können, durch Verringern dieses Werts. Diese Einstellung kann festgelegt werden, der `Application_Start` -Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration` Methode einer OWIN-Start-Klasse in einer selbst gehosteten Anwendung. Im folgende Beispiel wird veranschaulicht, wie Sie diesen Wert verringern, um Ihrer Anwendung, um die Menge des belegten Server zu reduzieren den Speicherbedarf zu reduzieren:

    **.NET Servercode in Global.asax zum Verringern der Größe des Nachrichtenpuffers Standard**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS-Konfigurationseinstellungen**

- **Max. Anzahl gleichzeitiger Anforderungen pro Anwendung**: die Anzahl der gleichzeitigen IIS Anforderungen steigt auch die Serverressourcen verfügbar sind, Anforderungen zu verarbeiten. Der Standardwert ist 5000. Um diese Einstellung erhöhen möchten, führen Sie die folgenden Befehle in einem Eingabeaufforderungsfenster mit erhöhten Rechten aus:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Konfiguration der ASP.NET-Einstellungen**

Dieser Abschnitt enthält Konfigurationseinstellungen, die festgelegt werden können, in der `aspnet.config` Datei. Diese Datei befindet sich in einem von zwei Speicherorten, je nach Plattform:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Die folgenden: ASP.NET-Einstellungen, die SignalR-Leistung verbessern können

- **Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: erhöhen diese Einstellung möglicherweise Leistungsengpässe verringern. Um diese Einstellung erhöhen möchten, fügen Sie die folgende Konfigurationseinstellung der `aspnet.config` Datei:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Anfordern der Begrenzung der Warteschlange**: Wenn die Gesamtzahl der Verbindungen überschreitet die `maxConcurrentRequestsPerCPU` festlegen, ASP.NET startet Einschränkung Anforderungen, die über eine Warteschlange. Um die Größe der Warteschlange zu erhöhen, erhöhen Sie die `requestQueueLimit` Einstellung. Fügen Sie hierzu die folgende Konfigurationseinstellung der `processModel` Knoten `config/machine.config` (statt `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Problembehandlung bei Performanceproblemen

Dieser Abschnitt beschreibt die Möglichkeiten, um Leistungsengpässe in der Anwendung zu finden.

### <a name="verifying-that-websocket-is-being-used"></a>Überprüfen, dass ein WebSocket verwendet wird

Obgleich SignalR eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, WebSocket bietet einen deutlichen Leistungsvorteil und sollte verwendet werden, wenn Client und Server unterstützen. Um festzustellen, ob der Client-als auch die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports). Um zu bestimmen, welcher Transport in Ihrer Anwendung verwendet wird, können Sie die Browser-Entwicklertools verwenden, und überprüfen die Protokolle, um festzustellen, welcher Transport für die Verbindung verwendet wird. Informationen zur Verwendung der Browser-Entwicklungstools in Internet Explorer und Chrome finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Verwenden von SignalR-Leistungsindikatoren

In diesem Abschnitt wird beschrieben, wie aktivieren und Verwenden von SignalR-Leistungsindikatoren finden der `Microsoft.AspNet.SignalR.Utils` Paket.

### <a name="installing-signalrexe"></a>Installieren von signalr.exe

Leistung-Leistungsindikatoren können mit dem Server mithilfe eines Dienstprogramms namens SignalR.exe hinzugefügt werden. Um dieses Programm zu installieren, gehen Sie folgendermaßen vor:

1. Wählen Sie in der Visual Studio-Anwendung **Tools**, **Bibliothekspaket-Manager**, **NuGet-Pakete für Projektmappe verwalten...**
2. Suchen Sie nach **signalr.utils**, und wählen Sie installieren.

    ![](signalr-performance/_static/image1.png)
3. Stimmen Sie dem Lizenzvertrag zum Installieren des Pakets an.
4. SignalR.exe installiert werden soll `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Installieren von Leistungsindikatoren mit SignalR.exe

Führen Sie zum Installieren von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Führen Sie zum Entfernen von SignalR-Leistungsindikatoren SignalR.exe in einem Eingabeaufforderungsfenster mit erhöhten Rechten, mit dem folgenden Parameter:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR-Leistungsindikatoren

Das Dienstprogramme installiert der folgenden Leistungsindikatoren. Die Leistungsindikatoren "Total" messen die Anzahl der Ereignisse an, seit der letzten Anwendungspool oder den Server neu starten.

**Verbindung Metriken**

Die folgenden Metriken messen die Lebensdauer-Verbindungsereignisse, die auftreten. Weitere Informationen finden Sie unter [verstehen und Behandeln von Verbindungsereignisse Lebensdauer](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Verbindungen verbunden**
- **Verbindungen, die die Verbindung wiederhergestellt**
- **Verbindungen Disonnected**
- **Aktuelle Verbindungen**

**Nachrichtenmetrik**

Die folgenden Metriken messen dem Volumen des Nachrichtenverkehrs von SignalR generiert.

- **Gesamtanzahl der empfangenen Nachrichten Verbindung**
- **Gesamtanzahl der gesendeten Nachrichten Verbindung**
- **Verbindung empfangene Nachrichten/s**
- **Verbindung gesendete Nachrichten/Sek.**

**Bus Nachrichtenmetrik**

Die folgenden Metriken messen Datenverkehr über den internen SignalR-Nachrichtenbus der Warteschlange, in der alle eingehende und ausgehende SignalR-Nachrichten eingefügt werden. Eine Nachricht ist **veröffentlichte** wann sie gesendet oder übertragen. Ein **Abonnenten** in diesem Kontext wird ein Abonnement für den Nachrichtenbus; Dies sollte die Anzahl der Clients und dem Server selbst entsprechen. Ein **reservierten Worker** ist eine Komponente, die Daten an die aktiven Verbindungen, sendet eine **ausgelasteten Worker** ist eine, die aktiv eine Nachricht sendet.

- **Nachrichtenbus Nachrichten empfangene gesamt**
- **Nachrichtenbus Nachrichten empfangen/Sekunde**
- **Nachricht Bus Nachrichten veröffentlicht gesamt**
- **Nachrichtenbus Nachrichten veröffentlicht/Sekunde**
- **Nachricht Bus Abonnenten aktuelle**
- **Bus Abonnenten gesamt**
- **Nachricht Bus Abonnenten/Sek.**
- **Nachrichtenbus reservierten Worker**
- **Nachricht Bus ausgelasteter Worker**
- **Nachricht Bus Themen aktuelle**

**Fehler-Metriken**

Die folgenden Metriken Messen von SignalR Volumen des Nachrichtenverkehrs generierten Fehler. **Hub-Auflösung** Fehler auftreten, wenn Sie einen Hub oder hubmethode nicht aufgelöst werden kann. **Hubaufruf** Fehler sind beim Aufrufen einer hubmethode ausgelösten Ausnahmen. **Transport** Fehler sind Verbindungsfehler während einer HTTP-Anforderung oder Antwort ausgelöst.

- **Fehler: Alle gesamt**
- **Fehler: Alle/Sek.**
- **Fehler: Gesamtanzahl der Hub-Lösung**
- **Fehler: Hub Auflösung/Sekunde**
- **Fehler: Gesamtanzahl der Hub-Aufruf**
- **Fehler: Hub Aufruf/Sekunde**
- **Fehler: Transport gesamt**
- **Fehler: Transport/Sek.**

**Metriken mit horizontaler Skalierung**

Die folgenden Metriken Measuregruppen Datenverkehr und Fehler, die vom Anbieter mit horizontaler Skalierung. Ein **Stream** in diesem Kontext ist eine Skalierungseinheit vom Anbieter mit horizontaler Skalierung verwendet; dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird und ein Abonnement, wenn Redis verwendet wird. Standardmäßig wird nur ein Datenstrom verwendet, aber dies kann über die Konfiguration für SQL Server und Service Bus erhöht werden. Ein **Pufferung** ist eine, die einen Fehlerzustand eingegeben hat; wird der Stream in den Fehlerzustand, alle Nachrichten, die an die Backplane gesendet werden erst, wenn sofort nicht mehr Fault. Die **Länge der Sendewarteschlange** ist die Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.

- **Nachrichtenbus mit horizontaler Skalierung Nachrichten empfangen/Sekunde**
- **Warteschlangen für horizontale Skalierung Streams gesamt**
- **Warteschlangen für horizontale Skalierung streamt öffnen**
- **Warteschlangen für horizontale Skalierung Streams Pufferung**
- **Gesamtanzahl der Fehler mit horizontaler Skalierung**
- **Warteschlangen für horizontale Skalierung/Sek.**
- **Länge der Sendewarteschlange für horizontale Skalierung**

Weitere Informationen, was diese Indikatoren gemessen werden, finden Sie unter [SignalR Scaleout mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Verwenden die übrigen Leistungsindikatoren

Die folgenden Leistungsindikatoren möglicherweise auch bei der Überwachung der Leistung Ihrer Anwendung nützlich.

**Arbeitsspeicher**

- .NET CLR Memory-Bytes in allen Heaps (für w3wp)

**ASP.NET**

- Aktuelle "ASP.net\aktuelle Anforderungen"
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Information\Processor Prozessorzeit

**TCP/IP**

- TCPv6/hergestellten Verbindungen.
- TCPv4/hergestellten Verbindungen.

**Webdienst**

- Web-Dienst\Aktuelle Verbindungen
- Service\Maximum Webverbindungen

**Threading**

- .NET CLR-Sperren\# aktuelle logische Threads
- .NET CLR LocksAnd Threads\# aktuelle physische Threads

<a id="otherresources"></a>

## <a name="other-resources"></a>Weitere Ressourcen

Weitere Informationen zu ASP.NET-Leistung überwachen und optimieren finden Sie unter den folgenden Themen:

- [ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.NET Thread Usage on IIS 7.5, IIS 7.0 und IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;ApplicationPool&gt; Element (Webeinstellungen)](https://msdn.microsoft.com/library/dd560842.aspx)
