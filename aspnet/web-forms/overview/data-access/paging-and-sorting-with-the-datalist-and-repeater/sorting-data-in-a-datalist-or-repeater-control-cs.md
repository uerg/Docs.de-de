---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Sortieren von Daten in einem DataList- oder Wiederholungssteuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir, wie Sie eine Sortierung unterstützt, in dem DataList- und Wiederholungssteuerelement umfassen als auch zum Erstellen von einem DataList- oder Repeater, deren Daten können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 85b59040cce266165353fe1627ffd983473bdcb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371638"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Sortieren von Daten in einem DataList- oder Wiederholungssteuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) oder [PDF-Datei herunterladen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In diesem Tutorial betrachten wir, wie Sie eine Sortierung unterstützt, in dem DataList- und Wiederholungssteuerelement umfassen als auch zum Erstellen von einem DataList- oder Repeater, deren Daten seitenweise durchlaufen und sortiert werden können.


## <a name="introduction"></a>Einführung

In der [vorherigen Tutorial](paging-report-data-in-a-datalist-or-repeater-control-cs.md) untersuchten wir wie einem DataList-Steuerelement Paging-Unterstützung hinzugefügt. Erstellt eine neue Methode in der `ProductsBLL` Klasse (`GetProductsAsPagedDataSource`) zurückgegebenen eine `PagedDataSource` Objekt. Wenn an einem DataList- oder Repeater gebunden ist, würde die DataList- oder Repeater nur die angeforderte Seite der Daten angezeigt. Dieses Verfahren ähnelt der was intern durch die GridView, DetailsView oder FormView-Steuerelemente verwendet wird, um die integrierten Standardsystem Pagingfunktionen bereitzustellen.

Bieten Unterstützung der Paginierung, nicht nur eine enthält die GridView auch standardmäßig Sortierung unterstützt. Weder die Repeater das DataList-Steuerelement bietet integrierte Sortierfunktionen; Allerdings kann das Sortieren von Funktionen mit ein paar Codezeilen hinzugefügt werden. In diesem Tutorial betrachten wir, wie Sie eine Sortierung unterstützt, in dem DataList- und Wiederholungssteuerelement umfassen als auch zum Erstellen von einem DataList- oder Repeater, deren Daten seitenweise durchlaufen und sortiert werden können.

## <a name="a-review-of-sorting"></a>Eine Beschreibung der Sortierung

Wie wir, in gesehen der [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-cs.md) Tutorial, das GridView-Steuerelement enthält, standardmäßig Sortierung unterstützt. Jedes Feld GridView haben ein zugeordnetes `SortExpression`, womit das Datenfeld, um die Daten zu sortieren. Wenn der GridView-s `AllowSorting` -Eigenschaftensatz auf `true`, jedes GridView-Feld, das ist eine `SortExpression` Eigenschaftswert kann der Header als ein LinkButton gerendert. Klickt ein Benutzer einen bestimmten GridView-Feld-s-Header, einem Postback, und die Daten werden gemäß dem geklickt s-Feld sortiert `SortExpression`.

Das GridView-Steuerelement verfügt über eine `SortExpression` -Eigenschaft, die speichert die `SortExpression` des GridView-Felds durch die Daten sortiert. Darüber hinaus eine `SortDirection` Eigenschaft gibt an, ob die Daten in aufsteigender oder absteigender Reihenfolge, (wenn ein Benutzer klickt auf eine bestimmte GridView Feld s-Header-Verknüpfung zweimal hintereinander und die Sortierreihenfolge wird umgeschaltet) sortiert werden.

Wenn an der Datenquellen-Steuerelement die GridView gebunden ist, übergibt Sie deaktivieren die `SortExpression` und `SortDirection` Eigenschaften auf die Daten der quellcodeverwaltung. Das Datenquellen-Steuerelement ruft die Daten ab und sortiert es anschließend gemäß der bereitgestellten `SortExpression` und `SortDirection` Eigenschaften. Nach dem Sortieren der das an, das Datenquellen-Steuerelement das wird an die GridView zurückgegeben.

Um diese Funktionalität mit dem DataList- oder Repeater-Steuerelementen zu replizieren, müssen Sie:

- Erstellen Sie eine Sortierung-Schnittstelle
- Merken Sie sich das Datenfeld nach zu sortieren und angibt, ob die Sortierung in aufsteigender oder absteigender Reihenfolge
- Weisen Sie dem ObjectDataSource-Steuerelement zum Sortieren der Daten nach einem bestimmten Feld

Wir werden diese drei Aufgaben in den Schritten 3 und 4 in Angriff nehmen. Danach untersuchen wir wie beinhalten Auslagern und sortieren die Unterstützung in einem DataList- oder Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Schritt 2: Anzeigen der Produkte in einem Wiederholungssteuerelement

Bevor wir sortieren-bezogenen Funktionen implementieren fürchten, können Sie s zunächst wird aufgelistet, die Produkte in ein Repeater-Steuerelement. Öffnen Sie zunächst die `Sorting.aspx` auf der Seite die `PagingSortingDataListRepeater` Ordner. Fügen Sie ein Repeater-Steuerelement auf der Webseite festlegen die `ID` Eigenschaft `SortableProducts`. Erstellen Sie eine neue, mit dem Namen "ObjectDataSource" aus dem Repeater-s-Smarttag `ProductsDataSource` und konfigurieren Sie ihn zum Abrufen von Daten aus der `ProductsBLL` Klasse s `GetProducts()` Methode. Wählen Sie die option (keine) aus den Dropdown-Listen auf den Registerkarten INSERT-, Update- und DELETE.


