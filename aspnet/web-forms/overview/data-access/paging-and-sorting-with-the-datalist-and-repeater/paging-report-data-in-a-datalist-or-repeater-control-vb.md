---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Auslagern von Berichtsdaten in einem DataList- oder Wiederholungssteuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Während weder DataList-Steuerelement noch Repeater Angebot automatische Paging und Unterstützung der datenquellensortierung zeigt in diesem Tutorial Hinzufügen von Paging-Unterstützung zu dem DataList- oder Repeater...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b1c3858e232d950d2cf9af0621f60cb9ba4ae767
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835216"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Auslagern von Berichtsdaten in einem DataList- oder Wiederholungssteuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) oder [PDF-Datei herunterladen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Obwohl Sie weder die Repeater das DataList-Angebot, die automatische paging oder das Sortieren unterstützt, die in diesem Tutorial wird gezeigt, wie die DataList- oder Repeater, viel flexibler Paging und Daten anzeigen von Schnittstellen ermöglicht Paging-Unterstützung hinzugefügt wird.


## <a name="introduction"></a>Einführung

Paginierung und Sortierung sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. Z. B. bei der Suche nach ASP.NET Bücher im Onlinebuchhandel gibt es möglicherweise Hunderte von Büchern, aber der Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Name des Autors und So weiter sortiert werden. Wie wir unter den [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-vb.md) Tutorial, das die GridView, DetailsView oder FormView-Steuerelemente bieten integrierte Unterstützung der Paginierung, die auf die Teilstriche eines Kontrollkästchens aktiviert werden kann. GridView umfasst auch das Sortieren unterstützt.

Leider weder DataList-Steuerelement noch Repeater bieten automatische paging und Sortieren unterstützt. In diesem Tutorial betrachten wir, wie die DataList- oder Repeater Paging-Unterstützung hinzugefügt. Wir müssen manuell die Paging-Schnittstelle erstellen, Anzeigen der entsprechenden Seite mit Datensätzen und speichern die Seite, die über Postbacks hinweg aufgerufen wird. Während dies mehr Zeit und Code als mit der GridView, DetailsView oder FormView-Steuerelement übernimmt, ermöglichen dem DataList- und Wiederholungssteuerelement viel flexibler Paging und Daten Anzeige-Schnittstellen.

> [!NOTE]
> Dieses Tutorial konzentriert sich ausschließlich für die Paginierung. Im nächsten Tutorial werden wir uns dem Hinzufügen von Sortierfunktionen aktivieren.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Paging und Sortieren von Tutorial Webseiten

