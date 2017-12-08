---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Daten, die Partitionierungsstrategien (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 8eddb7af2d9032153b30ab54d5e882f0b46cd4ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Daten, die Partitionierungsstrategien (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Informationen über die Reihen finden Sie unter [im Kapitel über das erste](introduction.md).


Zuvor haben wir gesehen, wie einfach es ist, die die Webebene einer Cloud-Anwendung skalieren, indem hinzufügen und Entfernen von Webservern. Aber wenn sie alle im selben Datenspeicher auftreten, Ihre Anwendung Engpass verschiebt von Front-End an das Back-End und die Datenebene ist am schwierigsten zu skalieren. In diesem Kapitel untersuchen wir wie Sie die Datenebenen skalierbare durch das Partitionieren von Daten in mehrere relationale Datenbanken oder durch Kombinieren von relationalen Datenbank-Speicher mit anderen Datenspeicheroptionen vornehmen können.

Einrichten eines Partitionierungsschemas eignet sich am besten von vornherein aus demselben Grund, die zuvor erwähnten "Fertig" ändert: Es ist sehr schwierig zu Ihrer Strategie zu Speicher zu ändern, nachdem eine Anwendung in der Produktion ist. Wenn Sie schwer vorab über unterschiedliche Ansätze vorstellen, können Sie vermeiden, müssen "Twitter kurzer" Wenn Ihre app abstürzt oder ausfallen einen längeren Zeitraum, während Sie Ihre app Daten und Datenzugriffscode neu anordnen.

## <a name="the-three-vs-of-data-storage"></a>Die drei Vs der datenspeicherung

Um festzustellen, ob Sie benötigen eine Partitionierungsstrategie besteht und was er sein soll, berücksichtigen Sie in drei Fragen zu Ihren Daten:

- Volume – wie viele Daten sehen Sie schließlich gespeichert werden? Wenige Gigabyte? Einige hundert Gigabytes? Terabyte? Petabytes?
- Geschwindigkeit – was der Wachstumsrate die Daten ist? Handelt es sich um eine interne app, die eine große Datenmenge generieren, ist nicht? Eine externe Anwendung, der Kunden Bilder und Videos in hochgeladen werden?
- Verschiedene – welche Art von Daten wird Sie zu speichern? Relationale, Bilder, Schlüssel-/ Wertpaare, soziale Diagramme?

Wenn Sie, dass ein hohes Maß an Volumes, Geschwindigkeit oder einer Vielzahl werden sollen glauben, müssen Sie sorgfältig überlegen, welche Art von Partitionierungsschema am besten sorgen, dass Ihre app Skalierung effizient und effektiv, wenn er wächst, und um sicherzustellen, dass Sie Engpässe auftreten nicht.

Im Grunde stehen drei Möglichkeiten zur Partitionierung:

- Vertikale Partitionierung
- horizontale Partitionierung
- Hybridpartitionierung

## <a name="vertical-partitioning"></a>Vertikale Partitionierung

Vertikale Portionierung ist wie eine Tabelle mit Spalten aufgeteilt: eine Gruppe von Spalten wird in einem Datenspeicher und eine andere Gruppe von Spalten in einem anderen Datenspeicher wechselt.

Angenommen Sie, dass meine app Daten zu Personen, einschließlich der Bilder gespeichert:

![Datentabelle](data-partitioning-strategies/_static/image1.png)

Wenn Sie diese Daten als Tabelle darstellen und sehen Sie sich die verschiedenen Varianten von Daten, können Sie sehen, dass die drei Spalten auf der linken Seite Zeichenfolgendaten, die von einer relationalen Datenbank effizient gespeichert werden können, während die beiden Spalten auf der rechten Seite im Wesentlichen Bytearrays sind c Ome von Bilddateien. Es ist möglich, Speicher Datei Bilddaten in einer relationalen Datenbank, und viele Personen dies tun, da sie nicht die Daten im Dateisystem speichern möchten. Sie möglicherweise nicht zu einem Dateisystem, das die erforderlichen Mengen an Daten speichern kann, oder möchte er möglicherweise nicht Verwalten einer separaten sichern und wiederherzustellen. Dieser Ansatz funktioniert gut für lokale Datenbanken und für kleine Mengen von Daten in der Cloud-Datenbanken. In der lokalen Umgebung kann es einfacher, nur der Datenbankadministrator alles erledigen lassen sein.

Aber in einer Clouddatenbank-Speicher ist relativ kostenintensiv, und eine große Anzahl von Bildern womöglich die Größe der Datenbank über die Grenzen hinaus vergrößert werden an dem sie effizient ausgeführt werden kann. Sie können diese Probleme beheben, indem Datenpartitionierung vertikal, was bedeutet, dass Sie am besten geeigneten Datenspeicher für jede Spalte in der Datentabelle auswählen. Was am besten geeignet für dieses Beispiel funktioniert möglicherweise darin, die Zeichenfolgendaten in einer relationalen Datenbank und die Bilder im Blob-Speicher.

![Datentabelle vertikal partitioniert](data-partitioning-strategies/_static/image2.png)

Speichern von Bildern im Blob-Speicher statt einer Datenbank ist praktikabler in der Cloud als in einer lokalen Umgebung, da Sie keine Gedanken machen, Dateiserver einrichten oder verwalten, Sichern und Wiederherstellen von Daten, die außerhalb der relationalen Datenbank gespeichert werden: alle die für Sie automatisch vom Blob-Speicherdienst erfolgt.

Dies ist die partitionierungsansatz wir implementiert, in der app zu beheben, und betrachten wir den Code, in der [BLOB-Speicher Kapitel](unstructured-blob-storage.md). Ohne diese Partitionierungsschema und eine durchschnittliche Image-Größe von 3 MB angenommen wird würde die app zu beheben nur ca. 40.000 Aufgaben zu speichern, bevor die maximale Datenbankgröße von 150 GB aktiviert zu sein. Nach dem Entfernen der Bilder, Speichern der Datenbank 10 dreimal so viele Aufgaben; Sie können wesentlich länger navigieren, bevor Sie über die Implementierung einer horizontalen Partitionierungsschema vorstellen müssen. Und wie die app wird skaliert, wird Ihre Ausgaben langsamer wachsen, weil der Großteil der Speicherbedarf sehr kostengünstig Blob-Speicher wiederhergestellt werden.

## <a name="horizontal-partitioning-sharding"></a>Horizontale Partitionierung (Sharding)

Horizontale Portionierung ist wie eine Tabelle mit Zeilen aufgeteilt: eine Gruppe von Zeilen wird in einem Datenspeicher und eine andere Gruppe von Zeilen in einem anderen Datenspeicher wechselt.

Wenn den gleichen Satz von Daten, wäre eine andere Möglichkeit, verschiedene Bereiche von Kundennamen in unterschiedlichen Datenbanken gespeichert.

![Datentabelle horizontal partitioniert.](data-partitioning-strategies/_static/image3.png)

Sie möchten nur mit großer Vorsicht Ihr shardingschema, um sicherzustellen, dass die Daten gleichmäßig verteilt werden, um Hotspots zu vermeiden. Dieses einfache Beispiel mit dem ersten Buchstaben des Nachnamens erfüllt nicht die diese Anforderung zu, da viele Personen haben Nachnamen zurück, die mit bestimmten gemeinsamen Buchstaben beginnen. Sie würden Tabelle größenbeschränkungen früher als zu erwarten, da einige Datenbanken sehr große erhalten würden, während die meisten kleinen bleiben würden erreicht.

Eine Seite nach unten, horizontale Partitionierung ist, dass es möglicherweise schwierig Abfragen über alle Daten. In diesem Beispiel würde eine Abfrage verfügen, aus bis zu 26 unterschiedlichen Datenbanken zum Abrufen aller Daten von der app gespeicherten gezeichnet werden soll.

## <a name="hybrid-partitioning"></a>Hybridpartitionierung

Sie können das vertikale und horizontale Partitionierung kombinieren. Sie können z. B. in den Beispieldaten speichern die Bilder im Blob-Speicher und horizontal partitionieren die Zeichenfolgendaten.

![Daten Tabelle Hybrid partitioniert](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partitionierung einer produktionsanwendung

Grundsätzlich ist es nicht schwer zu verstehen, wie ein Partitionierungsschema erfolgen kann, aber Partitionierungsschema Codekomplexität wird vergrößert und führt viele neue Komplikationen, denen Sie behandeln müssen. Wenn Sie Bilder für den blob-Speicher verschieben, was tun Sie, wenn der Speicherdienst ausgefallen ist? Wie behandle Sie Blob-Sicherheit? Was geschieht, wenn die Datenbank und Blob-Speicher nicht mehr synchron erhalten? Wenn Sie Sharding sind, werden Sie wie die Abfragen über alle Datenbanken verarbeitet?

Die Komplikationen können verwaltet werden, solange Sie für sie beabsichtigen, bevor Sie, bis hin zur Produktion fortfahren. Viele Personen haben nicht dazu später arbeitete werden soll. Unser Team Customer Advisory Team (CAT) Ruft die durchschnittliche panicked Telefonanrufe ungefähr einmal im Monat ab, von Kunden, deren apps auf einer sehr großen Weise, deaktivieren verbrauchen, und sie nicht verwenden, diese Planung. Und es wird argumentiert etwa: "Hilfe! Ich alles in einen einzigen Datenspeicher und in 45 Tagen werde ich darauf knapp!" Und wenn stehen Ihnen viele der Geschäftslogik integriert, wie Sie den Datenspeicher zugreifen und Sie haben Kunden, die von Ihrer app verwendet werden, es ist keine geeignete Zeitpunkt, um für einen Tag ausfallen, während Sie migrieren. Wir annehmen durchlaufen Herculean einschätzen die Customer-Partition können ihre Daten im Handumdrehen mit keine Ausfallzeiten. Es ist sehr vielseitig und sehr furchterregend und nicht beteiligt sein sollen, wenn Sie dies vermeiden können! Diese vorab weiterentwickeln und die Integration in Ihre app werden viel einfacher, wenn die app später wächst.

## <a name="summary"></a>Zusammenfassung

Eine effektive Partitionierungsschema kann Ihre Cloud-app Skalierung auf Daten in der Cloud, ohne dass Engpässe Petabytes aktivieren. Und Sie müssen vorab für massive Computer oder eine umfangreiche Infrastruktur pay-as-möglicherweise, wenn Sie die app in einem lokalen Datencenter ausgeführt wurden. In der Cloud können Sie Kapazität können inkrementell hinzufügen, wie Sie ihn benötigen, und Sie nur Zahlen für so viele, wie Sie verwenden diese Verwendung.

In der [nächsten Kapitels](unstructured-blob-storage.md) wir sehen, wie die app zu beheben implementiert vertikale Partitionierung, indem Sie Bilder im Blob-Speicher speichern.

## <a name="resources"></a>Ressourcen

Weitere Informationen zu Partitionierungsstrategien finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Windows Azure Cloud Services](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy.
- [Microsoft Patterns and Practices - Cloud-Entwurfsmuster](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Finden Sie unter Datenpartitionierung Leitfaden Sharding-Muster.

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Neun zweiteilige Reihe Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Finden Sie Partitionierung in Folge 7.
- [Erstellen von Big: Erkenntnisse aus Windows Azure-Kunden – Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms erläutert Partitionierungsschemas Sharding-Strategien zum Implementieren von Sharding und SQL-Datenbankverbunde, 19:49 ab. Vergleichbar mit dem Failsafe-Serie, aber wechselt in den Gewusst-wie-Informationen.

Beispielcode:

- [Grundlagen von Clouddiensten in Microsoft Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die eine sharddatenbank enthält. Eine Beschreibung der shardingschema implementiert, finden Sie unter [DAL – Sharding von RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) im Windows Azure-Blog.

>[!div class="step-by-step"]
[Zurück](data-storage-options.md)
[Weiter](unstructured-blob-storage.md)
