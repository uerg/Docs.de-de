---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: "Hinzufügen einer GridView-Spalte von Optionsfeldern (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm befasst sich mit eine Spalte von Optionsfeldern GridView-Steuerelements, um Benutzern eine intuitivere Möglichkeit der Auswahl einer einzelnen Zeile der hinzufügen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 386fcb1cef896edbb465ba36415712af70d916ec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Hinzufügen einer GridView-Spalte von Optionsfeldern (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) oder [PDF herunterladen](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> In diesem Lernprogramm untersucht, wie eine GridView-Steuerelements, um Benutzern eine intuitivere Möglichkeit, eine einzelne Zeile mit GridView Auswählen einer Spalte von Optionsfeldern hinzu.


## <a name="introduction"></a>Einführung

GridView-Steuerelements bietet zahlreiche integrierte Funktionen. Die Software umfasst einer Reihe verschiedener Felder für die Anzeige von Text, Bilder, links und Schaltflächen. Vorlagen unterstützt zur weiteren Anpassung. Mit wenigen Mausklicks es s möglich, damit eine GridView, in dem jede Zeile über eine Schaltfläche ausgewählt werden kann, oder bearbeiten oder Löschen von Funktionen zu aktivieren. Trotz der Fülle von bereitgestellten Funktionen sind oft Situationen, in denen zusätzliche, nicht unterstützte Funktionen hinzugefügt werden müssen. In diesem Lernprogramm und die nächsten beiden untersuchen wir wie um die GridView-s-Funktionalität, um zusätzliche Funktionen gehören zu verbessern.

