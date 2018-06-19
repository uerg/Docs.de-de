---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Hinzufügen von und reagieren auf Schaltflächen an eine GridView (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir uns zum Hinzufügen von benutzerdefinierter Schaltflächen, damit eine Vorlage und die Felder eines GridView oder DetailsView-Steuerelements. Insbesondere müssen wir Bui...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90648e10d5d058ea2e4aa5b3d8c4ed7448ea7166
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878294"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Hinzufügen von und reagieren auf Schaltflächen an eine GridView (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) oder [PDF herunterladen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> In diesem Lernprogramm sehen wir uns zum Hinzufügen von benutzerdefinierter Schaltflächen, damit eine Vorlage und die Felder eines GridView oder DetailsView-Steuerelements. Insbesondere müssen wir eine Schnittstelle erstellen, die einen FormView, die dem Benutzer ermöglicht verfügt, durch die Lieferanten blättern.


## <a name="introduction"></a>Einführung

Während viele berichtsszenarien nur-Lese Zugriff auf die Berichtsdaten einschließen, ist es nicht ungewöhnlich, dass Berichte die Fähigkeit zum Ausführen von Aktionen, die basierend auf der angezeigten Daten enthalten. In der Regel dadurch beteiligt Hinzufügen einer Schaltfläche, LinkButton oder ImageButton-Steuerelements mit jeder Datensatz im Bericht angezeigt, die beim Klicken auf einen Postback verursacht und serverseitigen Code aufruft. Bearbeiten und Löschen von Daten auf Basis von Datensätzen ist das häufigste Beispiel. In der Tat wie wir gesehen haben, beginnend mit der [Übersicht des einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm bearbeiten und Löschen von wird so häufig, dass solche Funktionen ohne GridView, DetailsView und FormView unterstützt die für eine einzige Codezeile schreiben müssen.

Darüber hinaus zum Bearbeiten und Löschen von Schaltflächen, die GridView, DetailsView und FormView-Steuerelemente können auch enthalten Schaltflächen, LinkButtons oder ImageButtons, wenn angeklickt, führen Sie einige benutzerdefinierte serverseitige Logik. In diesem Lernprogramm sehen wir uns zum Hinzufügen von benutzerdefinierter Schaltflächen, damit eine Vorlage und die Felder eines GridView oder DetailsView-Steuerelements. Insbesondere müssen wir eine Schnittstelle erstellen, die einen FormView, die dem Benutzer ermöglicht verfügt, durch die Lieferanten blättern. Für einen bestimmten Lieferanten wird FormView Informationen zu den Lieferanten zusammen mit einer Schaltfläche Websteuerelement angezeigt, die, wenn geklickt haben, alle ihre zugeordneten Produkte markieren wird als nicht mehr unterstützt. Darüber hinaus enthält eine GridView dieser Produkte, die den ausgewählten Lieferanten mit jeder Zeile mit Preis erhöhen und Discount Preis Schaltflächen, die, wenn geklickt haben, erhöhen oder verringern Sie des Produkts, gebotenen `UnitPrice` um 10 % (siehe Abbildung 1).


[![Die FormView und die GridView enthalten Schaltflächen, die benutzerdefinierte Aktionen durchzuführen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Abbildung 1**: beide FormView und GridView enthalten Schaltflächen, führen benutzerdefinierte Aktionen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten der Schaltfläche-Lernprogramm

Bevor wir untersucht, wie eine benutzerdefinierten Schaltflächen hinzufügen, werfen einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm müssen zuerst. Starten, indem Sie einen neuen Ordner namens `CustomButtons`. Fügen Sie die folgenden beiden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `CustomButtons.aspx`


![Fügen Sie die ASP.NET-Seiten für die benutzerdefinierte Schaltflächen-bezogene Lernprogramme](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die benutzerdefinierte Schaltflächen-bezogene Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `CustomButtons` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf der Seite Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Fügen Sie abschließend die Seiten hinzu, als Einträge an die `Web.sitemap` Datei. Insbesondere das folgende Markup nach dem Paging und sortieren hinzufügen `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für die Bearbeitung, einfügen und Löschen von Lernprogramme.


![Die Siteübersicht enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Lernprogramm](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Abbildung 4**: die Siteübersicht enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Lernprogramm


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Schritt 2: Hinzufügen einen FormView, die die Lieferanten auflistet.

Wir zum Einstieg dieses Lernprogramms durch Hinzufügen, die die Lieferanten listet FormView. Wie in der Einführung erwähnt, können diese FormView Benutzer seitenweise durch die Lieferanten, die Produkte, die von den Lieferanten in einem GridView bereitgestellten angezeigt. Darüber hinaus schließt dieses FormView eine Schaltfläche, die beim Klicken auf alle Produkte der Lieferant als markiert nicht mehr unterstützt. Bevor wir uns fügen Sie die benutzerdefinierte Schaltfläche FormView betreffen, wir zuerst erstellen Sie einfach die FormView so, dass die Lieferanteninformationen angezeigt.

Öffnen Sie zunächst die `CustomButtons.aspx` auf der Seite der `CustomButtons` Ordner. Hinzufügen einen FormView auf der Seite durch ziehen es aus der Toolbox auf die Designer, und legen seine `ID` Eigenschaft `Suppliers`. Erstellen Sie eine neue ObjectDataSource mit dem Namen aus der FormView-Smarttag, wahlweise `SuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource namens `SuppliersDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Diese neue ObjectDataSource so konfigurieren, dass er von Abfragen die `SuppliersBLL` Klasse `GetSuppliers()` Methode (siehe Abbildung 6). Da diese FormView keine Schnittstelle bietet für die Lieferanteninformationen aktualisieren, wählen Sie, dass die (None) aus der Dropdown-Liste auf der Registerkarte "UPDATE"-option.


[![Konfigurieren Sie die Datenquelle zur Verwendung der Klasse SuppliersBLL s GetSuppliers()-Methode](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Abbildung 6**: Konfigurieren Sie die Datenquelle verwendet den `SuppliersBLL` Klasse `GetSuppliers()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Nach dem Konfigurieren der ObjectDataSource, generiert Visual Studio eine `InsertItemTemplate`, `EditItemTemplate`, und `ItemTemplate` für FormView. Entfernen Sie die `InsertItemTemplate` und `EditItemTemplate` und Ändern der `ItemTemplate` , sodass sie nur die Lieferanten Company Name und Phone Nummer angezeigt. Schließlich auslagerungsunterstützung für FormView schalten, indem Sie das Paging aktivieren Kontrollkästchen aus seinem Smarttag (oder durch Festlegen seiner `AllowPaging` Eigenschaft `True`). Nach der Änderung sollte deklarativem Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Abbildung 7 zeigt die Seite CustomButtons.aspx, wenn Sie über einen Browser angezeigt.


[![Die FormView Listet die CompanyName und Phone Felder aus den aktuell ausgewählten Lieferanten](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Abbildung 7**: der FormView-Listet die `CompanyName` und `Phone` Felder aus den derzeit ausgewählten Lieferanten ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Schritt 3: Hinzufügen einer GridView, die den ausgewählten Lieferanten Produkte aufgeführt sind

Bevor wir die beenden Sie die Schaltfläche "alle Produkte" der FormView-Vorlage hinzuzufügen, fügen wir zuerst hinzu GridView unter FormView, die die von den ausgewählten Lieferanten bereitgestellte Produkte aufgeführt sind. Legen Sie dazu, Hinzufügen einer GridView auf der Seite ", seine `ID` Eigenschaft `SuppliersProducts`, und fügen Sie eine neue ObjectDataSource mit dem Namen `SuppliersProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersProductsDataSource ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Abbildung 8**: Erstellen einer neuen ObjectDataSource namens `SuppliersProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Konfigurieren Sie diese ObjectDataSource zur Verwendung der ProductsBLL Klasse `GetProductsBySupplierID(supplierID)` Methode (siehe Abbildung 9). Während dieser GridView werden für den Preis eines Produkts angepasst werden kann, wird nicht die integrierte bearbeiten oder Löschen von Funktionen aus der GridView verwendet wird. Daher können wir die Dropdown-Liste (keine) für das ObjectDataSource des Update-, INSERT- und Löschen von Registerkarten festlegen.


[![Konfigurieren Sie die Datenquelle zur Verwendung der Klasse ProductsBLL s GetProductsBySupplierID(supplierID)-Methode](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Abbildung 9**: Konfigurieren Sie die Datenquelle verwendet den `ProductsBLL` Klasse `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Da die `GetProductsBySupplierID(supplierID)` Methode akzeptiert einen Eingabeparameter, der ObjectDataSource-Assistent fordert Sie uns für die Quelle dieser Parameterwert. Übergeben der `SupplierID` aus FormView Wert, der Parameter-Quellliste Dropdown-Steuerelement und die ControlID Dropdown-Liste festgelegt `Suppliers` (in Schritt2 erstellte die FormView-ID).


[![Um anzugeben Sie, dass die SupplierID Parameter vom Lieferanten FormView Steuerelement stammen soll](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Abbildung 10**: darauf hinweisen, dass die *`supplierID`* Parameter herstammt sollte die `Suppliers` FormView-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Nach Abschluss des Assistenten ObjectDataSource wird die GridView eine BoundField- oder CheckBoxField, für die einzelnen Datenfelder des Produkts enthalten. Trim dies wir nach unten zum Anzeigen nur die `ProductName` und `UnitPrice` BoundFields zusammen mit der `Discontinued` CheckBoxField; darüber hinaus wir Formatieren der `UnitPrice` BoundField, dass sein Text als Währung formatiert ist. Die GridView und `SuppliersProductsDataSource` ObjectDataSources deklarative Markup sollte etwa wie das folgende Markup aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

Unser Tutorial zeigt zu diesem Zeitpunkt einen Master-/Detail-Bericht, der Benutzer einen Lieferanten FormView oben ausgewählt und die Produkte von Lieferanten über die GridView unten bereitgestellten anzeigen. Abbildung 11 zeigt einen Screenshot der Seite beim Lieferanten Tokyo Traders aus FormView auswählen.


[![Die ausgewählte s Lieferanten werden in der GridView angezeigt.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Abbildung 11**: die ausgewählte des Lieferanten in der GridView angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Schritt 4: Erstellen von DAL und BLL-Methoden, um alle Produkte für ein Lieferant einstellen

Bevor wir die FormView eine Schaltfläche hinzufügen können, wenn angeklickt, reagiert nicht mehr alle von den Lieferanten, müssen Sie zuerst die DAL und BLL, die diese Aktion führt eine Methode hinzugefügt. Diese Methode wird insbesondere benannt werden `DiscontinueAllProductsForSupplier(supplierID)`. Wenn die FormView-Schaltfläche geklickt wird, müssen wir diese Methode in der Geschäftslogikebene übergeben aufzurufen, in des ausgewählten Lieferanten `SupplierID`; Ruft die BLL dann nach unten an den entsprechenden Datenzugriffsebene-Methode, die ausstellen, die eine `UPDATE` Anweisung mit der Datenbank, die die angegebenen Lieferanten reagiert nicht mehr.

Wie wir in unserer vorherigen Lernprogrammen durchgeführt haben, verwenden einen Bottom-Up-Ansatz wir die DAL-Methode, die BLL-Methode, erstellen und schließlich auf der ASP.NET-Seite Implementieren der Funktionalität ab. Öffnen der `Northwind.xsd` typisierte DataSet in die `App_Code/DAL` Ordner und eine neue Methode zum Hinzufügen der `ProductsTableAdapter` (mit der rechten Maustaste auf die `ProductsTableAdapter` , und wählen Sie die Abfrage hinzufügen). Auf diese Weise wird der TableAdapter-Abfrage-Konfigurations-Assistenten anzuzeigen, in dem uns durch den Prozess des Hinzufügens der neuen Methode führt. Angabe, dass unsere DAL-Methode wird eine Ad-hoc-SQL-Anweisung beginnen.


[![Erstellen Sie die DAL-Methode, die mit Ad-hoc-SQL-Anweisungen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Abbildung 12**: Exemplarische Vorgehensweise: Erstellen von der DAL-Methode mit einer Ad-hoc-SQL-Anweisung ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Als Nächstes fordert der Assistent uns um welche Art von Abfrage zu erstellen. Da die `DiscontinueAllProductsForSupplier(supplierID)` Methode müssen Aktualisieren der `Products` Datenbanktabelle Festlegen der `Discontinued` 1 für alle Produkte bereitgestellt, die durch das angegebene Feld *`supplierID`*, müssen wir eine Abfrage erstellen, die Daten aktualisiert.


[![Wählen Sie den Abfragetyp UPDATE](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Abbildung 13**: Wählen Sie den Abfragetyp UPDATE ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


Im nächste Assistentenbildschirm bietet TableAdapter der vorhandenen `UPDATE` -Anweisung, die jedes der Felder im definierten aktualisiert die `Products` DataTable. Ersetzen Sie diese Abfragetext durch die folgende Anweisung aus:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Nach der Eingabe dieser Abfrage, und klicken Sie auf Weiter, fordert der letzten Seite des Assistenten für die neue Methode Namen `DiscontinueAllProductsForSupplier`. Schließen Sie den Assistenten, indem Sie auf "Fertig stellen". Nach der Rückgabe an den DataSet-Designer sehen Sie eine neue Methode in der `ProductsTableAdapter` mit dem Namen `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Name der neuen DiscontinueAllProductsForSupplier der DAL-Methode](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Abbildung 14**: Benennen Sie die neue DAL `DiscontinueAllProductsForSupplier` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Mit der `DiscontinueAllProductsForSupplier(supplierID)` erstellte Methode in der Datenzugriffsebene, unsere nächste Aufgabe ist die Erstellung der `DiscontinueAllProductsForSupplier(supplierID)` Methode in der Geschäftslogikebene. Um dies zu erreichen, öffnen Sie die `ProductsBLL` Klassendatei, und fügen Sie Folgendes hinzu:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Diese Methode ruft einfach nach unten, um die `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der DAL, weiter und übergibt dabei die angegebenen *`supplierID`* Parameterwert. Wären alle Geschäftsregeln, die nur einen Lieferanten unter bestimmten Umständen nicht fortgeführt werden dürfen, sollten diese Regeln in der BLL hier implementiert werden.

> [!NOTE]
> Im Gegensatz zu den `UpdateProduct` -Überladungen in der `ProductsBLL` -Klasse, die `DiscontinueAllProductsForSupplier(supplierID)` Methodensignatur enthält keine der `DataObjectMethodAttribute` Attribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Dies schließt die `DiscontinueAllProductsForSupplier(supplierID)` Methode aus der Dropdownliste das ObjectDataSource Konfigurieren von Datenquellen-Assistenten auf der Registerkarte "UPDATE". Ich Ve dieses Attribut weggelassen, da wir aufrufen, müssen die `DiscontinueAllProductsForSupplier(supplierID)` Methode direkt von einem Ereignishandler auf unserem ASP.NET-Seite.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Schritt 5: Hinzufügen einer unterbrechen Sie alle Produkte-Schaltfläche, um die FormView

Mit der `DiscontinueAllProductsForSupplier(supplierID)` Methode in der BLL und DAL abgeschlossen, der abschließenden Schritt zum Hinzufügen der Funktion, die alle Produkte für den ausgewählten Lieferanten zu beenden wird die FormView ein Websteuerelement Schaltfläche hinzugefügt `ItemTemplate`. Fügen wir eine solche Schaltfläche unten Telefonnummer mit der Schaltflächentext All Products Einstellen des Lieferanten hinzu und ein `ID` Eigenschaftswert `DiscontinueAllProductsForSupplier`. Sie können dieses Steuerelement Schaltfläche mithilfe des Designers durch Klicken auf den Link Bearbeiten von Vorlagen in der FormView-Smarttag hinzufügen (siehe Abbildung 15), oder direkt über die deklarative Syntax.


[![Hinzufügen einer alle Produkte Schaltfläche Websteuerelement auf FormView s ItemTemplate einstellen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Abbildung 15**: Hinzufügen ein Beenden aller Produkte Schaltfläche Websteuerelement die FormView `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Wenn die Schaltfläche geklickt wird, durch die ein Benutzer auf die Seite ein Postback erfolgt und die FormView [ `ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) ausgelöst wird. Zum Ausführen von benutzerdefinierten Codes als Reaktion auf diese Schaltfläche geklickt wird, können wir einen Ereignishandler für dieses Ereignis erstellen. Verstehen, aber, die `ItemCommand` Ereignis wird ausgelöst, wenn *alle* innerhalb der FormView Schaltfläche, LinkButton oder ImageButton-Steuerelement geklickt wird. Dies bedeutet, dass, wenn der Benutzer von einer Seite in eine andere in der FormView richtet die `ItemCommand` Ereignisses; identisch, wenn der Benutzer klickt auf New, Edit, oder Löschen von Daten in einem FormView, das Einfügen, aktualisieren oder Löschen von unterstützt.

Da die `ItemCommand` ausgelöst wird, unabhängig davon, welche Schaltfläche klicken, wird im Ereignisprotokoll Handler wir benötigen eine Möglichkeit, um festzustellen, ob die beenden Sie alle Produkte Schaltfläche geklickt wurde oder wenn es sich um eine Schaltfläche "Sonstige" war. Um dies zu erreichen, legen wir den Schaltfläche Websteuerelement `CommandName` Eigenschaft auf einen beliebigen Wert identifiziert. Wenn die Schaltfläche geklickt wird, dies `CommandName` übergebene Wert den `ItemCommand` -Ereignishandler, aktivieren uns, um festzustellen, ob die beenden Sie die Schaltfläche "alle Produkte" auf die Schaltfläche geklickt wurde. Beenden Sie alle Produkte Schaltfläche festlegen des `CommandName` DiscontinueProducts Eigenschaft.

Schließlich ermöglicht die Verwendung eines clientseitigen bestätigen (Dialogfeld das, stellen Sie sicher, dass der Benutzer möchte die ausgewählten Lieferanten eingestellt. Wie wir gesehen, in haben der [Hinzufügen von clientseitigen Bestätigung beim Löschen von](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) Tutorial, dies kann mit einem gewissen Grad JavaScript erreicht werden. Insbesondere auf die Schaltfläche Websteuerelement OnClientClick-Eigenschaft festgelegt `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Nachdem diese Änderungen vorgenommen wurden, sollte die FormView deklarative Syntax wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für die FormView `ItemCommand` Ereignis. In diesem Ereignishandler muss zuerst feststellen, ob die beenden Sie alle Produkte Schaltfläche geklickt wurde. Wenn daher wir erstellen eine Instanz von möchten der `ProductsBLL` Klasse, und rufen die `DiscontinueAllProductsForSupplier(supplierID)` -Methode auf und übergibt die `SupplierID` der ausgewählten FormView:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Beachten Sie, dass die `SupplierID` des aktuellen ausgewählten Lieferanten in FormView möglich, die mithilfe der FormView [ `SelectedValue` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). Die `SelectedValue` Eigenschaft gibt die ersten Daten-Schlüsselwert für den Datensatz in die FormView angezeigt wird. Die FormView [ `DataKeyNames` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), die Daten, die Felder aus dem die Daten die Schlüsselwerte in Raumfahrt aus, womit auf automatisch festgelegt wurde `SupplierID` von Visual Studio beim Binden der ObjectDataSource FormView in Schritt2.

Mit der `ItemCommand` -Ereignishandler erstellt haben, nehmen einen Moment Zeit, zu der Seite zu testen. Navigieren Sie zu der Cooperativa de Quesos "Las Cabras" Lieferanten (er befindet sich die fünfte Lieferanten in der FormView für mich). Diese Lieferanten bietet zwei Produkte, Queso Cabrales und Queso Manchego La Pastora beide *nicht* nicht mehr unterstützt.

Stellen Sie sich vor Cooperativa de Quesos "Las Cabras" nicht mehr im Unternehmen, seine Produkte sind daher nicht mehr unterstützt. Klicken Sie auf die Schaltfläche "alle Produkte" eingestellt. Dadurch wird das Dialogfeld "clientseitige bestätigen" angezeigt (siehe Abbildung 16).


[![Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Abbildung 16**: Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Wenn Sie im Dialogfeld die clientseitige bestätigen klicken, der Übermittlung des Formulars wird fortgesetzt, einen Postback verursacht, in dem die FormView `ItemCommand` Ereignis wird ausgelöst. Der Ereignishandler, die es erstellt wird dann ausgeführt, Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` -Methode und die Queso Cabrales Queso Manchego La Pastora Produkte und beenden.

Wenn Sie die GridView Ansichtszustand deaktiviert haben, wird die GridView ist mit dem zugrunde liegenden Datenspeicher auf jedes Postback erneut gebunden wird und aus diesem Grund wird sofort aktualisiert, um widerzuspiegeln, dass diese beiden Produkte eingestellt werden (siehe Abbildung 17). Wenn jedoch Sie nicht in die GridView Ansichtszustand deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nach dieser Änderung. Um dies zu erreichen, stellen Sie einfach einen Aufruf an der GridView `DataBind()` Methode sofort nach dem Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` Methode.


[![Nach dem Klicken auf das Beenden Sie die Schaltfläche "alle Produkte", werden die Lieferanten s entsprechend aktualisiert](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Abbildung 17**: nach dem Klicken auf das Beenden Sie die Schaltfläche "alle Produkte" aus, den Lieferanten Produkte werden entsprechend aktualisiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Schritt 6: Erstellen eine Überladung UpdateProduct in der Geschäftslogikschicht zum Anpassen der Preis eines Produkts

Wie müssen mit den beenden Sie die Schaltfläche "Alles Produkte" in der FormView zum Hinzufügen von Schaltflächen zum Erhöhen oder Verringern des Preis für ein Produkt in der GridView wir zunächst die entsprechenden Methoden der Datenzugriffsschicht und Business Logic Layer hinzufügen. Da wir bereits über eine Methode, die eine einzelnes Produktzeile in der DAL aktualisiert verfügen, können wir solche Funktionen bereitstellen, indem Sie erstellen eine neue Überladung für die `UpdateProduct` -Methode in der BLL.

Unsere Vergangenheit `UpdateProduct` Überladungen in eine Kombination von Produkt Felder als skalaren Eingabewerten ausgeführt haben und dann nur die Felder für das angegebene Produkt aktualisiert. Für diese Überladung wir weichen geringfügig von dieser Standard und stattdessen übergeben Sie des Produkts `ProductID` sowie den Prozentsatz nach dem Anpassen der `UnitPrice` (im Gegensatz zu übergeben, in der neuen, angepasst `UnitPrice` selbst). Dieser Ansatz wird vereinfacht den Code, in der Klasse ASP.NET Code-Behind-Seite zu schreiben, da wir nicht, um zu bestimmen, des aktuellen Produkts kümmern müssen `UnitPrice`.

Die `UpdateProduct` überladen für dieses Lernprogramm unten gezeigt wird:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Diese Überladung ruft Informationen über das angegebene Produkt über der DAL `GetProductByProductID(productID)` Methode. Es wird dann geprüft, ob des Produkts `UnitPrice` hat eine Datenbank `NULL` Wert. Wenn dies der Fall, kann der Preis bleibt unverändert. Wenn allerdings es keine ist`NULL` `UnitPrice` Wert, der Methode aktualisiert des Produkts `UnitPrice` durch den angegebenen Prozentsatz (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Schritt 7: Hinzufügen der erhöhen und die Verringerung der Schaltflächen an die GridView

Die GridView (und DetailsView) sowohl eine Auflistung von Feldern bestehen aus. Zusätzlich zur BoundFields, CheckBoxFields, und von TemplateFields schließt ASP.NET den ButtonField, die als eine Spalte mit einer Schaltfläche, LinkButton oder ImageButton für jede Zeile wie der Name schon sagt, rendert. Ähnlich wie die FormView auf *alle* innerhalb der GridView Pagingschaltflächen, bearbeiten oder löschen Schaltflächen Sortieren Schaltflächen und usw. einen Postback verursacht, und löst der GridView [ `RowCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Die ButtonField verfügt über eine `CommandName` -Eigenschaft, die auf jedem der zugehörigen Schaltflächen den angegebenen Wert weist `CommandName` Eigenschaften. Mit FormView, wie die `CommandName` Wert wird verwendet, durch die `RowCommand` -Ereignishandler, um zu bestimmen, welches Steuerelement die Schaltfläche geklickt wurde.

Fügen Sie zwei neue ButtonFields an die GridView, eine mit einem Schaltflächentext Preis + 10 % und der andere mit dem Text Preis-10 %. Um diese ButtonFields hinzuzufügen, klicken Sie auf den Link Bearbeiten von Spalten aus der GridView-Smarttag, wählen Sie den Feldtyp ButtonField aus der Liste in der oberen linken Ecke, und klicken Sie auf die Schaltfläche "hinzufügen".


![Hinzufügen von zwei ButtonFields an die GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Abbildung 18**: Hinzufügen von zwei ButtonFields an die GridView


Verschieben Sie zwei ButtonFields, so dass sie als die ersten beiden GridView-Felder angezeigt werden. Legen Sie anschließend die `Text` Eigenschaften von diesen beiden ButtonFields zum Preis + 10 % und Preis-10 % und die `CommandName` IncreasePrice und DecreasePrice, Eigenschaften bzw. Standardmäßig wird ein ButtonField die Spalte von Schaltflächen als LinkButtons gerendert. Dies kann geändert werden, jedoch über die ButtonField [ `ButtonType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Wir haben diese zwei ButtonFields als reguläre Schaltflächen gerendert. Legen Sie deshalb die `ButtonType` Eigenschaft `Button`. Abbildung 19 zeigt die Felder (Dialogfeld), nachdem diese Änderungen vorgenommen wurden; folgt, ist die GridView deklarativem Markup.


![Konfigurieren der ButtonFields Text, CommandName und ButtonType-Eigenschaften](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Abbildung 19**: Konfigurieren der ButtonFields `Text`, `CommandName`, und `ButtonType` Eigenschaften


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Mit diesen ButtonFields erstellt wird, ist der letzte Schritt erstellen Sie einen Ereignishandler für der GridView `RowCommand` Ereignis. Dieser Ereignishandler ausgelöst, da der Preis + 10 % oder %-Schaltflächen Kurs-10 wurden geklickt haben, muss, um zu bestimmen, die `ProductID` für die Zeile, deren Schaltfläche geklickt wurde, und rufen Sie dann, die `ProductsBLL` -Klasse `UpdateProduct` -Methode auf und übergibt den entsprechenden `UnitPrice` Prozentuale Anpassung zusammen mit den `ProductID`. Der folgende Code führt die folgenden Aufgaben:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Um zu bestimmen, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis-10 % Schaltfläche geklickt wurde, müssen wir der GridView zurate ziehen, um `DataKeys` Auflistung. Diese Auflistung enthält die Werte der Felder im angegebenen die `DataKeyNames` Eigenschaft für jede Zeile GridView. Seit der GridView `DataKeyNames` -Eigenschaft wurde auf "ProductID" von Visual Studio festgelegt, wenn das ObjectDataSource an die GridView gebunden `DataKeys(rowIndex).Value` bietet die `ProductID` für den angegebenen *RowIndex*.

Die ButtonField werden automatisch in die *RowIndex* der Zeile, deren geklickt wurde durch, die `e.CommandArgument` Parameter. Aus diesem Grund, um zu bestimmen, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis-10 % Schaltfläche geklickt wurde, verwenden wir: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Als mit der Schaltfläche "alle Produkte eingestellt" Wenn Sie die GridView Ansichtszustand deaktiviert haben die GridView ist mit dem zugrunde liegenden Datenspeicher auf jedes Postback erneut gebunden wird und daher sofort entsprechend auf wird eine Preisänderung aktualisiert einer der Schaltflächen. Wenn jedoch Sie nicht in die GridView Ansichtszustand deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nach dieser Änderung. Um dies zu erreichen, stellen Sie einfach einen Aufruf an der GridView `DataBind()` Methode sofort nach dem Aufrufen der `UpdateProduct` Methode.

Abbildung 20 zeigt die Seite beim Anzeigen der Produkte von OMA-Kelly Homestead bereitgestellt. Abbildung 21 zeigt die Ergebnisse nach dem Preis + 10 % für OMA Boysenberry verteilt und die Schaltfläche "Preis-10 %" zweimal einmal für Northwoods Cranberry Besonderheiten von geklickt wurde.


[![Die GridView enthält Preis + 10 % und Preis-10 % Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Abbildung 20**: der GridView umfasst Preis + 10 % und Preis-10 % Schaltflächen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Die Preise für das erste und dritte Produkt aktualisiert worden sind, über den Preis + 10 % und Preis-10 % Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Abbildung 21**: die Preise für das erste und dritte Produkt aktualisiert wurden über den Preis + 10 % und Preis-10 % Schaltflächen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> Die GridView (und DetailsView) können auch Schaltflächen, LinkButtons oder ImageButtons ihre von TemplateFields hinzugefügt haben. Wie Sie mit der BoundField diese Schaltflächen, die beim Klicken auf einen Postback nachdenken werden, durch das Auslösen der GridView `RowCommand` Ereignis. Beim Hinzufügen von Schaltflächen in einem TemplateField jedoch der Schaltfläche `CommandArgument` wird nicht automatisch festgelegt, der Index der Zeile wie bei Verwendung von ButtonFields ist. Wenn Sie den Zeilenindex der Schaltfläche zu bestimmen, die innerhalb von per Mausklick müssen die `RowCommand` Ereignishandler, d. h. Sie müssen manuell festlegen, der Schaltfläche `CommandArgument` Eigenschaft in seiner deklarativen Syntax innerhalb der TemplateField Verwendung eines Codes:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`


## <a name="summary"></a>Zusammenfassung

Die Steuerelemente GridView, DetailsView und FormView können Schaltflächen, LinkButtons oder ImageButtons enthalten. Solche Schaltflächen, die beim Klicken auf einen Postback verursacht und löst die `ItemCommand` Ereignis in die FormView und DetailsView-Steuerelemente und die `RowCommand` Ereignis in die GridView. Diese Web-Datensteuerelemente über integrierte Funktionalität behandelt allgemeine Befehle bezogenen Aktionen ausführen, z. B. löschen oder Bearbeiten von Datensätzen verfügen. Wir können jedoch auch mit Schaltflächen, die beim Klicken auf, antwortet mit eigenen benutzerdefinierten Code ausführen.

Um dies zu erreichen, müssen wir erstellen Sie einen Ereignishandler für das `ItemCommand` oder `RowCommand` Ereignis. In diesem Ereignishandler überprüfen wir zuerst die eingehende `CommandName` Wert zu bestimmen, welches Steuerelement die Schaltfläche geklickt wurde, und klicken Sie dann eine entsprechende Aktion. In diesem Lernprogramm haben wir gesehen, wie Schaltflächen und ButtonFields, Beenden alle Produkte für einen angegebenen Lieferanten oder zum Vergrößern oder Verkleinern des Preis für ein bestimmtes Produkt um 10 %.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](adding-and-responding-to-buttons-to-a-gridview-vb.md)
