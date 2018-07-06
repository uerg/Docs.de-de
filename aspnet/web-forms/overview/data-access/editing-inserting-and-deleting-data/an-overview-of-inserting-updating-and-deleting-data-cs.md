---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Eine Übersicht über einfügen, aktualisieren und Löschen von Daten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie ein "ObjectDataSource" Insert(), Update(), zuordnen und Delete()-Methoden, um die Methoden der Geschäftslogikschicht-Klassen sowie wie Sie konfigu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e1b47782bd24824707266d1ed61e24789cc7a49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369837"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Eine Übersicht über einfügen, aktualisieren und Löschen von Daten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) oder [PDF-Datei herunterladen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> In diesem Tutorial erfahren Sie, wie ein "ObjectDataSource" Insert(), Update(), zuordnen und Delete()-Methoden, um die Methoden der Geschäftslogikschicht-Klassen sowie die GridView, DetailsView oder FormView-Steuerelemente, um die Möglichkeiten zur Datenänderung bereitstellen zu konfigurieren.


## <a name="introduction"></a>Einführung

Über die letzten mehrere Tutorials haben wir Gewusst wie: Anzeigen von Daten in eine ASP.NET-Seite mithilfe von GridView, DetailsView und FormView untersucht. Diese Steuerelemente funktionieren einfach mit Daten, die bereitgestellt werden. Diese Steuerelemente zugreifen häufig, Daten durch die Verwendung einer Datenquellen-Steuerelement, z. B. dem ObjectDataSource-Steuerelement. Wir haben gesehen, wie dem ObjectDataSource-Steuerelement als Proxy zwischen der ASP.NET-Seite und den zugrunde liegenden Daten fungiert. Wenn einer GridView-Ansicht zum Anzeigen von Daten benötigt, ruft es die "ObjectDataSource" `Select()` -Methode, die wiederum eine Methode aus unseren Business Logic Layer (BLL) aufruft, das eine Methode in der entsprechenden der Datenzugriffsschicht (DAL) Ruft TableAdapter, die wiederum sendet eine `SELECT` Abfrage mit der Datenbank Northwind.

