---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Die Datenspeicheroptionen (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 3eb070167c36db7d8fb2e05af89716ee386b8211
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Die Datenspeicheroptionen (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Die meisten Personen mit relationalen Datenbanken verwendet werden, und sie tendenziell eher andere Datenspeicheroptionen übergangen, wenn sie eine Cloud-app entwerfen. Das Ergebnis kann nicht optimalen Leistung, hohe Kosten oder noch schlimmer, da [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (nicht relationale) Datenbanken können einige Aufgaben effizienter als relationale Datenbanken verarbeiten. Wenn Kunden uns Hilfe lösen von Problemen Speicher wichtiger Daten anfordern, ist es oft, da sie eine relationale Datenbank aufweisen, in denen eine der Optionen NoSQL würde eine bessere Leistung gearbeitet haben. In solchen Situationen hätten Kunden besser, wenn sie die NoSQL-Lösung implementiert hat, vor dem Bereitstellen der app bis hin zur Produktion.

Andererseits, auch wäre es ein Fehler unterläuft, wird davon ausgegangen, dass eine NoSQL-Datenbank mit alles gut oder gut genug ausführen kann. Es empfiehlt sich keine einzelne Data Management für alle Datenaufgaben Speicher; Lösungen für die unterschiedlichen Daten werden für verschiedene Aufgaben optimiert. Die meisten realen Cloud-apps bieten eine Vielzahl von Anforderungen an die datenspeicherung und häufig verarbeitet werden bewährte durch eine Kombination aus mehreren Data Storage-Lösungen.

Der Zweck dieses Kapitels besteht darin Sie ermöglichen einen größeren Eindruck von der Datenspeicheroptionen verfügbar, eine Cloud-app und einige grundlegende Anleitungen zum auswählen, die Ihrem Szenario passt. Es wird empfohlen, beachten Sie die verfügbaren Optionen und ihre Stärken und Schwächen überlegen, bevor Sie eine Anwendung entwickeln. Ändern Datenspeicheroptionen in einer Produktions-app werden äußerst schwierig ist, verfügen Sie über eine Jet-Datenbankmodul zu ändern, während die Ebene übertragen werden.

## <a name="data-storage-options-on-azure"></a>Daten-Speicheroptionen in Azure

Die Cloud erleichtert relativ leicht, eine Vielzahl von relationalen und NoSQL-Datenspeicher verwenden. Hier sind einige der Speicher-Plattformen, die Sie in Azure verwenden können.

![](data-storage-options/_static/image1.png)

Die Tabelle enthält vier Typen von NoSQL-Datenbanken:

- [Schlüssel/Wert-Datenbanken](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec7) ein einzelnes serialisiertes Objekt für jedes Schlüssel-Wert zu speichern. Sie sind für das Speichern von großen Datenmengen, in dem ein Element für einen bestimmten Schlüsselwert abgerufen werden soll, und Sie verfügen nicht über, gute abzufragende basierend auf anderen Eigenschaften des Elements.

    [Azure Blob-Speicher](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/) ist ein Schlüssel-Wert, der z. B. Speichern von Dateien in der Cloud mit Schlüsselwerten fungiert, die Ordner und den Dateinamen entsprechen. Sie rufen Sie eine Datei anhand der Ordner und den Dateinamen, nicht anhand der Werte in den Inhalt der Datei gesucht.

    [Azure-Tabellenspeicher](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/) ist auch eine Schlüssel/Wert-Datenbank. Jeder Wert wird als ein *Entität* (ähnlich wie eine Zeile, die durch einen Partitionsschlüssel und Zeilenschlüssel identifiziert) und enthält mehrere *Eigenschaften* (ähnlich wie Spalten, aber nicht alle Entitäten in einer Tabelle mit den gleichen (Spalten). Abfragen für andere Spalten als der Schlüssel ist äußerst ineffizient und sollte vermieden werden. Sie können z. B. Benutzerprofildaten, mit einer Partition, die Speichern von Informationen zu einem einzelnen Benutzer speichern. Sie können Daten wie Benutzernamen, Kennwort-hashtags, Geburtsdatum usw., in separaten Eigenschaften einer Entität oder in separate Entitäten in derselben Partition speichern. Aber nicht empfehlenswert für alle Benutzer mit einem angegebenen Bereich des Geburtsdaten Abfragen und eine Join-Abfrage nicht werden, zwischen Ihrem Profiltabelle und einer anderen Tabelle ausgeführt kann. Tabellenspeicher ist stärker skalierbare und weniger Kosten als eine relationale Datenbank, jedoch nicht aktivieren, komplexe Abfragen oder Joins.
- [Documentdatabases](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec8) sind Schlüssel/Wert-Datenbanken, die in der die Werte sind *Dokumente*. "Dokument" hier bedeutet eine Auflistung von benannten Felder und Werte, von die jeder ein untergeordnetes Dokument sein könnte aber wird nicht in dem Sinne eines Word- oder Excel-Dokuments verwendet. So möglicherweise ein Dokument Reihenfolge in eine Verlaufstabelle Reihenfolge z. B. Bestellnummer, Bestelldatum und Customer-Felder; und das Kundenfeld möglicherweise Felder Name und Adresse. Die Datenbank codiert Feld-Daten in ein Format wie z. B. XML, YAML, JSON oder BSON; oder es können nur-Text. Eine Funktion, die Dokument abgesehen von Schlüssel/Wert-Datenbanken-Datenbanken ist die Fähigkeit, auf nichtschlüsselfelder abzufragen und definieren sekundäre Indizes, um Abfragen effizienter zu gestalten. Diese Möglichkeit wird eine dokumentdatenbank für Anwendungen, die zum Abrufen von Daten auf Grundlage der Kriterien, die komplexer sind als der Wert des Dokumentschlüssels besser geeignet. Beispielsweise konnte in einem Verkaufsauftrag Dokument Verlaufsdatenbank in verschiedenen Feldern wie z. B. Produkt-ID, Kunden-ID, Kundennamen usw. abgefragt. [MongoDB](http://www.mongodb.org/) ist eine beliebte dokumentdatenbank.
- [Spalte-Familie Datenbanken](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec9) sind Schlüssel/Wert-Datenspeicher, mit denen Sie Struktur die Speicherung von Daten in Auflistungen der verknüpften Spalten, die Spalte Familien aufgerufen. Z. B. eine Census-Datenbank möglicherweise eine Gruppe von Spalten für den Namen einer Person (Vorname, zweiter Vorname, letzte), eine Gruppe für die Adresse einer Person und eine Gruppe für die Person-Profilinformationen (Geburtsdatum, Geschlecht usw.). Der Datenbank kann jede Familie Spalte klicken Sie dann in einer separaten Partition gespeichert, während bleiben alle Daten für eine Person, die im Zusammenhang mit dem gleichen Schlüssel. Anschließend können Sie alle Profilinformationen lesen, ohne alle Name und Adresse Informationen sowie zu lesen. [Cassandra](http://cassandra.apache.org/) ist eine beliebte Spalte-Familie-Datenbank.
- [Graph Datenbanken](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec10) Informationen als eine Auflistung von Objekten und Beziehungen zu speichern. Der Zweck einer Graph-Datenbank ist so aktivieren Sie eine Anwendung zum effizienten Ausführen von Abfragen, die im Netzwerk von Objekten und die Beziehungen zwischen ihnen zu durchlaufen. Z. B. die Objekten handelt es sich möglicherweise um Mitarbeiter in einer Datenbank der Personalabteilung, und sollten Sie zur Erleichterung Abfragen wie z. B. "suchen alle Mitarbeiter, die direkt oder indirekt arbeiten für Scott." [Neo4j](http://www.neo4j.org/) ist eine beliebte Graph-Datenbank.

Im Vergleich zu relationalen Datenbanken, bieten die NoSQL-Optionen sehr viel größere Skalierbarkeit und Kosteneffizienz für Speicherung und Analyse von unstrukturierten Daten. Der Nachteil besteht darin, dass sie die umfangreiche Queryability und zuverlässigen Integritätsfunktionen von relationalen Datenbanken nicht bereitstellen. NoSQL funktioniert gut für IIS-Protokolldaten, die große mit keine Notwendigkeit für Join-Abfragen umfasst. NoSQL funktioniert nicht so gut für Banktransaktionen, die absolute Datenintegrität erfordert und umfasst viele Beziehungen zu anderen Daten beziehen.

Es gibt auch eine neuere Kategorie von Datenbankplattform aufgerufen [NewSQL](http://en.wikipedia.org/wiki/NewSQL) kombiniert, die die Skalierbarkeit einer NoSQL-Datenbank, mit dem Queryability und Transaktionsintegrität einer relationalen Datenbank. NewSQL Datenbanken dienen für verteilte datenspeicherung und abfrageverarbeitung, was häufig schwer in "OldSQL" Datenbanken implementiert ist. [NuoDB](http://www.nuodb.com/) ist ein Beispiel für eine NewSQL-Datenbank, die in Azure verwendet werden kann.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop- und MapReduce

Die große Datenmengen in NoSQL-Datenbanken gespeichert werden können möglicherweise schwierig, effizient in kürzester Zeit zu analysieren. Möchten Sie die Verwendung von einem Framework, z. B. [Hadoop](http://hadoop.apache.org/) implementiert [MapReduce](http://en.wikipedia.org/wiki/MapReduce) Funktionalität. Im Wesentlichen lautet der ein MapReduce-Prozess ist wie folgt:

- Grenzwert für die Größe der Daten, die durch Auswahl aus den Daten, die verarbeitet werden müssen, speichern nur Daten, die Sie tatsächlich benötigen, zu analysieren. Sie möchten z. B. die Zusammensetzung der Basis von Geburtsjahr, Benutzer kennen, damit Sie nur Geburtsdatum Jahre aus Ihrer Benutzer Profildatenspeicher auswählen.
- Die Daten in Bereiche unterteilen und auf verschiedenen Computern für die Verarbeitung zu senden. Computer A, berechnet die Anzahl der Personen mit 1950 1959 Datumsangaben, Computer B ist 1960-1969 usw. Diese Gruppe von Computern wird bezeichnet eine *Hadoop-Cluster*.
- Fügen Sie die Ergebnisse der einzelnen Teile wieder zusammen, nachdem die Verarbeitung auf die Teile abgeschlossen ist. Sie verfügen nun über eine relativ kurze Liste der für jede Geburtsjahr wie viele Personen und die Aufgabe zur Berechnung der Prozentsätze in der gesamten Liste kann verwaltet.

In Azure [HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) ermöglicht es Ihnen, verarbeiten, analysieren und neue Einblicke big Data-verwenden die Leistungsfähigkeit von Hadoop. Beispielsweise können Sie es verwenden, um Webserverprotokolle zu analysieren:

- Aktivieren Sie die webserverprotokollierung in Ihr Speicherkonto. Damit Azure so eingerichtet Protokolle in den Blob-Dienst für jede HTTP-Anforderung für Ihre Anwendung zu schreiben. Der Blob-Dienst ist im Grunde genommen Cloud Dateispeicher und ordentlich in HDInsight integriert. 

    ![Die Protokolle, um Blob-Speicher](data-storage-options/_static/image2.png)
- Wie die app Datenverkehr erhält, werden Web-Server-IIS-Protokolle in den Blob-Speicher geschrieben. 

    ![Die Webserverprotokolle](data-storage-options/_static/image3.png)
- Klicken Sie im Portal auf **neu** - **Datendienste** - **HDInsight** - **Schnellerfassung**, und geben Sie einen HDInsight-Clusternamen, Clustergröße (Anzahl der Datenknoten für HDInsight-Cluster), und einen Benutzernamen und Kennwort für den HDInsight-Cluster. 

    ![HDInsight](data-storage-options/_static/image4.png)

Sie können jetzt MapReduce-Aufträge, um Ihre Protokolle analysieren und erhalten Sie Antworten auf Fragen wie z. B. festlegen:

- Welche Tageszeiten abrufen Meine app den Datenverkehr für die meisten oder wenigsten?
- Welche Länder ist mein Datenverkehr aus?
- Was ist die durchschnittliche Nachbarschaft Einkommen der Bereiche, denen Datenverkehr stammen. (Es ist eine öffentliche Dataset, das Sie Nachbarschaft Einkommen über IP-Adresse erhalten, und für IP-Adresse in der Webserverprotokolle übereinstimmen,.)
- Wie setzen Nachbarschaft Einkommen auf bestimmte Seiten oder Produkte in der Website in Beziehung?

Anschließend können Sie die Antworten auf Fragen, wie diese zielgerichtet basierend auf der Wahrscheinlichkeit, der ein Kunde interessiert wäre oder Wahrscheinlichkeit, dass ein bestimmtes Produkt kaufen.

Wie in beschrieben die [automatisieren alles Kapitel](automate-everything.md), die meisten Funktionen, die Sie im Portal können automatisiert werden und, umfasst das Einrichten und Ausführen von HDInsight Analysis-Aufträge. Ein typisches HDInsight-Skript enthalten die folgenden Schritte aus:

- Bereitstellen Sie einen HDInsight-Cluster, und verknüpfen Sie es in das Speicherkonto für die Eingabe der Blob-Speicher.
- Hochladen der MapReduce-Auftrags ausführbare Dateien (JAR oder .exe-Dateien) mit dem HDInsight-Cluster an.
- Senden Sie eine MapReduce, in dem die Ausgabedaten in den Blob-Speicher gespeichert.
- Warten Sie auf den Abschluss des Auftrags.
- Löschen Sie den HDInsight-Cluster.
- Zugriff auf die Ausgabe aus dem Blob-Speicher.

Durch Ausführen eines Skripts, das alle dies tut, minimieren Sie die Zeitspanne, die der HDInsight-Cluster bereitgestellt wird, die die Kosten minimiert.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platform as a Service (PaaS) im Vergleich zu Infrastructure as a Service (IaaS)

Die oben aufgelisteten Berechtigungskategorien Datenspeicheroptionen umfassen Platform-as-a-Service (PaaS) und Infrastructure-as-a-Service (IaaS)-Lösungen. PaaS bedeutet, dass wir die Infrastruktur von Hardware und Software verwalten und Sie einfach den Dienst verwenden. SQL-Datenbank ist eine PaaS-Funktion von Azure. Für Datenbanken werden aufgefordert, und im Hintergrund Azure eingerichtet und konfiguriert die virtuellen Computer, und richtet die Datenbanken auf. Sie haben keinen direkten Zugriff auf die virtuellen Computer und nicht für deren Verwaltung. IaaS bedeutet, dass Sie einrichten, konfigurieren und Verwalten von virtuellen Computern, die in unserer Infrastruktur von Rechenzentren ausgeführt, und Sie beliebig darauf ablegen. Wir stellen eine Sammlung von vorkonfigurierten VM-Images für häufig verwendete Konfigurationen der virtuellen Computer bereit. Beispielsweise können Sie die vorkonfigurierten VM-Images für Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle-Datenbank usw. installieren.

Azure bietet PaaS-Data-Lösungen umfassen:

- Azure SQL-Datenbank (früher SQL Azure). Eine Cloud relationale Datenbank auf SQL Server basieren.
- Azure-Tabellenspeicher. Ein Schlüssel/Wert NoSQL-Datenbank.
- Azure Blob-Speicher. Speichern von Dateien in der Cloud.

Für IaaS verwenden können Sie alle Elemente ausführen, die Sie auf einem virtuellen Computer, z. B. laden können:

- Relationale Datenbanken wie SQL Server, Oracle, MySQL, SQL Compact, SQLite und Postgres.
- Schlüssel/Wert-Datenspeicher wie z. B. Memcached, Redis Cassandra und Riak.
- Spaltendaten Datenspeicher wie z. B. HBase.
- Dokumentieren Sie Datenbanken beispielsweise MongoDB, RavenDB und CouchDB.
- Graph-Datenbanken, wie z. B. Neo4j.

![Daten-Speicheroptionen in Azure](data-storage-options/_static/image5.png)

Die IaaS-Option ermöglicht Ihnen nahezu unbegrenzte Datenspeicheroptionen, und viele von ihnen sind besonders einfach zu verwenden, da Sie virtuelle Computer mit vorkonfigurierten Images erstellen können. Wechseln Sie beispielsweise im Verwaltungsportal zu **VMs**, klicken Sie auf die **Bilder** Registerkarte, und klicken Sie auf **VM-Depot Durchsuchen**.

![VM-Depot durchsuchen](data-storage-options/_static/image6.png)

Sie sehen dann eine Liste der [Hunderte von vorkonfigurierten VM-Images](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), und Sie können einen virtuellen Computer aus einem Image, das ein Datenbank-Managementsystem vorinstalliert verfügt, z. B. MongoDB, Neo4J, Redis, Cassandra oder CouchDB erstellen:

![MongoDB im VM-Depot](data-storage-options/_static/image7.png)

Azure IaaS-Datenspeicheroptionen macht so einfach wie möglich zu verwenden, aber die PaaS-Angebote haben viele Vorteile, die sie kostengünstige und für viele Szenarien praktisch machen:

- Sie müssen keine virtuellen Computer zu erstellen, Sie nur das Portal oder ein Skript zum Einrichten von eines Datenspeichers verwenden. Gegebenenfalls einen Datenspeicher 200 TB Sie können nur auf eine Schaltfläche oder einen Befehl ausführen, und in Sekunden für die Verwendung bereit ist.
- Sie müssen keine verwalten und Patchen die virtuellen Computer, die vom Dienst verwendete; Microsoft übernimmt, die für Sie automatisch.-Sie müssen keine Gedanken machen, das Einrichten der Infrastruktur für die Skalierung oder hohe Verfügbarkeit; Microsoft erledigt alles für Sie.
- Sie müssen keine Erwerb zusätzlicher Lizenzen; Lizenzgebühren sind in der Dienstgebühren enthalten.
- Sie Zahlen nur für die tatsächliche Verwendung.

PaaS-Daten-Speicheroptionen in Azure sind Angebote von Drittanbietern. Beispielsweise können Sie wählen die [MongoLab-Add-On](https://azure.microsoft.com/en-us/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) aus dem Azure-Speicher eine MongoDB-Datenbank als Dienst bereitstellen.

## <a name="choosing-a-data-storage-option"></a>Wählen eine Option zum Speichern von Daten

Keine ein Ansatz besteht darin, die für alle Szenarien geeignet ist. Wenn jede Person anzeigt, dass diese Technologie die Antwort ist, wird als Erstes zu Fragen "Was die Frage ist?", da verschiedene Lösungen für unterschiedliche Dinge optimiert sind. Es gibt bestimmte Vorteile für das relationale Modell; daher so lange existiert bereits. Es sind jedoch ebenfalls Down-Seiten in SQL, die mit einer NoSQL-Lösung behoben werden können.

Häufig angezeigt wird, was wir funktionieren am besten eine festgelegte Ansatz ist, in dem Sie SQL und NoSQL in einer einzigen Projektmappe Selbst wenn Personen sagen sie sind Hinwendung NoSQL, wenn Sie einen Drilldown in was sie Sie häufig tun feststellen, dass sie verschiedene NoSQL-Frameworks verwenden: Verwenden sie [CouchDB](http://wiki.apache.org/couchdb/Introduction), und [Redis](http://redis.io/), und [ Riak](http://basho.com/riak/) für unterschiedliche Dinge. Sogar Facebook, dem NoSQL sehr häufig verwendet wird, verwendet verschiedene NoSQL-Frameworks für die verschiedenen Teile des Diensts. Die Flexibilität, mischen und Zuordnen von Daten Speicher Ansätze ist eine der Aufgaben, die über die Cloud nice des einfach mehrere Data-Lösungen und in einer app zu integrieren.

Hier sind einige Fragen zu bedenken, wenn Sie einen Ansatz entscheiden:

| Semantische Daten | -Der zentrale Speicher- und Datenzugriff semantische Neuigkeiten (Speichern Sie relationale oder unstrukturierte Daten)? Unstrukturierte Daten wie z. B. Mediendateien passt sich am besten im Blob-Speicher; eine Auflistung von verknüpften Daten, z. B. Produkte, Inventuren, Lieferanten, kundenbestellungen usw., passt sich am besten in einer relationalen Datenbank. |
| --- | --- |
| Unterstützung für Abfragen | – Ist wie leicht Ihnen die Daten Abfragen? -Welche Arten von Fragen effizient sein können gestellte? Schlüssel/Wert-Datenspeicher sind sehr gut an eine einzelne Zeile mit dem angegebenen Schlüssel-Wert abrufen, aber nicht so gut für komplexe Abfragen. Für einen Benutzer Profildatenspeicher, in dem Sie immer die Daten für einen bestimmten Benutzer abrufen, kann ein Schlüssel/Wert-Datenspeicher gut funktionieren; für einen Produktkatalog, in denen unterschiedliche Gruppierungen basierend auf verschiedenen Produktattribute abgerufen werden soll, funktionieren möglicherweise eine bessere Leistung eine relationale Datenbank. NoSQL-Datenbanken speichern große Mengen an Daten effizient pivotiert werden, jedoch müssen Sie die Struktur der Datenbank um, wie die Anwendung die Daten abfragt, und Dies erschwert ad-hoc-Abfragen möchten. Mit einer relationalen Datenbank können Sie nahezu jede Art von Abfrage erstellen. |
| Funktionale Projektion | -Fragen können, werden Aggregationen usw., ausgeführte serverseitige? Wenn ich, wählen Sie die Anzahl ausführen (\*) aus einer Tabelle in SQL, er sehr effizient erledigen die gesamte Arbeit auf dem Server und zurück, die ich sehe für wird. Wenn ich die gleiche Berechnung aus einem NoSQL-Datenspeicher, die keine Aggregation unterstützt möchte, eine ineffiziente "unbounded Abfrage" ist es wahrscheinlich einen Timeout beendet wird. Selbst wenn die Abfrage erfolgreich ist müssen alle Daten vom Server an den Client abrufen und zählen der Zeilen auf dem Client. – Welche Sprachen oder Typen von Ausdrücken verwendet werden können? Mit einer relationalen Datenbank kann ich SQL verwenden. Mit einigen NoSQL-Datenbanken, z. B. Azure-Tabellenspeicher, ich werde verwenden [OData](http://www.odata.org/), und ich kann lediglich auf primären Schlüssel zu filtern und Projektionen (Wählen Sie eine Teilmenge der verfügbaren Felder) abrufen. |
| Die mühelose Skalierbarkeit | – Wie häufig und wie viel werden die Daten müssen skalieren? -Implementiert die Plattform systemintern mit horizontaler Skalierung? – Ist wie einfach es, hinzufügen und Entfernen von Kapazität (Größe und den Durchsatz)? Relationale Datenbanken und Tabellen werden nicht automatisch partitioniert, um diese skalierbar, stellen daher schwierig zu skalieren, die über bestimmte Einschränkungen sind. NoSQL-Datenspeicher wie Azure-Tabellenspeicher partitionieren alles grundsätzlich, und es gibt fast keine Beschränkung für die Partitionen hinzufügen. Können Sie leicht Tabellenspeicher bis zu 200 TB skalieren, aber die maximale Datenbankgröße für Azure SQL-Datenbank beträgt 500 GB. Können Sie relationale Daten skalieren, indem Sie sie in mehrere Datenbanken partitionieren, aber einen Großteil der Arbeit Programmierung Einrichten einer Anwendung zur Unterstützung dieses Modells umfasst. |
| Instrumentation und verwaltbarkeit | – Ist wie einfach die Plattform instrumentieren, überwachen und verwalten? Sie benötigen, halten Sie sich über die Integrität und Leistung von Ihrer Datenspeicher, daher müssen Sie vorab wissen, welche Metriken eine Plattform Sie kostenlos erhalten, und was Sie selbst entwickelt haben. |
| Vorgänge | – Ist wie einfach die Bereitstellung und Ausführung auf Azure-Plattform? PaaS? IaaS? Linux? Tabellenspeicher und SQL-Datenbank sind einfach in Azure einzurichten. Plattformen, die integrierte Azure-PaaS-Lösungen sind nicht erforderlich, mehr Aufwand. |
| API-Unterstützung | – Dies ist eine API, die mit der Plattform arbeiten erleichtert? Für den Azure-Tabellendienst ist eine SDK mit einer .NET API, die das asynchrone Programmiermodell in .NET 4.5 unterstützt. Wenn Sie eine app .NET schreiben, müssen das zum Schreiben und Testen von Code für den Azure-Tabellendienst im Vergleich zu einem anderen Schlüssel/Wert-Spalte Store Datenplattform, die keine API oder eine weniger umfangreiche hat viel einfacher sein. |
| Transaktionskonsistenz Integrität und Daten | -Ist es wichtig, dass die Plattform Transaktionen unterstützen, um die Gewährleistung der Datenkonsistenz? Zum Nachverfolgen der Massen-e-Mails gesendet, Leistung und geringe Daten Speicherkosten wichtiger als automatische Unterstützung für Transaktionen oder die referenzielle Integrität in das Datenplattform möglicherweise ausführenden Azure-Tabellendienst eine gute Wahl. Zum Nachverfolgen von Bankkonto im Gleichgewicht sind, oder Bestellungen eine relationale Datenbankplattform, die bereitstellt, dass transaktionale garantiert, dass eine bessere Wahl sein würde. |
| Geschäftskontinuität | – Wie einfach Sicherung, Wiederherstellung und notfallwiederherstellung werden? Früher oder später Produktionsdaten beschädigt werden, und Sie benötigen eine Rückgängigfunktion. Relationale Datenbanken haben häufig weitere differenzierte Restore-Funktionen, z. B. die Möglichkeit, bis zu einem bestimmten Zeitpunkt wiederherzustellen. Verstehen, welche Wiederherstellungsfunktionen in jede Plattform verfügbar sind, die Sie in Erwägung ziehen, ist ein wichtiger Faktor zu berücksichtigen. |
| Kosten | – Wenn mehr als eine Plattform Ihrer arbeitsauslastung Daten unterstützen, wie vergleichen sie Kosten für das? Wenn Sie ASP.NET Identity verwenden, können Sie z. B. Benutzerprofildaten in Azure-Tabellendienst oder Azure SQL-Datenbank speichern. Wenn Sie die umfassenden Funktionen von SQL-Datenbank Abfragen benötigen, empfiehlt sich Azure-Tabellen Teil, da es viel kostet weniger für eine bestimmte Menge des Speichers. |

Im Allgemeinen empfohlen wird die Antwort auf die Fragen in jeder dieser Kategorien kennen, bevor Sie Ihre Daten speicherlösungen auswählen.

Darüber hinaus möglicherweise Ihre arbeitsauslastung bestimmte Anforderungen, die eine bessere Leistung als andere einige Plattformen unterstützen können. Zum Beispiel:

- Werden Funktionen für Ihre Anwendung erforderlich überwacht?
- Was sind Ihre Langlebigkeit datenanforderungen--benötigen Sie eine automatische Archivierung "oder" Entfernen von Funktionen?
- Haben Sie die speziellen Anforderungen? Z. B. die Daten einschließlich personenbezogener Informationen (persönlich identifizierbare Informationen), aber haben Sie in der Lage, um sicherzustellen, dass Abfrageergebnisse PII ausgeschlossen wird.
- Wenn Sie einige Daten, die in der Cloud behördlichen oder technologischen Gründen gespeichert werden kann haben, benötigen Sie möglicherweise eine Cloud-Speicher-Datenplattform, die erleichtert die Integration mit dem lokalen Speicher.

## <a name="demo--using-sql-database-in-azure"></a>Demo – verwenden in Azure SQL-Datenbank

Die app zu beheben verwendet eine relationale Datenbank, um Aufgaben zu speichern. Die Umgebung erstellen Windows PowerShell-Skript angezeigt, der [automatisieren alles Kapitel](automate-everything.md) zwei Instanzen von SQL-Datenbank erstellt. Diese im Portal können Sie sehen, indem Sie auf die **SQL-Datenbanken** Registerkarte.

![SQL-Datenbanken im Verwaltungsportal](data-storage-options/_static/image8.png)

Es ist auch zum Erstellen von Datenbanken mithilfe des Verwaltungsportals einfach.

Klicken Sie auf **neue--Datendienste** -- **SQL-Datenbank** -- **Schnellerfassung**, geben Sie einen Datenbanknamen ein, wählen Sie einen Server, die bereits in Ihrem Konto oder erstellen Sie ein neues Konto, und klicken Sie auf **SQL-Datenbank erstellen**.

![Neue SQL-Datenbank](data-storage-options/_static/image9.png)

Warten Sie einige Sekunden, und Sie eine Datenbank in Azure für Ihre Verwendung bereit.

![Neu erstellte SQL-Datenbank](data-storage-options/_static/image10.png)

Damit Azure in ein paar Sekunden was es Sie einen Tag dauern kann, oder eine Woche oder länger in der lokalen Umgebung zu erreichen. Und da Sie genauso einfach Datenbanken automatisch in einem Skript oder mithilfe einer Verwaltungs-API erstellen können, Sie können dynamisch zu skalieren durch das Verteilen von Daten in mehreren < o: p > Datenbanken so lange für, die Ihre Anwendung programmiert. </o : p >

Dies ist ein Beispiel für unsere Platform-as-a-Service-Modell. Sie müssen den Server zu verwalten, wir dafür. Sie keine Sicherungen kümmern, wir dafür. Es läuft in hohe Verfügbarkeit – die Daten in der Datenbank werden automatisch auf drei Servern repliziert. Wenn ein Computer nicht mehr aktiv ist, wird automatisch ein Failover aus, und Sie verlieren keine Daten. Der Server wird regelmäßig gepatcht, müssen Sie nicht kümmern.

Klicken Sie auf eine Schaltfläche, und die genaue Verbindungszeichenfolge müssen und können sofort verwenden, die neue Datenbank erhalten.

![Verbindungszeichenfolgen](data-storage-options/_static/image11.png)

Das Dashboard zeigt Ihnen, verbindungsverlauf und genutzten Speicher.

![SQL-Datenbank-Dashboard](data-storage-options/_static/image12.png)

Sie können die Datenbanken im Verwaltungsportal verwalten oder mithilfe von SQL Server-Tools sind Sie bereits vertraut mit, einschließlich SQL Server Management Studio (SSMS) und Visual Studio-Tools, SQL Server-Objekt-Explorer (SSOX) und Server-Explorer.

![SSOX](data-storage-options/_static/image13.png)

Eine andere Vorteil ist das Preismodell. Beginnen Sie Entwicklung, mit der eine kostenlose 20-MB-Datenbank und eine Produktionsdatenbank beginnt bei ca. 5 $ pro Monat. Sie Zahlen nur für die Menge an Daten, die Sie tatsächlich in der Datenbank, nicht die maximale Kapazität speichern. Sie müssen eine Lizenz erwerben.

SQL-Datenbank ist einfach zu skalieren. Die Datenbank, die wir in unserer Automatisierungsskripts erstellen ist für die app korrigieren auf 1 GB Obergrenze. Wenn sie bis zu 150 GB vergrößert oder verkleinert werden soll, Sie einfach in das Portal wechseln und diese Einstellung oder einen REST-API-Befehl ausführen und in Sekunden, die Sie haben eine 150 GB-Datenbank, der Sie Daten in bereitstellen können.

![SQL-Datenbankeditionen und-Größen](data-storage-options/_static/image14.png)

Dies ist die Leistungsfähigkeit der Cloud, um schnell und einfach die Infrastruktur aufrecht, und starten Sie sofort verwenden zu können.

Die app zu beheben verwendet wird, zwei SQL-Datenbanken, für die Mitgliedschaft (Authentifizierung und Autorisierung) und eine für Daten, und dies ist alles, was zum Bereitstellen und skalieren erfordern. Sie haben bereits gesehen die Datenbanken mithilfe von Windows PowerShell-Skripts, und nachdem Sie nun auch kennengelernt haben wie einfach es ist, führen Sie das Portal bereitstellen.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework im Vergleich zur direkten Datenbankzugriff mithilfe von ADO.NET

Die app zu beheben greift auf diese Datenbanken mithilfe von Entity Framework, Microsofts empfohlen ORM (objektrelationales Mapper) für .NET-Anwendungen. Ein ORM ist ein großartiges Tool, das die Produktivität der Entwickler vereinfacht, aber Produktivität auf Kosten der Beeinträchtigung der Leistung in einigen Szenarien kommt. In einer realen Cloud-app wird nicht und eine Auswahl zwischen der Verwendung von EF oder direkt mithilfe von ADO.NET – Sie beides verwenden. In den meisten Fällen, wenn Sie Code schreiben, die mit der Datenbank, Abrufen der maximalen Leistung ist nicht wichtig, und profitieren Sie die vereinfachte Codierung und zu testen, dass Sie mit dem Entity Framework erhalten. In Situationen, in denen EF Mehraufwand inakzeptablen Leistung führen würde, können Sie schreiben und Ausführen von Abfragen mithilfe von ADO.NET, idealerweise durch Aufrufen von gespeicherten Prozeduren.

Geeignete Methode für den Zugriff auf die Datenbank möchten "Chattiness" so weit wie möglich zu minimieren. Wenn Sie erhalten alle benötigten Daten in eine größere Abfrageergebnis anstatt Dutzende oder sogar Hunderte von kleinere festgelegt, ist dies also generell. Wenn Sie müssen Liste Studenten und die Kurse aus, in diese registriert wurden, ist es z. B. in der Regel besser zum Abrufen aller Daten in einer Join-Abfrage, sondern Studenten in einer Abfrage abrufen und für jede Student Kurse eine eigene Abfrage ausführen.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL-Datenbanken und das Entity Framework in der app zu beheben

In der app Korrigieren der `FixItContext` -Klasse, die das Entity Framework abgeleitet `DbContext` -Klasse, identifiziert die Datenbank und gibt die Tabellen in der Datenbank. Der Kontext gibt eine Entitätenmenge (Tabelle) für Vorgänge an, und an den Kontext der Verbindungszeichenfolgenname übergibt der Code. Dieser Name bezieht sich auf eine Verbindungszeichenfolge, die in der Datei "Web.config" definiert ist.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Die Verbindungszeichenfolge in der *"Web.config"* Datei heißt Appdb (Hier zeigen Sie die lokalen Entwicklungs-Datenbank):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Erstellt das Entity Framework ein *FixItTasks* Tabelle basierend auf Eigenschaften auf der `FixItTask` Entitätsklasse. Dies ist eine einfache POCO (Plain Old CLR-Objekt)-Klasse, d. h., es ist nicht oder davon erben keine Abhängigkeiten haben, auf dem Entity Framework. Aber Entity Framework weiß, wie eine Tabelle basierend auf diesen zu erstellen und Ausführen von CRUD-Vorgänge (erstellen-lesen-Update / Delete) mit.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks-Tabelle](data-storage-options/_static/image15.png)

Die app zu beheben umfasst eine Repository-Schnittstelle, die für die Arbeit mit dem Datenspeicher CRUD-Vorgänge verwendet werden.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Beachten Sie, dass die Repository-Methoden alle Async, damit der gesamte Datenzugriff auf eine vollständig asynchrone Weise erfolgen kann.

Die Repository-Implementierung Aufrufe Entity Framework-Async-Methoden zum Arbeiten mit den Daten, einschließlich LINQ-Abfragen als auch für INSERT-, Update- und Löschvorgänge. Hier ist ein Beispiel für den Code für das Suchen nach einer Aufgabe zu beheben.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Sie werden bemerken, es gibt auch einige zeitlichen Steuerung und Protokollierungscode hier Fehler, betrachten wir, die weiter unten in der [Kapitel Überwachung und Telemetrie](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Wählen im Vergleich zu SQLServer auf einem virtuellen Computer (IaaS) in Azure SQL-Datenbank (PaaS)

Ein Vorteil von SQL Server und Azure SQL-Datenbank ist, dass das Core-Programmiermodell für beide Identitäten identisch ist. Sie können die meisten Qualifikation in beiden Umgebungen verwenden. Sie können sogar ein SQL Server-Datenbank in der Entwicklung und eine SQL-Datenbankinstanz in der Cloud ist wie die app zu beheben eingerichtet ist.

Als Alternative können Sie dieselbe SQL Server in der Cloud ausführen, dass Sie lokal ausführen, indem Sie sie auf virtuellen IaaS-Computern installieren. Für einige älteren Anwendungen mit SQL Server auf einem virtuellen Computer eine bessere Lösung möglicherweise. Da SQL Server-Datenbank auf einem dedizierten virtuellen Computer ausgeführt wird, hat er mehr als eine Datenbank der SQL-Datenbank, die auf einem freigegebenen Server ausgeführt wird, verfügbaren Ressourcen. Bedeutet, dass SQL Server-Datenbank kann größer sein und auch noch ausführen. Im Allgemeinen funktioniert je kleiner die Größe der Datenbank und die Tabellengröße, desto besser der Anwendungsfall für SQL-Datenbank (PaaS).

Es folgen einige Richtlinien zum Auswählen zwischen den beiden Modellen.

| Azure SQL-Datenbank (PaaS) | SQLServer auf einem virtuellen Computer (IaaS) |
| --- | --- |
| **IT-Spezialisten** -nicht stehen Ihnen zum Erstellen oder Verwalten von virtuellen Computern, aktualisieren oder patch OS oder SQL; Azure übernimmt diese Aufgabe für Sie. -Integrierte hohe Verfügbarkeit mit einer SLA auf Datenbankebene. -Niedrige Gesamtbetriebskosten (TCO), da Sie Zahlen nur für Sie (keine Lizenz erforderlich) verwenden. -Gut für die Behandlung zahlreiche kleinere Datenbanken (&lt;= 500 GB). -Einfache dynamisch erstellen mit horizontaler Skalierung neue Datenbanken zu aktivieren. | ***IT-Spezialisten*** - Funktion mit einem kompatiblen mit einer lokalen SqlServer. -SQL Server können implementieren [hohe Verfügbarkeit mit AlwaysOn](https://www.microsoft.com/en-us/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) in 2 + VMs mit VM-Ebene SLA. -Sie haben die vollständige Kontrolle über wie SQL verwaltet wird. -SQL-Lizenzen erneut können, die Sie bereits besitzen, die oder für eine Stunde bezahlen. -Sich gut für die Behandlung von weniger aber größere (1 TB) Datenbanken. |
| **Nachteile** -einige Lücken im Vergleich zur lokalen SQL Server-feature (fehlende [CLR-Integration](https://technet.microsoft.com/en-us/library/ms131102.aspx), [TDE](https://technet.microsoft.com/en-us/library/bb934049.aspx), [komprimierungsunterstützung](https://technet.microsoft.com/en-us/library/cc280449.aspx), [SQL Server-Berichtsdienste](https://technet.microsoft.com/en-us/library/ms159106.aspx)usw.)-Datenbankgröße einzuhalten 500 GB. | ***Nachteile*** - Updates/Patches ("Betriebssystem" und "SQL") sind Ihrer Verantwortung - Erstellung und Verwaltung von Datenbanken Ihrer Verantwortung - Datenträger-IOPS (e/a-Vorgänge pro Sekunde) auf ca. 8000 (über 16 Datenlaufwerke) beschränkt. |

Wenn Sie SQL Server auf einem virtuellen Computer verwenden möchten, können Sie Ihre eigene SQL Server-Lizenz oder Sie können für eine pro Stunde bezahlen. Im Portal oder über die REST-API können Sie z. B. einen neuen virtuellen Computer mit einem SQL Server-Image erstellen.

![Erstellen Sie die virtuellen Computer mit SQLServer](data-storage-options/_static/image16.png)

![Liste der SQL Server-VM-images](data-storage-options/_static/image17.png)

Wenn Sie einen virtuellen Computer mit einem SQL Server-Image, wir Dienstlizenz die Kosten der SQL Server-Lizenz pro Stunde basierend auf Ihrer Nutzung des virtuellen Computers erstellen. Wenn Sie ein Projekt, die nur für einige Monate ausführen soll verfügen, ist es günstiger, die pro Stunde bezahlen. Wenn Sie, dass Ihr Projekt Jahre bis zum letzten geht glauben, ist es günstiger die Lizenz erwerben Sie die Möglichkeit, die Sie normalerweise ausführen.

## <a name="summary"></a>Zusammenfassung

Cloud computing, daher ist es sinnvoll, mischen und Übereinstimmung Daten Speicher Ansätze am besten passen die Anforderungen Ihrer Anwendung. Wenn Sie eine neue Anwendung erstellen, überlegen Sie sorgfältig die Fragen, die hier aufgeführten um Ansätze auswählen, die weiterhin funktionieren gut, wenn Ihre Anwendung vergrößert wird. Die [nächsten Kapitels](data-partitioning-strategies.md) wird erläutert, einige Partitionierungsstrategien, mit denen Sie mehrere Data Storage Ansätze kombinieren können.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Auswählen einer Datenbankplattform:

- [Datenzugriff für hoch skalierbare Lösungen: Verwenden von SQL, NoSQL und Polyglot Persistenz](http://aka.ms/dag-doc). E-Book vom Microsoft Patterns and Practices, die die verschiedenen Arten von Daten im Detail Gunsten speichert für Cloud-Anwendungen verfügbar.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/en-us/library/ff898430.aspx). Einführung in Konsistenz der Daten, die Datenreplikation und Synchronisierung Anleitungen Indextabelle Muster, materialisierte Sicht Muster angezeigt.
- [Basis-: Acid Alternative](http://queue.acm.org/detail.cfm?id=1394128). Artikel zur vor-und Nachteile zwischen Datenkonsistenz und Skalierbarkeit.
- [Sieben Datenbanken in sieben Wochen: eine Führungslinie für moderne Datenbanken und die Verschiebung der NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Buch von Eric Redmond und Jim R. Wilson. Dringend empfohlen, für die Einführung selbst auf den Bereich des Daten-Speicherplattformen heute verfügbar.

Auswählen zwischen SQLServer und SQL-Datenbank:

- [Leitfaden zur SQL-Datenbank Premium-Vorschau](https://msdn.microsoft.com/en-us/library/windowsazure/dn369873.aspx). Einführung in SQL-Datenbank Premium und Hinweise dazu, wann sie über die SQL-Datenbank für Web und Business Edition auswählen.
- [Richtlinien und Einschränkungen (Azure SQL-Datenbank)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394102.aspx). Portalseite, die in der Dokumentation zu Beschränkungen der SQL-Datenbank verknüpft, unterstützt z. B. eine, die auf SQL Server-Funktionen der SQL-Datenbank konzentriert sich nicht.
- [SQLServer auf Azure Virtual Machines](https://msdn.microsoft.com/en-us/library/windowsazure/jj823132.aspx). Portalseite, die links in der Dokumentation zu SQL Server in Azure ausgeführt wird.
- [Scott Guthrie erklärt SQL-Datenbanken in Azure](https://azure.microsoft.com/en-us/documentation/videos/sql-in-azure-scottgu/). 6-Minuten-video: Einführung in SQL-Datenbank von Scott Guthrie.
- [Anwendungsmuster und Entwicklungsstrategien für SQLServer auf Azure Virtual Machines](https://msdn.microsoft.com/en-us/library/windowsazure/dn574746.aspx).

Verwenden von Entity Framework und SQL-Datenbank in einer ASP.NET Web-app

- [Erste Schritte mit EF 6 mit MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Neun zweiteilige Reihe von Lernprogrammen, die führt Sie schrittweise durch das Erstellen einer MVC-app, die mithilfe von EF und stellt die Datenbank in Azure und SQL-Datenbank bereit.
- [ASP.NET Web-Bereitstellung mit Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Zwölf zweiteilige Reihe von Lernprogrammen, die mehr ins Detail Informationen zum Bereitstellen einer Datenbank mithilfe von EF Code First wechselt.
- [Eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website bereitstellen](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Schrittweises Lernprogramm, das Sie über eine Web-app erstellen, führt-Authentifizierung verwendet, speichert in der Mitgliedschaftsdatenbank Anwendungstabellen, ändert das Datenbankschema und die app in Azure bereitgestellt wird.
- [ASP.NET Data Access-Inhaltszuordnung](https://go.microsoft.com/fwlink/p/?LinkId=282414). Enthält Links zu Ressourcen zum Arbeiten mit EF und SQL-Datenbank.

Verwenden von MongoDB in Azure:

- [MongoLab - MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Portalseite zum Ausführen von MongoDB für Azure-Dokumentation.
- [Erstellen Sie ein Azure-Website die Herstellung der MongoDB, die auf einem virtuellen Computer in Azure ausgeführten](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Schrittweises Lernprogramm, das veranschaulicht, wie eine MongoDB-Datenbank in eine ASP.NET-Webanwendung verwendet.

HDInsight (Hadoop unter Azure):

- [HDInsight](https://azure.microsoft.com/en-us/documentation/services/hdinsight/). Portal zur HDInsight-Dokumentation auf der [Azure](https://azure.microsoft.com/) Website.
- [Hadoop und HDInsight: Big Data in Azure](https://msdn.microsoft.com/en-us/magazine/dn385705.aspx). MSDN Magazine-Artikel von Bruno Terkaly und Ricardo Villalobos, Einführung in Hadoop unter Azure.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/en-us/library/dn568099.aspx). MapReduce-Muster finden Sie unter.

>[!div class="step-by-step"]
[Zurück](single-sign-on.md)
[Weiter](data-partitioning-strategies.md)
