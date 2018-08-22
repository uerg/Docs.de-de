---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Anpassen der Benutzeroberfläche der Daten ändern (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Lernprogramm lernen wir an, wie die Schnittstelle eine bearbeitbare GridView anpassen, indem Sie ersetzt das standardmäßige Textfeld und CheckBox-Steuerelemente mit Alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f7004192edd636f4660f3184c3e725a6bfda865c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826703"
---
<a name="customizing-the-data-modification-interface-c"></a>Anpassen der Benutzeroberfläche der Daten ändern (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) oder [PDF-Datei herunterladen](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> In diesem Lernprogramm lernen wir an, wie die Schnittstelle eine bearbeitbare GridView anpassen, indem Sie ersetzt das standardmäßige Textfeld, und mit alternativen Web Eingabesteuerelemente CheckBox-Steuerelemente.


## <a name="introduction"></a>Einführung

Die BoundFields und CheckBoxFields, die von der GridView und DetailsView-Steuerelemente verwendet vereinfachen das Ändern von Daten aufgrund ihrer Fähigkeit, schreibgeschützt, bearbeitbare und einfügbare Schnittstellen zu rendern. Diese Schnittstellen können ohne die Notwendigkeit für das Hinzufügen von zusätzlichen deklarativen Markup oder Code gerendert werden. Die BoundField- und des CheckBoxField Schnittstellen fehlt jedoch die Anpassungsfähigkeit, die häufig in realen Szenarios erforderlich. Um die bearbeitbar oder einfügbar-Schnittstelle in einem GridView- oder DetailsView anpassen müssen wir stattdessen ein TemplateField verwenden.

In der [vorherigen Lernprogramm](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) erläutert, wie die Schnittstellen für Änderungen durch das Hinzufügen von Steuerelementen zur gültigkeitsprüfung Web anpassen. In diesem Tutorial betrachten wir die tatsächlichen Daten Auflistung Websteuerelemente, ersetzen Sie dabei die BoundField- und CheckBoxFields standard-TextBox und Kontrollkästchen-Steuerelemente mit alternativen Web Eingabesteuerelemente anpassen. Insbesondere erstellen wir eine bearbeitbare GridView, die eines Produkts Name, Kategorie, Lieferanten und nicht mehr unterstützte Status aktualisiert werden können. Bei der Bearbeitung einer bestimmten Zeile rendert die Kategorie und Lieferant Felder als DropDownList-Steuerelementen, mit dem Satz der verfügbaren Kategorien und Lieferanten zur Auswahl. Darüber hinaus wird CheckBoxFields Standard Kontrollkästchen durch ein RadioButtonList-Steuerelement, das zwei Optionen bietet ersetzt: "Aktiv" und "Nicht mehr unterstützt".


[![Bearbeiten des GridView-Schnittstelle enthält, DropDownList-Steuerelementen und RadioButtons](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Abbildung 1**: der GridView Bearbeitung Schnittstelle enthält DropDownList-Steuerelementen und RadioButtons ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Schritt 1: Erstellen der erforderlichen`UpdateProduct`überladen

In diesem Tutorial werden wir eine bearbeitbare GridView erstellt wird, ermöglicht die Bearbeitung der Name des Produkts, Kategorie, Lieferanten und nicht mehr unterstützte Status. Aus diesem Grund müssen wir eine `UpdateProduct` Überladung verwenden, akzeptiert der fünf Eingabeparameter diese vier Product-Werte, sowie die `ProductID`. Wie Sie in unseren vorherigen Überladungen, die dadurch ein:

1. Rufen Sie die Produktinformationen aus der Datenbank für den angegebenen `ProductID`,
2. Update der `ProductName`, `CategoryID`, `SupplierID`, und `Discontinued` Felder und
3. Senden Sie die aktualisierungsanforderung an die DAL durch der TableAdapters `Update()` Methode.

Aus Gründen der Übersichtlichkeit haben diese jeweilige Überlastung ich die regelüberprüfung Business weggelassen, die sicherstellt, dass ein Produkt wie nicht mehr unterstützt markiert das einzige Produkt, die von den zugehörigen Lieferanten angeboten werden nicht. Gerne in hinzufügen, falls gewünscht, oder, im Idealfall Umgestalten, die Logik auf eine separate Methode.

Der folgende Code zeigt die neue `UpdateProduct` überladen, der `ProductsBLL` Klasse:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Schritt 2: Erstellen der bearbeitbaren GridView

Mit der `UpdateProduct` Überladung hinzugefügt haben, wir können unsere bearbeitbaren GridView zu erstellen. Öffnen der `CustomizedUI.aspx` auf der Seite die `EditInsertDelete` Ordner, und fügen Sie in den Designer ein GridView-Steuerelement hinzu. Als Nächstes erstellen Sie eine neue "ObjectDataSource" in den GridView Smarttag. Konfigurieren Sie das "ObjectDataSource" zum Abrufen von Produktinformationen über der `ProductBLL` -Klasse `GetProducts()` Methode und Aktualisieren von Produkt Daten mithilfe der `UpdateProduct` Überladung, die wir gerade erstellt haben. Wählen Sie aus den Registerkarten INSERT- und DELETE (keine) aus den Dropdown-Listen.


[![Konfigurieren Sie die "ObjectDataSource", um die soeben erstellte UpdateProduct-Überladung verwenden](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Abbildung 2**: Konfigurieren Sie das "ObjectDataSource" Verwenden der `UpdateProduct` überladen gerade erstellt haben ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image6.png))


Wie wir in den Lernprogrammen für Data Änderung gesehen haben, weist die deklarative Syntax für das ObjectDataSource-Steuerelement von Visual Studio erstellt die `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Dies natürlich funktioniert nicht mit unseren Business Logic Layer da unsere Methoden erwarten, dass die ursprüngliche nicht `ProductID` Wert übergeben werden. Aus diesem Grund wie in vorherigen Tutorials haben, können Sie die deklarative Syntax dieser Eigenschaft die Zuweisung aufheben, oder legen Sie stattdessen der Wert dieser Eigenschaft auf `{0}`.

Nach dieser Änderung sollte dem ObjectDataSource-Steuerelement deklarative Markup wie folgt aussehen:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Beachten Sie, dass die `OldValuesParameterFormatString` Eigenschaft entfernt wurde und dass es eine `Parameter` in die `UpdateParameters` Auflistung für die einzelnen Eingabeparameter vom erwarteten unsere `UpdateProduct` überladen.

Während dem ObjectDataSource-Steuerelement konfiguriert ist, um nur eine Teilmenge der Product-Werte zu aktualisieren, zeigt die GridView derzeit *alle* der Felder Product. Können Sie die GridView zu bearbeiten, damit:

- Es enthält nur die `ProductName`, `SupplierName`, `CategoryName` BoundFields und `Discontinued` CheckBoxField
- Die `CategoryName` und `SupplierName` Felder vor (auf der linken Seite des) aufgeführt werden. die `Discontinued` CheckBoxField
- Die `CategoryName` und `SupplierName` 'BoundFields `HeaderText` -Eigenschaftensatz auf "Category" und "Supplier", bzw.
- Unterstützung für die Bearbeitung ist aktiviert (Überprüfen Sie das Kontrollkästchen Bearbeiten aktivieren, in den GridView Smarttag)

Nach diesen Änderungen sieht der Designer ähnlich aus wie in Abbildung 3, mit GridView deklarative Syntax des unten.


[![Entfernen Sie nicht benötigte Felder aus der GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Abbildung 3**: Entfernen Sie nicht benötigte Felder aus der GridView ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

GridView schreibgeschütztes Verhalten ist an diesem Punkt abgeschlossen. Wenn Sie die Daten anzeigen zu können, wird jedes Produkt als eine Zeile in der GridView, die mit den Namen des Produkts, Kategorie, Lieferanten, gerendert wird und nicht mehr unterstützte Status.


[![GridView Read-Only-Schnittstelle ist abgeschlossen.](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Abbildung 4**: der GridView Read-Only-Schnittstelle ist abgeschlossen. ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Siehe [eine Übersicht der einfügen, aktualisieren und Löschen von Daten Tutorial](an-overview-of-inserting-updating-and-deleting-data-cs.md), es ist äußerst wichtig, dass die GridView Ansichtszustand s werden (Standardverhalten) aktiviert. Setzen Sie das GridView-s `EnableViewState` Eigenschaft `false`, führen Sie das Risiko gleichzeitiger Benutzer versehentlich löschen oder zu bearbeiten zeichnet. Finden Sie unter [Warnung: Problem der Parallelität mit ASP.NET 2.0 GridViews/DetailsView/FormViews diese Unterstützung zu bearbeiten bzw. löschen und, deren Ansichtszustand deaktiviert ist](http://scottonwriting.net/sowblog/posts/10054.aspx) für Weitere Informationen.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Schritt 3: Verwenden einer DropDownList für die Kategorie und Lieferanten, die Bearbeitung von Schnittstellen

Bedenken Sie, dass die `ProductsRow` Objekt enthält `CategoryID`, `CategoryName`, `SupplierID`, und `SupplierName` Eigenschaften, die Geben Sie die tatsächlichen Fremdschlüssel-ID-Werte in der `Products` Datenbank, Tabelle und die entsprechende `Name` Werte in der `Categories` und `Suppliers` Tabellen. Die `ProductRow`des `CategoryID` und `SupplierID` können werden sowohl gelesen und geschrieben werden, um zwar die `CategoryName` und `SupplierName` Eigenschaften als schreibgeschützt gekennzeichnet sind.

Aufgrund des Status "schreibgeschützt" von der `CategoryName` und `SupplierName` Eigenschaften, die entsprechenden BoundFields hatten ihren `ReadOnly` -Eigenschaftensatz auf `true`, verhindert, dass diese Werte geändert wird, wenn eine Zeile bearbeitet wird. Während wir festlegen können die `ReadOnly` Eigenschaft `false`, der Rendering der `CategoryName` und `SupplierName` BoundFields als Textfelder ein, während der Bearbeitung, ein solcher Ansatz führt zu einer Ausnahme, wenn der Benutzer versucht, das Produkt zu aktualisieren, da gibt es keine `UpdateProduct` Überladung, die in akzeptiert `CategoryName` und `SupplierName` Eingaben. Wir wollen nicht in der Tat solche eine Überladung aus zwei Gründen erstellen:

- Die `Products` Tabelle keine `SupplierName` oder `CategoryName` Felder jedoch `SupplierID` und `CategoryID`. Daher möchten wir unsere Methode, diese bestimmte ID-Werte, nicht die Nachschlagetabellen Werte übergeben werden sollen.
- Aufforderung des Benutzers zum Geben Sie den Namen des Lieferanten bzw. die Kategorie ist nicht ideal, da den Benutzer möchte die verfügbaren Kategorien und Lieferanten und die richtigen Schreibweisen erforderlich ist.

Die Felder Supplier "und" Kategorie sollte die Kategorie und Lieferanten Namen im schreibgeschützten Modus anzeigen (wie es jetzt) und eine Dropdown-Liste der entsprechenden Optionen, wenn es bearbeitet wird. Verwenden eine Dropdown-Liste, der Endbenutzer sehen sofort, welche Kategorien und Lieferanten zum Auswählen verfügbar sind und können problemlos gestalten können ihre Auswahl.

Um dieses Verhalten zu ermöglichen, müssen wir konvertieren die `SupplierName` und `CategoryName` BoundFields in von TemplateFields, deren `ItemTemplate` gibt die `SupplierName` und `CategoryName` Werte und deren `EditItemTemplate` verwendet ein DropDownList-Steuerelement zur Liste der Verfügbare Kategorien und Lieferanten.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Hinzufügen der`Categories`und`Suppliers`DropDownList-Steuerelementen

Konvertieren der `SupplierName` und `CategoryName` BoundFields in von TemplateFields durch: Klicken auf den Link "Spalten bearbeiten" aus den GridView Smarttag; die BoundField in der linken unteren Ecke, in der Liste auswählen und auf die "konvertieren Sie dieses Feld in einer Verknüpfen Sie das TemplateField". Erstellt der Konvertierungsprozess ein TemplateField mit sowohl ein `ItemTemplate` und ein `EditItemTemplate`, wie in die deklarative Syntax weiter unten dargestellt:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Da die BoundField als schreibgeschützt markiert wurde sowohl die `ItemTemplate` und `EditItemTemplate` enthalten eine Bezeichnung Web-Steuerelement, dessen `Text` Eigenschaft an die entsprechenden Datenfeld gebunden ist (`CategoryName`, in der obigen Syntax). Wir müssen zum Ändern der `EditItemTemplate`, und Ersetzen Sie dabei das Label-Steuerelement mit einem DropDownList-Steuerelement.

Wie wir in vorherigen Tutorials gesehen haben, kann die Vorlage mithilfe des Designers oder direkt über die deklarative Syntax bearbeitet werden. Um sie über den Designer zu bearbeiten, klicken auf den Link "Vorlagen bearbeiten" aus den GridView Smarttag, und wählen Sie des Kategoriefeld gearbeitet `EditItemTemplate`. Entfernen Sie das Label-Steuerelement, und Ersetzen Sie ihn mit einem DropDownList-Steuerelement, Festlegen von ID-Eigenschaft der DropDownList auf `Categories`.


[![Entfernen Sie das Textfeld und fügen Sie einem DropDownList-Steuerelement das EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Abbildung 5**: Entfernen Sie das Textfeld, und fügen Sie einem DropDownList-Steuerelement, das `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image15.png))


Als Nächstes müssen wir der Dropdownliste mit den verfügbaren Kategorien auffüllen. Klicken Sie auf den Link "Datenquelle auswählen" aus der Dropdownlistes Smarttag, und aktivieren Sie zum Erstellen einer neuen, mit dem Namen "ObjectDataSource" `CategoriesDataSource`.


[![Erstellen Sie ein neues ObjectDataSource-Steuerelement, das mit dem Namen CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Abbildung 6**: Erstellen einer neuen ObjectDataSource-Steuerelement mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image18.png))


Um diese "ObjectDataSource" alle Kategorien zurückgegeben haben, binden Sie es an der `CategoriesBLL` Klasse `GetCategories()` Methode.


[![Binden Sie dem ObjectDataSource-Steuerelement an die CategoriesBLL des GetCategories()-Methode](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Abbildung 7**: dem ObjectDataSource-Steuerelement zu binden der `CategoriesBLL`des `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image21.png))


Konfigurieren Sie abschließend DropDownLists-Einstellungen so, dass die `CategoryName` Feld wird angezeigt, in jeder DropDownList `ListItem` mit der `CategoryID` Feld als Wert verwendet.


[![Das Feld "CategoryName" angezeigt, und die CategoryID als Wert verwendet.](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Abbildung 8**: haben die `CategoryName` Feld angezeigt, und die `CategoryID` als Wert verwendet ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image24.png))


Nach dem deklarative Markup für vornehmen dieser Änderungen werden die `EditItemTemplate` in die `CategoryName` TemplateField umfasst sowohl einem DropDownList-Steuerelement und einer ObjectDataSource gegeben:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Das "DropDownList" in der `EditItemTemplate` muss seinen Ansichtszustand aktiviert haben. Wir werden es bald ergänzen Databinding-Syntax deklarative Syntax des DropDownList und Databinding-Befehle wie `Eval()` und `Bind()` darf nur in Steuerelemente, dessen Ansichtszustand aktiviert ist.


Wiederholen Sie diese Schritte zum Hinzufügen einer DropDownList mit dem Namen `Suppliers` auf die `SupplierName` TemplateFields `EditItemTemplate`. Dabei müssen Sie einem DropDownList-Steuerelement zum Hinzufügen der `EditItemTemplate` und erstellen eine andere "ObjectDataSource". Die `Suppliers` DropDownLists ObjectDataSource-Steuerelement, jedoch sollten werden so konfiguriert, dass die `SuppliersBLL` Klasse `GetSuppliers()` Methode. Konfigurieren Sie außerdem die `Suppliers` DropDownList zum Anzeigen der `CompanyName` Feld, und Verwenden der `SupplierID` Feld als Wert für die `ListItem` s.

Nach dem Hinzufügen der DropDownList-Steuerelementen, die beiden `EditItemTemplate` s, laden Sie die Seite in einem Browser, und klicken Sie auf die Schaltfläche "Bearbeiten" für die Chef-Anton Cajun Seasoning Produkt. Wie in Abbildung 9 gezeigt, werden die Spalten des Produkts "Kategorie" und "Supplier als Dropdownlisten mit den verfügbaren Kategorien und Lieferanten zur Auswahl gerendert. Beachten Sie jedoch, dass die *erste* Elemente in beiden Dropdownlisten sind standardmäßig ausgewählt, (für die Kategorie Getränke) und außergewöhnlichen fluessiger Form als Lieferant, obwohl der Chef-Anton Cajun Seasoning eine Condiment von New Orleans Cajun bereitgestellt wird Lerne.


[![Standardmäßig ist das erste Element in den Dropdownlisten ausgewählt.](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Abbildung 9**: das erste Element in den Dropdown-Listen in ist standardmäßig aktiviert ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image27.png))


Darüber hinaus, wenn Sie auf "Aktualisieren" klicken, Sie finden, des Produkts `CategoryID` und `SupplierID` Werte festgelegt sind `NULL`. Beide unerwünschte Verhaltensweisen verursacht werden, da die DropDownList-Steuerelementen in der `EditItemTemplate` s aus der zugrunde liegenden Produktdaten nicht an alle Datenfelder gebunden sind.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Die DropDownList-Steuerelementen zu binden der`CategoryID`und`SupplierID`von Datenfeldern

Damit Kategorie und Lieferanten des Produkts, auf die bearbeitete Dropdownlisten legen Sie diese Werte, die zurück an der BLL des gesendet haben und die entsprechenden Werte `UpdateProduct` Methode nach dem Klicken auf aktualisieren, müssen wir die DropDownList-Steuerelementen binden `SelectedValue` Eigenschaften, die die `CategoryID` und `SupplierID` Datenfelder, die bidirektionale Datenbindung verwenden. Hierzu mit der `Categories` DropDownList, fügen Sie `SelectedValue='<%# Bind("CategoryID") %>'` direkt mit der deklarativen Syntax.

Alternativ können Sie DropDownLists-Databindings festlegen, durch Bearbeiten der Vorlage mithilfe des Designers, und klicken auf den Link "DataBindings bearbeiten" aus der Dropdownlistes Smarttag. Als Nächstes angeben, die die `SelectedValue` Eigenschaft gebunden werden soll die `CategoryID` Feld mithilfe von bidirektionalen Datenbindung (siehe Abbildung 10). Wiederholen Sie den deklarativen oder Designer-Prozess binden die `SupplierID` Datenfeld der `Suppliers` DropDownList.


[![Binden Sie die CategoryID an bidirektionale Datenbindung Verwenden des DropDownList SelectedValue-Eigenschaft](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Abbildung 10**: Binden der `CategoryID` zu DropDownList `SelectedValue` Eigenschaft mithilfe von bidirektionalen Datenbindung ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image30.png))


Sobald die Bindungen angewendet wurden, die `SelectedValue` Eigenschaften von zwei DropDownList-Steuerelementen, Kategorie und Lieferanten des Produkts, auf die bearbeitete-Spalten werden standardmäßig das aktuelle Produkt Werte. Nach dem Klicken auf aktualisieren, die `CategoryID` und `SupplierID` Werte des Elements ausgewählten Dropdown-Listenfeld zum Übergeben der `UpdateProduct` Methode. Abbildung 11 zeigt das Lernprogramm aus, nachdem die Databinding-Anweisungen hinzugefügt wurden; Beachten Sie, wie die ausgewählten Dropdown-Listenfeld-Elemente für Chef Anton's Cajun Seasoning ordnungsgemäß Condiment "und" New Orleans Cajun lerne werden.


[![Die bearbeitet und des Produkts aktuelle Kategorie Lieferanten Werte sind standardmäßig ausgewählt.](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Abbildung 11**: die bearbeitet und des Produkts aktuelle Kategorie Lieferanten Werte sind standardmäßig ausgewählt ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Behandeln von`NULL`Werte

Die `CategoryID` und `SupplierID` Spalten in der `Products` Tabelle möglich `NULL`, noch die DropDownList-Steuerelementen in der `EditItemTemplate` s enthalten kein Listenelement zur Darstellung einer `NULL` Wert. Dies hat zwei Auswirkungen:

- Benutzer kann die Schnittstelle nicht verwenden, so ändern Sie die Kategorie oder Lieferant eines Produkts nicht von einem`NULL` -Werts in einen `NULL` eine
- Wenn ein Produkt verfügt über eine `NULL` `CategoryID` oder `SupplierID`, klicken Sie auf die Schaltfläche "Bearbeiten" führt zu einer Ausnahme. Grund hierfür ist, die `NULL` Rückgabewert von `CategoryID` (oder `SupplierID`) in der `Bind()` Anweisung ordnet nicht auf einen Wert in der Dropdownliste (DropDownList löst eine Ausnahme aus. wenn die `SelectedValue` -Eigenschaftensatz auf einen Wert *nicht* in der Sammlung von Elementen).

Zur Unterstützung `NULL` `CategoryID` und `SupplierID` Werte müssen wir zum Hinzufügen eines weiteren `ListItem` auf jede DropDownList zur Darstellung der `NULL` Wert. In der [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Tutorial erläutert, wie Sie einen zusätzlichen hinzufügen `ListItem` auf einem datengebundenen DropDownList, die Beteiligten Festlegen des DropDownList `AppendDataBoundItems` Eigenschaft `true` und Manuelles Hinzufügen der zusätzlichen `ListItem`. In diesem Tutorial vorherigen, allerdings wir hinzugefügt eine `ListItem` mit einem `Value` von `-1`. Allerdings die Databinding-Logik in ASP.NET konvertiert automatisch eine leere Zeichenfolge, die eine `NULL` Wert und einen-umgekehrt. Aus diesem Grund für dieses Lernprogramm soll die `ListItem`des `Value` eine leere Zeichenfolge sein.

Beide DropDownLists setzen Sie zunächst `AppendDataBoundItems` Eigenschaft `true`. Fügen Sie als Nächstes die `NULL` `ListItem` durch Hinzufügen des folgenden `<asp:ListItem>` Element jedes DropDownList, damit der deklarative Markup sieht aus wie:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

Ich habe beschlossen, verwenden Sie "(None)" als Textwert für diesen `ListItem`, aber Sie können ändern, um auch eine leere Zeichenfolge sein, wenn Sie möchten.

> [!NOTE]
> Wie in beschrieben der *Master/Detail-Filtern mit einer DropDownList* Tutorial `ListItem` s kann zu einem DropDownList-Steuerelement mit dem Designer hinzugefügt werden, durch Klicken auf die DropDownList `Items` Eigenschaft im Eigenschaftenfenster (die Zeigt die `ListItem` Auflistungs-Editor). Achten Sie jedoch zum Hinzufügen der `NULL` `ListItem` für dieses Tutorial mithilfe der deklarativen Syntax. Bei Verwendung der `ListItem` Auflistungs-Editor für die deklarative Syntax des generierte lässt die `Value` Einstellung vollständig, wenn eine leere Zeichenfolge, und Erstellen von deklarativen Markup wie zugewiesen: `<asp:ListItem>(None)</asp:ListItem>`. Während dieser harmlos aussehen kann, der fehlende Wert bewirkt, dass der Dropdownliste mit den `Text` Eigenschaftswert an. Das bedeutet, dass bei dieser `NULL` `ListItem` wird ausgewählt, der Wert "(None)" wird versucht, zugewiesen werden soll die `CategoryID`, die führt zu einer Ausnahme. Durch das explizite Festlegen `Value=""`, `NULL` Wert zugewiesen wird, `CategoryID` bei der `NULL` `ListItem` ausgewählt ist.


Wiederholen Sie diese Schritte für die Lieferanten DropDownList.

Mit dieser zusätzlichen `ListItem`, können Sie jetzt die Bearbeitungsschnittstelle zuweisen `NULL` Werte eines Produkts `CategoryID` und `SupplierID` Felder, wie in Abbildung 12 dargestellt.


[![Wählen Sie (keine), weisen Sie einen NULL-Wert für die Kategorie- bzw. Lieferanteninformationen eines Produkts](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Abbildung 12**: zuweisen möchten (keine) eine `NULL` Wert für die Kategorie- bzw. Lieferanteninformationen eines Produkts ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Schritt 4: Verwenden von RadioButtons für den nicht mehr unterstützte Status

Derzeit des Produkts `Discontinued` Feld "Daten" wird angegeben, mit einem CheckBoxField, das ein deaktiviertes Kontrollkästchen für die Zeilen nur-Lese und eine aktivierte Kontrollkästchen für die bearbeitete Zeile gerendert wird. Während dieser Benutzeroberfläche oft geeignet ist, können wir es über ein TemplateField bei Bedarf anpassen. In diesem Tutorial ändern CheckBoxField in ein TemplateField, die ein RadioButtonList-Steuerelement mit den beiden Optionen "Aktiv" und "Nicht mehr unterstützt" verwendet, von dem der Benutzer des Produkts angeben kann `Discontinued` Wert.

Konvertieren Sie zunächst die `Discontinued` CheckBoxField in ein TemplateField, dadurch wird ein TemplateField mit erstellt, ein `ItemTemplate` und `EditItemTemplate`. Beide Vorlagen enthalten ein Kontrollkästchen mit der `Checked` -Eigenschaft gebunden werden, um die `Discontinued` Datenfeld, der einzige Unterschied zwischen den beiden, die die `ItemTemplate`des Kontrollkästchens `Enabled` -Eigenschaftensatz auf `false`.

Ersetzen Sie das Kontrollkästchen in der sowohl die `ItemTemplate` und `EditItemTemplate` ein RadioButtonList-Steuerelement, Festlegen von beiden RadioButtonList `ID` Eigenschaften `DiscontinuedChoice`. Geben Sie als Nächstes, die die RadioButtonList jeweils zwei Optionsfelder, eine mit der Bezeichnung "aktiv" sollte mit dem Wert "False" und eine, die mit der Bezeichnung "Discontinued" mit dem Wert "True". Um dies zu erreichen, Sie, entweder eingeben können, die `<asp:ListItem>` Elemente im direkt über die deklarative Syntax, oder Verwenden der `ListItem` Auflistungs-Editor im Designer. Abbildung 13 zeigt die `ListItem` Auflistungs-Editor, nachdem die beiden Optionen für Optionsfelder angegeben wurden.


[![Add](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Abbildung 13**: Hinzufügen von "Aktiv" und "Discontinued"-Optionen zu RadioButtonList ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image39.png))


Seit RadioButtonList in die `ItemTemplate` darf nicht bearbeitet werden, werden Festlegen der `Enabled` Eigenschaft, um `false`dadurch bleibt die `Enabled` Eigenschaft, um `true` (Standard) für RadioButtonList in die `EditItemTemplate`. Dadurch wird die Optionsfelder in der Zeile nicht bearbeitet werden, als schreibgeschützt, aber kann der Benutzer zum Ändern der RadioButton-Werte für die bearbeitete Zeile.

Wir benötigen die RadioButtonList-Steuerelementen zuweisen `SelectedValue` Eigenschaften, sodass das entsprechende Optionsfeld ausgewählt ist, auf Grundlage des Produkts `Discontinued` Feld "Daten". Wie bei der DropDownList-Steuerelementen überprüft, die weiter oben in diesem Tutorial, kann diese Syntax Databinding entweder direkt in deklarativen Markup oder über den DataBindings bearbeiten-Link in der RadioButtonList Smarttags hinzugefügt werden.

Nach dem Hinzufügen der zwei RadioButtonList und konfigurieren sie die `Discontinued` deklarative TemplateFields-Markup sollte wie folgt aussehen:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Mit diesen Änderungen die `Discontinued` Spalte aus einer Liste mit Kontrollkästchen umgewandelt wurde, um eine Liste von Optionsfeld-Schaltfläche-Paaren (siehe Abbildung 14). Wenn Sie ein Produkt zu bearbeiten, das entsprechende Optionsfeld ausgewählt ist, und des Produkts nicht mehr unterstützte Status kann aktualisiert werden, indem Sie das Optionsfeld "andere" auswählen und durch Klicken auf aktualisieren.


[![Die nicht mehr unterstützte Kontrollkästchen wurden durch Radio Schaltfläche Paare ersetzt](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Abbildung 14**: die nicht mehr unterstützte Kontrollkästchen wurden durch ersetzt Radio Schaltfläche Paare ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Da die `Discontinued` -Spalte in der `Products` Datenbank sind keine `NULL` Werte, wir müssen nicht kümmern, Erfassen von `NULL` Informationen in der Schnittstelle. IF, allerdings `Discontinued` Spalte enthalten kann `NULL` Werte würde eine dritte hinzugefügt werden soll Optionsfeld, um die Liste mit seiner `Value` auf eine leere Zeichenfolge festgelegt (`Value=""`) genau wie mit der Kategorie und Lieferanten DropDownList-Steuerelementen.


## <a name="summary"></a>Zusammenfassung

Während die BoundField- und CheckBoxField automatisch Rendern schreibgeschützt, bearbeiten und Einfügen von Schnittstellen, ihnen die Möglichkeit für die Anpassung fehlt. Oft, jedoch müssen wir die Bearbeitung oder Schnittstelle Einfügen anpassen, z. B. Hinzufügen von Validierungssteuerelementen (wie im vorherigen Tutorial beschrieben) oder durch das Anpassen der Benutzeroberfläche des Daten-Auflistung, (wie in diesem Tutorial beschrieben). Anpassen der Schnittstelle mit der ein TemplateField kann in den folgenden Schritten zusammengefasst werden:

1. Ein TemplateField hinzuzufügen oder zu einem vorhandenen BoundField- oder CheckBoxField in ein TemplateField konvertieren
2. Die Schnittstelle zu erweitern, je nach Bedarf
3. Binden Sie die entsprechenden Daten-Felder, die neu hinzugefügte-Websteuerelemente verwenden bidirektionale Datenbindung

Zusätzlich zu den integrierten ASP.NET Web-Steuerelemente verwenden, können Sie auch die Vorlagen des ein TemplateField mit benutzerdefinierten, kompilierte Serversteuerelemente und Benutzersteuerelemente anpassen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [Weiter](implementing-optimistic-concurrency-cs.md)
