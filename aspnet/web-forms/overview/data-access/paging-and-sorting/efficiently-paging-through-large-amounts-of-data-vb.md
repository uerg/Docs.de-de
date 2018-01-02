---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: "Effizient Paging durch große Mengen von Daten (VB) | Microsoft Docs"
author: rick-anderson
description: "Die Standardoption für die Auslagerungsdatei des Steuerelements eine Präsentation ist nicht geeignet, bei der Arbeit mit großen Datenmengen Daten wie die zugrunde liegenden Daten Source Control Retriev..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00a5358361fa3f37d13ea74d61c437088b388ece
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Effizient Paging durch große Mengen von Daten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) oder [PDF herunterladen](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Die Standardoption für die Auslagerungsdatei des Steuerelements eine Präsentation ist ungeeignet, bei der Arbeit mit großen Mengen von Daten, während die zugrunde liegenden Datenquellen-Steuerelement alle Datensätze abruft, obwohl nur eine Teilmenge der Daten angezeigt wird. Unter solchen Umständen müssen Aktualisierungsfehler an benutzerdefinierte paging.


## <a name="introduction"></a>Einführung

Wie im vorherigen Lernprogramm erläutert, kann die Auslagerung auf zwei Arten implementiert werden:

- **Standardnavigation** kann implementiert werden, indem Sie einfach das Paging aktivieren-Option in den Daten-Websteuerelement s Smarttag; allerdings immer eine Seite mit Daten anzeigen zu können, das ObjectDataSource ruft *alle* der Datensätze, sogar Obwohl nur eine Teilmenge davon auf der Seite angezeigt werden
- **Benutzerdefiniertes Paging** verbessert die Leistung von Standard-paging durch Abrufen von nur Datensätze aus der Datenbank, die für bestimmte Seite durch den Benutzer angeforderte Datenbereich angezeigt werden müssen, benutzerdefiniertes Paging umfasst jedoch ein wenig mehr Aufwand implementieren als Standardpaging

Aufgrund der einfachen Implementierung nur überprüfen ein Kontrollkästchen, und Sie re-Vorgang abgeschlossen. Standardnavigation ist einer attraktiven Option. Die Na gibt es beim Abrufen aller Datensätze, entscheiden, jedoch die Auswahl eines Kernfunktion beim paging durch ausreichend große Mengen von Daten oder Websites mit vielen gleichzeitigen Benutzern. Unter solchen Umständen aktivieren wir an das benutzerdefinierte paging, um ein reaktionsfähig System bereitzustellen.

Die Herausforderung, benutzerdefinierte Paging wird eine Abfrage schreiben, die den genauen Satz von Datensätzen, die für eine bestimmte Seite der Daten erforderlichen zurückgibt. Glücklicherweise bietet Microsoft SQL Server 2005 ein neues Schlüsselwort für Rangfolge Ergebnisse zu erzielen, die können wir eine Abfrage schreiben, die effizient echte Teilmenge von Datensätzen abrufen können. In diesem Lernprogramm sehen wir, wie dieses neue SQL Server 2005-Schlüsselwort verwenden, um benutzerdefinierte Paging in einem GridView-Steuerelement zu implementieren. Während die Benutzeroberfläche für das benutzerdefinierte Paging, für standardmäßige auslagerungen, schrittweise Ausführung von einer Seite mit dem nächsten entspricht kann benutzerdefiniertes Paging schneller Standardnavigation Größenordnung sein.

> [!NOTE]
> Der genaue Leistungsgewinn antischadsoftwaresignaturen nach benutzerdefiniertes Paging hängt von der Gesamtzahl der Datensätze, die über ausgelagerte und die Last auf dem Datenbankserver ab. Am Ende dieses Lernprogramms betrachten groben Metriken wir, die Vorteile der Leistung mithilfe von benutzerdefinierten Paging abgerufen zu veranschaulichen.


## <a name="step-1-understanding-the-custom-paging-process"></a>Schritt 1: Grundlegendes zur benutzerdefinierten Paging

Beim paging durch Daten hängen präzise auf einer Seite angezeigten Datensätze der Datenseite angefordert wird und die Anzahl der Datensätze pro Seite angezeigt. Angenommen Sie, dass wollten wir die 81 Produkte durchblättern 10 Produkte pro Seite angezeigt. Wenn Sie die erste Seite anzeigen zu können, möchten wir d Produkte 1 bis 10; Wenn die zweite Seite, die wir d Produkte 11 bis 20 usw. von Interesse anzeigen.

Es gibt drei Variablen, die vorgeben, welche Datensätze abgerufen werden sollen und wie die Paginierung Schnittstelle gerendert werden soll:

- **Starten Sie Zeilenindex** der Index der ersten Zeile auf der Seite der anzuzeigenden Daten; dieser Index kann durch Multiplizieren den Seitenindex durch die Datensätze pro Seite angezeigt und Hinzufügen von mindestens einem berechnet werden. Beim paging durch 10 Datensätze zu einem Zeitpunkt für die erste Seite (dessen Seitenindex ist 0), wird der Start Zeilenindex 0 \* 10 + 1 oder 1; für die zweite Seite (dessen Seitenindex ist 1), Zeile der Startindex ist 1 \* 10 + 1 , oder 11.
- **Maximale Zeilenanzahl** die maximale Anzahl der Datensätze pro Seite angezeigt werden sollen. Diese Variable wird als maximale Anzahl an Zeilen bezeichnet, da für die letzte Seite gibt es weniger Datensätze als die Seitengröße sein kann. Beispielsweise wird beim paging durch die 81 10 Produktdatensätze pro Seite die neunte und letzten Seite nur ein Datensatz haben. Seite "keine" werden jedoch mehrere Datensätze als der Wert der maximalen Anzahl von Zeilen angezeigt.
- **Die Anzahl der Datensätze insgesamt** die Gesamtzahl der Datensätze, die über ausgelagert wird. Während diese Variable nicht erforderlich, um zu bestimmen, welche Datensätze für eine angegebene Seite abgerufen, dies ist aufgrund von Paging-Schnittstelle. Beispielsweise 81 Produkte, die ausgelagerte über vorhanden sind, "weiß" die Auslagerungsschnittstelle neun Seitenzahlen im Paging-UI angezeigt.

Der Zeilenindex starten wird mit Standardpaging als Produkt des der Seitenindex und die Seitengröße plus eins, berechnet, während die maximale Zeilenanzahl einfach die Seitengröße ist. Da Standardnavigation Ruft alle Datensätze aus wird die Datenbank beim Rendern von einer beliebigen Seite der Daten, den Index für jede Zeile bezeichnet, wodurch gleitenden Zeilenindex starten Zeile leichte Aufgabe. Darüber hinaus wird die Anzahl der Datensätze insgesamt unmittelbar verfügbar, da sie s einfach die Anzahl der Datensätze in der DataTable (oder ein Objekt verwendet wird, in der Datenbankergebnisse enthalten).

Die Zeilenindex starten und die maximale Zeilenanzahl Variablen wird angegeben, muss eine benutzerdefinierte Implementierung von Paging nur die präzise Teilmenge der Datensätze, starten den Zeilenindex starten und bis zur maximalen Anzahl von Zeilen der Anzahl der Datensätze nach, zurückgeben. Benutzerdefiniertes Paging bietet zwei Herausforderungen:

- Es muss möglich, effizient zuordnen, einen Zeilenindex jede Zeile in die gesamten Daten über ausgelagert werden, sodass Rückgabe Datensätze im angegebenen Zeilenindex starten gestartet werden kann
- Wir müssen die Gesamtzahl der Datensätze, die ausgelagerte über bereitstellen

In den nächsten beiden Schritten untersuchen wir die SQL-Skript erforderlich, um auf diese beiden Probleme zu reagieren. Zusätzlich zu den SQL-Skript müssen wir auch für die Implementierung von Methoden in der DAL und BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Schritt 2: Zurückgeben der Gesamtzahl der Datensätze, die ausgelagerte über

Bevor wir zum Abrufen der präzise Teilmenge der Datensätze für die Seite angezeigt wird untersuchen, können Sie s uns zunächst an, wie die Gesamtanzahl von Datensätzen, die ausgelagerte über zurückzugeben. Diese Informationen werden benötigt, um die Benutzeroberfläche für die Auslagerungsdatei ordnungsgemäß zu konfigurieren. Die Gesamtanzahl der von einer bestimmten SQL-Abfrage zurückgegebenen Datensätze erhalten Sie, indem Sie mit der [ `COUNT` Aggregatfunktion](https://msdn.microsoft.com/en-US/library/ms175997.aspx). Angenommen, um zu bestimmen, die Gesamtanzahl von Datensätzen in der `Products` Tabelle, können wir die folgende Abfrage verwenden:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Lassen Sie s unsere DAL eine Methode hinzugefügt, die diese Informationen zurückgibt. Insbesondere erstellen wir eine DAL-Methode wird aufgerufen, `TotalNumberOfProducts()` ausführt, die die `SELECT` oben aufgeführten-Anweisung.

Öffnen Sie zunächst die `Northwind.xsd` typisierte DataSet-Datei in die `App_Code/DAL` Ordner. Als Nächstes mit der rechten Maustaste auf die `ProductsTableAdapter` im Designer, und wählen Sie die Abfrage hinzufügen. Sobald gibt es im vorherigen Lernprogrammen betrachtet, dadurch können wir die DAL eine neue Methode hinzu, die beim Aufrufen, wird eine bestimmte SQL-Anweisung oder gespeicherte Prozedur ausgeführt. Wie bei unserer TableAdapter-Methoden in den vorherigen Lernprogrammen, für diese wahlweise eine Ad-hoc-SQL-Anweisung verwenden.


![Verwenden Sie eine Ad-hoc-SQL-Anweisung](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Abbildung 1**: Verwenden Sie eine Ad-hoc-SQL-Anweisung


Auf dem nächsten Bildschirm festlegbaren wir, welche Art von Abfrage zu erstellen. Da diese Abfrage die Gesamtanzahl von Datensätzen in einem einzelnen skalaren Wert zurückgeben der `Products` Tabelle wählen Sie die `SELECT` die Option einen einzigen Wert zurückgibt.


![Konfigurieren Sie die Abfrage aus, um eine SELECT-Anweisung verwenden, die einen einzelnen Wert zurückgibt](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Abbildung 2**: Konfigurieren Sie die Abfrage aus, um eine SELECT-Anweisung verwenden, die einen einzelnen Wert zurückgibt


Nach dem, der angibt, in des Typs der Abfrage zu verwenden, müssen wir als Nächstes die Abfrage angeben.


![Verwenden Sie die SELECT COUNT(*) Produkte Abfrage](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Abbildung 3**: Verwenden Sie die Option COUNT (\*) Abfrage der FROM-Produkte


Geben Sie schließlich den Namen für die Methode an. Verwendung als zuvor erwähnten, können s `TotalNumberOfProducts`.


![Benennen Sie die TotalNumberOfProducts DAL-Methode](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Abbildung 4**: Benennen der TotalNumberOfProducts DAL-Methode


Nach dem Klicken auf "Fertig stellen" des Assistenten fügen die `TotalNumberOfProducts` Methode, um der DAL. Die skalare zurückgeben Methoden in der DAL auf NULL festlegbaren Typen zurück, für den Fall, dass das Ergebnis von der SQL-Abfrage ist `NULL`. Unsere `COUNT` Abfrage, jedoch gibt stets einen nicht-`NULL` -Wert, unabhängig davon, die DAL-Methode gibt eine NULL-Werte zulassen ganze Zahl zurück.

Zusätzlich zu der DAL-Methode benötigen wir auch eine Methode in der BLL. Öffnen der `ProductsBLL` Klassendatei, und fügen eine `TotalNumberOfProducts` Methode, die einfach mit dem DAL s, unten aufruft `TotalNumberOfProducts` Methode:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

Die DAL-s `TotalNumberOfProducts` Methode gibt eine NULL-Werte zulassen ganze Zahl zurück, aber wir Ve erstellt die `ProductsBLL` Klasse s `TotalNumberOfProducts` Methode, sodass dieser standard eine ganze Zahl zurückgegeben. Aus diesem Grund müssen wir die `ProductsBLL` Klasse s `TotalNumberOfProducts` Methode zurück, der Wertteil der NULL-Werte zulassen Ganzzahl zurückgegeben, durch die DAL-s. `TotalNumberOfProducts` Methode. Der Aufruf von `GetValueOrDefault()` Wert der Ganzzahl NULL-Werte zulässt, falls vorhanden; zurückgegeben, falls die NULL-Werte zulassen ganze Zahl ist `null`, jedoch gibt es die ganze Zahl Standardwert 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Schritt 3: Zurückgeben der präzise Teilmenge von Datensätzen

Unsere nächste Aufgabe ist die Erstellung von Methoden in der DAL und BLL, der den Zeilenindex starten zu akzeptieren und die maximale Zeilenanzahl Variablen zuvor erläuterten und die entsprechenden Datensätze zurückzugeben. Bevor wir dies tun, Let s uns zunächst an die benötigten SQL-Skript. Die Herausforderung, die für uns ist Transportservers, dass wir effizient zuweisen ein Indexes auf jede Zeile in die gesamten Ergebnisse über ausgelagert werden, sodass nur die Datensätze ab, auf den Zeilenindex starten (und bis zu die maximale Anzahl der Datensätze Anzahl der Datensätze) zurückgegeben werden können.

Dies ist eine Herausforderung nicht, wenn bereits eine Spalte in der Datenbanktabelle, die als ein Zeilenindex dient vorhanden ist. Auf den ersten Blick könnten wir meinen, dass die `Products` Tabelle s `ProductID` Feld ausreichen würde, wie das erste Produkt weist `ProductID` 1, der zweite ein 2, und so weiter. Löschen eines Produkts lässt jedoch eine Lücke in der Sequenz, die diesen Ansatz nullifying.

Es gibt zwei allgemeine Techniken verwendet, um einen Zeilenindex mit den Daten zu blättern, effizient zuzuordnen Dadurch wird die präzise Teilmenge der Datensätze abgerufen werden sollen:

- **Mit SQL Server 2005 s `ROW_NUMBER()` Schlüsselwort** noch nicht mit SQL Server 2005, die `ROW_NUMBER()` Schlüsselwort ordnet einen Rang jeder zurückgegebenen Datensatz basierend auf Sortierung. Diese Rangfolge kann als ein Zeilenindex für jede Zeile verwendet werden.
- **Eine Tabellenvariable mit und `SET ROWCOUNT`**  SQL Server-s [ `SET ROWCOUNT` Anweisung](https://msdn.microsoft.com/en-us/library/ms188774.aspx) können verwendet werden, um wie viele Datensätze insgesamt Geben Sie eine Abfrage sollte vor der Beendigung; verarbeiten [Tabellenvariablen](http://www.sqlteam.com/item.asp?ItemID=9454) sind lokale T-SQL-Variablen, die Tabellendaten, akin zum aufnehmen können [temporäre Tabellen](http://www.sqlteam.com/item.asp?ItemID=2029). Dieser Ansatz funktioniert ebenso gut mit Microsoft SQL Server 2005 und SQL Server 2000 (während der `ROW_NUMBER()` Ansatz funktioniert nur mit SQL Server 2005).  
  
 Die Idee ist, um eine Tabellenvariable zu erstellen, verfügt ein `IDENTITY` Spalten- und Spalten für die Primärschlüssel der Tabelle, deren Daten über ausgelagert wird werden. Als Nächstes wird der Inhalt der Tabelle, deren Daten über ausgelagerte, ausgegeben, in der Tabellenvariablen, wodurch zuordnen einen sequenziellen Zeilenindex (über die `IDENTITY` Spalte) für jeden Datensatz in der Tabelle. Nachdem die Tabellenvariable aufgefüllt wurde, eine `SELECT` Anweisung auf die Tabellenvariable mit der zugrunde liegenden Tabelle verknüpft, um die bestimmter Datensätze herausziehen ausgeführt werden kann. Die `SET ROWCOUNT` Anweisung wird verwendet, um die Anzahl der Datensätze Intelligent zu beschränken, die in der Tabellenvariablen gesichert werden müssen.  
  
 Dieser Ansatz s Effizienz basiert darauf, dass die Seitenzahl angefordert wird, als die `SET ROWCOUNT` Wert wird den Wert der Zeile startIndex sowie die maximale Zeilenanzahl zugewiesen. Dieser Ansatz ist sehr effizient, wenn paging durch niedrig nummerierten Seiten, z. B. das erste Paar Datenseiten. Allerdings weist es standardmäßig Paging-ähnliche Leistung beim Abrufen einer Seite in der Nähe des.

Dieses Lernprogramm implementiert nur benutzerdefinierte Paging mithilfe der `ROW_NUMBER()` Schlüsselwort. Weitere Informationen zur Verwendung der Tabellenvariablen und `SET ROWCOUNT` Technik, finden Sie unter [eine weitere effiziente Methode zum Paging durch große Resultsets](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Die `ROW_NUMBER()` Schlüsselwort jeder Datensatz über eine bestimmte Sortierung mit der folgenden Syntax zurückgegebene Rang zugeordnet:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()`Gibt einen numerischen Wert, der angibt, den Rang für jeden Datensatz im Hinblick auf die angegebene Reihenfolge zurück. Beispielsweise könnten um den Rang für jedes Produkt, geordnet von den am stärksten finden Sie unter zum am wenigsten aufwändige wir die folgende Abfrage verwenden:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Abbildung 5 zeigt diese Abfrage s Ergebnisse, wenn Sie über das Fenster "Abfrage" in Visual Studio ausführen. Beachten Sie, dass die Produkte nach Preis, zusammen mit dem Rang der Preis für jede Zeile sortiert werden.


![Der Rang der Preis wird aus Gründen der Datensatz für jede zurückgegeben](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Abbildung 5**: der Preis Rang ist enthalten, für jede zurückgegebene Datensatz


> [!NOTE]
> `ROW_NUMBER()`ist nur eine der vielen neuen Rangfolgefunktionen in SQL Server 2005 verfügbar. Eine ausführlichere Erläuterung der `ROW_NUMBER()`, zusammen mit den anderen Rangfolgefunktionen lesen [aufgelisteten Ergebnisse zurückgeben, mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Wenn die Ergebnisse mit der angegebenen Rangfolge `ORDER BY` Spalte in der `OVER` -Klausel (`UnitPrice`, im obigen Beispiel), SQL Server müssen die Ergebnisse zu sortieren. Dies ist ein schneller Vorgang, wenn es ein gruppierter Index über die Spalte(n), die die Ergebnisse werden ist nach, sortiert wird oder wenn es einem abdeckenden index jedoch andernfalls mehr teuer sein kann. Zur Verbesserung der Leistung für ausreichend große Abfragen erwägen Sie einen nicht gruppierten Index für die Spalte mit der die Ergebnisse geordnet sind. Finden Sie unter [Rangfolge Funktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) für eine detaillierte Darstellung der Leistungsaspekte.

Die zurückgegebene Rangfolgeinformationen `ROW_NUMBER()` kann nicht direkt verwendet werden, der `WHERE` Klausel. Allerdings kann eine abgeleitete Tabelle zurückzugebenden verwendet die `ROW_NUMBER()` Ergebnis, die dann in angezeigt werden kann die `WHERE` Klausel. Die folgende Abfrage verwendet z. B. eine abgeleitete Tabelle zurückzugebenden Spalten ProductName und UnitPrice zusammen mit der `ROW_NUMBER()` Ergebnis und dann verwendet, eine `WHERE` -Klausel, um nur solche Produkte, deren Preis Rang zurück zwischen 11 und 20 ist:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Erweitern dieses Konzept etwas weiter, kann dieser Ansatz zum Abrufen einer bestimmten Seite der Daten anhand des gewünschten Zeilenindex starten und die maximale Zeilenanzahl nutzen:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Wie wir später in diesem Lernprogramm sehen werden die  *`StartRowIndex`*  vom ObjectDataSource ist indiziert, beginnend mit 0 (null), wohingegen die `ROW_NUMBER()` von SQL Server 2005 zurückgegebene Wert ist indiziert, beginnend mit 1. Aus diesem Grund die `WHERE` -Klausel gibt die Datensätze zurück, in denen `PriceRank` ist streng größer als  *`StartRowIndex`*  und kleiner oder gleich  *`StartRowIndex`*   +  *`MaximumRows`*.


Jetzt, dass wir haben erläutert, wie `ROW_NUMBER()` können werden verwendet, um eine bestimmte Seite mit Daten anhand des Zeilenindex starten und die maximale Anzahl der Zeilen abrufen, jetzt müssen wir diese Logik als Methoden in der DAL und BLL implementieren.

Beim Erstellen dieser Abfrage an, dass wir die Reihenfolge entscheiden, müssen werden durch den die Ergebnisse geordnet werden; Lassen Sie s die Produkte nach ihrer Namen in alphabetischer Reihenfolge zu sortieren. Dies bedeutet, dass durch das benutzerdefinierte Paging-Implementierung in diesem Lernprogramm wir keinen benutzerdefinierte mehrseitigen Bericht zu erstellen, als auch sortiert werden kann. In den nächsten Lernprogrammen allerdings sehen wie solche Funktionen bereitgestellt werden kann.

Im vorherigen Abschnitt wurde die DAL-Methode als eine Ad-hoc-SQL-Anweisung erstellt. Leider der T-SQL-Parser in Visual Studio verwendet, die für die TableAdapter-Assistenten ist nicht t wie der `OVER` Syntax verwendet werden, indem Sie die `ROW_NUMBER()` Funktion. Aus diesem Grund müssen wir diese DAL-Methode als gespeicherte Prozedur erstellen. Wählen Sie im Server-Explorer aus dem Menü "Ansicht" (oder Klicken STRG + Alt + S), und erweitern Sie die `NORTHWND.MDF` Knoten. Klicken Sie zum Hinzufügen einer neuen gespeicherten Prozedur mit der rechten Maustaste auf den Knoten gespeicherte Prozeduren, und wählen Sie auf Hinzufügen einer neuen gespeicherten Prozedur (siehe Abbildung 6).


![Hinzufügen einer neuen gespeicherten Prozedur für das Paging durch die Produkte](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Abbildung 6**: hinzufügen eine neue gespeicherte Prozedur für das Paging durch die Produkte


Diese gespeicherte Prozedur sollten zwei ganzzahlige Eingabeparameter entgegen - akzeptieren `@startRowIndex` und `@maximumRows` und Verwenden der `ROW_NUMBER()` Funktion geordnet nach der `ProductName` Feld, das nur die Zeilen zurückgeben, die größer als der angegebene `@startRowIndex` und kleiner als oder gleich `@startRowIndex`  +  `@maximumRow` s. Geben Sie das folgende Skript in die neue gespeicherte Prozedur, und klicken Sie dann auf das Symbol "Speichern", um die gespeicherte Prozedur zur Datenbank hinzuzufügen.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Nehmen Sie einen Moment Zeit, um es zu testen, nach dem Erstellen der gespeicherten Prozedur können. Mit der rechten Maustaste auf die `GetProductsPaged` gespeicherten Prozedur benennen Sie im Server-Explorer, und wählen Sie die Execute-Option. Visual Studio fordert Sie dann für die Eingabeparameter `@startRowIndex` und `@maximumRow` s (siehe Abbildung 7). Probieren Sie verschiedene Werte und überprüfen Sie die Ergebnisse.


![Geben Sie einen Wert für die @startRowIndex und @maximumRows Parameter](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

**Abbildung 7**: Geben Sie einen Wert für die @startRowIndex und @maximumRows Parameter


Nach dem Auswählen dieser Parametern Eingabewerte, die Fenster "Ausgabe" zeigt die Ergebnisse. Abbildung 8 zeigt die Ergebnisse bei der Übergabe von 10 für beide die `@startRowIndex` und `@maximumRows` Parameter.


[![Die Datensätze, würde werden in der zweiten Seite der Daten werden zurückgegeben.](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Abbildung 8**: der Datensätze, dass angezeigt würde in die zweite Seitendaten zurückgegeben werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Mit dieser Prozedur erstellt, es erneut bereit, die zum Erstellen der `ProductsTableAdapter` Methode. Öffnen der `Northwind.xsd` typisierte DataSet mit der rechten Maustaste in den `ProductsTableAdapter`, und wählen Sie die Option der Abfrage hinzufügen. Erstellen Sie mithilfe einer vorhandenen gespeicherten Prozedur an, statt die Abfrage mit einer Ad-hoc-SQL-Anweisung.


![Erstellen Sie die DAL-Methode, die mit einer vorhandenen gespeicherten Prozedur](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Abbildung 9**: Erstellen Sie die DAL-Methode, die mit einer vorhandenen gespeicherten Prozedur


Als Nächstes werden wir aufgefordert, um die gespeicherte Prozedur zum Aufrufen auszuwählen. Wählen Sie die `GetProductsPaged` gespeicherten Prozedur aus der Dropdown Liste.


![Wählen Sie die GetProductsPaged gespeicherten Prozedur aus der Dropdown Liste](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Abbildung 10**: Wählen Sie die GetProductsPaged gespeicherten Prozedur aus der Dropdown Liste


Der nächste Bildschirm fragt Sie dann, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird: Tabellendaten, die einen einzelnen Wert oder keinen Wert. Da die `GetProductsPaged` gespeicherte Prozedur kann mehrere Datensätze zurückzugeben, anzugeben, dass sie Tabellendaten zurückgibt.


![Um anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurückgibt](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Abbildung 11**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt


Geben Sie schließlich an die Namen der Methoden, die Sie erstellt haben möchten. Wie bei unserer vorherigen Lernprogrammen fahren Sie fort, und Methoden, die beide die Füllung mit einer "DataTable" erstellen und Zurückgeben einer "DataTable". Benennen Sie die erste Methode `FillPaged` und das zweite `GetProductsPaged`.


![Name der Methoden FillPaged und GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Abbildung 12**: Name der Methoden FillPaged und GetProductsPaged


Erstellt eine DAL-Methode, um eine bestimmte Seite Produkte zurückzugeben, müssen wir darüber hinaus auch solche Funktionen in die BLL angeben. Wie die DAL-Methode der BLL s GetProductsPaged-Methode zustimmen zwei Integer-Eingaben zum Angeben der Zeilenindex starten und die maximale Zeilenanzahl sowie nur Datensätze, die innerhalb des angegebenen Bereichs liegen zurückgeben. Erstellen Sie eine solche BLL Methode in der ProductsBLL-Klasse, dass lediglich Aufrufe nach unten in der DAL-s GetProductsPaged-Methode, wie folgt:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Sie können einen beliebigen Namen verwenden Entscheidung, wie es in Kürze, die angezeigt wird, aber für die BLL Eingabeparameter für das s-Methode, für den Einsatz `startRowIndex` und `maximumRows` uns ein zusätzlicher erspart viel Arbeit, die beim Konfigurieren von einem ObjectDataSource zur Verwendung dieser Methode.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Schritt 4: Konfigurieren der ObjectDataSource Verwendung benutzerdefiniertes Paging

Mit den Methoden für den Zugriff auf eine bestimmte Teilmenge der Datensätze, die vollständige BLL und DAL steuern wir re kann jetzt eine GridView erstellen diese Seiten durch die zugrunde liegenden Datensätze mit benutzerdefiniertes Paging. Öffnen Sie zunächst die `EfficientPaging.aspx` auf der Seite der `PagingAndSorting` Ordner die Seite eine GridView hinzu, und konfigurieren, um ein neues ObjectDataSource-Steuerelement zu verwenden. In unseren Tutorials, letzten, mussten wir häufig das ObjectDataSource konfiguriert die `ProductsBLL` Klasse s `GetProducts` Methode. Dieses Mal jedoch wir verwenden möchten die `GetProductsPaged` Methode stattdessen seit der `GetProducts` -Methode zurückkehrt *alle* der Produkte in der Datenbank während `GetProductsPaged` gibt nur eine bestimmte Teilmenge der Datensätze.


![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Abbildung 13**: Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode


Da wir re erstellen eine nur-Lese GridView erkundet Festlegen der Methode Dropdown-Liste in den INSERT-, Update-, und löschen Sie Registerkarten (keine).

Im nächsten Schritt fordert uns das ObjectDataSource-Assistenten für die Datenquellen der `GetProductsPaged` Methode s `startRowIndex` und `maximumRows` Eingabewerte für Parameter. Diese Eingabeparameter werden tatsächlich festgelegt werden, indem die GridView automatisch so einfach behalten Sie die Quelle keine und klicken Sie auf ' Fertig stellen '.


![Lassen Sie die Eingabeparameter Quellen None](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Abbildung 14**: lassen Sie die Eingabeparameter Quellen None


Nach Abschluss des Assistenten ObjectDataSource wird die GridView eine BoundField- oder CheckBoxField, für die einzelnen Datenfelder Produkt enthalten. Wahlweise können die GridView-s-Darstellung anpassen, nach Bedarf. Ich haben sich entschieden, zur ausschließlichen Anzeige der `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, und `UnitPrice` BoundFields. Konfigurieren Sie zudem die GridView zur Unterstützung von Paging durch Aktivieren des Kontrollkästchens Paging aktivieren, in das Smarttag. Nach der Änderung sollte die GridView und ObjectDataSource deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Wenn Sie die Seite über einen Browser besuchen, die GridView ist jedoch keine Where gefunden werden.


![Die GridView wird nicht angezeigt.](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Abbildung 15**: die GridView wird nicht angezeigt.


Die GridView ist nicht vorhanden, da das ObjectDataSource derzeit für beide 0 als Werte verwendet die `GetProductsPaged` `startRowIndex` und `maximumRows` Eingabeparameter. Daher die resultierende SQL-Abfrage ist keine Datensätze zurückgeben, und daher die GridView wird nicht angezeigt.

Zur Behebung des Problems, müssen wir die ObjectDataSource benutzerdefiniertes Paging Verwendung konfigurieren. Dies kann in den folgenden Schritten erfolgen:

1. **Legen Sie das ObjectDataSource-s `EnablePaging` Eigenschaft `true`**  Dies gibt an, um das ObjectDataSource, die an ihn übergeben werden muss die `SelectMethod` zwei zusätzliche Parameter: einen zur Angabe der Zeilenindex starten ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)), und geben Sie die maximale Zeilenanzahl ([`MaximumRowsParameterName`](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Legen Sie das ObjectDataSource-s `StartRowIndexParameterName` und `MaximumRowsParameterName` Eigenschaften entsprechend** der `StartRowIndexParameterName` und `MaximumRowsParameterName` -Eigenschaften geben die Namen der Eingabeparameter übergebenen der `SelectMethod` für benutzerdefinierte Paging Zwecke . Standardmäßig sind diese Parameternamen `startIndexRow` und `maximumRows`, daher, beim Erstellen der `GetProductsPaged` Methode in der BLL diese Werte für die Eingabeparameter verwendet. Wenn Sie ausgewählt haben, verwenden Sie unterschiedliche Parameternamen für BLL s `GetProductsPaged` Methode z. B. `startIndex` und `maxRows`für Beispiel müssen Sie das ObjectDataSource-s festgelegt `StartRowIndexParameterName` und `MaximumRowsParameterName` Eigenschaften entsprechend (z. B. StartIndex für `StartRowIndexParameterName` und MaxRows für `MaximumRowsParameterName`).
3. **Legen Sie die s ObjectDataSource [ `SelectCountMethod` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) auf den Namen der Methode, die insgesamt Anzahl der Datensätze werden ausgelagerten über zurückgibt (`TotalNumberOfProducts`)** Bedenken Sie, dass die `ProductsBLL` Klasse s `TotalNumberOfProducts`Methode gibt die Gesamtzahl der Datensätze, die ausgelagerte durch eine DAL-Methode, die ausgeführt wird mithilfe einer `SELECT COUNT(*) FROM Products` Abfrage. Diese Informationen werden von der ObjectDataSource benötigt, um die Auslagerungsschnittstelle ordnungsgemäß zu rendern.
4. **Entfernen Sie die `startRowIndex` und `maximumRows` `<asp:Parameter>` Elemente aus der ObjectDataSource s deklarativem Markup** beim Konfigurieren der ObjectDataSource über den Assistenten hinzugefügt Visual Studio automatisch zwei `<asp:Parameter>` Elemente für die `GetProductsPaged` Methode s Eingabeparameter. Durch Festlegen von `EnablePaging` auf `true`, diese Parameter automatisch weitergegeben werden; Wenn sie auch in die deklarative Syntax angezeigt werden, versucht das ObjectDataSource Übergabe *vier* Parameter für die `GetProductsPaged` Methode und die zwei Parameter an die `TotalNumberOfProducts` Methode. Wenn Sie vergessen, entfernen Sie diese `<asp:Parameter>` Elemente, wenn Sie die Seite über einen Browser besuchen Sie eine Fehlermeldung wie erhalten: *ObjectDataSource "ObjectDataSource1" konnte nicht gefunden werden eine nicht generische Methode "TotalNumberOfProducts", die aufweist Parameter: StartRowIndex MaximumRows*.

Nachdem diese Änderungen vorgenommen wurden, sollte das ObjectDataSource s deklarative Syntax wie folgt aussehen:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Beachten Sie, dass die `EnablePaging` und `SelectCountMethod` Eigenschaften festgelegt wurden und die `<asp:Parameter>` Elemente entfernt wurden. Abbildung 16 zeigt einen Screenshot des Fensters Eigenschaften auf, nachdem diese Änderungen vorgenommen wurden.


![Um benutzerdefinierte Paging zu verwenden, konfigurieren Sie das ObjectDataSource-Steuerelement](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Abbildung 16**: um benutzerdefiniertes Paging zu verwenden, konfigurieren Sie das ObjectDataSource-Steuerelement


Finden Sie nachdem diese Änderungen vorgenommen wurden auf dieser Seite über einen Browser. Daraufhin sollte 10 Produkte aufgeführt, alphabetisch sortiert. Nehmen Sie einen Moment Zeit, um die Seite zu einem Zeitpunkt zu durchlaufen. Es gibt keinen visuellen Unterschied aus der Perspektive des Endbenutzers s standardmäßige und benutzerdefinierte Paging, Seiten benutzerdefinierte paging effizienter durch große Mengen von Daten während es nur die Datensätze abruft, die für eine angegebene Seite angezeigt werden müssen.


[![Die Daten, sortiert nach Produkt-s-Name ist ausgelagerten benutzerdefinierte mithilfe von Paging](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Abbildung 17**: die Daten, sortiert nach Produkt-s-Name ist ausgelagerten benutzerdefinierte mithilfe von Paging ([klicken Sie hier, um das Bild in voller Größe angezeigt](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Mit benutzerdefinierten Paging zählen der Seite "durch das ObjectDataSource-s. zurückgegebene Wert `SelectCountMethod` im Ansichtszustand GridView s gespeichert ist. Andere GridView-Variablen der `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` Auflistung usw. sind gespeichert *Steuerelementzustand*, dem wird beibehalten, unabhängig vom Wert der GridView s `EnableViewState` Diese Eigenschaft. Da die `PageCount` Wert wird beibehalten über Postbacks über den Ansichtszustand, wenn eine Auslagerungsdatei-Schnittstelle verwenden, die enthält einen Link, um die letzte Seite aufzurufen, ist es unabdinglich, der Ansichtszustand des GridView-s aktiviert werden. (Wenn Paging-Schnittstelle einen direkten Link zur letzten keine enthält Seite, Sie können dann die Ansichtszustand deaktivieren)


Bewirkt, dass einen Postback und weist die GridView zum Aktualisieren der letzten Seite Link seine `PageIndex` Eigenschaft. Wenn die letzte Seitenlink geklickt wird, weist die GridView seine `PageIndex` auf einen Wert einer Eigenschaft kleiner als die `PageCount` Eigenschaft. Mit Ansichtszustand deaktiviert die `PageCount` Wert wird über Postbacks verloren und die `PageIndex` wird stattdessen den Wert des maximalen ganzen Zahl zugewiesen. Als Nächstes die GridView versucht, durch Multiplizieren den Startindex für die Zeile zu bestimmen die `PageSize` und `PageCount` Eigenschaften. Dies führt zu einer `OverflowException` , da das Produkt die maximale zulässige ganzzahlige Größe überschreitet.

## <a name="implement-custom-paging-and-sorting"></a>Implementieren benutzerdefinierter Paging und sortieren

Unsere aktuelle benutzerdefiniertes Paging-Implementierung muss die Reihenfolge nach dem die Daten über ausgelagert werden statisch angegeben werden, für die Erstellung der `GetProductsPaged` gespeicherten Prozedur. Allerdings kann notierten enthält das Smarttag für GridView s ein Kontrollkästchen Sortieren aktivieren, zusätzlich zur Option Paging aktivieren. Leider werden sortierungsunterstützung an die GridView mit unseren aktuellen benutzerdefinierte Paging Implementierung hinzufügen, nur die Datensätze der aktuell angezeigten Seite der Daten sortiert. Wenn Sie die GridView auch Unterstützung von Paging, und klicken Sie dann beim Anzeigen der ersten Seite der Daten nach Produktname konfigurieren in absteigender Reihenfolge sortieren wird es z. B. die Reihenfolge der Produkte auf der Seite "1" umgekehrt. Wie in Abbildung 18 dargestellt, zeigt z. B. Carnarvon-Tiger als erste Produkt beim in umgekehrter alphabetischer Reihenfolge sortieren, wodurch die 71 anderen Produkte ignoriert, die in alphabetischer Reihenfolge nach Carnarvon-Tiger, folgen. nur die Datensätze auf der ersten Seite werden bei der Sortierung berücksichtigt.


[![Nur die Daten angezeigt, auf der aktuellen Seite wird sortiert.](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Abbildung 18**: nur die Daten angezeigt, auf der aktuellen Seite sortiert wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


Die Sortierung gilt nur für die aktuelle Seite der Daten, da die Sortierung erfolgt nach dem die Daten aus den s BLL abgerufen wurde `GetProductsPaged` , und diese Methode gibt nur die Datensätze für die Seite. Um die Sortierung ordnungsgemäß zu implementieren, müssen wir den Sortierungsausdruck auf übergeben der `GetProductsPaged` Methode, damit die Daten vor der Rückgabe der speziellen Seite der Daten entsprechend Rang haben können. Wir sehen, wie dies in unserem nächsten Lernprogramm zu erreichen.

## <a name="implementing-custom-paging-and-deleting"></a>Implementieren benutzerdefinierte Paging und löschen

Wenn Sie Funktionen in eine GridView, deren Daten werden mithilfe von benutzerdefinierten Paging Techniken werden, festgestellt werden wenn den letzten Datensatz von der letzten Seite löschen ausgelagert, löschen aktivieren, die GridView anstatt entsprechend dekrementiert die GridView s verschwindet`PageIndex`. Um diesen Fehler zu reproduzieren, aktivieren Sie für das Lernprogramm einfach wir gerade erstellten löschen. Wechseln Sie zur letzten Seite (Seite 9), in denen sollte ein einzelnes Produkt angezeigt werden, da wir über 81 Produkte, 10 Produkte zu einem Zeitpunkt paging sind. Löschen Sie dieses Produkt.

Beim Löschen des letzten Produkts, die GridView *sollten* automatisch zur Seite "achte" wechseln, und solche Funktionen mit Standardnavigation ausgestellt wird. Mit benutzerdefinierten Paging verschwindet jedoch nach dem Löschen dieses letzten Produkts auf der letzten Seite GridView einfach aus dem Bildschirm vollständig. Die genaue Ursache *warum* in diesem Fall ist vom Datentyp bit den Rahmen dieses lehrprogramms sprengen; Siehe [löschen den letzten Datensatz auf der letzten Seite aus eine GridView, für das benutzerdefinierte Paging](http://scottonwriting.net/sowblog/posts/7326.aspx) für die Details auf niedriger Ebene, auf die Quelle der Dieses Problem. Zusammenfassend es s aufgrund der folgenden Schrittfolge, die durch die GridView ausgeführt werden, wenn auf die Schaltfläche "löschen" geklickt wird:

1. Datensatz löschen
2. Erhalten Sie die entsprechenden Einträge für den angegebenen anzuzeigende `PageIndex` und`PageSize`
3. Kontrollkästchen, um sicherzustellen, dass die `PageIndex` Wenn es automatisch die GridView s Verringern der Fall ist, nicht die Anzahl der Datenseiten in der Datenquelle überschreitet `PageIndex` Eigenschaft
4. Binden Sie die entsprechende Seite der Daten an die GridView unter Verwendung der Datensätze, die in Schritt2 abgerufen

Das Problem ist die Tatsache, diese in Schritt2 der `PageIndex` verwendet, wenn Datensätze anzuzeigenden grabbing weiterhin ist die `PageIndex` der letzten Seite, deren einzige Datensatz wurde gerade gelöscht. Aus diesem Grund in Schritt2 *keine* Datensätze zurückgegeben werden, da diese letzte Seite der Daten nicht mehr Datensätze enthält. Klicken Sie dann in Schritt 3, erkennt die GridView, die die `PageIndex` -Eigenschaft ist größer als die Gesamtanzahl der Seiten, die in der Datenquelle (da wir Ve gelöscht. den letzten Datensatz in der letzten Seite) und daher verringert die `PageIndex` Eigenschaft. In Schritt 4 versucht die GridView selbst an die in Schritt2 abgerufenen Daten zu binden. Allerdings was in Schritt2, die keine Einträge zurückgegeben wurden, wiederum zu einer leeren GridView. Dieses Problem ist nicht t mit Standardnavigation, surface, da in Schritt2 *alle* Datensätze aus der Datenquelle abgerufen werden.

Zum Beheben dieses Problems haben wir zwei Optionen zur Verfügung. Der erste Schritt besteht im Erstellen Sie eines ereignishandlers für die GridView s `RowDeleted` Ereignishandler, der bestimmt, wie viele Datensätze auf der Seite angezeigt wurden, die gerade gelöscht wurde. Wenn gab es nur ein Datensatz, und klicken Sie dann der Datensatz nur gelöscht das letzte Lesezeichen wurde und wir benötigen, um die GridView s zu verringern. `PageIndex`. Natürlich nur aktualisiert werden sollen die `PageIndex` Wenn der Löschvorgang tatsächlich erfolgreich war, kann die Leuchten, die ermittelt die `e.Exception` Eigenschaft ist `null`.

Dieser Ansatz funktioniert, da er aktualisiert die `PageIndex` nach Schritt 1, aber vor Schritt2. Aus diesem Grund wird die entsprechende Gruppe von Datensätzen in Schritt2 zurückgegeben. Um dies zu erreichen, verwenden Sie Code wie folgt aus:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Eine alternative problemumgehung besteht im Erstellen eines ereignishandlers für das ObjectDataSource-s `RowDeleted` Ereignis und Festlegen der `AffectedRows` Eigenschaft, um den Wert 1. Nach dem Löschen des Datensatzes in Schritt 1 (jedoch vor dem erneuten Abrufen der Daten in Schritt2), die GridView aktualisiert seine `PageIndex` Eigenschaft, wenn eine oder mehrere Zeilen, die von dem Vorgang betroffen waren. Allerdings die `AffectedRows` Eigenschaft nicht durch das ObjectDataSource festgelegt ist und daher wird dieser Schritt ausgelassen. Eine Möglichkeit, diesen Schritt ausgeführt haben manuell festlegen, wird die `AffectedRows` Eigenschaft, wenn der Löschvorgang erfolgreich abgeschlossen wurde. Dies kann erreicht werden, mithilfe von Code wie folgt:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Der Code für beide diese Ereignishandler finden Sie im Code-Behind-Klasse, von der `EfficientPaging.aspx` Beispiel.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Vergleichen die Leistung von Standard- und benutzerdefinierte Paging

Da benutzerdefiniertes Paging nur die benötigten Datensätze abruft, serverwiederherstellungsmethode Standardnavigation *alle* der Datensätze für jede Seite angezeigt wird, sie deaktivieren, dass das benutzerdefiniertes Paging effizienter als die Standardnavigation ist. Aber nur wie viel effizienter benutzerdefiniertes Paging ist? Welche Art von Leistungssteigerungen durch Verschieben von Standardnavigation benutzerdefiniertes Paging angezeigt werden kann?

Leider dort s kein passendes alle hier beantworten. Die Leistungszuwachs hängt von einer Reihe von Faktoren, die bekannteste zwei wird die Anzahl der Datensätze, die über ausgelagerte und der Auslastung auf der Datenbank-Kanäle für Server und Kommunikation zwischen dem Webserver und Datenbankserver platziert. Für kleine Tabellen mit ein paar Dutzend Datensätze möglicherweise der Leistungsunterschied unerheblich. Bei großen Tabellen mit Tausenden und Hunderttausenden von Tausenden von Zeilen jedoch ist der Leistungsunterschied akuten.

Ein Artikel ändere, [benutzerdefinierte Paging in ASP.NET 2.0 mit SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), enthält einige ich ausgeführt wurde, um die Unterschiede zwischen diesen beiden Paging Verfahren beim paging durch eine Datenbanktabelle mit weisen Leistungstests 50.000 Datensätze. Bei diesen Tests untersucht ich beide die Uhrzeit zum Ausführen der Abfrage auf der SQL Server-Ebene (mit [SQL Profiler](https://msdn.microsoft.com/en-us/library/ms173757.aspx)) und an die Seite mit ASP.NET [ASP.NET s ablaufverfolgungsfeatures](https://msdn.microsoft.com/en-US/library/y13fw6we.aspx). Bedenken Sie, dass diese Tests auf meinem Entwicklung mit einem einzelnen aktiven Benutzer ausgeführt wurden und deshalb unwissenschaftlichen sind und nicht eine typische Website Auslastungsmustern zu imitieren. Unabhängig davon, veranschaulichen die Ergebnisse der relativen Unterschiede werden in die Ausführungszeit für Standard- und benutzerdefinierte Paging, bei der Arbeit mit ausreichend große Mengen von Daten.


|  | **Durchschn. Dauer (Sek.)** | **Lesevorgänge** |
| --- | --- | --- |
| **Standardmäßig Paging SQL Profiler** | 1.411 | 383 |
| **Benutzerdefinierte Paging SQL Profiler** | 0.002 | 29 |
| **Paging ASP.NET-Standardablaufverfolgung** | 2.379 | *N/V* |
| **Benutzerdefinierte Paging ASP.NET-Ablaufverfolgung** | 0.029 | *N/V* |


Wie Sie sehen können, das Abrufen von Daten einer bestimmten Seite 354 weniger Lesevorgänge durchschnittlich erforderlich und in einem Bruchteil der Zeit abgeschlossen. Auf der ASP.NET-Seite benutzerdefinierte Seite konnte für das Rendern nahe bei 1/100<sup>ten</sup> der Zeit bei Verwendung von Standardnavigation werden gedauert. Finden Sie unter [Meine Artikel](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) für Weitere Informationen zu diesen Ergebnissen zusammen mit Code und eine Datenbank, die Sie zum Reproduzieren von Tests in Ihrer eigenen Umgebung herunterladen können.

## <a name="summary"></a>Zusammenfassung

Standardnavigation ist ein Kinderspiel nur Überprüfung der Paging aktivieren das Kontrollkästchen in den Daten Web Control s smart Tag implementiert, jedoch solche Einfachheit stammen, auf Kosten der Leistung. Bei der Standardnavigation, wenn ein Benutzer eine beliebige Seite der Daten anfordert *alle* Datensätze zurückgegeben werden, obwohl nur ein kleiner Teil davon angezeigt werden darf. Um diese Verwaltungsaufwand zu begegnen, bietet das ObjectDataSource ein alternative Paging Option benutzerdefiniertes Paging.

Während das benutzerdefiniertes Paging verbessert Standard paging s Leistungsprobleme durch das Abrufen von nur Datensätze, die angezeigt werden, müssen sie s komplizierter benutzerdefiniertes Paging implementieren. Zuerst muss eine Abfrage geschrieben werden, die ordnungsgemäß (und effizient) bestimmte Teilmenge der Datensätze, die angeforderte zugreift. Dies kann auf vielfältige Weise erreicht werden. die in diesem Lernprogramm werden untersucht ist mit SQL Server 2005 s neue `ROW_NUMBER()` Rang Funktion durchgeführt, und klicken Sie dann nur die zurückzugebenden Ergebnisse, dessen Rangfolge in einem angegebenen Bereich fällt. Darüber hinaus müssen wir eine Möglichkeit zum Bestimmen der Gesamtzahl der Datensätze, die ausgelagerte über hinzufügen. Nach dem Erstellen dieser Methoden DAL und BLL, müssen wir auch das ObjectDataSource konfigurieren, sodass sie ermitteln kann, wie viele Datensätze werden über ausgelagerte und Zeilenindex starten und die maximale Anzahl an Zeilen Werte ordnungsgemäß an der BLL erfolgreich.

Beim Implementieren von benutzerdefinierten Paging eine Reihe von Schritten erfordert und nicht annähernd so einfach wie Standardnavigation ist, ist benutzerdefiniertes Paging beim paging durch ausreichend große Mengen von Daten. Wie die Ergebnisse untersucht angezeigte, benutzerdefinierte Paging Sekunden aus dem ASP.NET Render Seitenzeit bringen kann, und kann die Last auf dem Datenbankserver beträchtlichem eine oder mehrere crashes.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](paging-and-sorting-report-data-vb.md)
[Weiter](sorting-custom-paged-data-vb.md)
