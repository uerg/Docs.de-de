---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: "Anpassen der Benutzeroberfläche der Datenänderung (VB) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm lernen wir an, wie die Schnittstelle für einen bearbeitbaren GridView anpassen, indem Sie das standard-Textfeld ersetzen und das Kontrollkästchen-Steuerelemente mit Alternati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f8023efd4d0b32e81dd3aab70e6e7521066fc84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-data-modification-interface-vb"></a>Anpassen der Benutzeroberfläche der Datenänderung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) oder [PDF herunterladen](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> In diesem Lernprogramm lernen wir an, wie die Schnittstelle für einen bearbeitbaren GridView anpassen, indem Sie das standard-Textfeld ersetzen und Kontrollkästchen mit alternativen Web Eingabesteuerelemente steuert.


## <a name="introduction"></a>Einführung

Die BoundFields und CheckBoxFields verwendeten GridView und DetailsView vereinfachen das Ändern von Daten, die aufgrund ihrer Fähigkeit, schreibgeschützt, bearbeitet und einfügbare Schnittstellen zu rendern. Diese Schnittstellen können ohne zusätzliche deklarativem Markup oder Code hinzugefügt gerendert werden. Die BoundField- und des CheckBoxField Schnittstellen fehlt jedoch der Berichtsebene häufig in realen Szenarios erforderlich sind. Um die bearbeitbar oder einfügbar Schnittstelle in einer GridView oder DetailsView anpassen müssen wir stattdessen ein TemplateField verwenden.

In der [vorherigen Lernprogramm](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) wurde erläutert, wie Schnittstellen für die Änderung durch Hinzufügen von Validierung Websteuerelemente anpassen. In diesem Lernprogramm lernen wir an, wie die tatsächlichen Daten Auflistung Web Controls, ersetzen Sie dabei die BoundField- und CheckBoxFields standard-Textfeld und CheckBox-Steuerelementen mit alternativen Web Eingabesteuerelemente anpassen. Insbesondere müssen wir eine bearbeitbare GridView erstellen, die ein Produkt Name, Kategorie, Lieferanten und nicht mehr unterstützte Status aktualisiert werden können. Wenn Sie eine bestimmte Zeile zu bearbeiten, werden Felder Kategorie und Lieferanten als DropDownLists, gerendert, enthält den Satz der verfügbaren Kategorien und Lieferanten zur Auswahl. Darüber hinaus wird die CheckBoxField Standard Kontrollkästchen durch ein RadioButtonList-Steuerelement, das zwei Optionen bietet ersetzt: "Aktiv" und "Nicht mehr unterstützt".


[![Die GridView bearbeiten Schnittstelle umfasst DropDownLists und Optionsfelder](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Abbildung 1**: die GridView bearbeiten Schnittstelle enthält DropDownLists und Optionsfelder ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Schritt 1: Erstellen der entsprechenden`UpdateProduct`überladen

In diesem Lernprogramm werden wir eine bearbeitbare GridView erstellen, die erlaubt Bearbeitung der Name des Produkts, Kategorie, Lieferanten und Status eingestellt. Aus diesem Grund müssen wir ein `UpdateProduct` Überladung, die fünf Eingabeparameter diese vier produktwerte akzeptiert plus dem `ProductID`. Wie Sie in unserer vorherigen Überladungen, dadurch ein:

1. Rufen Sie die Produktinformationen aus der Datenbank für den angegebenen `ProductID`,
2. Update der `ProductName`, `CategoryID`, `SupplierID`, und `Discontinued` Felder, und
3. Die updateanforderung an die DAL durch des TableAdapters senden `Update()` Methode.

Aus Gründen der Übersichtlichkeit haben für dieses bestimmte Überladung ich die regelüberprüfung Business weggelassen wird, die sicherstellt, dass ein Produkt gekennzeichnet werden, wie nicht mehr unterstützt nicht das einzige Produkt, das durch seine Lieferanten angeboten ist. Gerne in hinzufügen, wenn Sie es vorziehen, oder, im Idealfall Umgestalten out Logik, um eine separate Methode.

Der folgende Code zeigt das neue `UpdateProduct` überladen, die der `ProductsBLL` Klasse:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Schritt 2: Erstellen der bearbeitbaren GridView

Mit der `UpdateProduct` Überladung hinzugefügt, die wir jetzt können Sie unsere bearbeitbaren GridView zu erstellen. Öffnen der `CustomizedUI.aspx` auf der Seite der `EditInsertDelete` Ordner und Hinzufügen eines GridView-Steuerelements in den Designer. Als Nächstes erstellen Sie eine neue ObjectDataSource aus der GridView Smarttag. Konfigurieren der ObjectDataSource zum Abrufen von Produktinformationen über die `ProductBLL` Klasse `GetProducts()` Methode und zum Aktualisieren von Daten verwenden die `UpdateProduct` Überladung, die soeben erstellt wurde. Wählen Sie aus den Registerkarten INSERT- und DELETE (keine) aus den Dropdown-Listen.


[![Konfigurieren der ObjectDataSource Verwendung die UpdateProduct-Überladung, die gerade erstellt haben](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Abbildung 2**: Konfigurieren der ObjectDataSource verwenden die `UpdateProduct` erstellte überladen ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image6.png))


Wie wir in den Daten Änderungen Lernprogrammen gesehen haben, weist die deklarative Syntax für das ObjectDataSource von Visual Studio erstellt die `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Dies natürlich funktioniert nicht mit unserem Business Logic Layer da unsere Methoden erwarten, dass die ursprüngliche nicht `ProductID` Wert übergeben werden muss. Wie wir im vorherigen Lernprogrammen ausgeführt haben, schalten Sie daher einen Moment Zeit, um die deklarative Syntax dieser Eigenschaft die Zuweisung aufheben, oder legen Sie stattdessen der Wert dieser Eigenschaft auf `{0}`.

Nach dieser Änderung sollte das ObjectDataSource deklarative Markup wie folgt aussehen:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Beachten Sie, dass die `OldValuesParameterFormatString` Eigenschaft entfernt wurde und dass es eine `Parameter` in der `UpdateParameters` Auflistung aller den Eingabeparametern unsere `UpdateProduct` überladen.

Während der ObjectDataSource konfiguriert wird, um nur eine Teilmenge der Product-Werte zu aktualisieren, zeigt die GridView momentan *alle* der Product-Felder. Nehmen Sie einen Moment Zeit, um die GridView zu bearbeiten, damit:

- Es enthält nur die `ProductName`, `SupplierName`, `CategoryName` BoundFields und `Discontinued` CheckBoxField
- Die `CategoryName` und `SupplierName` Felder vor (auf der linken Seite des) aufgeführt werden die `Discontinued` CheckBoxField
- Die `CategoryName` und `SupplierName` 'BoundFields `HeaderText` Eigenschaft auf "Kategorie" und "Supplier", bzw. festgelegt ist
- Bearbeiten-Unterstützung aktiviert ist (Überprüfen Sie das Kontrollkästchen Bearbeiten aktivieren, in die GridView-Smarttag)

Nach der Änderung wird der Designer Abbildung 3: mit der GridView deklarative Syntax unten gezeigten ähneln.


[![Entfernen Sie nicht benötigte Felder aus der GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Abbildung 3**: Entfernen Sie nicht benötigte Felder aus der GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

Die GridView schreibgeschütztes Verhalten ist zu diesem Zeitpunkt abgeschlossen. Beim Anzeigen der Daten jedes Produkt wird als eine Zeile in der GridView, die mit den Namen des Produkts, Kategorie, Hersteller, gerendert, und Sie werden nicht mehr unterstützte Status.


[![Der GridView-Read-Only-Schnittstelle ist abgeschlossen.](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Abbildung 4**: die GridView-Read-Only-Schnittstelle ist abgeschlossen. ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Entsprechend der Anleitung unter [eine Übersicht der einfügen, aktualisieren und Löschen von Daten Lernprogramm](an-overview-of-inserting-updating-and-deleting-data-cs.md), es ist äußerst wichtig, dass die GridView Ansichtszustand s werden (das Standardverhalten) aktiviert. Wenn Sie festlegen, dass die GridView s `EnableViewState` Eigenschaft `false`, laufen Sie Gefahr, dass gleichzeitige Benutzer versehentlich löschen oder Bearbeiten von Datensätzen. Finden Sie unter [Warnung: Parallelität Problem mit ASP.NET 2.0 GridViews/DetailsView/FormViews, Bearbeitung unterstützen und/oder löschen und, deren Ansichtszustand deaktiviert](http://scottonwriting.net/sowblog/posts/10054.aspx) für Weitere Informationen.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Schritt 3: Verwenden einer DropDownList für die Kategorie und Lieferanten, die Bearbeitung von Schnittstellen

Bedenken Sie, dass die `ProductsRow` Objekt enthält `CategoryID`, `CategoryName`, `SupplierID`, und `SupplierName` Eigenschaften, die die Fremdschlüssel-ID Istwerte in bieten die `Products` Datenbank, Tabelle und die entsprechende `Name` Werte in der `Categories` und `Suppliers` Tabellen. Die `ProductRow`des `CategoryID` und `SupplierID` können sowohl aus gelesen und geschrieben werden, dagegen die `CategoryName` und `SupplierName` Eigenschaften werden als schreibgeschützt gekennzeichnet.

Aufgrund der schreibgeschützte Status der `CategoryName` und `SupplierName` Eigenschaften, die entsprechenden BoundFields hatten ihre `ReadOnly` -Eigenschaftensatz auf `True`, verhindert, dass diese Werte geändert werden, wenn eine Zeile bearbeitet wird. Wir können zwar die `ReadOnly` Eigenschaft `False`, Rendering der `CategoryName` und `SupplierName` BoundFields als Textfelder ein, während der Bearbeitung, ein derartiger Ansatz wird eine Ausnahme ausgelöst, wenn der Benutzer versucht, das Produkt zu aktualisieren, da es ist keine `UpdateProduct` Überladung, die in akzeptiert `CategoryName` und `SupplierName` Eingaben. Wir möchte nicht tatsächlich eine Überladung verwendet zwei Gründen erstellen:

- Die `Products` Tabelle verfügt nicht über `SupplierName` oder `CategoryName` Felder, jedoch `SupplierID` und `CategoryID`. Daher möchten wir unsere Methode, um diese bestimmte ID-Werte, nicht ihre Nachschlagetabellen Werte übergeben werden.
- Geben Sie den Namen der Lieferanten oder Kategorie Benutzereingriff eignet sich weniger als, benötigt wird, die Benutzer wissen, die verfügbaren Kategorien und Lieferanten und die richtige Schreibweise.

Die Felder Supplier "und" Kategorie sollte anzeigen, die Kategorie und Lieferanten Namen im Nur-Lese-Modus (wie jetzt) und eine Dropdown-Liste der entsprechenden Optionen, wenn es bearbeitet wird. Verwenden eine Dropdown-Liste, die Endbenutzer können auf Anhieb, welche Kategorien und Lieferanten auszuwählen verfügbar sind und können weitere mühelos ihrer Auswahl vornehmen.

Um dieses Verhalten zu ermöglichen, müssen wir konvertieren die `SupplierName` und `CategoryName` BoundFields in von TemplateFields, deren `ItemTemplate` ausgibt, die `SupplierName` und `CategoryName` Werte und deren `EditItemTemplate` verwendet ein DropDownList-Steuerelement zur Liste der Verfügbare Kategorien und Lieferanten.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Hinzufügen der`Categories`und`Suppliers`DropDownLists

Konvertieren der `SupplierName` und `CategoryName` BoundFields in von TemplateFields durch: durch Klicken auf den Link Bearbeiten von Spalten aus der GridView-Smarttag; die BoundField in der unteren linken Ecke; in der Liste auswählen und auf die "konvertieren Sie dieses Feld in einer TemplateField"verknüpfen. Konvertierungsprozess erstellt ein TemplateField mit einer `ItemTemplate` und ein `EditItemTemplate`, wie in der folgenden deklarative Syntax gezeigt:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Da die BoundField als schreibgeschützt gekennzeichnet wurde sowohl die `ItemTemplate` und `EditItemTemplate` enthalten eine Bezeichnung Web Steuerelement, dessen `Text` Eigenschaft zum entsprechenden Datenfeld gebunden ist (`CategoryName`, in der obigen Syntax). Wir müssen so ändern Sie die `EditItemTemplate`, und Ersetzen Sie dabei das Label-Steuerelement mit einem DropDownList-Steuerelement.

Wie wir im vorherigen Lernprogrammen gesehen haben, kann die Vorlage mithilfe des Designers oder direkt über die deklarative Syntax bearbeitet werden. Um ihn durch den Designer zu bearbeiten, klicken Sie auf die Verknüpfung Vorlagen bearbeiten die GridView Smarttag, und wählen Sie des Kategoriefeld portzuweisung `EditItemTemplate`. Entfernen Sie das Label-Steuerelement, und Ersetzen Sie sie mit einem DropDownList-Steuerelement, das Festlegen der DropDownList-ID-Eigenschaft auf `Categories`.


[![Entfernen Sie die TextBox und ItemTemplate DropDownList hinzu](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Abbildung 5**: Entfernen Sie die TextBox und DropDownList zum Hinzufügen der `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image15.png))


Wir müssen neben der Dropdownliste mit verfügbaren Kategorien auffüllen. Klicken Sie auf den Link Datenquelle wählen Sie aus der DropDownList-Smarttag, und erstellen Sie eine neue, mit dem Namen ObjectDataSource wahlweise `CategoriesDataSource`.


[![Erstellen Sie ein neues ObjectDataSource-Steuerelement mit dem Namen CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Abbildung 6**: Erstellen einer neuen ObjectDataSource-Steuerelement mit dem Namen `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image18.png))


Um diese ObjectDataSource alle Kategorien zurückgegeben haben, binden Sie es an die `CategoriesBLL` Klasse `GetCategories()` Methode.


[![Binden Sie das ObjectDataSource an die CategoriesBLL GetCategories()-Methode](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Abbildung 7**: das ObjectDataSource zum Binden der `CategoriesBLL`des `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image21.png))


Schließlich der DropDownList-Einstellungen konfigurieren, dass die `CategoryName` Feld wird angezeigt, in jeder DropDownList `ListItem` mit der `CategoryID` Feld als Wert verwendet.


[![Die CategoryName-Feld angezeigt und CategoryID als Wert verwendet.](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Abbildung 8**: haben die `CategoryName` Feld angezeigt, und die `CategoryID` als Wert verwendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image24.png))


Nachdem diese deklarative Markup für Änderungen auf die `EditItemTemplate` in die `CategoryName` TemplateField DropDownList und einem ObjectDataSource enthält:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Der DropDownList in der `EditItemTemplate` muss seinen Ansichtszustand aktiviert haben. Wir werden es bald ergänzen Databinding-Syntax der DropDownList deklarative Syntax und Databinding-Befehle wie `Eval()` und `Bind()` kann nur verwendet werden, in Steuerelementen, dessen Ansichtszustand aktiviert ist.


Wiederholen Sie diese Schritte zum Hinzufügen einer DropDownList mit dem Namen `Suppliers` auf die `SupplierName` TemplateFields `EditItemTemplate`. Dies umfasst das Hinzufügen einer DropDownList auf die `EditItemTemplate` und erstellen eine andere ObjectDataSource. Die `Suppliers` DropDownLists ObjectDataSource, allerdings sollten konfiguriert werden zum Aufrufen der `SuppliersBLL` Klasse `GetSuppliers()` Methode. Konfigurieren Sie außerdem die `Suppliers` DropDownList zum Anzeigen der `CompanyName` Feld, und verwenden Sie die `SupplierID` -Felds ist, als der Wert für seine `ListItem` s.

Nach dem Hinzufügen der DropDownLists auf die beiden `EditItemTemplate` s, laden Sie die Seite in einem Browser, und klicken Sie auf die Schaltfläche "Bearbeiten" für die Chef Anton Cajun Seasoning Produkt. Wie in Abbildung 9 gezeigt, werden die Product Category und Lieferanten Spalten als Dropdownlisten mit den verfügbaren Kategorien und Lieferanten zur Auswahl gerendert. Beachten Sie jedoch, dass die *erste* Elemente in beiden Dropdown-Listen sind standardmäßig ausgewählt, (für die Kategorie Getränke) und Exotic Liquids als Lieferant, obwohl Chef Anton's Cajun Seasoning eine Condiment von New Orleans Cajun bereitgestellt wird Delights.


[![Das erste Element in der Dropdown-Listen ist standardmäßig aktiviert.](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Abbildung 9**: das erste Element in der Dropdown-Listen ist standardmäßig aktiviert ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image27.png))


Darüber hinaus, wenn Sie auf "Aktualisieren" klicken, Sie feststellen, dass des Produkts `CategoryID` und `SupplierID` Werte festgelegt sind `NULL`. Beide unerwünschte Verhalten verursacht werden, da die DropDownLists in die `EditItemTemplate` s aus der zugrunde liegenden Produktdaten nicht an alle Datenfelder gebunden sind.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Die DropDownLists zum Binden der`CategoryID`und`SupplierID`von Datenfeldern

Um das bearbeiteten Produkts Kategorie und Lieferanten haben Dropdownlisten legen Sie diese Werte, die zurück an der BLL gesendet haben und die entsprechenden Werte `UpdateProduct` -Methode beim Update klicken, müssen wir die DropDownLists binden `SelectedValue` Eigenschaften, die `CategoryID` und `SupplierID` Datenfelder, die bidirektionale Datenbindung verwenden. Hierfür mit der `Categories` DropDownList, fügen Sie `SelectedValue='<%# Bind("CategoryID") %>'` direkt an die deklarative Syntax.

Alternativ können Sie der DropDownList Databindings festlegen, durch die Bearbeitung der Vorlage mithilfe des Designers, und dann auf die Verknüpfung der DropDownList-Smarttag-Datenbindungen bearbeiten. Als Nächstes darauf hinweisen, dass die `SelectedValue` Eigenschaft gebunden werden soll die `CategoryID` Feld verwenden der bidirektionalen Datenbindung (siehe Abbildung 10). Wiederholen Sie den deklarativen oder Designer-Prozess Binden der `SupplierID` Datenfeld der Spalte die `Suppliers` DropDownList.


[![Binden Sie CategoryID "SelectedValue"-Eigenschaft der DropDownList verwenden der bidirektionalen Datenbindung](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Abbildung 10**: Binden der `CategoryID` der DropDownList `SelectedValue` Eigenschaft mithilfe von bidirektionalen Databinding ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image30.png))


Nachdem die Bindungen auf angewendet wurden die `SelectedValue` Eigenschaften von zwei DropDownLists, die bearbeitete Product Category und Lieferanten Spalten werden standardmäßig auf das aktuelle Produkt-Werte. Nachdem Sie das Update auf die `CategoryID` und `SupplierID` Werte des Elements ausgewählten Dropdown-Liste werden übergeben werden, um die `UpdateProduct` Methode. Abbildung 11 zeigt das Lernprogramm, nachdem die Databinding-Anweisungen hinzugefügt wurden; Beachten Sie, wie die Elemente der ausgewählten Dropdown-Liste für Chef Anton's Cajun Seasoning ordnungsgemäß Condiment und New Orleans Cajun Delights sind.


[![Das bearbeitet Produkt aktuelle Kategorie und Lieferanten Werte sind standardmäßig ausgewählt.](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Abbildung 11**: die Bearbeitung des Produkts aktuelle Kategorie und Lieferanten Werte sind standardmäßig ausgewählt ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Behandlung von`NULL`Werte

Der `CategoryID` und `SupplierID` Spalten in der `Products` Tabelle kann `NULL`, noch die DropDownLists im der `EditItemTemplate` s enthalten kein Listenelement zur Darstellung einer `NULL` Wert. Dies hat zwei Auswirkungen:

- Benutzer können keine unsere Schnittstelle so ändern Sie ein Produkt Kategorie oder Lieferanten aus einem nicht-`NULL` -Wert in einen `NULL` eine
- Wenn ein Produkt verfügt über eine `NULL` `CategoryID` oder `SupplierID`, auf die Schaltfläche "Bearbeiten" eine Ausnahme ausgelöst wird. Grund hierfür ist, die `NULL` Rückgabewert von `CategoryID` (oder `SupplierID`) in der `Bind()` Anweisung zuordnen nicht auf einen Wert in der Dropdownliste (DropDownList löst eine Ausnahme aus. wenn seine `SelectedValue` Eigenschaft auf einen Wert festgelegtist*nicht* in die Auflistung von Elementen in der).

Um unterstützen `NULL` `CategoryID` und `SupplierID` Werte, wir müssen einen anderen `ListItem` jedes DropDownList zum Darstellen der `NULL` Wert. In der [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Lernprogramm wurde erläutert, wie zusätzliche `ListItem` auf einem datengebundenen DropDownList, die Beteiligten Festlegen der DropDownList `AppendDataBoundItems` Eigenschaft `True` und Manuelles Hinzufügen der zusätzlichen `ListItem`. In diesem Lernprogramm vorherigen, allerdings wir hinzugefügt eine `ListItem` mit einem `Value` von `-1`. Allerdings die Databinding-Logik in ASP.NET Wandelt eine leere Zeichenfolge, die automatisch eine `NULL` Wert und eine umgekehrt. Aus diesem Grund für dieses Lernprogramm soll die `ListItem`des `Value` eine leere Zeichenfolge sein.

Beginnen Sie, indem Sie beide DropDownLists festlegen `AppendDataBoundItems` Eigenschaft `True`. Als Nächstes fügen Sie der `NULL` `ListItem` durch das Hinzufügen der folgenden `<asp:ListItem>` Element jedes DropDownList Pyramide deklarative Markup wie z. B.:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Ich habe "(keine)" zu verwenden als Textwert dafür `ListItem`, aber Sie können ändern, um auch eine leere Zeichenfolge sein, wenn Sie möchten.

> [!NOTE]
> Wie wir gesehen, in haben der *Master/Detail-Filtern mit einer DropDownList* Lernprogramm `ListItem` kann s DropDownList mithilfe des Designers hinzugefügt werden, indem Sie auf der DropDownList `Items` Eigenschaft im Eigenschaftenfenster angezeigt (die Zeigt die `ListItem` Auflistungs-Editor). Allerdings müssen Sie zum Hinzufügen der `NULL` `ListItem` für dieses Lernprogramm durch die deklarative Syntax. Bei Verwendung der `ListItem` Auflistungs-Editor für die generierte deklarative Syntax wird weglassen der `Value` Einstellung ganz, wenn eine leere Zeichenfolge, und Erstellen von deklarativem Markup wie zugewiesen: `<asp:ListItem>(None)</asp:ListItem>`. Dies harmlos aussehen könnte, der fehlende Wert führt dazu, dass der DropDownList zum Verwenden der `Text` Eigenschaftswert an seiner Stelle. Dies bedeutet, dass bei dieser `NULL` `ListItem` ist ausgewählt, der Wert "(keine)" wird versucht, zugewiesen werden soll der `CategoryID`, was zu einer Ausnahme führt. Durch das explizite Festlegen `Value=""`, `NULL` Wert zugewiesen wird, `CategoryID` bei der `NULL` `ListItem` ausgewählt ist.


Wiederholen Sie diese Schritte für die Lieferanten DropDownList.

Mit dieser zusätzlichen `ListItem`, die Bearbeitungsoberfläche kann jetzt zuweisen `NULL` Werte zu einem Produkt `CategoryID` und `SupplierID` Felder, wie in Abbildung 12 dargestellt.


[![Wählen Sie (keine), einen NULL-Wert für ein Produkt Kategorie oder-Lieferanten in Verbindung zuzuweisen](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Abbildung 12**: Wählen Sie (keine), weisen eine `NULL` Wert für ein Produkt Kategorie oder-Lieferanten in Verbindung ([klicken Sie, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Schritt 4: Mithilfe der Optionsfelder für die nicht mehr unterstützte Status

Derzeit des Produkts `Discontinued` Feld "Daten" wird angegeben, mit einem CheckBoxField, die rendert ein deaktiviertes Kontrollkästchen für die Zeilen nur-Lese und einem aktivierten Kontrollkästchen für die Zeile, die bearbeitet wird. Während dieser Benutzeroberfläche häufig geeignet ist, können wir ihn über ein TemplateField bei Bedarf anpassen. Für dieses Lernprogramm ändern CheckBoxField in ein TemplateField, die ein RadioButtonList-Steuerelement mit beiden Optionen "Aktiv" und "Discontinued" wird verwendet, von dem der Benutzer kann des Produkts angeben `Discontinued` Wert.

Konvertieren der `Discontinued` CheckBoxField in ein TemplateField, wodurch ein TemplateField mit erstellt, wird ein `ItemTemplate` und `EditItemTemplate`. Beide Vorlagen enthalten ein Kontrollkästchen mit seiner `Checked` Eigenschaft gebunden wird, um die `Discontinued` Datenfeld der Spalte, den einzigen Unterschied zwischen den beiden wird, die `ItemTemplate`des Kontrollkästchens `Enabled` -Eigenschaftensatz auf `False`.

Ersetzen Sie das Kontrollkästchen in den beiden die `ItemTemplate` und `EditItemTemplate` mit einem Steuerelement RadioButtonList festlegen beide RadioButtonList `ID` Eigenschaften `DiscontinuedChoice`. Als Nächstes geben Sie an, dass die RadioButtonList jeweils zwei Optionsfelder, eine mit der Bezeichnung "aktiv" sollte mit dem Wert "False" und eine, die mit der Bezeichnung "Discontinued" mit dem Wert "True". Zu diesem Zweck können Sie entweder die Geben Sie die `<asp:ListItem>` Elemente im direkt über die deklarative Syntax, oder Verwenden der `ListItem` Auflistungs-Editor vom Designer. Abbildung 13 zeigt die `ListItem` Auflistungs-Editor, nachdem die beiden Optionen-Optionsfeld angegeben wurden.


[![Aktive und nicht mehr unterstützte Optionen RadioButtonList hinzufügen](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Abbildung 13**: aktiv hinzufügen und nicht mehr unterstützte Optionen aus, um die RadioButtonList ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image39.png))


Seit RadioButtonList in der `ItemTemplate` sollte nicht bearbeitet werden, Festlegen seiner `Enabled` Eigenschaft, um `False`bleibt die `Enabled` Eigenschaft, um `True` (Standard) für RadioButtonList in der `EditItemTemplate`. Dies wird die Optionsfelder in der Zeile nicht bearbeitet werden, als schreibgeschützt, aber ermöglicht dem Benutzer die Werte "RadioButton" für die der bearbeiteten Zeile ändern.

Wir benötigen die RadioButtonList-Steuerelementen zuweisen `SelectedValue` Eigenschaften, so, dass das entsprechende Optionsfeld ausgewählt ist, basierend auf des Produkts `Discontinued` Feld "Daten". Wie bei der DropDownLists weiter oben in diesem Lernprogramm untersucht, kann diese Syntax Databinding entweder direkt in das deklarative Markup oder über den Link DataBindings bearbeiten in die RadioButtonList Smarttags hinzugefügt werden.

Nach dem Hinzufügen von zwei RadioButtonList und konfigurieren sie die `Discontinued` deklarative TemplateFields-Markup sollte wie folgt aussehen:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Mit diesen Änderungen die `Discontinued` Spalte aus einer Liste von Kontrollkästchen umgewandelt wurde, um eine Liste von Radio Schaltfläche Paaren (siehe Abbildung 14). Wenn Sie ein Produkt zu bearbeiten, wird das entsprechende Optionsfeld ausgewählt, und das Produkt nicht mehr unterstützte Status kann die andere Optionsfeld auswählen und dann auf Update aktualisiert werden.


[![Die nicht mehr unterstützte Kontrollkästchen wurden durch Radio Schaltfläche Paare ersetzt](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Abbildung 14**: die nicht mehr unterstützte Kontrollkästchen wurden durch ersetzt Radio Schaltfläche Paare ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Da die `Discontinued` Spalte in der `Products` Datenbank sind keine `NULL` Werte, es müssen nicht kümmern erfassen `NULL` Informationen in der Schnittstelle. IF, allerdings `Discontinued` Spalte enthalten kann `NULL` Werte, die wir eine dritte hinzufügen möchte Optionsfeld in der Liste mit seiner `Value` auf eine leere Zeichenfolge festgelegt (`Value=""`), z. B. nur die mit der Kategorie und Lieferanten DropDownLists.


## <a name="summary"></a>Zusammenfassung

Während die BoundField- und CheckBoxField automatisch Rendern nur lesen, bearbeiten und Einfügen von Schnittstellen, die Möglichkeit für die Anpassung fehlen. Häufig, jedoch benötigen wir anpassen, die Bearbeitung oder Einfügen von-Schnittstelle, vielleicht der Validierungssteuerelemente (wie im vorherigen Lernprogramm Filtermenü) hinzufügen oder Anpassen der Benutzeroberfläche des Daten-Auflistung (wie in diesem Lernprogramm wurde gezeigt). Anpassen der Benutzeroberfläche mit einem TemplateField kann in den folgenden Schritten addiert werden:

1. Fügen Sie ein TemplateField oder konvertieren Sie eine vorhandene BoundField- oder CheckBoxField in ein TemplateField
2. Erweitern Sie die Schnittstelle nach Bedarf
3. Binden Sie die entsprechenden Datenfelder an den neu hinzugefügten Websteuerelementen verwenden der bidirektionalen Datenbindung

Zusätzlich zu den integrierten ASP.NET Web-Steuerelemente verwenden, können Sie auch die Vorlagen von einem TemplateField mit benutzerdefinierten, kompilierte Serversteuerelemente und Benutzersteuerelemente anpassen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
[Weiter](implementing-optimistic-concurrency-vb.md)
