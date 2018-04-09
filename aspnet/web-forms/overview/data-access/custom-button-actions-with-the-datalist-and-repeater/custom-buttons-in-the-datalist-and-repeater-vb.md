---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Benutzerdefinierte Schaltflächen im DataList und Repeater (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm fügen wir eine Schnittstelle erstellen, die einen Repeater verwendet wird, um die Liste der Kategorien im System, mit jeder Kategorie bietet eine Schaltfläche, um seine Associ anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e470590252102c486bb72ff46f516180aa09ba8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Benutzerdefinierte Schaltflächen im DataList und Repeater (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) oder [PDF herunterladen](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> In diesem Lernprogramm fügen wir eine Schnittstelle erstellen, die einen Repeater verwendet wird, um die Liste der Kategorien im System, mit jeder Kategorie bietet eine Schaltfläche, um seine zugeordneten Produkte mit einem BulletedList-Steuerelement angezeigt.


## <a name="introduction"></a>Einführung

In den vergangenen siebzehn DataList und Repeater Lernprogrammen wir Ve erstellt sowohl nur-Lese Beispiele und bearbeiten und Löschen von Beispielen. Erleichterung von bearbeiten und Löschen von Funktionen in einem DataList wir Schaltflächen hinzugefügt, um DataList s `ItemTemplate` , die beim Klicken auf einen Postback verursacht, und eine entsprechende, auf die Schaltfläche "s" DataList-Ereignis ausgelöst hat `CommandName` Eigenschaft. Z. B. eine Schaltfläche zum Hinzufügen der `ItemTemplate` mit einer `CommandName` Eigenschaftswert Bearbeitung bewirkt, dass das DataList-s `EditCommand` zum Auslösen von Postbacks mit der `CommandName` löschen löst die `DeleteCommand`.

Darüber hinaus zum Bearbeiten und Löschen von Schaltflächen, die verschiedenen Steuerelemente können auch enthalten Schaltflächen, LinkButtons oder ImageButtons, wenn angeklickt, führen Sie einige benutzerdefinierte serverseitige Logik. In diesem Lernprogramm fügen wir eine Schnittstelle erstellen, die einen Repeater verwendet wird, um die Kategorien im System aufzulisten. Für jede Kategorie Repeater enthält eine Schaltfläche, um die Kategorie s, die Produkte, die über ein BulletedList-Steuerelement angezeigt (siehe Abbildung 1).


[![Klicken Sie auf Anzeigen Produkte Verknüpfung zeigt die Kategorie-s-Produkte in einer Aufzählung](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Abbildung 1**: Klicken Sie auf der anzeigen-Produkte Link zeigt die Kategorie s Produkte in einer Aufzählung ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Webseiten die benutzerdefinierte Schaltfläche-Lernprogramm

Bevor wir eine benutzerdefinierte Schaltfläche hinzufügen betrachten, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm benötigen. Starten, indem Sie einen neuen Ordner namens `CustomButtonsDataListRepeater`. Fügen Sie die folgenden beiden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `CustomButtons.aspx`


![Fügen Sie die ASP.NET-Seiten für die benutzerdefinierte Schaltflächen-bezogene Lernprogramme](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die benutzerdefinierte Schaltflächen-bezogene Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `CustomButtonsDataListRepeater` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Fügen Sie dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Fügen Sie abschließend die Seiten hinzu, als Einträge an die `Web.sitemap` Datei. Insbesondere das folgende Markup nach dem Sortieren, Paging und hinzufügen, mit dem DataList und Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für die Bearbeitung, einfügen und Löschen von Lernprogramme.


![Die Siteübersicht enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Lernprogramm](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Abbildung 4**: die Siteübersicht enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Lernprogramm


## <a name="step-2-adding-the-list-of-categories"></a>Schritt 2: Hinzufügen der Liste der Kategorien

Für dieses Lernprogramm müssen wir einen Repeater erstellen, die Listet alle Kategorien zusammen mit LinkButton Produkte anzeigen, die beim Klicken auf die zugeordnete Kategorie s-Produkte in einer Aufzählung angezeigt. Lassen Sie s zuerst einen einfachen Repeater erstellen, die im System die Kategorien aufgeführt. Öffnen Sie zunächst die `CustomButtons.aspx` auf der Seite der `CustomButtonsDataListRepeater` Ordner. Ziehen Sie einen Repeater aus der Toolbox auf die Designer, und legen Sie dessen `ID` Eigenschaft `Categories`. Als Nächstes erstellen Sie ein neues Datenquellen-Steuerelement aus dem Smarttag des Wiederholungsmoduls s. Insbesondere erstellen Sie ein neues ObjectDataSource-Steuerelement mit dem Namen `CategoriesDataSource` auswählt, deren Daten aus der `CategoriesBLL` Klasse s `GetCategories()` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL Klasse s GetCategories()-Methode](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Abbildung 5**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse s `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


Im Gegensatz zu DataList-Steuerelement, für die Visual Studio ein Standard-erstellt `ItemTemplate` basierend auf der Datenquelle, die Vorlagen Repeater s müssen manuell definiert werden. Darüber hinaus müssen die Repeater s-Vorlagen erstellt und bearbeitet deklarativ (d. h. es s keine Vorlagen bearbeiten-option im Wiederholungsmodul s smart Tag).

Klicken Sie auf der Registerkarte "Datenquelle" in der unteren linken Ecke, und fügen eine `ItemTemplate` , die Namen der Kategorie s in anzeigt ein `<h3>` Element und dessen Beschreibung eines Absatzes zu kennzeichnen; enthalten eine `SeparatorTemplate` , die eine horizontale Linie anzeigt (`<hr />`) zwischen den einzelnen Kategorie. Auch hinzufügen LinkButton mit seiner `Text` Eigenschaftensatz zu Produkten anzuzeigen. Nach Abschluss der Schritte sollten Ihre deklarativen Seitenmarkup für s wie folgt aussehen:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Abbildung 6 zeigt die Seite, wenn Sie über einen Browser angezeigt. Jeder Kategoriename und Beschreibung aufgeführt ist. Produkte anzeigen-Schaltfläche, die beim Klicken auf einen Postback verursacht jedoch führt keine noch eingreifen.


[![Jede Kategorie s Name und Beschreibung wird zusammen mit der LinkButton Produkte anzeigen angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Abbildung 6**: jede Kategorie s Name und Beschreibung wird angezeigt, zusammen mit LinkButton Produkte anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Schritt 3: Ausführen serverseitige Logik beim der anzeigen-Produkte LinkButton geklickt wird

Jedes Mal, wenn eine Schaltfläche, LinkButton oder ImageButton innerhalb einer Vorlage in einem DataList oder Repeater geklickt wird, was zu ein Postback tritt auf, und der s DataList oder Repeater `ItemCommand` Ereignis ausgelöst wird. Zusätzlich zu den `ItemCommand` Ereignis, das DataList-Steuerelement auch eine andere und genauere Ereignis ausgelöst kann, wenn die Schaltfläche "s" `CommandName` Eigenschaftensatz wird auf einen der reservierten Zeichenfolgen ("löschen", "Bearbeiten" Abbrechen ", Update oder wählen Sie"), aber die `ItemCommand` Ereignis ist *immer* ausgelöst.

Wenn eine Schaltfläche in einem DataList oder Repeater geklickt wird, muss häufig (im Fall, dass es möglicherweise mehrere Schaltflächen innerhalb des Steuerelements, wie z. B. beide eine Bearbeitung und Schaltfläche "löschen"), auf welche Schaltfläche geklickt wurde und möglicherweise einige zusätzliche Informationen (z. B. übergibt der Wert des Primärschlüssels des Elements, dessen Schaltfläche geklickt wurde). Die Schaltfläche, LinkButton und ImageButton bieten zwei Eigenschaften, deren Werte werden an, die `ItemCommand` Ereignishandler:

- `CommandName` eine Zeichenfolge, die normalerweise verwendet, um jede Schaltfläche in der Vorlage zu identifizieren.
- `CommandArgument` häufig verwendet, um den Wert eines Felds Daten, z. B. dem Primärschlüsselwert halten

In diesem Beispiel legen Sie die s LinkButton `CommandName` Eigenschaft ShowProducts und Binden der aktuelle Datensatz s Primärschlüsselwert `CategoryID` auf die `CommandArgument` Eigenschaft, die mit der Syntax Databinding `CategoryArgument='<%# Eval("CategoryID") %>'`. Nachdem Sie diese beiden Eigenschaften angegeben, sollte die LinkButton s deklarative Syntax wie folgt aussehen:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Wenn die Schaltfläche klicken, wird ein Postback erfolgt und die s DataList oder Repeater `ItemCommand` Ereignis ausgelöst wird. Der Ereignishandler wird die Schaltfläche "s" übergeben `CommandName` und `CommandArgument` Werte.

Erstellen Sie einen Ereignishandler für die Repeater s `ItemCommand` Ereignis, und beachten Sie den zweiten Parameter an den Ereignishandler übergeben (mit dem Namen `e`). Diese zweite Parameter ist vom Typ [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) und verfügt über die folgenden vier Eigenschaften:

- `CommandArgument` der Wert von der Schaltfläche s `CommandArgument` Eigenschaft
- `CommandName` der Wert der Schaltfläche s `CommandName` Eigenschaft
- `CommandSource` Ein Verweis auf das Schaltflächen-Steuerelement, auf die geklickt wurde
- `Item` Ein Verweis auf die [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , enthält die Schaltfläche, die per Mausklick; jeder Datensatz an Wiederholungsmoduls gebunden wird auswirken, als ein `RepeaterItem`

Seit der ausgewählten Kategorie s `CategoryID` über übergeben der `CommandArgument` -Eigenschaft, wir können den Satz von Produkten in der ausgewählten Kategorie zugeordnet der `ItemCommand` -Ereignishandler. Diese Produkte können in einem BulletedList-Steuerelement gebunden werden die `ItemTemplate` (die wir noch hinzuzufügende Ve). Alle, die verbleibt, besteht im Hinzufügen der BulletedList, verweisen sie in der `ItemCommand` -Ereignishandler, und binden, welche Produkte für die ausgewählte Kategorie, die wir in Schritt 4 konfigurieren müssen.

> [!NOTE]
> DataList-s `ItemCommand` übergebene Ereignishandler wird ein Objekt des Typs [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), welcher bietet die gleichen vier Eigenschaften wie die `RepeaterCommandEventArgs` Klasse.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Schritt 4: Anzeigen der ausgewählten Kategorieprodukte in einer Liste mit Aufzählungszeichen

Die ausgewählte Kategorie s Produkte angezeigt werden können, innerhalb der Repeater s `ItemTemplate` über eine beliebige Anzahl von Steuerelementen. Es konnte hinzugefügt werden, dass eine andere Repeater, DataList DropDownList, eine GridView und So weiter geschachtelt. Da wir die Produkte als Aufzählung angezeigt werden soll, jedoch verwenden wir das BulletedList-Steuerelement. Zum Zurückgeben der `CustomButtons.aspx` Seite s deklaratives Markup, fügen Sie ein BulletedList-Steuerelement, um die `ItemTemplate` nach LinkButton Produkte anzeigen. Legen Sie die s BulletedLists `ID` auf `ProductsInCategory`. BulletedList zeigt den Wert des Feld "Daten" angegeben, über die `DataTextField` Eigenschaft; da dieses Steuerelement hat Produktinformationen an es gebunden ist, legen Sie die `DataTextField` Eigenschaft `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

In der `ItemCommand` Ereignishandler, d. h. verweisen auf dieses Steuerelement mit `e.Item.FindControl("ProductsInCategory")` und binden Sie es auf den Satz von Produkten, die der ausgewählten Kategorie zugeordnet ist.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Vor dem Ausführen einer Aktion in der `ItemCommand` Ereignishandler, d. h. es unerlässlich, überprüfen Sie zunächst den Wert eines eingehenden s `CommandName`. Da die `ItemCommand` -Ereignishandler wird ausgelöst, wenn *alle* Schaltfläche geklickt wird, wenn es mehrere Schaltflächen in der Vorlage verwendet werden die `CommandName` Wert an, welche Aktion erkannt werden kann. Überprüfen der `CommandName` sieht hinfällig, da wir nur ein einzelnes Optionsfeld haben allerdings handelt es sich, Gewohnheit Formular. Als Nächstes wird die `CategoryID` der ausgewählten Kategorie abgerufen, von der `CommandArgument` Eigenschaft. Der BulletedList-Steuerelement in der Vorlage klicken Sie dann auf die verwiesen wird und gebunden an die Ergebnisse der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode.

In vorherigen Lernprogrammen, die die Schaltflächen in einem DataList, z. B. verwendet [eine Übersicht über die von bearbeiten und Löschen von Daten in das DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), ermittelt den Wert des Primärschlüssels ein bestimmtes Element über die `DataKeys` Auflistung. Während dieser Ansatz gut DataList geeignet, wird die Repeater verfügt nicht über eine `DataKeys` Eigenschaft. Stattdessen müssen wir einen alternativen Ansatz verwenden, für die Angabe des Primärschlüsselwerts, z. B. über die Schaltfläche "s" `CommandArgument` Eigenschaft oder durch Zuweisen des Primärschlüsselwerts ein ausgeblendetes Label-Websteuerelement in der Vorlage und Lesen des Werts zurück in die `ItemCommand`Ereignishandler mit `e.Item.FindControl("LabelID")`.

Nach Abschluss der `ItemCommand` -Ereignishandler können Sie diese Seite in einem Browser zu testen. Wie in Abbildung 7 gezeigt, auf die Produkte anzeigen Link führt dazu, dass einen Postback und zeigt die Produkte für die ausgewählte Kategorie in einem BulletedList. Darüber hinaus Beachten Sie, dass diese Produktinformationen bleibt, selbst wenn andere Kategorien anzeigen Produkte Links geklickt werden.

> [!NOTE]
> Wenn Sie das Verhalten dieses Berichts ändern möchten, dass jeweils nur eine Kategorie s Produkte aufgeführt sind, legen Sie einfach das BulletedList-Steuerelement s `EnableViewState` Eigenschaft `False`.


[![Ein BulletedList wird verwendet, um die Produkte der ausgewählten Kategorie angezeigt.](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Abbildung 7**: ein BulletedList wird verwendet, um die Produkte der ausgewählten Kategorie angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Zusammenfassung

Die verschiedenen Steuerelemente können eine beliebige Anzahl von Schaltflächen, LinkButtons oder ImageButtons in ihre Vorlagen enthalten. Solche Schaltflächen, die beim Klicken auf einen Postback verursacht und löst die `ItemCommand` Ereignis. Um eine Schaltfläche geklickt wird die benutzerdefinierte Aktion für serverseitige zuzuordnen, erstellen Sie einen Ereignishandler für das `ItemCommand` Ereignis. In diesem Ereignishandler überprüfen Sie zunächst den eingehenden `CommandName` um zu bestimmen, welches Steuerelement die Schaltfläche geklickt wurde. Weitere Informationen kann optional angegeben werden, über die Schaltfläche "s" `CommandArgument` Eigenschaft.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Dennis Patterson. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](custom-buttons-in-the-datalist-and-repeater-cs.md)
