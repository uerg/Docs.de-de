---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Verteilte Zwischenspeicherung (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 1f67c754fcc03462c25983a8397a1093997b1213
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835617"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Verteilte Zwischenspeicherung (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Im vorherige Kapitel behandeln vorübergehender Fehler betrachtet und erwähnt als Strategie zur Circuit-Breaker Zwischenspeichern. Dieses Kapitel bietet weitere Hintergrundinformationen zur Zwischenspeicherung, einschließlich der Verwendung, allgemeine Muster für die Verwendung von, und wie Sie es in Azure implementieren.

## <a name="what-is-distributed-caching"></a>Was ist die verteilte Zwischenspeicherung

Ein Cache bietet hohen Durchsatz, niedrige Latenzen beim Zugriff auf häufig verwendete Anwendungsdaten, durch Speicherung der Daten im Arbeitsspeicher. Für eine Cloud-app ist die nützlichste Typ des Caches verteilten Cache, d. h., die Daten werden nicht gespeichert werden, auf die einzelnen Webserver Speicher, sondern auf anderen Cloudressourcen, und die zwischengespeicherten Daten für alle von Webservern der Anwendung verfügbar gemacht werden (oder anderen cloud-VMs, ar e wird von der Anwendung).

![Diagramm mit mehreren Webservern, die Zugriff auf den gleichen Cacheservern](distributed-caching/_static/image1.png)

Wenn die Anwendung durch Hinzufügen oder Entfernen von Servern skaliert werden kann, oder Server aufgrund eines Upgrades oder Fehlern ersetzt werden, bleibt die zwischengespeicherten Daten zugegriffen werden kann, auf jedem Server, der die Anwendung ausgeführt wird.

Durch das Vermeiden des hohen Latenz Datenzugriff von einem persistenten Datenspeicher, kann die Zwischenspeicherung Reaktionsfähigkeit von Anwendungen drastisch verbessern. Beispielsweise ist das Abrufen von Daten aus dem Cache viel schneller als das Abrufen aus einer relationalen Datenbank.

Ein Nebeneffekt des Cachings wird reduziert, Datenverkehr an den persistenten Datenspeicher, der niedrigeren Kosten entstehen, wenn ausgehende Daten vorhanden sind Gebühren für den persistenten Datenspeicher.

## <a name="when-to-use-distributed-caching"></a>Wenn für das verteilte caching

Funktionsweise der Zwischenspeicherung am besten für Workloads von Anwendungen dazu weitere Informationen als das Schreiben von Daten, und wenn das Datenmodell unterstützt die Schlüssel/Wert-Organisation, die Sie verwenden, um Daten im Cache speichern und abrufen. Es ist auch sinnvoller, wenn Benutzer viele allgemeine Daten freigeben; Cache würde z. B. nicht so viele Vorteile bieten, wenn jeder Benutzer in der Regel Daten, die für diesen Benutzer eindeutig abruft. Ein Beispiel, in denen Zwischenspeicherung sehr vorteilhaft sein könnte, ist einem Produktkatalog, da die Daten nicht häufig ändert, und alle die gleichen Daten Kunden.

Der Vorteil der Zwischenspeicherung wird zunehmend messbare hochskaliert wird je eine Anwendung aus, den Grenzwerten für den Durchsatz und Latenz Verzögerungen bei der persistenten Datenspeicher, die mehrere der gesamtleistung der Anwendung eingeschränkt werden. Sie können jedoch implementieren, Zwischenspeichern aus anderen Gründen als auch die Leistung. Für Daten, die nicht perfekt auf dem neuesten Stand bei dem Benutzer angezeigt werden, dienen erfolgt ein Trennschalter für beim persistenten Datenspeicher nicht mehr reagiert oder nicht verfügbar ist.

## <a name="popular-cache-population-strategies"></a>Strategien für die Auffüllung von beliebten cache

Um Daten aus Cache abrufen können, müssen Sie es zuerst speichern. Es gibt verschiedene Strategien zum Abrufen von Daten, die Sie benötigen in einen Cache:

- Bei Bedarf / Cache-Aside

    Die Anwendung versucht, Daten aus Cache abrufen, und wenn der Cache nicht über die Daten (ein "Miss") verfügt, die Anwendung speichert die Daten im Cache, damit es das nächste Mal zur Verfügung stehen. Das nächste Mal, das die Anwendung versucht, die die gleichen Daten abrufen, sucht es, was er im Cache (ein "Treffer") gesucht wird. Um zu verhindern, Abrufen von zwischengespeicherten Daten, die geändert wurde für die Datenbank, wird der Cache ungültig, wenn Änderungen an den Datenspeicher.
- Hintergrund-Daten per Push

    Hintergrunddienste senden Daten in den Cache in regelmäßigen Abständen, und die app immer Pullvorgänge aus dem Cache. Dieser Ansatz funktioniert hervorragend mit hoher Latenz von Datenquellen, die Sie nicht immer benötigen zurück, die neuesten Daten.
- Trennschalter

    Die Anwendung normalerweise kommuniziert wird, direkt mit der persistenten Datenspeicher, aber wenn persistenten Datenspeicher Verfügbarkeitsproblemen verfügt, die Anwendung die Daten aus dem Cache abruft. Daten können im Cache, die mit dem Cache reserviert oder Hintergrund Strategie zum Übertragen von Daten gestellt wurden. Dies ist eine Fehlerbehandlung Strategie statt einer Strategie für die Verbesserung der Leistung.

Um die Daten im Cache aktuell zu halten, können Sie verwandte Cacheeinträge löschen, wenn Ihre Anwendung, aktualisiert erstellt, oder Löschen von Daten. Wenn es sich gut ist, die für Ihre Anwendung, die gelegentlich Daten abrufen, die etwas veraltet ist, können Sie verlassen auf eine konfigurierbare Ablaufzeit für einen Grenzwert für Alter Cache festgelegt werden kann.

Sie können die absolute Ablaufzeit (Zeitspanne, da das Element im Cache erstellt wurde) oder die gleitende Ablaufzeit (Zeitspanne seit dem letzten, ein Element im Cache zugegriffen wurde) konfigurieren. Absoluter Ablauf wird verwendet, wenn Sie von den Mechanismus zur Zwischenspeicherung Ablauf zu verhindern, dass die Daten zu sehr veraltet abhängig sind. Klicken Sie in der Fix It-app wir werden veraltete Cacheelemente manuell entfernen, und gleitende Ablaufzeit verwenden wir die aktuellsten Daten im Cache beibehalten. Unabhängig von der Ablaufrichtlinie, die Sie auswählen, wird der Cache automatisch die ältesten (zuletzt verwendete mindestens oder LRU) Elemente entfernen, wenn der Cache Speicher-Grenze erreicht wird.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Cache-Aside-Beispielcode für Fix It-app

Im folgenden Beispielcode prüfen Sie den Cache zunächst beim Abrufen einer Aufgabe zu beheben. Wenn die Aufgabe im Cache gefunden wird, geben wir zurück; Wenn keine gefunden, wir rufen es aus der Datenbank und in den Cache zu speichern. Die Änderungen, die Sie zum Hinzufügen von caching zu vornehmen würde die `FindTaskByIdAsync` Methode werden hervorgehoben.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Wenn Sie aktualisieren oder einer Aufgabe zu beheben löschen, müssen Sie (entfernen) den zwischengespeicherten Task für ungültig zu erklären. Andernfalls versucht zukünftige, lesen Sie, dass diese Aufgabe weiterhin die alten Daten aus dem Cache abzurufen.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Hierbei handelt es sich um Beispiele zur Veranschaulichung von einfachen Codes zur Zwischenspeicherung; Zwischenspeichern wurde nicht in den herunterladbaren Fix It-Projekt implementiert.

## <a name="azure-caching-services"></a>Azure-Cachedienste

Azure bietet die folgenden Dienste für die Zwischenspeicherung: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) und [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis Cache basiert auf dem beliebten [open-Source-Redis-Cache](http://redis.io/) und ist die Zwischenspeicherung zur ersten Wahl für die meisten Szenarien.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET-Sitzungszustand mit einem Cacheanbieter

Siehe die [Web Development best Practices Kapitel](web-development-best-practices.md), eine bewährte Methode ist, um zu vermeiden, mithilfe des Sitzungszustands. Wenn Ihre Anwendung den Sitzungsstatus erfordert, werden die nächsten bewährte Methode ab, den Standardanbieter für in-Memory zu vermeiden, da Sie nicht, die Scale out-(mehrere Instanzen des Webservers) ermöglicht. Die ASP.NET SQL Server-Sitzungszustandsanbieter ermöglicht, eine Website, die auf mehreren Webservern, um den Sitzungsstatus verwendet ausgeführt wird, aber verursacht eine hohe Latenz Kosten im Vergleich zu einer in-Memory-Anbieter. Die beste Lösung, wenn Sie den Sitzungszustand verwenden müssen, verwenden Sie einen Cacheanbieter, wie z. B. ist der [Session State Provider für Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Zusammenfassung

Sie haben gesehen, wie die Fix It-app implementieren kann zwischenspeichern, um die Antwortzeit und die Skalierbarkeit zu verbessern, und aktivieren Sie die app aus, um anzugeben, dass die Reaktionsfähigkeit bei Lesevorgängen werden, wenn die Datenbank nicht verfügbar ist. In der [im nächsten Kapitel](queue-centric-work-pattern.md) wir zeigen, wie weiter verbessern der Skalierbarkeit und die App weiterhin reaktionsfähig für Schreibvorgänge.

## <a name="resources"></a>Ressourcen

Weitere Informationen zum Zwischenspeichern finden Sie unter den folgenden Ressourcen.

Dokumentation

- [Azure-Cache](https://msdn.microsoft.com/library/gg278356.aspx). Offizielle MSDN-Dokumentation zur Zwischenspeicherung in Azure.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Caching Guidance "und" Cache-Aside-Muster angezeigt.
- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Caching finden Sie im Abschnitt.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Whitepaper von Mark Simms und Michael Thomassy. Finden Sie im Abschnitt zum verteilten Zwischenspeichern.
- [Verteilte Zwischenspeicherung auf dem Weg zur Skalierbarkeit](https://msdn.microsoft.com/magazine/dd942840.aspx). Ein älterer Artikel in MSDN Magazine (2009), aber eine klar formulierte Einführung in die verteilte Zwischenspeicherung im Allgemeinen; genauer als die Zwischenspeicherung Abschnitte der die ausfallsichere und Best Practices-Whitepaper wird.

Videos

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Teil 9-Reihe von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet einen Überblick über 400 auf Serverebene wie Sie Cloud-apps zu gestalten. Diese Serie konzentriert sich auf die Theorie und Gründe, warum; Weitere Informationen zur Vorgehensweise finden Sie in der Erstellung großer von Mark Simms. Finden Sie unter den Ausführungen zur Zwischenspeicherung in Folge 3 beginnend mit 1:24:14.
- [Erstellen von Big: Erfahrungen aus dem Azure-Kunden – Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies wird die verteilte Zwischenspeicherung ab 46:00 erläutert. Ähnlich wie die Failsafe-Serie, aber wird auf Weitere Gewusst-wie-Details. Die Präsentation wurde 31. Oktober 2012 angegeben werden, damit es nicht zwischenspeichern-Dienst der Web-Apps in Azure App Service behandelt wird, die in 2013 eingeführt wurde.

Codebeispiel

- [Clouddienstgrundlagen in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Eine beispielanwendung, die verteilte Zwischenspeicherung implementiert. Finden Sie im zugehörigen Blogbeitrag [Grundlagen der Cloud-Dienst – Grundlagen zwischenspeichern](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Zurück](transient-fault-handling.md)
> [Weiter](queue-centric-work-pattern.md)
