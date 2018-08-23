---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Hinzufügen von und reagieren auf Schaltflächen zu einer GridView-Ansicht (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere werden wir Buildspeicherort...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a081b1633e7762560aea68500f5bd614e4fb5a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826904"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Hinzufügen von und reagieren auf Schaltflächen zu einer GridView-Ansicht (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) oder [PDF-Datei herunterladen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere erstellen wir eine Schnittstelle mit einem FormView-Steuerelement, das dem Benutzer ermöglicht, über die Lieferanten Seite.


## <a name="introduction"></a>Einführung

Während viele Berichterstellungsszenarien schreibgeschützten Zugriff auf die Berichtsdaten enthalten, ist es nicht ungewöhnlich, dass Berichte die Möglichkeit zum Ausführen von Aktionen auf Grundlage von angezeigten Daten enthalten. In der Regel dadurch beim Hinzufügen einer Schaltfläche, ImageButton Web oder LinkButton-Steuerelement mit jeder Datensatz im Bericht angezeigt, die beim Klicken auf ein Postback auslöst und serverseitigen Code aufruft. Bearbeiten und Löschen von Daten auf Basis von Datensätzen ist das häufigste Beispiel. In der Tat wie wir gesehen haben, beginnend mit der [Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Tutorial, bearbeiten und Löschen von ist so gängig, dass solche Funktionen ohne die GridView, DetailsView oder FormView-Steuerelemente unterstützt die für eine einzige Codezeile schreiben müssen.

Darüber hinaus bearbeiten und Löschen von Schaltflächen, die GridView, DetailsView und FormView Steuerelemente können auch enthalten Schaltflächen, LinkButtons oder ImageButtons, führen Sie durch Klicken auf eine benutzerdefinierte serverseitige Logik. In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere erstellen wir eine Schnittstelle mit einem FormView-Steuerelement, das dem Benutzer ermöglicht, über die Lieferanten Seite. Für einen bestimmten Lieferanten wird die FormView Informationen zu den Lieferanten zusammen mit einer Schaltfläche-Websteuerelement angezeigt, die, wenn geklickt haben, alle ihre zugeordneten Produkte werden als markieren nicht mehr unterstützt. Darüber hinaus enthält einer GridView-Ansicht dieser Produkte, die von den ausgewählten Lieferanten bereitgestellt werden, in dem jede Zeile mit Preis erhöhen und Discount Preis Schaltflächen, die, wenn geklickt haben, erhöhen oder des Produkts reduzieren `UnitPrice` um 10 % (siehe Abbildung 1).


[![FormView und GridView enthalten Sie Schaltflächen, die benutzerdefinierte Aktionen ausführen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Abbildung 1**: beide FormView und GridView enthalten Schaltflächen, führen Sie benutzerdefinierte Aktionen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten der Schaltfläche-Tutorial

Bevor wir uns, wie befassen Sie eine benutzerdefinierten Schaltflächen hinzufügen, sehen wir uns einen Moment Zeit, um die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Tutorial benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `CustomButtons`. Fügen Sie die folgenden beiden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `CustomButtons.aspx`


![Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `CustomButtons` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf der Seite Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Abschließend fügen Sie die Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Paginierung und Sortierung `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Abbildung 4**: die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Schritt 2: Hinzufügen von einem FormView-Steuerelement, in der die Lieferanten aufgeführt.

Erste Schritte in diesem Tutorial durch Hinzufügen von FormView, der die Lieferanten auflistet. Wie in der Einleitung erwähnt, können diese FormView-Steuerelement den Benutzer auf der Seite über die Lieferanten, Produkte von Lieferanten in einer GridView-Ansicht angezeigt. Darüber hinaus enthält diese FormView-Steuerelement eine Schaltfläche, die beim Klicken auf alle den Namen des Lieferanten als markiert nicht mehr unterstützt. Bevor wir uns darum kümmern, die benutzerdefinierte Schaltfläche zu FormView hinzufügen, lassen Sie uns zuerst erstellen Sie einfach FormView, dass die Lieferanteninformationen angezeigt werden.

Öffnen Sie zunächst die `CustomButtons.aspx` auf der Seite die `CustomButtons` Ordner. Fügen Sie einem FormView-Steuerelement auf der Seite durch Ziehen aus der Toolbox in den Designer und den Satz der `ID` Eigenschaft `Suppliers`. Aus der FormView Smarttag, deaktivieren Sie zum Erstellen einer neuen, mit dem Namen "ObjectDataSource" `SuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource "ObjectDataSource"](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Diese neue "ObjectDataSource" So konfigurieren, dass abgefragt, die von der `SuppliersBLL` Klasse `GetSuppliers()` Methode (siehe Abbildung 6). Da diese FormView keine Schnittstelle bietet für die Aktualisierung der Lieferanteninformationen auswählen, dass die (None) aus der Dropdown-Liste auf der Registerkarte "UPDATE"-option.


[![Konfigurieren der Datenquelle zur Verwendung der Klasse SuppliersBLL s GetSuppliers()-Methode](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Abbildung 6**: Konfigurieren der Datenquelle verwendet die `SuppliersBLL` Klasse `GetSuppliers()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Nach der Konfiguration dem ObjectDataSource-Steuerelement, generiert Visual Studio ein `InsertItemTemplate`, `EditItemTemplate`, und `ItemTemplate` für das FormView-Steuerelement. Entfernen Sie die `InsertItemTemplate` und `EditItemTemplate` und Ändern der `ItemTemplate` , damit nur die Lieferanten Unternehmen Namen und die Telefonnummer Nummer angezeigt. Pagingunterstützung für das FormView schließlich aktivieren, indem Sie das Kontrollkästchen für die Auslagerungsdatei Aktivieren von smart Tag (oder durch Festlegen seiner `AllowPaging` Eigenschaft `True`). Nach diesen Änderungen sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Abbildung 7 zeigt die Seite CustomButtons.aspx, wenn Sie über einen Browser angezeigt.


[![Die FormView aufgeführt, die CompanyName und Phone-Felder des aktuell ausgewählten Lieferanten](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Abbildung 7**: das FormView-Listet die `CompanyName` und `Phone` Felder aus den derzeit ausgewählten Lieferanten ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Schritt 3: Hinzufügen einer GridView-Ansicht, die den ausgewählten Lieferanten Produkte aufgeführt sind

Bevor wir die Beenden alle Produkte-Schaltfläche "hinzufügen" der FormView Vorlage, lassen Sie uns zuerst eine GridView unter das FormView-Steuerelement hinzufügen, die Produkte von den ausgewählten Lieferanten aufgeführt. Legen Sie zu diesem Zweck werden Hinzufügen einer GridView-Ansicht auf der Seite, die `ID` Eigenschaft `SuppliersProducts`, und fügen Sie eine neue, mit dem Namen "ObjectDataSource" `SuppliersProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersProductsDataSource "ObjectDataSource"](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Abbildung 8**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Konfigurieren Sie diese "ObjectDataSource" zur Verwendung der ProductsBLL-Klasse `GetProductsBySupplierID(supplierID)` Methode (siehe Abbildung 9). Während dieser GridView werden für den Preis eines Produkts angepasst werden kann, verwenden sie nicht die integrierte bearbeiten oder Löschen von Funktionen aus der GridView. Aus diesem Grund können wir die Dropdown-Liste (keine) für dem ObjectDataSource-Steuerelement der Update-, INSERT- und Löschen von Registerkarten festlegen.


[![Konfigurieren der Datenquelle zur Verwendung der Klasse ProductsBLL s GetProductsBySupplierID(supplierID)-Methode](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Abbildung 9**: Konfigurieren der Datenquelle verwendet die `ProductsBLL` Klasse `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Da die `GetProductsBySupplierID(supplierID)` -Methode akzeptiert einen Eingabeparameter, der ObjectDataSource-Steuerelement-Assistent fordert uns für die Quelle der Wert dieses Parameters. Übergeben der `SupplierID` FormView Wert, der Parameter-Quellliste Dropdown-Steuerelement und die ControlID Dropdown-Liste festgelegt `Suppliers` (die ID des FormView, die in Schritt2 erstellt wird).


[![Anzugeben Sie, dass die Lieferanten FormView-Steuerelement durch die SupplierID Parameter stammen sollen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Abbildung 10**: anzugeben, die die *`supplierID`* Parameter stammen soll die `Suppliers` FormView-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Nach Abschluss des Assistenten "ObjectDataSource" wird die GridView eine BoundField- oder die CheckBoxField für die einzelnen Datenfelder des Produkts enthalten. Wir reduzieren diese anzuzeigenden lediglich die `ProductName` und `UnitPrice` BoundFields zusammen mit der `Discontinued` CheckBoxField; lassen Sie uns darüber hinaus zu formatieren der `UnitPrice` BoundField so, dass der Text als Währung formatiert ist. Ihre GridView und `SuppliersProductsDataSource` deklarative Markup des ObjectDataSource-Steuerelements sieht ähnlich wie das folgende Markup:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

An diesem Punkt zeigt unser Tutorial ein Master-/Detail-Berichts, gestattet es dem Benutzer ein Lieferant FormView oben ausgewählt und Produkte von Lieferanten über GridView im unteren Bereich anzeigen. Abbildung 11 zeigt einen Screenshot der Seite, bei der Auswahl der Lieferant Tokyo Traders aus der FormView-Steuerelement.


[![Die ausgewählten Lieferanten-s-Produkte werden in den GridView-Ansicht angezeigt.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Abbildung 11**: die ausgewählte des Lieferanten werden angezeigt, in den GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Schritt 4: Erstellen von DAL und BLL-Methoden, um alle Produkte für einen Lieferanten beenden

Bevor wir das FormView-Steuerelement eine Schaltfläche hinzufügen können, die beim Klicken auf beendet alle Produkte an den Lieferanten, müssen wir zuerst auf das Hinzufügen einer Methode für die DAL und BLL, der diese Aktion ausführt. Insbesondere diese Methode erhält `DiscontinueAllProductsForSupplier(supplierID)`. Wenn das FormView Schaltfläche geklickt wird, werden wir diese Methode in der Geschäftslogikebene übergeben aufrufen, in der ausgewählten Lieferanten `SupplierID`; die BLL aufgerufen wird, klicken Sie dann nach unten um die entsprechenden Daten von Zugriffsschicht-Methode, die Ausstellen einer `UPDATE` Anweisung in der Datenbank beendet, die die angegebenen des Lieferanten.

Wie wir in unserem vorherigen Tutorials abgeschlossen haben, verwenden wir einen Bottom-Up-Ansatz, erstellen die DAL-Methode, klicken Sie dann die BLL-Methode, und implementieren die Funktionalität zum Schluss auf der ASP.NET-Seite ab. Öffnen der `Northwind.xsd` typisierte DataSet in der `App_Code/DAL` Ordner und eine neue Methode zum Hinzufügen der `ProductsTableAdapter` (mit der rechten Maustaste auf die `ProductsTableAdapter` , und wählen Sie die Abfrage hinzufügen). Auf diese Weise wird der TableAdapter-Abfrage-Konfigurations-Assistent, anzuzeigen, in dem wir durch den Prozess des Hinzufügens der neuen Methode führt. Zunächst gibt an, dass die DAL-Methode eine Ad-hoc-SQL-Anweisung verwenden.


[![Erstellen Sie die DAL-Methode, die mit Ad-hoc-SQL-Anweisungen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Abbildung 12**: Exemplarische Vorgehensweise: Erstellen von der DAL-Methode mit einer Ad-hoc-SQL-Anweisung ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Als Nächstes fordert der Assistent, mit uns, welche Art von Abfrage zu erstellen. Da die `DiscontinueAllProductsForSupplier(supplierID)` Methode müssen Sie aktualisieren die `Products` Datenbanktabelle, Festlegen der `Discontinued` 1 für alle Produkte, die bereitgestellt werden, durch das angegebene Feld *`supplierID`*, müssen wir eine Abfrage erstellen, die Daten aktualisiert.


[![Wählen Sie den Typ der UPDATE-Abfrage](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Abbildung 13**: Wählen Sie den Typ der UPDATE-Abfrage ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


Im nächsten Assistentenbildschirm bietet der TableAdapter vorhandenen `UPDATE` -Anweisung, die jedes der Felder im definierten aktualisiert die `Products` DataTable. Ersetzen Sie den Text dieser Abfrage mit der folgenden Anweisung:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Nach der Eingabe dieser Abfrage ein, und klicken Sie auf Weiter, der letzte Assistentenbildschirm gefragt werden, für die Namen der neuen Methode `DiscontinueAllProductsForSupplier`. Schließen Sie den Assistenten, indem Sie auf die Schaltfläche "Fertig stellen". Bei der Rückkehr zum DataSet-Designer sollte eine neue Methode in der `ProductsTableAdapter` mit dem Namen `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Name der neuen DAL-Methode DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Abbildung 14**: Benennen Sie die neuen DAL `DiscontinueAllProductsForSupplier` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Mit der `DiscontinueAllProductsForSupplier(supplierID)` Methode, die in der Datenzugriffsebene erstellt werden, unsere nächste Aufgabe ist die Erstellung der `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der Geschäftslogikebene. Öffnen Sie dazu die `ProductsBLL` Klassendatei, und fügen Sie Folgendes:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Diese Methode ruft einfach nach unten, um die `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der die DAL aus, übergibt der bereitgestellten *`supplierID`* Parameterwert. Würden alle Geschäftsregeln, die nur zulässig, Produkten unter bestimmten Umständen eingestellt werden, sollten diese Regeln, in die BLL implementiert werden.

> [!NOTE]
> Im Gegensatz zu den `UpdateProduct` -Überladungen in die `ProductsBLL` -Klasse, die `DiscontinueAllProductsForSupplier(supplierID)` Methodensignatur enthält keinen der `DataObjectMethodAttribute` Attribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Dies verhindert, dass die `DiscontinueAllProductsForSupplier(supplierID)` Methode aus der Dropdownliste das "ObjectDataSource" Konfigurieren von Datenquellen-Assistenten auf der Registerkarte "UPDATE". Ich Ve dieses Attribut weggelassen, da wir aufrufen, werden die `DiscontinueAllProductsForSupplier(supplierID)` Methode direkt von einem Ereignishandler auf unsere ASP.NET-Seite.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Schritt 5: Hinzufügen einer Schaltfläche "alle Produkte", das FormView-Steuerelement stellt

Mit der `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der BLL- und DAL abgeschlossen, wird der letzte Schritt zum Hinzufügen von die Möglichkeit, alle Produkte für den ausgewählten Lieferanten stellt der FormView ein Websteuerelements mit Schaltfläche hinzugefügt `ItemTemplate`. Fügen Sie eine solche Schaltfläche unterhalb des Lieferanten-Telefonnummer mit der Text der Schaltfläche, beenden Sie alle Produkte und eine `ID` Eigenschaftswert `DiscontinueAllProductsForSupplier`. Sie können diese Schaltfläche Websteuerelements mit dem Designer hinzufügen, indem Sie auf den Link Vorlagen bearbeiten, in das FormView Smarttag (siehe Abbildung 15), oder direkt über die deklarative Syntax.


[![Hinzufügen einer beenden Sie alle Produkte Button-Web-Steuerelements, das FormView-s-ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Abbildung 15**: Hinzufügen ein Beenden aller Produkte Web Schaltflächensteuerelement, der FormView `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Wenn die Schaltfläche geklickt wird, indem Sie einen Benutzer auf ein Postback die Seite erfolgt und der FormView [ `ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) ausgelöst wird. Zum Ausführen von benutzerdefinierten Codes als Reaktion auf diese Schaltfläche geklickt wird, können wir einen Ereignishandler für dieses Ereignis erstellen. Zu verstehen, jedoch, die die `ItemCommand` Ereignis wird ausgelöst, wenn *alle* Schaltfläche, ImageButton Web oder LinkButton-Steuerelement innerhalb der FormView-Steuerelement geklickt wird. Dies bedeutet, dass, wenn der Benutzer von einer Seite zu einem anderen in der FormView-Steuerelement, das `ItemCommand` -Ereignis ausgelöst wird, dasselbe, wenn der Benutzer klickt auf die neu bearbeiten, oder löschen Sie in einem FormView-Steuerelement, das Einfügen, aktualisieren oder Löschen von unterstützt.

Da die `ItemCommand` ausgelöst wird, unabhängig davon, welche Schaltfläche klicken, wird im Ereignisprotokoll Handler benötigen wir eine Möglichkeit zu bestimmen, ob die Beenden alle Produkte Schaltfläche geklickt wurde, oder wenn es sich um eine andere Schaltfläche war. Um dies zu erreichen, können wir des Schaltfläche Websteuerelements festlegen `CommandName` Eigenschaft identifizierende Wert. Wenn die Schaltfläche geklickt wird, dies `CommandName` Wert wird übergeben, in der `ItemCommand` -Ereignishandler ermöglicht uns, zu bestimmen, ob die einstellen Schaltfläche "alle Produkte" die Schaltfläche geklickt wurde. Legen Sie die Beenden alle Produkte `CommandName` DiscontinueProducts Eigenschaft.

Abschließend verwenden Sie ein Dialogfeld für die clientseitige bestätigen wir sicherstellen, dass der Benutzer möchte die ausgewählten des Lieferanten zu beenden. Wie in beschrieben der [Hinzufügen der clientseitigen Bestätigung beim Löschen von](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) Tutorial, kann dies mit etwas JavaScript erreicht werden. Legen Sie "OnClientClick"-Eigenschaft der Schaltfläche "-Web-Steuerelements insbesondere auf `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Nach diesen Änderungen sollte das FormView deklarative Syntax wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für der FormView `ItemCommand` Ereignis. In diesem Ereignishandler müssen wir zunächst überprüfen, ob die Beenden alle Produkte Schaltfläche geklickt wurde. Wenn also eine Instanz von erstellt werden soll die `ProductsBLL` Klasse und rufen Sie die `DiscontinueAllProductsForSupplier(supplierID)` Methode und übergeben die `SupplierID` der ausgewählten FormView:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Beachten Sie, dass die `SupplierID` des aktuellen ausgewählten Lieferanten in der FormView-Steuerelement kann mit der FormView zugegriffen werden [ `SelectedValue` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). Die `SelectedValue` Eigenschaft zurückgibt, der erste Datenschlüsselwert für den Datensatz in der FormView-Steuerelement angezeigt wird. Die FormView [ `DataKeyNames` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), die Daten, die Felder, die von dem die Daten die Schlüsselwerte abgerufen werden, wodurch auf automatisch festgelegt wurde `SupplierID` von Visual Studio beim Binden von dem ObjectDataSource-Steuerelement an das FormView-Steuerelement in Schritt2.

Mit der `ItemCommand` -Ereignishandler erstellt haben, können Sie die Seite zu testen. Navigieren Sie zu der Cooperativa de Quesos "Las Cabras" Lieferanten (Dies ist die fünfte Lieferanten in der FormView-Steuerelement für mich). Dieser Lieferant bietet zwei Produkte, Queso Cabrales und Queso Manchego La Pastora, beide sind *nicht* nicht mehr unterstützt.

Stellen Sie sich vor, dass Cooperativa de Quesos 'Las Cabras' ist nicht mehr im Unternehmen und daher seine Produkte eingestellt werden. Klicken Sie auf die Schaltfläche "alle Produkte" einstellen. Dadurch wird das Dialogfeld "Client-Side bestätigen" angezeigt (siehe Abbildung 16).


[![Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Abbildung 16**: Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Wenn Sie auf "OK", klicken Sie im Dialogfeld die clientseitige bestätigen "klicken, die Übermittlung des Formulars wird fortgesetzt, einen Postback verursacht, in dem der FormView `ItemCommand` Ereignis ausgelöst wird. Der Ereignishandler, die wir erstellt haben wird ausgeführt, Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` -Methode und die Queso Cabrales Queso Manchego La Pastora Produkte und ein.

Wenn Sie den Status der GridView Ansicht deaktiviert haben, die GridView-Steuerelement ist in der zugrunde liegenden Datenspeicher bei jedem Postback erneut gebunden werden und aus diesem Grund wird sofort aktualisiert, um darauf hinzuweisen, dass diese beiden Produkte nicht mehr verwendet werden (siehe Abbildung 17). Wenn Sie jedoch, Sie nicht Ansichtszustand in den GridView-Ansicht deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nachdem diese Änderung vorgenommen. Um dies zu erreichen, stellen Sie einfach einen Aufruf von GridView `DataBind()` Methode sofort nach dem Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` Methode.


[![Sind nach der Schaltfläche Beenden alle Produkte, die Produkte von Lieferanten s entsprechend aktualisiert](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Abbildung 17**: nach der Schaltfläche Beenden alle Produkte, den Namen des Lieferanten werden entsprechend aktualisiert ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Schritt 6: Erstellen einer UpdateProduct-Überladung in der Geschäftslogikebene für die Anpassung von den Preis eines Produkts

Wie müssen mit "stellt alle Produkte" in das FormView-Steuerelement, zum Hinzufügen von Schaltflächen für die vergrößert und verkleinert den Preis für ein Produkt in der GridView wir zunächst die entsprechenden Daten von Zugriffsschicht und Business Logic Layer-Methoden hinzufügen. Da wir bereits über eine Methode, die eine einzelnes Produktzeile in die DAL aktualisiert verfügen, können wir solche Funktionen bereitstellen, indem Sie erstellen eine neue Überladung für die `UpdateProduct` -Methode in der die BLL.

Die Vergangenheit `UpdateProduct` Überladungen, die in einer Kombination von Produkt Felder als skalare Eingabewerte erstellt haben, und verfügen dann nur die Felder für das angegebene Produkt aktualisiert. Diese Überladung wird unterscheiden sich geringfügig von diesem Standard- und übergeben Sie stattdessen des produktanforderungen `ProductID` und der Prozentsatz, mit dem Anpassen der `UnitPrice` (im Gegensatz zu übergeben, in der neuen, angepasst `UnitPrice` selbst). Dieser Ansatz wird vereinfachen den Code, die wir benötigen, in der Klasse ASP.NET Code-Behind-Seite zu schreiben, da wir nicht zu kümmern, Bestimmen des aktuellen Produkts `UnitPrice`.

Die `UpdateProduct` überladen für unten in diesem Tutorial gezeigt wird:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Diese Überladung ruft Informationen über das angegebene Produkt über die DAL `GetProductByProductID(productID)` Methode. Es wird dann geprüft, ob des Produkts `UnitPrice` erhält eine Datenbank `NULL` Wert. Wenn es sich handelt, kann der Preis bleibt unverändert. Wenn Sie allerdings es ist keine`NULL` `UnitPrice` Wert, der die Methode updates des Produkts `UnitPrice` durch den angegebenen Prozentsatz (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Schritt 7: Hinzufügen der Erhöhung und Verringerung der Schaltflächen an die GridView

Das GridView (und DetailsView) sowohl eine Auflistung von Feldern bestehen. Neben dem BoundFields, CheckBoxFields und von TemplateFields bietet ASP.NET die ButtonField, die, wie der Name schon sagt, wie eine Spalte mit einer Schaltfläche, LinkButton oder ImageButton für jede Zeile gerendert wird. Ähnlich wie das FormView-Steuerelement, auf *alle* innerhalb der GridView Pagingschaltflächen, bearbeiten oder löschen Schaltflächen, Sortierung Schaltflächen und So weiter ein Postback auslöst, und löst des GridView [ `RowCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Die ButtonField verfügt über eine `CommandName` -Eigenschaft, die den angegebenen Wert zuweist, der die Schaltflächen `CommandName` Eigenschaften. Wie das FormView-Steuerelement, beispielsweise die `CommandName` Wert wird verwendet, durch die `RowCommand` -Ereignishandler, um zu bestimmen, welche Schaltfläche geklickt wurde.

Fügen Sie zwei neue ButtonFields an die GridView, eine mit einer Schaltflächentext Preis + 10 % und der andere mit dem Text Preis – 10 %. Um diese ButtonFields hinzuzufügen, klicken Sie auf den Link "Spalten bearbeiten" aus den GridView Smarttag, wählen Sie den Feldtyp ButtonField aus der Liste in der oberen linken Ecke, und klicken Sie auf die Schaltfläche "hinzufügen".


![Hinzufügen von zwei ButtonFields an die GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Abbildung 18**: Hinzufügen von zwei ButtonFields an die GridView


Verschieben Sie die zwei ButtonFields, sodass sie als die ersten beiden GridView-Felder angezeigt werden. Legen Sie als Nächstes die `Text` Eigenschaften von diesen beiden ButtonFields zum Preis + 10 % und Preis – 10 % und die `CommandName` Eigenschaften IncreasePrice und DecreasePrice, bzw. Standardmäßig rendert eine ButtonField die Spalte mit Schaltflächen als LinkButtons. Dies kann geändert werden, jedoch über die ButtonField des [ `ButtonType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Lassen Sie uns diese zwei ButtonFields als reguläre Schaltflächen gerendert; Legen Sie daher die `ButtonType` Eigenschaft `Button`. Abbildung 19 zeigt die Felder (Dialogfeld), nachdem diese Änderungen vorgenommen wurden, folgt den GridView deklaratives Markup.


![Konfigurieren Sie die ButtonFields Text, CommandName und ButtonType-Eigenschaften](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Abbildung 19**: Konfigurieren der ButtonFields `Text`, `CommandName`, und `ButtonType` Eigenschaften


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Mit diesen ButtonFields erstellt, der letzte Schritt ist die Erstellung einen Ereignishandler für des GridView `RowCommand` Ereignis. Dieser Ereignishandler, wenn ausgelöst, da der Preis + 10 % oder Preis – 10 % Schaltflächen wurden geklickt haben, Anforderungen, um zu bestimmen, die `ProductID` für die Zeile, deren Schaltfläche geklickt wurde, und rufen Sie dann, die `ProductsBLL` Klasse `UpdateProduct` Methode auf und übergibt in den entsprechenden `UnitPrice` Prozentuale Anpassung zusammen mit den `ProductID`. Der folgende Code führt folgende Aufgaben:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Um zu ermitteln, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis – 10 %-Schaltfläche geklickt wurde, müssen wir des GridView finden Sie in `DataKeys` Auflistung. Diese Auflistung enthält die Werte der Felder im angegebenen die `DataKeyNames` -Eigenschaft für jede GridView-Zeile. Seit des GridView `DataKeyNames` -Eigenschaft wurde auf "ProductID" von Visual Studio festgelegt, wenn es sich bei dem ObjectDataSource-Steuerelement an die GridView zu binden `DataKeys(rowIndex).Value` bietet die `ProductID` für den angegebenen *RowIndex*.

Die ButtonField werden automatisch in die *RowIndex* der Zeile, deren Schaltfläche, über geklickt wurde, die `e.CommandArgument` Parameter. Aus diesem Grund, um zu bestimmen, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis – 10 %-Schaltfläche geklickt wurde, verwenden wir: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Als mit der alle Produkte eingestellt, wenn Sie den GridView Ansichtstatus deaktiviert haben die GridView-Steuerelement ist in der zugrunde liegenden Datenspeicher bei jedem Postback erneut gebunden werden und aus diesem Grund wird sofort aktualisiert, um eine Preisänderung widerspiegeln, die zwischen dem Klicken auf tritt auf einer der Schaltflächen. Wenn Sie jedoch, Sie nicht Ansichtszustand in den GridView-Ansicht deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nachdem diese Änderung vorgenommen. Um dies zu erreichen, stellen Sie einfach einen Aufruf von GridView `DataBind()` Methode sofort nach dem Aufrufen der `UpdateProduct` Methode.

Abbildung 20 zeigt die Seite, wenn Sie Produkte von den OMA-Kelly Homestead anzeigen. Abbildung 21 zeigt die Ergebnisse nach dem Preis + 10, dass % für Großmutters Boysenberry verteilt und die Schaltfläche "Preis – 10 %" zweimal einmal für Northwoods Cranberry Sauce geklickt wurde.


[![GridView enthält Preis + 10 % und Preis – 10 %-Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Abbildung 20**: der GridView-Includes-Preis + 10 Preis – 10 % Schaltflächen "und" % ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Die Preise für das erste und dritte Produkt wurden aktualisiert, über den Preis + 10 % und Preis – 10 %-Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Abbildung 21**: die Preise für das erste und dritte Produkt wurden aktualisiert, über den Preis + 10 % und Preis – 10 % Schaltflächen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> Das GridView (und DetailsView) können auch Schaltflächen, LinkButtons oder ImageButtons ihre von TemplateFields hinzugefügt haben. Wie Sie mit der BoundField diese Schaltflächen, die beim Klicken auf einen Postback ausgelöst werden, Auslösen von GridView `RowCommand` Ereignis. Beim Hinzufügen von Schaltflächen in ein TemplateField, aber der Schaltfläche `CommandArgument` wird nicht automatisch festgelegt, der Index der Zeile wie bei ButtonFields verwenden. Wenn müssen Sie die Schaltfläche mit den Index der Zeile zu bestimmen, die in auf die geklickt wurde die `RowCommand` -Ereignishandler müssen Sie manuell auf der Schaltfläche festlegen `CommandArgument` -Eigenschaft in seiner deklarativen Syntax in das TemplateField, mit Code wie:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`


## <a name="summary"></a>Zusammenfassung

Die Steuerelemente GridView, DetailsView oder FormView können Schaltflächen, LinkButtons oder ImageButtons enthalten. Diese Schaltflächen, die beim Klicken auf einen Postback verursachen und Auslösen der `ItemCommand` Ereignis in die FormView und DetailsView-Steuerelemente und die `RowCommand` Ereignis in den GridView-Ansicht. Diese datenwebsteuerelemente haben integrierte Funktionen, die häufig verwendete Befehle bezogenen Aktionen, z. B. das Löschen oder Bearbeiten von Datensätzen zu behandeln. Wir können jedoch auch verwenden, Schaltflächen, die beim Klicken auf, reagieren Sie mit unserer eigenen benutzerdefinierten Code ausführen.

Zu diesem Zweck müssen wir erstellen einen Ereignishandler für die `ItemCommand` oder `RowCommand` Ereignis. In diesem Ereignishandler überprüfen wir zunächst den eingehenden `CommandName` Wert zu bestimmen, welche Schaltfläche geklickt wurde, und klicken Sie dann eine entsprechende Aktion. In diesem Tutorial wurde erläutert, wie Schaltflächen und ButtonFields verwenden, um alle Produkte für einen angegebenen Lieferanten zu beenden oder zu erhöhen oder verringern den Preis für ein bestimmtes Produkt um 10 %.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](adding-and-responding-to-buttons-to-a-gridview-vb.md)
