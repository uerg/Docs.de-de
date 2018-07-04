---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Hinzufügen von Validierungssteuerelementen zu DataList-Steuerelement die Schnittstelle (VB) bearbeiten | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial sehen wir, wie einfach es ist, die Steuerelemente zur gültigkeitsprüfung des DataList-Steuerelement EditItemTemplate hinzufügen, um eine weitere narrensicher Bearbeitung Benutzer "int". Geben Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b06020128daa01c58b27639ff1db23febc0cba9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397589"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Hinzufügen von Validierungssteuerelementen zu der die Bearbeitung von DataList (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) oder [PDF-Datei herunterladen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> In diesem Tutorial erfahren Sie, wie einfach es ist, die Steuerelemente zur gültigkeitsprüfung des DataList-Steuerelement EditItemTemplate hinzufügen, um eine weitere narrensicher Bearbeitung Benutzeroberfläche bereitzustellen.


## <a name="introduction"></a>Einführung

Im DataList-Steuerelement bisher Bearbeitung von Tutorials enthalten die Bearbeitung von Schnittstellen DataList-Steuerelementen unterstützt keine proaktive Validierung von Benutzereingaben, obwohl ungültige z. B. einen fehlenden Produktnamen oder negative Preis führt zu einer Ausnahme Benutzereingabe. In der [vorherigen Lernprogramm](handling-bll-and-dal-level-exceptions-vb.md) untersuchten wir Code an die Datenliste s zur Ausnahmebehandlung hinzufügen `UpdateCommand` -Ereignishandler zum Abfangen und ordnungsgemäß anzeigen von Informationen zu allen Ausnahmen, die ausgelöst wurden. Allerdings im Idealfall würde die Bearbeitungsschnittstelle Validierungssteuerelemente aus, um zu verhindern, dass einen Benutzer im vornherein eingeben von solchen ungültigen Daten enthalten.

In diesem Tutorial werden wir sehen, wie einfach es ist zum Hinzufügen von Steuerelementen zur gültigkeitsprüfung an die Datenliste s `EditItemTemplate` um eine weitere narrensicher Bearbeitung Benutzeroberfläche bereitzustellen. Genauer gesagt wird in diesem Tutorial verwendet das Beispiel im vorherigen Tutorial erstellt und ergänzt die Bearbeitungsschnittstelle entsprechende Validierungsfehlermeldung enthält.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Schritt 1: Replizieren das Beispiel aus[Ausnahmebehandlung auf BLL- und DAL-Ebene](handling-bll-and-dal-level-exceptions-vb.md)

In der [behandeln Geschäftslogikschicht und Ausnahmen auf DAL-Ebene](handling-bll-and-dal-level-exceptions-vb.md) Tutorial, die wir erstellt haben, eine Seite, die die Namen und Preise der Produkte in einem DataList-Steuerelement zwei Spalten, die bearbeitet werden aufgeführt. Unser Ziel für dieses Tutorial ist die s Bearbeitung von DataList Einbeziehung von Steuerelementen zur gültigkeitsprüfung zu erweitern. Insbesondere wird unsere Validierungslogik:

- Erfordert, dass der Name des Produkts s bereitgestellt werden
- Stellen Sie sicher, dass der eingegebene Wert für den Preis einer gültigen Währungsformat ist
- Stellen Sie sicher, dass den eingegebene Wert für der Preis größer als oder gleich 0 (null), da eine Negative ist `UnitPrice` Wert ist nicht zulässig

Bevor wir weiter erweitern im vorherige Beispiel um regulierte sehen können, müssen wir zuerst auf das Beispiel aus Replizieren der `ErrorHandling.aspx` auf der Seite die `EditDeleteDataList` Ordner auf der Seite für dieses Tutorial `UIValidation.aspx`. Um dies zu erreichen, wir beide kopiert müssen, die `ErrorHandling.aspx` auf der Seite s deklaratives Markup und Quellcode. Kopieren Sie zuerst die deklarativen Markup durch die folgenden Schritte ausführen:

1. Öffnen der `ErrorHandling.aspx` Seite in Visual Studio
2. Wechseln Sie zur Seite "s" deklarativen Markup (klicken Sie auf die Schaltfläche "Quelle" am unteren Rand der Seite ")
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 32), wie in Abbildung 1 dargestellt.