Bedenken Sie, dass beim Erstellen der TableAdapter-Steuerelemente in der Datenzugriffsschicht in [unserer ersten Tutorial](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automatisch hinzugefügt, die Methoden zum Einfügen, aktualisieren und Löschen von Daten aus der zugrunde liegenden Datenbanktabelle. Darüber hinaus in [Erstellen einer Geschäftslogikebene](../introduction/creating-a-business-logic-layer-cs.md) wir Methoden in der BLL, die aufgerufen wird, nach unten in dieser datenänderungsmethoden DAL entworfen.

Zusätzlich zu seiner `Select()` -Methode, dem ObjectDataSource-Steuerelement verfügt auch über `Insert()`, `Update()`, und `Delete()` Methoden. Wie die `Select()` -Methode, diese drei Methoden auf Methoden in einem zugrunde liegenden Objekt zugeordnet werden können. Wenn einfügen, aktualisieren oder Löschen von Daten konfiguriert haben, bieten die GridView, DetailsView oder FormView-Steuerelemente eine Benutzeroberfläche für die zugrunde liegenden Daten ändern. Diese Benutzeroberfläche ruft die `Insert()`, `Update()`, und `Delete()` Methoden dem ObjectDataSource-Steuerelement, das das zugrunde liegende Objekt rufen Sie dann die zugehörigen Methoden, die (siehe Abbildung 1).


[![Das "ObjectDataSource" Insert() Update() und Delete() Methoden dienen als Proxy, in die BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Abbildung 1**: der ObjectDataSource `Insert()`, `Update()`, und `Delete()` Methoden dienen als Proxy in der BLL ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


In diesem Tutorial erfahren Sie, wie dem ObjectDataSource-Steuerelement zuordnen `Insert()`, `Update()`, und `Delete()` -Methoden den Methoden der Klassen in die BLL sowie die GridView, DetailsView oder FormView-Steuerelemente, um die Änderung von Daten bieten konfigurieren Funktionen.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Schritt 1: Erstellen der INSERT-, Update- und Delete Lernprogramme von Webseiten

Bevor wir beginnen, wie Sie einfügen, aktualisieren und Löschen von Daten zu untersuchen, zuerst werfen wir einen Moment Zeit, die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die nächsten mehrere Explorer benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `EditInsertDelete`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

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


Wie in den anderen Ordnern `Default.aspx` in die `EditInsertDelete` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf der Seite Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Abschließend fügen Sie die Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Formatierung angepasst `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt die Einträge für das Bearbeiten, einfügen und löschen die Lernprogramme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Abbildung 4**: die Sitemap enthält nun Einträge für das Bearbeiten, einfügen und löschen die Lernprogramme


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Schritt 2: Hinzufügen und konfigurieren das ObjectDataSource-Steuerelement

Da die GridView, DetailsView und FormView jedes unterscheiden sich durch ihre Möglichkeiten zur Datenänderung und das Layout, betrachten wir jeweils einzeln. Anstatt jedes Steuerelement mittels der eigenen "ObjectDataSource" zu lassen, jedoch nur erstellen wir einen einzelnen "ObjectDataSource", die alle drei Beispiele für freigeben können.

Öffnen der `Basics.aspx` Seite, ein ObjectDataSource-Steuerelement aus der Toolbox in den Designer ziehen, und klicken Sie auf die Datenquelle konfigurieren-Link aus der Smarttag. Da die `ProductsBLL` ist die einzige BLL-Klasse, die bereitstellt, bearbeiten, einfügen und Löschen von Methoden, konfigurieren das "ObjectDataSource" zur Verwendung dieser Klasse.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL-Klasse](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Im nächsten Bildschirm können wir angeben, welche Methoden die `ProductsBLL` Klasse dem ObjectDataSource-Steuerelement zugeordnet sind `Select()`, `Insert()`, `Update()`, und `Delete()` durch Auswahl der entsprechenden Registerkarte und die Methode aus der Dropdown-Liste. Abbildung 6: die sollte jetzt vertraut aussehen, ordnet dem ObjectDataSource-Steuerelement `Select()` Methode, um die `ProductsBLL` Klasse `GetProducts()` Methode. Die `Insert()`, `Update()`, und `Delete()` Methoden können konfiguriert werden, indem Sie die entsprechende Registerkarte aus der Liste oben auswählen.


[![Müssen dem ObjectDataSource-Steuerelement zurückgegeben wird, alle Produkte](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Abbildung 6**: das "ObjectDataSource" Zurückgeben aller Produkte haben ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Abbildung 7, 8 und 9, die dem ObjectDataSource-Steuerelement UPDATE, INSERT und DELETE Anzeigen von Registerkarten. Konfigurieren Sie die folgenden Registerkarten, damit die `Insert()`, `Update()`, und `Delete()` Aufrufen von Methoden der `ProductsBLL` Klasse `UpdateProduct`, `AddProduct`, und `DeleteProduct` Methoden bzw.


[![Map-dem ObjectDataSource-Steuerelement Update()-Methode der Klasse ProductBLL UpdateProduct-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Abbildung 7**: Ordnen Sie dem ObjectDataSource-Steuerelement `Update()` Methode, um die `ProductBLL` Klasse `UpdateProduct` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Map-dem ObjectDataSource-Steuerelement Insert()-Methode der Klasse ProductBLL AddProduct-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Abbildung 8**: Ordnen Sie dem ObjectDataSource-Steuerelement `Insert()` Methode, um die `ProductBLL` -Klasse hinzufügen `Product` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Map-dem ObjectDataSource-Steuerelement Delete()-Methode der Klasse ProductBLL DeleteProduct-Methode](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Abbildung 9**: Ordnen Sie dem ObjectDataSource-Steuerelement `Delete()` Methode, um die `ProductBLL` Klasse `DeleteProduct` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Möglicherweise haben Sie bemerkt, dass der Dropdown-Listen in den Update-, INSERT- und DELETE-Registerkarten diese Methoden ausgewählt bereits. Dies ist dank der Verwendung von der `DataObjectMethodAttribute` , die die Methoden der ergänzt `ProducstBLL`. Beispielsweise weist die DeleteProduct-Methode die folgende Signatur:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Die `DataObjectMethodAttribute` Attribut gibt an den Zweck der einzelnen Methoden, ob es für das auswählen, einfügen, aktualisieren oder löschen und davon, ob sie der Standardwert ist. Wenn Sie diese Attribute ausgelassen, wenn Sie BLL Klassen erstellen, werden Sie müssen, wählen Sie die Methoden aus dem UPDATE manuell einfügen und löschen Registerkarten.

Nachdem Sie sichergestellt haben, die die entsprechende `ProductsBLL` Methoden dem ObjectDataSource-Steuerelement zugeordnet sind `Insert()`, `Update()`, und `Delete()` Methoden, klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.

## <a name="examining-the-objectdatasources-markup"></a>Untersuchen Markup mit dem ObjectDataSource-Steuerelement

Nachdem dem ObjectDataSource-Steuerelement mithilfe der Assistenten konfiguriert haben, wechseln Sie zur Quellansicht deklarative generierte Markup zu untersuchen. Die `<asp:ObjectDataSource>` -Tag gibt das zugrunde liegende Objekt und die Methoden aufrufen. Darüber hinaus stehen `DeleteParameters`, `UpdateParameters`, und `InsertParameters` , zuordnen, um die Eingabeparameter für die `ProductsBLL` Klasse `AddProduct`, `UpdateProduct`, und `DeleteProduct` Methoden:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Dem ObjectDataSource-Steuerelement enthält einen Parameter für jede der Eingabeparameter für die zugeordneten Methoden, wie eine Liste der `SelectParameter` s ist vorhanden, wenn eine select-Methode aufrufen, die einen Eingabeparameter erwartet, dass dem ObjectDataSource-Steuerelement konfiguriert ist (z. B. `GetProductsByCategoryID(categoryID)`). Wie wir in Kürze, Werte für diese sehen `DeleteParameters`, `UpdateParameters`, und `InsertParameters` werden automatisch von GridView, DetailsView und FormView-Steuerelement festgelegt, vor dem Aufrufen der "ObjectDataSource" `Insert()`, `Update()`, oder `Delete()` -Methode. Diese Werte können auch bei Bedarf programmgesteuert festgelegt werden, wie in einem späteren Lernprogramm besprochen.

Ein Nebeneffekt der Verwendung des Assistenten zum Konfigurieren von "ObjectDataSource" ist, dass es sich bei Visual Studio legt die [OldValuesParameterFormatString-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) zu `original_{0}`. Wert dieser Eigenschaft wird verwendet, um die ursprünglichen Werte der bearbeiteten Daten enthalten und wird in zwei Szenarien hilfreich:

- Wenn Sie beim Bearbeiten eines Datensatzes Benutzer den Wert des Primärschlüssels ändern können. In diesem Fall müssen sowohl auf dem neuen Wert des Primärschlüssels als auch auf dem ursprünglichen Wert des Primärschlüssels angegeben werden, damit der Datensatz mit dem ursprünglichen Wert des Primärschlüssels gefunden werden und deren Wert entsprechend aktualisiert werden kann.
- Wenn Sie vollständigen Parallelität verwenden zu können. Die optimistische Parallelität ein Verfahren, um sicherzustellen, dass zwei ist Benutzer gleichzeitig überschreiben Sie die Änderungen nicht und ist das Thema für einen zukünftigen Tutorial.

Die `OldValuesParameterFormatString` Eigenschaft gibt den Namen der Eingabeparameter in das zugrunde liegende Objekt-Update und Delete-Methoden für die ursprünglichen Werte. Wird besprochen, diese Eigenschaft und ihren Zweck ausführlicher beim untersucht, optimistischen Parallelität. Ich machen sie Sie jetzt jedoch daran, dass unsere BLL Methoden Sie nicht die ursprünglichen Werte erwarten nur, und daher es wichtig ist, dass wir diese Eigenschaft entfernen. Verlassen der `OldValuesParameterFormatString` -Eigenschaft auf einen anderen als den Standardwert festgelegt (`{0}`) verursacht einen Fehler, wenn ein Daten-Websteuerelement versucht wird, auf dem ObjectDataSource-Steuerelement aufrufen `Update()` oder `Delete()` Methoden da wird dem ObjectDataSource-Steuerelement versucht, die in beiden übergeben die `UpdateParameters` oder `DeleteParameters` sowie den ursprünglichen Wertparametern angegeben.

Sollte dies nicht allzu löschen an diesem Punkt keine Sorge, untersucht, diese Eigenschaft und dessen Dienstprogramm in einem späteren Tutorial. Jetzt, seien Sie sicher, dass diese Eigenschaftsdeklaration vollständig aus der deklarativen Syntax entfernen, oder legen Sie den Wert auf den Standardwert ({0}).

> [!NOTE]
> Wenn Sie einfach löschen, die `OldValuesParameterFormatString` Eigenschaftswert im Eigenschaftenfenster in der Entwurfsansicht, die Eigenschaft in der deklarativen Syntax auch weiterhin vorhanden, aber auf eine leere Zeichenfolge festgelegt werden. Dies leider führt immer noch das gleiche Problem, das weiter oben erläuterten. Daher entfernen Sie entweder die Eigenschaft vollständig aus der deklarativen Syntax oder, im Eigenschaftenfenster, legen Sie den Wert mit dem Standard `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Schritt 3: Hinzufügen eines Daten-Web-Steuerelements und seiner Konfiguration für die Datenänderung

Nach dem ObjectDataSource-Steuerelement der Seite hinzugefügt und konfiguriert wurde, können wir Daten Websteuerelemente hinzufügen, auf der Seite, um sowohl die Daten anzuzeigen, und bieten eine Möglichkeit für den Endbenutzer zu ändern. Sehen wir uns die GridView, DetailsView und FormView-Steuerelement, wie diese datenwebsteuerelemente in ihren Möglichkeiten zur Datenänderung und die Konfiguration unterscheiden.

Wir sehen uns im weiteren Verlauf dieses Artikels, Hinzufügen von sehr einfachen bearbeiten, einfügen und löschen über Support im Rahmen der GridView, DetailsView und FormView-Steuerelemente ist wirklich so einfach wie eine Reihe von Kontrollkästchen. Es gibt viele feinheiten und Grenzfälle in der Praxis, die machen solche Funktionen, die komplizierter als nur zeigen, und klicken Sie auf bereitstellen. Dieses Tutorial konzentriert sich jedoch ausschließlich auf Beweisen einfache Möglichkeiten zur Datenänderung. Zukünftige Tutorials werden Probleme untersuchen, die zweifellos in einer realen Umgebung auftreten.

## <a name="deleting-data-from-the-gridview"></a>Löschen von Daten aus der GridView

Starten einer GridView-Ansicht aus der Toolbox in den Designer ziehen. Binden Sie als Nächstes dem ObjectDataSource-Steuerelement an die GridView, indem Sie ihn aus der Dropdown-Liste in den GridView Smarttag auswählen. An diesem Punkt werden die GridView deklarative Markup:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Binden die GridView, an dem ObjectDataSource-Steuerelement über die Smarttag hat zwei Vorteile:

- BoundFields und CheckBoxFields werden automatisch für jedes der Felder zurückgegeben werden, von dem ObjectDataSource-Steuerelement erstellt. Darüber hinaus werden die BoundField- und des CheckBoxField Eigenschaften basierend auf Metadaten für das zugrunde liegende Feld festgelegt. Z. B. die `ProductID`, `CategoryName`, und `SupplierName` Felder werden als schreibgeschützt markiert die `ProductsDataTable` und aus diesem Grund sollte nicht aktualisierbar sein beim Bearbeiten. Um diese, diese BoundFields aufzunehmen [ReadOnly-Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) festgelegt `true`.
- Die [DataKeyNames-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) Primärschlüsselfelder des zugrunde liegenden Objekts zugewiesen wird. Dies ist wichtig, wenn unter Verwendung der GridView bearbeiten oder Löschen von Daten, wie diese Eigenschaft gibt an, das Feld (oder einen Satz von Feldern), eindeutig identifiziert jeden Datensatz. Weitere Informationen zu den `DataKeyNames` -Eigenschaft verweisen zurück auf die [Master/Detail unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Tutorial.

Auf diese Weise während dem ObjectDataSource-Steuerelement über das Fenster "Eigenschaften" oder die deklarative Syntax des GridView gebunden sein kann, müssen Sie die entsprechenden BoundField manuell hinzufügen und `DataKeyNames` Markup.

Das GridView-Steuerelement bietet integrierte Unterstützung für auf Zeilenebene zu bearbeiten und löschen. Konfigurieren einer GridView-Ansicht um gelöscht zu unterstützen, fügt eine Spalte mit Schaltflächen zum Löschen. Klickt der Endbenutzer auf die Schaltfläche "löschen" für eine bestimmte Zeile, ein Postback Sicherheitsvorschriften und GridView führt die folgenden Schritte aus:

1. Das "ObjectDataSource" `DeleteParameters` Werte werden zugewiesen.
2. Das "ObjectDataSource" `Delete()` Methode wird aufgerufen, den angegebenen Datensatz löschen
3. Das GridView erneuerte Bindungen selbst auf dem ObjectDataSource-Steuerelement, durch den Aufruf der `Select()` Methode

Die Werte für die `DeleteParameters` sind die Werte der der `DataKeyNames` Felder für die Zeile, deren löschen-Schaltfläche geklickt wurde. Es ist daher wichtig, das einer GridView `DataKeyNames` Eigenschaft richtig festgelegt sein. Wenn sie nicht vorhanden ist, ist die `DeleteParameters` zugewiesen wird eine `null` Wert in Schritt 1, was wiederum nicht in einem führt gelöschten Datensätzen in Schritt2.

> [!NOTE]
> Die `DataKeys` Sammlung befindet sich in den GridView-s-Steuerelementzustand, was bedeutet, dass die `DataKeys` Werte werden während des Zurücksendens gespeichert werden, auch wenn der Ansichtszustand der GridView-s deaktiviert wurde. Allerdings ist es sehr wichtig, dass der Ansichtszustand für GridViews, die unterstützt werden, bearbeiten oder löschen (das Standardverhalten) aktiviert bleibt. Wenn Sie festlegen, dass das GridView-s `EnableViewState` Eigenschaft `false`, das Bearbeiten und Löschen von Verhalten werden problemlos für einen einzelnen Benutzer funktionieren, aber wenn gleichzeitige Benutzer, die Löschen von Daten vorhanden sind, besteht die Möglichkeit, dass diese gleichzeitige Benutzer versehentlich können Löschen oder bearbeiten, dass sie nihnen t aufgezeichnet werden soll. Finden Sie in meinem Blog, [Warnung: Problem der Parallelität mit ASP.NET 2.0 GridViews/DetailsView/FormViews diese Unterstützung zu bearbeiten bzw. löschen und, deren Ansichtszustand deaktiviert ist](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), Weitere Informationen.


Diese gleichen Warnung gilt auch für DetailsViews und FormViews verwendet werden.

Um das Löschen von Funktionen zu einer GridView-Ansicht hinzuzufügen, wechseln Sie zu seinem Smarttag und aktivieren Sie das Kontrollkästchen Löschen aktivieren.


![Überprüfen Sie das Löschen von Kontrollkästchen aktivieren](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Abbildung 10**: Überprüfen Sie das Löschen von Kontrollkästchen aktivieren


Aktivieren des Kontrollkästchens löschen aktivieren, aus dem Smarttag hinzugefügt GridView eine CommandField. Die CommandField rendert eine Spalte in der GridView mit Schaltflächen zum Ausführen von mindestens einer der folgenden Aufgaben: Wählen Sie einen Datensatz, einen Datensatz zu bearbeiten und Löschen eines Datensatzes. Wir bereits gesehen haben, die CommandField in Aktion auswählen von Datensätzen in der [Master/Detail unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Tutorial.

Die CommandField enthält eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen in der CommandField angezeigt werden. Durch Aktivieren des Kontrollkästchens löschen aktivieren einer CommandField, deren `ShowDeleteButton` Eigenschaft `true` GridView Columns-Auflistung hinzugefügt wurde.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

An diesem Punkt haben wir ob Sie es glauben oder nicht, mit dem Hinzufügen von Unterstützung für das Löschen von an die GridView! Wie in Abbildung 11 dargestellt, wenn auf dieser Seite über einen Browser eine Spalte mit Schaltflächen zum Löschen vorhanden ist.


[![Die CommandField Fügt eine Spalte mit Schaltflächen zum Löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Abbildung 11**: die CommandField Fügt eine Spalte der löschen-Schaltflächen ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Wenn Sie in diesem Tutorial von Anfang an auf sich selbst erstellen wurde haben beim Testen von dieser Seite klicken wird die Schaltfläche "löschen" eine Ausnahme ausgelöst. Lesen Sie, warum diese Ausnahmen ausgelöst wurden und zur Behebung dieser Probleme finden Sie weiter.

> [!NOTE]
> Wenn Sie die Verwendung der im Downloads enthalten, in diesem Tutorial befolgt haben, haben diese Probleme bereits berücksichtigt wurden. Allerdings sollten Sie durch die Identifizierung von Problemen, die auftreten können und geeignete problemumgehungen unten aufgeführten Details zu lesen.


Wenn Sie beim Versuch, ein Produkt zu löschen, eine Ausnahme angezeigt, dessen Meldung ist vergleichbar mit "*ObjectDataSource"ObjectDataSource1"nicht gefunden. eine nicht generische Methode"DeleteProduct", die über Parameter verfügt: ProductID, ursprünglichen\_ "ProductID"*, "haben Sie wahrscheinlich vergessen, entfernen Sie die `OldValuesParameterFormatString` Eigenschaft aus dem ObjectDataSource-Steuerelement. Mit der `OldValuesParameterFormatString` Eigenschaft angegeben ist, dem ObjectDataSource-Steuerelement für die Übergabe in beiden `productID` und `original_ProductID` Eingabeparameter der `DeleteProduct` Methode. `DeleteProduct`, akzeptiert jedoch nur einen einzelnen Eingabeparameter, daher die Ausnahme. Entfernen der `OldValuesParameterFormatString` Eigenschaft (oder wenn diese Option auf `{0}`) weist Sie dem ObjectDataSource-Steuerelement nicht versuchte, die in der ursprünglichen Eingabeparameter übergeben.


[![Stellen Sie sicher, dass sich die OldValuesParameterFormatString Eigenschaft gelöscht wurde](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Abbildung 12**: sicherstellen, dass die `OldValuesParameterFormatString` Eigenschaft verfügt über wurde gelöscht, ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Auch wenn Sie entfernt haben die `OldValuesParameterFormatString` Eigenschaft weiterhin erhalten Sie eine Ausnahme beim Versuch, ein Produkt mit der Meldung zu löschen: "*die DELETE-Anweisung steht in Konflikt mit der REFERENCE-Einschränkung" FK\_Reihenfolge\_Details \_Produkte*. " Die Northwind-Datenbank enthält eine fremdschlüsseleinschränkung zwischen der `Order Details` und `Products` Tabelle, d. h., ein Produkt kann nicht aus dem System gelöscht werden, treten ein oder mehrere Datensätze für die sie in der `Order Details` Tabelle. Da jedes Produkt in der Northwind-Datenbank mindestens ein Datensatz `Order Details`, es kann keine Produkte gelöscht, bis wir zunächst den produktanforderungen verknüpft Details bestellungsdatensätze löschen.


[![Eine Foreign Key-Einschränkung verhindert, dass das Löschen von Produkten](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Abbildung 13**: eine Foreign Key-Einschränkung verhindert, dass die Löschung der Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Für unser Tutorial lassen Sie uns einfach Löschen aller Datensätze aus der `Order Details` Tabelle. In einer realen Anwendung müssen wir entweder:

- Haben Sie einen anderen Bildschirm zum Verwalten von Bestellinformationen für details
- Erweitern Sie die `DeleteProduct` Methode, um die Logik, um die Details für das angegebene Produkt Reihenfolge gelöscht werden, enthalten.
- Ändern Sie die SQL-Abfrage, die von der TableAdapter verwendet, um das Löschen von Auftragsdetails für das angegebene Produkt enthalten

Lassen Sie uns einfach Löschen aller Datensätze aus der `Order Details` Tabelle, foreign Key-Einschränkung zu umgehen. Wechseln Sie zu dem Server-Explorer in Visual Studio, mit der rechten Maustaste auf die `NORTHWND.MDF` Knoten, und wählen Sie die neue Abfrage. Führen Sie dann im Abfragefenster die folgende SQL-Anweisung ein: `DELETE FROM [Order Details]`


[![Löschen Sie alle Datensätze aus der Order Details-Tabelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Abbildung 14**: Löschen Sie alle Datensätze aus der `Order Details` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Nach dem beseitigen der `Order Details` Tabelle, die Sie auf die Schaltfläche "löschen" klicken, wird das Produkt ohne Fehler gelöscht. Wenn das Produkt durch Klicken auf die Schaltfläche "Delete" nicht gelöscht wird, zu überprüfen, um sicherzustellen, dass der GridView `DataKeyNames` -Eigenschaftensatz auf Feld mit dem Primärschlüssel (`ProductID`).

> [!NOTE]
> Beim Klicken auf die Schaltfläche "löschen" erfolgt ein Postback aus, und der Datensatz gelöscht. Dies kann gefährlich sein, da es einfach, versehentlich klicken auf die Schaltfläche zum Löschen der falschen Zeile handelt. In einem späteren Tutorial sehen wir, wie eine clientseitiger Bestätigung hinzufügen, wenn Sie einen Datensatz zu löschen.


## <a name="editing-data-with-the-gridview"></a>Bearbeiten von Daten mit GridView

Zusammen mit löschen, stellt das GridView-Steuerelement auch integrierte Unterstützung für die Bearbeitung auf Zeilenebene bereit. Konfigurieren einer GridView-Ansicht zur Unterstützung der Bearbeitung Fügt eine Spalte mit Bearbeitungsschaltflächen hinzu. Wenn Sie aus Sicht des Endbenutzers auf einer Zeile Bearbeiten Schaltfläche Ursachen, die Zeile bearbeitet werden, werden die Zellen in Textfelder ein, die die vorhandenen Werte enthält, und ersetzen die Schaltfläche "Bearbeiten" mit Update und "Abbrechen" Schaltflächen. Treffen Sie die gewünschten Änderungen vor, und können Benutzer die Schaltfläche "Aktualisieren", um die Änderungen zu übernehmen oder die Schaltfläche "Abbrechen", um sie zu verwerfen klicken. In beiden Fällen nach dem Klicken auf aktualisieren, oder der GridView-gibt den Zustand vor der Bearbeitung abbrechen.

Aus unserer Sicht als Seitenentwickler klickt der Endbenutzer auf die Schaltfläche "Bearbeiten" für eine bestimmte Zeile, ein Postback Sicherheitsvorschriften und GridView führt die folgenden Schritte aus:

1. GridView `EditItemIndex` -Eigenschaft zugewiesen, der Index der Zeile, deren Schaltfläche "Bearbeiten" geklickt wurde,
2. Das GridView erneuerte Bindungen selbst auf dem ObjectDataSource-Steuerelement, durch den Aufruf der `Select()` Methode
3. Der Index der Zeile entspricht der `EditItemIndex` gerendert wird, in "Edit Mode." In diesem Modus wird die Schaltfläche "Bearbeiten" Schaltflächen "Update" und "Abbrechen" und BoundFields ersetzt, deren `ReadOnly` Eigenschaften sind falsch (Standardeinstellung) werden als Textfeld Web, dessen Steuerelemente gerendert `Text` Eigenschaften werden die Datenfelder Werte zugewiesen.

An diesem Punkt wird das Markup an den Browser ermöglichen dem Endbenutzer keine Änderungen vornehmen, auf die Zeile der Daten zurückgegeben. Klickt der Benutzer auf die Schaltfläche "Aktualisieren", ein Postback auftritt, und die GridView führt die folgenden Schritte aus:

1. Das "ObjectDataSource" `UpdateParameters` Werte werden durch den Endbenutzer in den GridView Bearbeitungsoberfläche eingegebenen Werte zugewiesen
2. Das "ObjectDataSource" `Update()` Methode wird aufgerufen, den angegebenen Datensatz aktualisieren
3. Das GridView erneuerte Bindungen selbst auf dem ObjectDataSource-Steuerelement, durch den Aufruf der `Select()` Methode

Die Primärschlüsselwerte zugewiesen der `UpdateParameters` stammen Sie in Schritt 1 im angegebenen Werte die `DataKeyNames` -Eigenschaft, während nicht-primäre Schlüsselwerte aus dem Text in das Textfeld Websteuerelementen für die bearbeitete Zeile stammen. Wie gelöscht werden, es unerlässlich ist, das einer GridView `DataKeyNames` Eigenschaft richtig festgelegt sein. Wenn sie nicht vorhanden ist, ist die `UpdateParameters` Primärschlüsselwert zugewiesen eine `null` Wert in Schritt 1, was wiederum nicht in einem führt aktualisiert Datensätze, die in Schritt2.

Editing-Funktionalität kann durch einfaches aktivieren das Kontrollkästchen Bearbeiten aktivieren, in den GridView Smarttag aktiviert werden.


![Überprüfen Sie die Bearbeitung von Kontrollkästchen aktivieren](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Abbildung 15**: Überprüfen Sie die Bearbeitung von Kontrollkästchen aktivieren


Überprüfen das Kontrollkästchen Bearbeiten aktivieren wird eine CommandField hinzufügen (sofern erforderlich), und legen dessen `ShowEditButton` Eigenschaft `true`. Wie wir gesehen haben, wird die CommandField enthält eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen in der CommandField angezeigt werden. Aktivieren des Kontrollkästchens Bearbeiten aktivieren fügt die `ShowEditButton` Eigenschaft, um die vorhandenen CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Das ist alles zum Hinzufügen von Unterstützung für die rudimentäre Bearbeitung vorhanden ist. Die Bearbeitungsschnittstelle Figure16 zeigt, ist recht einfach gehalten jedes BoundField, deren `ReadOnly` -Eigenschaftensatz auf `false` (Standard) als ein TextBox-Element gerendert wird. Dies beinhaltet Felder wie `CategoryID` und `SupplierID`, die Schlüssel auf andere Tabellen sind.


[![Durch Klicken auf Chai s Schaltfläche "Bearbeiten" wird die Zeile im Bearbeitungsmodus angezeigt.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Abbildung 16**: s der Chai durch Klicken auf Schaltfläche "Bearbeiten" zeigt die Zeile im Bearbeitungsmodus ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Zusätzlich zu gerätebenutzern Fremdschlüsselwerte direkt zu bearbeiten, fehlen die Bearbeitungsschnittstelle-Schnittstelle auf folgende Weise:

- Wenn der Benutzer eingibt eine `CategoryID` oder `SupplierID` nicht in der Datenbank vorhandenen der `UPDATE` wird verletzt eine foreign Key-Einschränkung, dass eine Ausnahme ausgelöst werden soll.
- Die Bearbeitungsschnittstelle enthält keine Validierung. Wenn Sie nicht, dass einen erforderlichen Wert angeben (z. B. `ProductName`), oder geben Sie einen Zeichenfolgenwert ein, wo ein numerischer Wert erwartet wird (z. B. durch Eingabe von "Zu viel!" in der `UnitPrice` Textfeld), wird eine Ausnahme ausgelöst werden. Eine zukünftige Tutorial wird das Hinzufügen von Steuerelementen zur gültigkeitsprüfung die Bearbeitung der Benutzeroberfläche untersuchen.
- Derzeit *alle* Product-Felder, die nicht schreibgeschützt sind, die in den GridView-Ansicht enthalten sein müssen. Würden wir zum Entfernen eines Felds aus der GridView, z. B. `UnitPrice`beim Aktualisieren der Daten der GridView nicht festgelegt, wird die `UnitPrice` `UpdateParameters` -Wert, der des Datenbankdatensatzes ändern würde `UnitPrice` auf eine `NULL` Wert. Auf ähnliche Weise, wenn ein erforderliches Feld, z. B. `ProductName`, entfernt von GridView-das Update schlägt fehl, mit dem gleichen "*Spaltenname 'ProductName' lässt keine NULL-Werte*" Ausnahme, die oben genannten.
- Die Bearbeitung Schnittstelle Formatierung bleibt viel zu wünschen übrig. Die `UnitPrice` mit vier Dezimalstellen angezeigt. Im Idealfall die `CategoryID` und `SupplierID` Werte würde die DropDownList-Steuerelementen, die Liste der Kategorien und Lieferanten im System enthalten.

Hierbei handelt es sich um alle Mängel, die wir jetzt, aber ist mit live werden in zukünftigen Lernprogrammen adressiert werden.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Einfügen, bearbeiten und Löschen von Daten mit DetailsView

Wie wir in den vorherigen Tutorials gesehen haben, wird von DetailsView-Steuerelement zeigt einen Datensatz, zu einem Zeitpunkt, und wie GridView, ermöglicht das Bearbeiten und Löschen des derzeit angezeigten Datensatzes. Sowohl der Endbenutzer-Erfahrung mit bearbeiten und Löschen von Elementen aus einem DetailsView und der Workflow von der Seite von ASP.NET ist identisch mit der GridView. GridView, DetailsView unterscheidet ist, dass es auch integrierten einfügen Unterstützung bietet.

Um die Möglichkeiten zur Datenänderung der GridView zu demonstrieren, Hinzufügen einer DetailsView, zunächst die `Basics.aspx` Seite über der vorhandenen GridView, und binden sie an der vorhandenen "ObjectDataSource" über DetailsViews-Smarttag. Weiter, löschen Sie die DetailsView `Height` und `Width` Eigenschaften, und aktivieren Sie die Option Paging aktivieren, aus dem Smarttag. Zum Aktivieren der Bearbeitung die Kontrollkästchen einfügen und Löschen von Unterstützung, einfach die Bearbeitung aktivieren, aktivieren Sie einfügen und löschen aktivieren im Smarttag.


![Konfigurieren der DetailsView an den Support, bearbeiten, einfügen und löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Abbildung 17**: konfigurieren die DetailsView an den Support, bearbeiten, einfügen und löschen


Als hinzugefügt mit GridView hinzufügen, bearbeiten, einfügen oder löschen die Unterstützung einer CommandField DetailsView, wie im folgenden deklarative Syntax gezeigt:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Beachten Sie, dass für DetailsView der CommandField am Ende der Auflistung der Spalten in der Standardeinstellung angezeigt wird. Da die Felder der DetailsView als Zeilen gerendert werden die CommandField angezeigt wird, als eine Zeile mit Insert, bearbeiten und Löschen von Schaltflächen am unteren Rand der DetailsView.


[![Konfigurieren der DetailsView an den Support, bearbeiten, einfügen und löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Abbildung 18**: DetailsView zu bearbeiten, einfügen und löschen-Unterstützung konfigurieren ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Durch Klicken auf die Schaltfläche "löschen" die gleiche Sequenz von Ereignissen wie bei der GridView beginnt: a Postbacks gefolgt von der füllen seine "ObjectDataSource" DetailsView `DeleteParameters` basierend auf den `DataKeyNames` Werte und abgeschlossen durch einen Aufruf der "ObjectDataSource" `Delete()` -Methode, die das Produkt tatsächlich aus der Datenbank entfernt. Bearbeitung im DetailsView funktioniert auch in einer Weise, die identisch mit der GridView.

Zum Einfügen, der Endbenutzer erhält eine neu-Schaltfläche geklickt wird, rendert die DetailsView im "Einfügemodus befindet." Mit "Insert-Modus" wird die Schaltfläche "neue" Schaltflächen "Insert" und "Abbrechen", und nur diese BoundFields ersetzt, deren `InsertVisible` -Eigenschaftensatz auf `true` (Standardeinstellung) werden angezeigt. Diese identifiziert als automatisch inkrementierte Felder, z. B. Datenfelder `ProductID`, haben ihre [InsertVisible-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) festgelegt `false` beim Binden von DetailsView an die Datenquelle über das Smarttag.

Beim Binden einer Datenquelle an ein DetailsView über das Smarttag, legt Visual Studio die `InsertVisible` Eigenschaft `false` nur für automatisch inkrementierte Felder. Schreibgeschützte Felder, z. B. `CategoryName` und `SupplierName`, wird in der Benutzeroberfläche "Einfügemodus" angezeigt werden, es sei denn, ihre `InsertVisible` -Eigenschaftensatz explizit auf `false`. Legen Sie die folgenden beiden Felder in Ruhe `InsertVisible` Eigenschaften `false`, über deklarative Syntax des DetailsView oder über die Felder bearbeiten verknüpfen Sie im Smarttag. Abbildung 19 zeigt die Einstellung der `InsertVisible` Eigenschaften `false` durch Klicken auf die Felder bearbeiten zu verknüpfen.


[![Northwind Traders bietet jetzt die Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Abbildung 19**: Northwind Traders jetzt bietet Acme Tee ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Nach dem Festlegen der `InsertVisible` Eigenschaften, die Ansicht der `Basics.aspx` Seite in einem Browser, und klicken Sie auf die Schaltfläche "Neu". Abbildung 20 zeigt die DetailsView beim Hinzufügen einer neuen trinken, Acme Tee, um unsere-Produktlinie.


[![Northwind Traders bietet jetzt die Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Abbildung 20**: Northwind Traders jetzt bietet Acme Tee ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Nach dem Sie die Details für die Acme Tee eingeben, und klicken Sie auf die Schaltfläche "Insert", erfolgt ein Postback und der neue Datensatz hinzugefügt wird die `Products` Datenbanktabelle. Da diese DetailsView der Produkte in der Reihenfolge aufgeführt, mit dem sie in der Tabelle der Datenbank vorhanden sind, müssen wir Produkt bis zum letzten Seite, um das neue Produkt finden Sie unter.


[![Details für die Acme Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Abbildung 21**: Details für die Acme Tee ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsViews [CurrentMode Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) gibt die Schnittstelle wird angezeigt und kann einen der folgenden Werte: `Edit`, `Insert`, oder `ReadOnly`. Die [Eigenschaft "DefaultMode"](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) gibt an, der Modus DetailsView, gibt an, nach einer Bearbeitung, oder fügen Sie abgeschlossen wurde, und eignet sich für ein DetailsView, die dauerhaft in Bearbeitung oder Einfügemodus anzeigen.


Der Punkt und klicken Sie hier einfügen und Bearbeitungsfunktionen von DetailsView Leiden dieselben Einschränkungen wie GridView: der Benutzer muss die vorhandene eingeben `CategoryID` und `SupplierID` Werte über ein Textfeld; die besitzt der Schnittstelle keine Überprüfungslogik; alle Produkt-Felder, die keine zulassen `NULL` Werte oder keinen Standard auf der Datenbankebene angegebene Wert in der einfügen-Schnittstelle, und So weiter enthalten sein muss.

Die Techniken untersuchen wir zum Erweitern und Verbessern der GridView Bearbeitungsoberfläche in zukünftigen Artikeln auf der DetailsView-Steuerelement bearbeiten und Einfügen von Schnittstellen angewendet werden können.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Verwenden das FormView-Steuerelement für eine flexiblere Änderung Benutzeroberfläche

Die FormView-Steuerelement bietet integrierte Unterstützung für einfügen, bearbeiten und Löschen von Daten, aber da es Vorlagen anstelle von Feldern verwendet ist kein Platz für das Hinzufügen der BoundFields oder die CommandField durch die GridView und DetailsView-Steuerelemente verwendet, um die Daten bereitzustellen Änderung-Schnittstelle. Diese Schnittstelle, die die Web-Steuerelemente für das Sammeln von Benutzer eingeben, wenn Sie ein neues Element hinzufügen oder bearbeiten eine vorhandene zusammen mit der neu bearbeiten, löschen, einfügen, aktualisieren und Abbrechen (Schaltflächen) muss stattdessen manuell an den entsprechenden Vorlagen hinzugefügt werden. Glücklicherweise wird Visual Studio automatisch die erforderliche Schnittstelle erstellen, bei der Bindung von FormView an eine Datenquelle in der Dropdown-Liste in der Smarttag.

Um diese Techniken zu veranschaulichen, starten Sie durch das Hinzufügen von einem FormView-Steuerelement, das `Basics.aspx` Seite, und binden Sie sie aus der FormView smart Tag, an dem ObjectDataSource-Steuerelement bereits erstellt. Dadurch wird ein `EditItemTemplate`, `InsertItemTemplate`, und `ItemTemplate` für die FormView-Steuerelement mit dem TextBox-Websteuerelemente zum Sammeln von Eingabe- und Schaltfläche Websteuerelemente des Benutzers für die neue, bearbeiten, löschen, einfügen, aktualisieren und Abbrechen (Schaltflächen). Darüber hinaus der FormView `DataKeyNames` -Eigenschaftensatz auf Feld mit dem Primärschlüssel (`ProductID`) des Objekts zurückgegeben wird, von dem ObjectDataSource-Steuerelement. Aktivieren Sie abschließend in das FormView Smarttag-Option Paging aktivieren.

Das folgende Beispiel zeigt die deklarative Markup für der FormView `ItemTemplate` nachdem das FormView-Steuerelement auf dem ObjectDataSource-Steuerelement gebunden wurde. Standardmäßig jedes Product-Feld nicht booleschen Wert gebunden ist, um die `Text` Eigenschaft eines Label-Websteuerelements während jedes Feld boolescher Wert (`Discontinued`) gebunden ist die `Checked` Eigenschaft ein deaktiviertes Kontrollkästchen-Steuerelement. In der Reihenfolge für die Schaltflächen New, Edit und Delete auf bestimmte FormView-Verhalten beim Klicken auf auslösen, ist es zwingend erforderlich, die ihre `CommandName` Werte festgelegt werden, um `New`, `Edit`, und `Delete`bzw.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Abbildung 22 zeigt der FormView `ItemTemplate` über einen Browser angezeigt. Jedes Feld "Product" wird mit den New, Edit und Delete-Schaltflächen unten aufgeführt.


[![Das weisen FormView ItemTemplate führt jedes Feld "Product" sowie neue, bearbeiten und Löschen von Schaltflächen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Abbildung 22**: die weisen FormView `ItemTemplate` Listet jedes Produkt Feld zusammen mit Schaltflächen löschen, bearbeiten und neu ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Wie Sie mit GridView und DetailsView, klicken Sie auf die Schaltfläche "löschen" oder eine beliebige Schaltfläche, LinkButton oder ImageButton, deren `CommandName` Eigenschaft auf festgelegt wird, löschen bewirkt, dass einen Postback, füllt das "ObjectDataSource" `DeleteParameters` basierend auf der FormView `DataKeyNames`Wert und dem ObjectDataSource-Steuerelement ruft `Delete()` Methode.

Wenn auf die Schaltfläche "Bearbeiten" geklickt wird, erfolgt ein Postback und die Daten werden erneut gebunden, um die `EditItemTemplate`, die zum Rendern der Bearbeitungsschnittstelle verantwortlich ist. Diese Schnittstelle enthält die Web-Steuerelemente zum Bearbeiten von Daten zusammen mit den Schaltflächen Aktualisieren und "Abbrechen". Der Standardwert `EditItemTemplate` vom Visual Studio enthält eine Bezeichnung für automatisch inkrementierte Felder (`ProductID`), ein Textfeld für die einzelnen Felder nicht boolescher Wert und ein Kontrollkästchen für jedes Feld boolescher Wert. Dieses Verhalten ähnelt sehr der automatisch generierten BoundFields in den GridView und DetailsView-Steuerelementen.

> [!NOTE]
> Ein kleines Problem bei der FormView automatische Generierung von der `EditItemTemplate` besteht darin, dass die TextBox-Web-Steuerelemente für diese Felder rendert, die schreibgeschützt, z. B. `CategoryName` und `SupplierName`. Erfahren Sie, wie an diesen in Kürze.


Das TextBox-Steuerelemente der `EditItemTemplate` haben ihre `Text` -Eigenschaft gebunden, auf den Wert eines ihre entsprechenden Daten unter Verwendung *bidirektionale Datenbindung*. Bidirektionale Datenbindung, gekennzeichnet durch `<%# Bind("dataField") %>`, Datenbindung, die beide ausführt, beim Binden von Daten mit der Vorlage und dem ObjectDataSource-Steuerelement-Parameter für das Einfügen oder Bearbeiten von Datensätzen zu füllen. D. h., wenn der Benutzer die Schaltfläche "Bearbeiten" aus klickt der `ItemTemplate`, `Bind()` -Methode gibt den Wert des angegebenen Felds. Nachdem der Benutzer ihre Änderungen werden und Updates klickt, veröffentlicht die Werte zurück, die entsprechen, die Datenfelder angegeben `Bind()` gelten für das "ObjectDataSource" `UpdateParameters`. Alternativ unidirektionale Datenbindung, gekennzeichnet durch `<%# Eval("dataField") %>`, werden nur die Feldwerte für die Daten abgerufen, beim Binden von Daten mit der Vorlage und ist *nicht* Postback der vom Benutzer eingegebenen Werten zum Parameter mit der Datenquelle zurück.

Das folgende deklarative Markup zeigt die FormView `EditItemTemplate`. Beachten Sie, dass die `Bind()` Methode ist hier die Databinding-Syntax verwendet und der Update- und Schaltfläche-Abbrechen-Steuerelemente haben ihre `CommandName` Eigenschaften entsprechend festlegen.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Unsere `EditItemTemplate`, zurzeit zeigen, wird eine Ausnahme ausgelöst wird, wenn wir versuchen, ihn zu verwenden. Das Problem besteht darin, die die `CategoryName` und `SupplierName` Felder werden gerendert werden, da Textfeld Web gesteuert wird, in der `EditItemTemplate`. Wir müssen entweder diese Textfelder in Bezeichnungen zu ändern oder entfernen Sie sie vollständig. Lassen Sie uns einfach wieder löschen vollständig aus der `EditItemTemplate`.

Abbildung 23 zeigt das FormView-Steuerelement in einem Browser, nachdem auf die Schaltfläche "Bearbeiten" für Chai geklickt wurde. Beachten Sie, dass die `SupplierName` und `CategoryName` Felder, die der `ItemTemplate` sind nicht mehr vorhanden ist, wie wir gerade entfernt sie aus der `EditItemTemplate`. Die gleiche Sequenz von Schritten wie GridView und DetailsView durchläuft das FormView-Steuerelement, wenn auf die Schaltfläche "Aktualisieren" geklickt wird.


[![Standardmäßig zeigt die EditItemTemplate jedes bearbeitbare Produktfeld als Textfeld oder das Kontrollkästchen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Abbildung 23**: standardmäßig die `EditItemTemplate` zeigt jedes bearbeitbare Feld "Product" als Textfeld oder Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Wenn auf die Schaltfläche "Insert" der FormView geklickt wird `ItemTemplate` Sicherheitsvorschriften ein Postback. Allerdings ist keine Daten auf das FormView-Steuerelement gebunden, da ein neuer Datensatz hinzugefügt wird. Die `InsertItemTemplate` Schnittstelle umfasst die Web-Steuerelemente zum Hinzufügen eines neuen Datensatzes zusammen mit den Schaltflächen einfügen und Abbrechen. Der Standardwert `InsertItemTemplate` vom Visual Studio enthält ein Textfeld für die einzelnen Felder nicht boolescher Wert und ein Kontrollkästchen für jedes Feld boolescher Wert, ähnlich den automatisch generierten `EditItemTemplate`der Schnittstelle. Die TextBox-Steuerelemente haben ihre `Text` -Eigenschaft gebunden, auf den Wert ihrer entsprechenden Feld "Daten" Verwenden der bidirektionalen Datenbindung.

Das folgende deklarative Markup zeigt die FormView `InsertItemTemplate`. Beachten Sie, dass die `Bind()` Methode ist hier die Databinding-Syntax verwendet und die Websteuerelemente einfügen und Abbrechen Schaltfläche haben ihre `CommandName` Eigenschaften entsprechend festlegen.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Es gibt eine Besonderheit mit der FormView automatische Generierung von der `InsertItemTemplate`. Insbesondere werden die TextBox-Websteuerelemente erstellt, auch für diese Felder schreibgeschützt, z. B. `CategoryName` und `SupplierName`. Wie Sie mit der `EditItemTemplate`, müssen wir entfernen diese Textfelder aus der `InsertItemTemplate`.

Abbildung 24 zeigt das FormView-Steuerelement in einem Browser, wenn ein neues Produkt, Acme Kaffee hinzufügen. Beachten Sie, dass die `SupplierName` und `CategoryName` Felder, die der `ItemTemplate` sind nicht mehr vorhanden ist, wie wir gerade entfernt. Wenn die Schaltfläche "Insert" der FormView-Steuerelement wird fortgesetzt, über die gleiche Sequenz von Schritten wie DetailsView-Steuerelement geklickt wird, Hinzufügen eines neuen Datensatzes in die `Products` Tabelle. Abbildung 25 zeigt Acme Kaffee Produktdetails in das FormView-Steuerelement, nachdem es eingefügt wurde.


[![Die InsertItemTemplate bestimmt das FormView einfügen-Schnittstelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Abbildung 24**: die `InsertItemTemplate` bestimmt der FormView einfügen-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Die Details für das neue Produkt, Acme Kaffee, werden in der FormView-Steuerelement angezeigt.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Abbildung 25**: die Details für das neue Produkt, Acme Kaffee, werden angezeigt, in das FormView-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Durch Herausnehmen von der schreibgeschützten, ermöglicht das Bearbeiten und Einfügen von Schnittstellen in drei separate Vorlagen, die FormView-Steuerelement für eine feiner abgestufte Kontrolle über diese Schnittstellen als das GridView und DetailsView die.

> [!NOTE]
> Wie DetailsView, FormView `CurrentMode` Eigenschaft gibt an, die Schnittstelle, die angezeigt wird und die zugehörige `DefaultMode` Eigenschaft gibt den Modus der FormView-Steuerelement gibt, nach einer Bearbeitung oder Einfügevorgang abgeschlossen wurde.


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial untersucht es die Grundlagen der einfügen, bearbeiten und Löschen von Daten mit der GridView, DetailsView und FormView-Steuerelement. Alle drei dieser Steuerelemente bieten einige integrierte Möglichkeiten zur Datenänderung, die verwendet werden kann, ohne eine einzige Zeile Code schreiben, auf der ASP.NET-Seite Dank den datenwebsteuerelementen und dem ObjectDataSource-Steuerelement. Allerdings das einfache zeigen und klicken Sie auf Methoden rendern eine ziemlich frail und naive Änderung Benutzeroberfläche. Um die Validierung bereitstellen zu können, programmgesteuerte Werte einfügen, ordnungsgemäß Behandeln von Ausnahmen, Anpassen der Benutzeroberfläche und usw., müssen wir eine Reihe von Techniken abhängen, die über die nächsten mehrere Tutorials erläutert werden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
