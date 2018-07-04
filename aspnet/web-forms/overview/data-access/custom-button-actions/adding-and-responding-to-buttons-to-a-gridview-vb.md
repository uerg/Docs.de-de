---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Hinzufügen von und reagieren auf Schaltflächen zu einer GridView-Ansicht (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere werden wir Buildspeicherort...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 458f90bf70d6f10402583623ef62bf1572040ce4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365057"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Hinzufügen von und reagieren auf Schaltflächen zu einer GridView-Ansicht (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) oder [PDF-Datei herunterladen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere erstellen wir eine Schnittstelle mit einem FormView-Steuerelement, das dem Benutzer ermöglicht, über die Lieferanten Seite.


## <a name="introduction"></a>Einführung

Während viele Berichterstellungsszenarien schreibgeschützten Zugriff auf die Berichtsdaten umfassen es nicht ungewöhnlich, dass Berichte enthalten die Möglichkeit zum Ausführen von Aktionen basierend auf den Daten angezeigt. In der Regel dadurch beim Hinzufügen einer Schaltfläche, ImageButton Web oder LinkButton-Steuerelement mit jeder Datensatz im Bericht angezeigt, die beim Klicken auf ein Postback auslöst und serverseitigen Code aufruft. Bearbeiten und Löschen von Daten auf Basis von Datensätzen ist das häufigste Beispiel. In der Tat wie wir gesehen haben, beginnend mit der [Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial, bearbeiten und Löschen von ist so gängig, dass solche Funktionen ohne die GridView, DetailsView oder FormView-Steuerelemente unterstützt die für eine einzige Codezeile schreiben müssen.

Darüber hinaus bearbeiten und Löschen von Schaltflächen, die GridView, DetailsView und FormView Steuerelemente können auch enthalten Schaltflächen, LinkButtons oder ImageButtons, führen Sie durch Klicken auf eine benutzerdefinierte serverseitige Logik. In diesem Tutorial betrachten wir das Hinzufügen von benutzerdefinierten Schaltflächen, eine Vorlage und die Felder des ein GridView- oder DetailsView-Steuerelement. Insbesondere erstellen wir eine Schnittstelle mit einem FormView-Steuerelement, das dem Benutzer ermöglicht, über die Lieferanten Seite. Für einen bestimmten Lieferanten wird die FormView Informationen zu den Lieferanten zusammen mit einer Schaltfläche-Websteuerelement angezeigt, die, wenn geklickt haben, alle ihre zugeordneten Produkte werden als markieren nicht mehr unterstützt. Darüber hinaus enthält einer GridView-Ansicht dieser Produkte, die von den ausgewählten Lieferanten bereitgestellt werden, in dem jede Zeile mit Preis erhöhen und Discount Preis Schaltflächen, die, wenn geklickt haben, erhöhen oder verringern das Produkt s `UnitPrice` um 10 % (siehe Abbildung 1).


[![FormView und GridView enthalten Sie Schaltflächen, die benutzerdefinierte Aktionen ausführen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Abbildung 1**: beide FormView und GridView enthalten Schaltflächen, führen Sie benutzerdefinierte Aktionen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten der Schaltfläche-Tutorial

Bevor Sie sich Gewusst wie: Hinzufügen einer benutzerdefinierten Schaltflächen befassen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Tutorial benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `CustomButtons`. Fügen Sie die folgenden beiden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `CustomButtons.aspx`


![Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `CustomButtons` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Abschließend fügen Sie die Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Paginierung und Sortierung `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Abbildung 4**: die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Schritt 2: Hinzufügen von einem FormView-Steuerelement, in der die Lieferanten aufgeführt.

Lassen Sie s beginnen mit diesem Lernprogramm durch Hinzufügen von FormView, der die Lieferanten auflistet. Wie in der Einleitung erwähnt, können diese FormView-Steuerelement den Benutzer auf der Seite über die Lieferanten, Produkte von Lieferanten in einer GridView-Ansicht angezeigt. Darüber hinaus enthält diese FormView-Steuerelement eine Schaltfläche, die beim Klicken auf alle Produkte Lieferanten s als markiert nicht mehr unterstützt. Bevor wir uns darum kümmern, die benutzerdefinierte Schaltfläche zu FormView hinzufügen, können Sie s zuerst nur das FormView-Steuerelement erstellen, damit die Lieferanteninformationen angezeigt.

Öffnen Sie zunächst die `CustomButtons.aspx` auf der Seite die `CustomButtons` Ordner. Fügen Sie einem FormView-Steuerelement auf der Seite durch Ziehen aus der Toolbox in den Designer und den Satz der `ID` Eigenschaft `Suppliers`. Von FormView s Smarttags, deaktivieren Sie zum Erstellen einer neuen, mit dem Namen "ObjectDataSource" `SuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource "ObjectDataSource"](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Diese neue "ObjectDataSource" So konfigurieren, dass abgefragt, die von der `SuppliersBLL` Klasse s `GetSuppliers()` Methode (siehe Abbildung 6). Da diese FormView keine Schnittstelle bietet für die Aktualisierung der Lieferanteninformationen auswählen, dass die (None) aus der Dropdown-Liste auf der Registerkarte "UPDATE"-option.


[![Konfigurieren der Datenquelle zur Verwendung der Klasse SuppliersBLL s GetSuppliers()-Methode](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Abbildung 6**: Konfigurieren der Datenquelle verwendet die `SuppliersBLL` Klasse s `GetSuppliers()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Nach der Konfiguration dem ObjectDataSource-Steuerelement, generiert Visual Studio ein `InsertItemTemplate`, `EditItemTemplate`, und `ItemTemplate` für das FormView-Steuerelement. Entfernen Sie die `InsertItemTemplate` und `EditItemTemplate` und ändern Sie die `ItemTemplate` , damit nur die Lieferanten s Company Name "und" Phone Anzahl angezeigt. Pagingunterstützung für das FormView schließlich aktivieren, indem Sie das Kontrollkästchen für die Auslagerungsdatei Aktivieren von smart Tag (oder durch Festlegen seiner `AllowPaging` Eigenschaft `True`). Nach diesen Änderungen sollte Ihre Seite s deklarativen Markup etwa wie folgt aussehen:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Abbildung 7 zeigt die Seite CustomButtons.aspx, wenn Sie über einen Browser angezeigt.


[![Die FormView aufgeführt, die CompanyName und Phone-Felder des aktuell ausgewählten Lieferanten](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Abbildung 7**: das FormView-Listet die `CompanyName` und `Phone` Felder aus den derzeit ausgewählten Lieferanten ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Schritt 3: Hinzufügen einer GridView-Ansicht, die den ausgewählten Anbieter-s-Produkte aufgeführt sind

Bevor wir die einstellen Schaltfläche "alle Produkte" der FormView-s-Vorlage hinzufügen, können Sie s zuerst eine GridView unter FormView hinzufügen, die die Produkte, die von den ausgewählten Lieferanten bereitgestellte aufgelistet werden. Legen Sie zu diesem Zweck werden Hinzufügen einer GridView-Ansicht auf der Seite, die `ID` Eigenschaft `SuppliersProducts`, und fügen Sie eine neue, mit dem Namen "ObjectDataSource" `SuppliersProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersProductsDataSource "ObjectDataSource"](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Abbildung 8**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Konfigurieren Sie diese "ObjectDataSource" zur Verwendung der Klasse ProductsBLL s `GetProductsBySupplierID(supplierID)` Methode (siehe Abbildung 9). Während dieser GridView zu einem Produkt s Preis angepasst werden kann, gewinnt er t verwenden die integrierten bearbeiten oder Löschen von Funktionen aus der GridView. Aus diesem Grund können wir die Dropdown-Liste (keine) für den "ObjectDataSource"-s Update-, INSERT- und DELETE-Registerkarten festlegen.


[![Konfigurieren der Datenquelle zur Verwendung der Klasse ProductsBLL s GetProductsBySupplierID(supplierID)-Methode](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Abbildung 9**: Konfigurieren der Datenquelle verwendet die `ProductsBLL` Klasse s `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Da die `GetProductsBySupplierID(supplierID)` -Methode akzeptiert einen Eingabeparameter, der ObjectDataSource-Steuerelement-Assistent fordert uns für die Quelle der Wert dieses Parameters. Übergeben der `SupplierID` FormView Wert, der Parameter-Quellliste Dropdown-Steuerelement und die ControlID Dropdown-Liste festgelegt `Suppliers` (die ID des FormView, die in Schritt2 erstellt wird).


[![Anzugeben Sie, dass die Lieferanten FormView-Steuerelement durch die SupplierID Parameter stammen sollen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Abbildung 10**: anzugeben, die die *`supplierID`* Parameter stammen soll die `Suppliers` FormView-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Nach Abschluss des Assistenten "ObjectDataSource" wird die GridView eine BoundField- oder die CheckBoxField für jedes Produkt s Datenfelder enthalten. Let s herauspicken dies angezeigt nur die `ProductName` und `UnitPrice` BoundFields zusammen mit der `Discontinued` CheckBoxField; darüber hinaus können s Format der `UnitPrice` BoundField so, dass der Text als Währung formatiert ist. Ihre GridView und `SuppliersProductsDataSource` "ObjectDataSource" s deklaratives Markup sieht ähnlich wie das folgende Markup:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

An diesem Punkt zeigt unser Tutorial ein Master-/Detail-Berichts, gestattet es dem Benutzer ein Lieferant FormView oben ausgewählt und Produkte von Lieferanten über GridView im unteren Bereich anzeigen. Abbildung 11 zeigt einen Screenshot der Seite, bei der Auswahl der Lieferant Tokyo Traders aus der FormView-Steuerelement.


[![Die ausgewählten Lieferanten-s-Produkte werden in den GridView-Ansicht angezeigt.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Abbildung 11**: der ausgewählte Anbieter s Produkte werden angezeigt, in den GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Schritt 4: Erstellen von DAL und BLL-Methoden, um alle Produkte für einen Lieferanten beenden

Bevor wir das FormView-Steuerelement eine Schaltfläche hinzufügen können, die beim Klicken auf beendet alle die Lieferanten-s-Produkte, müssen wir zuerst auf das Hinzufügen einer Methode für die DAL und BLL, der diese Aktion ausführt. Insbesondere diese Methode erhält `DiscontinueAllProductsForSupplier(supplierID)`. Wenn das FormView-s-Schaltfläche geklickt wird, werden wir diese Methode in der Geschäftslogikebene übergeben aufrufen, in der ausgewählten Lieferant s `SupplierID`; die BLL aufgerufen wird, klicken Sie dann nach unten um die entsprechenden Daten von Zugriffsschicht-Methode, die Ausstellen einer `UPDATE` -Anweisung die Datenbank, die die angegebenen Lieferanten s beendet.

Wie wir in unserem vorherigen Tutorials abgeschlossen haben, verwenden wir einen Bottom-Up-Ansatz, erstellen die DAL-Methode, klicken Sie dann die BLL-Methode, und implementieren die Funktionalität zum Schluss auf der ASP.NET-Seite ab. Öffnen der `Northwind.xsd` typisierte DataSet in der `App_Code/DAL` Ordner und eine neue Methode zum Hinzufügen der `ProductsTableAdapter` (mit der rechten Maustaste auf die `ProductsTableAdapter` , und wählen Sie die Abfrage hinzufügen). Auf diese Weise wird der TableAdapter-Abfrage-Konfigurations-Assistent, anzuzeigen, in dem wir durch den Prozess des Hinzufügens der neuen Methode führt. Zunächst gibt an, dass die DAL-Methode eine Ad-hoc-SQL-Anweisung verwenden.


[![Erstellen Sie die DAL-Methode, die mit Ad-hoc-SQL-Anweisungen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Abbildung 12**: Exemplarische Vorgehensweise: Erstellen von der DAL-Methode mit einer Ad-hoc-SQL-Anweisung ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Als Nächstes fordert der Assistent, mit uns, welche Art von Abfrage zu erstellen. Da die `DiscontinueAllProductsForSupplier(supplierID)` Methode müssen Sie aktualisieren die `Products` Datenbanktabelle, Festlegen der `Discontinued` 1 für alle Produkte, die bereitgestellt werden, durch das angegebene Feld *`supplierID`*, müssen wir eine Abfrage erstellen, die Daten aktualisiert.


[![Wählen Sie den Typ der UPDATE-Abfrage](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Abbildung 13**: Wählen Sie den Typ der UPDATE-Abfrage ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


Im nächsten Assistentenbildschirm enthält die TableAdapter-s-vorhandene `UPDATE` -Anweisung, die jedes der Felder im definierten aktualisiert die `Products` DataTable. Ersetzen Sie den Text dieser Abfrage mit der folgenden Anweisung:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Verwenden Sie nach Eingabe dieser Abfrage, und klicken als Nächstes der letzte Assistentenbildschirm für den neuen Namen der Methode s fordert `DiscontinueAllProductsForSupplier`. Schließen Sie den Assistenten, indem Sie auf die Schaltfläche "Fertig stellen". Bei der Rückkehr zum DataSet-Designer sollte eine neue Methode in der `ProductsTableAdapter` mit dem Namen `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Name der neuen DAL-Methode DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Abbildung 14**: Benennen Sie die neuen DAL `DiscontinueAllProductsForSupplier` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Mit der `DiscontinueAllProductsForSupplier(supplierID)` Methode, die in der Datenzugriffsebene erstellt werden, unsere nächste Aufgabe ist die Erstellung der `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der Geschäftslogikebene. Öffnen Sie dazu die `ProductsBLL` Klassendatei, und fügen Sie Folgendes:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Diese Methode ruft einfach nach unten, um die `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der die DAL aus, übergibt der bereitgestellten *`supplierID`* Parameterwert. Falls alle Geschäftsregeln, die nur einen Lieferanten s Produkte eingestellt werden, unter bestimmten Umständen zulässig sind, sollten diese Regeln hier in die BLL implementiert werden.

> [!NOTE]
> Im Gegensatz zu den `UpdateProduct` -Überladungen in die `ProductsBLL` -Klasse, die `DiscontinueAllProductsForSupplier(supplierID)` Methodensignatur enthält keinen der `DataObjectMethodAttribute` Attribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Dies verhindert, dass die `DiscontinueAllProductsForSupplier(supplierID)` Methode aus der "ObjectDataSource" s Konfigurieren von Datenquellen-Assistenten s Dropdown-Liste auf der Registerkarte "UPDATE". Ich Ve dieses Attribut weggelassen, da wir aufrufen, werden die `DiscontinueAllProductsForSupplier(supplierID)` Methode direkt von einem Ereignishandler auf unsere ASP.NET-Seite.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Schritt 5: Hinzufügen einer Schaltfläche "alle Produkte", das FormView-Steuerelement stellt

Mit der `DiscontinueAllProductsForSupplier(supplierID)` -Methode in der die BLL- und DAL abzuschließen, ist den letzten Schritt zum Hinzufügen der Möglichkeit, alle Produkte eingestellt, für der ausgewählten Lieferanten besteht im Hinzufügen eines Websteuerelements mit Schaltfläche "der FormView-s `ItemTemplate`. Let s hinzufügen, eine solche Schaltfläche unten die Telefonnummer für s-Anbieter mit den Text der Schaltfläche, beenden Sie alle Produkte und eine `ID` Eigenschaftswert `DiscontinueAllProductsForSupplier`. Sie können diese Schaltfläche Websteuerelements mit dem Designer hinzufügen, indem Sie auf den Link Vorlagen bearbeiten, in das FormView-s-Smarttag (siehe Abbildung 15), oder direkt über die deklarative Syntax.


[![Hinzufügen einer beenden Sie alle Produkte Button-Web-Steuerelements, das FormView-s-ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Abbildung 15**: Fügen Sie ein Beenden aller Produkte Web Schaltflächensteuerelement der FormView-s `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Wenn die Schaltfläche geklickt wird, indem Sie einen Benutzer auf ein Postback die Seite erfolgt und das FormView-s [ `ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) ausgelöst wird. Zum Ausführen von benutzerdefinierten Codes als Reaktion auf diese Schaltfläche geklickt wird, können wir einen Ereignishandler für dieses Ereignis erstellen. Zu verstehen, jedoch, die die `ItemCommand` Ereignis wird ausgelöst, wenn *alle* Schaltfläche, ImageButton Web oder LinkButton-Steuerelement innerhalb der FormView-Steuerelement geklickt wird. Dies bedeutet, dass, wenn der Benutzer von einer Seite zu einem anderen in der FormView-Steuerelement, das `ItemCommand` -Ereignis ausgelöst wird, dasselbe, wenn der Benutzer klickt auf die neu bearbeiten, oder löschen Sie in einem FormView-Steuerelement, das Einfügen, aktualisieren oder Löschen von unterstützt.

Da die `ItemCommand` ausgelöst wird, unabhängig davon, welche Schaltfläche klicken, wird im Ereignisprotokoll Handler benötigen wir eine Möglichkeit zu bestimmen, ob die Beenden alle Produkte Schaltfläche geklickt wurde, oder wenn es sich um eine andere Schaltfläche war. Um dies zu erreichen, können wir festlegen, das Steuerelement "Schaltfläche" Web "s `CommandName` Eigenschaft identifizierende Wert. Wenn die Schaltfläche geklickt wird, dies `CommandName` Wert wird übergeben, in der `ItemCommand` -Ereignishandler ermöglicht uns, zu bestimmen, ob die einstellen Schaltfläche "alle Produkte" die Schaltfläche geklickt wurde. Legen Sie die Schaltfläche für alle Produkte beenden s `CommandName` DiscontinueProducts Eigenschaft.

Zum Schluss möchte s, die ein Dialogfeld für die clientseitige bestätigen zu verwenden, um sicherzustellen, dass der Benutzer möchte die Produkte der ausgewählten Lieferanten s einzustellen. Wie in beschrieben der [Hinzufügen der clientseitigen Bestätigung beim Löschen von](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) Tutorial, kann dies mit etwas JavaScript erreicht werden. Legen Sie vor allem der s-OnClientClick-Eigenschaft der Schaltfläche "Web-Steuerelements auf `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Nach diesen Änderungen durchführen, sollte die deklarative Syntax des FormView-s wie folgt aussehen:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das FormView-s `ItemCommand` Ereignis. In diesem Ereignishandler müssen wir zunächst überprüfen, ob die Beenden alle Produkte Schaltfläche geklickt wurde. Wenn also eine Instanz von erstellt werden soll die `ProductsBLL` Klasse und rufen Sie die `DiscontinueAllProductsForSupplier(supplierID)` Methode und übergeben die `SupplierID` der ausgewählten FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Beachten Sie, dass die `SupplierID` des aktuellen ausgewählten Lieferanten in der FormView-Steuerelement erfolgen kann mithilfe der s FormView [ `SelectedValue` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). Die `SelectedValue` Eigenschaft zurückgibt, der erste Datenschlüsselwert für den Datensatz in der FormView-Steuerelement angezeigt wird. Die FormView s [ `DataKeyNames` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), die Daten, die Felder, die von dem die Daten die Schlüsselwerte abgerufen werden, wodurch auf automatisch festgelegt wurde `SupplierID` von Visual Studio beim Binden von dem ObjectDataSource-Steuerelement in den Hintergrund der FormView-Steuerelement in Schritt2.

Mit der `ItemCommand` -Ereignishandler erstellt haben, können Sie die Seite zu testen. Navigieren Sie zu der Cooperativa de Quesos "Las Cabras" Lieferanten (es s den fünften Lieferanten in der FormView-Steuerelement für mich). Dieser Lieferant bietet zwei Produkte, Queso Cabrales und Queso Manchego La Pastora, beide sind *nicht* nicht mehr unterstützt.

Stellen Sie sich vor, dass Cooperativa de Quesos 'Las Cabras' ist nicht mehr im Unternehmen und daher seine Produkte eingestellt werden. Klicken Sie auf die Schaltfläche "alle Produkte" einstellen. Dadurch wird das Dialogfeld "Client-Side bestätigen" angezeigt (siehe Abbildung 16).


[![Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Abbildung 16**: Cooperativa de Quesos Las Cabras stellt zwei aktiven Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Wenn Sie auf "OK", klicken Sie im Dialogfeld die clientseitige bestätigen "klicken, die Übermittlung des Formulars wird fortgesetzt, einen Postback verursacht, in dem das FormView-s `ItemCommand` Ereignis ausgelöst wird. Der Ereignishandler, die wir erstellt haben wird ausgeführt, Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` -Methode und die Queso Cabrales Queso Manchego La Pastora Produkte und ein.

Wenn Sie den Ansichtszustand des GridView-s deaktiviert haben, die GridView-Steuerelement ist in der zugrunde liegenden Datenspeicher bei jedem Postback erneut gebunden werden und aus diesem Grund wird sofort aktualisiert, um darauf hinzuweisen, dass diese beiden Produkte nicht mehr verwendet werden (siehe Abbildung 17). Wenn Sie jedoch, Sie nicht Ansichtszustand in den GridView-Ansicht deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nachdem diese Änderung vorgenommen. Um dies zu erreichen, stellen Sie einfach einen Aufruf der GridView-s `DataBind()` Methode sofort nach dem Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)` Methode.


[![Sind nach der Schaltfläche Beenden alle Produkte, die Produkte von Lieferanten s entsprechend aktualisiert](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Abbildung 17**: nach der Schaltfläche Beenden alle Produkte, die Produkte von Lieferanten s werden entsprechend aktualisiert ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Schritt 6: Erstellen einer UpdateProduct-Überladung in der Geschäftslogikebene für die Anpassung der eines Produkt-s-Preis

Wie müssen mit "stellt alle Produkte" in das FormView-Steuerelement, zum Hinzufügen von Schaltflächen für die vergrößert und verkleinert den Preis für ein Produkt in der GridView wir zunächst die entsprechenden Daten von Zugriffsschicht und Business Logic Layer-Methoden hinzufügen. Da wir bereits über eine Methode, die eine einzelnes Produktzeile in die DAL aktualisiert verfügen, können wir solche Funktionen bereitstellen, indem Sie erstellen eine neue Überladung für die `UpdateProduct` -Methode in der die BLL.

Die Vergangenheit `UpdateProduct` Überladungen, die in einer Kombination von Produkt Felder als skalare Eingabewerte erstellt haben, und verfügen dann nur die Felder für das angegebene Produkt aktualisiert. Diese Überladung wird unterscheiden sich geringfügig von diesem Standard- und stattdessen übergeben Sie das Produkt s `ProductID` und der Prozentsatz, mit dem Anpassen der `UnitPrice` (im Gegensatz zu übergeben, in der neuen, angepasst `UnitPrice` selbst). Dieser Ansatz wird vereinfacht den Code, die wir in der ASP.NET Page-Code-Behind-Klasse schreiben müssen, da wir Raten zu zu bestimmen, das aktuelle Produkt s wurden `UnitPrice`.

Die `UpdateProduct` überladen für unten in diesem Tutorial gezeigt wird:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Diese Überladung ruft Informationen über das angegebene Produkt über die DAL s `GetProductByProductID(productID)` Methode. Es wird dann geprüft, ob das Produkt s `UnitPrice` eine Datenbank zugewiesen wird `NULL` Wert. Wenn es sich handelt, kann der Preis bleibt unverändert. Wenn Sie allerdings es ist keine`NULL` `UnitPrice` Wert, der die Methode aktualisiert das Produkt s `UnitPrice` durch den angegebenen Prozentsatz (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Schritt 7: Hinzufügen der Erhöhung und Verringerung der Schaltflächen an die GridView

Das GridView (und DetailsView) sowohl eine Auflistung von Feldern bestehen. Neben dem BoundFields, CheckBoxFields und von TemplateFields bietet ASP.NET die ButtonField, die, wie der Name schon sagt, wie eine Spalte mit einer Schaltfläche, LinkButton oder ImageButton für jede Zeile gerendert wird. Ähnlich wie das FormView-Steuerelement, auf *alle* innerhalb der GridView Pagingschaltflächen, bearbeiten oder löschen Schaltflächen, Sortierung Schaltflächen und So weiter ein Postback auslöst, und löst das GridView-s [ `RowCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Die ButtonField verfügt über eine `CommandName` -Eigenschaft, die den angegebenen Wert zuweist, der die Schaltflächen `CommandName` Eigenschaften. Wie das FormView-Steuerelement, beispielsweise die `CommandName` Wert wird verwendet, durch die `RowCommand` -Ereignishandler, um zu bestimmen, welche Schaltfläche geklickt wurde.

Let s hinzufügen zwei neuer ButtonFields an die GridView, eine mit einer Schaltflächentext Preis + 10 % und der andere mit dem Text Preis – 10 %. Um diese ButtonFields hinzuzufügen, klicken Sie auf den Link "Spalten bearbeiten" aus dem GridView-s-Smarttag, wählen Sie den Feldtyp ButtonField aus der Liste in der oberen linken Ecke, und klicken Sie auf die Schaltfläche "hinzufügen".


![Hinzufügen von zwei ButtonFields an die GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Abbildung 18**: Hinzufügen von zwei ButtonFields an die GridView


Verschieben Sie die zwei ButtonFields, sodass sie als die ersten beiden GridView-Felder angezeigt werden. Legen Sie als Nächstes die `Text` Eigenschaften von diesen beiden ButtonFields zum Preis + 10 % und Preis – 10 % und die `CommandName` Eigenschaften IncreasePrice und DecreasePrice, bzw. Standardmäßig rendert eine ButtonField die Spalte mit Schaltflächen als LinkButtons. Dies kann geändert werden, jedoch über die ButtonField s [ `ButtonType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Lassen Sie s haben diese zwei ButtonFields als reguläre Schaltflächen gerendert; Legen Sie daher die `ButtonType` Eigenschaft `Button`. Abbildung 19 zeigt die Felder (Dialogfeld), nachdem diese Änderungen vorgenommen wurden, dann folgt das deklarative GridView-s-Markup.


![Konfigurieren Sie die ButtonFields Text, CommandName und ButtonType-Eigenschaften](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Abbildung 19**: Konfigurieren der ButtonFields `Text`, `CommandName`, und `ButtonType` Eigenschaften


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Mit diesen ButtonFields erstellt, der letzte Schritt ist, erstellen Sie einen Ereignishandler für das GridView-s `RowCommand` Ereignis. Dieser Ereignishandler, wenn ausgelöst, da der Preis + 10 % oder Preis – 10 % Schaltflächen wurden geklickt haben, Anforderungen, um zu bestimmen, die `ProductID` für die Zeile, deren Schaltfläche geklickt wurde, und rufen Sie dann, die `ProductsBLL` s-Klasse `UpdateProduct` Methode auf und übergibt in den entsprechenden `UnitPrice` Prozentuale Anpassung zusammen mit den `ProductID`. Der folgende Code führt folgende Aufgaben:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Um zu ermitteln, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis – 10 %-Schaltfläche geklickt wurde, müssen wir das GridView-s finden Sie in `DataKeys` Auflistung. Diese Auflistung enthält die Werte der Felder im angegebenen die `DataKeyNames` -Eigenschaft für jede GridView-Zeile. Seit der GridView-s `DataKeyNames` -Eigenschaft wurde auf "ProductID" von Visual Studio festgelegt, wenn es sich bei dem ObjectDataSource-Steuerelement an die GridView zu binden `DataKeys(rowIndex).Value` bietet die `ProductID` für den angegebenen *RowIndex*.

Die ButtonField werden automatisch in die *RowIndex* der Zeile, deren Schaltfläche, über geklickt wurde, die `e.CommandArgument` Parameter. Aus diesem Grund, um zu bestimmen, die `ProductID` für die Zeile, deren Preis + 10 % oder Preis – 10 %-Schaltfläche geklickt wurde, verwenden wir: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Als mit der alle Produkte eingestellt, wenn Sie den Ansichtszustand der GridView-s deaktiviert haben die GridView-Steuerelement ist in der zugrunde liegenden Datenspeicher bei jedem Postback erneut gebunden werden und aus diesem Grund wird sofort aktualisiert, um eine Preisänderung widerspiegeln, die zwischen dem Klicken auf tritt auf einer der Schaltflächen. Wenn Sie jedoch, Sie nicht Ansichtszustand in den GridView-Ansicht deaktiviert haben, müssen Sie manuell die Daten an die GridView zu binden, nachdem diese Änderung vorgenommen. Um dies zu erreichen, stellen Sie einfach einen Aufruf der GridView-s `DataBind()` Methode sofort nach dem Aufrufen der `UpdateProduct` Methode.

Abbildung 20 zeigt die Seite, wenn Sie Produkte von den OMA-Kelly Homestead anzeigen. Abbildung 21 zeigt die Ergebnisse nach dem Preis + 10, dass % für Großmutters Boysenberry verteilt und die Schaltfläche "Preis – 10 %" zweimal einmal für Northwoods Cranberry Sauce geklickt wurde.


[![GridView enthält Preis + 10 % und Preis – 10 %-Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Abbildung 20**: der GridView-Includes-Preis + 10 Preis – 10 % Schaltflächen "und" % ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Die Preise für das erste und dritte Produkt wurden aktualisiert, über den Preis + 10 % und Preis – 10 %-Schaltflächen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Abbildung 21**: die Preise für das erste und dritte Produkt wurden aktualisiert, über den Preis + 10 % und Preis – 10 % Schaltflächen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Das GridView (und DetailsView) können auch Schaltflächen, LinkButtons oder ImageButtons ihre von TemplateFields hinzugefügt haben. Wie mit der BoundField diese Schaltflächen, die beim Klicken auf einen Postback ausgelöst werden, das Auslösen der GridView-s `RowCommand` Ereignis. Beim Hinzufügen von Schaltflächen in ein TemplateField, jedoch der Schaltfläche "s" `CommandArgument` wird nicht automatisch festgelegt, der Index der Zeile wie bei ButtonFields verwenden. Wenn Sie müssen den Zeilenindex der Schaltfläche zu ermitteln, die in auf die geklickt wurde der `RowCommand` -Ereignishandler müssen Sie manuell auf die Schaltfläche "s" festgelegt `CommandArgument` -Eigenschaft in seiner deklarativen Syntax in das TemplateField, mit Code wie:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`


## <a name="summary"></a>Zusammenfassung

Die Steuerelemente GridView, DetailsView oder FormView können Schaltflächen, LinkButtons oder ImageButtons enthalten. Diese Schaltflächen, die beim Klicken auf einen Postback verursachen und Auslösen der `ItemCommand` Ereignis in die FormView und DetailsView-Steuerelemente und die `RowCommand` Ereignis in den GridView-Ansicht. Diese datenwebsteuerelemente haben integrierte Funktionen, die häufig verwendete Befehle bezogenen Aktionen, z. B. das Löschen oder Bearbeiten von Datensätzen zu behandeln. Wir können jedoch auch verwenden, Schaltflächen, die beim Klicken auf, reagieren Sie mit unserer eigenen benutzerdefinierten Code ausführen.

Zu diesem Zweck müssen wir erstellen einen Ereignishandler für die `ItemCommand` oder `RowCommand` Ereignis. In diesem Ereignishandler überprüfen wir zunächst den eingehenden `CommandName` Wert zu bestimmen, welche Schaltfläche geklickt wurde, und klicken Sie dann eine entsprechende Aktion. In diesem Tutorial wurde erläutert, wie Schaltflächen und ButtonFields verwenden, um alle Produkte für einen angegebenen Lieferanten zu beenden oder zu erhöhen oder verringern den Preis für ein bestimmtes Produkt um 10 %.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Vorherige](adding-and-responding-to-buttons-to-a-gridview-cs.md)
