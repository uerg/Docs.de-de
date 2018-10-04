---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Daten, die Partitionierungsstrategien (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 07a80767ca2def26c0252037a3b8b990abcbe1b3
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578483"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Daten, die Partitionierungsstrategien (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen über die Reihe finden Sie unter [im ersten Kapitel](introduction.md).


Zuvor haben wir gesehen, wie einfach es ist die Webebene, einer Cloudanwendung, skalieren durch Hinzufügen und Entfernen von Webservern. Aber wenn sie alle den gleichen Datenspeicher auftreten, Ihrer Anwendung Engpass verschiebt aus dem Front-End an das Back-End und die Datenebene wird am schwersten zu skalieren. In diesem Kapitel erläutert wie Sie Ihre Datenebene skalierbare durch das Partitionieren von Daten auf mehrere relationale Datenbanken oder durch Kombinieren der relationalen Datenbank-Speicher mit anderen Optionen für die datenspeicherung vornehmen können.

Das Einrichten eines Partitionierungsschemas ist am besten fertig vorab aus demselben Grund erwähnt: Es ist sehr schwierig, Ihre Strategie für Datenspeicher zu ändern, nachdem eine app in der Produktion ist. Wenn Sie schwer vorab über verschiedene Vorgehensweisen vorstellen, können Sie vermeiden, "Twitter gleich" Wenn Ihre app abstürzt oder ausfällt einen längeren Zeitraum, während Sie Ihre app Daten und den Datenzugriffscode neu anordnen müssen.

## <a name="the-three-vs-of-data-storage"></a>Die drei Vs für die datenspeicherung

Um festzustellen, ob Sie benötigen eine Partitionierungsstrategie besteht und wie es sein soll, können Sie in der drei Fragen zu Ihren Daten:

- Volume: wie viele Daten Sie werden letztendlich zu speichern? Ein paar Gigabyte? Ein paar Hundert Gigabytes? Terabyte? Im Petabyte-Bereich?
- Geschwindigkeit: wie sieht die Rate, mit der Ihre Daten wächst? Handelt es sich um eine interne app, die große Datenmengen generieren, ist nicht? Eine externe app, der Kunden Bilder und Videos in hochladen?
- Verschiedene – welche Art von Daten wird Sie gespeichert werden? Relationale, Bilder, Schlüssel-Wert-Paare, soziale Graphen?

Wenn Sie, dass Sie also viele Volumen, Geschwindigkeit oder Reihe haben glauben, müssen Sie überlegen, welche Art von Partitionierungsschema, das am besten kann, dass Ihre app effizient und effektiv skalieren, während diese wächst, und um sicherzustellen, dass Sie alle Engpässe auftreten nicht.

Es gibt im Wesentlichen drei Ansätze für die Partitionierung:

- Vertikale Partitionierung
- Horizontale Partitionierung
- Hybridpartitionierung

## <a name="vertical-partitioning"></a>Vertikale Partitionierung

Vertikale Portionierung ist, wie eine Tabelle nach Spalten aufteilen: einen Satz von Spalten in einem Datenspeicher, und einen anderen Satz von Spalten in einem anderen Datenspeicher wird.

Nehmen wir beispielsweise an, dass meine app Daten zu Personen, einschließlich Bilder gespeichert:

![Datentabelle](data-partitioning-strategies/_static/image1.png)

Wenn Sie diese Daten als Tabelle darstellen, und sehen Sie sich die verschiedenen Arten von Daten, können Sie sehen, dass die drei Spalten auf der linken Seite Zeichenfolgendaten, die von einer relationalen Datenbank effizient gespeichert werden können, während die beiden Spalten auf der rechten Seite im Wesentlichen die Byte-Arrays enthalten, c Ome aus Bilddateien. Es ist möglich, Storage-Image-Dateidaten in einer relationalen Datenbank, und viele Leute ausführen, da sie nicht möchten, um die Daten im Dateisystem zu speichern. Sie möglicherweise nicht zu einem Dateisystem Speichern der erforderlichen Menge der Daten oder möchte er möglicherweise nicht verwalten eine separate Sicherung und Wiederherstellung von System. Dieser Ansatz funktioniert auch für lokale Datenbanken und für kleine Mengen von Daten in der Cloud-Datenbanken. In der lokalen Umgebung kann es einfacher, lassen Sie nur den Datenbankadministrator alles sein.

Aber in einer Clouddatenbank, Storage relativ aufwendig ist, und eine große Anzahl von Images kann die Größe der Datenbank, die über die Grenzwerte hinaus vergrößert werden an dem sie effizient arbeiten kann. Sie können diese Probleme beheben, indem Sie die Partitionierung der Daten senkrecht an, was bedeutet, dass Sie am besten geeigneten Datenspeicher für jede Spalte in der Tabelle von Daten auswählen. Was am besten geeignet, in diesem Beispiel sind möglicherweise besteht, die Zeichenfolgendaten in einer relationalen Datenbank und die Bilder im Blob-Speicher.

![Vertikal partitionierte Datentabelle](data-partitioning-strategies/_static/image2.png)

Speichern von Bildern im Blob-Speicher anstelle einer Datenbank ist praktischer, in der Cloud als in einer lokalen Umgebung, da Sie keine Gedanken um Dateiserver einrichten oder Verwalten von Sicherung und Wiederherstellung von Daten, die außerhalb der relationalen Datenbank gespeichert: alle die für Sie automatisch vom BLOB-Speicherdienst erfolgt.

Dies ist die partitionierungsansatz wir in der Fix It-app implementiert, und betrachten wir den Code, in der [BLOB-Speicher Kapitel](unstructured-blob-storage.md). Ohne dieses Partitionierungsschema und eine durchschnittliche Image-Größe von 3 MB angenommen wird wäre die Fix It-app nur zum Speichern von ungefähr 40.000 Aufgaben vor dem Erreichen der maximalen Datenbankgröße von 150 GB möglich. Nach dem Entfernen der Images, kann die Datenbank 10 Mal so viele Aufgaben speichern. Sie können wesentlich länger wechseln, bevor Sie einen Schema mit horizontalen Partitionierung implementieren müssen. Und wie die app skaliert werden kann, wird Ihre Ausgaben langsamer wachsen, da der Großteil Ihres Speicherbedarfs sind im Begriff im Blob-Speicher mit wenig Aufwand.

## <a name="horizontal-partitioning-sharding"></a>Horizontale Partitionierung (Sharding)

Horizontale Portionierung ist, wie eine Tabelle das Aufteilen von Zeilen: einen Satz von Zeilen in einem Datenspeicher, und einen anderen Satz von Zeilen in einem anderen Datenspeicher wird.

Wenn den gleichen Satz von Daten, wäre eine andere Möglichkeit zum Speichern von verschiedenen Bereichen der Namen der Kunden in unterschiedlichen Datenbanken.

![Datentabelle, die horizontal partitioniert.](data-partitioning-strategies/_static/image3.png)

Sie möchten Ihr shardingschema, um sicherzustellen, dass die Daten gleichmäßig verteilt werden, um Hotspots zu vermeiden, sehr vorsichtig sein. In diesem einfache Beispiel mit dem ersten Buchstaben des Nachnamens erfüllt nicht die diese Anforderung, da eine Menge Leute haben Nachnamen, die mit bestimmten allgemeine Buchstaben beginnen. Sie würden größenbeschränkungen für die Tabelle vor zu erwarten, da einige Datenbanken sehr große erhalten würde, während die meisten kleinen bleiben würden erreichen.

Eine Seite nach unten, der horizontalen Partitionierung ist, dass es möglicherweise schwierig, die Abfragen über alle Daten ausgeführt. In diesem Beispiel müsste eine Abfrage aus bis zu 26 unterschiedlichen Datenbanken zum Abrufen aller von der app gespeicherten Daten gezeichnet werden soll.

## <a name="hybrid-partitioning"></a>Hybridpartitionierung

Sie können das vertikale und horizontale Partitionierung kombinieren. Beispielsweise können Sie in den Beispieldaten speichern die Bilder im Blob-Speicher und horizontal partitioniert die Zeichenfolgendaten.

![Data Table Hybrid partitioniert](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partitionierung der Anwendung in einer produktionsumgebung

Vom Konzept her ist es leicht zu sehen, wie ein Partitionierungsschema funktionieren würde, aber das Partitionierungsschema steigert die Komplexität von Code und führt viele neue Komplikationen, die Sie für den Umgang mit. Wenn Sie Images in blob Storage verschieben, führen Sie was Sie tun, wenn der Storage-Dienst ausgefallen ist? Wie behandeln Sie Blob-Sicherheit? Was geschieht, wenn die Datenbank und Blob Storage mit der Synchronisierung hapern? Wenn Sie Sharding sind, wie verarbeitet Sie Abfragen für alle Datenbanken?

Schwierigkeiten können verwaltet werden, solange Sie für sie beabsichtigen, bevor Sie in die Produktion gehen. Viele Leute, die dies nicht möchten, sie haben später noch mal. Durchschnittlich wird unser Team Customer Advisory Teams (CAT) aufgeregten Anrufe zu monatlich von Kunden, deren apps in sehr großem Stil in Schwung werden, und dies nicht der Fall bei der Planung. Und etwa sagt: "Hilfe! Ich alles in einem einzigen Datenspeicher abgelegt, und innerhalb von 45 Tagen ich möchte kein Speicherplatz mehr zur Ausführung!" Und wenn Sie einen Großteil der Geschäftslogik integriert, wie Sie Ihren Datenspeicher zugreifen und Sie haben Kunden, die von Ihrer app verwendet werden, besteht keine Gelegenheit für einen Tag ausfällt, während Sie migrieren. Wir kommen durchlaufen Herculean bemühungen um die Partition des Kunden zu ihrer Daten im laufenden Betrieb ohne Downtime. Es ist sehr spannend und sehr schwieriges und nicht etwas beteiligt sein sollen, wenn Sie es vermeiden können! Ihre Gedanken tagein, voraus, und die Integration in Ihre app werden Ihnen das Leben viel leichter machen, wenn die app später zunimmt.

## <a name="summary"></a>Zusammenfassung

Eine effektive Partitionierungsschema können Ihre Cloud-app zu mehreren Petabytes an Daten in der Cloud ohne Engpässe zu skalieren. Und Sie müssen nicht vorab für umfangreiche Computer oder eine umfassende Infrastruktur bezahlen, wie Sie möglicherweise, wenn Sie die app in einem lokalen Rechenzentrum ausgeführt haben. In der Cloud können Sie Kapazität können inkrementell hinzufügen, wie Sie benötigen, und Sie nur bezahlen sind für Sie, wie Sie verwenden, ähnlich wie ihre Verwendung.

In der [im nächsten Kapitel](unstructured-blob-storage.md) sehen wir, wie die Fix It-app implementiert vertikale Partitionierung durch Speichern von Bildern im Blob-Speicher.

## <a name="resources"></a>Ressourcen

Weitere Informationen zu Partitionierungsstrategien finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Microsoft Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy.
- [Microsoft Patterns and Practices - Entwurfsmuster für die Cloud](https://msdn.microsoft.com/library/dn568099.aspx). Datenpartitionierung Anleitungen, Muster "Sharding" wird angezeigt.

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Teil 9-Reihe von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Lesen Sie die Partitionierung Beiträge in Folge 7 aus.
- [Erstellen von Big: Erfahrungen von Kunden für Windows Azure – Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms erläutert Partitionierungsschemas, Sharding-Strategien zum Implementieren von Sharding und SQL-Datenbankverbunde, beginnend ab 19:49. Ähnlich wie die Failsafe-Serie, aber wird auf Weitere Gewusst-wie-Details.

Beispielcode:

- [Clouddienstgrundlagen in Microsoft Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Eine beispielanwendung, die eine horizontal partitionierten Datenbank enthält. Eine Beschreibung der Sharding-Schema implementiert, finden Sie unter [DAL – Sharding von RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) im Windows Azure-Blog.

> [!div class="step-by-step"]
> [Zurück](data-storage-options.md)
> [Weiter](unstructured-blob-storage.md)
