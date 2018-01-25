---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Verteiltes Caching (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 24ede9cb9289c84140f6e2573f9d526f19cac64b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Verteiltes Caching (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Im Kapitel über das vorherige Behandlung vorübergehender Fehler betrachtet und erwähnt als Trennschalter Strategie Zwischenspeichern. Dieses Kapitel bietet weitere Hintergrundinformationen zum Zwischenspeichern, einschließlich der Verwendung, häufige Muster von verwenden, und wie Sie es in Azure zu implementieren.

## <a name="what-is-distributed-caching"></a>Was ist verteiltes caching

Ein Cache bietet hohen Durchsatz mit geringer Latenz Zugriff auf häufig verwendete Anwendung, durch das Speichern der Daten im Arbeitsspeicher. Für eine Cloud-app ist die nützlichste Typ des Caches verteilter Cache, d. h., die Daten werden nicht gespeichert werden, auf die einzelnen Webserver Arbeitsspeicher jedoch von anderen Cloudressourcen und die zwischengespeicherten Daten für alle Webserver einer Anwendung zur Verfügung gestellt werden (oder andere cloud-VMs, ar e wird von der Anwendung).

![Diagramm mit mehreren Webservern, die Zugriff auf den gleichen Cacheservern](distributed-caching/_static/image1.png)

Wenn die Anwendung skaliert, um hinzufügen oder Entfernen von Servern oder Server aufgrund eines Upgrades oder Fehlern ersetzt werden, bleibt die zwischengespeicherten Daten zugegriffen werden kann, auf jedem Server, der die Anwendung ausgeführt wird.

Durch das Vermeiden des hohen Latenz Datenzugriffs von einem persistenten Datenspeicher, kann die caching Reaktionsfähigkeit der Anwendung erheblich verbessern. Beispielsweise ist das Abrufen von Daten aus dem Cache wesentlich schneller, als sie aus einer relationalen Datenbank abrufen zu müssen.

Ein Seite Vorteil des Zwischenspeicherns wird reduziert Datenverkehr zum persistenten Datenspeicher, was möglicherweise zu niedrigeren Kosten führt, wenn ausgehende Daten vorhanden sind die Kosten für den persistenten Datenspeicher.

## <a name="when-to-use-distributed-caching"></a>Verwendung von verteiltes caching

Zwischenspeichern von funktioniert am besten für Anwendungsworkloads dazu weitere lesen als das Schreiben von Daten und wenn das Datenmodell die Schlüssel/Wert-Organisation, die Sie verwenden zum Speichern und Abrufen von Daten im Cache unterstützt. Es ist auch sinnvoller, wenn Anwendungsbenutzer viele allgemeine Daten freigeben; Beispielsweise würde Cache nicht so viele Vorteile bieten, wenn jeder Benutzer in der Regel nur für diesen Benutzer Daten abruft. Ein Beispiel, in dem caching sehr vorteilhaft sein könnte, ist ein Produktkatalog, da die Daten nicht häufig geändert, und alle Kunden die gleichen Daten betrachten.

Der Vorteil des Zwischenspeicherns wird zunehmend messbaren desto Skalierung eine Anwendung, Einschränkungen für Durchsatz und Latenz Verzögerungen bei der persistenten Datenspeicher der gesamtleistung der Anwendung eingeschränkt werden. Sie können jedoch implementieren, Zwischenspeichern von anderen Gründen als auch die Leistung. Für Daten, die nicht perfekt auf dem neuesten Stand, wenn dem Benutzer angezeigt werden, kann Cache Zugriff als ein Trennschalter für dienen, wenn es sich bei der persistenten Datenspeicher nicht mehr reagiert oder nicht verfügbar ist.

## <a name="popular-cache-population-strategies"></a>Beliebte Cache Auffüllung Strategien

Damit Daten aus dem Cache abgerufen werden können, müssen Sie es zuerst speichern. Es gibt mehrere Strategien zum Abrufen von Daten, die Sie benötigen in einem Cache:

- Bei Bedarf / Cache-Aside

    Die Anwendung versucht, Daten aus dem Cache abzurufen, und wenn der Cache nicht über die Daten (ein "Miss") verfügt, die Anwendung speichert die Daten im Cache, damit er das nächste Mal zur Verfügung stehen. Das nächste Mal, das die Anwendung versucht, dieselben Daten abzurufen, feststellt, im Cache (als "Treffer") wonach für. Um zu verhindern, Abrufen von zwischengespeicherten Daten, die geändert wurden für die Datenbank, für ungültig zu erklären Sie den Cache beim vornehmen von Änderungen im Datenspeicher.
- Hintergrund Datenpush

    Hintergrunddienste Verschieben von Daten in den Cache in regelmäßigen Abständen, und die app immer Wortlisten aus dem Cache. Dieser Ansatz funktioniert hervorragend mit hoher Latenz von Datenquellen, die Sie nicht immer erforderlich ist, geben die neuesten Daten zurück.
- Trennschalter

    Die Anwendung, die normalerweise direkt mit persistenten Datenspeicher kommuniziert werden, aber bei der persistenten Datenspeicher Verfügbarkeitsprobleme verfügt, die Anwendung die Daten aus dem Cache abruft. Daten können im Cache, die mit dem Cache reserviert oder Hintergrund-Push-Strategie gestellt wurden. Dies ist eine Fehlerbehandlung Strategie statt einer Strategie für die Leistung verbessern.

Um Daten im Cache aktuell zu halten, können Sie verwandte Cacheeinträge löschen, wenn Ihre Anwendung erstellt wird, Updates, Daten oder löscht. Ist er gut für die Anwendung, in einigen Fällen Daten abzurufen, die etwas veraltet ist, können Sie basieren auf einer konfigurierbaren Ablaufzeit wie ALT Cache festgelegt werden kann.

Sie können konfigurieren, absolute Ablaufzeit (Zeitraum seit dem Cacheelement, das erstellt wurde) oder eine gleitende Ablaufzeit (Menge an Zeit seit der letzten Ausführung ein Cacheelement zugegriffen wurde). Absoluter Ablauf wird verwendet, wenn Sie auf dem Cache Ablauf-Mechanismus, um zu verhindern, dass die Daten zu veralten abhängig sind. In der app korrigieren wir müssen manuell Entfernen veralteter Cacheelementen, und wir gleitende Ablaufzeit verwenden, um die aktuellsten Daten im Cache aktuell bleibt. Unabhängig von der Ablaufrichtlinie, die Sie auswählen, wird der Cache automatisch die ältesten (geringste zuletzt verwendete oder LRU) Elemente entfernen, wenn das Cache Speicher erreicht ist.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Cache-Aside-Beispielcode für die app korrigieren

Im folgenden Beispielcode überprüfen wir den Cache zunächst beim Abrufen eines Vorgangs zu beheben. Wenn der Task im Cache gefunden wird, geben wir zurück; Wenn es nicht gefunden, wir es aus der Datenbank abgerufen und im Cache zu speichern. Die Änderungen, die Sie zum Hinzufügen von caching zu vornehmen würden die `FindTaskByIdAsync` Methode werden hervorgehoben.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Beim Aktualisieren oder einer Aufgabe zu beheben löschen, müssen Sie (entfernen) die zwischengespeicherten Task für ungültig zu erklären. Andernfalls versucht Zukunft gelesen hat diese Aufgabe weiterhin die alten Daten aus dem Cache abzurufen.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Dabei handelt es sich um Beispiele zur Veranschaulichung einfachen caching-Codes. Zwischenspeichern wurde nicht in die herunterladbare korrigieren-Projekt implementiert.

## <a name="azure-caching-services"></a>Azure-zwischenspeicherdienste

Azure bietet die folgenden Cachingdienste: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) und [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis Cache basiert auf dem beliebten [open-Source-Redis-Cache](http://redis.io/) und ist die erste Wahl für die meisten zwischenspeicherszenarien.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET-Sitzungsstatus mit einem Cacheanbieter

Siehe die [Web Development best Practices Kapitel](web-development-best-practices.md), eine bewährte Methode wird zur Vermeidung des Sitzungsstatus. Wenn Ihre Anwendung den Sitzungsstatus erfordert, wird die nächste bewährte zu den in-Memory-Standardanbieter zu vermeiden, da die Dezentrales Skalieren (mehrere Instanzen des Webservers) ermöglichen nicht. Die ASP.NET SQL Server-Sitzungsstatusanbieter ermöglicht eine Website, die auf mehrere Webserver Sitzungsstatus verwendet ausgeführt wird, sondern im Vergleich zu einer in-Memory-Anbieter Kosten hoher Latenz verursacht. Die beste Lösung, wenn Sie die Sitzungszustand zu verwenden ist, in einen Cacheanbieter, z. B. die [Sitzungsstatusanbieter für Azure-Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Zusammenfassung

Sie haben gesehen, wie die app zu beheben, implementieren konnte zwischenspeichern, um die Antwortzeit und Skalierbarkeit zu verbessern, und aktivieren Sie die app aus, um anzugeben, dass für Lesevorgänge reagiert werden, wenn die Datenbank nicht verfügbar ist. In der [nächsten Kapitels](queue-centric-work-pattern.md) wir zeigen, wie weiter verbessern der Skalierbarkeit und die App reaktionsfähig für Schreibvorgänge werden weiterhin.

## <a name="resources"></a>Ressourcen

Weitere Informationen zum Zwischenspeichern finden Sie unter den folgenden Ressourcen.

Dokumentation

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx). Offizielle MSDN-Dokumentation für das caching in Azure.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Caching Guidance finden Sie in der Cache-Aside-Muster.
- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Caching finden Sie im Abschnitt.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Whitepaper von Mark Simms und Michael Thomassy. Finden Sie im Abschnitt für Verteiltes Zwischenspeichern.
- [Verteiltes Caching für den Pfad zur Skalierbarkeit](https://msdn.microsoft.com/magazine/dd942840.aspx). Eine ältere (2009) MSDN Magazine-Artikel, aber eine gut geschriebene Einführung in Verteiltes Zwischenspeichern im Allgemeinen; Wechselt in genauer als die Zwischenspeichern Abschnitte des Whitepapers FailSafe und Best Practices.

Videos

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Neun zweiteilige Reihe Marc Mercuri, Ulrich Homann und Mark Simms. Stellt eine Sicht 400-Ebene zum Cloud-apps zu entwickeln. Diese Reihe konzentriert sich auf der Theorie und Gründe, warum; Detaillierte Informationen zur Vorgehensweise finden Sie in das Erstellen von Big Reihe von Mark Simms. Finden Sie caching in Folge 3 beginnenden 1:24:14.
- [Erstellen von Big: Erkenntnisse aus Azure-Kunden – Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies erläutert verteilte caching beginnenden 46:00. Vergleichbar mit dem Failsafe-Serie, aber wechselt in den Gewusst-wie-Informationen. Die Präsentation wurde am 31. Oktober 2012 angegeben, damit er nicht zwischenspeichern-Dienst in Azure App Service-Web-Apps abdeckt, die in 2013 eingeführt wurde.

Codebeispiel

- [Grundlagen von Clouddiensten in Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Eine beispielanwendung, die implementiert Verteiltes Zwischenspeichern. Finden Sie im zugehörigen Blogbeitrag [Grundlagen von Clouddiensten – Grundlagen Caching](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

>[!div class="step-by-step"]
[Zurück](transient-fault-handling.md)
[Weiter](queue-centric-work-pattern.md)
