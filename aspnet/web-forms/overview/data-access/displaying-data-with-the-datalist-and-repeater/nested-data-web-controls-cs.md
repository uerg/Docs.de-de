---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Geschachtelte Daten Web Controls (c#) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm aus, die wir untersuchen ein anderes Repeater geschachtelt wie einen Repeater verwenden. In den Beispielen werden die inneren Repeater beide d Auffüllen veranschaulichen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 69fa0489ff8baed1423d29ee7bfaa3157d35a76b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="nested-data-web-controls-c"></a>Geschachtelte Daten Web Controls (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) oder [PDF herunterladen](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> In diesem Lernprogramm aus, die wir untersuchen ein anderes Repeater geschachtelt wie einen Repeater verwenden. In den Beispielen werden der inneren Repeater deklarativ und programmgesteuert Auffüllen von veranschaulicht.


## <a name="introduction"></a>Einführung

Zusätzlich zu den statischem HTML-Code und Databinding-Syntax können Vorlagen auch Web-Steuerelemente und Benutzersteuerelemente einschließen. Diese Web-Steuerelemente können ihre Eigenschaften verfügen über die Datenbindungssyntax der deklarativen zugewiesen oder in die entsprechende serverseitige Ereignishandler programmgesteuert zugegriffen werden kann.

Durch das Einbetten von Steuerelementen innerhalb einer Vorlage aus, kann die Darstellung und die benutzerfreundlichkeit angepasst und verbessert werden. Beispielsweise ist in der [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Lernprogramm wurde erläutert, wie die GridView-s-Anzeige anpassen, indem Sie ein Monatskalender-Steuerelement hinzufügen, in ein TemplateField ein Mitarbeiter s Einstellungsdatum angezeigt; in der [hinzufügen Validierungssteuerelemente werden an die bearbeiten und das Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) und [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Lernprogramme wurde erläutert, wie die Bearbeitung angepasst und Einfügen von Schnittstellen durch Hinzufügen einer Validierung Steuerelemente, Textfelder, DropDownLists und andere Websteuerelemente.

Vorlagen können auch andere Daten Websteuerelemente enthalten. D. h. können wir DataList verfügen, eine andere DataList (oder Repeater oder GridView oder DetailsView usw.) innerhalb der Vorlagen enthält. Die Herausforderung beim eine solche Schnittstelle wird die entsprechenden Daten auf die inneren Daten Websteuerelement binden. Ein paar unterschiedliche Ansätze sind verfügbar, im Bereich von deklarativen Optionen, die mithilfe der ObjectDataSource programmgesteuerte Vorgängen.

In diesem Lernprogramm aus, die wir untersuchen ein anderes Repeater geschachtelt wie einen Repeater verwenden. Die äußere Repeater enthält ein Element für jede Kategorie in der Datenbank anzeigen, die s-Kategorienamen und eine Beschreibung. Jedes Kategorieelement s innere Repeater werden Informationen für jedes Produkt, die zu dieser Kategorie gehören angezeigt (siehe Abbildung 1) in einer Liste mit Aufzählungszeichen aus. Diesen Beispielen werden der inneren Repeater deklarativ und programmgesteuert Auffüllen von veranschaulicht.


[![Jede Kategorie, zusammen mit seiner Produkte aufgeführt sind](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Abbildung 1**: jede Kategorie, zusammen mit seiner Produkte aufgeführt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Schritt 1: Erstellen der Liste der Kategorien

Wenn Data-Websteuerelemente erstellen eine Seite mit geschachtelt werden, Entwurf hilfreich I, erstellen und testen das Websteuerelement äußersten Daten zuerst, ohne auch die inneren geschachtelten Steuerelement kümmern zu müssen. Aus diesem Grund können Sie starten, indem Sie Schritt für Schritt durch die Schritte für ein Repeater auf der Seite hinzufügen, die den Namen und eine Beschreibung für die einzelnen Kategorien aufgelistet und s ein.

Öffnen Sie zunächst die `NestedControls.aspx` auf der Seite der `DataListRepeaterBasics` Ordner und fügen Sie eine Wiederholungsmodul-Steuerelement auf der Seite festlegen seiner `ID` Eigenschaft `CategoryList`. Wählen Sie das Smarttag Repeater s, zum Erstellen einer neuen ObjectDataSource mit dem Namen `CategoriesDataSource`.


[![Name der neuen ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Abbildung 2**: Benennen Sie die neue ObjectDataSource `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-data-web-controls-cs/_static/image6.png))


Das ObjectDataSource so konfigurieren, dass sie ihre Daten Abrufen der `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL Klasse s GetCategories-Methode](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse s `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-data-web-controls-cs/_static/image9.png))


Zum Angeben der Repeater s Vorlage muss Inhalt zur Quellansicht wechseln und die deklarative Syntax manuell eingeben. Hinzufügen einer `ItemTemplate` , die zeigt den Namen der Kategorie s in eine `<h4>` Element und die Beschreibung der Kategorie s in einem Absatzelement (`<p>`). Darüber hinaus können s trennen Sie jede Kategorie durch eine horizontale Linie (`<hr>`). Nach diesen Änderungen sollte Ihre Seite deklarativen Syntax enthalten, für die Repeater und ObjectDataSource, die der folgenden ähnlich ist:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Abbildung 4 zeigt unseren Fortschritt, wenn Sie über einen Browser angezeigt.


[![Jede Kategorie s Name und Beschreibung aufgelistet ist durch eine horizontale Linie getrennt](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Abbildung 4**: jede Kategorie s Name und Beschreibung aufgelistet ist, getrennt durch eine horizontale Linie ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Schritt 2: Hinzufügen von geschachtelten Produkt Repeater

Die Kategorie Angebot abgeschlossen ist, ist unsere nächste Aufgabe einen Repeater zum Hinzufügen der `CategoryList` s `ItemTemplate` , die Informationen zu diesen Produkten zur entsprechenden Kategorie anzeigt. Es gibt zahlreiche Möglichkeiten, die wir die Daten für diesen inneren Repeater abrufen können, von denen zwei wir kurz erläutert. Jetzt erstellen lassen s nur die Produkte Repeater innerhalb der `CategoryList` Repeater s `ItemTemplate`. Ermöglichen Sie insbesondere s haben das Produkt Repeater anzeigen, die jedes Produkt in einer Liste mit Aufzählungszeichen mit jedem Listenelement z. B. Produktname s und Preis.

Diese Repeater erstellen wir manuell eingeben, die innere Repeater s deklarative Syntax und die Vorlagen in müssen die `CategoryList` s `ItemTemplate`. Fügen Sie das folgende Markup innerhalb der `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Schritt 3: Binden von kategoriespezifische Produkte an die ProductsByCategoryList Repeater

Wenn Sie die Seite über einen Browser an diesem Punkt besuchen, der Bildschirm Erscheinungsbild ist dasselbe wie in Abbildung 4 da wir Ve noch, zum Binden von Daten an den Repeater. Es gibt einige Möglichkeiten, dass wir ziehen Sie die entsprechenden Datensätze und diese an die Repeater, einige effizienter als andere binden. Die wesentliche Herausforderung hier nur noch die entsprechenden Produkte für die angegebene Kategorie zurück.

Die Daten zum Binden an die innere Wiederholungsmodul-Steuerelement können entweder deklarativ über ein ObjectDataSource in zugegriffen werden die `CategoryList` Repeater s `ItemTemplate`, oder programmgesteuert über die ASP.NET Seite "s" Code-Behind-Seite. Auf ähnliche Weise, diese Daten können gebunden werden an den inneren Repeater entweder deklarativ - über den inneren Repeater s `DataSourceID` Eigenschaft oder mithilfe von deklarativen Databinding-Syntax oder programmgesteuert durch Verweisen auf die innere Repeater in die `CategoryList` Repeater s `ItemDataBound` Ereignishandler, d. h. Sie programmgesteuert festlegen seiner `DataSource` -Eigenschaft, und der Aufruf der `DataBind()` Methode. Lassen Sie s, die jedem dieser Ansätze untersuchen.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Zugreifen auf die Daten deklarativ mit einem ObjectDataSource-Steuerelement und dem`ItemDataBound`-Ereignishandler

Ve verwendet das ObjectDataSource umfassend in der gesamten dieser Reihe von Lernprogrammen, die am häufigsten gute Wahl für den Datenzugriff für dieses Beispiel besteht darin, mit der ObjectDataSource bleiben, da wir. Die `ProductsBLL` -Klasse verfügt über eine `GetProductsByCategoryID(categoryID)` Methode, die Informationen zu diesen Produkten zurückgibt, die in den angegebenen gehören  *`categoryID`* . Daher können wir eine ObjectDataSource zum Hinzufügen der `CategoryList` Repeater s `ItemTemplate` , und konfigurieren sie den Zugriff auf die Daten aus dieser Klasse s-Methode.

Leider die Repeater ist nicht zulassen ihre Vorlagen in der Entwurfsansicht bearbeitet werden, damit wir die deklarative Syntax für das ObjectDataSource-Steuerelement manuell hinzufügen müssen. Die folgende Syntax zeigt die `CategoryList` Repeater s `ItemTemplate` nach dem Hinzufügen dieses neue ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Bei Verwendung des Ansatzes ObjectDataSource müssen wir Festlegen der `ProductsByCategoryList` Repeater s `DataSourceID` Eigenschaft, um die `ID` der ObjectDataSource (`ProductsByCategoryDataSource`). Beachten Sie auch, die unsere ObjectDataSource verfügt über eine `<asp:Parameter>` Element, das angibt der  *`categoryID`*  -Wert, der an übergeben werden die `GetProductsByCategoryID(categoryID)` Methode. Aber wie wir diesen Wert angeben? Im Idealfall werden wir d kann nur festgelegt, die `DefaultValue` Eigenschaft von der `<asp:Parameter>` Element mit Databinding-Syntax wie folgt:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Databinding-Syntax ist leider nur gültig in Steuerelemente, die eine `DataBinding` Ereignis. Die `Parameter` Klasse verfügt nicht über einen solchen und daher die oben aufgeführten Syntax ist ungültig und führt zu einem Laufzeitfehler.

Um diesen Wert festzulegen, müssen wir erstellen einen Ereignishandler für das `CategoryList` Repeater s `ItemDataBound` Ereignis. Bedenken Sie, dass die `ItemDataBound` Ereignis auslöst einmal für jedes Element an der Repeater gebunden. Deshalb jedes Mal, die das Ereignis ausgelöst, für die äußere Repeater wird wir können zuweisen den aktuellen `CategoryID` -Wert an die `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` Parameter.

Erstellen Sie einen Ereignishandler für das `CategoryList` Repeater s `ItemDataBound` Ereignis mit den folgenden Code:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Dieser Ereignishandler startet, indem Sie sicherstellen, dass wir re Umgang mit Daten anstatt der Element-Header, Footer oder Trennzeichen Element. Als Nächstes verweisen wir den tatsächlichen `CategoriesRow` -Instanz, die nur mit dem aktuellen gebunden wurde `RepeaterItem`. Schließlich, verweisen wir ObjectDataSource der `ItemTemplate` , und weisen Sie die `CategoryID` Parameterwert, der `CategoryID` des aktuellen `RepeaterItem`.

Mit dieser Ereignishandler der `ProductsByCategoryList` Repeater in jedem `RepeaterItem` gebunden ist, für die Produkte in der `RepeaterItem` s-Kategorie. Abbildung 5 zeigt ein Screenshot, der die resultierende Ausgabe.


[![Die äußere Repeater enthält jede Kategorie; das Innere einer Listet die Produkte für diese Kategorie](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Abbildung 5**: der äußeren Repeater listet jede Kategorie; innere eine Listen, die Produkte für diese Kategorie ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Programmgesteuerter Zugriff auf die Produkte nach Kategoriedaten

Anstatt ein ObjectDataSource zum Abrufen von Produkten für die aktuelle Kategorie zu verwenden, konnten wir erstellen Sie eine Methode in unserer ASP.NET Seite "s" Code-Behind-Klasse (oder in der `App_Code` Ordner oder in einem separaten Klassenbibliotheksprojekt) des entsprechenden Satzes an zurückgibt Produkte, die beim Übergeben einer `CategoryID`. Stellen Sie sich vor, dass wir in unserer ASP.NET Seite "s" Code-Behind-Klasse eine solche Methode hatten und benannt wurde `GetProductsInCategory(categoryID)`. Mit dieser Methode vorhanden konnten wir die Produkte für die aktuelle Kategorie an den inneren Repeater mithilfe der folgenden deklarative Syntax binden:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Die Repeater s `DataSource` Eigenschaft verwendet die Databinding-Syntax, um anzugeben, dass die Daten stammen die `GetProductsInCategory(categoryID)` Methode. Da `Eval("CategoryID")` gibt einen Wert vom Typ `Object`, wir wandeln Sie das Objekt um eine `Integer` vor der Übergabe in die `GetProductsInCategory(categoryID)` Methode. Beachten Sie, dass die `CategoryID` zugegriffen hier über dem Datenbindung Syntax wird der `CategoryID` in der *äußeren* Repeater (`CategoryList`), der s gebunden, um die Datensätze in der `Categories` Tabelle. Daher wir wissen, `CategoryID` nicht mit eine Datenbank `NULL` Wert, der angibt, warum wir Blind umgewandelt werden kann die `Eval` Methode ohne überprüft, ob wir re Umgang mit einer `DBNull`.

Bei diesem Ansatz müssen wir erstellen die `GetProductsInCategory(categoryID)` Methode und die geeignete Reihe der erhält die angegebenen Produkte abrufen  *`categoryID`* . Wir erreichen dies durch eine einfache Rückkehr der `ProductsDataTable` zurückgegebenes der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode. S erstellen lassen die `GetProductsInCategory(categoryID)` Methode in der CodeBehind-Klasse für unsere `NestedControls.aspx` Seite. Dazu verwenden Sie den folgenden Code:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Diese Methode erstellt einfach eine Instanz von der `ProductsBLL` Methode und gibt die Ergebnisse der `GetProductsByCategoryID(categoryID)` Methode. Beachten Sie, dass die Methode markiert werden muss `Public` oder `Protected`; Wenn die Methode gekennzeichnet ist `Private`, es kann nicht aus der deklarativen ASP.NET Seitenmarkup s zugegriffen werden.

Nach diesen Änderungen dieses neue Verfahren verwenden, können Sie die Seite über einen Browser anzeigen. Die Ausgabe sollte an die Ausgabe identisch sein, bei Verwendung der ObjectDataSource und `ItemDataBound` Ereignis-Handler-Ansatz (siehe Abbildung 5, um einen Screenshot anzuzeigen).

> [!NOTE]
> Es mag aufwändige Routinearbeiten zum Erstellen der `GetProductsInCategory(categoryID)` Methode in der ASP.NET Seite "s" Code-Behind-Klasse. Nachdem alle erstellt diese Methode einfach eine Instanz von der `ProductsBLL` -Klasse und die Ergebnisse zurückgegeben werden seine `GetProductsByCategoryID(categoryID)` Methode. Warum nicht direkt aus der Databinding-Syntax in der inneren Repeater wie diese Methode nur aufrufen: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Obwohl diese Syntax mit die aktuelle Implementierung von gewonnen hat die `ProductsBLL` Klasse (seit der `GetProductsByCategoryID(categoryID)` Methode ist eine Instanzmethode), könnten Sie ändern `ProductsBLL` einschließen ein statischen `GetProductsByCategoryID(categoryID)` Methode oder die Klasse, die eine statische enthaltenist`Instance()` Methode, um eine neue Instanz der zurückzugeben der `ProductsBLL` Klasse.


Während Sie solche Änderungen erforderlich wäre die `GetProductsInCategory(categoryID)` Methode in der ASP.NET Seite "s" Code-Behind-Klasse, die Code-Behind-Klasse-Methode erhalten Sie mehr Flexibilität bei der Arbeit mit den Daten abgerufen, wie es in Kürze sehen.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Abrufen aller gleichzeitig die Produktinformationen

Die beiden vorherigen Verfahren wir Ve untersucht ziehen Sie diese Produkte für die aktuelle Kategorie von einem Aufruf an die `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode (der erste Ansatz hat daher über eine ObjectDataSource das zweite bis die `GetProductsInCategory(categoryID)` Methode in der Code-Behind-Klasse). Jedes Mal, diese Methode aufgerufen wird, die Business Logic Layer-Aufrufe an die Datenzugriffsebene fragt die Datenbank mit einer SQL-Anweisung, die aus Zeilen zurückgibt der `Products` Tabelle, deren `CategoryID` Feld mit den bereitgestellten Eingabeparameter übereinstimmt.

Angegebene *N* Kategorien im System, die diesen Ansatz Nettoermittlung *N* + 1 Aufrufe der Datenbankabfrage eine Datenbank, um alle Kategorien abrufen und dann *N* Aufrufe an die Produkte abrufen Besonderheit bei jeder Kategorie. Wir können jedoch alle benötigten Daten nur zwei Datenbank Aufrufe pro Aufruf zum Abrufen aller Kategorien und ein weiteres zum Abrufen aller Produkte abgerufen werden. Sobald wir alle Produkte haben, können wir diese Produkte so filtern, dass nur die Produkte, die die aktuelle Übereinstimmung `CategoryID` gebunden sind, für diese Kategorie s innere Repeater.

Um diese Funktionalität zu gewährleisten, müssen wir nur eine geringfügige Änderungen an Stellen die `GetProductsInCategory(categoryID)` -Methode in unserer ASP.NET Seite "s" Code-Behind-Klasse. Anstatt Blind Zurückgeben der Ergebnisse der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` -Methode, können wir stattdessen ersten Zugriff auf *alle* Produkte (Wenn sie t unklar wurde bereits zugegriffen) und dann nur die gefilterte Ansicht der zurückkehren der Produkte basierend auf der übergebenen in `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Beachten Sie das Hinzufügen der Variablen auf Seitenebene `allProducts`. Speichert Informationen für alle Produkte und erstmalig wird aufgefüllt, die `GetProductsInCategory(categoryID)` Methode aufgerufen wird. Nachdem sichergestellt wurde, dass die `allProducts` Objekt erstellt und aufgefüllt wurde, die Methode filtert die Ergebnisse, DataTable s aus, sodass nur die Zeilen, deren `CategoryID` entspricht dem angegebenen `CategoryID` zugegriffen werden. Dieser Ansatz reduziert die Anzahl der Zugriffe auf die Datenbank wird aus *N* + 1 auf zwei.

Diese Verbesserung führt jede Änderung der gerenderten Markup der Seite, und es erhält wieder weniger Datensätze als die anderen nicht linearen Ansatz. Es wird lediglich die Anzahl der Aufrufe der Datenbank reduziert.

> [!NOTE]
> Eine möglicherweise intuitiv Grund, dass die Verringerung von Datenbankzugriffe assuredly leistungsverbesserung. Dies kann jedoch die Groß-/Kleinschreibung nicht. Wenn stehen Ihnen eine große Anzahl von Produkten, deren `CategoryID` ist `NULL`, zum Beispiel, und klicken Sie dann auf den Aufruf von der `GetProducts` Methode gibt eine Anzahl von Produkten, die nie angezeigt werden. Darüber hinaus Zurückgeben aller Produkte kann aufwändig Wenn Sie nur eine Teilmenge der Kategorien, die die Groß-/Kleinschreibung möglicherweise implementiert, Paging angezeigt.


Wie immer, wenn es darum geht, Analysieren der Leistung von zwei Techniken, wird das Measure nur bombensichere gesteuerten Tests, die speziell für Ihre Anwendung s allgemeinen Groß-/Kleinschreibung Szenarien ausgeführt.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie ein Websteuerelement innerhalb einer anderen schachteln speziell untersuchen wie eine äußere Repeater mit einem inneren Repeater Auflisten von Produkten für jede Kategorie in eine Liste mit Aufzählungszeichen ein Element für jede Kategorie anzuzeigen. Die größte Herausforderung beim Erstellen einer geschachtelten Benutzeroberfläche liegt in der Zugriff auf und die richtigen Daten an das Websteuerelement inneren Daten binden. Es gibt eine Vielzahl von Techniken zur Verfügung, von denen zwei wir in diesem Lernprogramm untersucht. Der erste Ansatz untersucht verwendet ein ObjectDataSource in die äußeren Daten Websteuerelement s `ItemTemplate` , das an das Websteuerelement innere Daten über gebunden wurde die `DataSourceID` Eigenschaft. Die zweite Methode Zugriff auf die Daten über eine Methode in der ASP.NET Seite s-Code-Behind-Klasse. Diese Methode kann anschließend auf die inneren Daten Websteuerelement s gebunden werden `DataSource` Eigenschaft durch Databinding-Syntax.

Obwohl die geschachtelte Benutzeroberfläche, die in diesem Lernprogramm untersucht einen Repeater ein Repeater geschachtelt verwendet, können dieser Techniken zu den anderen Data-Websteuerelementen erweitert werden. Sie können einen Repeater innerhalb einer GridView oder eine GridView in einem DataList schachteln und so weiter.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones und Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
[Weiter](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