Dieses Lernprogramm und die nächste darauf konzentrieren, erweitern die Zeile Auswahlprozesses verwendet werden. Wie überprüft der [Master/Detail mit auswählbaren Master GridView mit einer Details-DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), wir können einen CommandField hinzufügen, an die GridView, die eine Schaltfläche auswählen. Beim Klicken auf ein Postback erfolgt und die GridView s `SelectedIndex` Eigenschaft wird auf den Index der Zeile, deren auswählen geklickt wurde, aktualisiert. In der [Master/Detail mit auswählbaren Master GridView mit einer Details-DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Lernprogramm wurde erläutert, wie diese Funktion verwenden, um die Details für die ausgewählte GridView Zeile anzuzeigen.

Die Schaltfläche auswählen in vielen Fällen funktioniert, zwar funktionieren es auch für andere nicht. Statt eine Schaltfläche zu verwenden, werden häufig zwei andere Elemente einer Benutzeroberfläche zur Auswahl verwendet: das Optionsfeld und Kontrollkästchen. Wir können die GridView erweitern, sodass anstelle einer Schaltfläche auswählen, jede Zeile ein Optionsfeld oder Kontrollkästchen enthält. In Szenarien, in denen der Benutzer nur einen der Datensätze GridView auswählen kann, kann das Optionsfeld über die Schaltfläche auswählen bevorzugte befinden. In Situationen, in denen der Benutzer möglicherweise auswählen kann, mehrere Datensätze wie z. B. in ein webbasiertes e-Mail-Anwendung, bei dem ein Benutzer mehrere Nachrichten So löschen Sie das Kontrollkästchen aktivieren möchten bietet Funktionen, die nicht über die Schaltfläche auswählen oder des Optionsfelds verfügbar ist Benutzeroberflächen.

In diesem Lernprogramm untersucht, wie eine Spalte von Optionsfeldern an die GridView hinzugefügt. Das Lernprogramm fortfahren untersucht die Verwendung von Kontrollkästchen.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Schritt 1: Erstellen von verbessern die GridView-Webseiten

Bevor wir beginnen, verbessern die GridView um eine Spalte von Optionsfeldern enthalten, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und die nächsten beiden benötigen. Starten, indem Sie einen neuen Ordner namens `EnhancedGridView`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu


Wie in den anderen Ordnern `Default.aspx` in den `EnhancedGridView` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


Abschließend fügen Sie diese vier Seiten als Einträge an die `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Using SqlDataSource-Steuerelement `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für die Bearbeitung, einfügen und Löschen von Lernprogramme.


![Die Siteübersicht enthält nun die Einträge zum Verbessern der GridView-Lernprogramme](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge zum Verbessern der GridView-Lernprogramme


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Schritt 2: Anzeigen von Lieferanten in einem GridView

Für dieses Lernprogramm kann s eine GridView erstellen, in der die Lieferanten aus den USA bietet ein Optionsfeld GridView Zeilen aufgelistet. Nach der Auswahl von eines anderen Lieferanten über die Schaltfläche "Radio", kann der Benutzer die Lieferanten-s-Produkte anzeigen, durch Klicken auf eine Schaltfläche. Während dieser Aufgabe triviale klingt, gibt es eine Anzahl von Besonderheiten, die besonders schwierig zu vereinfachen. Bevor wir in diesen Besonderheiten ausreicht, können Sie s erhalten zunächst eine GridView Lieferanten auflisten.

Öffnen Sie zunächst die `RadioButtonField.aspx` auf der Seite der `EnhancedGridView` Ordner durch eine GridView aus der Toolbox in den Designer ziehen. Legen Sie die GridView s `ID` auf `Suppliers` , und wählen Sie das Smarttag, um eine neue Datenquelle zu erstellen. Insbesondere erstellen Sie ein mit dem Namen ObjectDataSource `SuppliersDataSource` , die zugehörigen Daten aus zieht die `SuppliersBLL` Objekt.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Abbildung 4**: Erstellen einer neuen ObjectDataSource namens `SuppliersDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Abbildung 5**: Konfigurieren der ObjectDataSource verwenden die `SuppliersBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Da wir nur die Lieferanten in den USA aufführen möchten, wählen Sie die `GetSuppliersByCountry(country)` Methode aus der Dropdown-Liste auf der Registerkarte "SELECT".


[![Konfigurieren der ObjectDataSource zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Abbildung 6**: Konfigurieren der ObjectDataSource verwenden die `SuppliersBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


Auf der Registerkarte UPDATE wählen (keine) aus, und klicken Sie auf Weiter.


[![Konfigurieren der ObjectDataSource zur Verwendung der SuppliersBLL-Klasse](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Abbildung 7**: Konfigurieren der ObjectDataSource verwenden die `SuppliersBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Da die `GetSuppliersByCountry(country)` Methode akzeptiert einen Parameter, der Datenquelle konfigurieren-Assistent fordert Sie uns für die Quelle des Parameters. Zum Angeben ein hartcodierten Wert (in diesem Beispiel USA), lassen Sie den Parameter Quelle Dropdown-Liste auf None festgelegt, und geben Sie den Standardwert im Textfeld. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Verwenden Sie die USA als Standard für das Land Parameter](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Abbildung 8**: Verwenden der USA als Standardwert für die `country` Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


Nach Abschluss des Assistenten wird die GridView ein BoundField für jeden Lieferanten Datenfelder enthalten. Entfernen Sie alle außer den `CompanyName`, `City`, und `Country` BoundFields, und benennen Sie die `CompanyName` BoundFields `HeaderText` Eigenschaft an Lieferanten. Nachdem Sie auf diese Weise sollte die GridView und ObjectDataSource deklarative Syntax ähnlich der folgenden aussehen.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Können Sie für dieses Lernprogramm s, die der Benutzer an den ausgewählten Lieferanten s Produkte auf derselben Seite wie die Lieferantenliste oder auf eine andere Seite zulassen. Um dies zu berücksichtigen, fügen Sie zwei Schaltfläche Web-Steuerelemente auf der Seite "ein. Ich Ve Satz der `ID` s zwei Schaltflächen `ListProducts` und `SendToProducts`, mit dem Konzept, dass bei `ListProducts` geklickt wird ein Postback auftreten wird und die ausgewählten Lieferanten s werden auf der gleichen Seite; Wenn jedoch aufgeführt `SendToProducts` geklickt wird, der Benutzer wird zu einer anderen Seite whisked sein, der die Produkte aufgeführt sind.

Abbildung 9 zeigt die `Suppliers` GridView und zwei Webserver eine Schaltfläche gesteuert werden, wenn Sie über einen Browser angezeigt.


[![Diese Lieferanten aus den USA haben, deren Name, City und Länderinformationen aufgeführt](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Abbildung 9**: die Lieferanten von USA haben ihre Namen, Stadt und Land Informationen aufgelistet ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Schritt 3: Hinzufügen einer Spalte von Optionsfeldern

An diesem Punkt der `Suppliers` GridView verfügt über drei BoundFields den Firmennamen, Stadt und Land des einzelnen Lieferanten in den USA anzeigen. Es ist immer noch eine Spalte von Optionsfeldern, jedoch nicht gegeben. Leider enthalten t GridView verfügt über eine integrierte RadioButtonField, andernfalls wir konnten nur hinzugefügt werden, die im Raster und ausgeführt werden. Stattdessen kann ein TemplateField hinzufügen und Konfigurieren der `ItemTemplate` ein Optionsfeld, wodurch ein Optionsfeld für jede Zeile GridView gerendert.

Zu Beginn kann davon ausgegangen, dass es sich bei die gewünschten Benutzeroberfläche implementiert werden kann, durch das Hinzufügen eines Steuerelements "RadioButton" Web zu den `ItemTemplate` des ein TemplateField. Obwohl dadurch tatsächlich ein einzelnes Optionsfeld an jede Zeile der GridView hinzugefügt wird, wird die Optionsfelder nicht gruppiert werden und sind daher nicht gegenseitig. Ein Endbenutzer ist also mehrere Optionsfelder aus der GridView gleichzeitig auswählen.

Obwohl ein TemplateField "RadioButton" Websteuerelemente mit bietet nicht die Funktionalität, die wir benötigen, implementieren Let s Dieser Ansatz, wie er s lohnende zu überprüfen, weshalb die resultierende Optionsfelder nicht gruppiert werden. Starten Sie, indem ein TemplateField an den Lieferanten GridView, somit das am weitesten links stehende Feld hinzufügen. Als Nächstes den GridView s smart Tag, klicken Sie auf den Link Vorlagen bearbeiten und ziehen Sie ein Websteuerelement "RadioButton" aus der Toolbox in den TemplateField s `ItemTemplate` (siehe Abbildung 10). Legen Sie die "RadioButton" s `ID` Eigenschaft `RowSelector` und die `GroupName` Eigenschaft `SuppliersGroup`.


[![Hinzufügen eines "RadioButton" Web-Steuerelements zu ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Abbildung 10**: Fügen Sie ein Websteuerelement "RadioButton", um die `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Nach dem Herstellen dieser Ergänzungen durch den Designer, sollte Ihre s GridView-Markup etwa wie folgt aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

Die "RadioButton" s [ `GroupName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) wird verwendet, um eine Reihe von Optionsfeldern zu gruppieren. Alle RadioButton-Steuerelemente mit dem gleichen `GroupName` Wert gelten gruppiert; nur ein Optionsfeld aus einer Gruppe zu einem Zeitpunkt ausgewählt werden kann. Die `GroupName` Eigenschaft gibt den Wert für das gerenderte Optionsfeld s `name` Attribut. Der Browser überprüft die Optionsfelder `name` Attribute bestimmen des entsprechenden Optionsfelds Schaltfläche Gruppierungen.

Mit dem "RadioButton" Web-Steuerelement hinzugefügt, um die `ItemTemplate`, besuchen Sie diese Seite über einen Browser, und klicken Sie auf die Optionsfelder in Zeilen des Rasters s. Beachten Sie, wie die Optionsfelder nicht gruppiert werden, zeigt an, wodurch alle Zeilen, als abbildung11 auswählen.


[![Die GridView s Optionsfelder werden nicht gruppiert](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Abbildung 11**: Optionsfelder für die GridView s werden nicht gruppiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Der Grund, die die Optionsfelder werden nicht gruppiert ist, da ihre gerenderten `name` Attribute unterschiedlich sind, obwohl er besitzt die gleiche `GroupName` Einstellung der Eigenschaft. Um diese Unterschiede anzuzeigen, führen Sie eine Ansicht/der Quelle über den Browser, und überprüfen Sie das Optionsfeld Schaltfläche Markup:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Beachten Sie, dass wie die `name` und `id` Attribute sind nicht die genauen Werte entsprechend den Angaben im Eigenschaftenfenster angezeigt, jedoch mit einer Anzahl anderer vorangestellt `ID` Werte. Die zusätzlichen `ID` Werte hinzugefügt, die in den Vordergrund des gerenderten `id` und `name` Attribute sind die `ID` s des Optionsfelds Schaltflächen übergeordneter Steuerelemente der `GridViewRow` s `ID` e, s GridView `ID`, die Inhaltssteuerelement s `ID`, und das Web Form s `ID`. Diese `ID` s werden hinzugefügt, damit jeweils gerendert Websteuerelement in der GridView verfügt über einen eindeutigen `id` und `name` Werte.

Jedes Steuerelement muss ein anderes gerendert `name` und `id` da sieht wie der Browser eindeutig identifiziert jedes Steuerelement auf der Clientseite und wie er an den Webserver Maßnahme identifiziert oder geändert wurden beim Postback. Angenommen Sie, wir möchten, serverseitigen Code auszuführen, wenn ein "RadioButton" s überprüft, dass der Status geändert wurde. Wir konnten Sie dazu durch Festlegen der "RadioButton" s `AutoPostBack` Eigenschaft `true` und erstellen einen Ereignishandler für die `CheckChanged` Ereignis. Jedoch, wenn das gerenderte `name` und `id` Werte für alle Optionsfelder übereinstimmen, wurden auf postback welche spezifische konnte nicht ermittelt "RadioButton" geklickt wurde.

Der kurze davon ist, dass wir eine Spalte von Optionsfeldern in einem GridView, die über das RadioButton-Steuerelement erstellen können. Stattdessen müssen wir stattdessen veraltete Techniken verwenden, um sicherzustellen, dass das entsprechende Markup in jeder Zeile GridView eingeschleust wird.

> [!NOTE]
> Wie das RadioButton-Websteuerelement, das Optionsfeld HTML-Steuerelement, wenn eine Vorlage hinzugefügt eindeutigen enthalten `name` -Attribut, die Optionsfelder im Raster Gruppierung vornehmen. Wenn Sie nicht mit HTML-Steuerelementen vertraut sind, können Sie ignorieren dieser Hinweis als HTML-Steuerelementen selten, insbesondere in ASP.NET 2.0 verwendet werden. Aber wenn Sie mehr erfahren möchten, finden Sie unter [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s Blogeintrag [Websteuerelementen und HTML-Steuerelementen](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Verwenden ein Literal Steuerelement Radio Schaltfläche Markup einfügen

Um alle die Optionsfelder innerhalb der GridView ordnungsgemäß zu gruppieren, müssen wir manuell einfügen von Schaltflächen Optionsfeldgruppe Markup in der `ItemTemplate`. Jedes Optionsfeld erfordert die gleiche `name` Attribut, aber eine eindeutige haben sollten `id` -Attribut (für den Fall, dass wir ein Optionsfeld über clientseitiges Skript zugreifen möchten). Nachdem ein Benutzer wählt ein Optionsfeld aus, und Beiträge wieder die Seite, der Browser sendet wieder den Wert des aktivierten Optionsfelds s `value` Attribut. Aus diesem Grund benötigen jedes Optionsfeld eine eindeutige `value` Attribut. Schließlich beim Postback müssen wir sicherstellen, dass Hinzufügen der `checked` -Attribut, das ein Optionsfeld aus, die andernfalls, ausgewählt ist, wenn der Benutzer eine Auswahl auch Pfosten wieder hergestellt, die Optionsfelder zurück auf ihren Standardzustand zurückgesetzt (alle nicht ausgewählten).

Es gibt zwei Ansätze, die ausgeführt werden können, um die Low-Level-Markup in einer Vorlage einfügen. Eine besteht darin, eine Mischung von Markup und Aufrufe an die Formatierung von Methoden, die in der Code-Behind-Klasse definiert werden. Diese Technik wurde zuerst im erläutert die [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Lernprogramm. In unserem Fall kann es etwa folgendermaßen aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Hier `GetUniqueRadioButton` und `GetRadioButtonValue` wäre in der Code-Behind-Klasse, die die entsprechende zurückgegeben definierten Methoden `id` und `value` Attributwerte für jedes Optionsfeld. Dieser Ansatz funktioniert gut für das Zuweisen von der `id` und `value` Attribute, kurze greift, wenn angeben müssen jedoch die `checked` -Attributwert aus, da die Databinding-Syntax nur ausgeführt wird, wenn Daten zunächst an die GridView gebunden ist. Daher verfügt die GridView Ansichtszustand aktiviert, Formatierungsmethoden werden nur ausgelöst, wenn die Seite geladen ist (oder wenn GridView explizit an die Datenquelle gebunden ist), und daher die Funktion, die festlegt der `checked` Attribut/gewonnen t aufgerufen werden Postback. Es s vielmehr feine Problem und etwas über den Rahmen dieses Artikels, damit ich sie zurzeit lassen. Ich, jedoch empfehlen Ihnen, versuchen Sie es mit der oben beschriebenen Herangehensweise funktionieren und über zu dem Punkt, in dem Sie hängen bleiben müssen. Während solche eine Übung/gewonnen t erhalten Sie alle näher zu einer funktionierenden Version, hilft es ein tieferes Verständnis über das Databinding-Lebenszyklus und die GridView zu fördern.

Die anderen nicht linearen Ansatz einzuschleusen benutzerdefinierte, Low-Level-Markup in einer Vorlage und der Ansatz, die wir für dieses Lernprogramm verwenden ist, Hinzufügen einer [Literalsteuerelement](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) der Vorlage. Klicken Sie auf die GridView s `RowCreated` oder `RowDataBound` Ereignishandler, d. h. Literal-Steuerelement programmgesteuert zugegriffen werden kann und dessen `Text` -Eigenschaft, die zum Ausgeben von auf Markup festgelegt.

Starten, indem Sie "RadioButton" aus den s TemplateField `ItemTemplate`, ersetzen es mit einem Literal-Steuerelement. Legen Sie das Literal Steuerelement s `ID` auf `RadioButtonMarkup`.


[![Hinzufügen eines literalen Steuerelements zu ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Abbildung 12**: Fügen Sie ein Literal-Steuerelement auf die `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Als Nächstes erstellen Sie einen Ereignishandler für die GridView s `RowCreated` Ereignis. Die `RowCreated` Ereignis wird ausgelöst, sobald Sie für jede hinzugefügte Zeile fest, ob die Daten an die GridView gebunden wird ist. Dies bedeutet, dass selbst bei einem Postback beim erneuten der Daten aus dem Ansichtszustand laden, der `RowCreated` Ereignis weiterhin ausgelöst wird, und dies ist der Grund wir ihn statt des verwenden `RowDataBound` (dem auslöst, nur wenn die Daten explizit an den Daten-Websteuerelement gebunden ist).

In diesem Ereignishandler wir nur wenn fortfahren möchten wir re Umgang mit einer Datenzeile. Für jede Datenzeile wir programmgesteuert verweisen möchten die `RadioButtonMarkup` Literal-Steuerelement, und legen dessen `Text` Eigenschaft, um das Markup zum ausgeben. Wie der folgende Code zeigt, erstellt das Markup ausgegeben, ein Optionsfeld, dessen Schaltfläche `name` -Attributsatz zur `SuppliersGroup`, deren `id` -Attributsatz zur `RowSelectorX`, wobei *X* ist der Index der Zeile GridView und dessen `value` -Attribut auf den Index der Zeile GridView festgelegt ist.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Wenn eine GridView Zeile ausgewählt ist, und ein Postback kommt, interessieren uns die `SupplierID` des ausgewählten Lieferanten. Können daher vorstellen, dass der Wert für jedes Optionsfeld den tatsächlichen liegen `SupplierID` (anstatt den Index der Zeile GridView). Während dies möglicherweise unter bestimmten Umständen funktioniert, wäre es ein Sicherheitsrisiko Blind akzeptieren und Verarbeiten einer `SupplierID`. Unsere GridView listet z. B. nur die Lieferanten in den USA. Jedoch, wenn die `SupplierID` übergeben wird, direkt über das Optionsfeld neuerungen beim Beenden eines Makrosprachen Benutzers Bearbeiten der `SupplierID` Wert wieder auf Postback gesendet? Mithilfe der Index der Zeile als die `value`, und klicken Sie dann Abrufen der `SupplierID` beim Postback aus der `DataKeys` -Auflistung, können wir sicherstellen, dass der Benutzer nur eine der verwendet die `SupplierID` Werte, die mit einem der GridView Zeilen verknüpft ist.

Nach dem Hinzufügen dieses Ereignishandlercode, nehmen Sie auf der Seite in einem Browser zu testen. Notieren Sie sich zunächst, nur ein Optionsfeld Schaltfläche im Raster kann zu einem Zeitpunkt ausgewählt werden. Allerdings auf, wenn ein Optionsfeld aus, und klicken Sie auf eine der Schaltflächen, was zu ein Postback tritt auf, und die Optionsfelder, die alle auf den ursprünglichen Status zurückgesetzt (d. h. beim Postback aktivierten Optionsfelds ist nicht mehr ausgewählt). Um dieses Problem zu beheben, müssen wir Erweitern der `RowCreated` Ereignishandler so, dass er den aktivierten Optionsfelds Schaltflächenindex aus der Postback gesendet überprüft und fügt der `checked="checked"` -Attribut dem ausgegebenen Markup für die Zeile Index Übereinstimmungen.

Wenn ein Postback auftritt, sendet der Browser die `name` und `value` des ausgewählten Optionsfelds. Der Wert kann programmgesteuert mit abgerufen werden `Request.Form["name"]`. Die [ `Request.Form` Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) bietet eine [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) , die die Formularvariablen darstellt. Die Formularvariablen sind die Namen und Werte der Formularfelder auf der Webseite und wieder durch den Webbrowser gesendet werden, wenn ein Postback erfolgt. Da der gerenderten `name` die Optionsfelder in der GridView-Attribut ist `SuppliersGroup`, wenn die Webseite Back zurückgesendet wird, der Browser sendet `SuppliersGroup=valueOfSelectedRadioButton` zurück an den Webserver (zusammen mit den anderen Formularfelder). Diese Informationen dann möglich, die von der `Request.Form` mithilfe der Eigenschaft: `Request.Form["SuppliersGroup"]`.

Seit wir muss müssen bestimmt aktivierten Optionsfelds index nicht nur in der `RowCreated` Ereignishandler, aber in der `Click` Ereignishandler für die Schaltfläche Websteuerelemente, Let s Hinzufügen einer `SuppliersSelectedIndex` Eigenschaft, um die Code-Behind-Klasse, die zurückgibt`-1`, wenn keine Schaltfläche "Radio" ausgewählt wurde und den ausgewählten Index, wenn eine Kopie der Optionsfelder ausgewählt ist.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Mit dieser Eigenschaft hinzugefügt wird, wir wissen, dass das Hinzufügen der `checked="checked"` Markup in der `RowCreated` Ereignishandler bei `SuppliersSelectedIndex` gleich `e.Row.RowIndex`. Aktualisieren Sie den Ereignishandler, um diese Logik einschließen:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Durch diese Änderung bleibt nach einem Postback aktivierten Optionsfelds ausgewählt. Nun, wir die Möglichkeit haben, anzugeben, welches Optionsfeld aktiviert ist, konnten wir das Verhalten zu ändern, damit bei der ersten Anzeige die Seite wurde das erste Optionsfeld für GridView Zeile s aktiviert wurde (anstatt keine Optionsfelder, die standardmäßig ausgewählt haben sollten, also in der aktuellen Verhalten). Um das erste Optionsfeld in der Standardeinstellung ausgewählt haben, ändern Sie einfach die `if (SuppliersSelectedIndex == e.Row.RowIndex)` -Anweisung wie folgt: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

Zu diesem Zeitpunkt haben wir eine Spalte des gruppierten Optionsfelder an die GridView hinzugefügt, die für eine einzelne Zeile der GridView ausgewählt und über Postbacks gespeichert werden können. Unsere nächste Schritte sind für die Produkte von den ausgewählten Lieferanten bereitgestellt. In Schritt 4 wir erfahren, wie zum Umleiten des Benutzers zu einer anderen Seite senden entlang der ausgewählten `SupplierID`. In Schritt 5 sehen wir, wie die ausgewählten s Lieferanten in eine GridView auf derselben Seite angezeigt.

> [!NOTE]
> Statt ein TemplateField (der Schwerpunkt dieses langwierige Schritt 3) zu verwenden, konnten wir erstellen eine benutzerdefinierte `DataControlField` -Klasse, die die entsprechende Benutzeroberfläche und Funktionalität rendert. Die [ `DataControlField` Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) ist die Basisklasse, von der die BoundField-, CheckBoxField TemplateField und andere integrierte Felder für GridView und DetailsView abgeleitet werden. Erstellen einer benutzerdefinierten `DataControlField` Klasse würde bedeuten, dass die Spalte von Optionsfeldern verwenden deklarativen Syntax nur hinzugefügt werden, und würde auch das Replizieren der Funktionalität für andere Webseiten und anderen Webanwendungen wesentlich einfacher machen.


Haben Sie jemals erstellten benutzerdefinierten, kompiliert die Steuerelemente in ASP.NET, wissen jedoch Sie, dass viel Arbeit erfordert und Kontaktlisten enthält einen Host der Besonderheiten und Grenzfälle, die sorgfältig behandelt werden müssen. Es wird daher verzichten, implementieren eine Spalte von Optionsfeldern als benutzerdefinierte `DataControlField` fürs Klasse, und bleiben Sie mit der TemplateField-Option. Möglicherweise lassen wir die Möglichkeit zum Durchsuchen verwenden, erstellen und Bereitstellen von benutzerdefinierten `DataControlField` Klassen in einem späteren Lernprogramm!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Schritt 4: Anzeigen der ausgewählten s Lieferanten in einer separaten Seite

Nachdem der Benutzer eine GridView Zeile ausgewählt hat, müssen wir den ausgewählten Lieferanten s Produkte angezeigt. In einigen Fällen können wir diese Produkte in einer separaten Seite, in anderen anzeigen wir dafür auf der gleichen Seite bevorzugen möglicherweise möchten. Lassen Sie s, prüfen Sie zunächst wie die Produkte in einer separaten Seite angezeigt; in Schritt 5 betrachten wir eine GridView, hinzufügen `RadioButtonField.aspx` ausgewählten Lieferanten s angezeigt.

Es gibt derzeit zwei Schaltfläche Web-Steuerelemente auf der Seite `ListProducts` und `SendToProducts`. Wenn die `SendToProducts` geklickt wird, möchten wir den Benutzer senden `~/Filtering/ProductsForSupplierDetails.aspx`. Diese Seite wurde erstellt, der [Master/Detail-Filterung über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-cs.md) Lernprogramm und zeigt die Produkte für den Lieferanten, deren `SupplierID` übergeben wird, über das Feld "Querystring" mit dem Namen `SupplierID`.

Um diese Funktion bereitstellen möchten, erstellen Sie einen Ereignishandler für das `SendToProducts` Schaltfläche s `Click` Ereignis. In Schritt 3 hinzugefügt wir die `SuppliersSelectedIndex` Eigenschaft, die dem Index der Zeile, deren Optionsfeld zurückgibt ausgewählt ist. Das entsprechende `SupplierID` kann die GridView s abgerufen werden `DataKeys` Auflistung und der Benutzer können dann an gesendet werden `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` mit `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Dieser Code funktioniert wunderbar als eines der Optionsfelder aus der GridView ausgewählt ist. Wenn Sie zu Beginn die GridView verfügt nicht über alle Optionsfelder ausgewählt und der Benutzer klickt auf die `SendToProducts` Schaltfläche `SuppliersSelectedIndex` werden `-1`, wodurch eine Ausnahme ausgelöst wird, seit `-1` liegt außerhalb des Indexbereichs, der die `DataKeys`Auflistung. Dies ist nicht relevant, jedoch, wenn Sie sich entschieden, aktualisieren Sie die `RowCreated` Ereignishandler wie beschrieben in Schritt 3, um das erste Optionsfeld in der GridView, die ursprünglich ausgewählt haben.

Auf eine `SuppliersSelectedIndex` Wert `-1`, fügen Sie ein Websteuerelement Bezeichnung auf der Seite über die GridView. Festlegen seiner `ID` Eigenschaft, um `ChooseSupplierMsg`, dessen `CssClass` Eigenschaft, um `Warning`, dessen `EnableViewState` und `Visible` Eigenschaften `false`, und die zugehörige `Text` Eigenschaft auf einen anderen Lieferanten aus dem Raster auswählen. Die CSS-Klasse `Warning` Text in Rot, kursiv, fett, große Schriftart anzeigt und ist definiert `Styles.css`. Durch Festlegen der `EnableViewState` und `Visible` Eigenschaften `false`, die Bezeichnung wird nicht für jene postbacks Where außer gerendert Steuerelement s `Visible` -Eigenschaft programmgesteuert auf festgelegt ist `true`.


[![Fügen Sie ein Websteuerelement Bezeichnung über die GridView hinzu](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Abbildung 13**: Hinzufügen einer Bezeichnung Web Steuerelement über die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


Erweitern Sie als Nächstes die `Click` Ereignishandler zum Anzeigen der `ChooseSupplierMsg` bezeichnen, wenn `SuppliersSelectedIndex` ist kleiner als 0 (null), und leiten Sie die Benutzer `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` andernfalls.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Besuchen Sie die Seite in einen Browser, und klicken Sie auf die `SendToProducts` Schaltfläche, bevor Sie einen anderen Lieferanten GridView auswählen. Wie Abbildung 14 zeigt, zeigt der `ChooseSupplierMsg` Bezeichnung. Als nächstes wählen Sie einen Lieferanten, und klicken Sie auf die `SendToProducts` Schaltfläche. Dies wird Sie zu einer Seite weiter, die die von den ausgewählten Lieferanten bereitgestellte Produkte aufgeführt sind. Abbildung 15 zeigt die `ProductsForSupplierDetails.aspx` Startseite bei der Lieferant Bigfoot Brauereien ausgewählt wurde.


[![Die Bezeichnung ChooseSupplierMsg wird angezeigt, wenn keine Lieferanten ausgewählt ist](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Abbildung 14**: die `ChooseSupplierMsg` Bezeichnung wird angezeigt, wenn keine Lieferanten ausgewählt ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Die ausgewählte s Lieferanten werden im ProductsForSupplierDetails.aspx angezeigt.](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Abbildung 15**: die ausgewählte s Lieferanten werden angezeigt, `ProductsForSupplierDetails.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Schritt 5: Anzeigen der ausgewählten s Lieferanten auf derselben Seite

In Schritt 4 wurde erläutert, wie der Benutzer zu einer anderen Webseite zum Anzeigen des ausgewählten Lieferanten s Produkte zu senden. Alternativ können die ausgewählten s Lieferanten auf derselben Seite angezeigt werden. Um dies zu veranschaulichen, fügen wir eine andere GridView zu `RadioButtonField.aspx` ausgewählten Lieferanten s angezeigt.

Da nur soll diese GridView Produkte angezeigt, nachdem ein Lieferant ausgewählt wurde, fügen Sie ein Panel-Websteuerelement unterhalb der `Suppliers` GridView, Festlegen seiner `ID` auf `ProductsBySupplierPanel` und seine `Visible` Eigenschaft, um `false`. Innerhalb des Bereichs fügen Sie den Text Produkte für den Lieferanten ausgewählt, gefolgt von einem GridView mit dem Namen `ProductsBySupplier`. Aus den GridView s smart Tag auswählen, um die Bindung an eine neue, mit dem Namen ObjectDataSource `ProductsBySupplierDataSource`.


[![Die ProductsBySupplier GridView an einen neuen ObjectDataSource binden](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Abbildung 16**: Binden der `ProductsBySupplier` GridView, um eine neue ObjectDataSource ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


Konfigurieren Sie anschließend das ObjectDataSource verwenden die `ProductsBLL` Klasse. Da wir nur die Produkte, die von den ausgewählten Lieferanten bereitgestellte abrufen möchten, geben Sie, dass das ObjectDataSource aufgerufen werden sollen die `GetProductsBySupplierID(supplierID)` Methode, um die Daten abzurufen. Wählen Sie (keine) aus den Dropdown-Listen in der Update-, INSERT- und löschen Sie Registerkarten.


[![Konfigurieren der ObjectDataSource zur Verwendung der GetProductsBySupplierID(supplierID)-Methode](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Abbildung 17**: Konfigurieren der ObjectDataSource verwenden die `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Legen Sie die Dropdown-Listen zu (keine) in der Update-, INSERT- und Löschen von Registerkarten](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Abbildung 18**: Legen Sie das Dropdown-Listen zu (keine) in aktualisieren, einfügen und Löschen von Registerkarten ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Nach dem konfigurieren die SELECT-Anweisung, aktualisieren, einfügen, und Löschen von Registerkarten, klicken Sie auf Weiter. Da die `GetProductsBySupplierID(supplierID)` Methode erwartet einen Eingabeparameter, dass der Assistent zum Erstellen der Datenquelle aufgefordert werden wir die Quelle für die s-Parameterwert angeben.

Wir haben eine Reihe von Optionen, die hier in die Quelle des Parameterwerts s angibt. Wir verwenden das Standardobjekt für die Parameter, und programmgesteuert weisen den Wert der der `SuppliersSelectedIndex` Eigenschaft mit dem Parameter-s `DefaultValue` Eigenschaft in das ObjectDataSource-s `Selecting` -Ereignishandler. Verweisen auf die [Programmgesteuertes Festlegen von Parameterwerten für das ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) Tutorial aufzufrischen auf das ObjectDataSource-s-Parameter programmgesteuert Werte zuweisen.

Alternativ können wir verwenden eine ControlParameter und finden Sie in der `Suppliers` GridView s [ `SelectedValue` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (siehe Abbildung 19). Die GridView s `SelectedValue` -Eigenschaft gibt die `DataKey` entsprechenden Wert der [ `SelectedIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). In der Reihenfolge für diese Option funktioniert, müssen wir die GridView s programmgesteuert festlegen `SelectedIndex` Eigenschaft, die dem ausgewählten Datenzeile, wenn die `ListProducts` Schaltfläche geklickt wird. Als zusätzliche, durch Festlegen der `SelectedIndex`, schalten Sie der ausgewählte Datensatz wird die `SelectedRowStyle` definiert, der `DataWebControls` Design (einen gelben Hintergrund).


[![Verwenden Sie eine ControlParameter an die GridView s "SelectedValue" als Parameterquelle für](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Abbildung 19**: Verwenden einer ControlParameter an die GridView-s "SelectedValue" als Quelle für Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


Nach Abschluss des Assistenten, wird Visual Studio automatisch Felder für die Product-s-Datenfelder hinzufügen. Entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields, und ändern Sie die `HeaderText` Eigenschaften zum Produkt, Kategorie und Preis. Konfigurieren der `UnitPrice` BoundField, damit der Wert als Währung formatiert ist. Nachdem Sie diese Änderungen vornehmen, sollten Bereich GridView und ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Um diese Übung abschließen zu können, müssen wir die GridView s festgelegt `SelectedIndex` Eigenschaft, um die `SelectedSuppliersIndex` und die `ProductsBySupplierPanel` Bereich s `Visible` Eigenschaft, um `true` bei der `ListProducts` Schaltfläche geklickt wird. Um dies zu erreichen, erstellen Sie einen Ereignishandler für das `ListProducts` Schaltfläche Websteuerelement s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Wenn ein Lieferant nicht aus der GridView ausgewählt wurde die `ChooseSupplierMsg` Bezeichnung angezeigt wird und die `ProductsBySupplierPanel` Bereich ausgeblendet. Andernfalls, wenn ein Lieferant ausgewählt wurde, die `ProductsBySupplierPanel` wird angezeigt, und die GridView s `SelectedIndex` Eigenschaft aktualisiert wird.

Abbildung 20 zeigt die Ergebnisse nach der Lieferant Bigfoot Brauereien ausgewählt wurde und die Produkte anzeigen, auf die Schaltfläche "" geklickt wurde.


[![Die Produkte, die von Bigfoot Brauereien bereitgestellten sind auf derselben Seite aufgeführt](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Abbildung 20**: der Produkte von Bigfoot Brauereien bereitgestellten auf derselben Seite aufgelisteten ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

Wie in beschrieben die [Master/Detail mit auswählbaren Master GridView mit einer Details-DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Lernprogramm Datensätze aus einer mit einem CommandField GridView ausgewählt werden können, deren `ShowSelectButton` -Eigenschaftensatz auf `true`. Wird jedoch die CommandField zugehörigen Schaltflächen als reguläre Schaltflächen, Links oder Bilder angezeigt. Eine alternative Zeilenauswahl-Benutzeroberfläche ist Optionsfeld und das Kontrollkästchen in jeder Zeile GridView angeben. In diesem Lernprogramm untersucht wir zum Hinzufügen einer Spalteninhalts von Optionsfeldern.

Leider werden eine Spalte mit nicht für Sender Schaltflächen als unkompliziert und einfach hinzufügen, wie zu erwarten wäre. Es ist keine integrierte RadioButtonField, die bei einem Klick auf eine Schaltfläche hinzugefügt werden kann, und verwenden das RadioButton-Steuerelement in ein TemplateField führt einen eigenen Satz von Problemen. Am Ende eine solche Schnittstelle bereitstellen wir entweder haben, erstellen Sie eine benutzerdefinierte `DataControlField` Klasse oder Mittel, um den entsprechenden HTML-Code in ein TemplateField während Räumen der `RowCreated` Ereignis.

Gewusst wie: Hinzufügen einer Spalte von Optionsfeldern, untersucht müssen uns Ihre unsere Aufmerksamkeit auf das Hinzufügen einer Spalteninhalts der Kontrollkästchen zu aktivieren. Mit einer Spalte der Kontrollkästchen kann ein Benutzer eine oder mehrere GridView Zeilen auswählen und dann ausführen ein Vorgangs auf alle ausgewählten Zeilen (z. B. einen Satz von e-Mail-Nachrichten über ein webbasiertes e-Mail-Client anschließend auswählen und So löschen Sie alle ausgewählten e-Mail-Nachrichten). In den nächsten Lernprogrammen sehen wir, wie eine solche Spalte hinzugefügt wird.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde David Suru. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Nächste](adding-a-gridview-column-of-checkboxes-cs.md)
