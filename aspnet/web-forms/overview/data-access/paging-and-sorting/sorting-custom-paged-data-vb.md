---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Sortieren von benutzerdefinierten ausgelagerten Daten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben wir gelernt benutzerdefiniertes Paging zu implementieren, wenn Daten auf einer Webseite presentating. In diesem Tutorial erfahren Sie wie der vorherigen erweitern...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9921b541e0160054f080ff08468ddfc5cc92373b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363652"
---
<a name="sorting-custom-paged-data-vb"></a>Sortieren von benutzerdefinierten ausgelagerten Daten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) oder [PDF-Datei herunterladen](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> Im vorherigen Tutorial haben wir gelernt benutzerdefiniertes Paging zu implementieren, wenn Daten auf einer Webseite presentating. In diesem Tutorial sehen wir im vorherige Beispiel Unterstützung für die Sortierung von benutzerdefinierten Paging zu erweitern.


## <a name="introduction"></a>Einführung

Im Vergleich zu Standard-auslagerungen, benutzerdefinierte Paginierung kann die Leistung von Paging Daten verbessern, indem ein Vielfaches, sodass benutzerdefiniertes paging die Wahl der de-facto Paging-Implementierung, wenn paging durch große Mengen von Daten. Implementieren von benutzerdefiniertem Paging ist komplizierter als die Implementierung, standardmäßig paging, jedoch vor allem beim Hinzufügen einer Sortierung verwendet. In diesem Tutorial werden wir das Beispiel aus dem vorherigen Unterstützung für die Sortierung erweitern *und* benutzerdefiniertes Paging.

> [!NOTE]
> Seit dem vorherigen Beispiel, in diesem Tutorial aufbaut, vor dem Beginn können Sie die deklarative Syntax im Kopieren der `<asp:Content>` Element aus der vorherigen Lernprogramm-s-Webseite (`EfficientPaging.aspx`) und fügen Sie sie zwischen den `<asp:Content>` Element in der `SortParameter.aspx` Seite. Siehe Schritt 1 von der [Hinzufügen von Steuerelementen zur gültigkeitsprüfung zum Bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) Tutorial für eine ausführlichere Erläuterung zur Replikation von der Funktionalität einer ASP.NET-Seite in einen anderen.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Schritt 1: Überprüfen der das benutzerdefinierte Paging-Verfahren

Für das benutzerdefinierte Paging ordnungsgemäß funktioniert, müssen wir ein Verfahren implementieren, die eine bestimmte Teilmenge der Datensätze, die anhand der Startindex für die Zeile und die maximale Zeilenanzahl Parameter effizient erfasst werden können. Es gibt eine Reihe von Techniken, die verwendet werden kann, um dieses Ziel zu erreichen. Im vorherigen Tutorial erläutert, auf das erreichen dies mithilfe von Microsoft SQLServer 2005 s neue `ROW_NUMBER()` rangfolgefunktion. Kurz gesagt: die `ROW_NUMBER()` rangfolgefunktion weist jede Zeile zurück, die von einer Abfrage, die nach einer angegebenen Sortierreihenfolge geordnet wird eine Zeilennummer. Anschließend wird die entsprechende Teilmenge der Datensätze abgerufen, durch einen bestimmten Teil der nummerierten Ergebnisse zurückgeben. Die folgende Abfrage veranschaulicht, wie Sie dieses Verfahren, um zurückgeben dieser Produkte 11 bis 20 nummeriert, wenn nach der Rangfolge der Ergebnisse alphabetisch sortiert werden. die `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Dieses Verfahren eignet sich gut für Paging, verwenden keine bestimmte Sortierreihenfolge (`ProductName` alphabetisch sortiert, in diesem Fall), aber die Abfrage muss geändert werden, um die Ergebnisse sortiert nach einem Sortierungsausdruck für die verschiedenen zeigen. Im Idealfall die obige Abfrage neu geschrieben werden, um eine in-Parameter verwenden, die `OVER` -Klausel wie folgt:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Leider parametrisiert `ORDER BY` Klauseln sind nicht zulässig. Stattdessen müssen wir erstellen eine gespeicherte Prozedur, die akzeptiert eine `@sortExpression` input-Parameters, verwendet aber von einer der folgenden problemumgehungen:

- Schreiben Sie hart kodierte Abfragen, für die einzelnen die sortierungsausdrücke, die verwendet werden können; Verwenden Sie dann `IF/ELSE` T-SQL-Anweisungen, um zu bestimmen, welche die auszuführende Abfrage.
- Verwenden einer `CASE` Anweisung zu dynamischen `ORDER BY` Ausdrücke basierend auf der `@sortExpressio` n Eingabeparameter; finden Sie unter den wird verwendet, um dynamisch Sortieren von Abfrageergebnissen im Abschnitt [die Leistungsfähigkeit von SQL `CASE` Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Weitere Informationen.
- Gestalten Sie die entsprechende Abfrage als Zeichenfolge in der gespeicherten Prozedur, und verwenden Sie dann [der `sp_executesql` gespeicherte Systemprozedur](https://msdn.microsoft.com/library/ms188001.aspx) um die dynamische Abfrage auszuführen.

Jede der folgenden Vorgehensweisen hat einige Nachteile. Die erste Option ist nicht als verwaltbar als die anderen beiden, da es erfordert, dass Sie eine Abfrage für jeden möglichen Sortierungsausdruck erstellen. Aus diesem Grund, wenn Sie später entscheiden, um die GridView neue, sortierbar Felder hinzuzufügen auch müssen Sie zurückgehen und aktualisieren die gespeicherte Prozedur. Der zweite Ansatz hat einige Besonderheiten, die Bedenken hinsichtlich der Leistung führen, beim Sortieren von Datenbankspalten keine Zeichenfolge und unterliegt auch dieselben Probleme wie die erste Verwaltbarkeit. Und die dritte Option, die dynamisches SQL verwendet, bietet das Risiko für einen SQL-Injection-Angriff aus, wenn ein Angreifer die gespeicherte Prozedur übergeben werden, in der Eingabe von eingabeparameterwerten ihrer Wahl ausführen kann.

Während keiner dieser Ansätze perfekt ist, denke ich, dass die dritte Option das beste aus den drei ist. Mit der Verwendung von dynamischem SQL bietet es sich um ein Maß an Flexibilität, die die beiden anderen nicht der Fall ist. Darüber hinaus kann ein SQL-Injection-Angriff nur ausgenutzt werden, wenn ein Angreifer die gespeicherte Prozedur übergeben, die in den Eingabeparametern seiner Wahl ausführen kann. Da die DAL parametrisierte Abfragen verwendet, schützt ADO.NET diese Parameter, die über die Architektur, auf die Datenbank gesendet werden dies bedeutet, dass die SQL-Injection-Angriff Schwachstelle ist nur vorhanden, wenn der Angreifer die gespeicherte Prozedur direkt ausgeführt werden kann.

Um diese Funktionalität zu implementieren, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank, die mit dem Namen `GetProductsPagedAndSorted`. Diese gespeicherte Prozedur sollte drei Eingabezeichenfolge-Parameter akzeptieren: `@sortExpression`, einen Eingabeparameter vom Typ `nvarchar(100`), der angibt, wie die Ergebnisse sortiert werden soll, und wird direkt nach eingefügt der `ORDER BY` Text in die `OVER` -Klausel und `@startRowIndex` und `@maximumRows`, die gleichen beiden ganzzahligen Eingabeparameter aus der `GetProductsPaged` gespeicherte Prozedur, die im vorherigen Tutorial untersucht. Erstellen der `GetProductsPagedAndSorted` gespeicherte Prozedur, die mithilfe des folgenden Skripts:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

Die gespeicherte Prozedur startet, indem sichergestellt wird, die einen Wert für die `@sortExpression` Parameter angegeben wurde. Wenn sie nicht vorhanden ist, werden die Ergebnisse von geordnet `ProductID`. Als Nächstes wird die dynamische SQL-Abfrage erstellt. Beachten Sie, dass die dynamische SQL-Abfrage hier von unseren vorherigen Abfragen verwendet geringfügig, um alle Zeilen aus Produkttabelle abzurufen. In früheren Beispielen wir jede s zugeordneten Produktkategorie s und Lieferant s-Namen, die mit einer Unterabfrage abgerufen. Diese Entscheidung getroffen wurde, wieder in die [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Tutorial und erfolgte anstelle `JOIN` s weil TableAdapter zugeordnete Einfüge-, nicht automatisch erstellt Update- und delete-Methoden für ein solches Abfragen. Die `GetProductsPagedAndSorted` gespeicherte Prozedur, allerdings muss verwenden `JOIN` s für die Ergebnisse an die Kategorie- bzw. Lieferanteninformationen Namen sortiert werden.

Diese dynamische Abfrage durch die Verkettung der statische Abfrage Teile aufgebaut ist und die `@sortExpression`, `@startRowIndex`, und `@maximumRows` Parameter. Da `@startRowIndex` und `@maximumRows` sind ganzzahlige Parameter, sie müssen in Nvarchars um ordnungsgemäß verkettet werden, konvertiert werden. Sobald diese dynamischen SQL-Abfrage erstellt wurde, wird es über ausgeführt `sp_executesql`.

Testen mit unterschiedlichen Werten für diese gespeicherte Prozedur in Ruhe die `@sortExpression`, `@startRowIndex`, und `@maximumRows` Parameter. Klicken Sie im Server-Explorer mit der rechten Maustaste auf den Namen der gespeicherten Prozedur, und wählen Sie Execute. Dadurch gelangen in die Sie, die Eingabeparameter eingeben können (siehe Abbildung 1) das Dialogfeld für die gespeicherte Ausführungsprozedur. Verwenden Sie zum Sortieren der Ergebnisse durch den Namen der Kategorie "CategoryName", für die `@sortExpression` Parameterwert; CompanyName verwenden Sie zum Sortieren, indem der Name des Lieferanten s-Unternehmen. Nach dem Sie die Parameterwerte angegeben haben, klicken Sie auf "OK". Die Ergebnisse werden im Ausgabefenster angezeigt. Abbildung 2 zeigt die Ergebnisse, wenn 11 bis 20 Produkten zurückgeben eingestuft werden, wenn durch die `UnitPrice` in absteigender Reihenfolge.


![Testen Sie verschiedene Werte für die gespeicherte Prozedur s drei Eingabeparameter](sorting-custom-paged-data-vb/_static/image1.png)

**Abbildung 1**: Probieren Sie verschiedene Werte für die gespeicherte Prozedur s drei Eingabeparameter


[![Die gespeicherte Prozedur s werden Ergebnisse im Ausgabefenster angezeigt.](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Abbildung 2**: die gespeicherte Prozedur s Ergebnisse werden im Ausgabefenster angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Wenn die Ergebnisse mit der angegebenen Rangfolge `ORDER BY` -Spalte in der `OVER` -Klausel, SQL Server muss die Ergebnisse zu sortieren. Dies ist ein schneller Vorgang bei ein gruppierter Index über die Spalten, die die Ergebnisse nach sortiert wird werden oder wenn es ein abdeckender index, aber andernfalls teuer werden können. Zur Verbesserung der Leistung bei ausreichend großen Abfragen erwägen Sie, einen nicht gruppierten Index für die Spalte mit der die Ergebnisse nach sortiert sind. Finden Sie unter [Rangfolgefunktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) Weitere Details.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Schritt 2: Erweitern von Datenzugriffs- und Geschäftslogikschichten

Mit der `GetProductsPagedAndSorted` erstellten gespeicherten Prozedur im nächsten Schritt wird eine Möglichkeit zum Ausführen dieser gespeicherten Prozedur über die Architektur unserer Anwendung bereitstellen. Dies umfasst das Hinzufügen einer geeigneten Methode zum BLL- und DAL. Lassen Sie s beginnen, indem eine Methode an die DAL hinzufügen. Öffnen der `Northwind.xsd` typisierte DataSet, mit der rechten Maustaste auf die `ProductsTableAdapter`, und wählen Sie im Kontextmenü die Option "hinzufügen". Wie im vorherigen Tutorial so konfigurieren Sie diese neuen DAL-Methode, um eine vorhandene gespeicherte Prozedur - verwenden werden sollten `GetProductsPagedAndSorted`, in diesem Fall. Starten Sie an, dass Sie die neue TableAdapter-Methode, um eine vorhandene gespeicherte Prozedur verwenden möchten.


![Wählen Sie eine vorhandene gespeicherte Prozedur verwenden.](sorting-custom-paged-data-vb/_static/image5.png)

**Abbildung 3**: Wählen Sie eine vorhandene gespeicherte Prozedur verwenden.


Wählen Sie zum Angeben der gespeicherten Prozedur verwendet die `GetProductsPagedAndSorted` gespeicherte Prozedur aus der Dropdown-Liste im nächsten Bildschirm.


![Verwenden Sie die GetProductsPagedAndSorted gespeicherten Prozedur](sorting-custom-paged-data-vb/_static/image6.png)

**Abbildung 4**: Verwenden Sie die GetProductsPagedAndSorted gespeicherten Prozedur


Diese gespeicherte Prozedur gibt eine Gruppe von Datensätzen zurück, wie die Ergebnisse also auf dem nächsten Bildschirm angegeben, dass sie die tabellarische Daten zurückgibt.


![Anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurück.](sorting-custom-paged-data-vb/_static/image7.png)

**Abbildung 5**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurück.


Erstellen Sie schließlich DAL Methoden, mit denen sowohl die Füllung einer "DataTable" und Zurückgeben einer DataTable-Muster, und benennen die Methoden `FillPagedAndSorted` und `GetProductsPagedAndSorted`bzw.


![Wählen Sie die Methodennamen](sorting-custom-paged-data-vb/_static/image8.png)

**Abbildung 6**: Wählen Sie die Methodennamen


Nun, wir haben erweitert die DAL aus, es erneut bereit, um an die BLL zu aktivieren. Öffnen der `ProductsBLL` Klassendatei, und fügen Sie eine neue Methode `GetProductsPagedAndSorted`. Diese Methode akzeptiert drei Eingabezeichenfolge-Parameter muss `sortExpression`, `startRowIndex`, und `maximumRows` und einfach in die DAL s aufrufen sollten `GetProductsPagedAndSorted` -Methode wie folgt:


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Schritt 3: Konfigurieren von dem ObjectDataSource-Steuerelement an Pass im Parameter "SortExpression"

Müssen erweitert die DAL und den BLL, um die Methoden enthalten, die Nutzung der `GetProductsPagedAndSorted` gespeicherte Prozedur, die alle, die verbleibt, wird zu "ObjectDataSource" Konfigurieren der `SortParameter.aspx` Seite zur Verwendung der neuen BLL-Methode und übergeben der `SortExpression` Parameter basierend auf der Spalte, die der Benutzer angefordert hat, um die Ergebnisse nach zu sortieren.

Als erstes ändern das "ObjectDataSource"-s `SelectMethod` aus `GetProductsPaged` zu `GetProductsPagedAndSorted`. Dies kann über das Konfigurieren von Datenquellen-Assistenten, im Eigenschaftenfenster oder direkt über die deklarative Syntax erfolgen. Als Nächstes müssen wir geben Sie einen Wert für "ObjectDataSource" s [ `SortParameterName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Wenn diese Eigenschaft festgelegt ist, versucht dem ObjectDataSource-Steuerelement für die Übergabe in den GridView-s `SortExpression` Eigenschaft, um die `SelectMethod`. Insbesondere sucht dem ObjectDataSource-Steuerelement für einen Eingabeparameter, dessen Name dem Wert des entspricht, der `SortParameterName` Eigenschaft. Seit der BLL s `GetProductsPagedAndSorted` Methode verfügt über die Eingabeparameter der Sort-Ausdruck mit dem Namen `sortExpression`, legen Sie das "ObjectDataSource"-s `SortExpression` SortExpression Eigenschaft.

Nachdem Sie diese beiden Änderungen vornehmen, sollte die deklarative Syntax des "ObjectDataSource"-s etwa wie folgt aussehen:


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Wie stellen Sie sicher, dass dem ObjectDataSource-Steuerelement festgelegt wurde, mit dem vorherigen Lernprogramm *nicht* SortExpression StartRowIndex oder MaximumRows Eingabeparameter in der SelectParameters-Sammlung enthalten.


Zum Aktivieren der Sortierung in den GridView-Ansicht aktivieren Sie einfach die Kontrollkästchen Sortieren aktivieren im GridView s Smarttag, das GridView-s festgelegt `AllowSorting` Eigenschaft `true` und dadurch den Headertext für jede Spalte als ein LinkButton gerendert werden soll. Klickt der Endbenutzer auf eines der Header LinkButtons, erfolgt ein Postback und ablaufen, müssen die folgenden Schritte aus:

1. Das GridView-Updates der [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) auf den Wert des der `SortExpression` des Felds, dessen Verknüpfung Header geklickt wurde,
2. Dem ObjectDataSource-Steuerelement ruft die BLL s `GetProductsPagedAndSorted` Methode und übergeben das GridView-s `SortExpression` Eigenschaft als Wert für die s-Methode `sortExpression` Eingabeparameter (zusammen mit dem entsprechenden `startRowIndex` und `maximumRows` Werte der Eingabeparameter)
3. Die Geschäftslogikschicht Ruft die DAL s `GetProductsPagedAndSorted` Methode
4. Die DAL führt die `GetProductsPagedAndSorted` gespeicherte Prozedur übergeben, der `@sortExpression` Parameter (zusammen mit den `@startRowIndex` und `@maximumRows` Werte der Eingabeparameter)
5. Die gespeicherte Prozedur gibt die geeignete Teilmenge von Daten an die BLL, das an dem ObjectDataSource-Steuerelement zurückgegeben; Diese Daten werden dann an die GridView gebunden, in HTML gerendert und nach unten für den Endbenutzer gesendet

Abbildung 7 zeigt die erste Seite der Ergebnisse sortiert nach der `UnitPrice` in aufsteigender Reihenfolge.


[![Die Ergebnisse sind nach den Einzelpreis sortiert.](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Abbildung 7**: die Ergebnisse werden nach den Einzelpreis sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-custom-paged-data-vb/_static/image11.png))


Während die Ergebnisse nach Produktname, Kategoriename, Menge pro Einheit und Preis pro Einheit in die aktuelle Implementierung ordnungsgemäß sortieren kann, es wird versucht, zum Sortieren der Ergebnisse der Lieferant die Namen wird zu einer Laufzeitausnahme (siehe Abbildung 8).


![Es wird versucht, zum Sortieren der Ergebnisse von den Lieferanten-Ergebnissen in der folgenden Common Language Runtime-Ausnahme](sorting-custom-paged-data-vb/_static/image12.png)

**Abbildung 8**: Versuch, die zum Sortieren der Ergebnisse von den Lieferanten-Ergebnissen in der folgenden Common Language Runtime-Ausnahme


Diese Ausnahme tritt auf, weil die `SortExpression` der GridView Zuordnungsvorgänge `SupplierName` BoundField nastaven NA hodnotu `SupplierName`. Mit dem Namen des Lieferanten s jedoch in der `Suppliers` Tabelle aufgerufen wird `CompanyName` wir waren mit einem Alias versehen der Name dieser Spalte als `SupplierName`. Allerdings die `OVER` Klausel ein, die die `ROW_NUMBER()` Funktion kann nicht den Alias und den tatsächlichen Spaltennamen verwendet. Ändern Sie daher die `SupplierName` BoundField-s `SortExpression` aus Lieferantenname CompanyName (siehe Abbildung 9). Wie in Abbildung 10 gezeigt, können nach dieser Änderung die Ergebnisse werden vom Lieferanten sortiert.


![Ändern Sie die Lieferantenname BoundField-s SortExpression CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Abbildung 9**: Ändern der Lieferantenname BoundField-s SortExpression CompanyName


[![Die Ergebnisse können jetzt Lieferant sortiert werden](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Abbildung 10**: die Ergebnisse können jetzt sortiert werden vom Lieferanten ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

Die benutzerdefinierte Paging-Implementierung, die untersuchten wir im vorherigen Tutorial erforderlich, dass die Reihenfolge, die mit der wurden die Ergebnisse sortiert werden soll, die zur Entwurfszeit angegeben werden. Kurz gesagt, bedeutete dies, dass die benutzerdefinierte Paging-Implementierung, die wir implementiert nicht zur gleichen Zeit, Sortierfunktionen bereitstellen konnte. In diesem Tutorial handelsanwendung wir diese Einschränkung durch die Erweiterung von der gespeicherten Prozedur aus dem ersten sollen eine `@sortExpression` Eingabeparameter mit dem die Ergebnisse sortiert werden können.

Nach dem Erstellen dieser Prozedur und erstellen neue Methoden in der DAL und BLL gespeichert, es wurden einer GridView-Ansicht zu implementieren, die beide Sortieren von Angeboten und benutzerdefinierte paging durch Konfigurieren der "ObjectDataSource" in den GridView-s aktuelle übergeben `SortExpression` Eigenschaft an die BLL `SelectMethod`.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Carlos Santos. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](efficiently-paging-through-large-amounts-of-data-vb.md)
> [Weiter](creating-a-customized-sorting-user-interface-vb.md)
