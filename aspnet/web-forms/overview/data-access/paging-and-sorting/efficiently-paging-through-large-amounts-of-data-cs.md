---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Effizientes Auslagern von großen Datenmengen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Die Standardoption für die Auslagerung des Steuerelements eine Präsentation ist ungeeignet, bei der Arbeit mit großen Mengen von Daten, wie die zugrunde liegenden Data Source Control Retriev...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: feebee845a19a7cb462127a893a30ac7e0761965
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828328"
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Effizientes Auslagern von großen Datenmengen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) oder [PDF-Datei herunterladen](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> Die Standardoption für die Auslagerung des Steuerelements eine Präsentation ist ungeeignet, bei der Arbeit mit großen Mengen von Daten, während die zugrunde liegenden Datenquellen-Steuerelement alle Datensätze abruft, auch wenn nur eine Teilmenge der Daten angezeigt wird. In solchen Fällen müssen wir aktivieren an benutzerdefinierte paging.


## <a name="introduction"></a>Einführung

Wie im vorherigen Tutorial erläutert, kann die Auslagerung auf zwei Arten implementiert werden:

- **Das Standardpaging** kann durch einfaches Aktivieren der Option Paging aktivieren implementiert werden in den Daten-Websteuerelement s Smarttag; jedoch, wenn eine Seite mit Daten anzeigen zu können, dem ObjectDataSource-Steuerelement ruft *alle* sogar der Datensätze jedoch nur ein Teil davon auf der Seite angezeigt werden
- **Benutzerdefiniertes Paging** verbessert die Leistung von standardmäßig paging wird durch das Abrufen von nur die Datensätze aus der Datenbank, die für die bestimmte Seite der Daten, die angefordert wird, durch den Benutzer angezeigt werden müssen, benutzerdefiniertes Paging umfasst jedoch ein wenig mehr Aufwand zum Implementieren als Standardpaging

Aufgrund der einfachen Implementierung nur überprüfen ein Kontrollkästchen, und Sie erneut es geschafft! das Standardpaging ist eine attraktive Option. Die Na Ve Ansatz beim Abrufen aller Datensätze, erleichtert jedoch eine Kernfunktion Auswahl beim paging durch ausreichend große Mengen von Daten oder Websites mit vielen gleichzeitigen Benutzern. In solchen Fällen müssen wir an das benutzerdefinierte paging, um eine reaktionsfähige System bieten deaktivieren.

Die Herausforderung, benutzerdefiniertes Paging wird eine Abfrage schreiben, die den genauen Satz von Datensätzen, die für eine bestimmte Seite der Daten erforderlichen zurückgibt. Glücklicherweise bietet Microsoft SQL Server 2005 ein neues Schlüsselwort für die Rangfolge Ergebnisse zu erzielen, das können wir eine Abfrage schreiben, die effizient die richtige Teilmenge der Datensätze abrufen können. In diesem Tutorial sehen wir, wie Sie dieses neue SQL Server 2005-Schlüsselwort verwenden, um benutzerdefiniertes Paging in einem GridView-Steuerelement zu implementieren. Die Benutzeroberfläche für das benutzerdefinierte Paging für standardmäßige auslagerungen, schrittweise Ausführung von einer Seite, mit dem nächsten identisch ist kann benutzerdefiniertes Paging um ein Vielfaches schneller als das Standardpaging sein.

> [!NOTE]
> Der genaue Leistungsgewinn, die von benutzerdefiniertem Paging hängt von der Gesamtzahl der Datensätze, die über ausgelagert wird und die Last auf dem Datenbankserver platziert wird. Am Ende dieses Tutorials suchen wir auf einige groben Metriken, die die Vorteile in Bezug auf Leistung, die benutzerdefinierte Paginierung abgerufenes vorstellen.


## <a name="step-1-understanding-the-custom-paging-process"></a>Schritt 1: Grundlegendes zu den benutzerdefinierten Paging-Prozess

Beim paging durch die Daten, hängen die präzise Datensätze in einer Seite die Seite mit Daten, die angefordert wird und die Anzahl der Datensätze pro Seite angezeigt. Angenommen Sie, wir wollten Seiten über die 81 Produkte, 10 Produkte pro Seite angezeigt. Die erste Seite anzeigen zu können, möchten wir, dass d Produkte 1 bis 10; Wenn Sie die zweite Seite, die wir d Produkte 11 bis 20 usw. interessant anzeigen zu können.

Es gibt drei Variablen, die bestimmen, welche Datensätze abgerufen werden sollen und wie die Paging-Schnittstelle wiedergegeben werden sollen:

- **Starten Sie Zeilenindex** der Index der ersten Zeile der Daten für die Anzeige auf der Seite; dieser Index kann durch Multiplikation den Seitenindex durch die Datensätze pro Seite angezeigten sowie das Hinzufügen eines berechnet werden. Beim durch 10 Datensätze zu einem Zeitpunkt für die erste Seite paging (dessen Seitenindex ist 0), ist der Index der Zeile beginnen z. B. 0 \* 10 + 1 oder 1 für die zweite Seite (dessen Seitenindex ist 1), Zeile der Startindex ist 1 \* 10 + 1 , oder 11.
- **Maximale Zeilenanzahl** die maximale Anzahl der Datensätze pro Seite angezeigt. Diese Variable wird als die maximale Zeilenanzahl bezeichnet, da für die letzte Seite gibt es weniger Datensätze als die Seitengröße zurückgegeben werden kann. Z. B. beim paging durch die 81 10 Produktdatensätze pro Seite, haben die neunte und letzte Seite nur ein Datensatz. Keine Seite, wird mehrere Datensätze als der Wert der maximalen Anzahl von Zeilen jedoch angezeigt werden.
- **Die Anzahl der Datensätze insgesamt** die Gesamtzahl der Datensätze, die über ausgelagert wird. Während diese Variable ist t erforderlich, um zu bestimmen, welche Datensätze für eine bestimmte Seite abgerufen, dies die Paging-Schnittstelle ist aufgrund von. Beispielsweise treten 81 ausgelagerte über Produkte, weiß, dass die Paging-Schnittstelle neun Seitenzahlen in die Pagingbenutzeroberfläche anzuzeigen.

Mit Standard-Auslagerung, wird der Index der Zeile beginnen als Produkt des der Seitenindex und die Seitengröße plus eins, berechnet, während maximale Zeilenanzahl für den einfach die Seitengröße ist. Da das Standardpaging alle Datensätze aus abgerufen werden wird die Datenbank beim Rendern von einer beliebigen Seite der Daten, die den Index für jede Zeile bezeichnet, wodurch Zeilenindex starten Zeile eine einfache Aufgabe verschieben. Darüber hinaus wird die Anzahl der Datensätze insgesamt verfügbar, wie sie s einfach die Anzahl der Datensätze in der Datentabelle (oder welches Objekt verwendet wird, um die Datenbank zu speichern).

Wenn die Variablen Zeilenindex starten und die maximale Zeilenanzahl, muss eine benutzerdefinierte Implementierung der Paginierung nur die genaue Teilmenge der Datensätze, die den Zeilenindex gestartet und bis zur maximalen Anzahl von Zeilen der Anzahl der Datensätze ab, nach, die zurückgeben. Benutzerdefiniertes Paging bietet zwei Herausforderungen:

- Wir müssen in der Lage effizient zuordnen, einen Zeilenindex jede Zeile in die gesamten Daten, die ausgelagerte durch, damit können wir anfangen, Zurückgeben von Datensätzen auf der angegebenen Zeilenindex starten
- Wir müssen die Gesamtzahl der Datensätze, die über ausgelagerte angeben

In den nächsten beiden Schritten untersuchen wir die SQL-Skript erforderlich, um auf diese beiden Probleme zu reagieren. Zusätzlich zu den SQL-Skript verwenden müssen wir auch für die Implementierung von Methoden in der DAL und BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Schritt 2: Rückgabe der Gesamtanzahl von Datensätzen, die ausgelagerte über

Bevor wir So rufen Sie die genaue Teilmenge der Datensätze für die Seite angezeigt wird untersuchen, können Sie s, die Einführung an, wie die Gesamtanzahl von Datensätzen, die über ausgelagerte zurückzugeben. Diese Informationen werden benötigt, um die Pagingbenutzeroberfläche ordnungsgemäß zu konfigurieren. Die Gesamtzahl der von einer bestimmten SQL-Abfrage zurückgegebenen Datensätze abgerufen werden kann, mithilfe der [ `COUNT` Aggregatfunktion](https://msdn.microsoft.com/library/ms175997.aspx). Beispielsweise, um das Bestimmen der Gesamtzahl der Datensätze in der `Products` Tabelle können wir die folgende Abfrage verwenden:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Lassen Sie s DAL eine Methode hinzufügen, die diese Informationen zurückgibt. Insbesondere erstellen wir eine DAL-Methode wird aufgerufen, `TotalNumberOfProducts()` ausführt, die die `SELECT` Anweisung oben.

Öffnen Sie zunächst die `Northwind.xsd` typisierte DataSet-Datei in die `App_Code/DAL` Ordner. Als Nächstes mit der rechten Maustaste auf die `ProductsTableAdapter` im Designer, und wählen Sie die Abfrage hinzufügen. Wie wir gesehen, die in vorherigen Tutorials haben diese Weise können wir die DAL eine neue Methode hinzu, wenn aufgerufen, wird eine bestimmte SQL-Anweisung oder gespeicherte Prozedur ausgeführt. Deaktivieren Sie wie bei unserem TableAdapter-Methoden in vorherigen Tutorials, in diesem Beispiel um eine Ad-hoc-SQL-Anweisung zu verwenden.


![Verwenden Sie eine Ad-hoc-SQL-Anweisung](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Abbildung 1**: Verwenden Sie eine Ad-hoc-SQL-Anweisung


Auf dem nächsten Bildschirm können wir die Art der Abfrage erstellt angeben. Da diese Abfrage die Gesamtanzahl der Datensätze in einem einzelnen, skalaren Wert zurückgibt der `Products` Tabelle wählen Sie die `SELECT` ein einzelnes-Option "Value" zurückgegeben.


![Konfigurieren Sie die Abfrage, um eine SELECT-Anweisung verwenden, die einen einzelnen Wert zurückgibt](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Abbildung 2**: Konfigurieren Sie die Abfrage, um eine SELECT-Anweisung verwenden, die einen einzelnen Wert zurückgibt


Nach dem, der angibt, in des Typs der Abfrage verwenden, müssen wir als Nächstes die Abfrage angeben.


![Verwenden Sie die SELECT COUNT(*) FROM Produktabfrage](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Abbildung 3**: Verwenden Sie die Option (\*) FROM Produktabfrage


Abschließend geben Sie den Namen für die Methode an. Verwenden, als zuvor erwähnte, können s `TotalNumberOfProducts`.


![Benennen Sie die TotalNumberOfProducts DAL-Methode](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Abbildung 4**: Benennen der TotalNumberOfProducts DAL-Methode


Nach dem Klicken auf "Fertig stellen", fügt der Assistent die `TotalNumberOfProducts` Methode, um die DAL. Die skalaren zurückgeben Methoden in der DAL zurück auf NULL festlegbare Typen, für den Fall, dass das Ergebnis aus der SQL-Abfrage ist `NULL`. Unsere `COUNT` Abfragen, jedoch gibt stets einen nicht-`NULL` Wert, unabhängig davon, die DAL-Methode gibt eine auf NULL festlegbare Ganzzahl zurück.

Zusätzlich zur DAL-Methode benötigen wir auch eine Methode in der Geschäftslogikschicht. Öffnen der `ProductsBLL` Klassendatei, und fügen eine `TotalNumberOfProducts` Methode, die Sie einfach der DAL-s aufgerufen `TotalNumberOfProducts` Methode:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

Die DAL s `TotalNumberOfProducts` Methode gibt eine NULL-Werte zulassen ganze Zahl zurück, aber wir erstellt haben die `ProductsBLL` s-Klasse `TotalNumberOfProducts` Methode so, dass die It standard eine ganze Zahl zurückgegeben. Aus diesem Grund müssen wir haben die `ProductsBLL` Klasse s `TotalNumberOfProducts` Methode zurück, der Value-Abschnitt, der von der DAL s zurückgegebenen NULL-Werte zulässt Ganzzahl `TotalNumberOfProducts` Methode. Der Aufruf von `GetValueOrDefault()` Wert der ganzen Zahl Null-Werte zulässt, falls vorhanden; zurückgegeben, falls die NULL-Werte zulassen ganze Zahl ist `null`, jedoch gibt es die ganze Zahl Standardwert 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Schritt 3: Zurückgeben der genauen Teilmenge der Datensätze

Unsere nächste Aufgabe darin, Sie Methoden erstellen, in der DAL und BLL, die den Zeilenindex starten zu akzeptieren und die maximale Zeilenanzahl Variablen oben beschriebenen Szenarien und die entsprechenden Datensätze zurückzugeben. Bevor wir dies tun, können Sie s ein erster Blick auf die erforderlichen SQL-Skript. Die Herausforderung, die für uns ist, dass wir Lage sein muss, weisen Sie einen Index effizient auf jede Zeile in die gesamten Ergebnisse ausgelagerte durch, damit wir nur die Datensätze, die beginnend ab dem Zeilenindex starten (und bis zu die maximale Anzahl der Datensätze Anzahl der Datensätze) zurückgeben kann.

Dies ist eine Herausforderung nicht auf, wenn bereits eine Spalte in der Datenbanktabelle, die als ein Zeilenindex dient vorhanden ist. Auf den ersten Blick könnten wir meinen, dass die `Products` Tabelle s `ProductID` Feld ausreichen würde, wie das erste Produkt `ProductID` von 1, das zweite ein 2, und so weiter. Allerdings lässt das Löschen eines Produkts eine Lücke in der Sequenz Zusammenführungsoperationen werden annulliert diesen Ansatz.

Es gibt zwei allgemeine Verfahren verwendet, um effizient einen Zeilenindex mit den Daten seitenweise, verknüpfen und ermöglichen die genaue Teilmenge der Datensätze abgerufen werden sollen:

- **Mithilfe von SQL Server 2005-s `ROW_NUMBER()` Schlüsselwort** noch nicht mit SQL Server 2005, die `ROW_NUMBER()` Schlüsselwort ordnet einzelnen zurückgegebenen Datensätze basierend auf der eine Sortierung in eine Rangfolge. Diese Rangfolge kann als ein Index der Zeile für jede Zeile verwendet werden.
- **Verwenden eine Tabellenvariable und `SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT` Anweisung](https://msdn.microsoft.com/library/ms188774.aspx) können verwendet werden, um wie viele Datensätze insgesamt Geben Sie eine Abfrage verarbeitet werden soll, bevor Sie beendet wird; [Tabellenvariablen](http://www.sqlteam.com/item.asp?ItemID=9454) sind lokale T-SQL-Variablen, die Tabellendaten, akin zum aufnehmen können [temporäre Tabellen](http://www.sqlteam.com/item.asp?ItemID=2029). Dieser Ansatz eignet sich gleichermaßen gut mit Microsoft SQL Server 2005 und SQL Server 2000 (während der `ROW_NUMBER()` Ansatz funktioniert nur mit SQL Server 2005).  
  
  Die Idee dabei ist, um eine Tabellenvariable zu erstellen, verfügt ein `IDENTITY` Spalten- und Spalten für die Primärschlüssel der Tabelle, deren Daten über ausgelagert wird werden. Als Nächstes wird der Inhalt der Tabelle, deren Daten über ausgelagert wird werden, in die Table-Variable, wodurch zuordnen einen sequenziellen Zeilenindex ausgegeben (über die `IDENTITY` Spalte) für jeden Datensatz in der Tabelle. Wenn die Tabellenvariable aufgefüllt wurde, eine `SELECT` -Anweisung für die Table-Variable, mit der zugrunde liegenden Tabelle verknüpft, um bestimmte Datensätze herauszuziehen ausgeführt werden kann. Die `SET ROWCOUNT` Anweisung wird verwendet, um die Anzahl der Datensätze auf intelligente Weise zu beschränken, die in der Tabellenvariablen-diagnosesicherungsmechanismus gesichert werden müssen.  
  
  Dieser Ansatz s Effizienz basiert darauf, dass die Nummer der Seite angefordert wird, als die `SET ROWCOUNT` Wert wird den Wert der Startindex für die Zeile sowie die maximale Zeilenanzahl zugewiesen. Dieser Ansatz ist sehr effizient, wenn paging durch Seiten wie z. B. die erste niedrig nummerierten einige Seiten mit Daten. Allerdings weist das Programm standardmäßig Paging-ähnliche Leistung beim Abrufen einer Seite in der Nähe des.

Dieses Lernprogramm implementiert nur benutzerdefinierte Paging mithilfe der `ROW_NUMBER()` Schlüsselwort. Weitere Informationen zur Verwendung der Tabellenvariablen und `SET ROWCOUNT` Verfahren finden Sie unter [eine weitere effizienteste Methode für das Paging über große Resultsets](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Die `ROW_NUMBER()` Schlüsselwort jeden Datensatz zurückgegeben, die über einer bestimmten Reihenfolge mit der folgenden Syntax eine Rangfolge zugeordnet:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` Gibt einen numerischen Wert, der angibt, den Rang für jeden Datensatz im Hinblick auf die der angegebenen Reihenfolge zurück. Beispielsweise kann erkundigen, den Rang für jedes Produkt, die von den am stärksten sortiert teuer zu, wir die folgende Abfrage verwenden:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Abbildung 5 zeigt diese Abfrage s Ergebnisse angezeigt, wenn Sie über den Abfragefenster in Visual Studio ausführen. Beachten Sie, dass die Produkte nach Preis, zusammen mit dem Rang der Preis für jede Zeile sortiert werden.


![Befindet sich der Preis Rang für jedes zurückgegebene Datensatz](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Abbildung 5**: befindet sich der Preis Rang für jedes zurückgegebene Datensatz


> [!NOTE]
> `ROW_NUMBER()` ist nur eine von den vielen neuen Rangfolgefunktionen in SQL Server 2005 verfügbar. Eine ausführlichere Erläuterung der `ROW_NUMBER()`, lesen Sie zusammen mit den anderen Rangfolgefunktionen [aufgelisteten Ergebnisse zurückgeben, mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Wenn die Ergebnisse mit der angegebenen Rangfolge `ORDER BY` -Spalte in der `OVER` Klausel (`UnitPrice`, im obigen Beispiel), SQL Server müssen die Ergebnisse zu sortieren. Dies ist ein schneller Vorgang bei ein gruppierter Index über die Spalten, die die Ergebnisse werden, indem sortiert sind oder wenn es ein abdeckender index, aber andernfalls teuer werden können. Zur Verbesserung der Leistung für ausreichend große Abfragen, erwägen Sie einen nicht gruppierter Index für die Spalte mit der die Ergebnisse nach sortiert sind. Finden Sie unter [Rangfolgefunktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) eine ausführlichere Betrachtung der Systemleistung.

Von zurückgegebenen Rangfolgeinformationen `ROW_NUMBER()` kann nicht direkt verwendet werden, der `WHERE` Klausel. Allerdings kann eine abgeleitete Tabelle verwendet werden, zum Zurückgeben der `ROW_NUMBER()` Ergebnis, das dann in angezeigt werden kann die `WHERE` Klausel. Die folgende Abfrage verwendet beispielsweise eine abgeleitete Tabelle zurückzugebenden ProductName und UnitPrice-Spalten, zusammen mit den `ROW_NUMBER()` Ergebnis und verwendet dann ein `WHERE` -Klausel, um nur solche Produkte, deren Preis Rang zurück zwischen 11 und 20 ist:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Erweitern dieses Konzept etwas weiter, kann dieser Ansatz zum Abrufen einer bestimmten Seite der Daten, die anhand der gewünschten Zeile startIndex und die maximale Zeilenanzahl Werte verwenden:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Wie wir später in diesem Tutorial sehen die *`StartRowIndex`* vom dem ObjectDataSource-Steuerelement wird indiziert, beginnend mit 0 (null), während die `ROW_NUMBER()` Rückgabewert von SQL Server 2005 wird indiziert, beginnend mit 1. Aus diesem Grund die `WHERE` die Klausel gibt Datensätze zurück, in denen `PriceRank` ist größer als *`StartRowIndex`* und kleiner als oder gleich *`StartRowIndex`*  +  *`MaximumRows`*.


Nun, wir haben erläutert, wie `ROW_NUMBER()` kann zum Abrufen einer bestimmten Seite der Daten, die der Index der Zeile beginnen und die maximale Zeilenanzahl Werte verwendet, müssen wir nun diese Logik als im BLL- und DAL-Methoden zu implementieren.

Bei der Erstellung der Abfrage, dass die Reihenfolge wir entscheiden, müssen werden nach der die Ergebnisse geordnet werden; Lassen Sie s, die die Produkte anhand ihres Namens in alphabetischer Reihenfolge sortieren. Dies bedeutet, dass durch das benutzerdefinierte Paging-Implementierung in diesem Tutorial wir nicht können, um eine benutzerdefinierte mehrseitigen Bericht zu erstellen, als auch sortiert werden können. Im nächsten Tutorial allerdings sehen wie solche Funktionen bereitgestellt werden kann.

Im vorherigen Abschnitt haben wir die DAL-Methode als Ad-hoc-SQL-Anweisung erstellt. Leider der T-SQL-Parser in Visual Studio verwendet, die für die TableAdapter-Assistenten t wie die `OVER` Syntax ein, die die `ROW_NUMBER()` Funktion. Aus diesem Grund müssen wir diese DAL-Methode als eine gespeicherte Prozedur erstellen. Wählen Sie im Server-Explorer aus dem Menü "Ansicht" (oder drücken Strg + Alt + S), und erweitern Sie die `NORTHWND.MDF` Knoten. Um eine neue gespeicherte Prozedur hinzuzufügen, mit der rechten Maustaste auf den Knoten gespeicherte Prozeduren, und wählen Sie eine neue gespeicherte Prozedur hinzufügen (siehe Abbildung 6).


![Fügen Sie eine neue gespeicherte Prozedur für das Paging durch die Produkte hinzu.](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Abbildung 6**: Fügen Sie eine neue gespeicherte Prozedur für das Paging durch die Produkte hinzu.


Diese gespeicherte Prozedur sollte zwei ganzzahlige Eingabeparameter entgegen – akzeptieren `@startRowIndex` und `@maximumRows` und verwenden Sie die `ROW_NUMBER()` Funktion sortiert nach der `ProductName` Feld nur die Zeilen zurückgeben, die größer als der angegebene `@startRowIndex` und kleiner als oder gleich `@startRowIndex`  +  `@maximumRow` s. Geben Sie das folgende Skript in die neue gespeicherte Prozedur, und klicken Sie dann auf das Symbol "Speichern" um die gespeicherte Prozedur in der Datenbank hinzuzufügen.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Nach dem Erstellen der gespeicherten Prozedur an, nehmen Sie einen Moment Zeit, um dies zu testen. Mit der rechten Maustaste auf die `GetProductsPaged` gespeicherten Prozedur benennen Sie im Server-Explorer, und wählen Sie die Execute-Option. Visual Studio fordert Sie dann für die Eingabeparameter, `@startRowIndex` und `@maximumRow` s (siehe Abbildung 7). Probieren Sie unterschiedliche Werte aus, und überprüfen Sie die Ergebnisse.


![Geben Sie einen Wert für die @startRowIndex und @maximumRows Parameter](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Abbildung 7</strong>: Geben Sie einen Wert für die @startRowIndex und @maximumRows Parameter


Nach dem Auswählen dieser Parameterwerte eingeben, das Fenster "Ausgabe" zeigt die Ergebnisse. Abbildung 8 zeigt die Ergebnisse beim Übergeben von 10 für beide die `@startRowIndex` und `@maximumRows` Parameter.


[![Der Datensätze, würde werden in der zweiten Seite der Daten werden zurückgegeben.](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Abbildung 8**: der Datensätze, würde werden in der zweiten Seite der Daten zurückgegeben werden ([klicken Sie, um das Bild in voller Größe anzeigen](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Mit dieser gespeicherten Prozedur erstellt, es erneut bereit, die zum Erstellen der `ProductsTableAdapter` Methode. Öffnen der `Northwind.xsd` typisierte DataSet, mit der rechten Maustaste in den `ProductsTableAdapter`, und wählen Sie die Option "hinzufügen". Anstatt die Abfrage, die mit Ad-hoc-SQL-Anweisungen erstellen, erstellen Sie mithilfe einer vorhandenen gespeicherten Prozedur.


![Erstellen Sie die DAL-Methode, die mit einer vorhandenen gespeicherten Prozedur](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Abbildung 9**: Erstellen Sie die DAL-Methode, die mit einer vorhandenen gespeicherten Prozedur


Als Nächstes werden wir dazu aufgefordert werden, wählen Sie die gespeicherte Prozedur aufrufen. Wählen Sie die `GetProductsPaged` gespeicherte Prozedur aus der Dropdown-Liste.


![Wählen Sie die GetProductsPaged gespeicherte Prozedur aus der Dropdown-Liste](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Abbildung 10**: Wählen Sie die GetProductsPaged gespeicherte Prozedur aus der Dropdown-Liste


Im nächste Bildschirm dann gefragt, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird: Tabellendaten, die einen einzelnen Wert oder keinen Wert. Da die `GetProductsPaged` gespeicherte Prozedur können mehrere Datensätze zurückgeben, um anzugeben, dass es tabellarische Daten zurückgibt.


![Anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurück.](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Abbildung 11**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurück.


Abschließend geben Sie den Namen der Methoden, die Sie erstellt haben möchten. Wie bei unseren vorherigen Tutorials fahren Sie fort, und erstellen Sie Methoden, die sowohl die Füllung mithilfe einer "DataTable" und zurückgeben Sie DataTable. Benennen Sie die erste Methode `FillPaged` und das zweite `GetProductsPaged`.


![Namen der Methoden FillPaged und GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Abbildung 12**: Namen der Methoden FillPaged und GetProductsPaged


Erstellt eine DAL-Methode, um eine bestimmte Seite der Produkte zurückzugeben, müssen wir darüber hinaus auch die BLL solche Funktionalität. Wie bei der DAL-Methode die BLL s GetProductsPaged-Methode muss zwei ganzzahlige Eingaben für das Angeben der Startindex für die Zeile und der maximalen Anzahl von Zeilen akzeptieren, und muss nur die Datensätze, die innerhalb des angegebenen Bereichs liegen zurückgeben. Erstellen Sie eine solche Methode BLL, in der ProductsBLL-Klasse, dass nur Aufrufe nach unten an die DAL s GetProductsPaged-Methode, wie folgt:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Sie können einen beliebigen Namen verwenden Entscheidung, wie wir gleich sehen werden, aber für die BLL Eingabeparameter für das s-Methode, für den Einsatz `startRowIndex` und `maximumRows` erspart einen zusätzlichen Aufwand beim Konfigurieren einer "ObjectDataSource" zur Verwendung dieser Methode.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Schritt 4: Konfigurieren der "ObjectDataSource" zur Verwendung von benutzerdefiniertem Paging

Mit den BLL- und DAL-Methoden für den Zugriff auf eine bestimmte Teilmenge der Datensätze, die vollständige steuern wir erneut bereit, die zum Erstellen einer GridView-Ansicht dieser Seiten durch die zugrunde liegenden Datensätze, die mithilfe von benutzerdefinierten Paging. Öffnen Sie zunächst die `EfficientPaging.aspx` auf der Seite die `PagingAndSorting` , Hinzufügen einer GridView-Ansicht auf der Seite, und konfigurieren, um ein neues ObjectDataSource-Steuerelement zu verwenden. In unserer letzten Tutorials haben wir häufig dem ObjectDataSource-Steuerelement für die Verwendung konfiguriert die `ProductsBLL` Klasse s `GetProducts` Methode. Dieses Mal jedoch verwendet werden soll die `GetProductsPaged` Methode stattdessen seit der `GetProducts` Methodenrückgabe *alle* der Produkte in der Datenbank während `GetProductsPaged` gibt nur eine bestimmte Teilmenge der Datensätze.


![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Abbildung 13**: Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode


Seit wir erneut erstellen eine nur-Lese GridView können Sie die Liste der Dropdown-Methode in der INSERT-, Update-, festgelegt, und Löschen von Registerkarten (keine).

Im nächsten Schritt fordert uns der ObjectDataSource-Steuerelement-Assistent für die Datenquellen der `GetProductsPaged` s-Methode `startRowIndex` und `maximumRows` Eingabewerte für Parameter. Diese Eingabeparameter werden tatsächlich von der GridView automatisch festgelegt, also einfach behalten Sie auf "None" der Quelle und klicken Sie auf "Fertig stellen".


![Lassen Sie die Eingabeparameter Quellen mit "None"](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Abbildung 14**: lassen Sie die Eingabeparameter Quellen mit "None"


Nach Abschluss des Assistenten "ObjectDataSource" wird die GridView eine BoundField- oder die CheckBoxField für jedes Produkt Datenfelder enthalten. Können Sie die GridView-s-Darstellung anpassen nach Bedarf. Ich haben sich entschieden, zur Anzeige der `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, und `UnitPrice` BoundFields. Konfigurieren Sie außerdem die GridView zur Unterstützung von Paging durch Aktivieren des Kontrollkästchens Paging aktivieren, in der Smarttag. Nach diesen Änderungen sollte die GridView und "ObjectDataSource" deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Wenn Sie auf die Seite über einen Browser besuchen, jedoch die GridView ist kein Speicherort gefunden werden.


![Das GridView wird nicht angezeigt](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Abbildung 15**: die GridView wird nicht angezeigt


Das GridView ist nicht vorhanden, da es sich bei dem ObjectDataSource-Steuerelement derzeit für beide 0 als Werte verwendet die `GetProductsPaged` `startRowIndex` und `maximumRows` Eingabeparameter. Daher wird die resultierende SQL-Abfrage ist keine Datensätze zurückgeben, und aus diesem Grund GridView wird nicht angezeigt.

Um dies zu beheben, müssen wir dem ObjectDataSource-Steuerelement zur Verwendung von benutzerdefiniertem Paging zu konfigurieren. Dies kann in den folgenden Schritten erfolgen:

1. **Legen Sie das "ObjectDataSource"-s `EnablePaging` Eigenschaft `true`**  Dies gibt an, auf dem ObjectDataSource-Steuerelement, das sie übergeben muss die `SelectMethod` zwei zusätzliche Parameter: einen zur Angabe der Zeilenindex starten ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)), und geben Sie die maximale Zeilenanzahl ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Legen Sie das "ObjectDataSource"-s `StartRowIndexParameterName` und `MaximumRowsParameterName` Eigenschaften entsprechend** der `StartRowIndexParameterName` und `MaximumRowsParameterName` Eigenschaften geben die Namen der übergebenen Eingabeparameter an die `SelectMethod` für benutzerdefiniertes Paging-Zwecke. Standardmäßig sind diese Parameternamen `startIndexRow` und `maximumRows`, daher, beim Erstellen der `GetProductsPaged` Methode in der BLL, habe ich diese Werte verwendet, für die Eingabeparameter. Wenn Sie unterschiedliche Parameternamen für den BLL-s verwenden `GetProductsPaged` Methode, z. B. `startIndex` und `maxRows`für Beispiel müssen Sie die "ObjectDataSource"-s festgelegt `StartRowIndexParameterName` und `MaximumRowsParameterName` Eigenschaften entsprechend (z. B. StartIndex für `StartRowIndexParameterName` und MaxRows für `MaximumRowsParameterName`).
3. **Legen Sie das "ObjectDataSource"-s [ `SelectCountMethod` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) auf den Namen der Methode, die die gesamte Anzahl von Datensätze wird ausgelagerten über zurückgibt (`TotalNumberOfProducts`)** Bedenken Sie, dass die `ProductsBLL` Klasse s `TotalNumberOfProducts`Methode gibt die Gesamtzahl der Datensätze, die ausgelagerte durch die Verwendung einer DAL-Methode, die ausgeführt wird ein `SELECT COUNT(*) FROM Products` Abfrage. Diese Informationen werden von dem ObjectDataSource-Steuerelement benötigt, um die Paging-Schnittstelle ordnungsgemäß zu rendern.
4. **Entfernen Sie die `startRowIndex` und `maximumRows` `<asp:Parameter>` Elemente aus der "ObjectDataSource"-s deklaratives Markup** beim dem ObjectDataSource-Steuerelement mithilfe des Assistenten konfigurieren zu können, Visual Studio automatisch hinzugefügt, zwei `<asp:Parameter>` Elemente für die `GetProductsPaged` s-Methode Eingabeparameter. Durch Festlegen von `EnablePaging` zu `true`, diese Parameter werden automatisch übergeben werden, wenn sie auch in der deklarativen Syntax angezeigt werden, wird dem ObjectDataSource-Steuerelement übergeben versucht *vier* Parameter für die `GetProductsPaged` Methode und zwei Parameter an die `TotalNumberOfProducts` Methode. Wenn Sie vergessen, entfernen Sie diese `<asp:Parameter>` Elemente, wenn die Seite über einen Browser besuchen Sie eine Fehlermeldung wie erhalten: *ObjectDataSource "ObjectDataSource1" nicht gefunden. eine nicht generische Methode "TotalNumberOfProducts", die Parameter: StartRowIndex MaximumRows*.

Nach diesen Änderungen durchführen, sollte die deklarative Syntax des "ObjectDataSource"-s wie folgt aussehen:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Beachten Sie, dass die `EnablePaging` und `SelectCountMethod` festgelegt wurde und die `<asp:Parameter>` Elemente entfernt wurden. Abbildung 16 zeigt einen Screenshot des Fensters Eigenschaften auf, nachdem diese Änderungen vorgenommen wurden.


![Um benutzerdefinierte Paginierung zu verwenden, konfigurieren Sie das ObjectDataSource-Steuerelement](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Abbildung 16**: Wenn benutzerdefiniertes Paging verwenden möchten, konfigurieren Sie das ObjectDataSource-Steuerelement


Finden Sie nach diesen Änderungen auf dieser Seite über einen Browser. Daraufhin sollte die 10 Produkte aufgeführt, alphabetisch angeordnet sind. Nehmen Sie einen Moment Zeit, die eine Seite mit Daten zu einem Zeitpunkt durchlaufen. Es gibt zwar keine visuellen Unterschied aus der Perspektive des Endbenutzers s zwischen Standard und benutzerdefiniertes Paging, Seiten benutzerdefinierte paging effizienter über große Mengen von Daten als nur die Datensätze abgerufen, die für eine bestimmte Seite angezeigt werden soll.


[![Die Daten, die sortiert vom Produkt s Namen ist ausgelagerten verwenden benutzerdefinierte Paging.](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Abbildung 17**: die Daten, sortiert vom Produkt s Namen ist ausgelagerten benutzerdefinierte mithilfe von Paging ([klicken Sie, um das Bild in voller Größe anzeigen](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Mit benutzerdefiniertem Paging, Anzahl die Seite der Rückgabewert von "ObjectDataSource" s `SelectCountMethod` wird in den GridView-s-Ansichtszustand gespeichert. Andere GridView-Variablen den `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` Auflistung usw. befinden sich *Steuerelementzustand*, dem wird beibehalten, unabhängig vom Wert der GridView Zuordnungsvorgänge `EnableViewState` Diese Eigenschaft. Da die `PageCount` Wert wird beibehalten postbackübergreifend über den Ansichtszustand, wenn eine Paging-Schnittstelle verwenden, die einen Link, um Sie zu der letzten Seite gelangen enthält, ist es zwingend erforderlich, der Ansichtszustand des GridView-s aktiviert werden. (Wenn Ihre Paging-Schnittstelle keinen direkten Link zur letzten enthält Seite und der Ansichtszustand deaktiviert werden kann.)


Bewirkt, dass einen Postback und weist die GridView aktualisieren Sie auf den Seitenlink der letzten die `PageIndex` Eigenschaft. Wenn die letzte Seitenlink geklickt wird, weist die GridView seine `PageIndex` Eigenschaft auf einen Wert aus einer kleiner als die `PageCount` Eigenschaft. Mit der Ansichtszustand deaktiviert ist die `PageCount` Wert postbackübergreifend verloren und die `PageIndex` stattdessen die maximal zulässige Ganzzahl-Wert zugewiesen wird. Als Nächstes die GridView versucht, den Startzeilenindex durch Multiplikation zu bestimmen die `PageSize` und `PageCount` Eigenschaften. Dies führt zu einer `OverflowException` , da das Produkt die maximale zulässige ganzzahlige Größe überschreitet.

## <a name="implement-custom-paging-and-sorting"></a>Benutzerdefinierte Paginierung implementieren und sortieren

Unsere aktuelle benutzerdefiniertes Paging-Implementierung muss die Reihenfolge, die mit dem die Daten werden über ausgelagert statisch angegeben werden, beim Erstellen der `GetProductsPaged` gespeicherte Prozedur. Allerdings haben Sie möglicherweise notiert haben, dass das GridView-s-Smarttag ein Kontrollkästchen Sortieren aktivieren die Option zum Aktivieren der Auslagerungsdatei enthält. Leider wird das Hinzufügen von Unterstützung der datenquellensortierung an die GridView mit unserer aktuellen benutzerdefiniertes Paging-Implementierung nur die Datensätze auf die aktuell angezeigten Seite der Daten sortiert. Z. B. Wenn Sie die GridView auch unterstützen Paginierung, und klicken Sie dann beim Anzeigen der ersten Seite der Daten konfigurieren in absteigender Reihenfolge nach Produktnamen sortiert wird die Reihenfolge der Produkte auf der Seite "1" rückgängig gemacht. Wie in Abbildung 18 dargestellt wird, zeigt eine solche Carnarvon-Tiger als das erste Produkt in umgekehrter alphabetischer Reihenfolge sortieren die 71 anderen Produkten ignoriert, die Carnarvon-Tiger, alphabetisch folgen; nur die Datensätze auf der ersten Seite werden in der Sortierung berücksichtigt.


[![Nur die Daten angezeigt, auf der aktuellen Seite wird sortiert.](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Abbildung 18**: nur die Daten angezeigt, auf der aktuellen Seite sortiert wird ([klicken Sie, um das Bild in voller Größe anzeigen](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


Die Sortierung gilt nur für die aktuelle Seite der Daten, da die Sortierung erfolgt nach dem Abrufen der das von der BLL s `GetProductsPaged` -Methode, und diese Methode gibt nur die Datensätze zur aktiven Seite. Um ordnungsgemäß sortieren zu implementieren, müssen wir übergeben den Sortierungsausdruck auf die `GetProductsPaged` Methode so, dass die Daten entsprechend vor der Rückgabe der speziellen Seite der Daten bewertet werden können. Wir sehen, wie Sie dies in unserem nächsten Tutorial zu erreichen.

## <a name="implementing-custom-paging-and-deleting"></a>Implementieren benutzerdefinierte Paginierung und löschen

Wenn Sie das Löschen von Funktionen in einer GridView-Ansicht, deren Daten werden mithilfe von benutzerdefinierten Paging-Techniken, die Sie finden, wenn Sie den letzten Datensatz von der letzten Seite löschen ausgelagert, GridView statt entsprechend dekrementiert die GridView s verschwindet`PageIndex`. Um diesen Fehler zu reproduzieren, aktivieren Sie für das Tutorial einfach neu erstellten löschen. Wechseln Sie zur letzten Seite (Seite 9), wo sollte ein einzelnes Produkt angezeigt werden, da wir über 81 Produkte, 10 zu einem Zeitpunkt paging sind. Löschen Sie dieses Produkt.

Beim Löschen des letzten Produkts, das GridView *sollten* automatisch zur achte Seite wechseln und diese Funktionalität wird offensichtlich, bei der Standardnavigation. Mit benutzerdefiniertem Paging nicht mehr angezeigt wird jedoch nach dem Löschen dieses letzte Produkts auf der letzten Seite die GridView einfach auf dem Bildschirm ganz. Die genaue Ursache *warum* in diesem Fall ist ein wenig würde den Rahmen dieses Lernprogramms; Siehe [löschen den letzten Datensatz auf der letzten Seite aus einer GridView-Ansicht mit benutzerdefinierte Paging](http://scottonwriting.net/sowblog/posts/7326.aspx) für die Low-Level-Details, die Quelle der Dieses Problem. Zusammenfassend es s aufgrund der folgenden Reihenfolge der Schritte, die von der GridView ausgeführt werden, wenn die Schaltfläche Löschen geklickt wird:

1. Löschen Sie den Datensatz
2. Erhalten Sie die entsprechenden Einträge für den angegebenen anzuzeigende `PageIndex` und `PageSize`
3. Kontrollkästchen, um sicherzustellen, dass die `PageIndex` übersteigt nicht die Anzahl der Seiten mit Daten in der Datenquelle aus, wenn es automatisch die GridView s Verringern der Fall ist, `PageIndex` Eigenschaft
4. Binden Sie die entsprechende Seite der Daten an die GridView unter Verwendung der Datensätze, die in Schritt2 abgerufen

Das Problem rührt daher, in Schritt2 der `PageIndex` verwendet, wenn die anzuzeigenden Datensätze abrufen trotzdem ist die `PageIndex` der letzten Seite, dessen einziger Datensatz wurde gerade gelöscht. Aus diesem Grund in Schritt2 *keine* Datensätze zurückgegeben werden, da diese letzte Seite der Daten nicht mehr Datensätze enthält. Klicken Sie dann in Schritt 3, erkennt die GridView, die die `PageIndex` -Eigenschaft ist größer als die Gesamtzahl der Seiten in der Datenquelle (da wir Ve gelöscht. der letzte Eintrag in der letzten Seite) und aus diesem Grund wird die `PageIndex` Eigenschaft. In Schritt 4 versucht die GridView selbst an den in Schritt2 abgerufenen Daten zu binden. jedoch resultierende in Schritt2 keine Datensätze zurückgegeben wurden, daher in eine leere GridView-Ansicht. Dieses Problem t mit Standard-auslagerungen, surface, da in Schritt2 *alle* Datensätze aus der Datenquelle abgerufen werden.

Zum Beheben dieses Problems haben wir zwei Optionen zur Verfügung. Die erste besteht darin, erstellen Sie einen Ereignishandler für das GridView-s `RowDeleted` -Ereignishandler, der bestimmt, wie viele Datensätze auf der Seite angezeigt wurden, die gerade gelöscht wurde. Wenn gab es nur einen Eintrag und klicken Sie dann der Datensatz einfach gelöscht zuletzt wurde und wir die GridView s verringert müssen `PageIndex`. Natürlich nur aktualisiert werden sollen die `PageIndex` Wenn der Löschvorgang tatsächlich erfolgreich war, die bestimmt werden kann, indem Sie sicherstellen, dass die `e.Exception` Eigenschaft `null`.

Dieser Ansatz funktioniert, da er aktualisiert die `PageIndex` nach Schritt 1, aber vor Schritt2. Aus diesem Grund wird die entsprechende Gruppe von Datensätzen in Schritt2 zurückgegeben. Zu diesem Zweck verwenden Sie Code wie folgt aus:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Eine alternative problemumgehung ist, erstellen Sie einen Ereignishandler für das "ObjectDataSource"-s `RowDeleted` Ereignis und zum Festlegen der `AffectedRows` Eigenschaft mit einem Wert von 1. Nach dem Löschen des Datensatzes in Schritt 1 (jedoch vor dem erneuten Abrufen der Daten in Schritt2), die GridView aktualisiert seine `PageIndex` Eigenschaft, wenn eine oder mehrere Zeilen von dem Vorgang betroffen sind. Allerdings die `AffectedRows` Eigenschaft wird nicht von dem ObjectDataSource-Steuerelement festgelegt, und aus diesem Grund ist dieser Schritt ausgelassen. Eine Möglichkeit, diesen Schritt ausgeführt haben manuell festlegen, wird die `AffectedRows` Eigenschaft, wenn der Löschvorgang erfolgreich abgeschlossen wurde. Dies kann erreicht werden, mithilfe von Code wie folgt:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

Der Code für beide dieser Ereignisse-Handler finden Sie im Code-Behind-Klasse, von der `EfficientPaging.aspx` Beispiel.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Vergleichen die Leistung von Standard- und benutzerdefinierte Paginierung

Da benutzerdefiniertes Paging nur die erforderlichen Datensätze abruft, während das Standardpaging gibt *alle* der Datensätze für jede Seite angezeigt wird, sie deaktivieren, dass benutzerdefinierte Paginierung effizienter als das Standardpaging ist. Aber genau wie viel effizienter benutzerdefiniertes Paging wird? Welche Art von Leistungssteigerungen durch Verschieben von Standardpaging benutzerdefiniertes Paging angezeigt werden?

Leider gibt es s auf keine dieselbe Größe jedem passt alle hier beantwortet. Der Leistungsvorteil auf eine Reihe von Faktoren abhängig ist, die bekannteste zwei wird die Anzahl der Datensätze, die über ausgelagert wird und die Last auf die Datenbank-Server und die Kommunikation der Kanäle zwischen dem Webserver und Datenbankserver platziert. Für kleine Tabellen mit nur wenigen Dutzend Datensätzen kann der Leistungsunterschied vernachlässigbar sein. Bei großen Tabellen mit Tausende oder sogar Hunderte oder Tausende von Zeilen ist jedoch der Leistungsunterschied Spitzen.

Einen Artikel, [benutzerdefiniertes Paging in ASP.NET 2.0 mit SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), enthält Sie einige Webleistungstests, die ich ausgeführt wurde, um die Unterschiede zwischen diesen beiden Paging Verfahren aus, wenn paging durch eine Datenbanktabelle mit aufweisen 50.000 Datensätze. In diesen Tests, die ich überprüft sowohl die Zeit zum Ausführen der Abfrage auf der SQL Server-Ebene (mit [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) und auf der Seite mit ASP.NET [ASP.NET s Ablaufverfolgungsfunktionen](https://msdn.microsoft.com/library/y13fw6we.aspx). Bedenken Sie, dass diese Tests auf meinem Entwicklungscomputer mit einem einzelnen aktiven Benutzer, die ausgeführt wurden und daher unwissenschaftlichen und typische Website Auslastungsmuster ist nicht imitieren. Unabhängig davon, veranschaulichen die Ergebnisse die relativen Unterschiede bei der Ausführungszeit für Standard- und benutzerdefinierte Paginierung, bei der Arbeit mit ausreichend große Mengen von Daten.


|  | **Durchschn. Dauer (s)** | **Reads** |
| --- | --- | --- |
| **Standardmäßig Paging SQL Profiler** | 1.411 | 383 |
| **Benutzerdefiniertes Paging SQL Profiler** | 0.002 | 29 |
| **Paging ASP.NET-Standardablaufverfolgung** | 2.379 | *N/V* |
| **Benutzerdefinierte Paginierung ASP.NET-Ablaufverfolgung** | 0.029 | *N/V* |


Wie Sie sehen können, Abrufen von Daten eine bestimmte Seite 354 weniger Lesevorgänge durchschnittlich erforderlich und in einem Bruchteil der Zeit abgeschlossen. Auf der ASP.NET-Seite benutzerdefinierte Seite konnte in nahe bei 1/100 Rendern<sup>th</sup> der Zeit sie vorgenommen haben, wenn das Standardpaging verwenden. Finden Sie unter [meinen Artikel](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) für Weitere Informationen zu dieser Ergebnisse und Code und eine Datenbank, die Sie zum Reproduzieren dieser Tests in Ihrer eigenen Umgebung herunterladen können.

## <a name="summary"></a>Zusammenfassung

Das Standardpaging ist ganz einfach nur Überprüfung der Paging aktivieren Sie das Kontrollkästchen in den Daten s smart Tag des Websteuerelements implementieren, aber so unkompliziert erfolgt zu Lasten von Leistung. Bei der Standardnavigation, wenn ein Benutzer auf jeder Seite der Daten anfordert *alle* Datensätze zurückgegeben werden, auch wenn nur ein kleiner Teil davon angezeigt werden kann. Um diese Performance-Overhead zu begegnen, bietet dem ObjectDataSource-Steuerelement ein alternative Paging Option benutzerdefiniertes Paging.

Während Sie benutzerdefiniertes Paging optimiert Standardpaging s Leistungsprobleme durch Abrufen der nur die Datensätze, die angezeigt werden, müssen sie s komplizierter benutzerdefiniertes Paging zu implementieren. Zunächst muss eine Abfrage geschrieben werden, die ordnungsgemäß (und effizient) die bestimmte Teilmenge der Datensätze, die angeforderte zugreift. Dies kann auf verschiedene Weise erreicht werden. die untersuchten wir in diesem Tutorial ist auf die Verwendung der neuen SQL Server 2005-s `ROW_NUMBER()` führt die Funktion für die eine Rangfolge, und klicken Sie dann nur diejenigen zurückgegeben Ergebnisse, deren Rang in einem angegebenen Bereich fällt. Darüber hinaus müssen wir eine Möglichkeit, um zu bestimmen, die Gesamtzahl der Datensätze, die über ausgelagerte hinzufügen. Nach dem Erstellen dieser Methoden DAL und der BLL, müssen wir auch dem ObjectDataSource-Steuerelement so konfigurieren, dass er bestimmen kann, wie viele Datensätze insgesamt über ausgelagerte werden und können ordnungsgemäß an der BLL übergeben Sie die Werte Zeilenindex starten und die maximale Zeilenanzahl.

Beim Implementieren von benutzerdefinierten Paging eine Reihe von Schritten erfordert und nicht so einfach wie das Standardpaging ist, ist benutzerdefiniertes Paging eine Notwendigkeit, wenn paging durch ausreichend große Mengen von Daten. Die Ergebnisse untersucht wurde, benutzerdefinierte Paginierung kann Sekunden auf die Renderingzeit für ASP.NET Seite zu verteilen und kann die Last auf dem Datenbankserver nach einem oder mehreren Größenordnungen aufhellen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](paging-and-sorting-report-data-cs.md)
> [Weiter](sorting-custom-paged-data-cs.md)
