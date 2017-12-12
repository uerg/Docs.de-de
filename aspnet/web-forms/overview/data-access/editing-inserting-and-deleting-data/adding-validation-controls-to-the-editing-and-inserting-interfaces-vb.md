---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: "Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen (VB) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm sehen wir, wie einfach es ist, die Validierungssteuerelemente EditItemTemplate und InsertItemTemplate Datenmenge-Websteuerelement, geben Sie einen weiteren hinzufügen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a181be8d07ff15c33818077dc75b5cc6c6bbf65d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) oder [PDF herunterladen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> In diesem Lernprogramm sehen wir, wie einfach es ist, die Validierungssteuerelemente EditItemTemplate und InsertItemTemplate Datenmenge-Websteuerelement, geben Sie eine weitere sichere Benutzeroberfläche hinzufügen.


## <a name="introduction"></a>Einführung

Die GridView und DetailsView-Steuerelemente in den Beispielen haben wir in der letzten untersucht, die drei Lernprogramme alle BoundFields und CheckBoxFields (die Feldtypen automatisch von Visual Studio hinzugefügt werden, wenn eine GridView oder DetailsView an eine Datenquelle binden erstellte wurde Steuern Sie über den smart Tag). Wenn Sie eine Zeile in einer GridView oder DetailsView bearbeiten zu können, werden diese BoundFields, die nicht schreibgeschützt sind in Textfelder ein, konvertiert aus dem Ändern der Endbenutzer der vorhandenen Daten. Auf ähnliche Weise beim Einfügen einer neuen Datensatz mit einem DetailsView-Steuerelement diese BoundFields, deren `InsertVisible` -Eigenschaftensatz auf `True` (Standardeinstellung) werden gerendert als leere Textfelder ein, in die der Benutzer des neuen Datensatzes Feldwerte angeben kann. CheckBoxFields, die in der standard, schreibgeschützte Schnittstelle deaktiviert sind, werden ebenso in aktivierten Kontrollkästchen in die Bearbeitung und das Einfügen von Schnittstellen konvertiert.

Die Standardeinstellung, bearbeiten und Einfügen von Schnittstellen für die BoundField- und CheckBoxField kann hilfreich sein, fehlt die Schnittstelle jegliche Art von Validierung. Wenn ein Benutzer einen Fehler des Daten-Eintrag - z. B. auslassen macht die `ProductName` Feld oder einen ungültigen Wert für die Eingabe `UnitsInStock` (z. B.-50) eine Ausnahme ausgelöst von in die Tiefen der Anwendungsarchitektur. Während diese Ausnahme ordnungsgemäß behandelt werden kann, wie in der [vorherigen Lernprogramm](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), idealerweise die Bearbeitung oder Einfügen von Benutzeroberfläche zählen Validierungssteuerelemente, um zu verhindern, dass einen Benutzer eingeben solche ungültigen Daten in der Platzieren Sie zuerst.

Um eine angepasste bearbeiten oder Einfügen von Schnittstelle zu gewährleisten, müssen wir die BoundField- oder CheckBoxField durch ein TemplateField zu ersetzen. Von TemplateFields, die das Thema der Diskussion wurden in die [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) und [mithilfe von TemplateFields, DetailsView--Steuerelement](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) Lernprogramme, besteht aus mehreren Definieren von Vorlagen trennen Schnittstellen für andere Zeilenstatus. Die TemplateField `ItemTemplate` wird verwendet, um beim Rendern schreibgeschützte Felder oder Zeilen, DetailsView oder GridView-Steuerelemente, wohingegen die `EditItemTemplate` und `InsertItemTemplate` die Schnittstellen für die Bearbeitung und Einfügen von Modi bzw. anzugeben.

