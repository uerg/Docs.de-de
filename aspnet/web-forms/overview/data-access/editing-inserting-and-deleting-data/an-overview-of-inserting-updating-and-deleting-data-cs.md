---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Eine Übersicht über einfügen, aktualisieren und Löschen von Daten (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir, wie ein ObjectDataSource Insert(), Update(), zugeordnet und Delete()-Methoden, um die Methoden der BLL Klassen sowie zur Configu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: dbd111f79eda6006cb9aed59d8fd0b0342415833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Eine Übersicht über einfügen, aktualisieren und Löschen von Daten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) oder [PDF herunterladen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> In diesem Lernprogramm sehen wir ein ObjectDataSource Insert(), Update(), zuordnen und Delete()-Methoden, um die Methoden der BLL-Klassen sowie zum Konfigurieren der GridView, DetailsView und FormView-Steuerelemente zum Bereitstellen von Möglichkeiten zur Datenänderung.


## <a name="introduction"></a>Einführung

Über die letzten mehrere Lernprogramme haben wir die Vorgehensweise beim Anzeigen von Daten auf einer ASP.NET-Seite mit den Steuerelementen GridView, DetailsView und FormView untersucht. Diese Steuerelemente arbeiten einfach mit Daten, die bereitgestellt werden. In der Regel auf diese Steuerelemente Daten durch die Verwendung von ein Datenquellen-Steuerelement, z. B. das ObjectDataSource zugreifen. Wir haben gesehen, wie das ObjectDataSource als Proxy zwischen der ASP.NET-Seite und der zugrunde liegende Verfahren werden soll. Wenn eine GridView zum Anzeigen von Daten muss, ruft seine ObjectDataSource `Select()` -Methode, die wiederum eine Methode aus unserer Business Logic Layer (BLL) aufruft, der eine Methode aufgerufen wird, in der entsprechenden Data Access Layer (DAL) TableAdapter, die wiederum sendet eine `SELECT` Abfrage mit der Datenbank Northwind.

Bedenken Sie, dass, wenn wir die TableAdapters in der DAL in erstellt [unseren ersten Lernprogramm](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automatisch hinzugefügt, Methoden zum Einfügen, aktualisieren und Löschen von Daten aus der zugrunde liegenden Datenbanktabelle. Darüber hinaus in [erstellen eine Geschäftslogikschicht](../introduction/creating-a-business-logic-layer-cs.md) wir Methoden in der BLL, die aufgerufen wird, nach unten in dieser Änderung DAL Datenmethoden entworfen.

Zusätzlich zu seiner `Select()` -Methode, die ObjectDataSource verfügt auch über `Insert()`, `Update()`, und `Delete()` Methoden. Wie die `Select()` -Methode, diese drei Methoden, die an Methoden in ein zugrunde liegendes Objekt zugeordnet werden können. Wenn einfügen, aktualisieren oder Löschen von Daten konfiguriert, wird eine Benutzeroberfläche GridView, DetailsView und FormView zum Ändern der zugrunde liegenden Daten bereitstellen. Diese Benutzeroberfläche ruft die `Insert()`, `Update()`, und `Delete()` das ObjectDataSource-Methoden, die Sie dann das zugrunde liegende Objekt aufrufen zugeordnete Methoden (siehe Abbildung 1).


[![Das ObjectDataSource Insert() Update() und Delete() Methoden dienen als Proxy in der BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Abbildung 1**: das ObjectDataSource `Insert()`, `Update()`, und `Delete()` Methoden dienen als Proxy in der BLL ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


In diesem Lernprogramm sehen wir, wie das ObjectDataSource zugeordnet `Insert()`, `Update()`, und `Delete()` -Methoden den Methoden der Klassen in der BLL sowie zum Konfigurieren der GridView, DetailsView und FormView-Steuerelemente zum Bereitstellen der Änderung von Daten Funktionen.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Schritt 1: Erstellen der INSERT-, Update- und Delete-Lernprogramme Webseiten

Bevor wir beginnen, wie einfügen, aktualisieren und Löschen von Daten zu untersuchen, zunächst werfen wir einen Moment Zeit, die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und weiter mehrere diejenigen benötigen. Starten, indem Sie einen neuen Ordner namens `EditInsertDelete`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Fügen Sie die ASP.NET-Seiten für die Änderung bezogenen Daten-Lernprogramme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die Änderung bezogenen Daten-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `EditInsertDelete` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf der Seite Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Fügen Sie abschließend die Seiten hinzu, als Einträge an die `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Formatierung angepasst `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für die Bearbeitung, einfügen und Löschen von Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die Bearbeitung, einfügen und Löschen von Lernprogrammen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Abbildung 4**: die Siteübersicht enthält jetzt die Einträge für die Bearbeitung, einfügen und Löschen von Lernprogrammen


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Schritt 2: Hinzufügen und konfigurieren das ObjectDataSource-Steuerelement

Seit der GridView, DetailsView und FormView jedes unterscheiden sich in ihre Möglichkeiten zur Datenänderung und das Layout, sehen wir uns jeweils einzeln. Anstatt jedes Steuerelement, das über einen eigenen ObjectDataSource nutzen zu können, jedoch nur erstellen wir einen einzelnen ObjectDataSource, die alle drei Beispiele für freigeben können.

Öffnen der `Basics.aspx` Seite, ein ObjectDataSource aus der Toolbox in den Designer ziehen und klicken Sie auf die Datenquelle konfigurieren-Verknüpfung zwischen der Smarttag. Da die `ProductsBLL` ist die einzige BLL-Klasse, die bereitstellt, bearbeiten, einfügen und Löschen von Methoden, konfigurieren das ObjectDataSource zur Verwendung dieser Klasse.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL-Klasse](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Im nächsten Bildschirm können wir angeben, welche Methoden der der `ProductsBLL` Klasse zugeordnet sind, um das ObjectDataSource `Select()`, `Insert()`, `Update()`, und `Delete()` durch die entsprechende Registerkarte auswählen und die Methode aus der Dropdown-Liste. Abbildung 6: der sollte jetzt vertraut aussehen, ordnet das ObjectDataSource `Select()` Methode, um die `ProductsBLL` Klasse `GetProducts()` Methode. Die `Insert()`, `Update()`, und `Delete()` Methoden können konfiguriert werden, indem Sie die entsprechende Registerkarte aus der Liste am oberen Rand auswählen.


[![Haben die ObjectDataSource Zurückgeben aller Produkte](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Abbildung 6**: das ObjectDataSource Zurückgeben aller Produkte haben ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Abbildung 7, 8 und 9 die ObjectDataSource Update-, INSERT- und DELETE anzeigen Registerkarten. Konfigurieren Sie auf diesen Registerkarten, damit die `Insert()`, `Update()`, und `Delete()` Methoden aufrufen, die `ProductsBLL` Klasse `UpdateProduct`, `AddProduct`, und `DeleteProduct` Methoden, bzw.


[![Die ProductBLL Klassenmethode UpdateProduct Zuordnen der ObjectDataSource Update()-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Abbildung 7**: Zuordnen der ObjectDataSource `Update()` Methode, um die `ProductBLL` Klasse `UpdateProduct` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Die ProductBLL Klassenmethode AddProduct Zuordnen der ObjectDataSource Insert()-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Abbildung 8**: Zuordnen der ObjectDataSource `Insert()` Methode, um die `ProductBLL` Klasse hinzufügen `Product` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Die ProductBLL Klassenmethode DeleteProduct Zuordnen der ObjectDataSource Delete()-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Abbildung 9**: Zuordnen der ObjectDataSource `Delete()` Methode, um die `ProductBLL` Klasse `DeleteProduct` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Möglicherweise haben Sie bemerkt, dass die Dropdownlisten auf den Registerkarten Update-, INSERT- und DELETE diese Methoden ausgewählt bereits. Dies ist aufgrund der Verwendung von der `DataObjectMethodAttribute` , die die Methoden der ergänzt `ProducstBLL`. Die Methode DeleteProduct hat z. B. die folgende Signatur:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Die `DataObjectMethodAttribute` Attribut überprüfen, ob den Zweck der einzelnen Methoden ist für auswählen, einfügen, aktualisieren oder löschen und davon, ob es der Standardwert ist. Wenn Sie diese Attribute ausgelassen, wenn Sie BLL Klassen erstellen, müssen Sie das UPDATE manuell die Methoden aus einfügen, und Registerkarten löschen.

Nachdem sichergestellt wurde, dass die entsprechenden `ProductsBLL` Methoden werden zugeordnet, das ObjectDataSource `Insert()`, `Update()`, und `Delete()` Methoden, klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.

## <a name="examining-the-objectdatasources-markup"></a>Untersuchen das ObjectDataSource-Markup

Nach dem Konfigurieren der ObjectDataSource über den Assistenten an, wechseln Sie zur Quellansicht der generierten deklarative Markup zu untersuchen. Die `<asp:ObjectDataSource>` -Tag gibt an, das zugrunde liegende Objekt und die Methoden zum Aufrufen. Darüber hinaus stehen `DeleteParameters`, `UpdateParameters`, und `InsertParameters` zuordnen, die die Eingabeparameter für das `ProductsBLL` Klasse `AddProduct`, `UpdateProduct`, und `DeleteProduct` Methoden:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Das ObjectDataSource enthält einen Parameter für jede der Eingabeparameter für die zugeordneten Methoden, wie eine Liste der `SelectParameter` s ist vorhanden, wenn eine select-Methode aufrufen, die einen Eingabeparameter erwartet das ObjectDataSource konfiguriert ist (z. B. `GetProductsByCategoryID(categoryID)`). Eingehendem in Kürze, Werte für diese `DeleteParameters`, `UpdateParameters`, und `InsertParameters` werden automatisch durch die GridView, DetailsView und FormView festgelegt, vor dem Aufrufen der ObjectDataSource `Insert()`, `Update()`, oder `Delete()` Methode. Diese Werte können auch bei Bedarf programmgesteuert festgelegt werden, wie in einem späteren Lernprogramm erläutert.

Der mithilfe des Assistenten zum Konfigurieren von ObjectDataSource Nebeneffekt ist, Visual Studio legt die [OldValuesParameterFormatString Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) auf `original_{0}`. Dieser Eigenschaftswert wird verwendet, um die ursprünglichen Werte eines bearbeiteten Daten enthalten und ist in zwei Szenarien hilfreich:

- Wenn bei der Bearbeitung eines Datensatzes Benutzer den Primärschlüsselwert ändern können. In diesem Fall müssen dem neuen Primärschlüsselwert und den ursprünglichen primären Schlüsselwert angegeben werden, damit der Datensatz mit dem ursprünglichen primären Schlüsselwert gefunden werden und deren Wert entsprechend aktualisiert werden kann.
- Wenn die vollständigen Parallelität verwenden. Optimistische Parallelität eine Technik, um sicherzustellen, dass zwei ist gleichzeitige Benutzern nicht die jeweiligen Änderungen überschreiben und ist das Thema für einen späteren Lernprogramm.

Die `OldValuesParameterFormatString` Eigenschaft gibt den Namen der Eingabeparameter in das zugrunde liegende Objekt Update und Delete-Methoden für die ursprünglichen Werte. Erläutert diese Eigenschaft und ihren Zweck ausführlicher beim untersucht, vollständigen Parallelität. Ich schalten Sie ihn sich jetzt jedoch daran, dass unsere BLL Methoden nicht, die ursprünglichen Werten erwarten und daher es wichtig ist, dass wir diese Eigenschaft entfernen. Verlassen der `OldValuesParameterFormatString` -Eigenschaft auf einen anderen als den Standardwert festgelegt (`{0}`) verursacht einen Fehler, wenn ein Daten-Websteuerelement versucht das ObjectDataSource aufzurufenden `Update()` oder `Delete()` Methoden, da das ObjectDataSource wird Um beide zu übergeben versuchen die `UpdateParameters` oder `DeleteParameters` sowie den ursprünglichen Werteparameter angegeben.

Wenn dies in diesem Zusammenhang sehr klare enthalten ist, wird keine Sorge, untersucht, diese Eigenschaft und Nützlichkeit in einem späteren Lernprogramm. Vorerst nur sichergestellt sein, entfernen diese Eigenschaftendeklaration vollständig von deklarativer Syntax oder legen Sie den Wert auf den Standardwert ({0}).

> [!NOTE]
> Wenn Sie einfach löschen, die `OldValuesParameterFormatString` Eigenschaftswert im Eigenschaftenfenster in der Entwurfsansicht die Eigenschaft in der deklarativen Syntax auch weiterhin vorhanden, jedoch auf eine leere Zeichenfolge festgelegt werden. Hierzu leider führt weiterhin das gleiche Problem weiter oben erläuterten. Daher entfernen Sie entweder die Eigenschaft vollständig vom deklarative Syntax oder über das Eigenschaftenfenster, legen Sie den Wert auf den Standardwert `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Schritt 3: Hinzufügen eines Daten-Web-Steuerelements und seiner Konfiguration für die Änderung von Daten

Nachdem das ObjectDataSource zur Seite hinzugefügt und konfiguriert wurde, können wir fügen Sie Daten Websteuerelemente zur Seite, um sowohl die Daten anzuzeigen, und bieten eine Möglichkeit für den Endbenutzer, diese zu ändern. Sehen wir uns die GridView, DetailsView und FormView getrennt, wie ihre Möglichkeiten zur Datenänderung und Konfiguration dieser Daten-Websteuerelemente unterschiedlich sind.

Wir sehen uns im weiteren Verlauf dieses Artikels sehr einfachen bearbeiten, einfügen und löschen die Unterstützung durch die GridView, DetailsView, hinzufügen und Steuerelemente FormView ist wirklich einfach eine Reihe von Kontrollkästchen aktivieren. Es gibt viele Besonderheiten und Grenzfälle in der Praxis, die solche Funktionen, die komplizierter als einfach zeigen, und klicken Sie auf bereitstellen vornehmen. Dieses Lernprogramm konzentriert sich jedoch ausschließlich auf Nachweis der vereinfachte Möglichkeiten zur Datenänderung. Zukünftige Lernprogramme untersucht, die in einer realen zweifellos auftreten, bedenken.

## <a name="deleting-data-from-the-gridview"></a>Löschen von Daten aus der GridView

Starten Sie, indem Sie eine GridView aus der Toolbox in den Designer ziehen. Binden Sie als Nächstes das ObjectDataSource an die GridView durch Auswahl aus der Dropdown-Liste in die GridView Smarttag. An diesem Punkt werden die GridView deklarative Markup:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Binden die GridView an das ObjectDataSource über das Smarttag bietet zwei Vorteile:

- BoundFields und CheckBoxFields werden für jeden der von der ObjectDataSource zurückgegebenen Felder automatisch erstellt. Darüber hinaus werden die BoundField- und des CheckBoxField Eigenschaften basierend auf Metadaten des zugrunde liegenden Felds festgelegt. Z. B. die `ProductID`, `CategoryName`, und `SupplierName` Felder werden als schreibgeschützt markiert die `ProductsDataTable` und aus diesem Grund darf nicht aktualisiert werden beim Bearbeiten. Um diese, diese BoundFields Rechnung zu tragen [ReadOnly-Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) festgelegt `true`.
- Die [DataKeyNames Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) Primärschlüsselfelder des zugrunde liegenden Objekts zugewiesen ist. Dies ist wichtig, wenn die GridView zum Bearbeiten oder Löschen von Daten verwenden, wie diese Eigenschaft gibt an, das Feld (oder einen Satz von Feldern), eindeutig identifiziert jeden Datensatz. Weitere Informationen zu den `DataKeyNames` -Eigenschaft, verweisen zurück auf die [Master/Detail mit auswählbaren Master GridView mit einer Details-DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Lernprogramm.

Auf diese Weise während auf das ObjectDataSource über das Fenster "Eigenschaften" oder die deklarative Syntax die GridView gebunden werden kann, müssen Sie die entsprechenden BoundField manuell hinzufügen und `DataKeyNames` Markup.

GridView-Steuerelements bietet integrierte Unterstützung für auf Zeilenebene zu bearbeiten und löschen. Konfigurieren einer GridView zur Unterstützung von löschen Fügt eine Spalte von Schaltflächen zum Löschen. Klickt der Benutzer die Schaltfläche "löschen" für eine bestimmte Zeile, ein Postback erfolgt und die GridView führt die folgenden Schritte aus:

1. Das ObjectDataSource `DeleteParameters` Werte zugewiesen werden
2. Das ObjectDataSource `Delete()` Methode wird aufgerufen, den angegebenen Datensatz löschen
3. GridView bindet sich selbst auf die ObjectDataSource durch Aufrufen seiner `Select()` Methode

Die Werte für die `DeleteParameters` sind die Werte der die `DataKeyNames` Felder für die Zeile, deren Schaltfläche "löschen" geklickt wurde. Daher ist es wichtig, die einer GridView `DataKeyNames` Eigenschaft ordnungsgemäß festgelegt werden. Wenn sie nicht vorhanden ist, ist die `DeleteParameters` zugewiesen wird eine `null` Wert in Schritt 1, der nicht wiederum in einem verursacht gelöschten Datensätzen, die in Schritt2.

> [!NOTE]
> Die `DataKeys` Auflistung in die GridView-s-Steuerelementzustand gespeichert ist, d. h. die `DataKeys` Werte werden über Postbacks gespeichert werden, selbst wenn der Ansichtszustand für GridView s deaktiviert wurde. Allerdings ist es sehr wichtig, dass der Ansichtszustand für GridViews, die unterstützt werden, bearbeiten oder löschen (das Standardverhalten) aktiviert bleibt. Wenn Sie festlegen, dass die GridView s `EnableViewState` Eigenschaft `false`, bearbeiten und Löschen von Verhalten funktioniert gut für einen einzelnen Benutzer, aber wenn gleichzeitige Benutzer löschen von Daten vorhanden sind, besteht die Möglichkeit, dass diese gleichzeitigen Benutzern versehentlich möglicherweise Löschen oder bearbeiten, dass sie-t aufgezeichnet werden soll. Finden Sie unter meinem Blogeintrag [Warnung: Parallelität Problem mit ASP.NET 2.0 GridViews/DetailsView/FormViews, Bearbeitung unterstützen und/oder löschen und, deren Ansichtszustand ist deaktiviert,](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), Weitere Informationen.


Diese gleiche Warnung gilt auch für DetailsViews und FormViews.

Zum Löschen von Funktionen für eine GridView hinzuzufügen, wechseln Sie zu dessen Smarttag und aktivieren Sie das Kontrollkästchen Löschen aktivieren.


![Überprüfen Sie das Löschen von Kontrollkästchen aktivieren](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Abbildung 10**: Überprüfen Sie das Löschen von Kontrollkästchen aktivieren


Aktivieren des Kontrollkästchens löschen aktivieren aus dem Smarttag Fügt eine CommandField an die GridView. Die CommandField rendert eine Spalte in der GridView mit Schaltflächen für eine oder mehrere der folgenden Aufgaben ausführen: Auswählen eines Datensatzes, einen Datensatz bearbeiten und Löschen eines Datensatzes. Zuvor wurde erläutert, die CommandField in Aktion auswählen von Datensätzen in der [Master/Detail mit auswählbaren Master GridView mit einer Details-DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Lernprogramm.

Die CommandField enthält eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen in der CommandField angezeigt werden. Durch Aktivieren des Kontrollkästchens löschen aktivieren einer CommandField, deren `ShowDeleteButton` Eigenschaft `true` die GridView Columns-Auflistung hinzugefügt wurde.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

An diesem Punkt haben wir glauben es oder nicht mit dem Hinzufügen von Löschen von Unterstützung an die GridView! Wie Abbildung 11 zeigt, ist beim Zugriff auf diese Seite über einen Browser eine Spalte mit Schaltflächen zum Löschen vorhanden.


[![Die CommandField Fügt eine Spalte mit den Schaltflächen löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Abbildung 11**: die CommandField Fügt eine Spalte von löschen Schaltflächen ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Wenn Sie dieses Lernprogramm von a bis z auf Ihre eigenen, erstellen wurden haben beim Testen von dieser Seite klicken wird die Schaltfläche "löschen" eine Ausnahme ausgelöst. Um zu erfahren, warum diese Ausnahmen ausgelöst wurden und wie Sie das Problem beheben, lesen Sie weiter.

> [!NOTE]
> Wenn Sie zusammen mit der zugehörigen dieses Lernprogramms Download befolgen, wurden diese Probleme bereits berücksichtigt. Allerdings empfehlen wir Ihnen, durch die Identifizierung von Problemen, die auftreten können, und geeignete Abhilfemaßnahmen unten aufgeführten Informationen zu lesen.


Wenn beim Versuch, ein Produkt zu löschen, eine Ausnahme angezeigt, dessen Meldung ähnelt "*ObjectDataSource 'ObjectDataSource1' eine nicht generische Methode"DeleteProduct", die über Parameter verfügt nicht gefunden:" ProductID "ursprünglichen\_ "ProductID"*, "haben Sie wahrscheinlich vergessen, entfernen Sie die `OldValuesParameterFormatString` Eigenschaft aus der ObjectDataSource. Mit der `OldValuesParameterFormatString` Eigenschaft angegeben wird, das ObjectDataSource versucht, für die Übergabe in beiden `productID` und `original_ProductID` Eingabeparametern der `DeleteProduct` Methode. `DeleteProduct`, akzeptiert jedoch nur einen einzelnen Eingabeparameter, daher die Ausnahme. Entfernen der `OldValuesParameterFormatString` Eigenschaft (oder bei der Einstellung `{0}`) weist die ObjectDataSource nicht versucht, den ursprünglichen Eingabeparameter übergeben.


[![Stellen Sie sicher, dass die Eigenschaft OldValuesParameterFormatString Out gelöscht wurde](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Abbildung 12**: sicherstellen, dass die `OldValuesParameterFormatString` Eigenschaft hat wurde deaktiviert, ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Auch wenn Sie entfernt haben die `OldValuesParameterFormatString` Eigenschaft weiterhin erhalten Sie eine Ausnahme beim Versuch, ein Produkt mit der Nachricht zu löschen: "*die DELETE-Anweisung steht in Konflikt mit der REFERENCE-Einschränkung ' FK\_Reihenfolge\_Details \_Produkte*. " Die Northwind-Datenbank enthält eine foreign Key-Einschränkung zwischen den `Order Details` und `Products` Tabelle, was bedeutet, dass ein Produkt kann nicht aus dem System gelöscht werden, treten ein oder mehrere Datensätze für sie in der `Order Details` Tabelle. Da jedes Produkt in der Northwind-Datenbank mindestens ein Datensatz `Order Details`, alle Produkte aus, bis wir das Produkt zugeordneten Details bestellungsdatensätze zuerst löschen kann nicht gelöscht.


[![Eine Foreign Key-Einschränkung verhindert das Löschen von Produkten](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Abbildung 13**: eine Foreign Key-Einschränkung verhindert das Löschen von Produkten ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Für unser Tutorial aus, lassen Sie uns einfach Löschen aller Datensätze aus der `Order Details` Tabelle. In einer realen Anwendung würde wir entweder benötigen:

- Haben Sie einen anderen Bildschirm, um Detailbestellinformationen verwalten
- Erweitern der `DeleteProduct` Methode, um die Logik zum Löschen von Bestelldetails für das angegebene Produkt einschließen
- Ändern Sie die SQL-Abfrage von TableAdapter verwendet, um das Löschen von Bestelldetails für das angegebene Produkt einschließen

Nehmen wir einfach Löschen aller Datensätze aus der `Order Details` Tabelle, foreign Key-Einschränkung zu umgehen. Navigieren Sie zu dem Server-Explorer in Visual Studio, mit der rechten Maustaste auf die `NORTHWND.MDF` Knoten, und wählen Sie die neue Abfrage. Klicken Sie dann in das Abfragefenster ein, führen Sie die folgende SQL-Anweisung an: `DELETE FROM [Order Details]`


[![Löschen Sie alle Datensätze aus der Order Details-Tabelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Abbildung 14**: Löschen Sie alle Datensätze aus der `Order Details` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Nach dem Entfernen der `Order Details` Tabelle durch Klicken auf die Schaltfläche "löschen" löscht das Produkt ohne Fehler. Wenn das Produkt durch Klicken auf die Schaltfläche "löschen" nicht gelöscht werden, zu überprüfen, um sicherzustellen, dass der GridView `DataKeyNames` -Eigenschaft auf das Feld für den Primärschlüssel festgelegt ist (`ProductID`).

> [!NOTE]
> Beim Klicken auf die Schaltfläche "löschen" wird ein Postback erfolgt, und der Datensatz gelöscht. Dies kann gefährlich sein, da einfach versehentlich klicken Sie auf die Schaltfläche "löschen" den falschen Zeile auf. In einem späteren Lernprogramm sehen wir, wie eine Bestätigung für die clientseitige hinzufügen, wenn Sie einen Datensatz löschen.


## <a name="editing-data-with-the-gridview"></a>Bearbeiten von Daten mit GridView

Zusammen mit gelöscht wird, stellt des GridView-Steuerelements auch integrierte Unterstützung für das Bearbeiten auf Zeilenebene bereit. Konfigurieren einer GridView zur Unterstützung der Bearbeitung Fügt eine Spalte des bearbeiten-Schaltflächen. Wenn Sie hinsichtlich der Endbenutzer auf eine Zeile Bearbeiten Schaltfläche Ursachen Zeile bearbeitet werden, werden die Zellen in Textfelder ein, die vorhandenen Werte enthält, und Ersetzen Sie dabei die Schaltfläche "Bearbeiten", mit dem Update und die Schaltflächen "Abbrechen". Nachdem Sie die gewünschten Änderungen vornehmen, können die Endbenutzer die Schaltfläche "Aktualisieren", um die Änderungen zu übernehmen oder die Schaltfläche "Abbrechen", um diese zu verwerfen klicken. In beiden Fällen gibt nach dem Klicken auf Update- oder "Abbrechen" GridView Datenbankzustands vorab bearbeiten.

Unsere hinsichtlich der als der Entwickler der Seite klickt der Benutzer die Schaltfläche "Bearbeiten" für eine bestimmte Zeile, ein Postback erfolgt und die GridView führt die folgenden Schritte aus:

1. Der GridView `EditItemIndex` Eigenschaft zugewiesen ist, auf den Index der Zeile, deren Schaltfläche "Bearbeiten" geklickt wurde,
2. GridView bindet sich selbst auf die ObjectDataSource durch Aufrufen seiner `Select()` Methode
3. Der Index der Zeile entspricht der `EditItemIndex` wird gerendert, in "Bearbeitungsmodus". In diesem Modus wird die Schaltfläche "Bearbeiten" durch die Schaltflächen "Abbrechen" und "Update" und BoundFields ersetzt, deren `ReadOnly` Eigenschaften sind "false" (Standard) werden als Textfeld Web, dessen Steuerelemente gerendert `Text` Eigenschaften die Datenfelder Werte zugewiesen sind.

Zu diesem Zeitpunkt wird das Markup an den Browser, ermöglicht der Endbenutzer keine Änderungen vornehmen, um Daten aus der Zeile zurückgegeben. Klickt der Benutzer auf die Schaltfläche "Aktualisieren", ein Postback erfolgt und die GridView führt die folgenden Schritte aus:

1. Das ObjectDataSource `UpdateParameters` Werte der in Bearbeitung der GridView-Schnittstelle vom Endbenutzer eingegebenen Werte zugewiesen werden
2. Das ObjectDataSource `Update()` Methode wird aufgerufen, den angegebenen Datensatz aktualisieren
3. GridView bindet sich selbst auf die ObjectDataSource durch Aufrufen seiner `Select()` Methode

Die Primärschlüsselwerte zugewiesen der `UpdateParameters` in Schritt 1 stammen aus den Werten im angegebenen die `DataKeyNames` -Eigenschaft, während die Nichtprimär-Schlüsselwerte aus dem Text in das Textfeld Websteuerelementen der bearbeiteten Zeile stammen. Wie bei löschen, es wichtiger ist, die einer GridView `DataKeyNames` Eigenschaft ordnungsgemäß festgelegt werden. Wenn sie nicht vorhanden ist, ist die `UpdateParameters` Primärschlüsselwert zugewiesen ein `null` Wert in Schritt 1, der nicht wiederum in einem verursacht aktualisiert Datensätze in Schritt2.

Bearbeiten-Funktionalität kann durch Überprüfen einfach das Kontrollkästchen Bearbeiten aktivieren, in die GridView Smarttag aktiviert werden.


![Überprüfen Sie das Kontrollkästchen Bearbeiten aktivieren](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Abbildung 15**: Überprüfen Sie das Kontrollkästchen Bearbeiten aktivieren


Überprüfen das Kontrollkästchen Bearbeiten aktivieren wird eine CommandField hinzufügen (sofern erforderlich), und legen seine `ShowEditButton` Eigenschaft `true`. Wie bereits zuvor gesehen, wird die CommandField enthält eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen in der CommandField angezeigt werden. Überprüfen das Kontrollkästchen Bearbeiten aktivieren fügt die `ShowEditButton` Eigenschaft, um die vorhandenen CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Das ist alles vorhanden ist, zum Hinzufügen der Unterstützung für das rudimentäre bearbeiten. Wie Figure16 gezeigt, die Bearbeitungsoberfläche stattdessen einfach gehalten wird jede BoundField, deren `ReadOnly` -Eigenschaftensatz auf `false` (Standard) wird als ein Textfeld gerendert. Dies beinhaltet Felder wie `CategoryID` und `SupplierID`, die Schlüssel auf andere Tabellen sind.


[![Klicken Sie auf Chai s Schaltfläche "Bearbeiten" zeigt die Zeile im Bearbeitungsmodus](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Abbildung 16**: durch Klicken auf Chai s Schaltfläche "Bearbeiten" zeigt die Zeile im Bearbeitungsmodus ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Zusätzlich zur Aufforderung der Benutzer für eine direkte Bearbeitung Fremdschlüsselwerte, fehlt der Bearbeitung Schnittstelle auf folgende Weise:

- Wenn der Benutzer gibt eine `CategoryID` oder `SupplierID` , die in der Datenbank nicht vorhanden die `UPDATE` wird verletzt eine foreign Key-Einschränkung, dass eine Ausnahme ausgelöst werden soll.
- Die Bearbeitungsoberfläche ist keine Validierung nicht enthalten. Wenn Sie nicht, dass einen erforderlichen Wert angeben (z. B. `ProductName`), oder geben Sie einen Zeichenfolgenwert ein, in dem ein numerischer Wert erwartet wird, (wie das Eingeben von "Zu viele!" in der `UnitPrice` Textfeld), wird eine Ausnahme ausgelöst werden. Eine zukünftige Lernprogramm untersucht zum Hinzufügen von Steuerelementen für die Validierung zu der Benutzeroberfläche zur Bearbeitung.
- Derzeit *alle* Produkt-Felder, die nicht schreibgeschützt sind in der GridView enthalten sein müssen. Angenommen, wäre es ein Feld aus der GridView entfernen, `UnitPrice`, beim Aktualisieren der Daten die GridView nicht festgelegt werden die `UnitPrice` `UpdateParameters` -Wert, der des Datenbankdatensatz geändert würde `UnitPrice` auf eine `NULL` Wert. Auf ähnliche Weise, wenn ein erforderliches Feld, z. B. `ProductName`, entfernt aus der GridView-das Update schlägt fehl, mit dem gleichen "*Spaltenname 'ProductName' lässt keine NULL-Werte*" Ausnahme, die oben genannten.
- Die Bearbeitung Schnittstelle Formatierung bewirkt, dass viele wünschenswert sein. Die `UnitPrice` wird mit vier Dezimalstellen angezeigt. Im Idealfall der `CategoryID` und `SupplierID` Werten DropDownLists, die die Kategorien und Lieferanten im System auflisten.

Hierbei handelt es sich um alle Mängel, die wir mit live, jetzt aber wird in zukünftigen Lernprogrammen adressiert werden.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Einfügen, bearbeiten und Löschen von Daten mit DetailsView

Wie wir Steuerelement zeigt einen Datensatz zu einem Zeitpunkt, und wie die GridView kann zum Bearbeiten und Löschen des aktuell angezeigten Datensatzes in früheren Lernprogrammen, DetailsView gesehen haben. Sowohl der Endbenutzer-Erfahrung mit bearbeiten und Löschen von Elementen aus einem DetailsView und der Workflow von der Seite ASP.NET ist identisch mit der GridView. Die GridView, DetailsView unterscheidet besteht darin, dass es auch integrierten Einfügen von Unterstützung.

Zur Veranschaulichung der Möglichkeiten zur Datenänderung der GridView zu starten, indem Sie eine DetailsView zum Hinzufügen der `Basics.aspx` oberhalb der vorhandenen GridView und an den vorhandenen ObjectDataSource über die DetailsView Smarttag gebunden wird. Als Nächstes deaktivieren, DetailsView des `Height` und `Width` Eigenschaften, und aktivieren Sie die Option Paging aktivieren aus dem Smarttag. Zum Aktivieren der Bearbeitung Kontrollkästchen einfügen und Löschen von Unterstützung, einfach die Bearbeiten aktivieren, aktivieren Sie einfügen und löschen aktivieren im Smarttag.


![Konfigurieren von DetailsView an den Support, bearbeiten, einfügen und löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Abbildung 17**: Konfigurieren von DetailsView an den Support, bearbeiten, einfügen und löschen


Als hinzugefügt mit GridView hinzufügen, bearbeiten, einfügen oder löschen die Unterstützung einer CommandField, DetailsView wie im folgenden deklarative Syntax gezeigt:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Beachten Sie, dass für DetailsView der CommandField am Ende der Auflistung der Spalten in der Standardeinstellung angezeigt wird. Da der DetailsView Felder als Zeilen gerendert werden die CommandField angezeigt wird, als eine Zeile mit Insert, bearbeiten und Löschen von Schaltflächen am unteren Rand der DetailsView.


[![Konfigurieren von DetailsView an den Support, bearbeiten, einfügen und löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Abbildung 18**: Konfigurieren von DetailsView zu bearbeiten, einfügen und Löschen von Unterstützung ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Durch Klicken auf die Schaltfläche "löschen" die gleiche Sequenz von Ereignissen wie bei der GridView beginnt: a Postbacks gefolgt von dessen ObjectDataSource Auffüllen DetailsView `DeleteParameters` basierend auf den `DataKeyNames` Werte und durch Aufrufen seiner ObjectDataSource abgeschlossen `Delete()` -Methode, die das Produkt tatsächlich aus der Datenbank entfernt. Bearbeiten in DetailsView funktioniert auch in einer Weise, die identisch mit der GridView.

Zum Einfügen, erhält der Endbenutzer mit einem neu-Schaltfläche geklickt, DetailsView in "Einfügemodus" rendert Mit "Einfügemodus" die Schaltfläche "Neu" durch die Schaltflächen "und" Abbrechen "Insert", und nur diese BoundFields ersetzt wird, dessen `InsertVisible` -Eigenschaftensatz auf `true` (Standardeinstellung) werden angezeigt. Identifiziert als automatische Inkrementierung-Felder, wie z. B. Datenfelder `ProductID`, haben ihre [InsertVisible Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) festgelegt `false` beim Binden von DetailsView an die Datenquelle mithilfe des Smarttags.

Beim Binden einer Datenquelle an einen DetailsView über das Smarttag, legt Visual Studio die `InsertVisible` Eigenschaft `false` nur für automatisch inkrementierte Felder. Schreibgeschützte Felder, wie z. B. `CategoryName` und `SupplierName`, wird in der Benutzeroberfläche "Einfügemodus" angezeigt werden, es sei denn, ihre `InsertVisible` -Eigenschaft explizit festgelegt wird, um `false`. Können Sie diese beiden Felder festlegen `InsertVisible` Eigenschaften `false`, entweder über die DetailsView deklarative Syntax oder über die Felder bearbeiten im Smarttag verknüpfen. Abbildung 19 zeigt die Einstellung der `InsertVisible` Eigenschaften `false` durch Klicken auf die Felder bearbeiten zu verknüpfen.


[![Northwind Traders bietet jetzt Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Abbildung 19**: Northwind Traders jetzt bietet Acme Tee ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Nach dem Festlegen der `InsertVisible` Eigenschaften, die Ansicht der `Basics.aspx` Seite in einem Browser, und klicken Sie auf die Schaltfläche "Neu". Abbildung 20 zeigt DetailsView beim Hinzufügen einer neuen Getränke Acme Tee, um unsere Produktlinie.


[![Northwind Traders bietet jetzt Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Abbildung 20**: Northwind Traders jetzt bietet Acme Tee ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Nach dem Eingeben der Details für Acme Tee, und klicken Sie auf die Schaltfläche zum Einfügen, erfolgt ein Postback und der neue Datensatz hinzugefügt wird die `Products` Datenbanktabelle. Seit dieser DetailsView der Produkte in der Reihenfolge aufgeführt mit denen sie in der Datenbanktabelle vorhanden sind sind, müssen wir Produkt bis zum letzten Seite, um das neue Produkt finden Sie unter.


[![Details für Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Abbildung 21**: Details für Acme Tee ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> Der DetailsView [CurrentMode Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) gibt die Schnittstelle wird angezeigt und kann einen der folgenden Werte: `Edit`, `Insert`, oder `ReadOnly`. Die [DefaultMode-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) gibt der Modus, DetailsView gibt an, nach einer Änderung oder zum Einfügen von, wurde abgeschlossen und eignet sich zum Anzeigen einer DetailsView, die dauerhaft in Bearbeitung ist, oder im Einfügemodus.


Der Zeitpunkt und die auf Einfügen und Bearbeitungsfunktionen von DetailsView beeinträchtigt werden, über die gleichen Einschränkungen wie die GridView: der Benutzer muss die vorhandene eingeben `CategoryID` und `SupplierID` Werte über ein Textfeld, besitzt der Schnittstelle eine Validierungslogik; alle Produkt-Felder, die keine zulassen `NULL` Werte oder keinen Standardwert auf der Datenbankebene angegebene Wert in der einfügen-Schnittstelle usw. enthalten sein muss.

Die Techniken untersuchen wir zu erweitern, und erweitern die GridView Bearbeitungsoberfläche in zukünftigen Artikeln, DetailsView-Steuerelements bearbeiten und Einfügen von Schnittstellen auch angewendet werden können.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Die FormView verwenden für eine flexiblere Änderung Benutzeroberfläche

Die FormView bietet integrierten Unterstützung für das Einfügen, bearbeiten und Löschen von Daten, aber da Vorlagen anstelle von Feldern verwendet wird, besteht keine Möglichkeit, die BoundFields oder die CommandField von GridView und DetailsView-Steuerelementen verwendet werden, um die Daten hinzufügen Änderung-Schnittstelle. Diese Schnittstelle, die die Web-Steuerelemente für das Sammeln von Benutzer Eingabe, wenn Sie ein neues Element hinzufügen oder Bearbeiten einer vorhandenen zusammen mit dem neuen bearbeiten, löschen, einfügen, aktualisieren und Abbrechen (Schaltflächen) muss stattdessen manuell auf den entsprechenden Vorlagen hinzugefügt werden. Glücklicherweise, erstellt Visual Studio automatisch die erforderliche Schnittstelle, beim Binden von FormView an eine Datenquelle mithilfe der Dropdown-Liste in seinem Smarttag.

Um diese Verfahren zu veranschaulichen, starten Sie durch hinzufügen einen FormView der `Basics.aspx` Seite und aus der FormView-Smarttag, binden Sie es an das ObjectDataSource bereits erstellt. Dadurch wird ein `EditItemTemplate`, `InsertItemTemplate`, und `ItemTemplate` für FormView mit TextBox-Websteuerelemente zum Sammeln von Eingabe- und Websteuerelemente Schaltfläche des Benutzers für das Symbol neu, bearbeiten, löschen, einfügen, aktualisieren und Abbrechen (Schaltflächen). Darüber hinaus die FormView `DataKeyNames` -Eigenschaft auf das Feld für den Primärschlüssel festgelegt ist (`ProductID`) des Objekts durch das ObjectDataSource zurückgegeben. Aktivieren Sie schließlich in der FormView-Smarttag Option Paging aktivieren.

Nachstehend sehen Sie das deklarative Markup für die FormView `ItemTemplate` nach FormView an das ObjectDataSource gebunden wurde. Standardmäßig jedes Product-Feld nicht boolescher Wert gebunden ist, um die `Text` Eigenschaft eines Steuerelements Bezeichnung Web während jedes Feld boolescher Wert (`Discontinued`) gebunden ist die `Checked` Eigenschaft eines deaktivierten CheckBox Web-Steuerelements. In der Reihenfolge für die Schaltflächen New, Edit und Delete bestimmte FormView Verhalten beim Auslösen, es ist unbedingt erforderlich, die ihre `CommandName` Werte festgelegt werden, um `New`, `Edit`, und `Delete`zugeordnet.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Abbildung 22 zeigt die FormView `ItemTemplate` über einen Browser angezeigt. Jedes Feld "Product" wird mit den New, Edit und Delete-Schaltflächen im unteren Bereich aufgeführt.


[![Weisen FormView ItemTemplate Listet jedes Feld "Product" sowie neue, bearbeiten und Löschen von Schaltflächen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Abbildung 22**: die weisen FormView `ItemTemplate` Listet jedes Produkt Feld zusammen mit den Schaltflächen löschen, bearbeiten und neu ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Wie Sie mit dem GridView und DetailsView, klicken Sie auf die Schaltfläche "löschen" oder eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` Eigenschaft auf Löschen, wird ein Postback festgelegt ist, füllt das ObjectDataSource `DeleteParameters` basierend auf der FormView `DataKeyNames`aus und ruft das ObjectDataSource `Delete()` Methode.

Beim Klicken auf die Schaltfläche "Bearbeiten" ein Postback erfolgt und die Daten erneut gebunden wird, um die `EditItemTemplate`, die zum Rendern der Bearbeitungsoberfläche verantwortlich ist. Diese Schnittstelle enthält die Web-Steuerelemente für die Bearbeitung von Daten zusammen mit den Schaltflächen Aktualisieren und "Abbrechen". Die Standardeinstellung `EditItemTemplate` generiert, die von Visual Studio enthält eine Bezeichnung für alle Felder automatisch inkrementierten (`ProductID`), ein Textfeld für jedes Wertfeld nicht boolescher sowie ein Kontrollkästchen für jedes Feld boolescher Wert. Dieses Verhalten ist vergleichbar mit der automatisch generierten BoundFields die GridView und DetailsView-Steuerelemente.

> [!NOTE]
> Eine kleine Probleme mit der FormView automatische Generierung von der `EditItemTemplate` ist, dass TextBox Web-Steuerelemente für diese Felder gerendert, die z. B. schreibgeschützt sind `CategoryName` und `SupplierName`. Wir sehen, wie Sie für dieses Konto in Kürze.


Der TextBox-Steuerelemente der `EditItemTemplate` haben ihre `Text` Eigenschaft gebunden wird, auf den Wert von der entsprechenden Feld using *bidirektionale Datenbindung*. Die bidirektionale Datenbindung, gekennzeichnet durch `<%# Bind("dataField") %>`, führt Databinding sowohl beim Binden von Daten an die Vorlage und beim Auffüllen der ObjectDataSource-Parameter zum Einfügen oder Bearbeiten von Datensätzen. D. h., wenn der Benutzer die Schaltfläche "Bearbeiten" aus klickt der `ItemTemplate`die `Bind()` Methode gibt den Wert des angegebenen Felds zurück. Nachdem der Benutzer mit der ihre Änderungen und Updates klickt, veröffentlicht die Werte zurück, die entsprechen den angegeben wird, mithilfe von Datenfeldern `Bind()` gelten für das ObjectDataSource `UpdateParameters`. Alternativ unidirektionale Databinding gekennzeichnet durch `<%# Eval("dataField") %>`nur Datenfeldwerte beim Binden von Daten an die Vorlage abruft und führt *nicht* Postback der vom Benutzer eingegebenen Werten zum Parameter von der Datenquelle zurück.

Das folgende deklarative Markup zeigt die FormView `EditItemTemplate`. Beachten Sie, dass die `Bind()` Methode wird in der Databinding-Syntax verwendet und die Websteuerelemente Update und "Abbrechen" Schaltfläche denen ihre `CommandName` Eigenschaften entsprechend festgelegt.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Unsere `EditItemTemplate`, zurzeit zeigen, führt dazu, dass eine Ausnahme ausgelöst werden, wenn Sie versuchen, ihn zu verwenden. Das Problem besteht darin, die die `CategoryName` und `SupplierName` Felder werden gerendert, da Textfeld Web gesteuert wird, in der `EditItemTemplate`. Wir müssen entweder diesen Textfelder in Bezeichnungen ändern oder vollständig zu entfernen. Nehmen wir einfach löschen sie vollständig aus der `EditItemTemplate`.

Abbildung 23 zeigt FormView in einem Browser, nachdem die Schaltfläche "Bearbeiten" für Chai auf den geklickt wurde. Beachten Sie, dass die `SupplierName` und `CategoryName` Felder der `ItemTemplate` nicht mehr vorhanden sind, wie wir gerade entfernt sie aus der `EditItemTemplate`. Die gleiche Sequenz von Schritten wie GridView und DetailsView durchläuft die FormView, wenn auf die Schaltfläche "Aktualisieren" geklickt wird.


[![Standardmäßig zeigt ItemTemplate jedes bearbeitbaren Produktfeld als Textfeld oder das Kontrollkästchen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Abbildung 23**: standardmäßig die `EditItemTemplate` zeigt jede bearbeitbare Feld "Product" als Textfeld oder Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Wenn die Schaltfläche "Einfügen" der FormView geklickt wird `ItemTemplate` ein Postback erfolgt. Allerdings ist keine Daten an die FormView gebunden, da ein neuer Datensatz hinzugefügt wird. Die `InsertItemTemplate` -Schnittstelle enthält die Web-Steuerelemente zum Hinzufügen eines neuen Datensatzes zusammen mit den Schaltflächen einfügen und "Abbrechen". Die Standardeinstellung `InsertItemTemplate` generiert, die von Visual Studio enthält ein Textfeld für jedes Wertfeld nicht boolescher und ein Kontrollkästchen für jedes Feld boolescher Wert, die automatisch generierte ähnelt `EditItemTemplate`der Schnittstelle. TextBox-Steuerelemente haben ihre `Text` Eigenschaft gebunden wird, auf den Wert ihrer entsprechenden Feld "Daten" Verwenden der bidirektionalen Datenbindung.

Das folgende deklarative Markup zeigt die FormView `InsertItemTemplate`. Beachten Sie, dass die `Bind()` Methode wird in der Databinding-Syntax verwendet und die Websteuerelemente einfügen und Abbrechen Schaltfläche denen ihre `CommandName` Eigenschaften entsprechend festgelegt.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Es wird eine Besonderheit mit der FormView automatische Generierung von der `InsertItemTemplate`. Insbesondere werden die Textfeld-Websteuerelemente auch für die Felder, die z. B. schreibgeschützt sind erstellt `CategoryName` und `SupplierName`. Wie Sie mit der `EditItemTemplate`, müssen wir diese Textfelder aus Entfernen der `InsertItemTemplate`.

Abbildung 24 zeigt FormView in einem Browser, wenn ein neues Produkt, Acme Kaffee hinzufügen. Beachten Sie, dass die `SupplierName` und `CategoryName` Felder der `ItemTemplate` nicht mehr vorhanden sind, wie wir gerade entfernt. Wenn die Schaltfläche "Einfügen" der FormView-fortgesetzt, über die gleiche Sequenz von Schritten wie DetailsView-Steuerelement geklickt wird, Hinzufügen von ein neuer Datensatz der `Products` Tabelle. Abbildung 25 zeigt Acme Kaffee Produktdetails in der FormView, nachdem es eingefügt wurde.


[![Die InsertItemTemplate bestimmt die FormView einfügen-Schnittstelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Abbildung 24**: die `InsertItemTemplate` bestimmt die FormView einfügen-Schnittstelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Die Details für das neue Produkt, Acme Coffee, werden in der FormView angezeigt.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Abbildung 25**: Details für das neue Produkt, Acme Coffee, werden in der FormView angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Durch Herausnehmen von der schreibgeschützten, ermöglicht das Bearbeiten und Einfügen von Schnittstellen in drei unterschiedliche Vorlagen, die FormView für eine feinere Kontrolle über diese Schnittstellen als die, DetailsView und GridView.

> [!NOTE]
> Wie DetailsView, FormView des `CurrentMode` Eigenschaft gibt die Schnittstelle, die angezeigt wird und die zugehörige `DefaultMode` Eigenschaft gibt den Modus der FormView gibt, nach einer Änderung oder Insert wurde abgeschlossen.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm untersucht es die Grundlagen von einfügen, bearbeiten und Löschen von Daten mit GridView, DetailsView und FormView. Alle drei dieser Steuerelemente bieten einige integrierte Möglichkeiten zur Datenänderung, die verwendet werden kann, ohne eine einzige Codezeile schreiben, auf der ASP.NET-Seite unser Dank gilt den Websteuerelementen Daten und das ObjectDataSource. Allerdings die einfache zeigen und klicken Sie auf Techniken Render relativ frail und Naïve Änderung Benutzeroberfläche. Um die Bereitstellung der Validierung programmgesteuerte Werte einfügen, ordnungsgemäß Behandeln von Ausnahmen, Anpassen der Benutzeroberfläche und usw., benötigen wir eine Bevy Techniken vertrauen, die über die nächsten mehrere Lernprogramme erläutert werden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