[![Kopieren Sie den Text innerhalb der &lt;Asp: Content&gt; Steuerelement](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 2**: Kopieren Sie den Text innerhalb der `<asp:Content>` Control ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Öffnen der `UIValidation.aspx` Seite
2. Wechseln Sie zur Seite s deklarativen markup
3. Fügen Sie den Text innerhalb der `<asp:Content>` Steuerelement.

Öffnen Sie zum Kopieren von über den Quellcode der `ErrorHandling.aspx.vb` Seite, und kopieren Sie nur den Text *in* der `EditDeleteDataList_ErrorHandling` Klasse. Kopieren Sie die drei Ereignishandler (`Products_EditCommand`, `Products_CancelCommand`, und `Products_UpdateCommand`) zusammen mit den `DisplayExceptionDetails` -Methode, aber möchten **nicht** kopieren Sie die Klassendeklaration oder `using` Anweisungen. Fügen Sie den kopierten Text *in* der `EditDeleteDataList_UIValidation` -Klasse im `UIValidation.aspx.vb`.

Nach der Umstellung auf den Inhalt und Code aus `ErrorHandling.aspx` zu `UIValidation.aspx`, können Sie die Seiten in einem Browser zu testen. Sie sollten die Ausgabe sowie die gleiche Funktionalität in jede dieser zwei Seiten (siehe Abbildung 2).


[![Die Seite UIValidation.aspx imitiert die Funktionalität in ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: die `UIValidation.aspx` Seite imitiert die Funktionalität in `ErrorHandling.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Schritt 2: Hinzufügen der Steuerelemente zur gültigkeitsprüfung auf das DataList-s-EditItemTemplate

Wenn Dateneingabeformularen erstellen zu können, ist es wichtig, dass Benutzer alle erforderlichen Felder eingeben und, dass alle ihre bereitgestellten Eingaben für rechtliche, korrekt formatierte Werte sind. Um sicherzustellen, dass ein Benutzer s Eingaben gültig sind, bietet ASP.NET die fünf integrierten Validierungssteuerelemente, die zur Überprüfung des Werts, der ein einzelnes Eingabesteuerelement Web entwickelt wurden:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) wird sichergestellt, dass ein Wert bereitgestellt wurde
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) überprüft einen Wert für eine andere Web-Control-Wert oder einen konstanten Wert oder wird sichergestellt, dass das s Wertformat für einen angegebenen Datentyp gültig ist
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) wird sichergestellt, dass ein Wert innerhalb eines Bereichs von Werten ist.
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) überprüft einen Wert für eine [reguläre Ausdrücke](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) überprüft einen Wert für eine benutzerdefinierte, eine benutzerdefinierte Methode

Weitere Informationen zu diesen fünf Steuerelemente zurückgreifen der [Hinzufügen von Steuerelementen zur gültigkeitsprüfung zum Bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) Lernprogramm oder sehen Sie sich die [Steuerelemente zur gültigkeitsprüfung Abschnitt](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) von der [ASP.NET Schnellstarttutorials](https://quickstarts.asp.net).

Für unser Tutorial müssen wir verwenden ein RequiredFieldValidator, um sicherzustellen, dass ein Wert für den Namen des Produkts angegeben wurde und eine CompareValidator sicherstellen, dass der eingegebene Preis verfügt über einen Wert größer als oder gleich 0 und wird in einem gültigen Währungsformat angezeigt.

> [!NOTE]
> Während ASP.NET 1.x mussten diese gleichen fünf Validierungssteuerelemente, ASP.NET 2.0 wurde eine Reihe von Verbesserungen hinzugefügt, der Hauptseite für Browser neben dem Internet Explorer und die Möglichkeit, die Validierungssteuerelemente für Partition auf einer Seite in zwei des clientseitigen Skripts zu unterstützen Überprüfung von Gruppen. Weitere Informationen zu den neuen Features der Validierung-Steuerelement in 2.0 finden Sie unter [Analyse der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


S durch Hinzufügen der erforderlichen Überprüfungssteuerelemente an die Datenliste s beginnen können `EditItemTemplate`. Diese Aufgabe kann mithilfe des Designers durch Klicken auf den Link "Vorlagen bearbeiten" aus dem DataList-s-Smarttag oder mithilfe der deklarativen Syntax erfolgen. Lassen Sie s durchlaufen Sie den Prozess über die Option "Vorlagen bearbeiten" die Entwurfsansicht zu sehen. Nach dem Auswählen der Bearbeitung von DataList-Steuerelement s `EditItemTemplate`, fügen Sie einen RequiredFieldValidator durch Ziehen aus der Toolbox in die Vorlage bearbeiten-Schnittstelle, platzieren sie nach der `ProductName` Textfeld.


[![Fügen Sie einen RequiredFieldValidator das EditItemTemplate nach ProductName Textfeld](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: einen RequiredFieldValidator zum Hinzufügen der `EditItemTemplate After` der `ProductName` Textfeld ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Alle Validierungssteuerelemente funktionieren durch Überprüfen der Eingabe eines einzelnen ASP.NET Web-Steuerelements. Aus diesem Grund müssen wir angeben, dass das RequiredFieldValidator-Steuerelement uns hinzugefügten, für überprüfen soll die `ProductName` Textfeld; Dies erfolgt durch das Validierungssteuerelement s festlegen [ `ControlToValidate` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) auf die `ID` von das entsprechende Steuerelement für Web (`ProductName`, in diesem Fall). Legen Sie als Nächstes die [ `ErrorMessage` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) , müssen Sie den Namen des Produkts s bereitstellen und die [ `Text` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) zu \*. Die `Text` Eigenschaftswert, wenn angegeben, wird der Text, der durch das Validierungssteuerelement angezeigt wird, wenn die Validierung fehlschlägt. Die `ErrorMessage` Eigenschaftswert, der erforderlich ist, wird von dem Steuerelement ValidationSummary verwendet, wenn die `Text` -Eigenschaftswert fehlt, die `ErrorMessage` -Eigenschaftswert angezeigt wird, durch das Validierungssteuerelement bei ungültiger Eingabe.

Nach dem Festlegen dieser drei Eigenschaften des das RequiredFieldValidator-Steuerelement, sollte Ihr Bildschirm dem Beispiel in Abbildung 4 aussehen.


[![Legen Sie die RequiredFieldValidator s ControlToValidate, ErrorMessage und Eigenschaften von Text](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: Legen Sie das RequiredFieldValidator-s `ControlToValidate`, `ErrorMessage`, und `Text` Eigenschaften ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


Mit das RequiredFieldValidator-Steuerelement hinzugefügt, die `EditItemTemplate`, bleibt die erforderlichen Validierungen für s Produktpreis Textfeld ist. Da die `UnitPrice` ist optional, wenn einen Datensatz zu bearbeiten, wir Raten t muss einen RequiredFieldValidator hinzugefügt. Müssen wir tun, jedoch eine CompareValidator, um sicherzustellen, dass Hinzufügen der `UnitPrice`, sofern angegeben, wird ordnungsgemäß als Währung formatiert und ist größer als oder gleich 0.

CompareValidator in Hinzufügen der `EditItemTemplate` und legen Sie seine `ControlToValidate` Eigenschaft `UnitPrice`, dessen `ErrorMessage` Eigenschaft mit dem Preis muss größer als oder gleich 0 (null) und kann nicht das Währungssymbol enthalten und die zugehörige `Text` Eigenschaft \*. Gibt an, dass die `UnitPrice` Wert muss größer als oder gleich 0, legen Sie die s CompareValidator [ `Operator` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) zu `GreaterThanEqual`, dessen [ `ValueToCompare` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) auf 0 (null) und die [ `Type` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) zu `Currency`.

Nach dem Hinzufügen dieser beiden Validierungssteuerelemente, DataList-Steuerelement s `EditItemTemplate` deklarative Syntax des s sollte etwa wie folgt aussehen:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Öffnen Sie nachdem Sie diese Änderungen haben die Seite in einem Browser aus. Wenn Sie versuchen, den Namen des weglassen, oder geben einen ungültigen preiswert, wenn Sie ein Produkt zu bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 5 gezeigt, wird ein preiswert, der das Währungssymbol wie z. B. 19,95 $ enthält als ungültig angesehen. CompareValidator s `Currency` `Type` ermöglicht das Trennzeichen für Ziffern (z. B. Kommas oder Punkte, je nach den kultureinstellungen) und eine führende Plus- oder Minuszeichen (-), funktioniert jedoch *nicht* ein Währungssymbol zulassen. Dieses Verhalten kann Benutzer perplex, während die Bearbeitungsschnittstelle derzeit rendert die `UnitPrice` verwenden das Währungsformat.


[![Ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe angezeigt.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Während der Überprüfung funktioniert als-ist, muss der Benutzer das Währungssymbol manuell zu entfernen, wenn einen Datensatz bearbeiten, die nicht zulässig ist. Darüber hinaus treten ungültige Schnittstelle die Eingaben in die Bearbeitung, weder die Aktualisierung noch "Abbrechen" Schaltflächen einen Postback wird aufgerufen, wenn auf Sie geklickt. Im Idealfall würde die Schaltfläche "Abbrechen" DataList-Steuerelement in den Zustand vor der Bearbeitung, unabhängig von der Gültigkeit der Benutzereingabe s zurück. Darüber hinaus müssen wir sicherstellen, dass Daten auf der Seite s gültig ist, bevor Sie aktualisieren die Produktinformationen im DataList-Steuerelement s `UpdateCommand` Ereignishandler die clientbasierte Logik kann, von Benutzern, deren Browser umgangen werden entweder Einbau zusätzlichen t unterstützt JavaScript oder über die, Steuerelemente zur gültigkeitsprüfung die Unterstützung deaktiviert ist.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Entfernen das Währungssymbol aus dem EditItemTemplate s UnitPrice-Textfeld

Bei Verwendung der s CompareValidator `Currency``Type`, die Eingabe überprüft wird, darf keiner Währungssymbole enthalten. Das Vorhandensein solcher Symbole bewirkt, dass die CompareValidator, um die Eingabe als ungültig zu markieren. Unsere Bearbeitungsschnittstelle enthält jedoch derzeit ein Währungssymbol in die `UnitPrice` TextBox, d. h. der Benutzer muss das Währungssymbol explizit entfernen, bevor Sie ihre Änderungen speichern. Zur Behebung des Problems gibt es drei Optionen:

1. Konfigurieren der `EditItemTemplate` , damit die `UnitPrice` TextBox-Wert ist nicht als Währung formatiert.
2. Ermöglicht dem Benutzer ein Währungssymbol CompareValidator entfernen und Ersetzen durch eine RegularExpressionValidator, die prüft, ob eine ordnungsgemäß formatierte Währungswert eingeben. Die Schwierigkeit besteht darin, dass der reguläre Ausdruck, der einen Währungswert überprüfen nicht so einfach wie das CompareValidator und müsste das Schreiben von Code aus, wenn wir die kultureinstellungen integrieren möchten.
3. Entfernen Sie das Validierungssteuerelement und basieren auf benutzerdefinierte serverseitige Validierungslogik in den GridView-s `RowUpdating` -Ereignishandler.

Lassen Sie s mit Option 1 für dieses Tutorial zu wechseln. Derzeit den `UnitPrice` ist aufgrund der Datenbindungsausdruck für das Textfeld als Währungswert formatiert die `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Ändern der `Eval` Anweisung `Eval("UnitPrice", "{0:n2}")`, formatiert das Ergebnis als Zahl mit zwei Ziffern für die Genauigkeit. Dies kann erfolgen direkt über die deklarative Syntax oder durch Klicken auf den Link "DataBindings bearbeiten" aus der `UnitPrice` Textfeld im DataList-Steuerelement s `EditItemTemplate`.

Durch diese Änderung der formatierte Preis in die Bearbeitungsschnittstelle enthält Kommas als das Gruppentrennzeichen und einen Punkt als Dezimaltrennzeichen, aber das Währungssymbol lässt.

> [!NOTE]
> Wenn Sie das Währungsformat aus der bearbeitbaren Schnittstelle entfernen, finde ich hilfreich sein, die das Währungssymbol als außerhalb der TextBox-Text einfügen. Dies dient als Hinweis für den Benutzer, den sie nicht benötigen, um das Währungssymbol bereitzustellen.


## <a name="fixing-the-cancel-button"></a>Beheben die Schaltfläche "Abbrechen"

Standardmäßig geben die Validierungssteuerelemente für das Web JavaScript erstellt, um auf die clientseitige Validierung. Wenn eine Schaltfläche, LinkButton oder ImageButton geklickt wird, werden die Validierungssteuerelemente auf der Seite auf der Clientseite überprüft, bevor das Postback stattfindet. Wenn Sie ungültige Daten vorhanden sind, wird das Postback abgebrochen. Für bestimmte Schaltflächen kann jedoch die Gültigkeit der Daten unerheblich sein; In diesem Fall ist das Postback aufgrund von ungültigen Daten abgebrochen ein Ärgernis.

Die Schaltfläche "Abbrechen" ist ein Beispiel dafür. Stellen Sie sich, dass ein Benutzer ungültige Daten eingibt, z.B. das Auslassen der Name des Produkts s, und klicken Sie dann entscheidet sich Julie t des Produkts nach allen speichern möchten und die Schaltfläche "Abbrechen" klickt. Derzeit wird die Schaltfläche "Abbrechen" die Validierungssteuerelemente auf der Seite der gemeldet wird, dass der Name des Produkts ist nicht vorhanden, und zu verhindern, dass das Postback ausgelöst. Geben Sie Text in unserer Benutzer muss die `ProductName` Textfeld nur aus dem Prozess der Bearbeitung abbrechen.

Glücklicherweise verfügen über die Schaltfläche, LinkButton und ImageButton eine [ `CausesValidation` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) anzugeben, dass können, und zwar unabhängig davon, ob Sie auf die Schaltfläche mit der sollten die Validierungslogik initiieren (standardmäßig `True`). Legen Sie die Schaltfläche "Abbrechen"-s `CausesValidation` Eigenschaft `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Sicherstellen der Eingaben in die UpdateCommand-Ereignishandler gültig sind

Aufgrund der Client-seitige Skript ausgegeben, die von den Validation-Steuerelementen, wenn ein Benutzer ungültige Eingabe eingibt, die Validierungssteuerelemente Abbrechen Postbacks durch Schaltfläche LinkButton, initiiert, oder ImageButton Steuerelemente, deren `CausesValidation` Eigenschaften sind `True` (die (Standard). Wenn ein Benutzer Sie mit einem veralteten Browser oder eine besuchen ist, deren Unterstützung für JavaScript deaktiviert wurde, werden jedoch die clientseitige Überprüfungen nicht ausgeführt.

Alle der ASP.NET-Steuerelemente zur gültigkeitsprüfung wiederholen Sie die Validierungslogik sofort nach dem Postback, und melden Sie die gesamte Gültigkeit der Seite s Eingaben über die [ `Page.IsValid` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Die Seitenfluss besteht jedoch nicht unterbrochen oder beendet wird, in einer Weise basierend auf den Wert der `Page.IsValid`. Als Entwickler ist dafür verantwortlich, um sicherzustellen, dass die `Page.IsValid` Eigenschaft hat den Wert `True` vor dem Fortfahren mit Code, die gültige annimmt, Daten einzugeben.

Wenn ein Benutzer JavaScript deaktiviert hat, die Seite besucht, bearbeitet ein Produkt, gibt einen Preis zu teuer, und klickt auf die Schaltfläche "Aktualisieren", wird die clientseitige Überprüfung umgangen werden, und stellen Sie ein Postback sicher ist. Beim Postback der ASP.NET-Seite s `UpdateCommand` -Ereignishandler ausgeführt wird und eine Ausnahme wird ausgelöst, wenn Sie versuchen zu analysieren, aufwendig zu einem `Decimal`. Da wir haben die Behandlung von Ausnahmen, eine Ausnahme nicht erfolgreich behandelt werden wird, aber wir sollen die ungültigen Daten verhindern könnten über im vornherein von nur fortgesetzt der `UpdateCommand` Ereignishandler Wenn `Page.IsValid` hat den Wert `True`.

Fügen Sie den folgenden Code am Anfang der `UpdateCommand` -Ereignishandler direkt vor der `Try` blockieren:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Durch diese hinzufügen versucht das Produkt aktualisiert werden, nur dann, wenn die übermittelten Daten gültig sind. Die meisten Benutzer, die gewonnen haben t Lage, ungültige Daten aufgrund der Steuerelemente der clientseitigen validierungsskripts postback, aber der Benutzer, deren Browser Einbau zusätzlichen t unterstützt JavaScript oder für die Unterstützung von JavaScript, deaktiviert ist, können die clientseitige Überprüfungen umgehen und senden Sie ungültige Daten.

> [!NOTE]
> Der aufmerksame Leser werden sich erinnern, die beim Aktualisieren von Daten mit GridView wir hat müssen Sie explizit überprüfen die `Page.IsValid` -Eigenschaft in unserer Seite s-Code-Behind-Klasse. Dies ist, da die GridView berät das `Page.IsValid` -Eigenschaft für uns und nur fährt mit dem Update nur, wenn der Wert zurückgegeben `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Schritt 3: Zusammenfassung der Probleme mit der Dateneingabe

Zusätzlich zu den fünf Validation-Steuerelementen, enthält ASP.NET die [ValidationSummary-Steuerelement](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), welche zeigt die `ErrorMessage` s von diesen Steuerelementen zur gültigkeitsprüfung, die ungültige Daten erkannt. Diese zusammenfassenden Daten können als Text auf der Webseite oder über ein modales, clientseitige Messagebox angezeigt werden. Lassen Sie s dieses Tutorial enthält eine Zusammenfassung der Überprüfungsprobleme der clientseitigen-Messagebox zu verbessern.

Ziehen Sie zu diesem Zweck ein ValidationSummary-Steuerelement aus der Toolbox in den Designer. Der Speicherort der ValidationSummary-Steuerelement t unerheblich, da wir erneut zu konfigurieren, um die Zusammenfassung nur als eine Messagebox anzeigt. Nach Hinzufügen des Steuerelements, legen Sie dessen [ `ShowSummary` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) zu `False` und die zugehörige [ `ShowMessageBox` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) zu `True`. Mit folgender Ergänzung, Fehler bei der Validierung in einer clientseitigen Messagebox zusammengefasst sind (siehe Abbildung 6).


[![Fehler bei der Validierung werden in einer Client-Side-Messagebox zusammengefasst.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: die Validierungsfehler werden in einer Client-Side-Messagebox zusammengefasst ([klicken Sie, um das Bild in voller Größe anzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie die Wahrscheinlichkeit von Ausnahmen verringern, indem Sie Steuerelemente zur gültigkeitsprüfung verwenden, um proaktiv sicherzustellen, dass unsere Benutzer Eingaben gültig sind, bevor Sie versuchen, die sie in den Workflow für die Aktualisierung verwenden. ASP.NET bietet fünf Web Validierungssteuerelemente, die entwickelt wurden, um das Überprüfen einer bestimmten Webanforderung steuerelementeingaben s, und melden sich auf die Eingabe-s-Gültigkeit. In diesem Tutorial haben wir verwendet zwei dieser fünf Steuerelemente das RequiredFieldValidator-Steuerelement und CompareValidator, stellen Sie sicher, dass der Name des Produkts s bereitgestellt wurde und der Preis ein Währungsformat mit einem Wert größer als oder gleich 0 (null) haben.

Hinzufügen von Validierungssteuerelementen zu den bearbeiten-Schnittstelle DataList-s ist so einfach wie Drag & Drop auf die `EditItemTemplate` aus der Toolbox, und eine Reihe von Eigenschaften festlegen. Standardmäßig geben die Validierungssteuerelemente automatisch die clientseitige Validierung Skripts; Sie bieten auch eine serverseitige Validierung beim Postback, speichern das kumulierte Ergebnis in der `Page.IsValid` Eigenschaft. Um die clientseitige Überprüfung umgehen, wenn auf eine Schaltfläche, LinkButton oder ImageButton geklickt wird, legen Sie die s `CausesValidation` Eigenschaft `False`. Vor dem Ausführen von Aufgaben mit den Daten, die beim Postback übermittelt, darüber hinaus sicher, dass die `Page.IsValid` -Eigenschaft gibt `True`.

Alle DataList-Steuerelement Tutorials bearbeiten wir haben bisher untersucht hatten sehr einfache Bearbeitung Schnittstellen, ein Textfeld für den Produktnamen s und eine für den Preis. Die Bearbeitungsschnittstelle, kann jedoch eine Mischung aus verschiedenen Web-Steuerelemente, z. B. DropDownList-Steuerelementen, Kalender, Optionsfelder, Kontrollkästchen und So weiter enthalten. In unserem nächsten Tutorial betrachten wir erstellen eine Schnittstelle, die eine Vielzahl von Websteuerelementen verwendet.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Dennis Patterson, Ken Pespisa und Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](handling-bll-and-dal-level-exceptions-vb.md)
> [Weiter](customizing-the-datalist-s-editing-interface-vb.md)
