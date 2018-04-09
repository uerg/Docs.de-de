---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paging von Berichtsdaten in einem DataList oder Wiederholungsmodul-Steuerelement (VB) | Microsoft Docs
author: rick-anderson
description: Beim weder DataList noch Repeater Angebot automatische Paging oder sortierungsunterstützung veranschaulicht dieses Lernprogramms zum Hinzufügen von Unterstützung der Paginierung zur DataList oder Repeater...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 867f2a0a6de6da2ccda1526ef7c1d0edd97431c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Paging von Berichtsdaten in einem DataList oder Wiederholungsmodul-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) oder [PDF herunterladen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Während weder DataList noch Repeater Angebot automatische paging und sortieren die Unterstützung dieses Lernprogramm zeigt, wie Hinzufügen von Unterstützung der Paginierung zum DataList oder Repeater, viel flexibler Paging und Daten anzeigen von Schnittstellen ermöglicht.


## <a name="introduction"></a>Einführung

Paging und Sortieren von sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. Beispielsweise bei der Suche nach ASP.NET Bücher Onlinebuchhandel gibt es möglicherweise Hunderte von Büchern, aber der Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Name des Autors und So weiter sortiert werden. Wie in erläutert die [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-vb.md) Lernprogramm, das GridView, DetailsView und FormView-Steuerelemente, die alle bieten integrierte Unterstützung von Paging, die bei der Takt des ein Kontrollkästchen aktiviert werden kann. GridView umfasst auch das Sortieren unterstützt.

Leider weder DataList noch Repeater bieten automatische paging und Sortieren unterstützt. In diesem Lernprogramm untersuchen wir wie das DataList oder Repeater pagingunterstützung hinzugefügt. Wir müssen manuell die Auslagerungsschnittstelle erstellen, Anzeigen der entsprechenden Seite mit Datensätzen und beachten Sie die Seite, der über Postbacks aufgerufen wird. Während dies mehr Zeit und Code als mit GridView, DetailsView oder FormView akzeptiert, ermöglichen dem DataList und Repeater viel flexibler Paginierung und Daten anzeigen-Schnittstellen.

> [!NOTE]
> Dieses Lernprogramm konzentriert sich ausschließlich auf Paging. In den nächsten Lernprogrammen werden wir uns dem Hinzufügen von Sortierfunktionen aktivieren.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Paging und Sortieren von Tutorial Webseiten

Bevor wir in diesem Lernprogramm beginnen, können Sie s, schalten Sie zuerst einen Moment Zeit, die ASP.NET-Seiten hinzufügen, die wir für dieses Lernprogramm und die nächste benötigen. Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `PagingSortingDataListRepeater`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, dass alle von ihnen für die Verwendung die Masterseite konfiguriert `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Erstellen Sie einen Ordner PagingSortingDataListRepeater, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Abbildung 1**: Erstellen einer `PagingSortingDataListRepeater` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus den `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in erstellt die [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm, zählt die Siteübersicht und diese Lernprogramme im aktuellen Abschnitt in einer Aufzählung angezeigt.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Damit haben die Liste mit Aufzählungszeichen angezeigt, das Paging und sortieren die Lernprogramme, die wir erstellen, müssen wir diese Siteübersicht hinzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach dem Bearbeiten und löschen, mit dem DataList Standort Zuordnung Knoten Markup:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten


## <a name="a-review-of-paging"></a>Eine Überprüfung des Paging

In vorherigen Lernprogrammen wurde erläutert, wie die Daten in den Steuerelementen GridView, DetailsView und FormView durchblättern. Diese drei Steuerelemente bieten eine einfache Form der Auslagerung aufgerufen *Standardnavigation* , kann implementiert werden, indem Sie einfach die Paging aktivieren Option im Steuerelement s Smarttag. Mit Standardnavigation, jedes Mal eine Seite mit Daten angefordert wird entweder auf der ersten Seite aufrufen oder wenn der Benutzer navigiert zu einer anderen Seite der Daten die GridView, DetailsView, oder FormView Steuerelement erneut anfordert *alle* der Daten aus der ObjectDataSource. Es Ausschnitten dann aus der bestimmten Gruppe von Datensätzen angezeigt anhand der angeforderte Seitenindex und die Anzahl der Datensätze pro Seite angezeigt werden sollen. Standardnavigation ausführlich erörtert die [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-vb.md) Lernprogramm.

Da die Standardnavigation erneut alle Datensätze für jede Seite anfordert, ist es nicht ratsam, wenn paging durch ausreichend große Mengen von Daten. Nehmen Sie z. B., Paging über 50.000 Datensätze mit einer Seitengröße von 10. Jedes Mal, wenn der Benutzer auf eine neue Seite verschoben müssen alle 50.000 Datensätze aus der Datenbank abgerufen werden, auch wenn nur zehn davon angezeigt werden.

*Benutzerdefiniertes Paging* löst die Leistung Bedenken hinsichtlich der Standardnavigation grabbing nur die präzise Teilmenge von Datensätzen auf die angeforderte Seite angezeigt werden sollen. Wenn benutzerdefiniertes Paging zu implementieren, müssen wir die SQL-Abfrage schreiben, die effizient einfach der richtigen Gruppe von Datensätzen zurückgibt. Wurde erläutert, wie zum Erstellen einer Abfrage, die von mit der neuen SQL Server 2005 s [ `ROW_NUMBER()` Schlüsselwort](http://www.4guysfromrolla.com/webtech/010406-1.shtml) zurück in die [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) Lernprogramm.

Um Standardnavigation in den Steuerelementen DataList oder Repeater zu implementieren, können wir die [ `PagedDataSource` Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) als Wrapper um die `ProductsDataTable` , deren Inhalt werden ausgelagert wird. Die `PagedDataSource` -Klasse verfügt über eine `DataSource` -Eigenschaft, die auf ein beliebiges aufzählbare Objekt zugewiesen werden kann und [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) und [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) Eigenschaften, die angeben, wie viele Datensätze pro Seite und den Index der aktuellen Seite anzeigen. Sobald diese Eigenschaften festgelegt wurden, die `PagedDataSource` als die Quelle der Daten-Websteuerelement verwendet werden können. Die `PagedDataSource`, wenn aufgelistet, wird nur Rückgabewert der entsprechenden Teilmenge der Datensätze, die die inneren `DataSource` basierend auf den `PageSize` und `CurrentPageIndex` Eigenschaften. Abbildung 4 zeigt die Funktionalität der `PagedDataSource` Klasse.


![Die PagedDataSource umschließt ein aufzählbares Objekt mit einer auslagerbaren-Schnittstelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Abbildung 4**: die `PagedDataSource` dient als Wrapper für ein aufzählbares Objekt mit einer auslagerbaren-Schnittstelle


Die `PagedDataSource` Objekt kann erstellt und konfiguriert direkt der Geschäftslogikebene und einem DataList oder Repeater über ein ObjectDataSource gebunden oder können erstellt und konfiguriert werden direkt in der ASP.NET Seite "s" Code-Behind-Klasse. Wenn der zweite Ansatz verwendet wird, müssen wir mithilfe der ObjectDataSource verzichten und stattdessen die ausgelagerten Daten an das DataList oder Repeater programmgesteuert binden.

Die `PagedDataSource` Objekt verfügt auch über Eigenschaften, die benutzerdefinierte Paginierung unterstützt. Wir können jedoch umgehen, verwenden eine `PagedDataSource` für das benutzerdefinierte paging, da es bereits BLL Methoden, in verfügen der `ProductsBLL` -Klasse für das benutzerdefinierte Paging, die zum Anzeigen der präzise Datensätze zurück.

In diesem Lernprogramm lernen wir Standardnavigation in einem DataList implementieren, indem Sie eine neue Methode zum Hinzufügen der `ProductsBLL` Klasse, die einem ordnungsgemäß konfigurierten zurückgibt `PagedDataSource` Objekt. In den nächsten Lernprogrammen sehen wir, wie Sie benutzerdefiniertes Paging.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Schritt 2: Hinzufügen einer Standard-Paging-Methode in der Geschäftslogikebene

Die `ProductsBLL` -Klasse verfügt derzeit über eine Methode zum Zurückgeben von Produktinformationen für alle `GetProducts()` und eine für eine bestimmte Teilmenge von Produkten an einem startIndex zurückgeben `GetProductsPaged(startRowIndex, maximumRows)`. Bei der Standardnavigation, steuert die GridView, DetailsView und FormView alle Verwendung der `GetProducts()` Methode, um alle Produkte abgerufen, jedoch sollten Sie eine `PagedDataSource` intern, um nur die richtige Teilmenge der Datensätze anzuzeigen. Um diese Funktionalität mit den verschiedenen Steuerelementen zu replizieren, können wir eine neue Methode in der BLL erstellen, die dieses Verhalten imitiert.

Hinzufügen einer Methode zur der `ProductsBLL` Klasse mit dem Namen `GetProductsAsPagedDataSource` akzeptiert, die zwei ganzzahlige Eingabeparameter entgegen:

- `pageIndex` der Index der Seite, um anzuzeigen, auf 0 (null), indiziert und
- `pageSize` die Anzahl der Datensätze pro Seite angezeigt werden sollen.

`GetProductsAsPagedDataSource` beginnt mit dem Abrufen von *alle* Datensätze aus `GetProducts()`. Sie erstellt dann eine `PagedDataSource` Objekt, und legen seine `CurrentPageIndex` und `PageSize` Eigenschaften mit den Werten der übergebenen `pageIndex` und `pageSize` Parameter. Die Methode abgeschlossen ist, wird durch diese Konfiguration zurückgeben `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Schritt 3: Anzeigen von Produktinformationen in einer DataList Standardnavigation verwenden

Mit der `GetProductsAsPagedDataSource` Methode hinzugefügt, um die `ProductsBLL` -Klasse, wir können jetzt einen erstellen DataList oder Repeater, die Standardnavigation bereitstellt. Öffnen Sie zunächst die `Paging.aspx` auf der Seite der `PagingSortingDataListRepeater` Ordner, und ziehen Sie eine DataList aus der Toolbox in den Designer, Festlegen der DataList s `ID` Eigenschaft `ProductsDefaultPaging`. Aus dem Smarttag DataList s, erstellen Sie eine neue ObjectDataSource mit dem Namen `ProductsDefaultPagingDataSource` und ihn so konfigurieren, dass er Daten mithilfe ruft der `GetProductsAsPagedDataSource` Methode.


[![Erstellen Sie ein ObjectDataSource und konfigurieren Sie, um die GetProductsAsPagedDataSource ()-Methode zu verwenden](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Abbildung 5**: ein ObjectDataSource erstellen und konfigurieren Sie sie verwenden die `GetProductsAsPagedDataSource` `()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine).


[![Legen Sie das Dropdown-Listen in der Update-, INSERT- und Löschen von Registerkarten (keine)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Abbildung 6**: Legen Sie das Dropdown-Listen in der Update-, INSERT- und Löschen von Registerkarten (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Da die `GetProductsAsPagedDataSource` Methode erwartet zwei Eingabeparameter, die der Assistent fordert Sie uns für die Quelle des diese Parameterwerte.

Der Seitenindex und Seitengrößenwert müssen über Postbacks gespeichert. Sie können im Ansichtszustand gespeichert werden, in die Abfragezeichenfolge permanent gespeichert, in Sitzungsvariablen gespeichert oder gespeichert wurde, verwenden ein anderes Verfahren. Für dieses Lernprogramm verwenden wir die Abfragezeichenfolge besitzt den Vorteil, dass eine bestimmte Seite der Daten mit einem Lesezeichen versehen werden.

Verwenden Sie insbesondere den Querystring-Felder PageIndex und PageSize für den `pageIndex` und `pageSize` Parameter bzw. (siehe Abbildung 7). Nehmen Sie einen Moment Zeit, um die Standardwerte für diese Parameter festgelegt, wie die/t gewonnen Querystring-Werte vorhanden sein, wenn ein Benutzer zunächst auf dieser Seite besucht. Für `pageIndex`, legen Sie den Standardwert auf 0 (d. h. die erste Seite der Daten angezeigt werden) und `pageSize` s Standardwert 4.


[![Verwenden Sie die Abfragezeichenfolge als Quelle für die PageIndex und PageSize-Parameter](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Abbildung 7**: Verwenden Sie die Abfragezeichenfolge als Quelle für die `pageIndex` und `pageSize` Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Nach dem Konfigurieren der ObjectDataSource, erstellt Visual Studio automatisch eine `ItemTemplate` für DataList. Anpassen der `ItemTemplate` , sodass nur der Produktname s, Kategorie und Lieferanten angezeigt werden. Legen Sie auch die DataList s `RepeatColumns` -Eigenschaft auf 2, seine `Width` 100 % und die zugehörige `ItemStyle` s `Width` auf 50 %. Diese Einstellungen Breite gebe gleich Abstand der zwei Spalten.

Nachdem diese Änderungen vorgenommen wurden, sollte das DataList und ObjectDataSource s Markup etwa wie folgt aussehen:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Da wir ein Update nicht ausführen oder Löschen von Funktionen in diesem Lernprogramm, wird möglicherweise das DataList-s-Ansichtszustand zum Reduzieren der Größe der gerenderten Seite deaktiviert.


Beim anfänglich keins dieser Seite über einen Browser Zugriff auf die `pageIndex` noch `pageSize` Querystring-Parameter angegeben werden. Daher werden die Standardwerte 0 und 4 verwendet. Wie in Abbildung 8 gezeigt, führt dies in einem DataList, in dem die ersten vier Produkte angezeigt.


[![Die ersten vier Produkte aufgeführt sind](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Abbildung 8**: die ersten vier Produkte aufgeführt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Ohne eine Auslagerungsdatei-Schnittstelle, dort s, die derzeit nicht einfach für einen Benutzer Navigieren auf der zweiten Seite des Data bedeutet. Wir erstellen eine Auslagerungsdatei-Schnittstelle in Schritt 4. Jetzt kann jedoch Paging nur erfolgen durch direktes Angeben der Kriterien Paging in der Abfragezeichenfolge. Beispielsweise um die zweite Seite anzeigen, ändern Sie die URL in die Adressleiste des Browsers s aus `Paging.aspx` auf `Paging.aspx?pageIndex=2` , und drücken Sie die EINGABETASTE. Dies bewirkt, dass die zweite Seite der Daten angezeigt werden (siehe Abbildung 9).


[![Die zweite Seitendaten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Abbildung 9**: der zweiten Seite der Daten wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Schritt 4: Erstellen der Auslagerungsdatei-Schnittstelle

Es gibt eine Vielzahl von verschiedenen Paging-Schnittstellen, die implementiert werden kann. GridView, DetailsView und FormView enthalten vier unterschiedliche Schnittstellen auszuwählen:

- **Nächsten, vorherigen** Benutzer können eine Seite zu einem Zeitpunkt, an den nächsten oder vorherigen eine verschieben.
- **Nächsten, vorherigen, ersten, letzten** zusätzlich zu den Schaltflächen Weiter und zurück, diese Schnittstelle erste und letzte Schaltflächen für die Umstellung auf der ersten oder letzten Seite umfasst.
- **Numerische** Listet die Seitenzahlen in der Auslagerungsdatei-Oberfläche, sodass Benutzer schnell zu einer bestimmten Seite zu wechseln.
- **Numerisch, ersten, letzten** zusätzlich zu den numerischen Seitenzahlen enthält Schaltflächen zum Verschieben in der ersten oder letzten Seite.

DataList und Repeater sind wir für die Entscheidung über eine Auslagerungsdatei-Schnittstelle und implementieren, verantwortlich. Dies umfasst das Erstellen der erforderlichen Web-Steuerelemente auf der Seite und die angeforderte Seite anzeigen, wenn eine bestimmte Paging Schnittstelle Schaltfläche geklickt wird. Darüber hinaus müssen bestimmte Steuerelemente der Benutzeroberfläche Paging können deaktiviert werden. Beispielsweise wird beim Anzeigen der ersten Seite der Daten mithilfe den nächsten zurück, ersten, letzten-Schnittstelle, würde der erste und die vorherige Schaltflächen deaktiviert werden.

Für dieses Lernprogramm können s mit der nächsten zurück, zunächst zuletzt Schnittstelle. Die Seite Websteuerelemente vier Schaltfläche hinzu, und legen Sie ihre `ID` s `FirstPage`, `PrevPage`, `NextPage`, und `LastPage`. Legen Sie die `Text` Eigenschaften &lt; &lt; zuerst &lt; Prev, Weiter &gt;, und das letzte &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Als Nächstes erstellen Sie eine `Click` -Ereignishandler für jede dieser fünf Schaltflächen. Im Anschluss fügen wir den erforderlichen Code für die angeforderte Seite anzuzeigen.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Die Gesamtzahl der Datensätze, die ausgelagerte über merken

Unabhängig von der Auslagerungsdatei-Schnittstelle ausgewählt haben müssen wir berechnen und speichern die Gesamtzahl der Datensätze, die ausgelagerte durch. Die Gesamtzeilenanzahl (in Verbindung mit der Größe der Seite ") wird bestimmt, wie viele Seiten mit Daten insgesamt über diesen Bericht ausgelagert werden die bestimmt, welche Paging von Benutzeroberflächen-Steuerelemente hinzugefügt werden oder aktiviert sind. Im weiter, vorherigen, ersten wird die letzte Schnittstelle, die wir erstellen, Anzahl der Seiten auf zwei Arten verwendet:

- Um festzustellen, ob wir in diesem Fall die letzte Seite eingesehene sind die Schaltflächen Weiter und letzten deaktiviert.
- Wenn der Benutzer die Schaltfläche klickt, die wir sie bis zum letzten weiter müssen, zu zählen, dessen Index eine ist Seite, die kleiner als die Seite.

Anzahl der Seiten wird berechnet, wie die Obergrenze für die Gesamtzeilenanzahl durch die Seitengröße geteilt. Wenn wir durch 79 Datensätze mit vier Datensätze pro Seite paging, klicken Sie dann die Seitenanzahl ist beispielsweise 20 (die Höchstwert des 79 / 4). Wenn wir die numerische Paging-Schnittstelle anzuzeigenden diese Informationen werden Sie darüber informiert, wie viele numerische Schaltflächen verwenden; Wenn unsere Schnittstelle Paging weiter oder letzte Schaltflächen enthält, wird die Seitenanzahl verwendet, zu bestimmen, wann Sie die Schaltflächen Weiter oder letzte deaktivieren.

Wenn die Auslagerungsschnittstelle eine letzte Schaltfläche enthält, ist es erforderlich, dass die Gesamtanzahl von Datensätzen, die ausgelagerte über über Postbacks gespeichert werden, damit wir auf die letzte Schaltfläche geklickt wird der Index der letzten Seite ermitteln können. Um dies zu ermöglichen, erstellen Sie eine `TotalRowCount` Eigenschaft in der ASP.NET Seite "s" Code-Behind-Klasse, die den Wert Ansichtszustand beibehält:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Zusätzlich zu `TotalRowCount`, nehmen Sie sich zum Erstellen von nur-Lese Seitenebene-Eigenschaften für den Zugriff auf einfache Weise der Seitenindex, die Seitengröße und Seitenanzahl:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Bestimmen die Gesamtzahl der Datensätze, die ausgelagerte über

Die `PagedDataSource` Objekt zurückgegeben, von der ObjectDataSource s `Select()` Methode hat darin *alle* der Datensätze, obwohl nur eine Teilmenge davon in DataList angezeigt werden. Die `PagedDataSource` s [ `Count` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) gibt nur die Anzahl der Elemente, die in der DataList; angezeigt werden die [ `DataSourceCount` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) gibt die Gesamtanzahl der Elemente in der `PagedDataSource`. Aus diesem Grund müssen wir die Seite "s" ASP.NET zuweisen `TotalRowCount` Eigenschaft den Wert von der `PagedDataSource` s `DataSourceCount` Eigenschaft.

Um dies zu erreichen, erstellen Sie einen Ereignishandler für das ObjectDataSource-s `Selected` Ereignis. In der `Selected` Ereignishandler, die wir haben Zugriff auf den Rückgabewert der ObjectDataSource s `Select()` Methode in diesem Fall die `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Die angeforderte Seite der Daten anzeigen

Klickt der Benutzer eine der Schaltflächen in der Auslagerungsdatei-Schnittstelle, müssen wir die angeforderte Seite der Daten anzeigen. Da die Auslagerungsparameter über die Abfragezeichenfolge angegeben werden, auf die angeforderte Seite Datenverwendung anzuzeigen `Response.Redirect(url)` haben die Benutzer s Browser erneut anfordern der `Paging.aspx` Seite mit den entsprechenden Parametern für die Auslagerung. Klicken wir z. B. zum Anzeigen von Daten der zweiten Seite umleiten des Benutzers zu `Paging.aspx?pageIndex=1`.

Um dies zu ermöglichen, erstellen Sie eine `RedirectUser(sendUserToPageIndex)` -Methode, die den Benutzer umgeleitet `Paging.aspx?pageIndex=sendUserToPageIndex`. Rufen Sie dann diese Methode über die Schaltfläche mit den vier `Click` Ereignishandler. In der `FirstPage` `Click` -Ereignishandler, rufen `RedirectUser(0)`, um sie zu senden, zur ersten Seite; in der `PrevPage` `Click` -Ereignishandler `PageIndex - 1` als der Seitenindex; und so weiter.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Mit der `Click` Ereignishandler abgeschlossen ist, die Datensätze DataList s über ausgelagert werden können, indem Sie auf die Schaltflächen. Nehmen Sie einen Moment Zeit, zu testen.

## <a name="disabling-paging-interface-controls"></a>Durch Deaktivieren des Paging von Benutzeroberflächen-Steuerelemente

Derzeit sind alle vier Schaltflächen unabhängig von der angezeigten Seite aktiviert. Wir möchten jedoch die Schaltflächen "First" und "zurück" zu deaktivieren, wenn die erste Seite der Daten und die Schaltflächen Weiter und letzten anzeigen, wenn die letzte Seite angezeigt. Die `PagedDataSource` das ObjectDataSource-s zurückgegebenes Objekt `Select()` Methode verfügt über Eigenschaften [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) und [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , die kann untersucht werden, um festzustellen, ob wir anzeigen die ersten oder letzten Seite der Daten.

Fügen Sie die folgenden, mit dem ObjectDataSource s `Selected` Ereignishandler:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Mit diese Ergänzung wird die Schaltflächen "First" und "zurück" deaktiviert werden, wenn die erste Seite anzeigen, während die Schaltflächen Weiter und letzten beim Anzeigen der letzten Seite deaktiviert werden.

Let s abzuschließen, die Paging-Schnittstelle durch den Benutzer darüber informiert Seite sie re derzeit anzeigen und wie viele Seiten insgesamt vorhanden sind. Die Seite ein Label-Websteuerelement hinzu, und legen Sie dessen `ID` Eigenschaft `CurrentPageNumber`. Legen Sie dessen `Text` Eigenschaft im ObjectDataSource s ausgewählte Ereignishandler solche enthält die aktuelle Seite angezeigt wird (`PageIndex + 1`) und die Gesamtanzahl der Seiten (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Abbildung 10 zeigt `Paging.aspx` beim ersten Mal besucht hat. Da die Abfragezeichenfolge leer ist, standardmäßig zeigt die ersten vier Produkte DataList; die Schaltflächen "First" und "zurück" sind deaktiviert. Klicken Sie weiter auf, wird die nächsten vier Datensätze (siehe Abbildung 11); die Schaltflächen "First" und "zurück" sind jetzt aktiviert.


[![Die erste Seite der Daten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Abbildung 10**: die erste Seite der Daten wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Die zweite Seitendaten wird angezeigt.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Abbildung 11**: der zweiten Seite der Daten wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> Die Schnittstelle Paging kann weiter verbessert werden, indem der Benutzer angeben, wie viele Seiten pro Seite anzeigen. Beispielsweise konnte eine DropDownList Optionen für die Auflistung Seitengröße z. B. 5, 10, 25, 50 und alle hinzugefügt werden. Nach Auswahl einer Seitengröße, muss der Benutzer wieder zu umgeleitet werden `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Ich lassen, implementieren diese Erweiterung als Übung für den Leser.


## <a name="using-custom-paging"></a>Verwenden benutzerdefiniertes Paging

Über seine Daten mithilfe der ineffizient Paging Standardverfahren DataList-Seiten. Wenn paging über ausreichend große Mengen von Daten ist es obligatorisch, dass das benutzerdefinierte Paging verwendet werden. Obwohl die Implementierungsdetails geringfügig unterscheiden zu können, gelten die Konzepte hinter implementieren benutzerdefiniertes Paging in einem DataList stimmt mit dem Standardpaging. Benutzerdefiniertes Paging anhand der `ProductBLL` Klasse s `GetProductsPaged` Methode (anstelle von `GetProductsAsPagedDataSource`). Entsprechend der Anleitung unter dem [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) Lernprogramm `GetProductsPaged` muss übergeben werden, den Start Zeile Index und die maximale Anzahl der zurückzugebenden Zeilen. Diese Parameter verwaltet werden können, über die Abfragezeichenfolge wie die `pageIndex` und `pageSize` im Standardmodus paging verwendeten Parameter.

Da es s keine `PagedDataSource` mit benutzerdefiniertes Paging alternative Techniken müssen verwendet werden, um zu bestimmen, die Gesamtzahl der Datensätze, die ausgelagerte durch und gibt an, ob wir re die ersten oder letzte Seite der Daten anzeigen. Die `TotalNumberOfProducts()` Methode in der `ProductsBLL` Klasse gibt die Gesamtzahl der ausgelagerte über Produkte zurück. Um festzustellen, ob die erste Seite der Daten angezeigt wird, überprüfen Sie den Startindex für die Zeile ist er 0 (null), und klicken Sie dann die erste Seite angezeigt wird. Die letzte Seite wird angezeigt wird, wenn die erste Zeilenindex sowie die Max. Anzahl zurückzugebender Zeilen ist größer als oder gleich der Gesamtzahl der Datensätze, die ausgelagerte über.

Untersucht werden, implementieren benutzerdefiniertes Paging ausführlicher in den nächsten Lernprogrammen.

## <a name="summary"></a>Zusammenfassung

Weder die DataList noch ein Repeater bietet die Out-of Box-Paging-Unterstützung in der GridView, DetailsView, gefunden und FormView steuert, solche Funktionen kann mit minimalem Aufwand hinzugefügt werden. Die einfachste Möglichkeit zum Implementieren von Standardnavigation wird zum Umschließen der Gesamtmenge der Produkte innerhalb einer `PagedDataSource` und dann binden Sie die `PagedDataSource` DataList oder Repeater. In diesem Lernprogramm wurde hinzugefügt der `GetProductsAsPagedDataSource` Methode, um die `ProductsBLL` Klasse die Rückgabe der `PagedDataSource`. Die `ProductsBLL` Klasse enthält bereits die Methoden, die für das benutzerdefinierte Paging benötigt `GetProductsPaged` und `TotalNumberOfProducts`.

Zusammen mit Abrufen von entweder der genaue Satz von Datensätzen, die für das benutzerdefinierte Paging angezeigt oder alle Datensätze in einem `PagedDataSource` für standardmäßige auslagerungen, müssen ebenfalls manuell hinzufügen der Auslagerungsdatei-Schnittstelle. Für dieses Lernprogramm erstellt es eine weiter, Previous, zunächst zuletzt eine Verbindung mit vier Schaltfläche Websteuerelemente. Darüber hinaus wurde ein Bezeichnungsfeld-Steuerelement anzeigen, die aktuelle Seitenzahl und Gesamtseitenzahl hinzugefügt.

In den nächsten Lernprogrammen sehen wir, wie die DataList und Repeater sortierungsunterstützung hinzugefügt werden. Wir sehen auch wie DataList erstellen, können sowohl per Pager benachrichtigt und werden (mit Beispielen, die mithilfe von Standard- und benutzerdefinierte Paging) sortiert.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Liz Shulok Ken Pespisa und Bernadette Leigh. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Weiter](sorting-data-in-a-datalist-or-repeater-control-vb.md)
