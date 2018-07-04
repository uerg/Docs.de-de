---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Datenspeicheroptionen (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 43e7a03f3bebcca820452ea10e13459d8d275b5a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363506"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Datenspeicheroptionen (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Die meisten Benutzer werden verwendet, um relationale Datenbanken, und sie neigen dazu, andere Optionen für die datenspeicherung übergangen, wenn sie eine Cloud-app entwerfen. Das Ergebnis kann suboptimaler Leistung, hohe Ausgaben oder noch schlimmer, sein, da [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (nicht relationale) Datenbanken können einige Aufgaben effizienter als relationale Datenbanken verarbeiten. Wenn Kunden uns Hilfe lösen von Problemen Speicher wichtiger Daten anfordern, ist es häufig, da sie eine relationale Datenbank haben, in denen eine der Optionen NoSQL würde besser gearbeitet haben. In solchen Situationen hätten Kunden besser, wenn sie die NoSQL-Lösung implementiert hat, bevor Sie die app in einer produktionsumgebung bereitstellen.

Andererseits, wäre es auch ein Fehler unterlaufen angenommen werden, dass eine NoSQL-Datenbank mit alles gut "oder" gut genug tun kann. Es gibt keine einzelne Data Management ideal für alle. verschiedene datenverwaltungslösungen sind für unterschiedliche Aufgaben optimiert. Die meisten realer Cloud-apps haben eine Vielzahl von Anforderungen an die datenspeicherung und werden durch eine Kombination von mehreren datenspeicherlösungen häufig besten bedient.

Der Zweck dieses Kapitels werden Sie gewähren einem weiteren Sinne von Speicheroptionen zur Verfügung, auf einer Cloud-app, und einige grundlegende Anleitungen zum auswählen, die Ihrem Szenario passen. Es wird empfohlen, beachten Sie die Optionen, die Ihnen zur Verfügung, und Stärken und Schwächen ihrer überlegen, bevor Sie eine Anwendung entwickeln. Ändern Optionen für die datenspeicherung in einer Produktions-app kann sehr schwierig sein, mit einer Jet-Modul geändert werden, wenn die Ebene in der Luft ist.

## <a name="data-storage-options-on-azure"></a>Optionen für die datenspeicherung in Azure

Die Cloud ist es relativ einfach, eine Vielzahl von relationalen und NoSQL-Datenspeicher verwenden. Hier sind einige der Daten Speicherplattformen, die Sie in Azure verwenden können.

![](data-storage-options/_static/image1.png)

Die Tabelle enthält vier Arten von NoSQL-Datenbanken:

- [Schlüssel/Wert-Datenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec7) ein serialisiertes Objekt für jedes Schlüssel-Wert zu speichern. Diese eignen sich gut zum Speichern großer Datenmengen, in denen Sie ein Element für einen bestimmten Schlüsselwert abrufen möchten, und Sie haben keine, Abfrage basierend auf anderen Eigenschaften des Elements.

    [Azure-blobspeicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) ist eine Schlüssel/Wert-Datenbank, die wie Dateispeicher in der Cloud mit Schlüsselwerten-Funktionen, die Ordner und Dateinamen entsprechen. Sie rufen Sie eine Datei durch die Ordner und Dateinamen an, nicht nach Werten in den Inhalt der Datei suchen.

    [Azure-Tabellenspeicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) ist auch eine Schlüssel-Wert-Datenbank. Jeder Wert wird als ein *Entität* (ähnlich wie eine Zeile, die durch einen Partitionsschlüssel und Zeilenschlüssel identifiziert) und enthält mehrere *Eigenschaften* (ähnlich wie Spalten, aber nicht alle Entitäten in einer Tabelle denselben (Spalten). Abfragen von Spalten als der Schlüssel ist sehr ineffizient und sollte vermieden werden. Beispielsweise können Sie den Profildaten des Benutzers, mit einer Partition Speichern von Informationen zu einem einzelnen Benutzer speichern. Sie können Daten wie z. B. Benutzername, Kennwort-hashtags, Geburtsdatum usw., in separaten Eigenschaften einer Entität oder in separaten Entitäten in derselben Partition speichern. Aber wäre nicht für alle Benutzer mit einem angegebenen Bereich von Geburtsdaten abgefragt werden soll, und eine Join-Abfrage nicht werden, zwischen der Profiltabelle und einer anderen Tabelle ausgeführt kann. Table Storage ist skalierbarer und weniger Kosten als eine relationale Datenbank, aber sie nicht, komplexe Abfragen oder Verknüpfungen zu aktivieren.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sind Schlüssel/Wert-Datenbanken, die in der die Werte sind *Dokumente*. "Dokument" hier nicht im Sinne eines Word- oder Excel-Dokuments verwendet, aber es bedeutet, dass eine Auflistung von benannten Feldern und Werten, von denen ein untergeordnetes Dokument handeln. So möglicherweise ein Order-Dokument in einer Reihenfolge Verlaufstabelle z. B. Bestellnummer, Bestelldatum und Customer-Felder; und das Kundenfeld möglicherweise Namens- und Adressfelder. Die Datenbank codiert Feld-Daten in ein Format z. B. XML, YAML, JSON oder BSON; Sie können auch nur-Text. Ein Feature, das legt Dokumentdatenbanken, abgesehen von Schlüssel/Wert-Datenbanken ist die Möglichkeit, auf nicht-Schlüsselfelder Abfragen und definieren sekundäre Indizes, um die Abfragen effizienter zu gestalten. Möglichkeit, eine Dokumentendatenbank ist für Anwendungen, die zum Abrufen von Daten basierend auf Kriterien, die komplexer als der Wert des Dokumentschlüssels besser geeignet. Die in einer Bestellung Verlauf können Sie z. B. für verschiedene Felder wie Produkt-ID, Kunden-ID, Kundennamen usw. Abfragen. [MongoDB](http://www.mongodb.org/) ist eine beliebte dokumentdatenbank.
- [Spaltenfamilien-Datenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sind Schlüssel/Wert-Datenspeicher, die Sie zum Strukturieren des Speichers Daten in Sammlungen mit verwandten Spalten namens spaltenfamilien zu aktivieren. Eine Census-Datenbank kann z. B. eine Gruppe von Spalten für den Namen einer Person enthalten (zuerst zweiter Vorname, Nachname), eine Gruppe für die Adresse einer Person und eine Gruppe für die Profilinformationen für die Person (Geburtsdatum, Geschlecht usw.). Der Datenbank kann jede spaltenfamilie klicken Sie dann in einer separaten Partition gespeichert, wobei alle Daten für eine Person, die im Zusammenhang mit dem gleichen Schlüssel. Anschließend können Sie alle Profilinformationen lesen, ohne alle sowie der Name und Adresse Informationen zu lesen. [Cassandra](http://cassandra.apache.org/) ist eine beliebte spaltenfamilien-Datenbank.
- [Graphdatenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec10) Speichern von Informationen als eine Auflistung von Objekten und Beziehungen. Der Zweck einer Diagrammdatenbank ist eine Anwendung effiziente Abfragen durchführen, die das Netzwerk von Objekten und die Beziehungen zwischen ihnen zu durchlaufen. Z. B. Objekte können Mitarbeiter in einer personalverwaltungsdatenbank sein und sollten Sie um zu erleichtern Abfragen wie z. B. "alle Mitarbeiter finden, die direkt oder indirekt zu arbeiten für Scott." [Neo4j](http://www.neo4j.org/) ist eine beliebte Diagrammdatenbank.

Im Vergleich zu relationalen Datenbanken, bieten die NoSQL-Optionen, sehr viel größere Skalierbarkeit und Kosteneffizienz für Speicher und die Analyse unstrukturierter Daten. Der Nachteil besteht darin, dass sie nicht die umfassende Queryability und zuverlässigen Integrität von relationalen Datenbanken bereitstellen. NoSQL funktioniert gut für die IIS-Protokolldaten, die umfangreiche, ohne dass Join-Abfragen umfasst. NoSQL würde nicht so gut geeignet für Banktransaktionen, die absolute Datenintegrität erfordert und umfasst viele Beziehungen zu anderen kontobezogene Daten.

Es gibt auch eine neuere Kategorie von Datenbank-Dienstplattform, die Namen [NewSQL](http://en.wikipedia.org/wiki/NewSQL) , die die Skalierbarkeit einer NoSQL-Datenbank mit dem Queryability und Datenintegrität bei Transaktionen von einer relationalen Datenbank kombiniert. NewSQL Datenbanken dienen für verteilte Speicherung und Verarbeitung von Abfragen, was häufig schwer in "OldSQL" Datenbanken implementiert ist. [NuoDB](http://www.nuodb.com/) ist ein Beispiel für eine NewSQL-Datenbank, die in Azure verwendet werden kann.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop und MapReduce

Die große Mengen von Daten, die in der NoSQL-Datenbanken gespeichert werden können möglicherweise schwierig, rechtzeitig effizient zu analysieren. Möchten Sie die Verwendung eines Frameworks wie [Hadoop](http://hadoop.apache.org/) implementiert [MapReduce](http://en.wikipedia.org/wiki/MapReduce) Funktionalität. Im Wesentlichen lautet ein MapReduce-Prozess ist wie folgt:

- Grenzwert für die Größe der Daten, die durch Auswahl aus den Daten verarbeitet werden müssen, speichern nur die Daten, die Sie tatsächlich analysieren müssen. Sie möchten z. B. die Zusammensetzung von Geburtsjahr, Ihren Benutzerstamm kennen, damit Sie aus Ihrem Benutzer-Profildatenspeicher nur Geburtsdatum Jahre auswählen.
- Die Daten in Teile unterteilen Sie, und senden Sie sie auf einen anderen Computer für die Verarbeitung. Ein berechnet die Anzahl der Personen mit 1950-1959-Datumsangaben, Computer B ist 1960-1969, usw. Diese Gruppe von Computern wird aufgerufen, eine *Hadoop-Cluster*.
- Fügen Sie die Ergebnisse der einzelnen Teile wieder zusammen, nach Abschluss die Verarbeitung zu den Teilen. Sie verfügen nun über eine relativ kurze Liste der für jede Geburtsjahr wie viele Personen ein, und der Task, der Berechnung der Prozentwerte in der gesamten Liste kann verwaltet.

In Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) ermöglicht es Ihnen, zu verarbeiten, analysieren und Gewinnen neuer Erkenntnisse aus big Data, die mit der Leistungsfähigkeit von Hadoop. Beispielsweise können Sie es verwenden, um Webserverprotokolle zu analysieren:

- Aktivieren Sie die webserverprotokollierung in Ihr Speicherkonto. Azure zum Schreiben von Protokollen in den Blob-Dienst für jede HTTP-Anforderung für Ihre Anwendung eingerichtet. Der Blob-Dienst ist im Grunde clouddateispeicher, und alles ist perfekt mit HDInsight. 

    ![Protokolle, um Blob-Speicher](data-storage-options/_static/image2.png)
- Wie die app den Datenverkehr empfängt, werden die Web-Server-IIS-Protokolle in Blob Storage geschrieben. 

    ![Webserverprotokolle](data-storage-options/_static/image3.png)
- Klicken Sie im Portal auf **neu** - **Datendienste** - **HDInsight** - **Schnellerfassung**, und geben Sie den Namen eines HDInsight-Clusters, Clustergröße (Anzahl der Datenknoten für HDInsight-Cluster), und einen Benutzernamen und Kennwort für den HDInsight-Cluster. 

    ![HDInsight](data-storage-options/_static/image4.png)

Sie können jetzt festlegen, um MapReduce-Aufträgen zum Analysieren der Protokolle und erhalten Sie Antworten auf Fragen wie:

- Welchen Tageszeiten erhalten Meine app den Datenverkehr für die meisten oder wenigsten?
- Welchen Ländern ist meine Datenverkehr aus?
- Was ist die durchschnittliche Nachbarschaft Einkommen die Bereiche, denen meinen Datenverkehr stammt. (Es ist ein öffentliches Dataset, das Sie Nachbarschaft Einkommen von IP-Adresse erhalten, und für IP-Adresse in den Webserverprotokollen übereinstimmen,.)
- Wie korrelieren die Nachbarschaft Einkommen auf bestimmte Seiten oder Produkte auf der Website?

Sie können dann die Antworten auf Fragen wie diese zielgerichtet gemäß der Wahrscheinlichkeit, die, der ein Kunde interessiert wäre, oder wahrscheinlich ein bestimmtes Produkt kaufen.

Siehe die [automatisieren alles Kapitel](automate-everything.md), die meisten Funktionen, die Sie im Portal können automatisiert werden können und, umfasst das Einrichten und Ausführen von analyseaufträgen HDInsight. Ein typisches HDInsight-Skript kann die folgenden Schritte umfassen:

- Bereitstellen eines HDInsight-Clusters, und klicken Sie mit Ihrem Speicherkonto für Blob Storage-Eingabe zu verknüpfen.
- Laden Sie die MapReduce-Auftrag ausführbaren Dateien (JAR oder .exe-Dateien) mit dem HDInsight-Cluster hoch.
- Senden Sie ein MapReduce, in der die Ausgabedaten in Blob Storage gespeichert.
- Warten Sie, bis des Auftrags abgeschlossen.
- Löschen Sie den HDInsight-Cluster.
- Auf die Ausgabe aus dem Blob-Speicher zugreifen.

Durch Ausführen eines Skripts, das alle dies tut, minimieren Sie die verstrichene Zeitspanne, die der HDInsight-Cluster bereitgestellt wird, wodurch die Kosten reduziert wird.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platform as a Service (PaaS) und Infrastructure as a Service (IaaS)

Die oben aufgeführten Optionen für die datenspeicherung beinhalten Platform-as-a-Service (PaaS) und Infrastructure-as-a-Service (IaaS)-Lösungen. PaaS bedeutet, dass wir die Hardware und Software-Infrastruktur verwalten Sie einfach den Dienst verwenden. SQL-Datenbank ist ein PaaS-Feature von Azure. Sie stellen für Datenbanken, und hinter den Kulissen Azure eingerichtet und konfiguriert die virtuellen Computer, und legt diese fest, um die Datenbanken auf. Sie haben keinen direkten Zugriff auf die virtuellen Computer und nicht für deren Verwaltung. IaaS bedeutet, dass Sie einrichten, konfigurieren und Verwalten von virtuellen Computern, die in unserer Infrastruktur von Rechenzentren ausgeführt, und Sie beliebig auf. Einen Katalog von vorkonfigurierten VM-Images für allgemeine VM-Konfigurationen zur Verfügung. Beispielsweise können Sie die vorkonfigurierte VM-Images für Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle-Datenbank usw. installieren.

PaaS Data-Lösungen, die Azure-Angebote enthalten:

- Azure SQL-Datenbank (früher SQL Azure). Eine relationalen Clouddatenbank auf Basis von SQL Server.
- Azure-Tabellenspeicher. Ein Schlüssel/Wert-NoSQL-Datenbank.
- Azure Blob Storage her. Dateispeicher in der Cloud.

Für IaaS können Sie alles ausführen, die Sie auf einen virtuellen Computer, z. B. laden können:

- Relationale Datenbanken wie SQL Server, Oracle, MySQL, SQL Compact, SQLite, oder Postgres.
- Schlüssel/Wert-Datenspeicher wie z. B. Memcached, Redis, Cassandra und Riak.
- Spalte Datenspeicher wie z.B. HBase.
- Dokumentieren Sie die Datenbanken, z.B. MongoDB, CouchDB und RavenDB.
- Graph-Datenbanken, z. B. Neo4j.

![Optionen für die datenspeicherung in Azure](data-storage-options/_static/image5.png)

Die IaaS-Option erhalten Sie nahezu unbegrenzte Optionen für die datenspeicherung, und viele davon sind besonders einfach zu verwenden, da Sie virtuelle Computer mithilfe von vorkonfigurierten Images erstellen können. Wechseln Sie z. B. im Verwaltungsportal zu **VMs**, klicken Sie auf die **Images** Registerkarte, und klicken Sie auf **VM-Depot Durchsuchen**.

![VM-Depot durchsuchen](data-storage-options/_static/image6.png)

Sie sehen dann eine Liste der [Hunderte von vorkonfigurierten VM-Images](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), und Sie können einen virtuellen Computer über ein Image mit einem Datenbank-Managementsystem vorinstalliert, z. B. Neo4J, MongoDB, Redis, Cassandra oder CouchDB erstellen:

![MongoDB im VM-Depot](data-storage-options/_static/image7.png)

Azure ist ein IaaS-Optionen für die datenspeicherung so einfach wie möglich zu verwenden, aber die PaaS-Angebote haben viele Vorteile, die sie kostengünstiger und für viele Szenarios geeignet machen:

- Sie müssen keine virtuellen Computer erstellen, die Sie nur das Portal oder ein Skript zum Einrichten von eines Datenspeichers verwenden. Wenn Sie einen Datenspeicher 200 Terabyte möchten, Sie können nur auf eine Schaltfläche klicken, oder führen Sie einen Befehl, und in Sekunden für die Verwendung bereit ist.
- Sie müssen keine verwalten oder zu patchen die virtuellen Computer, die vom Dienst verwendet werden; Microsoft übernimmt dies für Sie automatisch.-Sie müssen keine Gedanken um das Einrichten der Infrastruktur für die Skalierung oder mit hoher Verfügbarkeit; Microsoft übernimmt alles für Sie.
- Sie müssen keine Lizenzen kaufen; Lizenzgebühren sind in der-Dienstgebühren enthalten.
- Sie Zahlen nur für die tatsächliche Nutzung.

PaaS-Daten-Speicheroptionen in Azure sind Angebote von Drittanbietern einzuholen. Beispielsweise können Sie die [MongoLab-Add-On](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) aus dem Azure-Store, eine MongoDB-Datenbank als Dienst bereitstellen.

## <a name="choosing-a-data-storage-option"></a>Wählen eine Option für storage

Ohne ein Ansatz ist für alle Szenarien geeignet. Wenn jede Person anzeigt, dass diese Technologie die Antwort ist, wird als erstes Fragen "Was die Frage ist?", da verschiedene Lösungen für verschiedene Dinge optimiert sind. Es gibt das relationale Modell hat eindeutige Vorteile. daher so lange existiert bereits. Es gibt jedoch auch nach unten-Seiten in SQL, die mit einer NoSQL-Lösung behoben werden können.

Häufig finden Sie unter Was wir funktionieren am besten einen festgelegten Ansatz ist, verwenden Sie SQL- und NoSQL in einer einzigen Lösung. Wenn auch Personen sagen, sie sind embracing NoSQL, wenn Sie einen Drilldown in das, was Sie häufig ausgeführt wird feststellen, dass sie mehrere verschiedene NoSQL-Frameworks verwenden: Verwenden sie [CouchDB](http://wiki.apache.org/couchdb/Introduction), und [Redis](http://redis.io/), und [ Riak](http://basho.com/riak/) für verschiedene Dinge. Auch Facebook, dem NoSQL ausgiebig verwendet wird, verwendet verschiedene NoSQL-Frameworks für die verschiedenen Teile des Diensts. Die Flexibilität zum Mischen und Zuordnen von Daten speicheransätzen ist eines der Dinge, die Vorteile der Cloud, da es ist einfach, mehrere Data-Lösungen verwenden und diese in einer app zu integrieren.

Hier sind einige Fragen zu bedenken, wenn Sie einen Ansatz auswählen können:

| Semantische Daten | – Die Core-Speicher und Datenzugriff semantische neuerungen (sind Sie das Speichern von relationalen oder unstrukturierten Daten)? Unstrukturierte Daten wie Mediendateien passt am besten im Blob-Speicher; eine Auflistung von verknüpften Daten wie z. B. Produkte, Bestände, Lieferanten, kundenbestellungen usw., passt am besten in einer relationalen Datenbank. |
| --- | --- |
| Unterstützung für Abfragen | – Ist wie einfach es, um die Daten abzufragen? – Welche Arten von Fragen effizient sein können aufgefordert? Schlüssel/Wert-Datenspeicher sind sehr gut zum Abrufen einer einzelnen Zeile, die ein Schlüssel-Wert zugewiesen, aber nicht so gut für komplexe Abfragen. Für einen Benutzer Profildatenspeicher, in dem Sie immer die Daten für einen bestimmten Benutzer abrufen, kann ein Schlüssel/Wert-Datenspeicher gut funktionieren; eine relationale Datenbank möglicherweise besser funktioniert, für einen Produktkatalog, in denen unterschiedliche Gruppierungen basierend auf verschiedenen Produktattribute abgerufen werden soll. NoSQL-Datenbanken können große Mengen an Daten effizient zu speichern, aber müssen Sie die Struktur der Datenbank um, wie die app die Daten Abfragen, und Dies erschwert ad-hoc-Abfragen möchten. Mit einer relationalen Datenbank können Sie nahezu jede Art von Abfrage erstellen. |
| Funktionale Projektion | – Fragen Sie können, Aggregationen, usw., serverseitige ausgeführt? Wenn ich Ausführen von SELECT COUNT (\*) aus einer Tabelle in SQL, wird sehr effizient erledigen die gesamte Arbeit auf dem Server und zurück, die ich Suche. Wenn soll die gleiche Berechnung aus einem NoSQL-Datenspeicher, der Aggregation nicht unterstützt, dies ist eine ineffiziente "unbounded Query", und es wird wahrscheinlich einen Timeout beendet werden. Auch wenn die Abfrage erfolgreich ist, habe ich alle Daten an den Client vom Server abzurufen und zum zählen der Zeilen auf dem Client. – Welche Sprachen oder Typen von Ausdrücken verwendet werden können? Mit einer relationalen Datenbank kann ich SQL verwenden. Mit einigen NoSQL-Datenbanken wie Azure Table Storage, ich verwende hier [OData](http://www.odata.org/), und kann ich nur auf primären Schlüssel zu filtern und Projektionen (Wählen Sie eine Teilmenge der verfügbaren Felder) zu erhalten. |
| Die mühelose Skalierbarkeit | – Wie häufig und wie viel wird die Daten zum skalieren müssen? – Werden ist die Plattform nativ horizontale Skalierung implementiert? – Wie einfach es zum Hinzufügen oder Entfernen von Kapazität (Größe und Durchsatz) ist? Relationale Datenbanken und Tabellen werden nicht automatisch partitioniert, um sie skalierbare, machen und schwierig zu skalieren, die über bestimmte Einschränkungen. NoSQL-Datenspeicher wie Azure Table Storage ist grundsätzlich partitionieren, alles, und es gibt fast keine Beschränkung für das Hinzufügen von Partitionen. Sie können Table Storage problemlos skalieren, bis zu 200 Terabyte, aber die maximale Datenbankgröße für Azure SQL-Datenbank beträgt 500 GB. Sie können relationale Daten skalieren, indem Sie sie in mehrere Datenbanken partitionieren, aber einen Großteil der Programmierung von Arbeit umfasst das Einrichten einer Anwendung, die dieses Modell unterstützen. |
| Instrumentierung und verwaltbarkeit | – Ist wie leicht die Plattform zum Instrumentieren, überwachen und verwalten? Sie benötigen, bleiben Sie über die Integrität und Leistung des Datenspeichers, daher müssen Sie vorab wissen, welche Metriken eine Plattform, die Sie kostenlos erhalten, und was man selbst entwickeln. |
| Vorgänge | – Ist wie leicht die Plattform bereitstellen und Ausführen in Azure? PaaS? IaaS? Linux? Table Storage und SQL-Datenbank sind einfach in Azure einrichten. Plattformen, die nicht von integrierten Azure-PaaS-Lösungen erfordern mehr Aufwand. |
| API-Unterstützung | -Ist eine API zur Verfügung, die Zusammenarbeit mit der Plattform vereinfacht? Für Azure-Tabellenspeicherdienst ist eine SDK mit einer .NET API, die das asynchrone Programmiermodell von .NET 4.5 unterstützt. Wenn Sie eine app .NET schreiben, werden sie zum Schreiben und Testen von Code für den Azure-Tabellenspeicherdienst im Vergleich zu einem anderen Schlüssel/Wert-Spalte Store Datenplattform, die keine API oder einen weniger umfangreiche hat viel einfacher. |
| Transaktionskonsistenz für Integrität und Daten | – Ist es wichtig, dass die Plattform Transaktionen unterstützen, damit die Konsistenz der Daten gewährleistet? Zum Nachverfolgen der Massen-e-Mails gesendet, Leistung und geringe datenspeicherkosten wichtiger als automatische Unterstützung für Transaktionen oder referenzielle Integrität in das Data-Plattform, möglicherweise ausführenden Azure-Tabellendienst eine gute Wahl. Lastenausgleich für die nachverfolgung von Bankkonto oder Bestellungen eine relationalen Datenbankplattform, die bereitstellt, dass starke Transaktionsgarantien eine bessere Wahl wäre. |
| Die Geschäftskontinuität | – Wie einfache Sicherung, Wiederherstellung und notfallwiederherstellung sind? Früher oder später Produktionsdaten werden beschädigt werden, und Sie benötigen eine Rückgängigfunktion. Relationale Datenbanken haben häufig weitere differenzierte Wiederherstellungsfunktionen, z. B. die Möglichkeit, zu einem bestimmten Zeitpunkt wiederherstellen. Verstehen, welche Wiederherstellungsfunktionen in jede Plattform verfügbar sind, die Sie darüber nachdenken, ist ein wichtiger zu berücksichtigender Faktor. |
| Kosten | – Wenn mehr als eine Plattform Ihrer Daten Workload unterstützen kann, wie Unterschiede sie Kosten weisen? Bei Verwendung von ASP.NET Identity können Sie z. B. Benutzerprofildaten, die im Azure-Tabellendienst oder Azure SQL-Datenbank speichern. Wenn Sie die umfassenden Funktionen von SQL-Datenbank Abfragen nicht benötigen, empfiehlt sich Azure-Tabellen Teil, da sich die Kosten viel weniger für eine bestimmte Menge an Speicher. |

Was empfehlen wir im Allgemeinen ist die Antwort auf die Fragen in jeder dieser Kategorien kennen, bevor Sie Ihre datenspeicherlösungen auswählen.

Darüber hinaus möglicherweise Ihre Workload bestimmte Anforderungen, die auf einigen Plattformen besser als andere unterstützen kann. Zum Beispiel:

- Werden Funktionen für Ihre Anwendung erfordern überwacht?
- Was sind Ihre Langlebigkeit datenanforderungen – benötigen Sie Funktionen für automatische Archivierung oder löschen?
- Haben Sie die speziellen Anforderungen? Z. B. die Daten umfassen personenbezogene Informationen (persönlich identifizierbare Informationen), aber Sie haben in der Lage, um sicherzustellen, dass personenbezogene Informationen aus den Abfrageergebnissen ausgeschlossen ist.
- Wenn Sie einige Daten, die in der Cloud aus gesetzlichen oder technologische gespeichert werden können haben, benötigen Sie möglicherweise eine Cloud-Speicher-Datenplattform, die erleichtert die Integration mit dem lokalen Speicher.

## <a name="demo--using-sql-database-in-azure"></a>Demo: Verwenden von SQL-Datenbank in Azure

Die Fix It-app verwendet eine relationale Datenbank zum Speichern von Aufgaben. Die Umgebung erstellen Windows PowerShell-Skript, das angezeigt wird, der [automatisieren alles Kapitel](automate-everything.md) zwei Instanzen von SQL-Datenbank erstellt. Sie können dies im Portal anzeigen, indem Sie auf die **SQL-Datenbanken** Registerkarte.

![SQL-Datenbanken im portal](data-storage-options/_static/image8.png)

Es ist auch einfach zum Erstellen von Datenbanken mithilfe des Portals.

Klicken Sie auf **neu: Data Services** -- **SQL-Datenbank** -- **Schnellerfassung**, geben Sie einen Datenbanknamen ein, wählen Sie einen Server, Sie bereits in Ihrem Konto haben, oder ein neues erstellen, und klicken Sie auf **SQL-Datenbank erstellen**.

![Neue SQL-Datenbank](data-storage-options/_static/image9.png)

Warten Sie einige Sekunden, und Sie verfügen über eine Datenbank in Azure für die Verwendung bereit.

![Neu erstellte SQL-Datenbank](data-storage-options/_static/image10.png)

Damit Azure in einigen Sekunden, was es Sie einen Tag dauern kann, oder eine Woche oder länger in der lokalen Umgebung umgesetzt werden können. Und da Sie ganz einfach Datenbanken automatisch in einem Skript oder mithilfe einer Verwaltungs-API erstellen können, können Sie dynamisch horizontal hochskalieren durch Ihre Daten über mehrere < O:p > Datenbanken verteilen, solange die Anwendung dafür programmgemäß. </o : p >

Dies ist ein Beispiel für unsere Plattform-as-a-Service-Modell. Sie müssen keine Server verwalten, wir übernehmen. Sie müssen keine Sicherungen kümmern, wir übernehmen. Es läuft in hohe Verfügbarkeit – die Daten in der Datenbank werden automatisch auf drei Servern repliziert. Wenn ein Computer ausfällt, wir automatisch ein Failover aus, und Sie keine Daten verloren gehen. Der Server regelmäßig gepatcht wird, ist keine Gedanken.

Klicken Sie auf eine Schaltfläche, und erhalten Sie die genaue Verbindungszeichenfolge, die Sie benötigen, und können sofort mit der neuen Datenbank.

![Verbindungszeichenfolgen](data-storage-options/_static/image11.png)

Das Dashboard zeigt Ihnen, verbindungsverlauf und die Menge des verwendeten Speichers.

![SQL-Datenbank-Dashboard](data-storage-options/_static/image12.png)

Sie können die Datenbanken im Verwaltungsportal verwalten oder mithilfe von SQL Server-Tools sind Sie bereits kennen, einschließlich SQL Server Management Studio (SSMS) und der Visual Studio-Tools, SQL Server-Objekt-Explorer (SSOX) und Server-Explorer.

![SSOX](data-storage-options/_static/image13.png)

Schöne an der eine andere ist das Preismodell. Entwicklung beginnen Sie mit eine kostenlose 20-MB-Datenbank und eine Produktionsdatenbank startet bei ca. 5 USD pro Monat. Sie Zahlen nur für die Menge an Daten, die Sie tatsächlich in der Datenbank, nicht die maximale Kapazität speichern. Sie müssen eine Lizenz erwerben.

SQL-Datenbank ist einfach zu skalieren. Für die Fix It-app ist die Datenbank, die wir in unserer Automatisierungsskript erstellen auf 1 GB begrenzt. Wenn sie bis zu 150 GB vergrößert oder verkleinert werden sollen, Sie können nur greifen über das Portal, und ändern Sie diese Einstellung oder einen REST-API-Befehl ausführen und in Sekunden, die Sie haben eine 150 GB-Datenbank, der Sie Daten bereitstellen können.

![SQL-Datenbankeditionen und-Größen](data-storage-options/_static/image14.png)

Dies ist die Leistungsfähigkeit der Cloud-Infrastruktur schnell und einfach erstellen und starten sie sofort verwenden.

Die Fix It-app verwendet wird, zwei SQL-Datenbanken, für die Mitgliedschaft (Authentifizierung und Autorisierung) und für Daten, und dies ist alles, was Sie tun, um das Bereitstellen und skalieren Sie ihn. Sie haben gesehen, weiter oben beschrieben, wie die Datenbanken, die über Windows PowerShell-Skripts, und Sie auch wissen, wie einfach es ist, führen Sie das Portal bereitstellen.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entitätsframework im Vergleich zu direkten Datenbankzugriff mithilfe von ADO.NET

Die Fix It-app greift auf diese Datenbanken mithilfe von Entity Framework, die von Microsoft empfohlen ORM (Objektrelationaler Mapper) für Anwendungen mit .NET. Produktivität Rückumwandlung Beeinträchtigung der Leistung in einigen Szenarien ORM ist ein großartiges Tool, das Produktivität von Entwicklern erleichtert. Sie wird nicht Treffen einer Entscheidung zwischen der mithilfe von EF oder ADO.NET direkt – verwenden Sie beide, in einer echten Cloud-app. In den meisten Fällen, wenn Sie Code schreiben, die mit der Datenbank, maximale Leistung ist nicht entscheidend, und profitieren Sie von der vereinfachte Programmierung und testen, dass Sie mit dem Entity Framework erhalten. In Situationen, in denen EF Mehraufwand nicht akzeptablen Leistung führen würde, können Sie schreiben und eigene Abfragen mit ADO.NET, im Idealfall durch Aufrufen von gespeicherten Prozeduren ausführen.

Beliebige Methode Sie verwenden, um die Datenbank zugreifen möchten, dass Sie "chattiness""so weit wie möglich zu minimieren. Das heißt, wenn Sie in einem größeren Abfrageresultset statt Dutzende oder Hunderte von kleinere alle benötigten Daten erhalten können, empfiehlt sich, die in der Regel. Wenn Sie möchten die Liste Schüler/Studenten und Kurse, die, denen Sie registriert sind, ist es z. B. in der Regel besser, zum Abrufen aller Daten in einer Join-Abfrage anstelle der Schüler/Studenten in einer Abfrage abrufen und Ausführen von separate Abfragen für Studenten, Kurse.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL-Datenbanken und das Entity Framework in der Fix It-app

In der app Fix It die `FixItContext` -Klasse, die Entity Framework abgeleitet `DbContext` Klasse, identifiziert die Datenbank und gibt die Tabellen in der Datenbank. Der Kontext gibt eine Entitätenmenge (Tabelle), für Aufgaben, und in den Kontext der Name der Verbindungszeichenfolge übergibt des Codes. Dieser Name bezieht sich auf eine Verbindungszeichenfolge, die in der Datei "Web.config" definiert ist.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Die Verbindungszeichenfolge in der *"Web.config"* Datei heißt Appdb (Hier zeigt auf die lokale Entwicklung-Datenbank):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Erstellt das Entity Framework eine *FixItTasks* Tabelle auf Grundlage der Eigenschaften enthalten, die der `FixItTask` Entitätsklasse. Dies ist eine einfache POCO (Plain Old CLR Object)-Klasse, die bedeutet, dass es nicht erben oder Abhängigkeiten auf Entity Framework haben. Aber die Entity Framework weiß, wie zum Erstellen einer Tabelle, die darauf basieren, und führen Sie CRUD-Vorgänge (erstellen-lesen-aktualisieren-löschen) mit.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks-Tabelle](data-storage-options/_static/image15.png)

Die Fix It-app enthält eine Repositoryschnittstelle, die für die CRUD-Vorgänge, die Arbeit mit dem Datenspeicher verwendet werden.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Beachten Sie, dass die repositorymethoden alle Async, damit der gesamte Datenzugriff auf eine vollständige asynchrone Weise erfolgen kann.

Die Repository-Implementierung Aufrufe Entity Framework-Async-Methoden zum Arbeiten mit Daten, einschließlich LINQ-Abfragen sowie für Einfüge-, Update- und Löschvorgänge. Hier ist ein Beispiel für den Code für die Suche nach einem Fix It-Aufgabe.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Sie werden feststellen, es gibt auch einige zeitlichen Steuerung und hier fehlerprotokollierungscode, betrachten wir, die weiter unten in der [Überwachung und Telemetrie Kapitel](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Wählen im Vergleich zu SQLServer auf einem virtuellen Computer (IaaS) in Azure SQL-Datenbank (PaaS)

Eine schöne an der SQL Server und Azure SQL-Datenbank ist, dass das Core-Programmiermodell für beide identisch ist. Sie können die meisten Kenntnisse in beiden Umgebungen verwenden. Sie können sogar die SQL Server-Datenbank in der Entwicklung und eine SQL-Datenbank-Instanz verwenden, in der Cloud, d.h. wie die Fix It-app eingerichtet ist.

Als Alternative können Sie dieselbe SQL Server in der Cloud ausführen, dass Sie lokal ausführen, indem Sie es auf IaaS-VMs installieren. Für einige älteren Anwendungen mit SQL Server auf einem virtuellen Computer eine bessere Lösung möglicherweise. Da es sich bei eine SQL Server-Datenbank auf einem dedizierten virtuellen Computer ausgeführt wird, hat er Weitere Ressourcen zur Verfügung als bei einer SQL-Datenbank-Datenbank, die auf einem freigegebenen Server ausgeführt wird. Bedeutet, dass SQL Server-Datenbank kann größer sein und auch weiterhin ausführen. Im Allgemeinen funktioniert je kleiner die Größe der Datenbank und die Größe, die besser der Anwendungsfall für SQL-Datenbank (PaaS).

Es folgen einige Richtlinien zur Auswahl zwischen den beiden Modellen.

| Azure SQL-Datenbank (PaaS) | SQLServer auf einem virtuellen Computer (IaaS) |
| --- | --- |
| **Experten** -müssen Sie nicht erstellen oder Verwalten von virtuellen Computern, aktualisieren oder Patchen von Betriebssystem oder SQL; Azure übernimmt, die für Sie. -Integrierte Hochverfügbarkeit, mit einer SLA auf Datenbankebene. -Niedrige Gesamtbetriebskosten (TCO), da Sie Zahlen nur für die tatsächliche Nutzung (keine Lizenz erforderlich). -Gut zum Verarbeiten von großen Anzahl von kleineren Datenbanken (&lt;= jeweils 500 GB). – Einfach, dynamisch erstellen horizontaler neue Datenbanken zu aktivieren. | ***Experten*** – mit lokalen SQLServer-Feature-kompatibel. – Sie können SQL Server implementieren [Hochverfügbarkeit über AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) auf 2 virtuellen Computern mit VM-Ebene-SLA. – Sie haben vollständige Kontrolle über die wie SQL verwaltet wird. – Sie können SQL-Lizenzen Sie bereits besitzen, die oder Zahlen Sie pro Stunde für einen erneut verwenden. -Gut für die Behandlung von weniger aber größere (1 TB) Datenbanken. |
| **Nachteile** -einige Lücken im Vergleich zu lokalen SQL Server-feature (fehlenden [CLR-Integration](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [komprimierungsunterstützung](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)usw.) – maximale Datenbankgröße 500 GB. | ***Nachteile*** - Aktualisierungen/Patches (Betriebssystem und SQL) liegen in Ihrer Verantwortung - Erstellung und Verwaltung von Datenbanken liegen in Ihrer Verantwortung - Datenträger-IOPS (e/a-Vorgänge pro Sekunde) auf ungefähr 8000 (über 16 Festplattenlaufwerke) beschränkt. |

Wenn Sie SQL Server auf einem virtuellen Computer verwenden möchten, können Sie Ihre eigene SQL Server-Lizenz, oder Sie können für eine pro Stunde bezahlen. Im Portal oder über die REST-API können Sie beispielsweise einen neuen virtuellen Computer mit einem SQL Server-Image erstellen.

![Erstellen des virtuellen Computers mit SQLServer](data-storage-options/_static/image16.png)

![Liste der SQL Server-VM-images](data-storage-options/_static/image17.png)

Wenn Sie einen virtuellen Computer mit einem SQL Server-Image, anteilsmäßig der SQL Server-Lizenzkosten pro Stunde basierend auf der Nutzung des virtuellen Computers erstellen. Wenn Sie ein Projekt verfügen, die nur für einige Monate ausgeführt wird, ist es günstiger, bezahlen Sie pro Stunde. Wenn Sie, dass Ihr Projekt auf der letzten Jahre vor sich geht glauben, ist es günstiger, um die Lizenz die Möglichkeit zu erwerben, die Sie normalerweise durchführen.

## <a name="summary"></a>Zusammenfassung

Cloud-computing erleichtert sehr praktisch, kombinieren und Übereinstimmung Daten speicheransätzen am besten entsprechen die Anforderungen Ihrer Anwendung. Wenn Sie eine neue Anwendung erstellen, überlegen Sie sorgfältig die Fragen, die hier aufgeführt werden, um Methoden auswählen, die weiterhin funktionieren gut, wenn Ihre Anwendung wächst. Die [im nächsten Kapitel](data-partitioning-strategies.md) wird erläutert, einige Partitionierungsstrategien, mit denen Sie verwenden können, um mehrere Data Storage Ansätze zu kombinieren.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Auswählen einer Datenbankplattform:

- [Datenzugriff für hoch skalierbare Lösungen: mit SQL, NoSQL und Polyglot Persistence](http://aka.ms/dag-doc). E-Book von Microsoft Patterns and Practices, die ausführlich die verschiedenen Arten von Daten zu Gunsten speichert für Cloud-Anwendungen verfügbar.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/ff898430.aspx). Data Consistency Primer, Datenreplikation und Synchronisierung Leitfaden, Muster "Indextabelle", das Muster für materialisierte Sichten angezeigt.
- [BASE: Acid Alternative](http://queue.acm.org/detail.cfm?id=1394128). Der Artikel über die Kompromisse zwischen Konsistenz der Daten und Skalierbarkeit.
- [Sieben Datenbanken in sieben Wochen: eine Anleitung für moderne Datenbanken und NoSQL-Bewegung](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Buch von Eric Redmond und Jim R. Wilson. Dringend empfohlen, für die Einführung von sich selbst auf den Bereich von Data-Speicher-Plattformen erhältlich.

Auswählen zwischen SQLServer und SQL-Datenbank:

- [Leitfaden zur SQL-Datenbank Premium-Vorschau](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Eine Einführung in SQL-Datenbank Premium und Anleitungen dazu, wann Sie sie über die SQL-Datenbank Web und Business Edition auswählen.
- [Richtlinien und Einschränkungen (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Portalseite, die in der Dokumentation zu Einschränkungen bei der SQL-Datenbank verknüpft, unterstützt nicht ein, das SQL Server-Funktionen der SQL-Datenbank behandelt.
- [SQLServer auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Portalseite, die links zur Dokumentation zu SQL Server in Azure ausgeführt wird.
- [Scott Guthrie wird erläutert, SQL-Datenbanken in Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6-minütigen video-Einführung in SQL-Datenbank von Scott Guthrie.
- [Anwendungsmuster und Entwicklungsstrategien für SQLServer auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Verwenden von Entitätsframework und SQL-Datenbank in einer ASP.NET Web-app

- [Erste Schritte mit EF 6 anhand von MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Neun Tutorial Webinaren, das zum Erstellen einer MVC-app, die EF verwendet und stellt die Datenbank von Azure und SQL-Datenbank bereit.
- [ASP.NET-webbereitstellung mithilfe von Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Zwölf Teilen tutorialserie, die in weitere ausführliche Informationen zum Bereitstellen einer Datenbank mithilfe von EF Code First geht.
- [Bereitstellen eine sicheren ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Ausführliches Tutorial, das Sie führt durch die Erstellung einer Web-app, die, das-Authentifizierung verwendet, speichert in der Mitgliedschaftsdatenbank Anwendungstabellen, ändert das Datenbankschema und die app in Azure bereitgestellt wird.
- [ASP.NET Data Access-Inhaltszuordnung](https://go.microsoft.com/fwlink/p/?LinkId=282414). Enthält Links zu Ressourcen zum Arbeiten mit Entity Framework und SQL-Datenbank.

Mithilfe von MongoDB in Azure:

- [MongoLab - MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Dokumentation zu Ausführen von MongoDB auf Azure-Portalseite.
- [Erstellen einer Azure-Website, die auf einem virtuellen Computer in Azure ausgeführten MongoDB herstellt](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Ausführliches Tutorial, das zeigt, wie Sie mit einer MongoDB-Datenbank in eine ASP.NET-Webanwendung.

HDInsight (Hadoop in Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). In der HDInsight-Dokumentation für Portals die [Azure](https://azure.microsoft.com/) Website.
- [Hadoop und HDInsight: Big Data in Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). MSDN Magazine-Artikel von Bruno Terkaly und Ricardo Villalobos, Einführung in Hadoop in Azure.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). MapReduce-Muster finden Sie unter.

> [!div class="step-by-step"]
> [Zurück](single-sign-on.md)
> [Weiter](data-partitioning-strategies.md)
