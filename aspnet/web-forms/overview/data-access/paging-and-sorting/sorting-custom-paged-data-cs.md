---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Benutzerdefinierte Sortierung von ausgelagerten Daten (c#) | Microsoft Docs
author: rick-anderson
description: Im vorherigen Lernprogramm haben wir gelernt, benutzerdefiniertes Paging implementieren, wenn Daten auf einer Webseite presentating. In diesem Lernprogramm sehen, wie das vorherige erweitern...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a71405bc84304bf7c47f400dfa9886208316d223
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="sorting-custom-paged-data-c"></a>Benutzerdefinierte Sortierung von ausgelagerten Daten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) oder [PDF herunterladen](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Im vorherigen Lernprogramm haben wir gelernt, benutzerdefiniertes Paging implementieren, wenn Daten auf einer Webseite presentating. In diesem Lernprogramm sehen wir das vorige Beispiel Unterstützung für das benutzerdefiniertes Paging sortieren zu erweitern.


## <a name="introduction"></a>Einführung

Im Vergleich zu Standardpaging, benutzerdefiniertes Paging kann die Leistung von Paging durch Daten verbessern, indem Größenordnung, benutzerdefinierte paging die de facto Paging Implementierungsauswahl beim paging durch große Mengen von Daten vornehmen. Implementieren benutzerdefiniertes Paging ist komplizierter als das standardmäßige paging, jedoch vor allem beim Hinzufügen einer Sortierung aus der Mischung implementieren. In diesem Lernprogramm fügen wir das Beispiel aus dem vorherigen Unterstützung für die Sortierung einschließen erweitern *und* benutzerdefiniertes Paging.

> [!NOTE]
> Da dieses Lernprogramm auf der vorherigen Abfrage, aufbaut, vor dem Anfang erkundet So kopieren Sie die deklarative Syntax innerhalb der `<asp:Content>` Element aus dem vorherigen Lernprogramm s-Webseite (`EfficientPaging.aspx`) und fügen Sie ihn zwischen den `<asp:Content>` Element in der `SortParameter.aspx` Seite. Verweisen Sie zurück zu Schritt 1 von der [Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) Lernprogramm für eine ausführlichere Erläuterung zur Replikation von der Funktionalität von einer ASP.NET-Seite in einen anderen.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Schritt 1: Überprüfen das benutzerdefinierte Paging-Verfahren

Für das benutzerdefinierte Paging ordnungsgemäß funktioniert, müssen wir einige Verfahren implementieren, die effizient eine bestimmte Teilmenge der Datensätze, die anhand der Parameter Zeilenindex starten und die maximale Anzahl an Zeilen ziehen können. Es gibt eine Handvoll Techniken, die verwendet werden kann, um dieses Ziel zu erreichen. In dem vorherigen Lernprogramm erläutert, dies mithilfe von Microsoft SQLServer 2005 s neue `ROW_NUMBER()` rangfolgefunktion. Kurz gesagt, der `ROW_NUMBER()` rangfolgefunktion weist Sie eine Zeilennummer an den einzelnen Zeilen zurückgegeben, die von einer Abfrage, die nach einer angegebenen Sortierreihenfolge geordnet ist. Anschließend wird die entsprechenden Teilmenge der Datensätze durch Rückgabe eines bestimmten Bereichs der nummerierten Ergebnisse abgerufen. Die folgende Abfrage veranschaulicht, wie Sie diese Technik zum Zurückgeben dieser Produkte 11 bis 20 nummeriert, wenn nach Rangfolge der Ergebnisse in alphabetischer Reihenfolge sortiert werden. die `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Dieses Verfahren eignet sich insbesondere für Paging mithilfe einer bestimmten Sortierreihenfolge (`ProductName` alphabetisch sortiert, in diesem Fall), aber die Abfrage muss geändert werden, um die Ergebnisse sortiert nach einem anderen Sortierungsausdruck anzuzeigen. Im Idealfall könnte die oben dargestellte Abfrage umgeschrieben werden, um mithilfe eines Parameters in der `OVER` -Klausel wie folgt:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Leider parametrisiert `ORDER BY` Klauseln sind nicht zulässig. Stattdessen müssen wir erstellen, eine gespeicherte Prozedur, die akzeptiert eine `@sortExpression` input-Parameter, aber verwendet eine der folgenden problemumgehungen:

- Schreiben Sie hartcodierte Abfragen für jeden der von Sortierungsausdrücken, die verwendet werden kann; Verwenden Sie dann `IF/ELSE` T-SQL-Anweisungen, um zu bestimmen, welche die auszuführende Abfrage.
- Verwenden einer `CASE` Anweisung dynamische bereitstellen `ORDER BY` Ausdrücke basierend auf der `@sortExpressio` n Eingabeparameter; finden Sie unter der wird verwendet, um dynamisch Sortieren von Abfrageergebnissen Abschnitt in [Power von SQL `CASE` Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Weitere Informationen.
- Erstellen Sie die entsprechende Abfrage als Zeichenfolge in der gespeicherten Prozedur, und verwenden Sie dann [der `sp_executesql` gespeicherte Systemprozedur](https://msdn.microsoft.com/library/ms188001.aspx) zum Ausführen der dynamischen Abfrage.

Jede der folgenden Vorgehensweisen verfügt über einige Nachteile. Die erste Option ist nicht als verwaltbar als die anderen beiden benötigt wird, dass Sie eine Abfrage für jeden möglichen Sortierungsausdruck erstellen. Aus diesem Grund, wenn Sie später entscheiden, um die GridView neue, sortierbar Felder hinzuzufügen auch müssen Sie zurückgehen und Aktualisieren der gespeicherten Prozedur. Der zweite Ansatz weist einige Besonderheiten, die einführen von leistungsbeeinträchtigungen beim Sortieren nach keine Zeichenfolgenmethoden Datenbankspalten und verwaltbarkeit dasselbe wie das erste auch noch dadurch erschwert. Und die dritte Option, dynamische SQL-Anweisungen, die verwendet, bietet das Risiko für einen SQL-Injection-Angriff auf, wenn ein Angreifer die gespeicherte Prozedur übergeben werden, in der Eingabe von eingabeparameterwerten ihrer Wahl ausführen kann.

Während keiner dieser Ansätze perfekt ist, finde ich die dritte Option das beste aus den drei. Mit der Verwendung von dynamischem SQL bietet es sich um ein Maß an Flexibilität, die durch die beiden anderen nicht der Fall ist. Darüber hinaus kann ein SQL-Injection-Angriff nur ausgenutzt werden, wenn ein Angreifer die gespeicherte Prozedur übergeben, die in den Eingabeparametern seiner Wahl ausführen kann. Da die DAL parametrisierte Abfragen verwendet, schützt ADO.NET diese Parameter, die gesendet werden, auf die Datenbank über die Architektur Dies bedeutet, dass die SQL-Injection-Angriff Sicherheitslücke nur vorhanden ist, wenn der Angreifer die gespeicherte Prozedur direkt ausgeführt werden kann.

Um diese Funktionalität zu implementieren, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank mit dem Namen `GetProductsPagedAndSorted`. Diese gespeicherte Prozedur sollte drei Eingabeparameter akzeptieren: `@sortExpression`, Eingabeparameter vom Typ `nvarchar(100`), der angibt, wie die Ergebnisse sortiert werden soll, und wird direkt nach dem eingefügten der `ORDER BY` Text in der `OVER` -Klausel und `@startRowIndex` und `@maximumRows`, die denselben beiden ganzzahligen Eingabeparameter aus der `GetProductsPaged` gespeicherte Prozedur, die in dem vorherigen Lernprogramm untersucht. Erstellen der `GetProductsPagedAndSorted` gespeicherte Prozedur mithilfe des folgenden Skripts:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Die gespeicherte Prozedur startet, indem sichergestellt wird, die einen Wert für die `@sortExpression` Parameter wurde angegeben. Wenn es nicht vorhanden ist, die Ergebnisse werden mit dem höchsten `ProductID`. Als Nächstes wird die dynamische SQL-Abfrage erstellt. Beachten Sie, dass die dynamische SQL-Abfrage hier von den unserer vorherigen Abfragen verwendet geringfügig, um alle Zeilen aus Produkttabelle abzurufen. In vorherigen Beispielen wir jede s zugeordneten Produktkategorie s und Lieferanten s-Namen, die mit einer Unterabfrage abgerufen. Diese Entscheidung wurde getroffen haben, wieder in die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm und erfolgte anstelle `JOIN` s weil TableAdapter zugeordnete einfügen nicht automatisch erstellt aktualisieren und Löschen von Methoden für eine solche Abfragen. Die `GetProductsPagedAndSorted` gespeicherte Prozedur, allerdings muss verwenden `JOIN` s für die Ergebnisse nach Namen der Kategorie oder Lieferanten sortiert werden.

Diese dynamischen Abfrage durch Verketten der statische Abfrage Teile aufgebaut ist und die `@sortExpression`, `@startRowIndex`, und `@maximumRows` Parameter. Da `@startRowIndex` und `@maximumRows` ganzzahligen Parameter, sie müssen in Nvarchars um ordnungsgemäß verkettet werden, konvertiert werden. Sobald diese dynamischen SQL-Abfrage erstellt wurde, wird Sie über ausgeführt `sp_executesql`.

So testen Sie diese gespeicherte Prozedur mit verschiedenen Werten für erkundet die `@sortExpression`, `@startRowIndex`, und `@maximumRows` Parameter. Klicken Sie im Server-Explorer mit der rechten Maustaste auf den Namen der gespeicherten Prozedur, und wählen Sie Execute. Hierdurch wird das Dialogfeld für die gespeicherte Ausführungsprozedur angezeigt, in dem Sie die Eingabeparameter eingeben können (siehe Abbildung 1). Verwenden Sie zum Sortieren der Ergebnisse von den Kategorienamen CategoryName für die `@sortExpression` Parameterwert; nach dem Lieferanten s Firmennamen sortieren, CompanyName verwenden. Klicken Sie nachdem Sie die Parameterwerte haben auf "OK". Die Ergebnisse werden im Ausgabefenster angezeigt. Abbildung 2 zeigt die Ergebnisse, wenn 11 bis 20 Produkte zurückgeben geordnet werden, wenn durch die Reihenfolge der `UnitPrice` in absteigender Reihenfolge.


![Probieren Sie verschiedene Werte für die gespeicherte Prozedur s drei Eingabeparameter](sorting-custom-paged-data-cs/_static/image1.png)

**Abbildung 1**: Probieren Sie verschiedene Werte für die gespeicherte Prozedur s drei Eingabeparameter


[![Die gespeicherte Prozedur s werden die Ergebnisse im Ausgabefenster angezeigt.](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Abbildung 2**: die gespeicherte Prozedur s die Ergebnisse werden im Ausgabefenster angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Wenn die Ergebnisse mit der angegebenen Rangfolge `ORDER BY` Spalte in der `OVER` -Klausel, SQL Server muss die Ergebnisse zu sortieren. Dies ist ein schneller Vorgang, wenn es ein gruppierter Index über die Spalte(n), die die Ergebnisse werden ist nach sortiert wird oder wenn es einem abdeckenden index jedoch andernfalls mehr teuer sein kann. Zum Verbessern der Leistung für ausreichend große Abfragen erwägen Sie einen nicht gruppierten Index für die Spalte mit der die Ergebnisse geordnet sind. Verweisen auf [Rangfolge Funktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) Weitere Details.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Schritt 2: Erweitern der Datenzugriff und Geschäftsschichten-Logik

Mit der `GetProductsPagedAndSorted` gespeicherte Prozedur erstellt, im nächsten Schritt zum Ausführen dieser gespeicherten Prozedur über unsere Anwendungsarchitektur systemverarbeitungsaufwand ist. Dies umfasst sowohl die DAL und BLL eine geeignete Methode hinzugefügt. Lassen Sie s durch Hinzufügen einer Methode zur DAL starten. Öffnen der `Northwind.xsd` typisierte DataSet mit der rechten Maustaste auf die `ProductsTableAdapter`, und wählen Sie aus dem Kontextmenü die Option der Abfrage hinzufügen. Wie wir im vorherigen Lernprogramm möchten wir so konfigurieren Sie diese neue DAL-Methode, um eine vorhandene gespeicherte Prozedur - verwenden `GetProductsPagedAndSorted`, in diesem Fall. Starten Sie an, dass Sie die neue TableAdapter-Methode, um eine vorhandene gespeicherte Prozedur verwenden möchten.


![Wählen Sie eine vorhandene gespeicherte Prozedur verwenden](sorting-custom-paged-data-cs/_static/image5.png)

**Abbildung 3**: Wählen Sie eine vorhandene gespeicherte Prozedur verwenden


Wählen Sie zum Angeben der gespeicherten Prozedur verwendet die `GetProductsPagedAndSorted` gespeicherte Prozedur aus der Dropdown-Liste auf dem nächsten Bildschirm.


![Verwenden Sie die GetProductsPagedAndSorted gespeicherte Prozedur](sorting-custom-paged-data-cs/_static/image6.png)

**Abbildung 4**: Verwenden der GetProductsPagedAndSorted gespeicherte Prozedur


Diese gespeicherte Prozedur gibt eine Gruppe von Datensätzen, seine Ergebnisse also auf dem nächsten Bildschirm geben, dass Tabellendaten zurückgegeben.


![Um anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurückgibt](sorting-custom-paged-data-cs/_static/image7.png)

**Abbildung 5**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt


Erstellen Sie abschließend DAL-Methoden, die beide die Füllung verwenden einer "DataTable" und Zurückgeben einer DataTable-Muster, benennen die Methoden `FillPagedAndSorted` und `GetProductsPagedAndSorted`zugeordnet.


![Wählen Sie die Methodennamen](sorting-custom-paged-data-cs/_static/image8.png)

**Abbildung 6**: Wählen Sie die Methodennamen


Jetzt, dass wir Ve erweiterte DAL, wir re kann jetzt die BLL aktivieren. Öffnen der `ProductsBLL` Klassendatei, und fügen Sie eine neue Methode `GetProductsPagedAndSorted`. Diese Methode muss drei Eingabeparameter akzeptieren `sortExpression`, `startRowIndex`, und `maximumRows` sollte einfach nach unten in der DAL s Aufrufen `GetProductsPagedAndSorted` -Methode, wie folgt:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Schritt 3: Konfigurieren der ObjectDataSource erfolgreich im Parameters SortExpression

HAVING ergänzt, die DAL und BLL erfolgen, die genutzt werden die `GetProductsPagedAndSorted` gespeicherte Prozedur, die alle, die verbleibt, wird "ObjectDataSource" Konfigurieren der `SortParameter.aspx` Seite zur Verwendung der neuen BLL-Methode und übergeben der `SortExpression` Parameter basierend auf der Spalte, die der Benutzer angefordert hat, um die Ergebnisse nach zu sortieren.

Als erstes ändern die s ObjectDataSource `SelectMethod` aus `GetProductsPaged` auf `GetProductsPagedAndSorted`. Dies kann mithilfe des Assistenten konfigurieren von Datenquellen über das Eigenschaftenfenster oder direkt über die deklarative Syntax erfolgen. Wir als Nächstes geben Sie einen Wert für das ObjectDataSource-s [ `SortParameterName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Wenn diese Eigenschaft festgelegt ist, versucht das ObjectDataSource Übergabe in die GridView s `SortExpression` Eigenschaft, um die `SelectMethod`. Insbesondere sucht das ObjectDataSource für einen Eingabeparameter, dessen Name gleich dem Wert von ist, der `SortParameterName` Eigenschaft. Seit der BLL s `GetProductsPagedAndSorted` Methode hat den Expression-Eingabeparameter sortieren mit dem Namen `sortExpression`, legen Sie die s ObjectDataSource `SortExpression` SortExpression Eigenschaft.

Nach diesen beiden Änderungen durchführen, sollte das ObjectDataSource-s-Deklarationssyntax etwa wie folgt aussehen:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Wie stellen Sie sicher, dass das ObjectDataSource verwendet wird, mit dem vorherigen Lernprogramm *nicht* SortExpression, StartRowIndex oder MaximumRows Eingabeparameter in seiner SelectParameters-Auflistung enthalten.


Zum Aktivieren der Sortierung in die GridView einfach das Kontrollkästchen Sortieren aktivieren im GridView s Smarttag, die die GridView s festlegt `AllowSorting` Eigenschaft `true` und verursacht den Headertext für jede Spalte als LinkButton gerendert werden soll. Wenn der Endbenutzer auf einem des Headers LinkButtons klickt, erfolgt ein Postback und ablaufen, müssen die folgenden Schritte aus:

1. Die GridView Updates seine [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) auf den Wert von der `SortExpression` des Felds, dessen Header Link geklickt wurde,
2. Das ObjectDataSource ruft die BLL s `GetProductsPagedAndSorted` -Methode auf und übergibt die GridView-s `SortExpression` Eigenschaft als Wert für die Methode s `sortExpression` input-Parameter (zusammen mit dem entsprechenden `startRowIndex` und `maximumRows` Werte für Eingabeparameter)
3. Die BLL Ruft die DAL s `GetProductsPagedAndSorted` Methode
4. Die DAL führt die `GetProductsPagedAndSorted` gespeicherte Prozedur übergeben, der `@sortExpression` Parameter (zusammen mit der `@startRowIndex` und `@maximumRows` Werte für Eingabeparameter)
5. Die gespeicherte Prozedur gibt die entsprechenden Teilmenge der Daten an die BLL, das an das ObjectDataSource zurückgegeben; Diese Daten ist dann an die GridView gebunden, in HTML gerendert und an den Endbenutzer

Abbildung 7 zeigt die erste Seite der Ergebnisse sortiert nach der `UnitPrice` in aufsteigender Reihenfolge.


[![Die Ergebnisse sind nach den Einzelpreis sortiert.](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Abbildung 7**: die Ergebnisse werden anhand der Einzelpreis sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-custom-paged-data-cs/_static/image11.png))


Während die aktuelle Implementierung ordnungsgemäß sortieren Sie die Ergebnisse nach Produktname, Kategoriename, Menge pro Einheit und Preis pro Einheit kann, versuchen Sie zum Sortieren der Ergebnisse der Lieferant die Name-Ergebnisse in einer Laufzeitausnahme (siehe Abbildung 8).


![Es wird versucht, zum Sortieren der Ergebnisse durch die Ergebnisse der Lieferanten in der folgenden Common Language Runtime-Ausnahme](sorting-custom-paged-data-cs/_static/image12.png)

**Abbildung 8**: Versuch zum Sortieren der Ergebnisse durch die Ergebnisse der Lieferanten in der folgenden Common Language Runtime-Ausnahme


Diese Ausnahme tritt auf, weil die `SortExpression` der GridView s `SupplierName` BoundField festgelegt ist, um `SupplierName`. Mit dem Namen des Lieferanten s jedoch in der `Suppliers` Tabelle aufgerufen wird `CompanyName` wir wurden als Alias der Name dieser Spalte als `SupplierName`. Allerdings die `OVER` -Klausel, indem Sie die `ROW_NUMBER()` Funktion kann nicht den Alias und den tatsächlichen Spaltennamen verwendet. Ändern Sie deshalb die `SupplierName` BoundField-s `SortExpression` aus Lieferantenname CompanyName (siehe Abbildung 9). Wie in Abbildung 10 gezeigt, können nach dieser Änderung die Ergebnisse vom Lieferanten sortiert.


![Lieferantenname BoundField-s SortExpression CompanyName ändern](sorting-custom-paged-data-cs/_static/image13.png)

**Abbildung 9**: Lieferantenname BoundField-s SortExpression CompanyName ändern


[![Die Ergebnisse können jetzt Lieferant sortiert werden](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Abbildung 10**: die Ergebnisse können jetzt sortiert werden vom Lieferanten ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

Das benutzerdefinierte Paging-Implementierung, die wir im vorherigen Lernprogramm untersucht erforderlich, dass die Reihenfolge, wurden die Ergebnisse sortiert werden, zur Entwurfszeit angegeben werden. Kurz gesagt, bedeutet dies, dass das benutzerdefinierte Paging-Implementierung, die wir implementiert keine, zur gleichen Zeit Sortierfunktionen konnte. In diesem Lernprogramm überwand wir diese Einschränkung durch die Erweiterung von der gespeicherten Prozedur aus dem ersten einschließen einer `@sortExpression` Eingabeparameter mit dem die Ergebnisse sortiert werden können.

Nach dem Erstellen dieser Prozedur und erstellen neue Methoden in der DAL und BLL gespeichert, wir können eine GridView implementieren, die beide sortieren angeboten und benutzerdefinierte paging durch Konfigurieren der ObjectDataSource GridView s aktuelle übergeben `SortExpression` Eigenschaft, um die BLL `SelectMethod`.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Carlos Santos. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](efficiently-paging-through-large-amounts-of-data-cs.md)
[Weiter](creating-a-customized-sorting-user-interface-cs.md)