Bevor wir in diesem Tutorial beginnen, können Sie zuerst nehmen einen Moment Zeit, um die ASP.NET-Seiten hinzuzufügen, benötigen wir für dieses Lernprogramm und die nächste s ein. Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `PagingSortingDataListRepeater`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, wenn all diese Einstellungen zu konfigurieren, um die Masterseite verwenden `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Erstellen Sie einen Ordner PagingSortingDataListRepeater, und fügen Sie die Tutorial ASP.NET-Seiten](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Abbildung 1**: Erstellen einer `PagingSortingDataListRepeater` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus der `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in den erstellt die [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Tutorial, listet die Sitemap und zeigt diese Tutorials im aktuellen Abschnitt in einer Liste mit Aufzählungszeichen.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Um die Liste mit Aufzählungszeichen angezeigt, die Paginierung und Sortierung von Tutorials, die wir erstellen, müssen wir diese Siteübersicht hinzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach dem Bearbeiten und löschen, mit dem DataList-Steuerelement Site Map-Knoten-Markup:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten


## <a name="a-review-of-paging"></a>Eine Beschreibung der Auslagerung

In vorherigen Tutorials wurde erläutert, wie durch die Daten in die GridView, DetailsView oder FormView-Steuerelemente der Seite. Diese drei Steuerelemente bieten eine einfache Form der Auslagerung namens *Standardpaging* durch einfaches Aktivieren der Auslagerungsdatei aktivieren-Option in Smarttags des Steuerelements s implementiert werden kann. Mit Standard-auslagerungen, jedes Mal eine Seite mit Daten angefordert wird entweder auf der ersten Seite finden Sie auf oder wenn der Benutzer navigiert zu einer anderen Seite der Daten der GridView, DetailsView oder FormView-Steuerelement erneut anfordert *alle* der Daten aus der "ObjectDataSource". Es Ausschnitten klicken Sie dann, den bestimmten Satz von anzuzeigenden Datensätze anhand der angeforderte Seitenindex und die Anzahl der Datensätze pro Seite angezeigt. Das Standardpaging ausführlich erörtert die [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-vb.md) Tutorial.

Da Standardpaging erneut alle Datensätze für jede Seite anfordert, ist es nicht praktisch, wenn paging durch ausreichend große Mengen von Daten. Angenommen Sie, Paging durch 50.000 Datensätze mit einer Seitengröße von 10. Jedes Mal, wenn der Benutzer zu einer neuen Seite wechselt müssen alle 50.000 Datensätze aus der Datenbank abgerufen werden, auch wenn nur zehn davon angezeigt werden.

*Benutzerdefiniertes Paging* löst die Bedenken hinsichtlich der Leistung von Standardpaging am einfachsten mit nur die genaue Teilmenge der Datensätze, die auf die angeforderte Seite angezeigt werden sollen. Wenn benutzerdefiniertes Paging zu implementieren, müssen wir die SQL-Abfrage schreiben, die effizient den richtigen Satz von Datensätzen zurückgibt. Erläutert, wie zum Erstellen einer solchen Abfrage mithilfe von SQL Server 2005-s neue [ `ROW_NUMBER()` Schlüsselwort](http://www.4guysfromrolla.com/webtech/010406-1.shtml) zurück in die [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) Tutorial.

Um das Standardpaging in den DataList- oder Repeater-Steuerelementen zu implementieren, können wir die [ `PagedDataSource` Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) als Wrapper für die `ProductsDataTable` , deren Inhalt werden ausgelagert wird. Die `PagedDataSource` -Klasse verfügt über eine `DataSource` -Eigenschaft, die jedes aufzählbare Objekt zugewiesen werden kann und [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) und [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) Eigenschaften, die angeben, wie viele Datensätze pro Seite und den Index der aktuellen Seite angezeigt. Nachdem Sie diese Eigenschaften festgelegt wurden, die `PagedDataSource` kann als die Quelle der Daten-Websteuerelement verwendet werden. Die `PagedDataSource`, bei der Enumeration ist, wird nur Zurückgeben der entsprechenden Teilmenge der Datensätze der inneren `DataSource` basierend auf den `PageSize` und `CurrentPageIndex` Eigenschaften. Abbildung 4 zeigt die Funktionalität der `PagedDataSource` Klasse.


![Die PagedDataSource dient als Wrapper für ein aufzählbares Objekt mit einer navigierbaren-Schnittstelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Abbildung 4**: die `PagedDataSource` dient als Wrapper für ein aufzählbares Objekt mit einer navigierbaren-Schnittstelle


Die `PagedDataSource` Objekt kann es sich erstellt und konfiguriert werden, direkt aus der Geschäftslogikebene und an einem DataList- oder Repeater über ein ObjectDataSource-Steuerelement, gebunden oder können erstellt und konfiguriert werden direkt in der CodeBehind-Klasse in ASP.NET Seite "s". Wenn der zweite Ansatz verwendet wird, müssen wir verzichten, mit dem ObjectDataSource-Steuerelement und stattdessen die ausgelagerten Daten an die DataList- oder Repeater programmgesteuert binden.

Die `PagedDataSource` -Objekt verfügt außerdem über Eigenschaften, die benutzerdefinierte Paginierung unterstützen. Wir können jedoch umgehen, verwenden eine `PagedDataSource` für das benutzerdefinierte paging, da wir bereits BLL Methoden, in verfügen der `ProductsBLL` -Klasse für das benutzerdefinierte Paging, die die präzise anzuzeigenden Datensätze zurückgeben.

In diesem Tutorial betrachten wir Standardpaging in einem DataList-Steuerelement implementieren, indem Sie eine neue Methode zum Hinzufügen der `ProductsBLL` -Klasse, die einer ordnungsgemäß konfigurierten gibt `PagedDataSource` Objekt. Im nächsten Tutorial sehen wir, wie Sie benutzerdefiniertes Paging verwendet wird.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Schritt 2: Hinzufügen einer Standard-Paging-Methode in der Geschäftslogikebene

Die `ProductsBLL` -Klasse verfügt derzeit über eine Methode zur Rückgabe aller Produktinformationen `GetProducts()` und eine für die Rückgabe einer bestimmten Teilmenge von Produkten auf einem startIndex `GetProductsPaged(startRowIndex, maximumRows)`. Mit Standard-auslagerungen, steuert die GridView, DetailsView und FormView-Steuerelement alle verwenden die `GetProducts()` Methode, um alle Produkte abzurufen, aber verwenden Sie dann eine `PagedDataSource` intern, um nur die richtige Teilmenge der Datensätze anzuzeigen. Um diese Funktionalität mit dem DataList- und Wiederholungssteuerelement-Steuerelementen zu replizieren, können wir eine neue Methode in der BLL erstellen, die dieses Verhalten imitiert.

Fügen Sie eine Methode, die `ProductsBLL` Klasse mit dem Namen `GetProductsAsPagedDataSource` akzeptiert, die zwei ganzzahlige Eingabeparameter entgegen:

- `pageIndex` der Index der Seite, um anzuzeigen, auf 0 (null), indiziert und
- `pageSize` die Anzahl der Datensätze pro Seite angezeigt werden soll.

`GetProductsAsPagedDataSource` beginnt mit dem Abrufen von *alle* Datensätze aus `GetProducts()`. Er erstellt dann eine `PagedDataSource` Objekt, und legen dessen `CurrentPageIndex` und `PageSize` Eigenschaften mit den Werten des übergebenen `pageIndex` und `pageSize` Parameter. Die Methode wird abgeschlossen, indem Sie diese Konfiguration zurückgeben `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Schritt 3: Anzeigen von Produktinformationen in einem DataList-Steuerelement verwenden standardmäßig Paging

Mit der `GetProductsAsPagedDataSource` Methode hinzugefügt, die `ProductsBLL` -Klasse kann jetzt erstellen wir ein DataList- oder Repeater, das das Standardpaging bereitstellt. Öffnen Sie zunächst die `Paging.aspx` auf der Seite die `PagingSortingDataListRepeater` Ordner, und ziehen Sie einem DataList-Steuerelement aus der Toolbox auf den Designer, Festlegen der DataList s `ID` Eigenschaft `ProductsDefaultPaging`. Erstellen Sie eine neue, mit dem Namen "ObjectDataSource" aus dem DataList-s-Smarttag `ProductsDefaultPagingDataSource` er so konfiguriert, dass ihm Daten mithilfe abgerufenen der `GetProductsAsPagedDataSource` Methode.


[![Erstellen Sie ein ObjectDataSource-Steuerelement und konfigurieren Sie, um die GetProductsAsPagedDataSource ()-Methode zu verwenden](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Abbildung 5**: ein ObjectDataSource-Steuerelement zu erstellen und konfigurieren Sie sie so verwenden die `GetProductsAsPagedDataSource` `()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Legen Sie die Dropdownlisten in der Update-, INSERT-, und löschen Sie die Registerkarten auf (keine).


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Abbildung 6**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Da die `GetProductsAsPagedDataSource` Methode erwartet zwei Parameter, die der Assistent fordert uns für die Quelle des dieser Parameterwerte angegeben.

Der Seitenindex und Seitengrößenwert müssen postbackübergreifend gespeichert werden. Sie können im Ansichtszustand gespeichert werden, in der Abfragezeichenfolge beibehalten, in der Sitzungsvariablen gespeichert oder gespeichert wird, verwenden ein anderes Verfahren. In diesem Tutorial verwenden wir die Abfragezeichenfolge, die hat den Vorteil, dass eine bestimmte Seite der Daten, die mit einem Lesezeichen versehen werden.

Insbesondere verwenden das Querystring-Felder PageIndex und die Seitengröße für die `pageIndex` und `pageSize` Parameter bzw. (siehe Abbildung 7). Nehmen Sie einen Moment Zeit, um die Standardwerte für diese Parameter können festgelegt, wie die Querystring-Werte, die gewonnen haben t vorhanden sein, wenn ein Benutzer zunächst auf dieser Seite besucht. Für `pageIndex`, standardmäßig ist der Wert auf 0 festgelegt (was die erste Seite der Daten angezeigt werden) und `pageSize` s Standardwert 4.


[![Verwenden Sie für die PageIndex und PageSize-Parameter der Abfragezeichenfolge als Quelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Abbildung 7**: Verwenden Sie die Abfragezeichenfolge, als Quelle für die `pageIndex` und `pageSize` Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Nach der Konfiguration dem ObjectDataSource-Steuerelement, Visual Studio erstellt automatisch eine `ItemTemplate` für DataList-Steuerelement. Anpassen der `ItemTemplate` , damit nur die-s-Produktname, Kategorie und Lieferanten angezeigt werden. Außerdem legen Sie die DataList s `RepeatColumns` Eigenschaft auf 2 die `Width` auf 100 % und die zugehörige `ItemStyle` s `Width` auf 50 %. Diese Breite Einstellungen bieten gleichen Abstand der zwei Spalten.

Nach diesen Änderungen sollte das DataList-Steuerelement und "ObjectDataSource"-s-Markup etwa wie folgt aussehen:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Da wir kein Update durchführen oder Funktionen in diesem Tutorial löschen möchten, können Sie den Ansichtszustand des DataList-s zur Reduzierung der Größe der gerenderten Seite deaktivieren.


Wenn zunächst diese Seite über einen Browser, auch Zugriff auf die `pageIndex` noch `pageSize` Querystring-Parameter angegeben werden. Daher werden die Standardwerte 0 und 4 verwendet. Wie in Abbildung 8 gezeigt, führt dies in einem DataList-Steuerelement, das die ersten vier Produkte anzeigt.


[![Es werden die ersten vier Produkte aufgeführt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Abbildung 8**: die ersten vier Produkte aufgeführt sind ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Ohne bedeutet, eine Schnittstelle für Paging, dort s, die derzeit nicht einfach für einen Benutzer dass, zu der zweiten Seite der Daten zu navigieren. Wir erstellen eine Paging-Schnittstelle in Schritt 4. Jetzt kann jedoch Paging nur durch erfolgen direkt die Paging-Kriterien angeben, in der Abfragezeichenfolge. Zum Anzeigen der zweiten Seite ändern Sie z. B. die URL in die Browseradressleiste s aus `Paging.aspx` zu `Paging.aspx?pageIndex=2` , und drücken Sie die EINGABETASTE. Dies bewirkt, dass die zweite Seite der Daten angezeigt werden (siehe Abbildung 9).


[![Die zweite Seite der Daten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Abbildung 9**: der zweiten Seite der Daten wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Schritt 4: Erstellen der Paging-Schnittstelle

Es gibt eine Vielzahl von anderen Paging-Schnittstellen, die implementiert werden kann. Die GridView, DetailsView oder FormView-Steuerelemente bieten vier verschiedene Schnittstellen zum auswählen:

- **Nächsten, vorherigen** Benutzer können eine Seite zu einem Zeitpunkt, zu den nächsten oder vorherigen einer verschieben.
- **Nächsten, vorherigen, ersten, letzten** zusätzlich zu die Schaltflächen Weiter und zurück, diese Schnittstelle, die erste und letzte Schaltflächen für das Verschieben in der ersten oder letzten Seite umfasst.
- **Numerische** Listet die Seitennummern in den Paging-Schnittstelle, sodass Benutzer schnell zu einer bestimmten Seite zu wechseln.
- **Numerisch, ersten, letzten** zusätzlich zu den numerischen Seitenzahlen, enthält Sie Schaltflächen für das Verschieben in der ersten oder letzten Seite.

Dem DataList- und Wiederholungssteuerelement sind wir dafür verantwortlich, für die Entscheidung über eine Paging-Schnittstelle und implementieren. Dies umfasst das Erstellen der erforderlichen Web-Steuerelemente auf der Seite und die angeforderte Seite angezeigt, wenn eine bestimmte Paging-Schnittstelle Schaltfläche geklickt wird. Darüber hinaus müssen bestimmte Steuerelemente der Benutzeroberfläche Paging deaktiviert werden soll. Beispielsweise wird beim Anzeigen der ersten Seite der Daten, die mit der nächsten zurück, ersten, letzten Schnittstelle, würde der erste und die zurück-Schaltflächen deaktiviert werden.

In diesem Tutorial können Verwendung nächsten zurück, zunächst zuletzt Schnittstelle. Hinzufügen von vier Websteuerelemente für die Schaltfläche zur Seite, und legen Sie deren `ID` s, um `FirstPage`, `PrevPage`, `NextPage`, und `LastPage`. Legen Sie die `Text` Eigenschaften &lt; &lt; zuerst &lt; zurück, die nächste &gt;, und das letzte &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Als Nächstes erstellen Sie eine `Click` -Ereignishandler für jede dieser fünf Schaltflächen. In Kürze fügen wir den erforderlichen Code für die angeforderte Seite nicht anzeigen.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Speichern Sie die Gesamtzahl der Datensätze, die ausgelagerte über

Unabhängig von der Paging-Schnittstelle ausgewählt haben müssen wir berechnen und speichern die Gesamtzahl der Datensätze, die über ausgelagert wird. Die gesamte Zeilenanzahl (in Verbindung mit der Seitengröße) bestimmt, wie viele Seiten mit Daten, die insgesamt über ausgelagert werden die bestimmt, welche Paging von Benutzeroberflächen-Steuerelemente hinzugefügt werden oder aktiviert sind. Im weiter, vorherigen, ersten wird die letzte Schnittstelle, die wir erstellen, Anzahl der Seiten auf zwei Arten verwendet:

- Um zu bestimmen, ob wir in diesem Fall sind die Schaltflächen Weiter und letzten deaktiviert die letzte Seite anzeigen.
- Wenn der Benutzer die letzte Schaltfläche klickt, die wir sie mit dem letzten weiter müssen, zählen Seite, deren Index eine ist kleiner als die Seite.

Anzahl der Seiten wird berechnet, wie die Obergrenze für die Gesamtzeilenanzahl durch die Seitengröße geteilt. Wenn wir die paging durch die vier Datensätze pro Seite 79 Datensätze sind, klicken Sie dann die Seitenanzahl ist beispielsweise 20 (die Obergrenze von 79 / 4). Wenn wir die numerischen Paging-Schnittstelle,, diese Informationen werden Sie darüber informiert, wie viele numerische Seitenschaltflächen verwenden werden angezeigt. Wenn unsere Auslagerungsschnittstelle weiter oder letzte Schaltflächen enthält, wird die Seitenanzahl zum Ermitteln des Zeitpunkts, deaktivieren Sie die Schaltflächen Weiter oder letzte verwendet.

Wenn die Paging-Schnittstelle eine letzte Schaltfläche enthält, ist es zwingend erforderlich, dass die Gesamtzahl der Datensätze, die über ausgelagerte über Postbacks hinweg gespeichert werden, wenn die letzte Schaltfläche geklickt wird können wir den Index der letzten Seite bestimmen. Um dies zu ermöglichen, erstellen Sie eine `TotalRowCount` Eigenschaft in der ASP.NET Seite s Code-Behind-Klasse, die den Wert Ansichtszustand speichert:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Zusätzlich zu `TotalRowCount`kurz zum Erstellen von schreibgeschützten auf Seitenebene-Eigenschaften für den Zugriff auf einfache Weise den Seitenindex, die Seitengröße und Seitenanzahl:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Bestimmen die Gesamtzahl der Datensätze, die ausgelagerte über

Die `PagedDataSource` Objekt zurückgegeben wird, von der "ObjectDataSource"-s `Select()` Methode hat darin *alle* der Datensätze, obwohl nur eine Teilmenge davon im DataList-Steuerelement angezeigt werden. Die `PagedDataSource` s [ `Count` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) gibt nur die Anzahl der Elemente, die im DataList-Steuerelement; angezeigt wird der [ `DataSourceCount` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) gibt die Gesamtzahl der Elemente in der `PagedDataSource`. Aus diesem Grund müssen wir die ASP.NET-Seite s zuweisen `TotalRowCount` Eigenschaft ist der Wert von der `PagedDataSource` s `DataSourceCount` Eigenschaft.

Um dies zu erreichen, erstellen Sie einen Ereignishandler für das "ObjectDataSource"-s `Selected` Ereignis. In der `Selected` Ereignishandler haben Zugriff auf den Rückgabewert der "ObjectDataSource" Zuordnungsvorgänge `Select()` -Methode in diesem Fall die `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Die angeforderte Seite der Daten anzeigen

Klickt der Benutzer eine der Schaltflächen in den Paging-Schnittstelle, müssen wir die angeforderte Seite der Daten anzuzeigen. Da die Paging-Parameter über die Abfragezeichenfolge angegeben werden, um die angeforderte Seite der Datennutzung anzuzeigen `Response.Redirect(url)` haben die Benutzer s Browser erneut anfordern der `Paging.aspx` Seite mit den entsprechenden Parametern für die Auslagerung. Klicken wir z. B. um der zweiten Seite der Daten anzuzeigen, leiten die Benutzer `Paging.aspx?pageIndex=1`.

Um dies zu ermöglichen, erstellen Sie eine `RedirectUser(sendUserToPageIndex)` -Methode, die den Benutzer leitet `Paging.aspx?pageIndex=sendUserToPageIndex`. Rufen Sie dann die Schaltfläche mit den vier dieser Methode `Click` -Ereignishandler. In der `FirstPage` `Click` -Ereignishandler, rufen `RedirectUser(0)`, um sie zu senden, auf die erste Seite; in der `PrevPage` `Click` -Ereignishandler `PageIndex - 1` als den Seitenindex; und so weiter.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Mit der `Click` Ereignishandler abgeschlossen ist, die DataList-s-Datensätze über ausgelagert werden können, indem Sie auf die Schaltflächen. Nehmen Sie einen Moment Zeit, zu testen!

## <a name="disabling-paging-interface-controls"></a>Durch Deaktivieren des Paging von Benutzeroberflächen-Steuerelemente

Derzeit sind alle vier Schaltflächen, unabhängig von der angezeigten Seite aktiviert. Allerdings möchten wir die erste ' und ' Vorheriger Schaltflächen zu deaktivieren, wenn es sich bei die erste Seite der Daten und die Schaltflächen Weiter und zuletzt angezeigt, wenn die letzte Seite angezeigt. Die `PagedDataSource` "ObjectDataSource" s zurückgegebenes Objekt `Select()` Methode verfügt über Eigenschaften [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) und [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , die wir untersuchen können, um festzustellen, ob es angezeigt wird die ersten oder letzten Seite der Daten.

Fügen Sie die folgenden in das "ObjectDataSource"-s `Selected` -Ereignishandler:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Mit folgender Ergänzung werden die erste ' und ' Vorheriger Schaltflächen deaktiviert beim Anzeigen der ersten Seite, während die Schaltflächen Weiter und letzten beim Anzeigen der letzten Seite deaktiviert werden.

Let s abzuschließen, die Paging-Schnittstelle durch den Benutzer darüber informiert welche Seite sie re gerade angezeigt und wie viele Gesamtanzahl der Seiten vorhanden sind. Die Seite ein Label-Steuerelement hinzu, und legen Sie dessen `ID` Eigenschaft `CurrentPageNumber`. Legen Sie dessen `Text` Eigenschaft im "ObjectDataSource" s ausgewählte Ereignishandler solche, enthalten die aktuelle Seite angezeigt wird (`PageIndex + 1`) und die Gesamtzahl der Seiten (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Abbildung 10 zeigt `Paging.aspx` beim ersten Mal besucht hat. Da die Abfragezeichenfolge leer ist, erhält standardmäßig den DataList-Steuerelement die ersten vier Produkte angezeigt. die erste ' und ' Vorheriger Schaltflächen sind deaktiviert. Klicken Sie auf Weiter, werden die nächsten vier Datensätze (siehe Abbildung 11) angezeigt; die erste ' und ' Vorheriger Schaltflächen sind nun aktiviert.


[![Die erste Seite der Daten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Abbildung 10**: die erste Seite der Daten wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Die zweite Seite der Daten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Abbildung 11**: der zweiten Seite der Daten wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> Die Paging-Schnittstelle kann weiter verbessert werden, indem der Benutzer angeben, wie viele Seiten pro Seite anzeigen kann. Beispielsweise konnte einem DropDownList-Steuerelement die Liste Größenoptionen wie 5, 10, 25, 50 und alle hinzugefügt werden. Nach dem Auswählen einer Seitengröße, muss der Benutzer wird an die zurückgeleitet werden `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Ich verlasse, implementieren diese Erweiterung für den Leser als Übung.


## <a name="using-custom-paging"></a>Verwendung von benutzerdefiniertem Paging

Die zugehörigen Daten mithilfe der ineffizienten standardmäßig Paging Technik DataList-Seiten. Beim paging durch ausreichend große Mengen von Daten, ist es zwingend erforderlich, dass das benutzerdefinierte Paging verwendet werden. Obwohl die Details der Implementierung geringfügig unterscheiden, sind die Konzepte für die Implementierung von benutzerdefinierten Paging in einem DataList-Steuerelement stimmt mit dem das Standardpaging an. Verwendung von benutzerdefiniertem Paging die `ProductBLL` s-Klasse `GetProductsPaged` Methode (anstelle von `GetProductsAsPagedDataSource`). Siehe die [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) Tutorial `GetProductsPaged` muss der Start Zeile Index und die maximale Anzahl der zurückzugebenden Zeilen übergeben werden. Diese Parameter können verwaltet werden, über die Abfragezeichenfolge wie die `pageIndex` und `pageSize` verwendeten Parameter in der Auslagerungsdatei.

Da es s keine `PagedDataSource` mit benutzerdefiniertem Paging, alternative Techniken müssen verwendet werden, um zu bestimmen, die Gesamtzahl der Datensätze, die ausgelagerte durch und gibt an, ob wir erneut die erste oder letzte Seite der Daten anzeigen. Die `TotalNumberOfProducts()` -Methode in der die `ProductsBLL` Klasse gibt die Gesamtanzahl der Produkte, die über ausgelagert wird. Um festzustellen, ob die erste Seite der Daten angezeigt wird, Überprüfen der Startindex für die Zeile ist dies 0 (null), und klicken Sie dann die erste Seite angezeigt wird. Die letzte Seite wird angezeigt wird, wenn der Zeilenindex für den Start sowie die Max. Anzahl zurückzugebender Zeilen ist größer als oder gleich der Gesamtzahl der Datensätze, die ausgelagerte über.

Wir werden untersuchen, Implementieren von benutzerdefiniertem Paging ausführlicher im nächsten Tutorial.

## <a name="summary"></a>Zusammenfassung

Während weder die Repeater das DataList-Steuerelement bietet die Out-of pagingunterstützung finden Sie in der GridView, DetailsView und FormView-Steuerelemente, mit minimalem Aufwand eine solche Funktionalität hinzugefügt werden kann. Die einfachste Möglichkeit zum Implementieren von Standardpaging besteht darin, den gesamten Satz von Produkten in eine `PagedDataSource` und binden Sie dann die `PagedDataSource` zur DataList oder Repeater. In diesem Tutorial wir hinzugefügt, die `GetProductsAsPagedDataSource` Methode, um die `ProductsBLL` Klasse die Rückgabe der `PagedDataSource`. Die `ProductsBLL` Klasse enthält bereits die Methoden, die erforderlich sind, für das benutzerdefinierte Paging `GetProductsPaged` und `TotalNumberOfProducts`.

Sowie das Abrufen von entweder der genaue Satz von Datensätzen, die für benutzerdefiniertes Paging angezeigt werden soll oder alle Datensätze in einem `PagedDataSource` für standardmäßige auslagerungen, wir müssen auch die Paging-Schnittstelle manuell hinzufügen. In diesem Tutorial erstellten eine weiter, die zurück zuletzt eine Verbindung mit vier Schaltfläche Websteuerelemente. Darüber hinaus wurde ein Label-Steuerelement anzeigen, die aktuelle Seitenzahl und die Gesamtzahl der Seiten hinzugefügt.

Im nächsten Tutorial sehen wir, wie dem DataList- und Wiederholungssteuerelement sortierungsunterstützung hinzugefügt werden. Wir sehen auch einem DataList-Steuerelement zu erstellen, die können sowohl seitenweise durchlaufen und sortiert werden sollen (einschließlich Beispielen, die standardmäßig mit benutzerdefiniertem Paging).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Liz Shulok, Ken Pespisa und Bernadette Leigh. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Weiter](sorting-data-in-a-datalist-or-repeater-control-vb.md)
