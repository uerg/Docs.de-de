---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Benutzerdefinierte Schaltflächen im DataList- oder Wiederholungssteuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erstellen wir eine Schnittstelle, die einen Repeater verwendet wird, um die Liste der im System, wobei jede Kategorie eine Schaltfläche zum Anzeigen der Associ bereitstellen anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 802f52e8e4e1ca1addec3321503ac92474ffd6b8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369485"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Benutzerdefinierte Schaltflächen im DataList- oder Wiederholungssteuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) oder [PDF-Datei herunterladen](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> In diesem Tutorial erstellen wir eine Schnittstelle, die einen Repeater verwendet wird, um die Liste von Kategorien im System, wobei jede Kategorie, die Bereitstellung einer Schaltfläche, um die zugehörige Produkte, die mithilfe eines BulletedList-Steuerelements anzeigen.


## <a name="introduction"></a>Einführung

In den letzten siebzehn DataList- oder Wiederholungssteuerelement Tutorials wir erstellt haben, sowohl Beispiele für die nur-Lese und bearbeiten und Löschen von Beispielen. Um zu ermöglichen, bearbeiten und Löschen von Funktionen in einem DataList-Steuerelement, wir Schaltflächen hinzugefügt, an die Datenliste s `ItemTemplate` , die beim Klicken auf einen Postback verursacht, und eine für die Schaltfläche "s" DataList-Ereignis ausgelöst hat `CommandName` Eigenschaft. Z. B. eine Schaltfläche zum Hinzufügen der `ItemTemplate` mit einer `CommandName` Eigenschaftswert Bearbeitung bewirkt, dass die DataList-s `EditCommand` auf Postbacks ausgelöst werden mit der `CommandName` löschen löst die `DeleteCommand`.

Darüber hinaus zum Bearbeiten und Löschen von Schaltflächen, die DataList- oder Repeater-Steuerelemente können auch enthalten Schaltflächen, LinkButtons oder ImageButtons, führen Sie durch Klicken auf eine benutzerdefinierte serverseitige Logik. In diesem Tutorial erstellen wir eine Schnittstelle, die einen Repeater verwendet wird, um die Liste der Kategorien im System. Für jede Kategorie umfasst der Repeater eine Schaltfläche, um die Kategorie verknüpft sind Produkte, die über ein BulletedList-Steuerelement angezeigt werden (siehe Abbildung 1).