In diesem Lernprogramm fügen wir sehen, wie einfach es ist die TemplateField Validierungssteuerelemente hinzuzufügende `EditItemTemplate` und `InsertItemTemplate` um eine weitere sichere Benutzeroberfläche bereitzustellen. In diesem Lernprogramm nimmt insbesondere im Beispiel in erstellt die [untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Lernprogramm und Aufwertung die Bearbeitung und das Einfügen von Schnittstellen, um die entsprechenden Validierung beinhalten.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Schritt 1: Replizieren das Beispiel aus[einfügen, aktualisieren und Löschen von zugeordneten Ereignisse untersuchen](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

In der [untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Lernprogramm erstellten eine Seite, die die Namen und Preise der Produkte in einem bearbeitbaren GridView aufgeführt. Darüber hinaus enthalten die Seite eine DetailsView, deren `DefaultMode` -Eigenschaft wurde festgelegt, um `Insert`, wodurch immer Rendering im Einfügemodus. Aus diesem DetailsView der Benutzer konnte Geben Sie den Namen und den Preis für ein neues Produkt auf Einfügen, und es dem System hinzugefügt haben (siehe Abbildung 1).


[![Im vorherige Beispiel ermöglicht Benutzern das Hinzufügen neuer Produkte und Bearbeiten vorhandener Beziehungen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Abbildung 1**: das vorherige Beispiel ermöglicht es Benutzern, neue Produkte hinzufügen und Bearbeiten vorhandener Sitzungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Unser Ziel für dieses Lernprogramm ist erweitern, die DetailsView und GridView, um Validierungssteuerelemente bereitzustellen. Insbesondere wird Überprüfungslogik:

- Erfordern Sie, dass der Name angegeben werden, beim Einfügen oder Bearbeiten eines Produkts
- Erfordert, dass der Preis bereitgestellt werden, wenn einen Datensatz eingefügt; Wenn Sie einen Datensatz zu bearbeiten, wir benötigen Sie dennoch einen Preis, verwenden jedoch das programmgesteuerte Logik in der GridView `RowUpdating` -Ereignishandler aus dem früheren Lernprogramm bereits vorhanden.
- Stellen Sie sicher, dass der eingegebene Wert für den Preis ein Währungsformat gültig ist

Bevor wir am erweitern im vorherigen Beispiels betrachten können um eine Validierung beinhalten, müssen Sie zuerst replizieren das Beispiel aus der `DataModificationEvents.aspx` Seite, um die Seite für dieses Lernprogramm `UIValidation.aspx`. Um dies zu erreichen, wir über beide kopieren müssen, der `DataModificationEvents.aspx` deklarativen Seitenmarkup und den Quellcode. Kopieren Sie zuerst über deklaratives Markup, anhand der folgenden Schritte:

1. Öffnen der `DataModificationEvents.aspx` Seite in Visual Studio
2. Wechseln Sie zu der Seite deklarativem Markup (klicken Sie auf die Schaltfläche "Quelle" am unteren Rand der Seite)
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 44) als siehe Abbildung 2.


[![Kopieren Sie den Text innerhalb der &lt;Asp: Inhalt&gt; Steuerelement](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Abbildung 2**: Kopieren Sie den Text innerhalb der `<asp:Content>` Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Öffnen der `UIValidation.aspx` Seite
2. Wechseln Sie zur deklarativen Seitenmarkup
3. Fügen Sie den Text innerhalb der `<asp:Content>` Steuerelement.

Um den Quellcode kopiert haben, öffnen Sie die `DataModificationEvents.aspx.vb` Seite, und kopieren Sie nur den Text *in* der `EditInsertDelete_DataModificationEvents` Klasse. Kopieren Sie die drei Ereignishandler (`Page_Load`, `GridView1_RowUpdating`, und `ObjectDataSource1_Inserting`), jedoch **nicht** kopieren die Klassendeklaration. Fügen Sie den kopierten Text *in* der `EditInsertDelete_UIValidation` in Klasse `UIValidation.aspx.vb`.

Nach dem verschieben, die über den Inhalt und den Code von `DataModificationEvents.aspx` zu `UIValidation.aspx`, können Sie den Status in einem Browser zu testen. Daraufhin sollte die gleiche Ausgabe und auftreten, die gleiche Funktionalität in diesen beiden Seiten (finden Sie in Abbildung 1 zurück, für einen Screenshot der `DataModificationEvents.aspx` in Aktion).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Schritt 2: Umwandeln von TemplateFields der BoundFields

Um die Bearbeitung und Einfügen von Schnittstellen Validierungssteuerelemente hinzugefügt haben, müssen der BoundFields, DetailsView und GridView-Steuerelemente in von TemplateFields konvertiert wird. Um dies zu erreichen, klicken Sie jeweils auf die Spalten bearbeiten und Felder bearbeiten Links in der GridView und DetailsView des Smarttags. Vorhanden ist, wählen Sie die einzelnen der BoundFields, und klicken Sie auf den Link "Dieses Feld in ein TemplateField konvertieren".


[![Umwandeln Sie jede der DetailsView und des GridView-BoundFields von TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Abbildung 3**: Konvertieren Sie jeweils die DetailsViews und GridView BoundFields in von TemplateFields ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Konvertieren einer BoundField in ein TemplateField über die Felder (Dialogfeld), generiert ein TemplateField, der die gleichen schreibgeschützten, bearbeiten und Einfügen von Schnittstellen, die BoundField selbst darstellt. Das folgende Markup zeigt die deklarative Syntax für die `ProductName` DetailsView nach der Konvertierung in ein TemplateField Feld:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Beachten Sie, dass diese TemplateField drei Vorlagen, die automatisch erstellt hatte `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate`. Die `ItemTemplate` zeigt einen einzelnen Datenwert des Felds (`ProductName`) dagegen mit einem Label-Steuerelement, das `EditItemTemplate` und `InsertItemTemplate` präsentieren den Wert des Felds in einem Textfeld-Steuerelement, das das Feld "Daten" Textfeldss zuordnet `Text` Verwenden der bidirektionalen Datenbindung-Eigenschaft. Da wir für das Einfügen von nur DetailsView auf dieser Seite verwenden, können Sie entfernen die `ItemTemplate` und `EditItemTemplate` aus den zwei von TemplateFields, es gibt zwar keinen Schaden in verlassen diese.

Da GridView nicht über die integrierten Funktionen von DetailsView einfügen, Konvertieren der GridView unterstützt `ProductName` Feld in ein TemplateField führt zu einer `ItemTemplate` und `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Durch Klicken auf "Konvertieren dieses Feld in ein TemplateField" "" erstellt Visual Studio ein TemplateField, deren Vorlagen die Benutzeroberfläche des konvertierten BoundField-imitieren. Sie können dies überprüfen, indem Sie diese Seite über einen Browser besuchen. Sie werden feststellen, dass es sich bei das Aussehen und Verhalten von den von TemplateFields ist identisch mit der Erfahrung, wenn BoundFields stattdessen verwendet wurden.

> [!NOTE]
> Wahlweise können Sie die Bearbeitung Schnittstellen in den Vorlagen nach Bedarf anpassen. Angenommen, wir möchten das Textfeld enthalten, der `UnitPrice` von TemplateFields als kleinere Textbox als die `ProductName` Textfeld. Um dies zu erreichen können Sie des Textfelds festgelegt `Columns` Eigenschaft auf einen geeigneten Wert, oder geben Sie eine absolute Breite über die `Width` Eigenschaft. In den nächsten Lernprogrammen sehen wir, wie Sie vollständig die Bearbeitungsoberfläche anpassen, indem Sie das Textfeld mit einer alternativen Dateneingabe Websteuerelement ersetzen.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Schritt 3: Hinzufügen der Validierungssteuerelemente an der GridView`EditItemTemplate` s

Beim Dateneingabeformulare erstellen zu können, ist es wichtig, dass Benutzer über alle erforderlichen Felder eingeben und, dass alle bereitgestellte Eingaben zulässigen Werte ordnungsgemäß formatiert sind. Um sicherzustellen, dass Eingaben des Benutzers gültig sind, bietet ASP.NET fünf integrierte Validierungssteuerelemente, die zur Überprüfung des Werts eines einzigen Steuerelements verwendet werden soll:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) wird sichergestellt, dass ein Wert bereitgestellt wurde
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) überprüft einen Wert auf eine andere Web Control-Wert oder einen konstanten Wert oder wird sichergestellt, dass das Format des Werts für einen angegebenen Datentyp zulässig ist
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) wird sichergestellt, dass ein Wert innerhalb eines Bereichs von Werten ist.
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) überprüft einen Wert für eine [reguläre Ausdrücke](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) überprüft einen Wert für eine benutzerdefinierte, vom Benutzer definierten-Methode

Weitere Informationen zu diesen fünf Steuerelemente, sehen Sie sich die [Validierungssteuerelemente Abschnitt](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) von der [ASP.NET Schnellstart-Lernprogrammen](https://asp.net/QuickStart/aspnet/).

Für unser Tutorial benötigen wir verwenden einen RequiredFieldValidator im DetailsView und GridView `ProductName` von TemplateFields und einen RequiredFieldValidator in der DetailsView `UnitPrice` TemplateField. Darüber hinaus benötigen wir beide Steuerelemente ein CompareValidator hinzuzufügende `UnitPrice` von TemplateFields, der sicherstellt, dass der eingegebene Preis einen Wert größer als oder gleich 0 hat und werden in einem gültigen Währungsformat dargestellt.

> [!NOTE]
> Während ASP.NET 1.x hatte diese gleichen fünf Validierungssteuerelemente, ASP.NET 2.0 verfügt über eine Reihe von Verbesserungen hinzugefügt, den Hauptknoten zwei Skriptdatei auf Clientseite wird Unterstützung für Browser als Internet Explorer und die Möglichkeit, die Validierungssteuerelemente für Partition auf einer Seite in Überprüfung Gruppen. Weitere Informationen zu den neuen Funktionen der Überprüfung-Steuerelement in 2.0 finden Sie unter [unterzogen, wodurch der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Beginnen wir durch Hinzufügen der erforderlichen Validierungssteuerelemente, die `EditItemTemplate` s in der GridView von TemplateFields. Um dies zu erreichen, klicken Sie auf die Verknüpfung Vorlagen bearbeiten die GridView Smarttag, um die Vorlage bearbeiten Schnittstelle anzuzeigen. Von hier aus können Sie die Vorlage so bearbeiten Sie aus der Dropdown-Liste auswählen. Da wir die Bearbeitungsoberfläche ergänzen möchten, müssen wir Validierungssteuerelemente zum Hinzufügen der `ProductName` und `UnitPrice`des `EditItemTemplate` s.


[![Wir müssen die ProductName und UnitPrice des EditItemTemplates erweitern](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Abbildung 4**: müssen wir erweitern die `ProductName` und `UnitPrice`des `EditItemTemplate` s ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


In der `ProductName` `EditItemTemplate`, fügen Sie einen RequiredFieldValidator hinzu, indem Sie ziehen es aus der Toolbox in die Vorlage bearbeiten-Oberfläche, nach dem Textfeld platzieren.


[![Fügen Sie einen RequiredFieldValidator ProductName ItemTemplate hinzu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Abbildung 5**: hinzufügen einen RequiredFieldValidator auf die `ProductName` `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Alle Validierungssteuerelemente durch Überprüfen der Eingabe eines einzelnen ASP.NET Web-Steuerelements arbeiten. Aus diesem Grund müssen wir darauf hinweisen, dass wir gerade hinzugefügten RequiredFieldValidator, für das Textfeld in überprüfen soll der `EditItemTemplate`; dies geschieht durch Festlegen des Validierungssteuerelements [ControlToValidate-Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) auf die `ID` des entsprechenden Web-Steuerelements. Das Textfeld hat derzeit stattdessen nondescript `ID` von `TextBox1`, aber wir in etwas besser geeignet. Klicken Sie auf das Textfeld in der Vorlage, und ändern Sie dann im Fenster Eigenschaften die `ID` aus `TextBox1` auf `EditProductName`.


[![Ändern Sie das Textfeld-ID in EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Abbildung 6**: Ändern des Textfelds `ID` auf `EditProductName` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Legen Sie anschließend die RequiredFieldValidator `ControlToValidate` Eigenschaft `EditProductName`. Legen Sie schließlich die ["ErrorMessage"-Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "müssen Sie den Namen des Produkts bereitstellen" und die [Texteigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) zu "\*". Die `Text` Eigenschaftswert, falls vorhanden, wird der Text, der durch das Validierungssteuerelement angezeigt wird, wenn die Validierung fehlschlägt. Die `ErrorMessage` Eigenschaftswert, der erforderlich ist, wird vom Steuerelement ValidationSummary; verwendet, wenn die `Text` Eigenschaftswert weggelassen wird, die `ErrorMessage` Eigenschaftswert ist auch der Text angezeigt, die für das Validierungssteuerelement für die Eingabe ist ungültig.

Nach dem Festlegen dieser drei Eigenschaften des RequiredFieldValidator, sollte der Bildschirm in Abbildung 7 ähneln.


[![Legen Sie die RequiredFieldValidator ControlToValidate, ErrorMessage und Eigenschaften von Text](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Abbildung 7**: Legen Sie die RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, und `Text` Eigenschaften ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Mit dem RequiredFieldValidator hinzugefügt der `ProductName` `EditItemTemplate`, dass alle, bleibt die erforderlichen Validierungen der `UnitPrice` `EditItemTemplate`. Da wir, dass für diese Seite sich dafür entschieden haben die `UnitPrice` ist optional, wenn Editieren eines Datensatzes, wir müssen einen RequiredFieldValidator hinzufügen. Wir tun, allerdings müssen eine CompareValidator, um sicherzustellen, dass die `UnitPrice`, sofern angegeben, wird als Währung ordnungsgemäß formatiert und ist größer als oder gleich 0.

Bevor wir CompareValidator zum Hinzufügen der `UnitPrice` `EditItemTemplate`, ändern wir das TextBox-Websteuerelement-ID zuerst `TextBox2` auf `EditUnitPrice`. Nach dieser Änderung der CompareValidator hinzufügen Festlegen seiner `ControlToValidate` Eigenschaft, um `EditUnitPrice`, dessen `ErrorMessage` -Eigenschaft auf "Preis muss größer als oder gleich null sein und darf keine enthalten das Währungssymbol", und die zugehörige `Text` Eigenschaft, um "\*".

Gibt an, dass die `UnitPrice` Wert muss größer als oder gleich 0, legen Sie die CompareValidator [Operator-Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) auf `GreaterThanEqual`, dessen [ValueToCompare Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) auf "0" und dessen [ Type-Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) auf `Currency`. Die folgende deklarative Syntax zeigt die `UnitPrice` TemplateFields `EditItemTemplate` nachdem diese Änderungen vorgenommen wurden:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Öffnen Sie nachdem diese Änderungen vorgenommen wurden die Seite in einem Browser aus. Wenn Sie versuchen, lassen Sie den Namen, oder geben einen ungültigen preiswert, wenn ein Produkt zu bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 8 gezeigt, wird ein preiswert, der das Währungssymbol ein, z. B. $19.95 enthält als ungültig betrachtet. Die CompareValidator `Currency` `Type` für die zifferngruppierung (z. B. Kommas oder Punkte, je nach den kultureinstellungen) und einem vorangestellten Plus- oder Minuszeichen (-) können im Gegensatz zu *nicht* ein Währungssymbol zulassen. Dieses Verhalten kann Benutzer perplex, die Bearbeitungsoberfläche einzugehen sind derzeit die `UnitPrice` das Währungsformat.

> [!NOTE]
> Denken Sie daran, dass in der *einfügen, aktualisieren und Löschen von Ereignissen zu zugeordnet* Lernprogramm legen wir den BoundField `DataFormatString` Eigenschaft `{0:c}` um es als Währung formatiert. Darüber hinaus legen wir den `ApplyFormatInEditMode` -Eigenschaft auf "true", verursacht die GridView das Bearbeitungssteuerelement Schnittstelle zum Formatieren der `UnitPrice` als Währung. Bei der Konvertierung von der BoundField in ein TemplateField Visual Studio diese Einstellungen notiert und des Textfelds formatiert `Text` Eigenschaft als Währung mit der Syntax Databinding `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe angezeigt.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Abbildung 8**: ein Sternchen wird weiter ', um die Textfelder ein, mit der ungültigen Eingabe ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Während der Überprüfung erfolgt wie-ist, muss der Benutzer das Währungssymbol manuell zu entfernen, wenn einen Datensatz bearbeiten, was nicht zulässig ist. Zur Behebung des Problems, haben wir drei Optionen:

1. Konfigurieren der `EditItemTemplate` , damit die `UnitPrice` Wert ist nicht als Währung formatiert.
2. Ermöglicht dem Benutzer ein Währungssymbol eingeben, indem CompareValidator entfernen und ersetzen es mit einem RegularExpressionValidator, die ordnungsgemäß für eine ordnungsgemäß formatierte Währungswert überprüft. Hier das Problem ist, dass der reguläre Ausdruck, der einen Währungswert überprüfen nicht ziemlich und müsste Schreiben von Code aus, wenn die kultureinstellungen integriert werden sollten.
3. Das Validierungssteuerelement vollständig zu entfernen und basieren auf serverseitige Validierungslogik in der GridView `RowUpdating` -Ereignishandler.

Gehen Sie mit der Option #1 für diese Übung wir noch. Derzeit die `UnitPrice` formatiert ist, als Währung aufgrund der Datenbindungsausdruck für das Textfeld in der `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Ändern Sie die Bind-Anweisung zu `Bind("UnitPrice", "{0:n2}")`, formatiert das Ergebnis als Zahl mit zwei Ziffern Genauigkeit. Dies ist möglich, direkt über die deklarative Syntax oder durch Klicken auf den Link "DataBindings bearbeiten" aus der `EditUnitPrice` Textfeld in der `UnitPrice` TemplateFields `EditItemTemplate` (siehe Abbildung 9 und 10).


[![Klicken Sie auf das Textfeld DataBindings bearbeiten link](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie auf das Textfeld DataBindings bearbeiten Link ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Geben Sie den Formatbezeichner in der Bind-Anweisung](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Abbildung 10**: Geben Sie den Formatbezeichner in der `Bind` Anweisung ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Durch diese Änderung der formatierte Preis in die Bearbeitungsoberfläche Kommas als Gruppentrennzeichen für die und einen Punkt als dezimales Trennzeichen enthält, lässt jedoch deaktiviert das Währungssymbol.

> [!NOTE]
> Die `UnitPrice` `EditItemTemplate` umfasst jedoch nicht den einen RequiredFieldValidator, sodass das Postback, stellen Sie sicher und aktualisierbare Logik, um zu beginnen. Allerdings die `RowUpdating` -Ereignishandler aus kopiert die *untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von* Lernprogramm umfasst eine programmgesteuerte Überprüfung, die garantiert, dass die `UnitPrice` wird bereitgestellt. Entfernen diese Logik, lassen Sie ihn als gerne-ist, oder fügen Sie einen RequiredFieldValidator auf die `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Schritt 4: Zusammenfassung der Probleme mit der Dateneingabe

Zusätzlich zu den fünf Validierungssteuerelemente ASP.NET umfasst die [ValidationSummary Steuerelement](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), welche zeigt die `ErrorMessage` s die Validierungssteuerelemente, die ungültige Daten erkannt. Diese zusammengefassten Daten können als Text auf der Webseite oder über ein modales, clientseitige Messagebox angezeigt werden. Wir erweitern Sie dieses Tutorial aus, um eine Zusammenfassung der Überprüfungsprobleme clientseitige-Messagebox enthalten ist.

Um dies zu erreichen, müssen ziehen Sie ein ValidationSummary-Steuerelement aus der Toolbox in den Designer. Der Speicherort des Validierungssteuerelements ist wirklich unerheblich, da wir die offensichtlichen So zeigen Sie die Zusammenfassung nur als eine MessageBox-Datenbank zu konfigurieren. Legen Sie nach dem Hinzufügen des Steuerelements an, dessen [ShowSummary-Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) auf `False` und seine [ShowMessageBox-Eigenschaft](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) auf `True`. Durch diese hinzufügen werden Validierungsfehler in eine clientseitige Messagebox zusammengefasst.


[![Validierungsfehler werden in einer clientseitigen Messagebox zusammengefasst.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Abbildung 11**: die Validierungsfehler werden in eine clientseitige Messagebox zusammengefasst ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Schritt 5: Hinzufügen der Validierungssteuerelemente, DetailsView des`InsertItemTemplate`

Alle für dieses Lernprogramm bleibt Einfügen der DetailsView-Schnittstelle der Validierungssteuerelemente hinzu. Beim Hinzufügen von Steuerelementen für die Validierung, DetailsView Vorlagen ist identisch mit dem in Schritt 3 untersucht. Wir müssen deshalb mithilfe der Aufgabe in diesem Schritt breeze. Wie wir mit der GridView `EditItemTemplate` s, sollten Sie zum Umbenennen der `ID` s der Textfelder aus der Nondescript `TextBox1` und `TextBox2` auf `InsertProductName` und `InsertUnitPrice`.

Hinzufügen einen RequiredFieldValidator auf die `ProductName` `InsertItemTemplate`. Festlegen der `ControlToValidate` auf die `ID` des Textfelds in der Vorlage seine `Text` Eigenschaft, um "\*" und die zugehörige `ErrorMessage` -Eigenschaft auf "müssen Sie den Namen des Produkts bereitstellen".

Da die `UnitPrice` ist erforderlich für diese Seite, wenn Sie einen neuen Datensatz hinzufügen, einen RequiredFieldValidator zum Hinzufügen der `UnitPrice` `InsertItemTemplate`wird durch das Festlegen der `ControlToValidate`, `Text`, und `ErrorMessage` Eigenschaften entsprechend. Fügen Sie schließlich eine CompareValidator auf die `UnitPrice` `InsertItemTemplate` auch konfigurieren die `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, und `ValueToCompare` Eigenschaften wie wir mit der haben`UnitPrice`des CompareValidator in der GridView `EditItemTemplate`.

Nach dem Hinzufügen dieser Validierungssteuerelemente, kann kein neues Produkt zum System hinzugefügt werden, wenn dessen Name nicht angegeben wird oder wenn es sich bei ihren Preis eine negative Zahl ist oder illegal formatiert werden.


[![Validierungslogik wurde Einfügen der DetailsView-Schnittstelle hinzugefügt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Abbildung 12**: Validierungslogik Einfügen der DetailsView-Schnittstelle hinzugefügt wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Schritt 6: Partitionierung der Validierungssteuerelemente Überprüfung gruppiert

Unsere Seite besteht aus zwei logisch unterschiedliche Sätze von Validierungssteuerelemente: an die GridView entsprechen der Schnittstelle bearbeiten und, DetailsView entsprechen der Schnittstelle einfügen. Standardmäßig wird bei einem Postback *alle* Validierungssteuerelemente auf der Seite werden überprüft. Allerdings nicht beim Bearbeiten eines Datensatzes wir der DetailsView einfügende Schnittstelle Validierungssteuerelemente überprüft werden soll. Abbildung 13 veranschaulicht unseren aktuellen Dilemma beim Bearbeiten eines Produkts perfekt zulässigen Werte eines Benutzers, auf Update verursacht tritt ein Validierungsfehler auf, da die Namen und den Preis Werte in der einfügen-Schnittstelle leer sind.


[![Aktualisieren eines Produkts führt dazu, dass Validierungssteuerelemente für das Einfügen von Schnittstelle zum Auslösen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Abbildung 13**: Aktualisieren eines Produkts führt dazu, dass Validierungssteuerelemente für das Einfügen von-Schnittstelle zum Auslösen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Die Validierungssteuerelemente in ASP.NET 2.0 in Überprüfung Gruppen über partitioniert werden können ihre `ValidationGroup` Eigenschaft. Wenn Sie einen Satz von Steuerelementen für die Validierung in einer Gruppe zuordnen möchten, legen Sie einfach ihre `ValidationGroup` Eigenschaft auf den gleichen Wert. Legen Sie für unser Tutorial aus, die `ValidationGroup` Eigenschaften der Validierungssteuerelemente in die GridView von TemplateFields auf `EditValidationControls` und die `ValidationGroup` Eigenschaften der DetailsView von TemplateFields zu `InsertValidationControls`. Diese Änderungen direkt in das deklarative Markup unternommen werden können, oder Bearbeiten über das Fenster "Eigenschaften", beim Verwenden des Designers Vorlage-Schnittstelle.

Zusätzlich zur Validierung Steuerelemente, die Schaltfläche und die Schaltfläche bezogene Steuerelemente in ASP.NET 2.0 auch enthalten eine `ValidationGroup` Eigenschaft. Eine Validierungsgruppe Validierungssteuerelemente auf Gültigkeit überprüft werden nur wenn ein Postback durch eine Schaltfläche verursacht werden, die demselben `ValidationGroup` Einstellung der Eigenschaft. Beispielsweise ist in der DetailsView Insert Schaltfläche zum Auslösen der `InsertValidationControls` Validierungsgruppe wir die CommandField festlegen müssen `ValidationGroup` Eigenschaft, um `InsertValidationControls` (siehe Abbildung 14). Darüber hinaus legen Sie der GridView CommandFields `ValidationGroup` Eigenschaft `EditValidationControls`.


[![Gruppe, DetailsView's CommandFields ValidationGroup-Eigenschaft, um InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Abbildung 14**: Festlegen der DetailsView CommandFields `ValidationGroup` Eigenschaft `InsertValidationControls` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Nach der Änderung sollte die, DetailsView und GridView von TemplateFields und CommandFields etwa wie folgt aussehen:

Der DetailsView von TemplateFields und CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

Der GridView CommandField und von TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

An diesem Punkt werden den bearbeiten-spezifische Validierungssteuerelemente nur bei der Behebung des Problems hervorgehoben Abbildung 13 der DetailsView Insert-Schaltfläche klicken, wird nur angezeigt, wenn die GridView-Schaltfläche "Aktualisieren" geklickt wird und die Überprüfung von Insert-spezifische Steuerelemente auslösen ausgelöst. Allerdings wird durch diese Änderung unserer Kontrolle ValidationSummary nicht mehr bei der Eingabe von ungültiger Daten. Das Steuerelement ValidationSummary enthält auch eine `ValidationGroup` -Eigenschaft und nur zeigt zusammenfassende Informationen für diese Validierungssteuerelemente in einer Gruppe von Überprüfung. Aus diesem Grund müssen wir zwei Validierungssteuerelemente auf dieser Seite eine für die `InsertValidationControls` Validierungsgruppe und eine für `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Diese Ergänzung ist unser Tutorial abgeschlossen!

## <a name="summary"></a>Zusammenfassung

Während BoundFields sowohl eine einfügen und bearbeiten-Schnittstelle bereitgestellt werden können, ist die Schnittstelle nicht anpassbar. In der Regel möchten wir die Bearbeitung und Einfügen-Schnittstelle, um sicherzustellen, dass der Benutzer, erforderliche Eingaben in einem Dezimalformat eingibt Validierungssteuerelemente hinzugefügt. Zu diesem Zweck müssen wir die BoundFields in von TemplateFields konvertieren und die Validierungssteuerelemente, die entsprechenden Vorlagen hinzufügen. In diesem Lernprogramm erweiterte wir das Beispiel aus der *untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von* Lernprogramm beide DetailsView Validierungssteuerelemente Hinzufügen des Schnittstelle und der GridView einfügen Bearbeiten die Schnittstelle. Darüber hinaus haben wir gesehen, Vorgehensweise beim Anzeigen der Zusammenfassung Überprüfungsinformationen mithilfe des Steuerelements ValidationSummary und wie die Validierungssteuerelemente auf der Seite in unterschiedlichen Überprüfung Gruppen partitioniert.

Wie wir in diesem Lernprogramm gesehen haben, können von TemplateFields bearbeiten und Einfügen von Schnittstellen auf Einbeziehung Validierungssteuerelemente erweitert werden. Von TemplateFields können auch ausgedehnt werden zusätzliche Web Eingabesteuerelemente, aktivieren das Textfeld durch ein geeigneter Websteuerelement ersetzt werden soll. In unserem nächsten Lernprogramm sehen wir, wie das TextBox-Steuerelement mit einem datengebundenen DropDownList-Steuerelement ersetzen, die ideal geeignet ist, wenn einen Fremdschlüssel zu bearbeiten (z. B. `CategoryID` oder `SupplierID` in der `Products` Tabelle).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Liz Shulok und Zack Jones. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[Weiter](customizing-the-data-modification-interface-vb.md)
