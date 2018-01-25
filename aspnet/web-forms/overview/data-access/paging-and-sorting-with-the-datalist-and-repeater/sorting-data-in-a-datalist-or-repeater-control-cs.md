---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Sortieren von Daten in einem DataList oder Wiederholungsmodul-Steuerelement (c#) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm untersuchen wir zum Sortieren der Unterstützung im DataList und Repeater einschließen als auch das Erstellen einer DataList oder Repeater, deren Daten können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cfd0cdb0afe3bf71686715c0b1891adfbbd5019a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Sortieren von Daten in einem DataList oder Wiederholungsmodul-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) oder [PDF herunterladen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In diesem Lernprogramm untersuchen wir zum Sortieren der Unterstützung im DataList und Repeater einschließen als auch das Erstellen einer DataList oder Repeater, deren Daten ausgelagert und sortiert werden können.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](paging-report-data-in-a-datalist-or-repeater-control-cs.md) untersucht, wie eine DataList Paging-Unterstützung hinzugefügt. Es erstellt eine neue Methode in der `ProductsBLL` Klasse (`GetProductsAsPagedDataSource`) zurückgegebenen ein `PagedDataSource` Objekt. Wenn an einem DataList oder Repeater gebunden ist, würde die DataList oder Repeater nur die angeforderte Seite der Daten angezeigt. Dieses Verfahren ähnelt der intern von GridView, DetailsView und FormView befüllt. damit ihre integrierten Standardsystem-Pagingfunktionen bereitzustellen.

Zusätzlich zur Unterstützung der Paginierung Angebots hinaus GridView sortieren Unterstützung ausgegeben. Weder die DataList noch ein Repeater bietet integrierte Sortierfunktionen; Allerdings kann das Sortierfunktionen mit viel Code hinzugefügt werden. In diesem Lernprogramm untersuchen wir zum Sortieren der Unterstützung im DataList und Repeater einschließen als auch das Erstellen einer DataList oder Repeater, deren Daten ausgelagert und sortiert werden können.

## <a name="a-review-of-sorting"></a>Eine Überprüfung der Sortierung

Wie wir gesehen, in haben der [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-cs.md) Lernprogramm des GridView-Steuerelements bietet Unterstützung sortieren ausgegeben. Jedes Feld GridView haben ein zugeordnetes `SortExpression`, wodurch das Feld "Daten", um die Daten zu sortieren. Wenn die GridView-s `AllowSorting` -Eigenschaftensatz auf `true`, jede GridView-Feld, enthält eine `SortExpression` Eigenschaftswert kann im Header als LinkButton gerendert. Klickt ein Benutzer einen bestimmten GridView-Feld-s-Header, einem Postback und die Daten gemäß den geklickt wurde Feld s sortiert `SortExpression`.

Des GridView-Steuerelements verfügt über eine `SortExpression` -Eigenschaft, die speichert die `SortExpression` des GridView-Felds durch die Daten sortiert. Darüber hinaus eine `SortDirection` Eigenschaft gibt an, ob die Daten in aufsteigender oder absteigender Reihenfolge (wenn ein Benutzer klickt auf ein bestimmter GridView Feld s Header Link zweimal hintereinander, die Sortierreihenfolge kann ein-/ausgeschaltet) sortiert werden.

GridView an die Datenquellen-Steuerelement gebunden ist, übergibt aus seiner `SortExpression` und `SortDirection` Eigenschaften auf die Daten der quellcodeverwaltung. Die Datenquellen-Steuerelement ruft die Daten ab und sortiert Sie es anschließend entsprechend der angegebenen `SortExpression` und `SortDirection` Eigenschaften. Nach dem Sortieren der Daten, die Datenquellen-Steuerelement das wird an die GridView zurückgegeben.

Um diese Funktionalität mit den Steuerelementen DataList oder Repeater zu replizieren, müssen wir:

- Erstellen Sie eine Sortierung-Schnittstelle
- Vergessen Sie nicht das Feld "Daten" Sortierungskriterium und ob in aufsteigender oder absteigender Reihenfolge sortiert
- Weisen Sie das ObjectDataSource zum Sortieren der Daten durch ein bestimmtes Datenfeld

Wir werden diese drei Aufgaben in den Schritten 3 und 4 konfigurieren. Danach untersuchen wir zum sowohl paging und -Unterstützung in einem DataList oder Repeater Sortierung einschließen.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Schritt 2: Anzeigen der Produkte in ein Repeater

Bevor wir Gedanken machen, die Sortierung bezogene Funktionalität implementieren, können Sie s starten, indem Sie die Liste der Produkte im Wiederholungsmodul-Steuerelement. Öffnen Sie zunächst die `Sorting.aspx` auf der Seite der `PagingSortingDataListRepeater` Ordner. Hinzufügen eines Repeater-Steuerelements zur Webseite, Festlegen seiner `ID` Eigenschaft `SortableProducts`. Erstellen Sie eine neue ObjectDataSource mit dem Namen aus dem Smarttag des Wiederholungsmoduls s `ProductsDataSource` und konfigurieren Sie ihn zum Abrufen von Daten aus der `ProductsBLL` Klasse s `GetProducts()` Methode. Wählen Sie die option (keine) aus den Dropdown-Listen auf den Registerkarten INSERT-, Update- und DELETE.


[![Erstellen Sie ein ObjectDataSource und konfigurieren Sie dafür die GetProductsAsPagedDataSource()-Methode](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Abbildung 1**: ein ObjectDataSource erstellen und konfigurieren Sie sie verwenden die `GetProductsAsPagedDataSource()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Legen Sie das Dropdown-Listen in der Update-, INSERT- und Löschen von Registerkarten (keine)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Abbildung 2**: Legen Sie das Dropdown-Listen in der Update-, INSERT- und Löschen von Registerkarten (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


Im Gegensatz zu mit DataList, Visual Studio erstellt nicht automatisch ein `ItemTemplate` für Wiederholungsmodul-Steuerelement nach dem binden es an eine Datenquelle. Darüber hinaus müssen wir Hinzufügen dieser `ItemTemplate` deklarativ, wie das Steuerelement s-Smarttag Repeater verfügt nicht über die die Vorlagen bearbeiten-Option in der DataList s gefunden. Verwenden Sie den gleichen Let s `ItemTemplate` aus dem vorherigen Lernprogramm, dem der Produktname s, Lieferanten und Kategorie angezeigt.

Nach dem Hinzufügen der `ItemTemplate`, Repeater und ObjectDataSource s deklarative Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Abbildung 3 zeigt diese Seite, wenn Sie über einen Browser angezeigt.


[![Jedes Produkt s Name, Hersteller und Kategorie wird angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Abbildung 3**: jedes Produkt s Name, Lieferanten und Kategorie angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Schritt 3: Angewiesen wird, das ObjectDataSource beim Sortieren von Daten

Zum Sortieren der Daten im Wiederholungsmodul angezeigt, müssen wir die ObjectDataSource des Sortierausdrucks zu informieren, von dem die Daten sortiert werden sollen. Bevor das ObjectDataSource seine Daten abruft, löst es zuerst die [ `Selecting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), stellt eine Möglichkeit, einen Sortierausdruck angeben. Die `Selecting` übergebene Ereignishandler wird ein Objekt des Typs [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), die über eine Eigenschaft namens verfügt [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) vom Typ [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). Die `DataSourceSelectArguments` Klasse dient zum datenbezogene Anforderungen von einem Consumer der Daten an das Datenquellensteuerelement zu übergeben, und enthält eine [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Um Sortierinformationen von der ASP.NET-Seite an das ObjectDataSource übergeben möchten, erstellen Sie einen Ereignishandler für das `Selecting` Ereignisses und verwenden Sie den folgenden Code:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Die *SortExpression* Wert sollte den Namen des Datenfelds zum Sortieren der Daten durch (z. B. ProductName) zugewiesen werden. Ist keine Richtung bezogene Sort-Eigenschaft, also, wenn Sie die Daten in absteigender Reihenfolge sortieren möchten, hängen Sie die Zeichenfolge "DESC" auf die *SortExpression* ist (z. B. ProductName DESC).

Fahren Sie fort, und wiederholen Sie einige andere hartcodierte Werte für *SortExpression* und die Ergebnisse in einem Browser testen. Wie Abbildung 4 zeigt bei Verwendung von ProductName DESC als, die *SortExpression*, werden die Produkte nach Namen in umgekehrter alphabetischer Reihenfolge sortiert.


[![Die Produkte werden nach ihren Namen in alphabetischer Reihenfolge umkehren sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Abbildung 4**: werden die Produkte nach Namen in alphabetischer Reihenfolge umkehren sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Schritt 4: Erstellen die Sortierung-Schnittstelle, und denken Sie daran, den Sortierungsausdruck und die Richtung

Sortieren-Unterstützung in der GridView einschalten jedes Headertext sortierbaren Feld s in LinkButton konvertiert, wenn angeklickt, sortiert die Daten entsprechend. Eine Sortierung Schnittstelle ist sinnvoll für GridView, in dem die Daten problemlos in Spalten angeordnet sind. Für die verschiedenen Steuerelemente ist jedoch eine andere Sortierung Schnittstelle erforderlich. Eine Sortierung allgemeine Schnittstelle für eine Liste mit Daten (im Gegensatz zu einem Datenraster), wird eine Dropdownliste, die enthält die Felder, nach denen die Daten sortiert werden können. Lassen Sie s, die für dieses Lernprogramm eine solche Schnittstelle implementieren.

Hinzufügen eines DropDownList-Web-Steuerelements, das oben die `SortableProducts` Repeater und legen seine `ID` Eigenschaft `SortBy`. Klicken Sie im Fenster Eigenschaften auf die Auslassungszeichen in der `Items` Eigenschaft zum Öffnen von "ListItem" Typauflistungs-Editor. Hinzufügen `ListItem` s zum Sortieren der Daten durch die `ProductName`, `CategoryName`, und `SupplierName` Felder. Fügen Sie auch eine `ListItem` die Produkte nach Namen in umgekehrter alphabetischer Reihenfolge sortiert.

Die `ListItem` `Text` Eigenschaften können festgelegt werden, auf einen beliebigen Wert (z. B. Name), aber die `Value` Eigenschaften müssen auf den Namen des Datenfelds (z. B. ProductName) festgelegt werden. Um die Ergebnisse in absteigender Reihenfolge zu sortieren, fügen Sie die Zeichenfolge "DESC" in den Datenfeldnamen wie ProductName DESC.


![Fügen Sie eine "ListItem" für jeden sortierbaren Datenfelder hinzu.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Abbildung 5**: Hinzufügen einer `ListItem` für jedes der Felder sortierbarer Datentyp


Abschließend fügen Sie ein Websteuerelement Schaltfläche rechts neben der DropDownList hinzu. Legen Sie seine `ID` auf `RefreshRepeater` und dessen `Text` Eigenschaft zu aktualisieren.

Nach dem Erstellen der `ListItem` s und die Schaltfläche "Aktualisieren" hinzugefügt haben, sollte die s-Deklarationssyntax DropDownList und Schaltfläche folgt oder ähnlich aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Mit der Sortierung DropDownList abgeschlossen ist, müssen als Nächstes aktualisieren Sie das ObjectDataSource-s `Selecting` -Ereignishandler, damit die ausgewählten verwendet `SortBy``ListItem` s `Value` Eigenschaft im Gegensatz zu einer hartcodierten Sortierungsausdruck.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

An diesem Punkt, wenn zuerst auf der Seite auf die Produkte anfänglich sortiert die `ProductName` Feld "Daten", da sie s der `SortBy` `ListItem` standardmäßig ausgewählt (siehe Abbildung 6). Auswählen eines anderen Option wie z. B. die Kategorie zu sortieren und Sie auf Aktualisieren klicken, wird dazu führen, dass einen Postback und sortieren Sie die Daten erneut durch den Kategorienamen, wie die Abbildung 7 zeigt ein Beispiel.


[![Die Produkte sind zunächst nach Namen sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Abbildung 6**: die Produkte werden anfänglich nach Namen sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Die Produkte sind jetzt nach Kategorie sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Abbildung 7**: die Produkte werden jetzt nach Kategorie sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Klicken auf die Schaltfläche "Aktualisieren" bewirkt, dass die Daten automatisch neu sortiert werden, da die Repeater s Ansichtszustand deaktiviert wurde, und somit verursacht der Repeater erneut an die Datenquelle auf jedes Postback binden. Bleibt Sie Ve Repeater s Ansichtszustand aktiviert ist, Ändern der Sortierung Dropdown-Liste/t gewonnen haben wirkt sich auf die Sortierreihenfolge. Zur Behebung des Problems, erstellen Sie einen Ereignishandler für die Schaltfläche Aktualisieren s `Click` Ereignis und Seitenindex Repeater mit der Datenquelle (durch Aufrufen der Repeater s `DataBind()` Methode).


## <a name="remembering-the-sort-expression-and-direction"></a>Denken Sie daran, den Sortierungsausdruck und die Richtung

Beim Erstellen eines sortierbaren DataList oder Repeater auf einer Seite, bei denen nicht sortieren Postbacks auftreten verknüpfte, es s Imperative, dass der Sortierungsausdruck und die Richtung über Postbacks gespeichert werden. Angenommen Sie, dass wir in diesem Lernprogramm aus, um eine Schaltfläche "löschen" mit den einzelnen Produkten enthalten Repeater aktualisiert. Klickt der Benutzer die Schaltfläche "löschen" führen wir d Code zum Löschen des ausgewählten Produkts und binden Sie dann die Daten an die Repeater. Wenn die Sortierung Details über Postback nicht beibehalten werden, werden die auf dem Bildschirm angezeigten Daten die ursprüngliche Sortierreihenfolge wiederhergestellt.

Für dieses Lernprogramm speichert der DropDownList implizit die Sortierreihenfolge Ausdruck und die Richtung in seinen Ansichtszustand für uns. Wenn eine andere Sortierung Schnittstelle einen mit, z. B. LinkButtons verwendet wurden, die die verschiedenen Sortieroptionen bereitgestellt müssen wir d dafür sorgen, dass die Sortierreihenfolge über Postbacks speichern müssen. Hierzu kann verwendet werden, durch die Sortierung Parameter im Ansichtszustand Seite s durch Einschließen des Sort-Parameters in der Abfragezeichenfolge oder durch ein anderes Zustand Persistenz Verfahren speichern.

Zukünftige Beispielen in diesem Lernprogramm untersuchen, wie die Sortierung Details in den Anzeigezustand der Seite s beibehalten wird.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Schritt 5: Hinzufügen von Unterstützung an, die Standardnavigation verwendet DataList sortieren

In der [vorherigen Lernprogramm](paging-report-data-in-a-datalist-or-repeater-control-cs.md) untersucht, wie mit einem DataList Standardnavigation implementiert. Lassen Sie s Erweitern dieses vorherigen Beispiel, um die Möglichkeit zum Sortieren der ausgelagerten Daten einzuschließen. Öffnen Sie zunächst die `SortingWithDefaultPaging.aspx` und `Paging.aspx` Seiten in der `PagingSortingDataListRepeater` Ordner. Von der `Paging.aspx` Seite, klicken Sie auf die Schaltfläche "Quelle" das deklarative Markup der Seite "s" anzeigen. Kopieren Sie den ausgewählten Text (siehe Abbildung 8) und fügen Sie ihn in deklarativen Markup `SortingWithDefaultPaging.aspx` zwischen den `<asp:Content>` Tags.


[![Replizieren der deklarativem Markup in der &lt;Asp: Inhalt&gt; Tags aus Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Abbildung 8**: Replizieren von deklarativen Markup in der `<asp:Content>` Tags aus `Paging.aspx` auf `SortingWithDefaultPaging.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Nach dem Kopieren der deklarativen Markups, kopieren Sie die Methoden und Eigenschaften in der `Paging.aspx` Seite s Code-Behind-Klasse, um die Code-Behind-Klasse für `SortingWithDefaultPaging.aspx`. Als Nächstes nehmen einen Moment Zeit zum Anzeigen der `SortingWithDefaultPaging.aspx` Seite in einem Browser. Es sollten zeigen, die dieselbe Funktionalität und die Darstellung als `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Erweitern von ProductsBLL den Standardwert, Paging und sortieren die Methode einschließen

Im vorherigen Lernprogramm erstellten eine `GetProductsAsPagedDataSource(pageIndex, pageSize)` Methode in der `ProductsBLL` -Klasse zurückgegebenen ein `PagedDataSource` Objekt. Dies `PagedDataSource` mit gefüllt wurde *alle* Produkte (über die BLL s `GetProducts()` Methode), jedoch bei der Bindung an das DataList nur die Datensätze, die der angegebenen *PageIndex* und *PageSize* Eingabeparameter angezeigt wurden.

Weiter oben in diesem Lernprogramm wir sortierungsunterstützung hinzugefügt, durch angeben den Sortierungsausdruck aus den s ObjectDataSource `Selecting` -Ereignishandler. Dies funktioniert gut bei der ObjectDataSource ein Objekt zurückgegeben werden, die, wie z. B. sortiert werden kann, die `ProductsDataTable` zurückgegebenes die `GetProducts()` Methode. Allerdings die `PagedDataSource` zurückgegebenes Objekt die `GetProductsAsPagedDataSource` Methode unterstützt nicht die Sortierung von dessen inneren Datenquelle. Stattdessen müssen zum Sortieren der Ergebnisse zurückgegeben werden, aus der `GetProducts()` Methode *vor* wir fügen Sie ihn in die `PagedDataSource`.

Um dies zu erreichen, erstellen Sie eine neue Methode in der `ProductsBLL` Klasse `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Zum Sortieren der `ProductsDataTable` zurückgegebenes der `GetProducts()` -Methode, geben Sie die `Sort` Eigenschaft von seinem Standardwert `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Die `GetProductsSortedAsPagedDataSource` Methoden unterscheiden sich nur geringfügig von den `GetProductsAsPagedDataSource` Methode, die im vorherigen Lernprogramm erstellt. Insbesondere `GetProductsSortedAsPagedDataSource` zusätzlichen Eingabeparameter akzeptiert `sortExpression` und weist diesen Wert auf die `Sort` Eigenschaft von der `ProductDataTable` s `DefaultView`. Einige Zeilen Code später die `PagedDataSource` s DataSource-Objekt zugeordnet ist die `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Aufrufen der Methode GetProductsSortedAsPagedDataSource und Angeben des Werts für die SortExpression Input-Parameter

Mit der `GetProductsSortedAsPagedDataSource` Methode vollständig ist, der nächste Schritt besteht, den Wert für diesen Parameter bereitzustellen. ObjectDataSource `SortingWithDefaultPaging.aspx` ist derzeit so konfiguriert, dass das Aufrufen der `GetProductsAsPagedDataSource` -Methode auf und übergibt die beiden Eingabeparameter durch seine beiden `QueryStringParameters`, im angegeben werden die `SelectParameters` Auflistung. Diese beiden `QueryStringParameters` darauf hinweisen, dass die Quelle für die `GetProductsAsPagedDataSource` Methode s *PageIndex* und *PageSize* Parameter herstammt Querystring-Felder `pageIndex` und `pageSize`.

Aktualisieren Sie die s ObjectDataSource `SelectMethod` Eigenschaft, sodass die It die neue ruft `GetProductsSortedAsPagedDataSource` Methode. Fügen Sie dann ein neues `QueryStringParameter` , damit die *SortExpression* Eingabeparameter erfolgt über den Querystring-Feld `sortExpression`. Legen Sie die `QueryStringParameter` s `DefaultValue` zu ProductName.

Nach der Änderung sollte das ObjectDataSource s deklarative Markup formuliert werden:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

An diesem Punkt der `SortingWithDefaultPaging.aspx` sortieren Seite seine Ergebnisse in alphabetischer Reihenfolge nach dem Produktnamen (siehe Abbildung 9). Dies liegt daran, dass standardmäßig ein Wert von ProductName in übergeben wird, als die `GetProductsSortedAsPagedDataSource` Methode s *SortExpression* Parameter.


[![Standardmäßig werden die Ergebnisse nach Produktname sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Abbildung 9**: Standardmäßig werden die Ergebnisse nach sortiert `ProductName` ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Wenn Sie manuell hinzufügen einer `sortExpression` Querystring-Feld, z. B. `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` die Ergebnisse sortiert werden durch das angegebene `sortExpression`. Allerdings dies `sortExpression` Parameter ist nicht in der Abfragezeichenfolge enthalten, wenn Sie zu einer anderen Seite der Daten zu verschieben. In der Tat durch Klicken auf die nächste bzw. letzten Seite rufen uns in erneut Schaltflächen `Paging.aspx`! Darüber hinaus bietet s zurzeit keine Sortierung Schnittstelle. Die einzige Möglichkeit, die ein Benutzer die Sortierreihenfolge der ausgelagerten Daten ändern kann, ist durch die Abfragezeichenfolge direkt bearbeiten.

## <a name="creating-the-sorting-interface"></a>Zum Erstellen der Sortierung-Schnittstelle

Wir müssen zunächst beim Aktualisieren der `RedirectUser` Methode, um den Benutzer zu senden `SortingWithDefaultPaging.aspx` (anstelle von `Paging.aspx`) und einzuschließen der `sortExpression` Wert in der Abfragezeichenfolge. Es sollte auch eine schreibgeschützte, auf Seitenebene mit dem Namen hinzufügen `SortExpression` Eigenschaft. Diese Eigenschaft ähnelt der `PageIndex` und `PageSize` Eigenschaften, die im vorherigen Lernprogramm erstellt gibt den Wert der `sortExpression` Querystring-Feld, wenn er vorhanden ist, und der standardmäßige Wert (ProductName) andernfalls.

Derzeit die `RedirectUser` akzeptiert nur einen einzelnen Eingabeparameter der Index der anzuzeigenden Methode. Möglicherweise gibt es jedoch Situationen wir zum Umleiten des Benutzers zu einer bestimmten Seite der Daten mithilfe eines Sortierungsausdrucks als neuerungen in der Abfragezeichenfolge angegeben. Im Anschluss erstellen wir die Sortierung Schnittstelle für diese Seite eine Reihe von Schaltfläche Websteuerelemente zum Sortieren der Daten durch eine angegebene Spalte enthalten. Wenn einer der über diese Schaltflächen geklickt wird, möchten wir zum Umleiten des Benutzers in den entsprechenden Ausdruck Sortierwert übergeben. Um diese Funktionalität zu gewährleisten, erstellen Sie zwei Versionen der `RedirectUser` Methode. Die erste Vorlage sollte nur den Seitenindex anzeigen, während das zweite Argument der Seite "Index und die Sortierreihenfolge-Ausdruck akzeptiert, akzeptieren.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Im ersten Beispiel in diesem Lernprogramm haben wir eine Sortierung Schnittstelle mithilfe einer DropDownList erstellt. In diesem Beispiel verwenden können s drei Schaltfläche Websteuerelemente über eine DataList positioniert wird, für die Sortierung von `ProductName`möglich für `CategoryName`, und eine für `SupplierName`. Die drei Schaltfläche Web Steuerelemente hinzufügen, Festlegen ihrer `ID` und `Text` Eigenschaften entsprechend:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Als Nächstes erstellen Sie eine `Click` für die einzelnen Ereignishandler. Die Ereignishandler aufrufen sollte die `RedirectUser` Methode, die den Benutzer auf die erste Seite mit den entsprechenden Sortierungsausdruck zurückgeben.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Wenn die Seite zuerst besuchen zu können, die Daten durch den Namen des Produkts alphabetisch sortiert (siehe Abbildung 9). Klicken Sie auf die Schaltfläche "Weiter", um auf der zweiten Seite des Data setzen, und klicken Sie dann auf die Sortierung nach Kategorie. Dies gibt uns zur ersten Seite der Daten, sortiert nach Kategorienamen (siehe Abbildung 10). Klicken auf die Sortierreihenfolge durch Lieferanten Schaltfläche sortiert ebenso die Daten vom Lieferanten, die von der ersten Seite der Daten ab. Die Sortierungsoption gespeichert wie die Daten über ausgelagert werden. Abbildung 11 zeigt die Seite nach dem Sortieren nach Kategorie, und klicken Sie dann das dreizehnte Datenseite Vorlauf.


[![Die Produkte sind nach Kategorien sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Abbildung 10**: werden die Produkte nach Kategorie sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Der Sortierausdruck ist gespeicherten beim Paging durch die Daten](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Abbildung 11**: der Sortierungsausdruck ist gespeicherten beim Paging durch die Daten ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Schritt 6: Benutzerdefinierte Paging durch die Datensätze in ein Repeater

Das DataList-Beispiel, die in Schritt 5 Seiten über seine Daten mithilfe der ineffizient Paging Standardverfahren untersucht. Wenn paging über ausreichend große Mengen von Daten ist es obligatorisch, dass das benutzerdefinierte Paging verwendet werden. In der [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) und [benutzerdefinierte ausgelagerten Daten sortieren](../paging-and-sorting/sorting-custom-paged-data-cs.md) Lernprogramme, untersucht die Unterschiede zwischen Standard- und benutzerdefinierte Paging und erstellte Methoden in der BLL für Verwenden von benutzerdefinierten paging und Sortieren von benutzerdefinierten ausgelagerten Daten. In diesen beiden vorherigen Lernprogrammen hinzugefügt wird insbesondere die folgenden drei Methoden für die `ProductsBLL` Klasse:

- `GetProductsPaged(startRowIndex, maximumRows)`Gibt eine bestimmte Teilmenge der Datensätze, die beginnend am *StartRowIndex* und nicht größer als *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Gibt eine bestimmte Teilmenge der Datensätze, die durch das angegebene sortiert *SortExpression* input-Parameters.
- `TotalNumberOfProducts()`Stellt die Gesamtzahl der Datensätze in der `Products` Datenbanktabelle.

Diese Methoden können für eine effiziente Seite und sortieren Sie Daten mithilfe eines Steuerelements DataList oder Repeater verwendet werden. Um dies zu veranschaulichen, können Sie beginnen, indem ein Wiederholungsmodul-Steuerelement mit Unterstützung für benutzerdefiniertes Paging erstellen s; Diese wird dann Sortierfunktionen hinzugefügt.

Öffnen der `SortingWithCustomPaging.aspx` auf der Seite der `PagingSortingDataListRepeater` Ordner und fügen Sie auf der Seite einen Repeater Festlegen seiner `ID` Eigenschaft `Products`. Erstellen Sie eine neue ObjectDataSource mit dem Namen aus dem Smarttag des Wiederholungsmoduls s `ProductsDataSource`. Konfigurieren sie zum Auswählen der zugehörigen Daten aus der `ProductsBLL` Klasse s `GetProductsPaged` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Abbildung 12**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProductsPaged` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine), und klicken Sie dann auf die Schaltfläche "Weiter". Der Datenquelle konfigurieren-Assistent fordert nun die Quellen der `GetProductsPaged` Methode s *StartRowIndex* und *MaximumRows* Eingabeparameter. In Wirklichkeit werden diese Parameter ignoriert. Stattdessen die *StartRowIndex* und *MaximumRows* Werte werden durch übergeben werden die `Arguments` Eigenschaft in das ObjectDataSource-s `Selecting` Ereignishandler, d. h. wie wir angegeben wie die *SortExpression* in diesem Lernprogramm s der ersten Demo. Daher lassen Sie die Parameterquelle Dropdownlisten im Assistenten auf None festgelegt.


[![Behalten Sie die keine Quellen Parametersatz](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Abbildung 13**: lassen Sie die Quellen Parametersatz auf None ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Führen Sie *nicht* legen Sie die s ObjectDataSource `EnablePaging` Eigenschaft `true`. Dies bewirkt, dass das ObjectDataSource automatisch einen eigenen aufnehmen *StartRowIndex* und *MaximumRows* Parameter für die `SelectMethod` s vorhandenen Parameterliste. Die `EnablePaging` Eigenschaft ist nützlich, wenn die Bindung benutzerdefinierte Daten an ein Steuerelement GridView, DetailsView oder FormView ausgelagert, da diese Steuerelemente bestimmte Verhalten unterscheidet sich von der ObjectDataSource s erwarten nur verfügbar, wenn `EnablePaging` Eigenschaft ist `true`. Da wir haben das auslagerungsunterstützung für das DataList und Repeater manuell hinzufügen, lassen Sie diese Eigenschaft auf festgelegt `false` (Standard), wie wir in unserer ASP.NET-Seite direkt in die benötigte Funktionalität integrieren müssen.


Definieren Sie schließlich die Repeater s `ItemTemplate` , damit der Produktname s, Kategorie und Lieferanten angezeigt werden. Nach der Änderung sollte die s-Deklarationssyntax Repeater und ObjectDataSource etwa wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Nehmen Sie einen Moment Zeit, besuchen die Seite über einen Browser, und beachten, dass keine Datensätze zurückgegeben werden. Grund hierfür ist, wir haben noch an die *StartRowIndex* und *MaximumRows* Parameterwerte; aus diesem Grund werden Werte von 0 in übergeben wird, für beide. Um diese Werte festzulegen, erstellen Sie einen Ereignishandler für das ObjectDataSource-s `Selecting` Ereignis, und legen Sie diese Parameter Werte programmgesteuert auf hartcodierte Werte von 0 und 5, bzw.:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Durch diese Änderung wird die Seite, bei der Anzeige über einen Webbrowser, und die ersten fünf Produkte.


[![Die ersten fünf Datensätze werden angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Abbildung 14**: die ersten fünf Datensätze angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> In Abbildung 14 aufgelisteten Produkte geschehen nach Produktname sortiert werden, da die `GetProductsPaged` gespeicherte Prozedur, die die effiziente benutzerdefiniertes Pagingabfrage führt sortiert die Ergebnisse nach `ProductName`.


Damit der Benutzer zu den Seiten durchlaufen kann, müssen wir Verfolgen der Startindex für die Zeile und die maximale Anzahl an Zeilen und speichern diese Werte über mehrere Postbacks. Im Beispiel Paging Standard verwendet wurde Querystring-Felder, um diese Werte beibehalten werden; Lassen Sie für diese Demo s, die diese Informationen in den Anzeigezustand der Seite s persistent zu speichern. Erstellen Sie die folgenden zwei Eigenschaften:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Den Code im Ereignishandler durch auswählen als Nächstes aktualisieren, sodass er verwendet die `StartRowIndex` und `MaximumRows` Eigenschaften anstelle der hartcodierte Werte von 0 und 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

Unsere-Seite wird an diesem Punkt immer noch nur die ersten fünf Datensätze. Allerdings mit diesen Eigenschaften eingerichtet ist, wir re kann jetzt unsere Paging-Schnittstelle zu erstellen.

## <a name="adding-the-paging-interface"></a>Hinzufügen der Auslagerungsdatei-Schnittstelle

Let s verwenden, die die gleichen ersten, zurück, anschließend werden die letzten paging-Schnittstelle, die in Standard Paging-Beispiel, einschließlich der Bezeichnung Web Steuerelemente aus, welche Seite der Daten angezeigt wird und wie viele Seiten insgesamt vorhanden verwendet werden. Fügen Sie vier Schaltfläche Websteuerelementen und Bezeichnung unter Wiederholungsmoduls.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Als Nächstes erstellen Sie `Click` -Ereignishandler für die vier Schaltflächen. Wenn eine der folgenden Schaltflächen geklickt wird, müssen wir aktualisiert die `StartRowIndex` und binden Sie die Daten an die Repeater. Ist der Code für die erste zurück und Weiter Schaltflächen einfach genug, aber für die letzte Schaltfläche wie feststellen wir der Startindex für die Zeile für die letzte Seite der Daten? Zum Berechnen dieser Index als auch bestimmen, ob die Schaltflächen Weiter und letzten aktiviert werden soll, müssen wir wissen, wie viele Datensätze insgesamt über ausgelagert werden können. Wir kann dies durch Aufrufen von bestimmen die `ProductsBLL` Klasse s `TotalNumberOfProducts()` Methode. Let s erstellen Sie eine schreibgeschützte, auf Seitenebene-Eigenschaft, die mit dem Namen `TotalRowCount` , die die Ergebnisse zurückgegeben werden die `TotalNumberOfProducts()` Methode:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Mit dieser Eigenschaft können wir nun den Zeilenindex der letzten Seite "s" Starten bestimmen. Insbesondere, s die ganze Zahl als Ergebnis von der `TotalRowCount` minus 1 dividiert durch `MaximumRows`multipliziert mit `MaximumRows`. Jetzt können Sie schreiben die `Click` Ereignishandler für die vier Schaltflächen der Auslagerungsdatei-Schnittstelle:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Schließlich müssen wir die ersten und zurück-Schaltfläche in der Auslagerungsdatei-Schnittstelle zu deaktivieren, wenn die erste Seite der Daten und die Schaltflächen Weiter und letzten angezeigt werden, wenn die letzte Seite anzeigen. Um dies zu erreichen, fügen Sie den folgenden Code, mit dem ObjectDataSource s `Selecting` Ereignishandler:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Nach dem Hinzufügen dieser `Click` Ereignishandler und den Code zu aktivieren oder deaktivieren die Elemente der Benutzeroberfläche Paging basierend auf der aktuellen Zeile startIndex testen Sie die Seite in einem Browser. Abbildung 15 veranschaulicht, wenn zuerst die erste Seite besuchen und werden der vorherige Schaltflächen sind deaktiviert. Klicken auf Weiter repräsentieren die zweite Seite der Daten, zuletzt auf die letzte Seite angezeigt (siehe Abbildung 16 und 17). Beim Anzeigen der letzten Seite der Daten sind die Schaltflächen Weiter "und" Letzte "deaktiviert.


[![Die Schaltflächen zurück und letzten sind deaktiviert, wenn die erste Seite Produkte anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Abbildung 15**: der vorherigen und der letzte Schaltflächen sind deaktiviert, wenn die erste Seite Produkte anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Die zweite Seite Produkte werden angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Abbildung 16**: der zweiten Seite des Produkte sind fest ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Klicken Sie auf letzte zeigt die letzte Seite der Daten](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Abbildung 17**: Klicken Sie zuletzt auf die endgültige Seitendaten wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Schritt 7: Repeater ausgelagerten einschließlich Unterstützung mit der benutzerdefinierten Sortierung

Jetzt, benutzerdefiniertes Paging implementiert wurde, unterstützen wir re Installationsbereit Sortierung einschließen. Die `ProductsBLL` Klasse s `GetProductsPagedAndSorted` Methode hat die gleiche *StartRowIndex* und *MaximumRows* Eingabeparameter als `GetProductsPaged`, jedoch kann eine zusätzliche  *SortExpression* input-Parameters. Verwenden der `GetProductsPagedAndSorted` Methode `SortingWithCustomPaging.aspx`, müssen wir die folgenden Schritte ausführen:

1. Ändern Sie die s ObjectDataSource `SelectMethod` Eigenschaft von `GetProductsPaged` auf `GetProductsPagedAndSorted`.
2. Hinzufügen einer *SortExpression* `Parameter` Objekt mit dem ObjectDataSource s `SelectParameters` Auflistung.
3. Erstellen Sie eine "Privat", auf Seitenebene `SortExpression` Eigenschaft, die den Wert über Postbacks über die Seite "s" Ansichtszustand beibehält.
4. Aktualisieren Sie die s ObjectDataSource `Selecting` -Ereignishandler hinzu, weisen Sie das ObjectDataSource-s *SortExpression* Parameter den Wert der Ebene der Seite `SortExpression` Eigenschaft.
5. Erstellen Sie die Sortierung-Schnittstelle.

Starten mit dem Aktualisieren der ObjectDataSource s `SelectMethod` -Eigenschaft und das Hinzufügen einer *SortExpression* `Parameter`. Stellen Sie sicher, dass die *SortExpression* `Parameter` s `Type` -Eigenschaftensatz auf `String`. Nach Abschluss dieser ersten beiden Aufgaben, sollte das ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Wir als Nächstes eine Seitenebene `SortExpression` Eigenschaft, deren Wert in der Statusansicht serialisiert wird. Wenn keine Sortierung Ausdruckswert festgelegt wurde, können verwenden Sie ProductName als Standard:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Bevor das ObjectDataSource aufgerufen wird die `GetProductsPagedAndSorted` Methode wir festlegen müssen der *SortExpression* `Parameter` auf den Wert des der `SortExpression` Eigenschaft. In der `Selecting` Ereignishandler, fügen Sie die folgende Codezeile hinzu:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Alle bleibt die Sortierung-Schnittstelle implementiert werden. Wie wir im letzten Beispiel können Sie s, die die Sortierung Schnittstelle mithilfe von drei Schaltfläche Web-Steuerelemente, mit denen den Benutzer zum Sortieren der Ergebnisse nach Produktname, Kategorie oder Lieferanten implementiert haben.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Erstellen Sie `Click` Ereignishandler für diese drei Schaltflächen-Steuerelemente. Ereignishandler im Ereignis Zurücksetzen der `StartRowIndex` auf 0 (null) festgelegt die `SortExpression` auf die entsprechenden Wert und Seitenindex zu Wiederholungsmoduls Daten:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

S alles ist es! Während eine Anzahl von Schritten für benutzerdefinierte Paging und Sortieren von implementiert wurden, wurden die Schritte für standardmäßige auslagerungen Virtualisierung mit einzelstamm sehr ähnlich. Abbildung 18 zeigt die Produkte an, beim Anzeigen der letzten Seite der Daten nach Kategorie sortiert.


[![Der letzten Seite der Daten, die Sorted nach der Kategorie wird angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Abbildung 18**: der letzten Seite der Daten, Sorted nach der Kategorie wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> In vorherigen Beispielen beim Sortieren nach der Lieferant Lieferantenname als Sortierausdruck verwendet wurde. Für das benutzerdefinierte Paging-Implementierung müssen wir jedoch CompanyName verwenden. Grund hierfür ist die gespeicherte Prozedur, die verantwortlich für das benutzerdefiniertes Paging implementieren `GetProductsPagedAndSorted` übergibt den Sortierungsausdruck in der `ROW_NUMBER()` -Schlüsselwort, das `ROW_NUMBER()` -Schlüsselwort benötigt einen Alias, anstatt den tatsächlichen Spaltennamen. Deshalb müssen wir verwenden `CompanyName` (der Name der Spalte in der `Suppliers` Tabelle) anstelle der Alias verwendet, die der `SELECT` Abfrage (`SupplierName`) für den Sortierungsausdruck.


## <a name="summary"></a>Zusammenfassung

Weder die DataList noch Repeater bieten integrierte sortierungsunterstützung, jedoch mit ein wenig Code und eine benutzerdefinierte Sortierung-Schnittstelle, die solche Funktionen hinzugefügt werden kann. Beim Sortieren, aber keine Auslagerung implementieren, kann der Sortierungsausdruck angegeben werden, über die `DataSourceSelectArguments` -Objekt übergeben, in der ObjectDataSource s `Select` Methode. Dies `DataSourceSelectArguments` s-Objekt `SortExpression` Eigenschaft zugewiesen werden kann, in das ObjectDataSource-s `Selecting` -Ereignishandler.

Der einfachste Ansatz werden, um eine DataList oder Repeater, die bereits zur Unterstützung der Paginierung Verfügung Sortierfunktionen hinzuzufügen, Anpassen der Business Logic Layer, um eine Methode enthalten, die einen Sortierungsausdruck akzeptiert. Diese Informationen kann dann übergeben werden, über einen Parameter in der ObjectDataSource s `SelectParameters`.

Dieses Lernprogramm ist unsere Untersuchung von paging und Sortieren mit den verschiedenen Steuerelementen abgeschlossen. Unser Tutorial aus, nächsten und letzten untersucht wie Vorlagen s DataList und Repeater Schaltfläche Websteuerelemente hinzugefügt, um einige benutzerdefinierte, vom Benutzer initiierten Funktionen auf einer pro-Item-Basis bereitzustellen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde David Suru. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
[Weiter](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