[![Produkte Link zeigt die Kategorie-s-Produkte in einer Liste mit Aufzählungszeichen der anzeigen klicken](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Abbildung 1**: Klicken Sie auf Link zeigt für die Produkte zeigen die Kategorie-s-Produkte in einer Liste mit Aufzählungszeichen ([klicken Sie, um das Bild in voller Größe anzeigen](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen, die benutzerdefinierte Schaltfläche Tutorial-Webseiten

Bevor Sie an, wie eine benutzerdefinierte Schaltfläche hinzufügen befassen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Tutorial benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `CustomButtonsDataListRepeater`. Fügen Sie die folgenden beiden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `CustomButtons.aspx`


![Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für die benutzerdefinierten Schaltflächen-bezogene-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `CustomButtonsDataListRepeater` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Fügen Sie dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Abschließend fügen Sie die Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Paginierung und Sortierung, mit dem DataList- und Wiederholungssteuerelement `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Abbildung 4**: die Sitemap enthält jetzt den Eintrag für das benutzerdefinierte Schaltflächen-Tutorial


## <a name="step-2-adding-the-list-of-categories"></a>Schritt 2: Hinzufügen der Liste der Kategorien

Für dieses Tutorial benötigen wir einen Repeater zu erstellen, die alle Kategorien zusammen mit LinkButton-Produkte anzeigen aufgeführt, die beim Klicken auf die zugeordnete Kategorie s-Produkte in einer Aufzählung angezeigt. Lassen Sie s, erstellen Sie zunächst ein einfaches Repeater, die im System die Kategorien aufgeführt. Öffnen Sie zunächst die `CustomButtons.aspx` auf der Seite die `CustomButtonsDataListRepeater` Ordner. Ziehen Sie einen Repeater aus der Toolbox in den Designer und den Satz der `ID` Eigenschaft `Categories`. Als Nächstes erstellen Sie ein neues Datenquellen-Steuerelement, aus dem Repeater-s-Smarttag. Insbesondere erstellen Sie ein neues ObjectDataSource-Steuerelement, das mit dem Namen `CategoriesDataSource` auswählt, die Daten aus der die `CategoriesBLL` Klasse s `GetCategories()` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der CategoriesBLL Klasse s GetCategories()-Methode](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` s-Klasse `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Im Gegensatz zu dem DataList-Steuerelement, für die Visual Studio ein Standard erstellt- `ItemTemplate` basierend auf der Datenquelle, die Vorlagen Repeater s manuell definiert werden müssen. Darüber hinaus müssen die Repeater-s-Vorlagen erstellt und bearbeitet, deklarativ (d. h. es s keine Vorlagen bearbeiten-option im Smarttag Repeater s).

Klicken Sie auf der Registerkarte "Datenquelle" in der unteren linken Ecke, und fügen eine `ItemTemplate` , die den Kategorienamen s in anzeigt ein `<h3>` -Element und seine Beschreibung in einem Absatz markieren, enthalten eine `SeparatorTemplate` , die eine horizontale Trennlinie anzeigt (`<hr />`) zwischen den einzelnen die Kategorie. Auch hinzufügen eine LinkButton mit seiner `Text` Eigenschaftensatz zu Produkten anzuzeigen. Nach Abschluss dieser Schritte sollte im deklarativen Markup Ihrer Seite s wie folgt aussehen:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Abbildung 6 zeigt die Seite, wenn Sie über einen Browser angezeigt. Jeder Name und die Beschreibung wird aufgeführt. Die Produkte anzeigen wird beim Klicken auf ein Postback auslöst aber eine Aktion noch nicht ausgeführt wird.


[![Jede Kategorie s Name und die Beschreibung wird zusammen mit LinkButton-Produkte anzeigen angezeigt](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Abbildung 6**: jede Kategorie-s-Name und Beschreibung wird angezeigt, zusammen mit LinkButton-Produkte anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Schritt 3: Ausführen der serverseitigen Logik bei der anzeigen-Produkte LinkButton geklickt wird

Jedes Mal, wenn eine Schaltfläche, LinkButton oder ImageButton innerhalb einer Vorlage in einem DataList- oder Repeater geklickt wird, ein Postback auftritt und der s DataList- oder Repeater `ItemCommand` -Ereignis ausgelöst wird. Zusätzlich zu den `ItemCommand` -Ereignis, das DataList-Steuerelement möglicherweise auch eine andere und spezifischere-Ereignis ausgelöst, wenn die Schaltfläche "s" `CommandName` -Eigenschaftensatz auf eine der reservierten Zeichenfolgen ("löschen", "Bearbeiten", "Abbrechen", "Update" oder "auswählen"), aber die `ItemCommand` Ereignis ist *immer* ausgelöst.

Wenn eine Schaltfläche in einem DataList- oder Repeater geklickt wird, muss häufig (in dem Fall, dass möglicherweise mehrere Schaltflächen innerhalb des Steuerelements, z. B. eine Bearbeitungs- und Schaltfläche "löschen"), auf welche Schaltfläche geklickt wurde und möglicherweise einige zusätzliche Informationen (z. B. übergibt die Primärschlüssel-Wert des Elements, dessen Schaltfläche geklickt wurde). Die Schaltfläche, LinkButton und ImageButton verfügen über zwei Eigenschaften, deren Werte an, die `ItemCommand` -Ereignishandler:

- `CommandName` eine Zeichenfolge, die in der Regel verwendet, um jede Schaltfläche in der Vorlage zu identifizieren.
- `CommandArgument` häufig verwendet, um den Wert eines Datenfelds, z. B. der primäre Schlüsselwert enthalten soll

In diesem Beispiel legen Sie die s LinkButton `CommandName` Eigenschaft ShowProducts und die Bindung für den aktuellen Datensatz s Primärschlüsselwert `CategoryID` auf die `CommandArgument` Eigenschaft, die mit der Datenbindungssyntax `CategoryArgument='<%# Eval("CategoryID") %>'`. Wenn Sie diese beiden Eigenschaften angegeben, sollte die deklarative Syntax des LinkButton-s wie folgt aussehen:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Wenn die Schaltfläche geklickt wird ein Postback auftritt, und der s DataList- oder Repeater `ItemCommand` -Ereignis ausgelöst wird. Der Ereignishandler wird die Schaltfläche "s" übergeben `CommandName` und `CommandArgument` Werte.

Erstellen Sie einen Ereignishandler für das Repeater s `ItemCommand` Ereignis und beachten Sie den zweiten Parameter an den Ereignishandler übergeben (mit dem Namen `e`). Dieser zweite Parameter ist vom Typ [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) und weist die folgenden vier Eigenschaften:

- `CommandArgument` der Wert der angeklickten Schaltfläche s `CommandArgument` Eigenschaft
- `CommandName` der Wert der Schaltfläche s `CommandName` Eigenschaft
- `CommandSource` Ein Verweis auf das Schaltflächen-Steuerelement, auf die geklickt wurde
- `Item` Ein Verweis auf die [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , enthält der angeklickten Schaltfläche; jeder Datensatz des Repeaters gebunden ist der datenträgerverwaltung als ein `RepeaterItem`

Seit der ausgewählten Kategorie s `CategoryID` über übergeben die `CommandArgument` -Eigenschaft, wir können den Satz von Produkten, die in der ausgewählten Kategorie zugeordnet der `ItemCommand` -Ereignishandler. Diese Produkte klicken Sie dann in einem BulletedList-Steuerelement gebunden werden können die `ItemTemplate` (die wir haben noch hinzuzufügende). Alles, was bleibt, und klicken Sie dann, ist das Hinzufügen von BulletedList, verweisen sie in der `ItemCommand` -Ereignishandler und binden, den Satz von Produkten für die ausgewählte Kategorie, die wir in Schritt 4 in Angriff zu nehmen müssen.

> [!NOTE]
> DataList-Steuerelement s `ItemCommand` -Ereignishandler wird ein Objekt des Typs übergeben [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), welcher bietet die gleichen vier Eigenschaften wie die `RepeaterCommandEventArgs` Klasse.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Schritt 4: Anzeigen der Produkte der ausgewählten Kategorie s in eine Liste mit Aufzählungszeichen

Die ausgewählte Kategorie s Produkte angezeigt werden können, innerhalb des Repeaters s `ItemTemplate` mit einer Reihe von Steuerelementen. Es konnte hinzugefügt werden, dass eine andere Repeater, einem DataList-Steuerelement, einem DropDownList-Steuerelement, eine GridView und So weiter geschachtelt. Da wir Produkte als Aufzählung anzeigen möchten, dagegen verwenden wir das BulletedList-Steuerelement. Zum Zurückgeben der `CustomButtons.aspx` Seite s deklaratives Markup, das Hinzufügen eines Steuerelements BulletedList, um die `ItemTemplate` nach LinkButton-Produkte anzeigen. Legen Sie die s BulletedLists `ID` zu `ProductsInCategory`. BulletedList zeigt den Wert des Felds angegeben wird, über die `DataTextField` Eigenschaft, da dieses Steuerelement Produktinformationen gebunden hat, legen Sie die `DataTextField` Eigenschaft `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

In der `ItemCommand` Ereignishandler, verweisen Sie auf dieses Steuerelement `e.Item.FindControl("ProductsInCategory")` und binden Sie es auf den Satz von Produkten, die der ausgewählten Kategorie zugeordnet.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Vor dem Ausführen einer Aktion in der `ItemCommand` -Ereignishandler es ratsam, zuerst prüfen muss den Wert des eingehenden s `CommandName`. Da die `ItemCommand` -Ereignishandler wird ausgelöst, wenn *alle* Schaltfläche geklickt wird, treten mehrere Schaltflächen in der Vorlage verwendet die `CommandName` Wert, die auszuführende Aktion an, zu erkennen. Überprüfen der `CommandName` sieht sich fraglich, da wir nur eine einzelne Schaltfläche haben allerdings handelt es sich, eine sollte zur guten Gewohnheit Formular. Als Nächstes wird der `CategoryID` der ausgewählten Kategorie abgerufen, von der `CommandArgument` Eigenschaft. Der BulletedList-Steuerelement in der Vorlage klicken Sie dann auf die verwiesen wird und gebunden an die Ergebnisse der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode.

In vorherigen Tutorials, mit denen die Schaltflächen in einem DataList-Steuerelement, z. B. [eine Übersicht über die von bearbeiten und Löschen von Daten im DataList-Steuerelement](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), wir festgestellt, dass der Wert des Primärschlüssels ein bestimmtes Element über die `DataKeys` Auflistung. Obwohl dieser Ansatz gut mit DataList-Steuerelement funktioniert, wird der Repeater verfügt nicht über eine `DataKeys` Eigenschaft. Wir müssen stattdessen eine alternative Methode zum Angeben von des Wert des Primärschlüssels, z. B. über die Schaltfläche "s" `CommandArgument` Eigenschaft oder ein ausgeblendetes Bezeichnung-Steuerelement in der Vorlage mit den Wert des Primärschlüssels zugewiesen, und Lesen des Werts in der `ItemCommand`Event Handler mit `e.Item.FindControl("LabelID")`.

Nach Abschluss der `ItemCommand` -Ereignishandler können Sie diese Seite in einem Browser zu testen. Wie in Abbildung 7 dargestellt, auf die Produkte anzeigen Link ein Postback auslöst und zeigt die Produkte für die ausgewählte Kategorie in eine BulletedList. Beachten Sie außerdem, dass diese Produktinformationen bleibt, auf, auch wenn andere Kategorien anzeigen Produkte Links geklickt werden.

> [!NOTE]
> Wenn Sie das Verhalten dieses Berichts, ändern möchten, die nur eine Kategorie s-Produkte zu einem Zeitpunkt aufgeführt sind, legen Sie einfach das Steuerelement BulletedList s `EnableViewState` Eigenschaft `False`.


[![Eine BulletedList wird verwendet, um die Produkte der ausgewählten Kategorie angezeigt.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Abbildung 7**: ein BulletedList wird verwendet, um die Produkte der ausgewählten Kategorie angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Zusammenfassung

Die DataList- oder Repeater-Steuerelemente können eine beliebige Anzahl von Schaltflächen, LinkButtons oder ImageButtons in ihren Vorlagen enthalten. Diese Schaltflächen, die beim Klicken auf einen Postback verursachen und Auslösen der `ItemCommand` Ereignis. Um benutzerdefinierte serverseitige-Aktion mit einer Schaltfläche geklickt wird zu verknüpfen, erstellen Sie einen Ereignishandler für die `ItemCommand` Ereignis. In diesem Ereignishandler überprüfen Sie zunächst den eingehenden `CommandName` um zu bestimmen, welche Schaltfläche geklickt wurde. Weitere Informationen kann optional angegeben werden, über die Schaltfläche "s" `CommandArgument` Eigenschaft.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Dennis Patterson. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](custom-buttons-in-the-datalist-and-repeater-vb.md)
