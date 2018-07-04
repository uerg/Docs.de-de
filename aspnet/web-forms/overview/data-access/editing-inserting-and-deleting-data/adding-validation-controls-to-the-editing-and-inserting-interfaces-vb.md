---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Hinzufügen von Validierungssteuerelementen zu bearbeiten und Einfügen von Schnittstellen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial sehen wir, wie einfach es ist das EditItemTemplate und InsertItemTemplate eines Daten-Websteuerelement, geben Sie eine Überprüfungssteuerelemente hinzufügen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a2fc00426022513c6e2adc49b0df30f943403302
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366228"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Hinzufügen von Validierungssteuerelementen zu bearbeiten und Einfügen von Schnittstellen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) oder [PDF-Datei herunterladen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> In diesem Tutorial erfahren Sie, wie einfach es ist hinzuzufügende Steuerelemente zur gültigkeitsprüfung der EditItemTemplate und InsertItemTemplate eines Daten-Websteuerelement oder eine weitere narrensicher Benutzeroberfläche bereitzustellen.


## <a name="introduction"></a>Einführung

Die GridView und DetailsView-Steuerelemente in den Beispielen haben wir in den letzten untersucht, die drei Lernprogramme alle BoundFields CheckBoxFields (die Feldtypen von Visual Studio automatisch hinzugefügt, bei der Bindung von einem GridView- oder DetailsView an eine Datenquelle aus und wurden haben Steuern Sie, über das Smarttag). Wenn Sie eine Zeile in einem GridView- oder DetailsView bearbeiten zu können, werden diese BoundFields, die nicht schreibgeschützt sind in Textfelder ein, konvertiert in dem der Endbenutzer die vorhandenen Daten ändern können. Auf ähnliche Weise beim Einfügen einer neuen Notieren Sie in einem DetailsView-Steuerelement, diese BoundFields, deren `InsertVisible` -Eigenschaftensatz auf `True` (Standardeinstellung) werden gerendert als leere Textfelder ein, in die der Benutzer des neuen Datensatzes Feldwerte angeben kann. CheckBoxFields, die in der standard, schreibgeschützte Schnittstelle deaktiviert sind, werden ebenso in aktivierten Kontrollkästchen in das Bearbeiten und Einfügen von Schnittstellen konvertiert.

Der Standardwert, bearbeiten und Einfügen von Schnittstellen für die BoundField- und CheckBoxField kann hilfreich sein, fehlen die Schnittstelle jede Art von Validierung an. Wenn ein Benutzer einen Fehler für das Data-Eintrag - z. B. das Auslassen macht die `ProductName` Feld oder einen ungültigen Wert für die Eingabe `UnitsInStock` (z. B.-50) eine Ausnahme ausgelöst, falls von in die Tiefe der Anwendungsarchitektur. Während dieser Ausnahme ordnungsgemäß verarbeitet werden kann, wie in der [vorherigen Tutorial](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), im Idealfall die Bearbeitung oder Einfügen von Benutzeroberfläche würde enthalten Validierungssteuerelemente aus, um zu verhindern, dass einen Benutzer eingeben solche ungültigen Daten in der zuerst platzieren.

Um eine benutzerdefinierte Bearbeitung oder das Einfügen von Schnittstelle bereitzustellen, müssen wir die BoundField- oder CheckBoxField durch ein TemplateField zu ersetzen. Von TemplateFields, die im Rahmen der Diskussion Waren in der [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) und [Verwenden von TemplateFields, im DetailsView-Steuerelement](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) Tutorials kann aus mehreren bestehen Schnittstellen für die verschiedenen Zeilenstatus trennen Sie die Vorlagen definieren. Das TemplateFields `ItemTemplate` wird verwendet, um beim Rendern von schreibgeschützten Feldern oder Zeilen in der DetailsView oder GridView-Steuerelemente, während die `EditItemTemplate` und `InsertItemTemplate` die Schnittstellen für die Bearbeitung und Einfügen von Modi bzw. anzugeben.

