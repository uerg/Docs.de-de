---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Hinzufügen einer GridView-Spalte mit Optionsfeldern (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial dargestellt, wie eine Spalte mit Optionsfeldern zu einem GridView-Steuerelement, sodass den Benutzer mit einer stärker intuitive Möglichkeit der Auswahl einer einzelnen Zeile der hinzufügen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 6932892ae75d9a4cb6d84ff7e92d9f2f611f6013
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395421"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Hinzufügen einer GridView-Spalte mit Optionsfeldern (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) oder [PDF-Datei herunterladen](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> In diesem Tutorial dargestellt, wie eine Spalte mit Optionsfeldern ein GridView-Steuerelement, sodass den Benutzer mit einer stärker intuitive Möglichkeit der Auswahl einer einzelnen Zeile der GridView hinzu.


## <a name="introduction"></a>Einführung

Das GridView-Steuerelement bietet eine große Menge von integrierten Funktionen. Sie enthält eine Reihe von unterschiedlichen Feldern für die Anzeige von Text, Bilder, links und Schaltflächen. Weitere Anpassung unterstützt Vorlagen. Mit wenigen Mausklicks es möglich, einer GridView-Ansicht vornehmen, in dem jede Zeile über die Schaltfläche ausgewählt werden kann, oder zu bearbeiten oder Löschen von Funktionen. Trotz der Fülle von Funktionen fallen häufig in Situationen, in denen zusätzliche, nicht unterstützte Funktionen hinzugefügt werden müssen. In diesem Tutorial und die nächsten beiden untersuchen wir die GridView-s-Funktionalität, um die folgende zusätzliche Features zu verbessern.

In diesem Tutorial und die nächste konzentrieren Sie sich an der Verbesserung des Zeilenauswahl Prozess. Wie in untersucht die [Master/Detail unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), wir können eine CommandField hinzufügen, an die GridView, die eine auswählen-Schaltfläche enthält. Beim Klicken auf ein Postback erfolgt und die GridView s `SelectedIndex` Eigenschaft wird aktualisiert, um den Index der Zeile, deren auswählen-Schaltfläche geklickt wurde. In der [Master/Detail unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Tutorial erläutert, wie dieses Feature zu verwenden, um die Details für die ausgewählte GridView-Zeile anzuzeigen.

Die auswählen-Schaltfläche in vielen Situationen funktioniert, zwar es funktioniert möglicherweise nicht als auch für andere Benutzer. Statt einer Schaltfläche zu verwenden, werden im Allgemeinen zwei andere Elemente der Benutzeroberfläche für die Auswahl verwendet: das Optionsfeld "sowie das Kontrollkästchen. Wir können GridView erweitern, sodass anstelle einer Schaltfläche auswählen, jede Zeile ein Optionsfeld oder ein Kontrollkästchen enthält. In Szenarien, in denen der Benutzer nur einen der GridView-Datensätze auswählen kann, kann das Optionsfeld "über die auswählen-Schaltfläche vorgezogen werden. In Situationen, in denen der Benutzer auswählen kann, bietet mehrere Datensätze wie z. B. in einer webbasierten e-Mail-Anwendung, in denen ein Benutzer mehrere Nachrichten So löschen Sie das Kontrollkästchen auswählen sollten Funktionen, die nicht über die Schaltfläche "auswählen" oder des Optionsfelds verfügbar ist. Benutzeroberflächen.

In diesem Tutorial dargestellt, wie eine Spalte mit Optionsfeldern an die GridView hinzufügen. Das Tutorial fortfahren beschreibt die Verwendung der Kontrollkästchen.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Schritt 1: Erstellen, das Verbessern der GridView-Webseiten

Bevor wir beginnen, Verbessern der GridView, um eine Spalte mit Optionsfeldern enthalten, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die nächsten beiden benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `EnhancedGridView`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Abbildung 1**: die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzufügen


Wie in den anderen Ordnern `Default.aspx` in die `EnhancedGridView` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Abschließend fügen Sie diese vier Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach den dem SqlDataSource-Steuerelement `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt die Einträge zum Verbessern der GridView-Lernprogramme](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Abbildung 3**: die Sitemap enthält Einträge, die jetzt zum Verbessern der GridView-Lernprogramme


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Schritt 2: Anzeigen der Lieferanten in einer GridView-Ansicht

Für dieses Tutorial kann s einer GridView-Ansicht zu erstellen, in der die Lieferanten aus den USA, in dem jede GridView-Zeile, die ein Optionsfeld Bereitstellung aufgeführt. Nach der Auswahl von eines Lieferanten über das Optionsfeld ", kann der Benutzer die Lieferanten-s-Produkte anzeigen, indem Sie auf eine Schaltfläche. Während dieser Aufgabe einfach, Sie klingt können gibt es eine Anzahl von Besonderheiten, die es besonders schwierig machen. Bevor wir in diese feinheiten beschäftigen, können Sie zunächst eine GridView, die die Lieferanten auflisten s an.

Öffnen Sie zunächst die `RadioButtonField.aspx` auf der Seite die `EnhancedGridView` Ordner von einer GridView-Ansicht aus der Toolbox in den Designer ziehen. Legen Sie die GridView s `ID` zu `Suppliers` und sein Smarttag, auswählen, um eine neue Datenquelle zu erstellen. Erstellen Sie eine mit dem Namen "ObjectDataSource" insbesondere `SuppliersDataSource` , die die Daten aus der bezieht die `SuppliersBLL` Objekt.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource "ObjectDataSource"](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Abbildung 4**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Abbildung 5**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `SuppliersBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Da wir nur die Lieferanten in den USA auflisten möchten, wählen Sie die `GetSuppliersByCountry(country)` Methode aus der Dropdown-Liste in der Registerkarte "SELECT".


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Abbildung 6**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `SuppliersBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


Auf der Registerkarte Updates wählen (keine) aus, und klicken Sie auf Weiter.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Abbildung 7**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `SuppliersBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Da die `GetSuppliersByCountry(country)` -Methode akzeptiert einen Parameter, der Konfigurieren von Datenquellen-Assistent fordert uns für die Quelle dieses Parameters. Zum Angeben ein hartcodierten Wert "USA, in diesem Beispiel", lassen Sie den Parameter Quelle Dropdown-Listenfeld auf None festgelegt, und geben Sie den Standardwert im Textfeld für die. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Verwenden von USA als den Standardwert für das Land-Parameter](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Abbildung 8**: Verwenden der USA als den Standardwert für die `country` Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Nach Abschluss des Assistenten, enthalten die GridView ein BoundField für die einzelnen Datenfelder, Lieferanten. Entfernen Sie alle außer den `CompanyName`, `City`, und `Country` BoundFields, und benennen Sie die `CompanyName` BoundFields `HeaderText` Eigenschaft an Lieferanten. Danach sollte die deklarative Syntax des GridView und "ObjectDataSource" etwa wie folgt aussehen.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

In diesem Tutorial können Sie s, die der Benutzer an den ausgewählten Lieferanten auf derselben Seite wie der Lieferantenliste oder auf einer anderen Seite s Produkte ermöglichen. Um dies zu unterstützen, fügen Sie zwei Websteuerelemente für die Schaltfläche auf der Seite hinzu. Ich Ve Satz der `ID` s zwei Schaltflächen `ListProducts` und `SendToProducts`, die Idee, die bei `ListProducts` geklickt wird ein Postback ausgeführt und die ausgewählten Lieferanten s-Produkte werden auf derselben Seite, wenn jedoch aufgelistet `SendToProducts` geklickt wird, der Benutzer wird zu einer anderen Seite whisked werden, die die Produkte aufgelistet werden.

Abbildung 9 zeigt die `Suppliers` GridView und die beiden Schaltfläche Web steuert, wenn Sie über einen Browser angezeigt.


[![Die Lieferanten aus den USA haben, ihren Namen, Stadt und Land Informationen aufgelistet](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Abbildung 9**: die Lieferanten aus den USA müssen deren Namen, Stadt und Land Informationen aufgelistet ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Schritt 3: Hinzufügen einer Spalte mit Optionsfeldern an.

An diesem Punkt die `Suppliers` GridView hat drei BoundFields den Firmennamen, Stadt und Land des einzelnen Lieferanten in den USA anzeigen. Es ist immer noch eine Spalte mit Optionsfeldern, jedoch nicht gegeben. Leider GridView t enthalten einen integrierten RadioButtonField, andernfalls wir können einfach hinzufügen, um dem Raster und ausgeführt werden. Stattdessen können wir ein TemplateField hinzufügen und Konfigurieren der `ItemTemplate` ein Optionsfeld, wodurch ein Optionsfeld für jede Zeile GridView zu rendern.

Zunächst kann davon ausgegangen, dass es sich bei die gewünschten Benutzeroberfläche implementiert werden kann, indem Sie ein RadioButton-Steuerelement, Hinzufügen der `ItemTemplate` des ein TemplateField. Obwohl dies tatsächlich ein einzelnes Optionsfeld an jede Zeile der GridView erhöht werden, wird die Optionsfelder nicht gruppiert werden und sind daher nicht gegenseitig. Ein Endbenutzer kann, also mehrere Optionsfelder GridView gleichzeitig aus.

Obwohl ein TemplateField von RadioButton-Websteuerelementen mit bietet nicht die Funktionalität, die wir benötigen, implementieren können s Dieser Ansatz, wie sie s sinnvoll sein, zu überprüfen, weshalb die resultierende Optionsfelder nicht gruppiert werden. Starten Sie durch ein TemplateField an die Lieferanten-GridView, sodass das Feld ganz links hinzufügen. Als Nächstes von GridView s Smarttags, klicken Sie auf den Link für die Vorlagen bearbeiten und die ein RadioButton-Steuerelement aus der Toolbox ziehen, in das TemplateField s `ItemTemplate` (siehe Abbildung 10). Legen Sie das RadioButton-s `ID` Eigenschaft `RowSelector` und `GroupName` Eigenschaft `SuppliersGroup`.


[![Fügen Sie ein RadioButton-Steuerelement für ItemTemplate verbleibt](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Abbildung 10**: Fügen Sie ein RadioButton-Steuerelement, das `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Treffen Sie diese Erweiterungen über den Designer, und sollte das GridView-s-Markup etwa wie folgt aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

Das RadioButton-s [ `GroupName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) wird verwendet, um eine Reihe von Optionsfeldern zu gruppieren. Alle RadioButton-Steuerelemente, mit dem gleichen `GroupName` Wert gelten gruppiert; nur ein Optionsfeld aus einer Gruppe zu einem Zeitpunkt ausgewählt werden kann. Die `GroupName` Eigenschaft gibt den Wert für das gerenderte Optionsfeld s `name` Attribut. Der Browser überprüft die Optionsfelder `name` Attribute, um zu bestimmen, den Sender Schaltfläche Gruppierungen.

Mit das RadioButton-Steuerelement hinzugefügt, die `ItemTemplate`, besuchen Sie diese Seite über einen Browser, und klicken Sie auf die Optionsfelder in der Zeilen im Raster s. Beachten Sie, wie die Optionsfelder nicht gruppiert werden, zeigt an, sodass alle Zeilen, wählen Sie als Abbildung 11.


[![Die Optionsfelder des GridView-s werden nicht gruppiert](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Abbildung 11**: Optionsfelder für die GridView s werden nicht gruppiert ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Die Optionsfelder werden nicht gruppiert ist, weil die gerenderten `name` Attribute unterscheiden sich trotz der Verwendung der gleichen `GroupName` Einstellung der Eigenschaft. Um diese Unterschiede anzuzeigen, führen Sie eine Ansicht/Source aus dem Browser, und überprüfen Sie das Optionsfeld Schaltfläche Markup:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Beachten Sie, dass sowohl die `name` und `id` Attribute sind nicht die genauen Werte wie angegeben in das Fenster "Eigenschaften", aber eine Reihe von anderen vorangestellt `ID` Werte. Die zusätzlichen `ID` Werte hinzugefügt, die in den Vordergrund des gerenderten `id` und `name` Attribute sind die `ID` s des Optionsfelds Schaltflächen übergeordnete Steuerelemente der `GridViewRow` s `ID` s, das GridView-s `ID`, wird die Inhaltssteuerelement s `ID`, und das Web Form-s `ID`. Diese `ID` s werden hinzugefügt, sodass jede gerendert Websteuerelement in der GridView hat eine eindeutige `id` und `name` Werte.

Jedes Steuerelement muss einen anderen gerendert `name` und `id` da es sich handelt, wie der Browser hat jedes Steuerelement auf der Clientseite und wie es an den Webserver welche Aktion identifiziert eindeutig identifiziert, oder erfolgte Änderungen beim Postback. Angenommen Sie, dass wir auch serverseitigen Code ausführen, wenn ein RadioButton-s überprüft, dass der Zustand geändert wurde. Wir können dies erreichen, indem Sie die RadioButton-s festgelegt `AutoPostBack` Eigenschaft `True` und erstellen einen Ereignishandler für die `CheckChanged` Ereignis. Aber wenn das gerenderte `name` und `id` Werte für alle Optionsfelder Waren dieselben, auf postback auf welcher konnte nicht ermittelt RadioButton geklickt wurde.

Kurz gesagt ist, dass wir eine Spalte mit Optionsfeldern in einem GridView, die über das RadioButton-Steuerelement erstellen können. Stattdessen müssen wir stattdessen veraltete Techniken verwenden, um sicherzustellen, dass das entsprechende Markup in jede GridView-Zeile eingefügt wird.

> [!NOTE]
> Wie das RadioButton-Steuerelement, das Optionsfeld "HTML-Steuerelement, wenn eine Vorlage hinzugefügt die eindeutige enthält `name` -Attribut, sodass die Optionsfelder im Raster nicht gruppierte. Wenn Sie nicht mit HTML-Steuerelementen vertraut sind, können Sie ignorieren diese Beachten Sie, wie Steuerelemente (HTML) nur selten, besonders in ASP.NET 2.0 verwendet werden. Aber wenn Sie weiteren interessiert sind, finden Sie unter [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s Blogeintrag [Websteuerelemente und Steuerelemente (HTML)](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Verwenden ein literales Steuerelement, Radio Schaltfläche Markup einzufügen.

Um alle Optionsfelder in der GridView ordnungsgemäß zu gruppieren, müssen wir das Optionsfeld Schaltflächen Markup in manuell einfügen der `ItemTemplate`. Jedes Optionsfeld erfordert gleich `name` Attribut, jedoch müssen Sie einen eindeutigen `id` -Attribut (für den Fall, dass wir ein Optionsfeld über Client-seitige Skript zugreifen möchten). Nachdem ein Benutzer wählt ein Optionsfeld aus, und Postback der Seite ausführt, sendet der Browser wird wieder den Wert des aktivierten Optionsfelds s `value` Attribut. Aus diesem Grund benötigen jedes Optionsfeld eine eindeutige `value` Attribut. Abschließend beim Postback wir müssen sicherstellen, dass Sie fügen die `checked` -Attribut, das ein Optionsfeld aus, die andernfalls, ausgewählt ist, nachdem der Benutzer eine Auswahl und Beiträge wieder hat, die Optionsfeldern zurück auf ihren Standardzustand zurückgesetzt (alle nicht ausgewählten).

Es gibt zwei Ansätze, die ausgeführt werden können, um Low-Level-Markup in einer Vorlage einfügen. Eine besteht darin, eine Mischung aus Markup und Aufrufen von Formatierungsmethoden, die in der CodeBehind-Klasse definiert werden. Diese Technik wurde zuerst diskutiert die [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Tutorial. In unserem Fall können sie wie folgt aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Hier `GetUniqueRadioButton` und `GetRadioButtonValue` wäre in der CodeBehind-Klasse, die die entsprechende zurückgegeben definierten Methoden `id` und `value` Attributwerte für jedes Optionsfeld. Dieser Ansatz eignet sich gut für die Zuweisung der `id` und `value` Attribute, jedoch unzureichend, wenn erforderlich, dass angeben der `checked` Attributwert, da die Datenbindungssyntax nur ausgeführt wird, wenn Daten zunächst an die GridView gebunden werden. Daher verfügt die GridView Ansichtszustand aktiviert, Formatierungsmethoden werden nur ausgelöst, wenn zuerst die Seite geladen wird (oder wenn die GridView ist explizit erneut gebunden an die Datenquelle), und daher die Funktion, die festlegt der `checked` Attribut gewonnen t aufgerufen werden Postback. Es s ein ziemlich spitzfindig Problem und sprengt den Rahmen in diesem Artikel, sodass ich diese beibehalten werden. Ich funktionieren, allerdings wird empfohlen, versuchen Sie es mit der oben beschriebene Ansatz und über zu dem Punkt, in dem Sie hängen bleiben müssen. Während solche ein Übung gewonnen t erhalten Sie auf eine funktionierende Version näher, hilft es, ein tieferes Verständnis der GridView und die Databinding-Lebenszyklus zu fördern.

Die andere Methode zum Einfügen von benutzerdefinierten Low-Level Markup in einer Vorlage und der Ansatz, den wir für dieses Tutorial verwenden ist, Hinzufügen einer [Literalsteuerelement](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) zur Vorlage. Klicken Sie dann in den GridView-s `RowCreated` oder `RowDataBound` -Ereignishandler das literale Steuerelement programmgesteuert zugegriffen werden kann und die zugehörige `Text` Eigenschaftensatz an das Markup zum ausgeben.

Durch das Entfernen der RadioButton aus den s TemplateField starten `ItemTemplate`, durch ein literales Steuerelement ersetzen. Festlegen des literalen Steuerelements s `ID` zu `RadioButtonMarkup`.


[![Fügen Sie ein literales Steuerelement für ItemTemplate verbleibt](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Abbildung 12**: Fügen Sie ein Literalsteuerelement auf der `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Als Nächstes erstellen Sie einen Ereignishandler für das GridView-s `RowCreated` Ereignis. Die `RowCreated` Ereignis wird ausgelöst, nachdem Sie für jede Zeile hinzugefügt wird, unabhängig davon, ob die Daten an die GridView neu gebunden werden werden. Dies bedeutet, dass selbst bei einem Postback beim erneuten die Daten aus dem Ansichtszustand laden, der `RowCreated` Ereignis weiterhin ausgelöst wird, und dies ist der Grund wir diesen anstelle von verwenden `RowDataBound` (die löst nur wenn die Daten explizit an den Daten-Websteuerelement gebunden ist).

In diesem Ereignishandler nur möchten wir fahren Sie fort, wenn wir erneut Umgang mit einer Datenzeile. Für jede Datenzeile wir programmgesteuert verweisen möchten die `RadioButtonMarkup` Literal-Steuerelement und legen dessen `Text` Eigenschaft, um das Markup zum ausgeben. Wie der folgende Code zeigt, erstellt das Markup ausgegeben, ein Optionsfeld, dessen Schaltfläche `name` -Attributsatz auf `SuppliersGroup`, dessen `id` -Attributsatz auf `RowSelectorX`, wobei *X* ist der Index der Zeile GridView und dessen `value` -Attribut auf den Index der Zeile GridView festgelegt ist.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Wenn eine GridView-Zeile ausgewählt ist, und ein Postback auftritt, sind wir interessiert die `SupplierID` des ausgewählten Lieferanten. Aus diesem Grund eine lässt zunächst vermuten, dass der Wert jedes Optionsfeld den tatsächlichen sein sollten `SupplierID` (anstatt den Index der Zeile GridView). Während dies unter bestimmten Umständen möglicherweise funktioniert, wäre es ein Sicherheitsrisiko dar, die blind akzeptiert und verarbeitet eine `SupplierID`. Unsere GridView, listet z. B. nur die Lieferanten in den USA. Jedoch, wenn der `SupplierID` übergeben wird, direkt aus dem Optionsfeld neuerungen beim Bearbeiten von einen schelmischen Benutzer beenden die `SupplierID` wieder beim Postback gesendeten Wert? Mithilfe der Index der Zeile als die `value`, und die `SupplierID` beim Postback aus der `DataKeys` -Auflistung, können wir sicherstellen, dass der Benutzer nur eines verwendet die `SupplierID` Werte, die eine der Zeilen GridView zugeordnet.

Nach dem Hinzufügen dieser Ereignishandlercode, kurz auf die Seite in einem Browser zu testen. Notieren Sie sich zunächst, nur ein Optionsfeld-Schaltfläche im Raster gleichzeitig ausgewählt werden kann. Allerdings auf, wenn ein Optionsfeld aus, und klicken Sie auf eine der Schaltflächen, um ein Postback auftritt und die Optionsfelder, die alle auf deren ursprünglichen Zustand wiederherstellen (d. h. beim Postback aktivierten Optionsfelds nicht mehr ausgewählt ist). Um dieses Problem zu beheben, müssen wir erweitern die `RowCreated` -Ereignishandler so, dass die It den aktivierten Optionsfelds Schaltflächenindex von des Postbacks gesendet überprüft, und fügt die `checked="checked"` Attribut das ausgegebene Markup des Index mit Zeile übereinstimmt.

Wenn ein Postback auftritt, sendet der Browser die `name` und `value` des ausgewählten Optionsfelds. Der Wert kann programmgesteuert mit abgerufen werden `Request.Form("name")`. Die [ `Request.Form` Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) bietet eine [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) , die die Formularvariablen darstellt. Die Formularvariablen sind die Namen und Werte der Formularfelder auf der Webseite, und es werden wieder vom Webbrowser gesendet, wenn ein Postback erfolgt. Da das gerenderte `name` der Optionsfelder in der GridView-Attribut ist `SuppliersGroup`, wenn die Webseite zurück gesendet wird, die der Browser sendet `SuppliersGroup=valueOfSelectedRadioButton` zurück an den Webserver (zusammen mit der anderer Felder des Formulars). Diese Informationen anschließend aus zugegriffen werden kann die `Request.Form` Eigenschaft: `Request.Form("SuppliersGroup")`.

Seit wir müssen um zu bestimmen, aktivierten Optionsfelds indizieren nicht nur in der `RowCreated` -Ereignishandler, aber in der `Click` -Ereignishandler für die Schaltfläche "-Web-Steuerelemente, Let s Hinzufügen einer `SuppliersSelectedIndex` -Eigenschaft für die Code-Behind-Klasse, die zurückgibt`-1`Wenn kein Optionsfeld ausgewählt wurde und der ausgewählte Index, wenn eines der Optionsfelder ausgewählt wird.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Mit dieser Eigenschaft hinzugefügt wird, wir wissen, dass das Hinzufügen der `checked="checked"` Markup in der `RowCreated` -Ereignishandler bei `SuppliersSelectedIndex` gleich `e.Row.RowIndex`. Aktualisieren Sie den Ereignishandler, um diese Logik einzuschließen:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Mit dieser Änderung bleibt nach einem Postback aktivierten Optionsfelds ausgewählt. Nun, wir die Möglichkeit haben, anzugeben, welche Optionsfeld ausgewählt ist, konnten wir das Verhalten ändern, sodass bei der zuerst die Seite besucht wurde, das erste Optionsfeld der GridView-Zeile s ausgewählt wurde (statt müssen keine Optionsfelder, die standardmäßig ausgewählt, dies ist die aktuelle Verhalten). Um das erste Optionsfeld in der Standardeinstellung ausgewählt haben, ändern Sie einfach die `If SuppliersSelectedIndex = e.Row.RowIndex Then` -Anweisung wie folgt: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

An diesem Punkt haben wir eine Spalte mit gruppierten Optionsfeldern an die GridView hinzugefügt, die für eine einzelne GridView-Zeile ausgewählt und über Postbacks hinweg gespeichert werden können. Unsere nächsten Schritte sind für die Produkte von den ausgewählten Lieferanten bereitgestellt. In Schritt 4 erfahren Sie, wie zum Umleiten des Benutzers zu einer anderen Seite, auf dem ausgewählten senden `SupplierID`. In Schritt 5 sehen wir, wie die ausgewählten Lieferanten-s-Produkte in einer GridView-Ansicht auf derselben Seite angezeigt werden.

> [!NOTE]
> Statt ein TemplateField (Schwerpunkt dieser langen Schritt 3) zu verwenden, konnten wir erstellen eine benutzerdefinierte `DataControlField` -Klasse, die die entsprechende Benutzeroberfläche und Funktionalität rendert. Die [ `DataControlField` Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) ist die Basisklasse, von denen die BoundField-, CheckBoxField, TemplateField und andere integrierte GridView und DetailsView-Felder abgeleitet werden. Erstellen einer benutzerdefinierten `DataControlField` Klasse würde bedeuten, dass die Spalte mit Optionsfeldern konnte nur mit deklarativer Syntax hinzugefügt werden und würde auch das Replizieren der Funktionalität für andere Webseiten und anderen Webanwendungen erheblich einfacher.


Wenn haben Sie jemals erstellten benutzerdefinierten, kompiliert die Steuerelemente in ASP.NET, allerdings wissen Sie, dass dies so viel Arbeit erfordert und bringt mit sich, eine Vielzahl von feinheiten und Grenzfälle, die sorgfältig behandelt werden müssen. Aus diesem Grund werden wir verzichten, implementieren eine Spalte mit Optionsfeldern als benutzerdefinierte `DataControlField` vorerst Klasse, und bleiben Sie mit der TemplateField-Option. Vielleicht haben wir dann die Möglichkeit, erkunden, erstellen, verwenden und Bereitstellen von benutzerdefinierten `DataControlField` Klassen in einem späteren Tutorial!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Schritt 4: Die ausgewählte Lieferanten-s-Produkte in einer separaten Seite anzeigen

Nachdem der Benutzer eine GridView-Zeile ausgewählt hat, müssen wir den ausgewählten Lieferanten s-Produkte angezeigt. In einigen Fällen sollten wir diese Produkte in einer separaten Seite, in anderen zeigen wir bevorzugen möglicherweise auf der gleichen Seite. Lassen Sie s, prüfen Sie daher zunächst wie die Produkte in einer separaten Seite angezeigt; in Schritt 5 betrachten wir Hinzufügen einer GridView-Ansicht zu `RadioButtonField.aspx` für die ausgewählte Lieferanten s Produkte.

Derzeit sind zwei Schaltfläche-Web-Steuerelemente auf der Seite `ListProducts` und `SendToProducts`. Wenn die `SendToProducts` -Schaltfläche geklickt wird, möchten wir den Benutzer senden `~/Filtering/ProductsForSupplierDetails.aspx`. Diese Seite wurde erstellt, der [Anzeigen von Filtern von Master-/Detailberichten über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) Tutorial und zeigt die Produkte für den Lieferanten, deren `SupplierID` übergeben wird, über das Querystring-Feld, das mit dem Namen `SupplierID`.

Um diese Funktionalität zu ermöglichen, erstellen Sie einen Ereignishandler für die `SendToProducts` Schaltfläche s `Click` Ereignis. In Schritt 3 wir hinzugefügt, die `SuppliersSelectedIndex` -Eigenschaft, die den Index der Zeile, deren Optionsfeld zurückgibt ausgewählt ist. Das entsprechende `SupplierID` abgerufen werden können, von der GridView-s `DataKeys` Sammlung und der Benutzer können dann an gesendet werden `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` mit `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Dieser Code funktioniert einwandfrei, solange eines der Optionsfelder aus der GridView ausgewählt wird. Wenn Anfangs GridView verfügt nicht über alle Optionsfelder aktiviert, und der Benutzer klickt auf die `SendToProducts` Schaltfläche `SuppliersSelectedIndex` werden `-1`, wodurch eine Ausnahme ausgelöst werden, da `-1` liegt außerhalb des Indexbereichs, der die `DataKeys`Auflistung. Dies ist nicht wichtig ist, jedoch, wenn Sie sich entschieden, aktualisieren Sie die `RowCreated` Ereignishandler wie unter Schritt 3, um das erste Optionsfeld in der GridView, die ursprünglich ausgewählt haben.

Um zu ermöglichen eine `SuppliersSelectedIndex` Wert `-1`, fügen Sie ein Label-Steuerelement auf der Seite über die GridView. Legen Sie seine `ID` Eigenschaft, um `ChooseSupplierMsg`, dessen `CssClass` Eigenschaft, um `Warning`, dessen `EnableViewState` und `Visible` Eigenschaften, die `False`, und die zugehörige `Text` Eigenschaft auf einen Lieferanten zu wählen, aus dem Raster. Die CSS-Klasse `Warning` zeigt Text in Rot, kursiv, fett, große Schriftart und wird in definiert `Styles.css`. Durch Festlegen der `EnableViewState` und `Visible` Eigenschaften `False`, die Bezeichnung wird nicht für jene postbacks Where außer gerendert das Steuerelement s `Visible` -Eigenschaftensatz programmgesteuert auf `True`.


[![Fügen Sie ein Label-Websteuerelement über GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Abbildung 13**: Hinzufügen einer Bezeichnung Web-Steuerelement über das GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Erweitern Sie als Nächstes die `Click` -Ereignishandler zum Anzeigen der `ChooseSupplierMsg` sofern `SuppliersSelectedIndex` ist kleiner als 0 (null), und leiten Sie den Benutzer zu `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` andernfalls.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Besuchen Sie die Seite in einen Browser, und klicken Sie auf die `SendToProducts` Schaltfläche vor der Auswahl von eines Lieferanten von GridView. Wie in Abbildung 14 gezeigt, zeigt diese an die `ChooseSupplierMsg` Bezeichnung. Als nächstes wählen Sie einen Lieferanten, und klicken Sie auf die `SendToProducts` Schaltfläche. Dadurch werden Sie zu einer Seite weiter, die die Produkte von den ausgewählten Lieferanten auflistet. Abbildung 15 zeigt die `ProductsForSupplierDetails.aspx` Seite, wenn der Lieferant Bigfoot Brauereien ausgewählt wurde.


[![Die Bezeichnung ChooseSupplierMsg wird angezeigt, wenn kein Anbieter ausgewählt ist](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Abbildung 14**: die `ChooseSupplierMsg` Bezeichnung wird angezeigt, wenn kein Anbieter ausgewählt ist ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Die ausgewählten Lieferanten-s-Produkte werden in ProductsForSupplierDetails.aspx angezeigt.](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Abbildung 15**: die ausgewählte Lieferanten s Produkte werden angezeigt, `ProductsForSupplierDetails.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Schritt 5: Die ausgewählte Lieferanten-s-Produkte auf derselben Seite anzeigen

In Schritt 4 wurde erläutert, wie der Benutzer zu einer anderen Webseite zum Anzeigen des ausgewählten Lieferanten s Produkte zu senden. Alternativ können die ausgewählten Lieferanten-s-Produkte auf derselben Seite angezeigt werden. Um dies zu veranschaulichen, fügen wir eine andere GridView zu `RadioButtonField.aspx` für die ausgewählte Lieferanten s Produkte.

Da nur soll diese GridView-Produkte angezeigt, nachdem ein Anbieter ausgewählt wurde, fügen Sie ein Panel-Steuerelement unterhalb der `Suppliers` GridView, Festlegen der `ID` zu `ProductsBySupplierPanel` und dessen `Visible` Eigenschaft, um `False`. Innerhalb des Bereichs, fügen Sie den Text Produkte für den Lieferanten ausgewählt haben, gefolgt von einer GridView-Ansicht mit dem Namen `ProductsBySupplier`. Das GridView s Smarttag auswählen, um die Bindung an eine neue, mit dem Namen "ObjectDataSource" `ProductsBySupplierDataSource`.


[![ProductsBySupplier GridView zu binden, um eine neue "ObjectDataSource"](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Abbildung 16**: Binden der `ProductsBySupplier` GridView, um eine neue "ObjectDataSource" ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Als Nächstes konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse. Da wir nur die Produkte von den ausgewählten Lieferanten abrufen möchten, geben Sie, dass es sich bei dem ObjectDataSource-Steuerelement aufgerufen werden soll die `GetProductsBySupplierID(supplierID)` Methode, um die Daten abzurufen. Wählen Sie (keine) aus den Dropdown-Listen in den Update-, INSERT- und Löschen von Registerkarten.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetProductsBySupplierID(supplierID)-Methode](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Abbildung 17**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Legen Sie die Dropdownlisten auf (keine) in der Update-, INSERT- und Löschen von Registerkarten](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Abbildung 18**: Legen Sie das Dropdown-Listen auf (keine) in der Update-, INSERT- und Löschen von Registerkarten ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Nach dem konfigurieren die SELECT-Anweisung, aktualisieren, einfügen, und Löschen von Registerkarten, klicken Sie auf Weiter. Da die `GetProductsBySupplierID(supplierID)` Methode erwartet einen Eingabeparameter, der Erstellen von Datenquellen-Assistenten verlangt, um die Quelle für die s-Parameterwert anzugeben.

Wir haben eine Reihe von Optionen, die hier in an, die Quelle des s-Wert des Parameters. Wir könnten das Standardobjekt für den Parameter verwenden und programmgesteuert weisen den Wert für die `SuppliersSelectedIndex` Eigenschaft, um die Parameter-s `DefaultValue` -Eigenschaft in der "ObjectDataSource"-s `Selecting` -Ereignishandler. Siehe die [Programmgesteuertes Festlegen der Parameterwerte der ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) Tutorial zu auffrischen programmgesteuert Zuweisen von Werten an die "ObjectDataSource"-s-Parameter.

Alternativ können wir verwenden eine ControlParameter und finden Sie in der `Suppliers` GridView s [ `SelectedValue` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (siehe Abbildung 19). Das GridView-s `SelectedValue` -Eigenschaft gibt die `DataKey` Wert entspricht der [ `SelectedIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Damit kann für diese Option funktioniert, müssen wir die GridView s programmgesteuert festlegen `SelectedIndex` Eigenschaft, die dem ausgewählten Datenzeile, wenn die `ListProducts` geklickt wird. Als zusätzlichen Vorteil, durch Festlegen der `SelectedIndex`, der ausgewählte Datensatz treten am die `SelectedRowStyle` in definiert die `DataWebControls` Design (einen gelben Hintergrund).


[![Verwenden Sie eine ControlParameter an die GridView-s-SelectedValue als Parameterquelle](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Abbildung 19**: Verwenden Sie eine ControlParameter an die GridView s SelectedValue als Quelle-Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Nach Abschluss des Assistenten wird in Visual Studio automatisch Felder für die Product-s-Datenfelder hinzugefügt. Entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields, und ändern Sie die `HeaderText` Eigenschaften, die Produkte, Kategorie und Preis. Konfigurieren der `UnitPrice` BoundField, damit der Wert als Währung formatiert ist. Nach diesen Änderungen sollte Bereich GridView und "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Um in dieser Übung abgeschlossen haben, müssen wir die GridView s festgelegt `SelectedIndex` Eigenschaft, um die `SelectedSuppliersIndex` und die `ProductsBySupplierPanel` Bereich s `Visible` Eigenschaft `True` bei der `ListProducts` geklickt wird. Um dies zu erreichen, erstellen Sie einen Ereignishandler für die `ListProducts` Schaltfläche Websteuerelement s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Wenn ein Anbieter von GridView nicht ausgewählt wurde die `ChooseSupplierMsg` Bezeichnung wird angezeigt, und die `ProductsBySupplierPanel` Bereich ausgeblendet. Andernfalls, wenn ein Anbieter ausgewählt wurde, die `ProductsBySupplierPanel` angezeigt wird und der GridView-s `SelectedIndex` Eigenschaft aktualisiert wird.

Abbildung 20 zeigt die Ergebnisse nach dem der Lieferant Bigfoot Brauereien ausgewählt wurde und die Produkte anzeigen, auf der Seite-Schaltfläche geklickt wurde.


[![Die Produkte vom Bigfoot Brauereien sind auf derselben Seite aufgeführt](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Abbildung 20**: auf derselben Seite aufgeführt sind die Produkte von Bigfoot Brauereien bereitgestellt ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

Siehe die [Master/Detail unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Tutorial Datensätze aus einer GridView-Ansicht mit einem CommandField ausgewählt werden können, deren `ShowSelectButton` -Eigenschaftensatz auf `True`. Aber die CommandField werden die Schaltflächen, entweder als reguläre Schaltflächen, Links oder Bilder. Eine alternative Zeilenauswahl-Benutzeroberfläche ist ein Optionsfeld oder die Kontrollkästchen in jeder Zeile GridView angeben. In diesem Tutorial untersucht wir, wie Sie eine Spalte mit Optionsfeldern hinzufügen.

Leider werden eine Spalte vom Radio Schaltflächen ist t als unkompliziert und einfach hinzufügen, wie zu erwarten wäre. Es gibt keine integrierte RadioButtonField, die beim Klicken auf eine Schaltfläche hinzugefügt werden kann, und verwenden das RadioButton-Steuerelement in ein TemplateField führt einen eigenen Satz von Probleme. Schließlich geben Sie eine solche Schnittstelle entweder haben wir eine benutzerdefinierte `DataControlField` Klasse oder die Möglichkeit zum Einfügen von des entsprechenden HTML-Code in ein TemplateField während der `RowCreated` Ereignis.

Müssen haben, wie Sie eine Spalte mit Optionsfeldern, hinzufügen, können wir unsere Aufmerksamkeit auf das Hinzufügen einer Spalteninhalts mit Kontrollkästchen zu aktivieren. Ein Benutzer kann mit einer Spalte mit Kontrollkästchen wählen Sie eine oder mehrere Zeilen von GridView und führen Sie dann einige Vorgänge für alle ausgewählten Zeilen (z. B. eine Gruppe von e-Mails aus einem webbasierten e-Mail-Client auswählen, und wählen Sie dann So löschen Sie alle ausgewählten e-Mail-Nachrichten). Im nächsten Tutorial sehen wir, wie Sie eine solche Spalte hinzufügen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist David Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Weiter](adding-a-gridview-column-of-checkboxes-vb.md)