[![Erstellen Sie ein ObjectDataSource-Steuerelement, und konfigurieren Sie, dass die GetProductsAsPagedDataSource()-Methode verwendet](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Abbildung 1**: ein ObjectDataSource-Steuerelement zu erstellen und konfigurieren Sie sie so verwenden die `GetProductsAsPagedDataSource()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Abbildung 2**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


Im Gegensatz zu mit DataList-Steuerelement, Visual Studio erstellt nicht automatisch ein `ItemTemplate` für das Repeater-Steuerelement nach dem Element an eine Datenquelle gebunden wird. Darüber hinaus müssen wir Hinzufügen dieser `ItemTemplate` deklarativ an, wie die Smarttags des Repeater-Steuerelements s verfügt nicht über die die Vorlagen bearbeiten-Option im DataList-Steuerelement s gefunden. Let-s verwenden die gleiche `ItemTemplate` aus dem vorherigen Lernprogramm aus, die den Produktnamen s, Lieferanten und Kategorie angezeigt.

Nach dem Hinzufügen der `ItemTemplate`, Repeater- und das "ObjectDataSource" s deklarative Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Abbildung 3 zeigt diese Seite, wenn Sie über einen Browser angezeigt.


[![Jedes Produkt s Name, Hersteller und Kategorie wird angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Abbildung 3**: jedes Produkt s Name, Lieferanten und Kategorie angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Schritt 3: Weist dem ObjectDataSource-Steuerelement zum Sortieren der Daten

Um die im Wiederholungsmodul angezeigten Daten zu sortieren, müssen wir dem ObjectDataSource-Steuerelement des Sortierausdrucks zu informieren, mit dem die Daten sortiert werden sollen. Vor dem ObjectDataSource-Steuerelement seine Daten abruft, löst es zuerst die [ `Selecting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), die bietet einer Möglichkeit, Angeben eines Sortierungsausdrucks. Die `Selecting` -Ereignishandler wird ein Objekt des Typs übergeben [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), die besitzt eine Eigenschaft namens [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) des Typs [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). Die `DataSourceSelectArguments` Klasse dient zum datenbezogene Anforderungen von einem Consumer der Daten an das Datenquellen-Steuerelement zu übergeben, und enthält eine [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Um Sortierinformationen von der ASP.NET-Seite zu dem ObjectDataSource-Steuerelement zu übergeben, erstellen Sie einen Ereignishandler für die `Selecting` Ereignisses und verwenden Sie den folgenden Code:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Die *SortExpression* Wert den Namen des Felds zum Sortieren der Daten durch (z. B. ProductName) zugewiesen werden soll. Keine Richtung bezogene Sort-Eigenschaft vorhanden ist, also wenn Sie die Daten in absteigender Reihenfolge sortieren möchten, fügen Sie der Zeichenfolge DESC, die *SortExpression* Wert (z. B. ProductName DESC).

Fahren Sie fort, und versuchen Sie es einige andere hartcodierte Werte für *SortExpression* und die Ergebnisse in einem Browser zu testen. Wie Abbildung 4 zeigt die Verwendung von ProductName DESC als, die *SortExpression*, werden die Produkte anhand ihres Namens in umgekehrter alphabetischer Reihenfolge sortiert.


[![Die Produkte werden anhand ihres Namens in die umgekehrte alphabetische Reihenfolge sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Abbildung 4**: die Produkte werden durch ihren Namen in die umgekehrte alphabetische Reihenfolge sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Schritt 4: Erstellen der Schnittstelle sortieren und speichern den Sortierungsausdruck und die Richtung

Jede Headertext sortierbar Feld s einschalten, sortieren die Unterstützung in den GridView-Ansicht konvertiert werden, in ein LinkButton, wenn angeklickt, sortiert die Daten entsprechend. Eine Sortierung Schnittstelle ist sinnvoll für GridView, in dem ihre Daten sauber in Spalten angeordnet werden. Für die DataList- oder Repeater-Steuerelemente ist jedoch eine andere Sortierung Schnittstelle erforderlich. Eine Sortierung allgemeine Schnittstelle für eine Liste der Daten (im Gegensatz zu einem Datenraster), wird eine Dropdown-Liste, die verfügt über die Felder, die mit dem die Daten sortiert werden können. Lassen Sie s, die für dieses Tutorial derartige Schnittstelle implementiert.

Fügen Sie ein DropDownList-Steuerelement oben die `SortableProducts` Repeater, und legen seine `ID` Eigenschaft `SortBy`. Klicken Sie im Eigenschaftenfenster auf die Auslassungspunkte in der `Items` Eigenschaft, um den ListItem-Auflistungs-Editor zu öffnen. Hinzufügen `ListItem` s zum Sortieren der Daten durch die `ProductName`, `CategoryName`, und `SupplierName` Felder. Hinzufügen einer `ListItem` die Produkte anhand ihres Namens in umgekehrter alphabetischer Reihenfolge sortiert.

Die `ListItem` `Text` Eigenschaften können festgelegt werden, um einen beliebigen Wert (z. B. Name), aber die `Value` Eigenschaften müssen festgelegt werden, auf den Namen des Felds (z. B. ProductName). Um die Ergebnisse in absteigender Reihenfolge sortieren zu können, fügen Sie der Zeichenfolge DESC, der den Datenfeldnamen wie ProductName DESC.


![Fügen Sie ein ListItem für jeden der sortierbare Datenfelder hinzu.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Abbildung 5**: Hinzufügen einer `ListItem` für die einzelnen Datenfelder, sortierbar


Abschließend fügen Sie ein Steuerelement Schaltfläche rechts neben der Dropdownliste hinzu. Festlegen der `ID` zu `RefreshRepeater` und die zugehörige `Text` Eigenschaft zu aktualisieren.

Nach dem Erstellen der `ListItem` s und die Schaltfläche "Aktualisieren" hinzugefügt haben, sollte die DropDownList und Schaltfläche s deklarative Syntax des etwa wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Mit der Sortierung DropDownList-Steuerelement abgeschlossen, wir als Nächstes müssen die "ObjectDataSource"-s aktualisieren `Selecting` -Ereignishandler so, dass die It die ausgewählte verwendet `SortBy``ListItem` s `Value` Eigenschaft im Gegensatz zu einem hartcodierten Sortierungsausdruck.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

An diesem Punkt aus, wenn die Seite zuerst besuchen werden die Produkte zunächst nach sortiert werden die `ProductName` Feld "Daten", wie sie s der `SortBy` `ListItem` standardmäßig aktiviert (siehe Abbildung 6). Auswählen eines anderen Option wie z. B. Kategorie sortieren und auf Aktualisieren klicken, wird dazu führen, dass einen Postback und neu sortieren der Daten durch den Namen der Kategorie, wie in Abbildung 7 dargestellt.


[![Die Produkte sind zunächst nach dem Namen sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Abbildung 6**: die Produkte sind zunächst anhand ihres Namens sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Die Produkte sind jetzt nach Kategorie sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Abbildung 7**: die Produkte sind jetzt nach Kategorie sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Klicken Sie auf die Schaltfläche "Aktualisieren" bewirkt, dass die Daten automatisch neu sortiert sein, da der Repeater-s-Ansichtszustand deaktiviert wurde, wodurch des Repeaters, um mit der Datenquelle bei jedem Postback erneut zu binden. Wenn Sie haben den Repeater s Ansichtszustand aktiviert ist, ändern die Sortierung Dropdownliste links Liste gewonnen t wirken sich alle auf die Sortierreihenfolge. Um dies zu beheben, erstellen Sie einen Ereignishandler für die Schaltfläche "Aktualisieren"-s `Click` Ereignis- und Binden des Repeaters mit der Datenquelle (durch Aufrufen des Repeaters s `DataBind()` Methode).


## <a name="remembering-the-sort-expression-and-direction"></a>Denken Sie daran, den Sortierungsausdruck und die Richtung

Wenn Sie eine sortierbare DataList- oder Repeater auf einer Seite erstellen, bei denen nicht sortieren Postbacks auftreten verknüpfte, es s unverzichtbar, dass der Sortierungsausdruck und die Richtung über Postbacks hinweg gespeichert werden. Angenommen Sie, dass wir in diesem Lernprogramm sollen eine Löschen-Schaltfläche mit den einzelnen Produkten Repeater aktualisiert. Klickt der Benutzer die Schaltfläche "Delete" führen wir d von Code zum Löschen des ausgewählten Produkts aus, und binden Sie dann die Daten der Repeater. Wenn Sie die Details für die Sortierung für Postbacks nicht beibehalten werden, werden die Daten auf dem Bildschirm angezeigt, die ursprüngliche Sortierreihenfolge zurückgesetzt.

In diesem Tutorial speichert DropDownList implizit die Sortierung Ausdruck und die Richtung in seinen Ansichtszustand für uns. Wenn wir eine andere Sortierung Schnittstelle einer mit, z. B. LinkButtons verwendet haben, die die verschiedenen Optionen für die Sortierung bereitgestellt müssen wir d darauf achten, dass um die Sortierreihenfolge über Postbacks hinweg zu speichern. Dies kann erreicht werden, durch die Sortierung Parameter in Ansichtszustand der Seite s, speichern, indem Sie den sortierungsparameter einschließen, in der Abfragezeichenfolge oder über ein anderes State Persistence-Verfahren.

Zukünftige Beispiele in diesem Tutorial erfahren Sie, wie die Sortierung Details in den Ansichtszustand der Seite s beibehalten werden.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Schritt 5: Hinzufügen einer Sortierung unterstützen, einem DataList-Steuerelement, das das Standardpaging verwendet

In der [vorherigen Lernprogramm](paging-report-data-in-a-datalist-or-repeater-control-cs.md) wurde beschrieben, wie das Standardpaging und einem DataList-Steuerelement zu implementieren. Lassen Sie s, die diese vorherigen Beispiel um die Möglichkeit zum Sortieren der ausgelagerten Daten zu erweitern. Öffnen Sie zunächst die `SortingWithDefaultPaging.aspx` und `Paging.aspx` Seiten in der `PagingSortingDataListRepeater` Ordner. Von der `Paging.aspx` Seite, klicken Sie auf die Schaltfläche "Quelle" im deklarativen Markup der Seite s anzeigen. Kopieren Sie den ausgewählten Text (siehe Abbildung 8), und fügen Sie ihn in deklarativen Markup `SortingWithDefaultPaging.aspx` zwischen der `<asp:Content>` Tags.


[![Replizieren von deklarativen Markup in der &lt;Asp: Content&gt; Tags aus Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Abbildung 8**: Replizieren von deklarativen Markup in der `<asp:Content>` von Tags `Paging.aspx` zu `SortingWithDefaultPaging.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Kopieren Sie nach dem Kopieren der deklarativen Markups an, die Methoden und Eigenschaften in der `Paging.aspx` Seite s CodeBehind-Klasse, um die Code-Behind-Klasse für `SortingWithDefaultPaging.aspx`. Als Nächstes nehmen einen Moment Zeit, an die `SortingWithDefaultPaging.aspx` Seite in einem Browser. Es sollte weisen die gleiche Funktionalität und die Darstellung als `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Verbessern der ProductsBLL den Standardwert Auslagern und sortieren die Methode einschließen

Im vorherigen Tutorial erstellt es einen `GetProductsAsPagedDataSource(pageIndex, pageSize)` -Methode in der die `ProductsBLL` -Klasse, die zurückgegeben eine `PagedDataSource` Objekt. Dies `PagedDataSource` mit gefüllt wurde *alle* der Produkte (über die BLL s `GetProducts()` Methode), aber bei der Datenbindung DataList-Steuerelement nur die Datensätze, die der angegebenen *PageIndex* und *PageSize* Eingabeparameter werden angezeigt.

Weiter oben in diesem Tutorial wir Unterstützung der datenquellensortierung hinzugefügt, durch Angabe den Sortierausdruck, von der "ObjectDataSource"-s `Selecting` -Ereignishandler. Dies funktioniert gut, wenn dem ObjectDataSource-Steuerelement ein Objekt zurückgegeben wird, die, wie z. B. sortiert werden kann, die `ProductsDataTable` zurückgegebenes der `GetProducts()` Methode. Allerdings die `PagedDataSource` zurückgegebenes Objekt der `GetProductsAsPagedDataSource` Methode unterstützt nicht die Sortierung von der internen Datenquelle. Stattdessen müssen wir die zurückgegebenen Ergebnisse zu sortieren die `GetProducts()` Methode *vor* wir fügen Sie ihn in das `PagedDataSource`.

Um dies zu erreichen, erstellen Sie eine neue Methode in der `ProductsBLL` -Klasse, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Zum Sortieren der `ProductsDataTable` zurückgegebenes der `GetProducts()` -Methode, geben Sie die `Sort` Eigenschaft den Standardwert `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Die `GetProductsSortedAsPagedDataSource` Methode unterscheidet sich nur geringfügig von den `GetProductsAsPagedDataSource` Methode im vorherigen Tutorial erstellt haben. Insbesondere `GetProductsSortedAsPagedDataSource` akzeptiert einen zusätzlichen Eingabeparameter `sortExpression` und weist diesen Wert auf die `Sort` Eigenschaft der `ProductDataTable` s `DefaultView`. Ein paar Zeilen Code später die `PagedDataSource` s DataSource-Objekt zugewiesen wird die `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Aufrufen der Methode GetProductsSortedAsPagedDataSource und Angeben des Werts für die SortExpression Input-Parameter

Mit der `GetProductsSortedAsPagedDataSource` Methode vollständig ist, der nächste Schritt besteht, um den Wert für diesen Parameter bereitzustellen. Zu "ObjectDataSource" `SortingWithDefaultPaging.aspx` ist zurzeit so konfiguriert, dass das Aufrufen der `GetProductsAsPagedDataSource` -Methode auf und übergibt die beiden Eingabeparameter durch die zwei `QueryStringParameters`, im angegebenen die `SelectParameters` Auflistung. Diese beiden `QueryStringParameters` anzugeben, die die Quelle für die `GetProductsAsPagedDataSource` s-Methode *PageIndex* und *PageSize* Parameter stammen vom Querystring-Felder `pageIndex` und `pageSize`.

Aktualisieren Sie das "ObjectDataSource"-s `SelectMethod` Eigenschaft, sodass die It die neuen ruft `GetProductsSortedAsPagedDataSource` Methode. Fügen Sie dann ein neues `QueryStringParameter` , damit die *SortExpression* Eingabeparameter wird aus dem Querystring-Feld zugegriffen `sortExpression`. Legen Sie die `QueryStringParameter` s `DefaultValue` zu ProductName.

Nach diesen Änderungen sollte das deklarative "ObjectDataSource"-s-Markup ähneln:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

An diesem Punkt die `SortingWithDefaultPaging.aspx` Seite werden die Ergebnisse alphabetisch sortieren nach dem Produktnamen (siehe Abbildung 9). Dies liegt daran, die standardmäßig ein Wert von ProductName in übergeben wird, als die `GetProductsSortedAsPagedDataSource` s-Methode *SortExpression* Parameter.


[![Standardmäßig werden die Ergebnisse nach Produktname sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Abbildung 9**: standardmäßig, werden die Ergebnisse nach sortiert `ProductName` ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Wenn Sie manuell hinzufügen einer `sortExpression` Querystring-Feld, z. B. `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` die Ergebnisse sortiert werden durch das angegebene `sortExpression`. Aber dies `sortExpression` Parameter ist nicht in der Abfragezeichenfolge enthalten, bei der Umstellung auf eine andere Seite der Daten. Damit die Schaltflächen rufen wir in erneut, klicken auf der nächsten oder letzten Seite `Paging.aspx`! Darüber hinaus gibt es s derzeit keine Sortierung Schnittstelle. Die einzige Möglichkeit, die ein Benutzer die Sortierreihenfolge der ausgelagerten Daten ändern kann, ist durch die Abfragezeichenfolge direkt bearbeiten.

## <a name="creating-the-sorting-interface"></a>Erstellen die Sortierung-Schnittstelle

Müssen wir zuerst auf das Aktualisieren der `RedirectUser` Methode, um den Benutzer senden `SortingWithDefaultPaging.aspx` (anstelle von `Paging.aspx`) und um die `sortExpression` Wert in der Abfragezeichenfolge. Wir sollten auch eine schreibgeschützte, auf Seitenebene mit dem Namen hinzufügen `SortExpression` Eigenschaft. Diese Eigenschaft, die ähnlich wie die `PageIndex` und `PageSize` Eigenschaften, die im vorherigen Tutorial erstellt haben, gibt den Wert des der `sortExpression` Querystring-Feld, falls vorhanden, und der standardmäßige Wert (ProductName) andernfalls.

Derzeit den `RedirectUser` -Methode akzeptiert nur einen einzelnen Eingabeparameter der Index der anzuzeigenden Seite. Allerdings gibt es möglicherweise vorkommen, dass wir den Benutzer an eine bestimmte Seite der Daten, die mithilfe eines Sortierungsausdrucks als Neuheiten in der Abfragezeichenfolge angegebene umleiten möchten. In Kürze erstellen wir die Sortierung-Schnittstelle für diese Seite, die eine Reihe von Websteuerelementen mit Schaltfläche zum Sortieren der Daten durch eine angegebene Spalte enthält. Wenn eine dieser Schaltflächen geklickt wird, möchten wir zum Umleiten des Benutzers, der in der entsprechenden Sortierausdruckswert übergeben. Um diese Funktionalität zu ermöglichen, erstellen Sie zwei Versionen der `RedirectUser` Methode. Erstens sollte nur den Seitenindex angezeigt werden, während das zweite Argument den Seite Index und die Sortierreihenfolge Ausdruck akzeptiert, akzeptieren.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Im ersten Beispiel in diesem Tutorial haben wir eine Sortierung Schnittstelle, die mit einem DropDownList-Steuerelement erstellt. In diesem Beispiel verwenden können s drei Schaltfläche Websteuerelemente über DataList-Steuerelement eine positioniert wird, für die Sortierung von `ProductName`, eine für `CategoryName`, und eine für `SupplierName`. Die drei Schaltfläche Web Steuerelemente hinzufügen, Festlegen ihrer `ID` und `Text` Eigenschaften entsprechend:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Als Nächstes erstellen Sie eine `Click` -Ereignishandler für die einzelnen. Die Ereignishandler aufrufen sollten die `RedirectUser` Methode, die den Benutzer auf die erste Seite mit den entsprechenden Sortierungsausdruck zurückgeben.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Wenn die Seite zuerst besuchen zu können, die Daten nach dem Produktnamen alphabetisch sortiert (siehe Abbildung 9). Klicken Sie auf die Schaltfläche "Weiter", fahren Sie mit der zweiten Seite der Daten fort, und klicken Sie dann auf die Sortierung von kategorieschaltfläche. Dies gibt uns zur ersten Seite der Daten, sortiert nach Kategorienamen (siehe Abbildung 10). Ebenso sortiert die Sortierung von Lieferanten-Schaltfläche auf die Daten vom Lieferanten, die von der ersten Seite der Daten ab. Die Sortierungsoption wird gespeichert, wie die Daten über ausgelagert werden. Abbildung 11 zeigt die Seite nach dem Sortieren nach Kategorie, und klicken Sie dann das dreizehnte Datenseite gelangt sind.


[![Die Produkte sind nach Kategorien sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Abbildung 10**: sind die Produkte nach Kategorie geordnet ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Der Sortierausdruck wird gespeichert, wenn Paging durch die Daten](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Abbildung 11**: der Sortierausdruck wird gespeichert, wenn Paging durch die Daten ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Schritt 6: Benutzerdefiniertes Paging durch die Datensätze in einem Wiederholungssteuerelement

Das DataList-Beispiel, die in Schritt 5 Seiten über die Daten mithilfe der ineffizienten standardmäßig Paging Technik überprüft. Beim paging durch ausreichend große Mengen von Daten, ist es zwingend erforderlich, dass das benutzerdefinierte Paging verwendet werden. In der [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) und [Sortieren von benutzerdefinierten ausgelagerten Daten](../paging-and-sorting/sorting-custom-paged-data-cs.md) Tutorials, untersuchten wir die Unterschiede zwischen Standard- und benutzerdefinierte Paginierung und erstellten Methoden in der BLL für verwenden benutzerdefinierte Auslagern und Sortieren von benutzerdefinierten ausgelagerten Daten. In diesen beiden vorherigen Tutorials hinzugefügt wir vor allem die folgenden drei Methoden für die `ProductsBLL` Klasse:

- `GetProductsPaged(startRowIndex, maximumRows)` Gibt eine bestimmte Teilmenge der Datensätze, die beginnend bei *StartRowIndex* und nicht länger als *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Gibt eine bestimmte Teilmenge der Datensätze, sortiert nach dem angegebenen *SortExpression* input-Parameters.
- `TotalNumberOfProducts()` Gibt die Gesamtzahl der Datensätze in der `Products` Datenbanktabelle.

Diese Methoden können verwendet werden, effizient Seite und Sortieren von Daten mithilfe eines DataList- oder Repeater-Steuerelements. Um dies zu veranschaulichen, können Sie zunächst erstellen Sie ein Repeater-Steuerelement mit Unterstützung für benutzerdefiniertes Paging s; Wir fügen dann Sortierfunktionen.

Öffnen der `SortingWithCustomPaging.aspx` auf der Seite der `PagingSortingDataListRepeater` Ordner und fügen Sie einem Wiederholungssteuerelement auf der Seite festlegen seiner `ID` Eigenschaft `Products`. Erstellen Sie eine neue, mit dem Namen "ObjectDataSource" aus dem Repeater-s-Smarttag `ProductsDataSource`. So konfigurieren, dass die Daten aus der wählen die `ProductsBLL` Klasse s `GetProductsPaged` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL Klasse s GetProductsPaged-Methode](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Abbildung 12**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` s-Klasse `GetProductsPaged` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine), und klicken Sie dann auf die Schaltfläche "Weiter". Der Konfigurieren von Datenquellen-Assistenten jetzt aufgefordert, für die Datenquellen der `GetProductsPaged` s-Methode *StartRowIndex* und *MaximumRows* Eingabeparameter. In Wirklichkeit werden diese Parameter ignoriert. Stattdessen die *StartRowIndex* und *MaximumRows* Werte durch Übergeben der `Arguments` -Eigenschaft in der "ObjectDataSource"-s `Selecting` -Ereignishandler, wie wir angegeben wie die *SortExpression* in dieser ersten Tutorial s-Demo. Aus diesem Grund lassen Sie die Parameterquelle Dropdownlisten im Assistenten auf None festgelegt.


[![Lassen Sie den Parametersatz für die Datenquellen auf "None"](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Abbildung 13**: lassen Sie die Quellen Parametersatz auf "None" ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Führen Sie *nicht* legen Sie das "ObjectDataSource"-s `EnablePaging` Eigenschaft `true`. Dadurch wird dem ObjectDataSource-Steuerelement automatisch einen eigenen aufnehmen *StartRowIndex* und *MaximumRows* Parameter für die `SelectMethod` Liste vorhandenen s-Parameter. Die `EnablePaging` Eigenschaft ist nützlich, bei der Bindung benutzerdefinierte Daten zu einem GridView, DetailsView oder FormView-Steuerelement ausgelagert, da diese Steuerelemente bestimmte Verhalten aus dem ObjectDataSource-Steuerelement, s erwarten, dass nur verfügbar, wenn `EnablePaging` Eigenschaft `true`. Lassen Sie diese Eigenschaft auf festgelegt, da wir haben die pagingunterstützung für DataList- oder Wiederholungssteuerelement manuell hinzufügen, `false` (Standardeinstellung), wie wir in die benötigte Funktionalität direkt in unser ASP.NET-Seite integrieren müssen.


Definieren Sie schließlich die Repeater s `ItemTemplate` , damit der Produktname s, Kategorie und Lieferant angezeigt werden. Nach diesen Änderungen sollte die Repeater- und das "ObjectDataSource" s deklarative Syntax des etwa wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Nehmen Sie einen Moment Zeit, besuchen die Seite über einen Browser, und beachten Sie, dass keine Datensätze zurückgegeben werden. Grund hierfür ist, wir haben noch an die *StartRowIndex* und *MaximumRows* Parameterwerte; aus diesem Grund werden Werte von 0 an übergeben wird, für beide. Um diese Werte anzugeben, erstellen Sie einen Ereignishandler für das "ObjectDataSource"-s `Selecting` -Ereignis und legen Sie diese Parameter Werte programmgesteuert hart kodierte Werte von 0 und 5, bzw.:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Durch diese Änderung zeigt die Seite, wenn Sie über einen Webbrowser angezeigt. die ersten fünf Produkte.


[![Die ersten fünf Datensätze werden angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Abbildung 14**: die ersten fünf Datensätze angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Die Produkte aufgelistet, die in Abbildung 14 ausgeführt wird, nach dem Produktnamen sortiert werden, da die `GetProductsPaged` gespeicherte Prozedur, die die effiziente benutzerdefinierte Pagingabfrage ausführt, sortiert die Ergebnisse nach `ProductName`.


Damit den Benutzer die Seiten durchlaufen zu können, müssen wir verfolgt den Startindex für die Zeile und der maximalen Anzahl von Zeilen, und speichern diese Werte über mehrere Postbacks. In der Standard-Paging-Beispiel verwendet haben wir Querystring-Felder, um diese Werte beizubehalten; Lassen Sie für diese Demo s, die diese Informationen in den Ansichtszustand der Seite s beibehalten werden. Erstellen Sie die folgenden zwei Eigenschaften:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Als Nächstes den Code im Ereignishandler auswählen aktualisieren, sodass er verwendet den `StartRowIndex` und `MaximumRows` Eigenschaften anstelle der hartcodierten Werte von 0 und 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

An diesem Punkt zeigt die Seite immer noch nur die ersten fünf Datensätze. Allerdings mit den folgenden Eigenschaften vorhanden, es erneut bereit, um die Paging-Schnittstelle zu erstellen.

## <a name="adding-the-paging-interface"></a>Hinzufügen von Paging-Schnittstelle

Let-s verwenden, die die gleichen, zurück und Weiter, die zuletzt paging Schnittstelle verwendet standardmäßig Paging beispielsweise einschließlich das Bezeichnung-Web-Steuerelement, das zeigt, an welche Seite der Daten angezeigt wird und wie viele Gesamtanzahl der Seiten vorhanden sind. Fügen Sie die vier Schaltfläche Websteuerelemente und die Bezeichnung unterhalb des Repeaters hinzu.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Erstellen Sie als Nächstes `Click` -Ereignishandlern für die vier Schaltflächen. Wenn eine dieser Schaltflächen geklickt wird, müssen wir aktualisieren den `StartRowIndex` und binden Sie die Daten der Repeater. Ist der Code für die erste zurück und Weiter Schaltflächen recht einfach, aber für die letzte Schaltfläche wie feststellen wir, den Startindex für die Zeile für die letzte Seite der Daten? Zum Berechnen dieser Index als auch bestimmen, ob die Schaltflächen Weiter und letzten aktiviert werden soll, müssen wir wissen, wie viele Datensätze insgesamt über ausgelagert werden können. Wir können dies ermitteln, durch den Aufruf der `ProductsBLL` Klasse s `TotalNumberOfProducts()` Methode. Let s erstellen Sie eine schreibgeschützte, auf Seitenebene-Eigenschaft, die mit dem Namen `TotalRowCount` , die die Ergebnisse zurückgibt, die `TotalNumberOfProducts()` Methode:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Mit dieser Eigenschaft können wir nun der Zeilenindex der letzten Seite s Start bestimmen. Insbesondere, s, das ganzzahlige Ergebnis von der `TotalRowCount` minus 1 dividiert durch `MaximumRows`multipliziert mit `MaximumRows`. Wir können nun die `Click` -Ereignishandlern für die vier Pagingschaltflächen Interface:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Abschließend müssen wir die erste ' und ' Vorheriger Schaltflächen in den Paging-Schnittstelle zu deaktivieren, wenn es sich bei die erste Seite der Daten und die Schaltflächen Weiter und letzten anzeigen, wenn die letzte Seite anzeigen. Um dies zu erreichen, fügen Sie den folgenden Code, der "ObjectDataSource"-s `Selecting` -Ereignishandler:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Nach dem Hinzufügen dieser `Click` -Ereignishandler und den Code zum Aktivieren oder deaktivieren die Elemente der Benutzeroberfläche für Paging wird basierend auf der aktuellen Zeile startIndex, testen Sie die Seite in einem Browser. Abbildung 15 veranschaulicht, wenn die erste Seite zuerst besuchen und werden der zurück-Schaltflächen sind deaktiviert. Klicken Sie auf Weiter die zweite Seite der Daten zeigt, während die letzte Seite auf der letzten angezeigt werden (siehe Abbildung 16 und 17). Beim Anzeigen von Daten der letzten Seite werden die Schaltflächen "Weiter" und "letzte deaktiviert.


[![Die Schaltflächen zurück und letzten sind deaktiviert, wenn die erste Seite der Produkte anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Abbildung 15**: die vorherigen und den letzten Schaltflächen sind deaktiviert, wenn die erste Seite der Produkte anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Die zweite Seite des Produkte sind fest](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Abbildung 16**: der zweiten Seite des Produkte sind fest ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Klicken Sie auf letzte zeigt die letzte Seite der Daten](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Abbildung 17**: Klicken Sie auf letzte zeigt die letzte Seite der Daten ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Schritt 7: Einschließlich Sortierung unterstützt, mit dem benutzerdefinierten Repeater ausgelagerten

Nun, da benutzerdefinierte Paginierung implementiert wurde, unterstützen wir erneut bereit, die Sortierung einschließen. Die `ProductsBLL` Klasse s `GetProductsPagedAndSorted` Methode hat die gleiche *StartRowIndex* und *MaximumRows* Eingabeparameter als `GetProductsPaged`, jedoch ermöglicht eine zusätzliche  *SortExpression* input-Parameters. Verwenden der `GetProductsPagedAndSorted` aus `SortingWithCustomPaging.aspx`, müssen wir die folgenden Schritte ausführen:

1. Ändern Sie das "ObjectDataSource"-s `SelectMethod` Eigenschaft `GetProductsPaged` zu `GetProductsPagedAndSorted`.
2. Hinzufügen einer *SortExpression* `Parameter` Objekt, das "ObjectDataSource"-s `SelectParameters` Auflistung.
3. Erstellen Sie eine Private, auf Seitenebene `SortExpression` -Eigenschaft, die den Wert über den Ansichtszustand der Seite s postbackübergreifend beibehalten.
4. Aktualisieren Sie das "ObjectDataSource"-s `Selecting` -Ereignishandler für das Zuweisen von "ObjectDataSource" s *SortExpression* Parameter den Wert des auf Seitenebene `SortExpression` Eigenschaft.
5. Erstellen Sie die Sortierung-Schnittstelle.

Starten mit dem Aktualisieren der "ObjectDataSource"-s `SelectMethod` -Eigenschaft und das Hinzufügen einer *SortExpression* `Parameter`. Stellen Sie sicher, dass die *SortExpression* `Parameter` s `Type` -Eigenschaftensatz auf `String`. Nach Abschluss dieser ersten beiden Aufgaben an, sollte "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Als Nächstes benötigen wir ein auf Seitenebene `SortExpression` Eigenschaft, deren Wert dem Ansichtszustand serialisiert wird. Wenn keine Sortierausdruckswert festgelegt wurde, können verwenden Sie ProductName als Standard:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Vor dem ObjectDataSource-Steuerelement ruft die `GetProductsPagedAndSorted` Methode wir legen müssen die *SortExpression* `Parameter` auf den Wert des der `SortExpression` Eigenschaft. In der `Selecting` Ereignishandler, fügen Sie die folgende Codezeile hinzu:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Alle diese bleibt die Sortierung-Schnittstelle implementiert werden. Wie im letzten Beispiel können Sie die s-Sortierung Schnittstelle mithilfe von drei Schaltfläche Websteuerelemente, mit denen den Benutzer zum Sortieren der Ergebnisse nach Produktname, Kategorie oder Lieferant implementiert.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Erstellen Sie `Click` -Ereignishandlern für diese drei Schaltflächen-Steuerelemente. Im Handler, Zurücksetzen der `StartRowIndex` auf 0 (null) festgelegt die `SortExpression` auf den entsprechenden Wert ein, und binden Sie die Daten des Repeaters:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Alles, was, s wird es! Zwar gab es eine Reihe von Schritten zum Abrufen von benutzerdefiniertem Paging und Sortierung implementiert, wurden die Schritte für das Standardpaging sehr ähnlich. Abbildung 18 zeigt die Produkte an, beim Anzeigen der letzten Seite der Daten nach Kategorie sortiert.


[![Die letzte Seite der Daten, sortiert nach Kategorie wird angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Abbildung 18**: die letzte Seite der Daten, sortiert nach Kategorie angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> In den vorherigen Beispielen, beim Sortieren von vom Lieferanten, die als Sortierausdruck Lieferantenname verwendet wurde. Für die benutzerdefinierte Implementierung der Paginierung müssen wir jedoch CompanyName verwenden. Grund hierfür ist die gespeicherte Prozedur, die verantwortlich für das Implementieren von benutzerdefinierten Paging `GetProductsPagedAndSorted` übergibt den Sortierungsausdruck in der `ROW_NUMBER()` -Schlüsselwort, das `ROW_NUMBER()` Schlüsselwort erfordert einen Alias, sondern den tatsächlichen Spaltennamen. Aus diesem Grund müssen wir verwenden `CompanyName` (der Name der Spalte in der `Suppliers` Tabelle) anstatt der Alias verwendet, der `SELECT` Abfrage (`SupplierName`) für den Sortierungsausdruck.


## <a name="summary"></a>Zusammenfassung

Weder das DataList oder Repeater bieten integrierte Unterstützung der datenquellensortierung, aber mit ein wenig Code und eine benutzerdefinierte Sortierung-Schnittstelle, eine solche Funktionalität hinzugefügt werden kann. Beim Sortieren, aber kein paging zu implementieren, kann der Sortierungsausdruck angegeben werden, über die `DataSourceSelectArguments` -Objekt übergeben, in der "ObjectDataSource"-s `Select` Methode. Dies `DataSourceSelectArguments` s-Objekt `SortExpression` Eigenschaft kann zugewiesen werden, in das "ObjectDataSource"-s `Selecting` -Ereignishandler.

Um einem DataList- oder Repeater, die bereits Unterstützung der Paginierung bietet Sortierfunktionen hinzuzufügen, ist der einfachste Ansatz zum Anpassen der Business Logic Layer, um eine Methode enthalten, die einen Sortierungsausdruck akzeptiert. Diese Informationen können Sie dann im übergeben werden, über einen Parameter in der "ObjectDataSource"-s `SelectParameters`.

Dieses Tutorial ist abgeschlossen, unsere Untersuchung der Paginierung und Sortierung mit dem DataList- und Wiederholungssteuerelement-Steuerelementen. Unser Tutorial zum nächsten und letzten untersucht wie Vorlagen s DataList- oder Wiederholungssteuerelement Websteuerelemente Schaltfläche hinzugefügt, um einige Funktionen von benutzerdefinierten, vom Benutzer initiierte pro pro-Element bereitzustellen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist David Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Weiter](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