In diesem Tutorial werden wir sehen, wie einfach es ist das TemplateField Steuerelementen zur gültigkeitsprüfung hinzuzufügende `EditItemTemplate` und `InsertItemTemplate` eine mehr narrensicher Benutzeroberfläche bereitstellen. In diesem Tutorial werden insbesondere das Beispiel erstellt der [Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Tutorial und Argumente, die das Bearbeiten und Einfügen von Schnittstellen, für die entsprechende Validierungsfehlermeldung enthalten.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Schritt 1: Replizieren das Beispiel aus[Überprüfen von Ereignissen im Zusammenhang mit einfügen, aktualisieren und löschen](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

In der [Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Tutorial, die wir erstellt haben, eine Seite, die die Namen und Preise der Produkte in einem bearbeitbaren GridView aufgeführt. Darüber hinaus enthalten die Seite eine DetailsView, deren `DefaultMode` -Eigenschaft wurde festgelegt, um `Insert`, wodurch immer Rendering im Einfügemodus befindet. Aus diesem DetailsView, der Benutzer konnte Geben Sie den Namen und den Preis für ein neues Produkt auf Einfügen, und es dem System hinzugefügt (siehe Abbildung 1).


[![Im vorherige Beispiel ermöglicht Benutzern das Hinzufügen eines neuen Produkts, und vorhandene bearbeiten](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Abbildung 1**: das vorherige Beispiel ermöglicht es Benutzern, neue Produkte hinzufügen und vorhandene bearbeiten ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Unser Ziel für dieses Tutorial ist das DetailsView und GridView, zum Bereitstellen von Validierungssteuerelementen zu erweitern. Insbesondere wird unsere Validierungslogik:

- Erfordern Sie, dass der Name angegeben werden, beim Einfügen oder Bearbeiten eines Produkts
- Erfordern Sie, dass der Preis bereitgestellt werden, wenn einen Datensatz eingefügt; Wenn Sie einen Datensatz zu bearbeiten, wir benötigen Sie bei einen Preis, verwenden jedoch die programmgesteuerte Logik in des GridView `RowUpdating` -Ereignishandler aus dem Lernprogramm weiter oben bereits vorhanden.
- Stellen Sie sicher, dass der eingegebene Wert für den Preis einer gültigen Währungsformat ist

Bevor wir weiter erweitern im vorherige Beispiel um regulierte sehen können, müssen wir zuerst auf das Beispiel aus Replizieren der `DataModificationEvents.aspx` Seite, um die Seite für dieses Tutorial `UIValidation.aspx`. Um dies zu erreichen, wir beide kopiert müssen, die `DataModificationEvents.aspx` deklaratives Markup der Seite und deren Quellcode. Kopieren Sie zuerst die deklarativen Markup durch die folgenden Schritte ausführen:

1. Öffnen der `DataModificationEvents.aspx` Seite in Visual Studio
2. Wechseln Sie zu der Seite deklaratives Markup (klicken Sie auf die Schaltfläche "Quelle" am unteren Rand der Seite ")
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 44), dargestellt in Abbildung 2.


[![Kopieren Sie den Text innerhalb der &lt;Asp: Content&gt; Steuerelement](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Abbildung 2**: Kopieren Sie den Text innerhalb der `<asp:Content>` Control ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Öffnen der `UIValidation.aspx` Seite
2. Wechseln Sie zum deklarativen Markup der Seite
3. Fügen Sie den Text innerhalb der `<asp:Content>` Steuerelement.

Öffnen Sie zum Kopieren von über den Quellcode der `DataModificationEvents.aspx.vb` Seite, und kopieren Sie nur den Text *in* der `EditInsertDelete_DataModificationEvents` Klasse. Kopieren Sie die drei Ereignishandler (`Page_Load`, `GridView1_RowUpdating`, und `ObjectDataSource1_Inserting`), aber **nicht** kopieren Sie die Klassendeklaration. Fügen Sie den kopierten Text *in* der `EditInsertDelete_UIValidation` -Klasse im `UIValidation.aspx.vb`.

Nach der Umstellung auf den Inhalt und Code aus `DataModificationEvents.aspx` zu `UIValidation.aspx`, können Sie Ihre Fortschritte in einem Browser zu testen. Daraufhin sollte die gleichen ausgegeben, und erleben die gleiche Funktionalität in jede dieser zwei Seiten (siehe Abbildung 1 für einen Screenshot des `DataModificationEvents.aspx` in Aktion).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Schritt 2: Konvertieren der BoundFields in von TemplateFields

Um die Schnittstellen zum Bearbeiten und Einfügen von Steuerelementen zur gültigkeitsprüfung hinzugefügt haben, müssen die BoundFields ein, die die DetailsView und GridView-Steuerelemente in von TemplateFields konvertiert werden soll. Um dies zu erreichen, klicken Sie jeweils auf die Links "Bearbeiten von Spalten und Felder bearbeiten" in der GridView und DetailsView des Smarttags. Wählen Sie dort jeweils von den BoundFields, und klicken Sie auf den Link "Dieses Feld in ein TemplateField konvertieren".


[![Konvertieren Sie jede der DetailsView und GridView BoundFields in von TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Abbildung 3**: Konvertieren Sie jede der DetailsView und GridView BoundFields in von TemplateFields ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Konvertieren einer BoundField in ein TemplateField über die Felder (Dialogfeld), wird ein TemplateField, die die gleichen schreibgeschützten, bearbeiten und Einfügen von Schnittstellen die BoundField selbst weist generiert. Das folgende Markup zeigt die deklarative Syntax für die `ProductName` Feld in der DetailsView, nachdem es in ein TemplateField konvertiert wurde:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Beachten Sie, dass diese TemplateField drei Vorlagen, die automatisch erstellt hatte `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate`. Die `ItemTemplate` zeigt einen einzelnen Datenwert des Felds (`ProductName`) verwenden ein Label-Steuerelement, das `EditItemTemplate` und `InsertItemTemplate` den Datenfeldwert in ein Textfeld-Steuerelement, das das Datenfeld Textfeldss zuordnet vorhanden `Text` Eigenschaft, die bidirektionale Datenbindung verwenden. Da wir für das Einfügen von nur DetailsView auf dieser Seite verwenden, können Sie entfernen die `ItemTemplate` und `EditItemTemplate` aus den zwei von TemplateFields, jedoch es nicht schädlich ist, sodass sie.

Da die GridView nicht die integrierten Funktionen von DetailsView einfügen, Konvertieren des GridView unterstützt `ProductName` Feld in ein TemplateField führt nur eine `ItemTemplate` und `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Visual Studio verfügt über ein TemplateField erstellt, indem Sie auf "Convert dieses Feld in ein TemplateField", dessen Vorlagen die Benutzeroberfläche der konvertierten BoundField-imitieren, kann. Sie können dies überprüfen, indem Sie auf dieser Seite über einen Browser. Sie werden feststellen, dass das Aussehen und Verhalten von der von TemplateFields auf die Benutzeroberfläche identisch ist, wenn BoundFields stattdessen verwendet wurden.

> [!NOTE]
> Die Bearbeitung Schnittstellen in den Vorlagen nach Bedarf anpassen können. Wir möchten z. B. das Textfeld enthalten, der `UnitPrice` von TemplateFields gerendert als Textfeld kleiner als die `ProductName` Textfeld. Hierzu können Sie festlegen, des Textfelds `Columns` Eigenschaft auf einen geeigneten Wert, oder geben Sie die Breite eine absolute über die `Width` Eigenschaft. Im nächsten Tutorial sehen wir, wie Sie die Bearbeitungsschnittstelle ersetzen das Textfeld mit einem alternativen Eintrag Websteuerelement vollständig anpassen.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Schritt 3: GridView Steuerelemente zur gültigkeitsprüfung hinzugefügt`EditItemTemplate` s

Wenn Dateneingabeformularen erstellen zu können, ist es wichtig, dass Benutzer alle erforderlichen Felder eingeben und, dass alle bereitgestellte Eingaben für den gesetzlichen, korrekt formatierte Werte. Um sicherzustellen, dass ein Benutzer die Eingaben gültig sind, bietet ASP.NET die fünf integrierten Validierungssteuerelemente, die entwickelt wurden, um zur Überprüfung des Werts, der ein einzelnes Eingabesteuerelement verwendet werden:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) wird sichergestellt, dass ein Wert bereitgestellt wurde
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) überprüft einen Wert für eine andere Web-Control-Wert oder einen konstanten Wert oder wird sichergestellt, dass das Format des Werts für einen angegebenen Datentyp gültig ist
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) wird sichergestellt, dass ein Wert innerhalb eines Bereichs von Werten ist.
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) überprüft einen Wert für eine [reguläre Ausdrücke](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) überprüft einen Wert für eine benutzerdefinierte, eine benutzerdefinierte Methode

Finden Sie weitere Informationen zu diesen fünf Steuerelemente, die [Steuerelemente zur gültigkeitsprüfung Abschnitt](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) von der [Schnellstart-Lernprogramme für ASP.NET](https://asp.net/QuickStart/aspnet/).

Für unser Tutorial müssen wir verwenden einen RequiredFieldValidator in den DetailsView und GridView `ProductName` von TemplateFields und ein RequiredFieldValidator im DetailsView `UnitPrice` TemplateField. Darüber hinaus müssen beide Steuerelemente ein CompareValidator-Steuerelement hinzuzufügende `UnitPrice` von TemplateFields, die sicherstellt, dass der eingegebene Preis verfügt über einen Wert größer als oder gleich 0 und wird in einem gültigen Währungsformat angezeigt.

> [!NOTE]
> Während ASP.NET 1.x mussten diese gleichen fünf Validierungssteuerelemente, ASP.NET 2.0 wurde eine Reihe von Verbesserungen hinzugefügt, die dem hauptblatt zwei Client-seitige Skript Unterstützung von Browsern als Internet Explorer sowie die Möglichkeit, Steuerelementen zur gültigkeitsprüfung Partition auf einer Seite in Überprüfung von Gruppen. Weitere Informationen zu den neuen Features der Validierung-Steuerelement in 2.0 finden Sie unter [Analyse der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Zunächst fügen die erforderlichen Validierungssteuerelemente auf der `EditItemTemplate` s in den GridView von TemplateFields. Um dies zu erreichen, klicken Sie auf die Vorlagen bearbeiten aus den GridView Smarttag, um die Vorlage Bearbeitungsschnittstelle anzuzeigen. Von hier aus können Sie die Vorlage so bearbeiten Sie in der Dropdown-Liste auswählen. Da wir die Bearbeitungsschnittstelle erweitern möchten, müssen Sie Steuerelemente zur gültigkeitsprüfung zum Hinzufügen der `ProductName` und `UnitPrice`des `EditItemTemplate` s.


[![Wir müssen die ProductName "und" der UnitPrice EditItemTemplates erweitern](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Abbildung 4**: möchten wir erweitern die `ProductName` und `UnitPrice`des `EditItemTemplate` s ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


In der `ProductName` `EditItemTemplate`, fügen Sie einen RequiredFieldValidator durch Ziehen aus der Toolbox in die Vorlage bearbeiten-Oberfläche, nachdem das Textfeld platzieren.


[![Das ProductName EditItemTemplate einen RequiredFieldValidator hinzugefügt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Abbildung 5**: einen RequiredFieldValidator zum Hinzufügen der `ProductName` `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Alle Validierungssteuerelemente funktionieren durch Überprüfen der Eingabe eines einzelnen ASP.NET Web-Steuerelements. Aus diesem Grund müssen wir angeben, dass für das Textfeld auf das soeben hinzugefügte RequiredFieldValidator-Steuerelement überprüfen soll die `EditItemTemplate`; dies geschieht durch Festlegen des Validierungssteuerelements [ControlToValidate-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) auf der `ID` des entsprechenden Websteuerelements. Das Textfeld verfügt derzeit über das Recht unbestimmter `ID` von `TextBox1`, aber ändern Sie es in eine passendere. Klicken Sie auf das Textfeld in der Vorlage, und ändern Sie dann im Eigenschaftenfenster die `ID` aus `TextBox1` zu `EditProductName`.


[![Ändern Sie den Text des Textfelds-ID in EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Abbildung 6**: Ändern des Textfelds `ID` zu `EditProductName` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Legen Sie als Nächstes des RequiredFieldValidator-Steuerelement `ControlToValidate` Eigenschaft `EditProductName`. Legen Sie schließlich die ["ErrorMessage"-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "müssen Sie den Namen des Produkts bereitstellen" und die [Texteigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) zu "\*". Die `Text` Eigenschaftswert, wenn angegeben, wird der Text, der durch das Validierungssteuerelement angezeigt wird, wenn die Validierung fehlschlägt. Die `ErrorMessage` Eigenschaftswert, der erforderlich ist, wird von dem Steuerelement ValidationSummary verwendet, wenn die `Text` -Eigenschaftswert fehlt, die `ErrorMessage` Eigenschaftswert ist auch der vom Validierungssteuerelement bei ungültiger Eingabe angezeigte Text.

Nachdem diese drei Eigenschaften, der das RequiredFieldValidator-Steuerelement festlegen, sollte Ihr Bildschirm Abbildung 7 ähneln.


[![Legen Sie des RequiredFieldValidator-Steuerelement ControlToValidate, ErrorMessage und Eigenschaften von Text](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Abbildung 7**: Legen Sie des RequiredFieldValidator-Steuerelement `ControlToValidate`, `ErrorMessage`, und `Text` Eigenschaften ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Mit das RequiredFieldValidator-Steuerelement hinzugefügt, die `ProductName` `EditItemTemplate`, bleibt die erforderlichen Überprüfung hinzufügen wird der `UnitPrice` `EditItemTemplate`. Da wir, dass für diese Seite, entschieden haben die `UnitPrice` ist optional, wenn einen Datensatz bearbeiten, es müssen keinen RequiredFieldValidator hinzufügen. Müssen wir tun, jedoch eine CompareValidator, um sicherzustellen, dass Hinzufügen der `UnitPrice`, sofern angegeben, wird ordnungsgemäß als Währung formatiert und ist größer als oder gleich 0.

Vor dem Hinzufügen der CompareValidator auf die `UnitPrice` `EditItemTemplate`, zuerst ändern wir das TextBox-Websteuerelements-ID aus `TextBox2` zu `EditUnitPrice`. Nach dieser Änderung festlegen CompareValidator, Hinzufügen der `ControlToValidate` Eigenschaft, um `EditUnitPrice`, dessen `ErrorMessage` Eigenschaft auf "der Preis muss größer als oder gleich 0 (null) sein und darf keine enthalten das Währungssymbol", und die zugehörige `Text` Eigenschaft "\*".

Gibt an, dass die `UnitPrice` Wert muss größer als oder gleich 0, Festlegen des CompareValidator [Operatoreigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) zu `GreaterThanEqual`, dessen [ValueToCompare-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" und dessen [ Type-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) zu `Currency`. Die folgende deklarative Syntax stellt die `UnitPrice` TemplateFields `EditItemTemplate` nachdem diese Änderungen vorgenommen wurden:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Öffnen Sie nachdem Sie diese Änderungen haben die Seite in einem Browser aus. Wenn Sie versuchen, den Namen des weglassen, oder geben einen ungültigen preiswert, wenn Sie ein Produkt zu bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 8 gezeigt, wird ein preiswert, der das Währungssymbol wie z. B. 19,95 $ enthält als ungültig angesehen. CompareValidator `Currency` `Type` ermöglicht das Trennzeichen für Ziffern (z. B. Kommas oder Punkte, je nach den kultureinstellungen) und eine führende Plus- oder Minuszeichen (-), funktioniert jedoch *nicht* ein Währungssymbol zulassen. Dieses Verhalten kann Benutzer perplex, während die Bearbeitungsschnittstelle derzeit rendert die `UnitPrice` verwenden das Währungsformat.

> [!NOTE]
> Denken Sie daran, dass in der *einfügen, aktualisieren und Löschen von Ereignissen zu zugeordneten* Tutorial legen wir die BoundField des `DataFormatString` Eigenschaft `{0:c}` um es als Währung zu formatieren. Darüber hinaus legen wir die `ApplyFormatInEditMode` -Eigenschaft auf "true", verursacht der GridView des bearbeiten-Schnittstelle zum Formatieren der `UnitPrice` als Währung. Wenn Sie die BoundField in ein TemplateField konvertieren, wird Visual Studio diese Einstellungen angegeben und formatiert des Text des Textfelds `Text` Eigenschaft als eine Währung, die mit der Datenbindungssyntax `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe angezeigt.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Abbildung 8**: ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Während der Überprüfung funktioniert als-ist, muss der Benutzer das Währungssymbol manuell zu entfernen, wenn einen Datensatz bearbeiten, die nicht zulässig ist. Zur Behebung des Problems, gibt es drei Optionen:

1. Konfigurieren der `EditItemTemplate` , damit die `UnitPrice` ist nicht als Währung formatiert.
2. Ermöglicht dem Benutzer ein Währungssymbol eingeben, indem CompareValidator entfernen und Ersetzen durch eine RegularExpressionValidator ordnungsgemäß für eine ordnungsgemäß formatierte Currency-Wert für die Überprüfung. Das Problem hier besteht darin, dass der reguläre Ausdruck, der einen Währungswert überprüfen nicht sehr und müsste das Schreiben von Code aus, wenn wir die kultureinstellungen integrieren möchten.
3. Entfernen Sie das Validierungssteuerelement und basieren auf serverseitige Validierungslogik in des GridView `RowUpdating` -Ereignishandler.

Kehren Sie mit der Option #1 für diese Übung. Derzeit den `UnitPrice` als eine Währung aufgrund der Datenbindungsausdruck für das Textfeld in die `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Ändern Sie die Bind-Anweisung, um `Bind("UnitPrice", "{0:n2}")`, formatiert das Ergebnis als Zahl mit zwei Ziffern für die Genauigkeit. Dies kann erfolgen direkt über die deklarative Syntax oder durch Klicken auf den Link "DataBindings bearbeiten" aus der `EditUnitPrice` im Textfeld die `UnitPrice` TemplateFields `EditItemTemplate` (Siehe Abbildungen 9 und 10).


[![Klicken Sie auf das Textfelds DataBindings bearbeiten link](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie auf das Textfelds DataBindings bearbeiten Link ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Geben Sie den Formatbezeichner in der Bind-Anweisung](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Abbildung 10**: Geben Sie den Formatbezeichner in der `Bind` Anweisung ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Durch diese Änderung der formatierte Preis in die Bearbeitungsschnittstelle enthält Kommas als das Gruppentrennzeichen und einen Punkt als Dezimaltrennzeichen, aber das Währungssymbol lässt.

> [!NOTE]
> Die `UnitPrice` `EditItemTemplate` einen RequiredFieldValidator, sodass das Postback, stellen Sie sicher und Update Logik, um zu beginnen. sind nicht enthalten. Allerdings die `RowUpdating` -Ereignishandler aus kopiert die *Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen* Tutorial enthält eine programmgesteuerte Überprüfung, die wird, dass sichergestellt die `UnitPrice` wird bereitgestellt. Entfernen Sie diese Logik, behalten sie in gerne-ist, oder fügen Sie einen RequiredFieldValidator auf die `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Schritt 4: Zusammenfassung der Probleme mit der Dateneingabe

Zusätzlich zu den fünf Validation-Steuerelementen, enthält ASP.NET die [ValidationSummary-Steuerelement](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), welche zeigt die `ErrorMessage` s von diesen Steuerelementen zur gültigkeitsprüfung, die ungültige Daten erkannt. Diese zusammenfassenden Daten können als Text auf der Webseite oder über ein modales, clientseitige Messagebox angezeigt werden. Erweitern wir jetzt dieses Lernprogramm enthält eine Zusammenfassung der Überprüfungsprobleme der clientseitigen-Messagebox.

Ziehen Sie zu diesem Zweck ein ValidationSummary-Steuerelement aus der Toolbox in den Designer. Der Speicherort des Validierungssteuerelements ist unerheblich, da wir konfigurieren jetzt, um die Zusammenfassung nur als eine Messagebox anzeigt. Nach Hinzufügen des Steuerelements, legen Sie seine [ShowSummary-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) zu `False` und die zugehörige [ShowMessageBox Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) zu `True`. Durch diese hinzufügen werden Validierungsfehler in einer clientseitigen Messagebox zusammengefasst.


[![Fehler bei der Validierung werden in einer Client-Side-Messagebox zusammengefasst.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Abbildung 11**: die Validierungsfehler werden in einer Client-Side-Messagebox zusammengefasst ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Schritt 5: DetailsView Steuerelemente zur gültigkeitsprüfung hinzugefügt`InsertItemTemplate`

Alle diese bleibt in diesem Tutorial wird die Validierungssteuerelemente Einfügen DetailsViews-Schnittstelle hinzugefügt. Beim Hinzufügen von Steuerelementen zur gültigkeitsprüfung DetailsViews-Vorlagen ist identisch mit den in Schritt 3 überprüft; aus diesem Grund werden wir durch die Aufgabe in diesem Schritt breeze. Wie wir mit GridView `EditItemTemplate` s, sollten Sie zum Umbenennen der `ID` s der TextBox-Elemente aus der unbestimmter `TextBox1` und `TextBox2` zu `InsertProductName` und `InsertUnitPrice`.

Einen RequiredFieldValidator zum Hinzufügen der `ProductName` `InsertItemTemplate`. Festlegen der `ControlToValidate` zu der `ID` des Textfelds in der Vorlage seine `Text` Eigenschaft, um "\*" und dessen `ErrorMessage` Eigenschaft auf "müssen Sie den Namen des Produkts bereitstellen".

Da die `UnitPrice` ist für diese Seite erforderlich, wenn Sie einen neuen Datensatz hinzufügen, einen RequiredFieldValidator zum Hinzufügen der `UnitPrice` `InsertItemTemplate`wird durch das Festlegen der `ControlToValidate`, `Text`, und `ErrorMessage` Eigenschaften entsprechend. Fügen Sie schließlich ein CompareValidator-Steuerelement auf der `UnitPrice` `InsertItemTemplate` , konfigurieren die `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, und `ValueToCompare` Eigenschaften, wie wir mit der `UnitPrice`des CompareValidator in des GridView `EditItemTemplate`.

Ein neues Produkt kann nicht zum System hinzugefügt werden, wenn der Name nicht angegeben ist, oder wenn der Preis berechnet eine negative Zahl ist oder illegal formatiert werden, nach dem Hinzufügen von diesen Steuerelementen zur gültigkeitsprüfung.


[![Validierungslogik wurde Einfügen DetailsViews-Schnittstelle hinzugefügt](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Abbildung 12**: Einfügen DetailsViews-Schnittstelle Validierungslogik hinzugefügt wurde ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Schritt 6: Die Validierungssteuerelemente in Validierungsgruppen partitionieren

Die Seite besteht aus zwei logisch unterschiedlichen Gruppen von Steuerelementen zur gültigkeitsprüfung: an die GridView entsprechen den bearbeiten-Schnittstelle und, DetailsView entsprechen der Schnittstelle einfügen. Wenn ein Postback auftritt, wird standardmäßig *alle* Validierungssteuerelemente auf der Seite überprüft werden. Allerdings sollten nicht beim Bearbeiten eines Datensatzes Einfügen von Schnittstelle DetailsView Steuerelementen zur gültigkeitsprüfung überprüfen. Abbildung 13 zeigt unseren aktuellen Dilemma aus, wenn ein Benutzer ein Produkt mit durchaus zulässige Werte bearbeitet, wird durch Klicken auf Aktualisieren tritt ein Validierungsfehler verursacht, da die Werte für Name und Preis in der Schnittstelle einfügen leer sind.


[![Aktualisieren eines Produkts führt dazu, dass Validierungssteuerelementen für die einfügende Schnittstelle zum Auslösen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Abbildung 13**: Aktualisieren eines Produkts führt dazu, dass die einfügen-Schnittstelle von Validierungssteuerelementen auszulösende ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Die Validierungssteuerelemente in ASP.NET 2.0 können partitioniert werden, in Validierungsgruppen über ihre `ValidationGroup` Eigenschaft. Um einen Satz von Steuerelementen zur gültigkeitsprüfung in einer Gruppe zuzuordnen, legen Sie einfach ihre `ValidationGroup` Eigenschaft auf den gleichen Wert. Legen Sie für unser Tutorial die `ValidationGroup` Eigenschaften der Steuerelemente zur gültigkeitsprüfung in den GridView von TemplateFields zu `EditValidationControls` und `ValidationGroup` Eigenschaften DetailsViews von TemplateFields zu `InsertValidationControls`. Diese Änderungen können direkt im deklarativen Markup ausgeführt werden, oder Bearbeiten über das Eigenschaftenfenster bei Verwendung des Designers des vorlagenschnittstelle.

Neben der Überprüfung Steuerelemente, die Schaltfläche und schaltflächenbezogene Steuerelemente in ASP.NET 2.0 auch enthalten eine `ValidationGroup` Eigenschaft. Einer Validierungsgruppe Validierungssteuerelemente auf Gültigkeit überprüft nur wenn ein Postback über eine Schaltfläche ausgelöst wird mit dem gleichen `ValidationGroup` Einstellung der Eigenschaft. Z. B. in der Reihenfolge für DetailsViews-Einfügen-Schaltfläche zum Auslösen der `InsertValidationControls` Validierungsgruppe wir die CommandFields festlegen müssen `ValidationGroup` Eigenschaft `InsertValidationControls` (siehe Abbildung 14). Legen Sie zudem des GridView CommandFields `ValidationGroup` Eigenschaft `EditValidationControls`.


[![Set DetailsView ist CommandFields ValidationGroup-Eigenschaft, um InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Abbildung 14**: Festlegen des DetailsView CommandFields `ValidationGroup` Eigenschaft `InsertValidationControls` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Nach diesen Änderungen sollte das DetailsView und GridView von TemplateFields und CommandFields etwa wie folgt aussehen:

Die DetailsView von TemplateFields und CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

GridView CommandField und von TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

An diesem Punkt werden Steuerelemente zur gültigkeitsprüfung des bearbeiten-spezifische nur bei der Behebung des Problems markiert durch den in Abbildung 13 DetailsView Einfügen-Schaltfläche klicken, wird nur angezeigt, wenn die GridView Schaltfläche "Aktualisieren" geklickt wird und Insert-spezifische Validierung Steuerelemente Fire ausgelöst. Unsere ValidationSummary-Steuerelement zeigt jedoch durch diese Änderung nicht mehr bei der Eingabe von ungültiger Daten. ValidationSummary-Steuerelement enthält auch eine `ValidationGroup` -Eigenschaft und nur zeigt zusammenfassende Informationen für die Validierungssteuerelemente in der Gruppe für die Überprüfung. Aus diesem Grund müssen wir haben zwei Steuerelementen zur gültigkeitsprüfung auf dieser Seite eine für die `InsertValidationControls` Validierungsgruppe und eine für `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Mit folgender Ergänzung für unser Tutorial abgeschlossen ist.

## <a name="summary"></a>Zusammenfassung

Zwar BoundFields sowohl eine Schnittstelle zum Einfügen und bearbeiten können, ist die Schnittstelle nicht anpassbar. Normalerweise möchten wir das Bearbeiten und Einfügen-Schnittstelle, um sicherzustellen, dass der Benutzer die erforderliche Eingaben in einem legal-Format Gibt Steuerelemente zur gültigkeitsprüfung hinzugefügt. Zu diesem Zweck müssen wir die BoundFields in von TemplateFields konvertieren, und fügen Steuerelemente zur gültigkeitsprüfung der entsprechenden Vorlagen hinzu. In diesem Tutorial, die wir Erweitert des Beispiel aus der *Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen* Tutorials: dem Hinzufügen von Steuerelementen zur gültigkeitsprüfung zur beide DetailsView der Schnittstelle und GridView einfügen Bearbeiten die-Schnittstelle. Darüber hinaus haben wir gesehen, wie zum Anzeigen der Zusammenfassung Validierungsinformationen, die mit dem ValidationSummary-Steuerelement und die Validierungssteuerelemente auf der Seite in unterschiedlichen Validierungsgruppen zu partitionieren.

Wie wir in diesem Tutorial gesehen haben, können von TemplateFields bearbeiten und Einfügen von Schnittstellen ergänzt werden, um Steuerelemente zur gültigkeitsprüfung einzuschließen. Von TemplateFields kann auch erweitert werden um zusätzliche Eingabe Websteuerelemente, aktivieren das Textfeld durch ein geeigneter-Steuerelement ersetzt werden. In unserem nächsten Tutorial erfahren Sie, wie das TextBox-Steuerelement mit einem datengebundenen DropDownList-Steuerelement, eignet sich ideal, wenn einen Fremdschlüssel zu bearbeiten, ersetzen (z. B. `CategoryID` oder `SupplierID` in die `Products` Tabelle).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Liz Shulok und Zack Jones. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Weiter](customizing-the-data-modification-interface-vb.md)
