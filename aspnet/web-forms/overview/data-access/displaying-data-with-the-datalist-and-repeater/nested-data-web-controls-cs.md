---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Geschachtelte Web-Steuerelemente (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Lernprogramm aus, die wir untersuchen werden mit einem Wiederholungssteuerelement in einer anderen Repeater geschachtelt. In den Beispielen werden dem inneren Repeater beide d Auffüllen veranschaulicht...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fdac16f3f3460e75887aa0f4b4540da1d2227c88
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368378"
---
<a name="nested-data-web-controls-c"></a>Geschachtelte Datenwebsteuerelemente (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) oder [PDF-Datei herunterladen](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> In diesem Lernprogramm aus, die wir untersuchen werden mit einem Wiederholungssteuerelement in einer anderen Repeater geschachtelt. Den Beispielen werden wie den inneren Repeater deklarativ und programmgesteuert aufgefüllt werden.


## <a name="introduction"></a>Einführung

Zusätzlich zu statischem HTML-Code und die Databinding-Syntax können Vorlagen auch Web-Steuerelemente und Benutzersteuerelemente einschließen. Diese Web-Steuerelemente können ihre Eigenschaften verfügen über die Datenbindungssyntax der deklarativen zugewiesen oder in die entsprechenden Ereignishandler für die serverseitige programmgesteuert zugegriffen werden.

Durch Einbetten von Steuerelementen in einer Vorlage, kann die Darstellung und benutzerfreundlichkeit angepasst und verbessert werden. Z. B. in der [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Tutorial wurde erläutert, wie die GridView-s-Anzeige anpassen, indem Sie ein Kalender-Steuerelement hinzufügen, in ein TemplateField ein Mitarbeiter Einstellungsdatum s angezeigt, in der [hinzufügen Validierungssteuerelemente zum Bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) und [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Tutorials erläutert, wie Sie die Bearbeitung anpassen und Einfügen von Schnittstellen durch Hinzufügen der Validierung Steuerelemente, Textfelder, DropDownList-Steuerelementen und anderen Websteuerelementen.

Vorlagen können auch andere Daten Websteuerelemente enthalten. Wir haben, also einem DataList-Steuerelement, das eine andere DataList-Steuerelement (oder Repeater oder GridView oder DetailsView und So weiter) innerhalb der Vorlagen enthält. Die Herausforderung bei der eine solche Schnittstelle gebunden werden die entsprechenden Daten auf die inneren Daten Websteuerelement. Ein paar unterschiedliche Ansätze stehen zur Verfügung, die im Bereich von deklarativen Optionen, die mit dem ObjectDataSource-Steuerelement zu programmgesteuerte.

In diesem Lernprogramm aus, die wir untersuchen werden mit einem Wiederholungssteuerelement in einer anderen Repeater geschachtelt. Der äußere Repeater enthält ein Element für jede Kategorie in der Datenbank anzeigen, Kategorie-s-Name und Beschreibung. Jedes Kategorieelement s in inneren Repeater zeigt Informationen zu jedem Produkt, die zu dieser Kategorie gehören (siehe Abbildung 1) in eine Liste mit Aufzählungszeichen. Unseren Beispielen werden den inneren Repeater sowohl deklaratives und Programmgesteuertes Auffüllen veranschaulicht.


[![Jede Kategorie sowie die Produkte, werden aufgeführt.](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Abbildung 1**: jede Kategorie sowie die Produkte, aufgeführt sind ([klicken Sie, um das Bild in voller Größe anzeigen](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Schritt 1: Erstellen der Liste der Kategorien

Wenn datenwebsteuerelemente erstellen eine Seite, die verwendet geschachtelt werden, ich es hilfreich zu entwerfen, erstellen, und das äußerste Web Datensteuerelement zuerst testen, ohne auch die inneren geschachtelten Steuerelements kümmern. Aus diesem Grund können Sie s starten, indem Sie die Schritte notwendig, einem Wiederholungssteuerelement zur Seite hinzuzufügen, die den Namen und eine Beschreibung für die einzelnen Kategorien auflistet.

Öffnen Sie zunächst die `NestedControls.aspx` auf der Seite die `DataListRepeaterBasics` Ordner und fügen Sie ein Repeater-Steuerelement auf der Seite festlegen seiner `ID` Eigenschaft `CategoryList`. Wählen Sie aus dem Smarttag Repeater s, zum Erstellen einer neuen, mit dem Namen "ObjectDataSource" `CategoriesDataSource`.


[![Name der neuen CategoriesDataSource "ObjectDataSource"](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Abbildung 2**: Benennen Sie die neue "ObjectDataSource" `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](nested-data-web-controls-cs/_static/image6.png))


Dem ObjectDataSource-Steuerelement so konfigurieren, dass er die Daten aus der zieht die `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der CategoriesBLL Klasse s GetCategories-Methode](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` s-Klasse `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](nested-data-web-controls-cs/_static/image9.png))


Zum Angeben der Repeater-s-Vorlage muss Inhalt wechseln zur Quellansicht, und geben Sie die deklarative Syntax manuell ein. Hinzufügen einer `ItemTemplate` , die den Kategorienamen s in zeigt eine `<h4>` Element und die Beschreibung der Kategorie s in einem Absatzelement (`<p>`). Darüber hinaus können s trennen Sie die einzelnen Kategorien durch eine horizontale Trennlinie (`<hr>`). Nach diesen Änderungen sollte Ihre Seite deklarativen Syntax enthalten, für das Repeater- und das ObjectDataSource-Steuerelement, das etwa wie folgt:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Abbildung 4 zeigt unseren Fortschritt, wenn Sie über einen Browser angezeigt.


[![Jede Kategorie s Name und Beschreibung aufgelistet ist getrennt durch eine horizontale Trennlinie](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Abbildung 4**: jede Kategorie-s-Name und Beschreibung aufgelistet ist, getrennt durch eine horizontale Trennlinie ([klicken Sie, um das Bild in voller Größe anzeigen](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Schritt 2: Hinzufügen geschachtelter Produkt Repeater

Die erste Methode untersucht verwendet eine "ObjectDataSource" in der äußeren Websteuerelement s `CategoryList` , gebunden an das interne Web-Steuerelement über seine `ItemTemplate` Eigenschaft. Die zweite Methode Zugriff auf die Daten über eine Methode in der ASP.NET Page-s-Code-Behind-Klasse. Diese Methode klicken Sie dann auf die inneren Daten Websteuerelement s gebunden werden kann `CategoryList` Eigenschaft über die Databinding-Syntax. Während die geschachtelten Benutzeroberfläche untersucht, die in diesem Tutorial einen Repeater, die in einem Wiederholungssteuerelement geschachtelt verwendet, können diese Techniken auf den anderen datenwebsteuerelementen erweitert werden.

Sie können schachteln, einen Repeater innerhalb einer GridView-Ansicht oder einer GridView-Ansicht in einem DataList-Steuerelement und so weiter. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Liz Shulok.


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Schritt 3: Binden von kategoriespezifische Produkte an die ProductsByCategoryList Repeater

Wenn Sie an dieser Stelle die Seite über einen Browser besuchen, Ihren Bildschirm wird genauso aussehen wie in Abbildung 4 da wir haben noch, um alle Daten an den Repeater zu binden. Es gibt einige Möglichkeiten, können wir ziehen Sie die entsprechenden Datensätze und diese an die Repeater, einige effizienter als andere binden. Das Hauptproblem hier erhält die entsprechenden Produkte für die angegebene Kategorie zurück.

Die Daten zum Binden an das interne Repeater-Steuerelement können entweder deklarativ über eine "ObjectDataSource" zugegriffen werden die `CategoryList` Repeater s `ItemTemplate`, oder programmgesteuert über die ASP.NET Seite s Code-Behind-Seite. Auf ähnliche Weise diese Daten gebunden werden können, den inneren Repeater entweder deklarativ - über die inneren Repeater s `DataSourceID` Eigenschaft oder mithilfe von deklarativer Datenbindungssyntax oder programmgesteuert durch Verweisen auf die innere Repeater in die `CategoryList` Repeater s `ItemDataBound` -Ereignishandler programmgesteuerten Festlegen der `DataSource` -Eigenschaft und Aufrufen von dessen `DataBind()` Methode. Lassen Sie s, die jeder dieser Ansätze zu untersuchen.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Zugreifen auf die Daten deklarativ mit ein ObjectDataSource-Steuerelement und die`ItemDataBound`-Ereignishandler

Seit wir verwendet haben, dem ObjectDataSource-Steuerelement ausführlich in dieser Reihe von Tutorials, die beste Wahl für den Zugriff auf Daten, für dieses Beispiel ist, bleiben Sie mit dem ObjectDataSource-Steuerelement. Die `ProductsBLL` -Klasse verfügt über eine `GetProductsByCategoryID(categoryID)` Methode, die Informationen zu diesen Produkten zurückgibt, die mit dem angegebenen gehören *`categoryID`*. Aus diesem Grund können wir ein "ObjectDataSource" zum Hinzufügen der `CategoryList` Repeater s `ItemTemplate` , und konfigurieren sie den Zugriff auf die Daten von dieser Klasse s-Methode.

Leider Repeater t ermöglichen die Vorlagen, über die Entwurfsansicht bearbeitet werden, daher wir die deklarative Syntax für das ObjectDataSource-Steuerelement manuell hinzufügen müssen. Die folgende Syntax stellt die `CategoryList` Repeater s `ItemTemplate` nach dem Hinzufügen dieses neuen "ObjectDataSource" (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Bei Verwendung des Ansatzes "ObjectDataSource" müssen wir legen die `ProductsByCategoryList` Repeater s `DataSourceID` Eigenschaft, um die `ID` von dem ObjectDataSource-Steuerelement (`ProductsByCategoryDataSource`). Außerdem Beachten Sie, dass unsere "ObjectDataSource" eine `<asp:Parameter>` Element, das angibt der *`categoryID`* -Wert, der an übergeben werden die `GetProductsByCategoryID(categoryID)` Methode. Aber wie wir diesen Wert angeben? Im Idealfall wir d in der Lage, legen Sie einfach die `DefaultValue` Eigenschaft der `<asp:Parameter>` Element mithilfe der Datenbindungssyntax, wie folgt:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Databinding-Syntax ist leider nur gültig in Steuerelemente, die eine `DataBinding` Ereignis. Die `Parameter` -Klasse fehlen eines Ereignisses und aus diesem Grund die oben aufgeführten Syntax ist nicht zulässig, und führt zu einem Laufzeitfehler.

Um diesen Wert festlegen, müssen wir erstellen einen Ereignishandler für die `CategoryList` Repeater s `ItemDataBound` Ereignis. Bedenken Sie, dass die `ItemDataBound` -Ereignis wird einmal für jedes Element an der Repeater gebunden ausgelöst. Daher jedes Mal, die dieses Ereignis wird, für die äußere Repeater ausgelöst können weisen wir die aktuelle `CategoryID` Wert der `ProductsByCategoryDataSource` "ObjectDataSource" s `CategoryID` Parameter.

Erstellen Sie einen Ereignishandler für die `CategoryList` Repeater s `ItemDataBound` -Ereignis mit den folgenden Code:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Dieser Ereignishandler startet, indem Sie sicherstellen, dass wir erneut Umgang mit einem statt der Kopfzeilen, Fußzeilen oder Trennzeichen Element Element. Als Nächstes, wir verweisen auf die tatsächliche `CategoriesRow` -Instanz, die nur mit dem aktuellen gebunden wurde `RepeaterItem`. Außerdem verweisen wir zu "ObjectDataSource" die `ItemTemplate` und weisen Sie die `CategoryID` Parameterwert, der die `CategoryID` des aktuellen `RepeaterItem`.

Mit diesem Ereignishandler die `ProductsByCategoryList` Repeater in den einzelnen `RepeaterItem` gebunden ist, um diese Produkte in der `RepeaterItem` s-Kategorie. Abbildung 5 zeigt einen Screenshot, der die resultierende Ausgabe.


[![Der äußere Repeater sind jeder Kategorie aufgeführt. das Innere einer Listet die Produkte für diese Kategorie](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Abbildung 5**: der äußere Repeater listet jede Kategorie %% amp; die innere eine Listen die Produkte für die Kategorie ([klicken Sie, um das Bild in voller Größe anzeigen](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Programmgesteuerter Zugriff auf die Produkte nach Kategoriedaten

Statt einer ObjectDataSource gegeben, die Produkte für die aktuelle Kategorie abzurufen, können wir erstellen Sie eine Methode in unserer ASP.NET Seite s Code-Behind-Klasse (oder in der `App_Code` Ordner oder in einem separaten Klassenbibliotheksprojekt), die den entsprechenden Satz von zurückgibt Produkte, die beim Übergeben einer `CategoryID`. Stellen Sie sich vor, dass wir in unserer ASP.NET Seite s Code-Behind-Klasse eine solche Methode hatten und benannt wurde `GetProductsInCategory(categoryID)`. Mit dieser Methode vorhanden konnten wir die Produkte für die aktuelle Kategorie an den inneren Repeater mithilfe der folgenden deklarative Syntax binden:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Der Repeater s `DataSource` Eigenschaft verwendet die Databinding-Syntax, um anzugeben, dass die Daten stammen die `GetProductsInCategory(categoryID)` Methode. Da `Eval("CategoryID")` gibt einen Wert vom Typ `Object`, wir wandeln Sie das Objekt in ein `Integer` vor der Übergabe in die `GetProductsInCategory(categoryID)` Methode. Beachten Sie, dass der `CategoryID` zugegriffen hier über die Datenbindung Syntax ist der `CategoryID` in der *äußeren* Repeater (`CategoryList`), der s gebunden, auf die Datensätze in der `Categories` Tabelle. Aus diesem Grund wissen wir, dass `CategoryID` eine Datenbank nicht möglich `NULL` Wert, der angibt, warum wir Blind umgewandelt werden kann die `Eval` -Methode ohne überprüfen, ob wir erneut Umgang mit einer `DBNull`.

Bei diesem Ansatz müssen wir erstellen den `GetProductsInCategory(categoryID)` Methode und den entsprechenden Satz von Produkten, erhält die angegebene abrufen *`categoryID`*. Wir erreichen dies durch eine einfache Rückkehr der `ProductsDataTable` zurückgegebenes der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode. S erstellen lassen die `GetProductsInCategory(categoryID)` Methode in der CodeBehind-Klasse für unseren `NestedControls.aspx` Seite. Dazu verwenden Sie den folgenden Code:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Diese Methode erstellt einfach eine Instanz von der `ProductsBLL` Methode und gibt die Ergebnisse der `GetProductsByCategoryID(categoryID)` Methode. Beachten Sie, dass die Methode markiert sein muss `Public` oder `Protected`; Wenn die Methode markiert ist `Private`, es kann nicht aus dem ASP.NET Seite s deklaratives Markup zugegriffen werden.

Nach diesen Änderungen zum Verwenden dieses neuen Verfahrens nehmen einen Moment Zeit, um die Seite über einen Browser anzuzeigen. Die Ausgabe sollte in die Ausgabe identisch sein, wenn mit dem ObjectDataSource-Steuerelement und `ItemDataBound` Event Handler Ansatz (siehe Abbildung 5, um einen Screenshot finden Sie unter).

> [!NOTE]
> Es mag nichtproduktive Beschäftigung, zum Erstellen der `GetProductsInCategory(categoryID)` -Methode in der ASP.NET Page s-Code-Behind-Klasse. Schließlich erstellt diese Methode einfach eine Instanz von der `ProductsBLL` -Klasse und gibt die Ergebnisse der seine `GetProductsByCategoryID(categoryID)` Methode. Warum also nicht direkt über die Databinding-Syntax in der inneren Repeater, wie diese Methode nur aufrufen: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Obwohl diese Syntax t-arbeiten mit unseren aktuellen Implementierung von gewonnen hat die `ProductsBLL` Klasse (da die `GetProductsByCategoryID(categoryID)` Methode ist eine Instanzmethode), können Sie ändern `ProductsBLL` zum Einschließen der eines statisches `GetProductsByCategoryID(categoryID)` Methode oder über die Klasse, die statisch `Instance()` Methode, um eine neue Instanz der zurückzugeben. die `ProductsBLL` Klasse.


Während Sie solche Änderungen die Notwendigkeit beseitigen, würde die `GetProductsInCategory(categoryID)` -Methode in der ASP.NET Page s-Code-Behind-Klasse, die Code-Behind-Klassenmethode ergibt mehr Flexibilität bei der Arbeit mit den Daten abgerufen, wie wir gleich sehen werden.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Abrufen aller gleichzeitig die Produktinformationen

Die beiden früheren Techniken wir untersucht haben diese Produkte für die aktuelle Kategorie ziehen Sie von einem Aufruf an die `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)` Methode (der erste Ansatz wurde dies durch ein ObjectDataSource-Steuerelement, das zweite bis der `GetProductsInCategory(categoryID)` -Methode in der die Code-Behind-Klasse). Jedes Mal, diese Methode aufgerufen wird, die Business Logic Layer-Aufrufe auf der Datenzugriffsebene, die Abfragen der Datenbank mit einer SQL-Anweisung, die Zeilen zurückgibt. die `Products` Tabelle, deren `CategoryID` Feld mit den bereitgestellten Eingabeparameter übereinstimmt.

Erhält *N* Kategorien im System, die diesen Ansatz Nettoermittlung *N* + 1 Aufrufe an die Datenbank eine Datenbank-Abfrage zum Abrufen aller Kategorien und dann *N* aufrufen, um die Produkte zu erhalten. für jede Kategorie. Wir können jedoch alle erforderlichen Daten in nur zwei Datenbank-Aufrufe ein Aufruf zum Abrufen aller Kategorien und eine zum Abrufen aller Produkte abrufen. Nachdem wir alle Produkte haben, können wir diese Produkte also filtern, dass nur die Produkte, die die aktuelle Übereinstimmung `CategoryID` gebunden sind, auf diese Kategorie s innere Repeater.

Um diese Funktionalität zu gewährleisten, müssen wir nur eine kleine Änderung an Stellen die `GetProductsInCategory(categoryID)` -Methode in unserer ASP.NET Page s-Code-Behind-Klasse. Anstatt Sie wahllos Zurückgeben der Ergebnisse von der `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)` -Methode, können wir stattdessen ersten Zugriff auf *alle* der Produkte (Wenn sie t wurde wurde bereits zugegriffen) und wieder nur die gefilterte Ansicht der der Produkte basierend auf der übergebenen `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Beachten Sie das Hinzufügen der Variable auf Seitenebene `allProducts`. Dies enthält Informationen zu allen Produkten und wird beim ersten aufgefüllt, die `GetProductsInCategory(categoryID)` -Methode wird aufgerufen. Nachdem Sie sichergestellt haben, die die `allProducts` Objekt erstellt und aufgefüllt wurde, die Methode filtert die Ergebnisse, DataTable s aus, sodass nur die Zeilen, deren `CategoryID` entspricht dem angegebenen `CategoryID` zugegriffen werden. Dieser Ansatz reduziert die Anzahl der Male, die die Datenbank über erfolgt *N* + 1 auf zwei.

Diese Verbesserung führt keine Änderungen, dem gerenderten Markup der Seite, und bringt sie wieder weniger Datensätze als der andere Ansatz. Es wird einfach die Anzahl der Aufrufe an die Datenbank verringert.

> [!NOTE]
> Eine möglicherweise intuitiv Grund, dass die Reduzierung der Anzahl von Datenbankzugriffe unbedingt einer leistungsverbesserung. Allerdings kann dies nicht der Fall sein. Wenn Sie über eine große Anzahl von Produkten verfügen, deren `CategoryID` ist `NULL`, für das Beispiel, und klicken Sie dann auf den Aufruf von der `GetProducts` Methode gibt eine Reihe von Produkten, die nie angezeigt werden. Darüber hinaus Zurückgeben aller Produkte möglich Vergeudung Wenn Sie erneut mit nur eine Teilmenge der Kategorien, was der Fall sein kann, wenn Sie Paging implementiert haben.


Wie immer, wenn es zum Analysieren der Leistung von zwei Techniken geht, wird das Measure nur scherzhafter Kommentar gesteuerten Tests, die für Ihre Anwendung s allgemeine Anwendungsszenarien maßgeschneiderte ausgeführt.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie ein Websteuerelement innerhalb einer anderen schachteln speziell untersucht, wie man eine äußere Repeater, das ein Element für jede Kategorie mit einem inneren Repeater Auflisten von Produkten für jede Kategorie in eine Liste mit Aufzählungszeichen angezeigt haben. Die größte Herausforderung beim Erstellen einer geschachtelten Benutzeroberfläche liegt in den Zugriff auf und binden die richtigen Daten an das interne Web-Steuerelement. Es gibt eine Vielzahl von Techniken zur Verfügung, von denen zwei wir in diesem Tutorial untersuchten. Die erste Methode untersucht verwendet eine "ObjectDataSource" in der äußeren Websteuerelement s `ItemTemplate` , gebunden an das interne Web-Steuerelement über seine `DataSourceID` Eigenschaft. Die zweite Methode Zugriff auf die Daten über eine Methode in der ASP.NET Page-s-Code-Behind-Klasse. Diese Methode klicken Sie dann auf die inneren Daten Websteuerelement s gebunden werden kann `DataSource` Eigenschaft über die Databinding-Syntax.

Während die geschachtelten Benutzeroberfläche untersucht, die in diesem Tutorial einen Repeater, die in einem Wiederholungssteuerelement geschachtelt verwendet, können diese Techniken auf den anderen datenwebsteuerelementen erweitert werden. Sie können schachteln, einen Repeater innerhalb einer GridView-Ansicht oder einer GridView-Ansicht in einem DataList-Steuerelement und so weiter.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Weiter](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
