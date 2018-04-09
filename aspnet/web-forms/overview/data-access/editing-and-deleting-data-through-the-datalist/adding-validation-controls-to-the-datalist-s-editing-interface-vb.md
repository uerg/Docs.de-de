---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Hinzufügen von Steuerelementen für die Validierung zu DataList der Schnittstelle (VB) bearbeiten | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir, wie einfach es ist so das DataList EditItemTemplate Validierungssteuerelemente hinzu, um eine weitere sichere Bearbeitung Benutzer int. bereitzustellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 46ff0b18c1ea24dd73c9e3034c1b5e53f2e6d0c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Hinzufügen von Validierungssteuerelemente zum Bearbeiten der DataList-Schnittstelle (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) oder [PDF herunterladen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> In diesem Lernprogramm sehen wir, wie einfach es ist so das DataList EditItemTemplate Validierungssteuerelemente hinzu, um eine weitere sichere Bearbeitung Benutzeroberfläche bereitzustellen.


## <a name="introduction"></a>Einführung

In DataList bisher Bearbeiten der Lernprogramme haben die Bearbeitung von Schnittstellen DataList-Steuerelementen unterstützt keine proaktive Validierung von Benutzereingaben enthalten, obwohl ungültige z. B. eine fehlende Produktnamen oder negativen Preis zu einer Ausnahme Benutzereingabe. In der [vorherigen Lernprogramm](handling-bll-and-dal-level-exceptions-vb.md) zum Hinzufügen von Ausnahmebehandlungscode zu DataList s untersucht `UpdateCommand` Ereignishandler zum Abfangen und ordnungsgemäß anzeigen von Informationen zu Ausnahmen, die ausgelöst wurden. Allerdings im Idealfall würden die Bearbeitungsoberfläche Validierungssteuerelemente, um zu verhindern, dass einen Benutzer in erster Linie solche ungültigen Daten eingeben enthalten.

In diesem Lernprogramm werden wir sehen, wie einfach es ist das DataList s Validierungssteuerelemente hinzuzufügende `EditItemTemplate` um eine weitere sichere Bearbeitung Benutzeroberfläche bereitzustellen. Insbesondere in diesem Lernprogramm wird im Beispiel, in dem vorherigen Lernprogramm erstellt und ergänzt die Bearbeitung-Schnittstelle, um eine entsprechende Validierung beinhalten.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Schritt 1: Replizieren das Beispiel aus[Ausnahmebehandlung BLL und DAL-Ebene](handling-bll-and-dal-level-exceptions-vb.md)

In der [Behandlung von BLL- und DAL-Ebene Ausnahmen](handling-bll-and-dal-level-exceptions-vb.md) Lernprogramm erstellten eine Seite, die die Namen und Preise der Produkte in zwei Spalten, bearbeitbare DataList aufgeführt. Unser Ziel für dieses Lernprogramm ist um DataList Bearbeitungsoberfläche s Einbeziehung Validierungssteuerelemente zu erweitern. Insbesondere wird Überprüfungslogik:

- Erfordert, dass der Name des Produkts s bereitgestellt werden
- Stellen Sie sicher, dass der eingegebene Wert für den Preis ein Währungsformat gültig ist
- Stellen Sie sicher, dass den eingegebene Wert für der Preis größer als oder gleich 0 (null), da eine Negative ist `UnitPrice` Wert ist ungültig

Bevor wir am erweitern im vorherigen Beispiels betrachten können um eine Validierung beinhalten, müssen Sie zuerst replizieren das Beispiel aus der `ErrorHandling.aspx` auf der Seite der `EditDeleteDataList` Ordner auf der Seite für dieses Lernprogramm `UIValidation.aspx`. Um dies zu erreichen, wir über beide kopiert müssen, die `ErrorHandling.aspx` Seite s deklarativem Markup und den Quellcode. Kopieren Sie zuerst über deklaratives Markup, anhand der folgenden Schritte:

1. Öffnen der `ErrorHandling.aspx` Seite in Visual Studio
2. Wechseln Sie zur Seite "s" deklarativen Markup (klicken Sie auf die Schaltfläche "Quelle" am unteren Rand der Seite)
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 32), wie in Abbildung 1 dargestellt.


[![Kopieren Sie den Text innerhalb der &lt;Asp: Inhalt&gt; Steuerelement](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 2**: Kopieren Sie den Text innerhalb der `<asp:Content>` Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Öffnen der `UIValidation.aspx` Seite
2. Wechseln Sie zu der deklarativen s-Seitenmarkup
3. Fügen Sie den Text innerhalb der `<asp:Content>` Steuerelement.

Um den Quellcode kopiert haben, öffnen Sie die `ErrorHandling.aspx.vb` Seite, und kopieren Sie nur den Text *in* der `EditDeleteDataList_ErrorHandling` Klasse. Kopieren Sie die drei Ereignishandler (`Products_EditCommand`, `Products_CancelCommand`, und `Products_UpdateCommand`) zusammen mit den `DisplayExceptionDetails` -Methode, aber führen **nicht** kopieren Sie die Klassendeklaration oder `using` Anweisungen. Fügen Sie den kopierten Text *in* der `EditDeleteDataList_UIValidation` in Klasse `UIValidation.aspx.vb`.

Nach dem verschieben, die über den Inhalt und den Code von `ErrorHandling.aspx` zu `UIValidation.aspx`, können Sie die Seiten in einem Browser zu testen. Dieselbe Ausgabe angezeigt, und die gleiche Funktionalität in diesen beiden Seiten (siehe Abbildung 2) auftreten.


[![Die Seite "UIValidation.aspx" imitiert die Funktionalität in ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: die `UIValidation.aspx` Seite imitiert die Funktionalität in `ErrorHandling.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Schritt 2: Hinzufügen der Validierungssteuerelemente zu DataList s ItemTemplate

Beim Dateneingabeformulare erstellen zu können, ist es wichtig, dass Benutzer über alle erforderlichen Felder eingeben und, dass alle diesbezüglichen Eingaben bereitgestellten zulässigen Werte ordnungsgemäß formatiert sind. Um sicherzustellen, dass eine s-Benutzereingaben gültig sind, bietet ASP.NET fünf integrierte Validierungssteuerelemente, die zur Überprüfung des Werts eines einzelnen Web-Steuerelements entwickelt wurden:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) wird sichergestellt, dass ein Wert bereitgestellt wurde
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) überprüft einen Wert auf eine andere Web Control-Wert oder einen konstanten Wert oder wird sichergestellt, dass das Wertformat s für eine angegebene Datentyp gültig ist
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) wird sichergestellt, dass ein Wert innerhalb eines Bereichs von Werten ist.
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) überprüft einen Wert für eine [reguläre Ausdrücke](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) überprüft einen Wert für eine benutzerdefinierte, vom Benutzer definierten-Methode

Weitere Informationen zu diesen fünf Steuerelemente verweisen zurück auf den [Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) Lernprogramm oder Auschecken der [Validierungssteuerelemente Abschnitt](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) von der [ASP.NET Schnellstart-Lernprogrammen](https://quickstarts.asp.net).

Für unser Tutorial müssen wir verwenden eine RequiredFieldValidator, um sicherzustellen, dass ein Wert für den Namen des Produkts angegeben wurde und eine CompareValidator, um sicherzustellen, dass der eingegebene Preis einen Wert größer als oder gleich 0 hat und werden in einem gültigen Währungsformat dargestellt.

> [!NOTE]
> Während ASP.NET 1.x hatte diese gleichen fünf Validierungssteuerelemente, ASP.NET 2.0 verfügt über eine Reihe von Verbesserungen hinzugefügt, den Hauptknoten zwei Skriptdatei auf Clientseite wird Unterstützung für Browser neben dem Internet Explorer und die Möglichkeit, die Validierungssteuerelemente für Partition auf einer Seite in Überprüfung Gruppen. Weitere Informationen zu den neuen Funktionen der Überprüfung-Steuerelement in 2.0 finden Sie unter [unterzogen, wodurch der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Lassen Sie s starten, indem Sie die erforderlichen Validierungssteuerelemente DataList s hinzufügen `EditItemTemplate`. Diese Aufgabe kann mithilfe des Designers durch Klicken auf die Verknüpfung Vorlagen bearbeiten das DataList-s-Smarttag oder durch die deklarative Syntax erfolgen. Lassen Sie s durchlaufen Sie den Prozess über den Vorlagen bearbeiten-Option in der Entwurfsansicht. Nach dem auswählen die DataList s bearbeiten `EditItemTemplate`, fügen Sie einen RequiredFieldValidator hinzu, indem Sie ziehen es aus der Toolbox in die Vorlage bearbeiten-Oberfläche, legen Sie das Element nach der `ProductName` Textfeld.


[![Fügen Sie einen RequiredFieldValidator ItemTemplate nach ProductName TextBox](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: hinzufügen einen RequiredFieldValidator auf die `EditItemTemplate After` der `ProductName` Textfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Alle Validierungssteuerelemente durch Überprüfen der Eingabe eines einzelnen ASP.NET Web-Steuerelements arbeiten. Aus diesem Grund müssen wir darauf hinweisen, dass wir gerade hinzugefügten RequiredFieldValidator gegen überprüfen soll die `ProductName` Textfeld; dies geschieht, indem das Validierungssteuerelement s [ `ControlToValidate` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) auf die `ID` von das entsprechende Websteuerelement (`ProductName`, in dieser Instanz). Legen Sie anschließend die [ `ErrorMessage` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) , müssen Sie den Namen des Produkts s bereitstellen und die [ `Text` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) auf \*. Die `Text` Eigenschaftswert, falls vorhanden, wird der Text, der durch das Validierungssteuerelement angezeigt wird, wenn die Validierung fehlschlägt. Die `ErrorMessage` Eigenschaftswert, der erforderlich ist, wird vom Steuerelement ValidationSummary; verwendet, wenn die `Text` Eigenschaftswert weggelassen wird, die `ErrorMessage` Eigenschaftswert wird angezeigt, durch das Validierungssteuerelement für die Eingabe ist ungültig.

Nach dem Festlegen dieser drei Eigenschaften des RequiredFieldValidator, sollte der Bildschirm in Abbildung 4 ähneln.


[![Legen Sie die RequiredFieldValidator s ControlToValidate, ErrorMessage und Eigenschaften von Text](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: Legen Sie die s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, und `Text` Eigenschaften ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


Mit dem RequiredFieldValidator hinzugefügt, um die `EditItemTemplate`, dass alle, bleibt die erforderliche Validierung für die Ausgaben des s Produktpreis Textfeld hinzugefügt wird. Da die `UnitPrice` ist optional bei der Bearbeitung eines Datensatzes Verschlüsselungskennwort wir t muss einen RequiredFieldValidator hinzugefügt. Wir tun, allerdings müssen eine CompareValidator, um sicherzustellen, dass die `UnitPrice`, sofern angegeben, wird als Währung ordnungsgemäß formatiert und ist größer als oder gleich 0.

Hinzufügen CompareValidator in der `EditItemTemplate` und legen Sie seine `ControlToValidate` Eigenschaft `UnitPrice`, dessen `ErrorMessage` Eigenschaft, um den Preis muss größer als oder gleich null sein und darf keine das Währungssymbol und dessen `Text` Eigenschaft \*. Gibt an, dass die `UnitPrice` Wert muss größer als oder gleich 0, legen Sie die s CompareValidator [ `Operator` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) auf `GreaterThanEqual`, dessen [ `ValueToCompare` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) auf 0 (null) und die [ `Type` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) auf `Currency`.

Nach dem Hinzufügen dieser beiden Validierungssteuerelemente DataList s `EditItemTemplate` s deklarative Syntax sollte etwa wie folgt aussehen:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Öffnen Sie nachdem diese Änderungen vorgenommen wurden die Seite in einem Browser aus. Wenn Sie versuchen, lassen Sie den Namen, oder geben einen ungültigen preiswert, wenn ein Produkt zu bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 5 gezeigt, wird ein preiswert, der das Währungssymbol ein, z. B. $19.95 enthält als ungültig betrachtet. Die CompareValidator s `Currency` `Type` für die zifferngruppierung (z. B. Kommas oder Punkte, je nach den kultureinstellungen) und einem vorangestellten Plus- oder Minuszeichen (-) können im Gegensatz zu *nicht* ein Währungssymbol zulassen. Dieses Verhalten kann Benutzer perplex, die Bearbeitungsoberfläche einzugehen sind derzeit die `UnitPrice` das Währungsformat.


[![Ein Sternchen wird neben die Textfelder ein, mit der ungültigen Eingabe angezeigt.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: ein Sternchen wird weiter ', um die Textfelder ein, mit der ungültigen Eingabe ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Während der Überprüfung erfolgt wie-ist, muss der Benutzer das Währungssymbol manuell zu entfernen, wenn einen Datensatz bearbeiten, was nicht zulässig ist. Darüber hinaus treten ungültige Schnittstelle Eingaben in die Bearbeitung weder die Aktualisierung noch "Abbrechen" Schaltflächen geklickt haben, wird einen Postback aufgerufen. Im Idealfall würde die Schaltfläche "Abbrechen" das DataList Datenbankzustands vorab Bearbeitung unabhängig von der Gültigkeit der s Benutzereingaben zurückgeben. Darüber hinaus muss sichergestellt werden, dass die Daten der Seite s gültig ist, vor dem Aktualisieren der Produktinformationen in den DataList s `UpdateCommand` Ereignishandler, die Validierungssteuerelemente, die die clientseitige Logik werden, von Benutzern umgangen kann, deren Browser JavaScript unterstützt Verschlüsselungskennwort oder haben, die Unterstützung deaktiviert ist.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Entfernen das Währungssymbol aus dem EditItemTemplate s UnitPrice-Textfeld

Bei Verwendung der CompareValidator s `Currency``Type`, die Eingabe, die überprüft wird, darf keiner Währungssymbole enthalten. Das Vorhandensein solcher Symbole bewirkt, dass die CompareValidator, um die Eingabe als ungültig zu markieren. Unsere Bearbeitung Schnittstelle enthält jedoch derzeit ein Währungssymbol in die `UnitPrice` TextBox, d. h. der Benutzer muss das Währungssymbol explizit entfernen, bevor Sie ihre Änderungen speichern. Zur Behebung des Problems, haben wir drei Optionen:

1. Konfigurieren der `EditItemTemplate` , damit die `UnitPrice` TextBox-Wert ist nicht als Währung formatiert.
2. Ermöglicht dem Benutzer ein Währungssymbol eingeben, indem CompareValidator entfernen und ersetzen es mit einem RegularExpressionValidator, die prüft, ob eine ordnungsgemäß formatierte Währung-Wert. Das Herausforderung ist, dass der reguläre Ausdruck, der einen Währungswert überprüfen nicht so einfach wie das CompareValidator und müsste Schreiben von Code aus, wenn die kultureinstellungen integriert werden sollten.
3. Das Validierungssteuerelement vollständig zu entfernen und basieren auf benutzerdefinierte serverseitige Validierungslogik in den GridView s `RowUpdating` -Ereignishandler.

Lassen Sie s für Option 1 für dieses Lernprogramm herunterzuladen. Derzeit die `UnitPrice` formatiert als Währungswert aufgrund der Datenbindungsausdruck für das Textfeld in der `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Ändern der `Eval` Anweisung `Eval("UnitPrice", "{0:n2}")`, formatiert das Ergebnis als Zahl mit zwei Ziffern Genauigkeit. Dies ist möglich, direkt über die deklarative Syntax oder durch Klicken auf den Link "DataBindings bearbeiten" aus der `UnitPrice` Textfeld in der DataList s `EditItemTemplate`.

Durch diese Änderung der formatierte Preis in die Bearbeitungsoberfläche Kommas als Gruppentrennzeichen für die und einen Punkt als dezimales Trennzeichen enthält, lässt jedoch deaktiviert das Währungssymbol.

> [!NOTE]
> Wenn Sie das Währungsformat aus der bearbeitbaren Schnittstelle zu entfernen, ist es hilfreich, das Währungssymbol als Text außerhalb der Textfeld eingefügt werden soll. Dies dient als Hinweis für den Benutzer, den sie nicht benötigen, um das Währungssymbol bereitzustellen.


## <a name="fixing-the-cancel-button"></a>Korrigieren die Schaltfläche "Abbrechen"

Standardmäßig ausgeben, die Validierungssteuerelemente für das Web JavaScript zum Durchführen von Validierungen auf der Clientseite. Wenn auf eine Schaltfläche, LinkButton oder ImageButton geklickt wird, werden die Validierungssteuerelemente auf der Seite auf der Clientseite überprüft, bevor das Postback auftritt. Wenn ungültige Daten vorhanden sind, wird das Postback abgebrochen. Für bestimmte Schaltflächen kann jedoch die Gültigkeit der Daten unwesentlich sein; In diesem Fall ist das Postback abgebrochen aufgrund von ungültigen Daten ein Sicherheitsrisiko.

Die Schaltfläche "Abbrechen" ist ein Beispiel. Angenommen Sie, ein Benutzer ungültige Daten, z. B. den Produktnamen s auslassen eingibt und dann, She ist nicht entscheidet für das Produkt zu speichern, nachdem alle t möchten und die Schaltfläche "Abbrechen trifft". Derzeit löst die Schaltfläche "Abbrechen", die Validierungssteuerelemente auf der Seite der gemeldet, dass der Name des Produkts ist nicht vorhanden und zu verhindern, dass das Postback. Geben Sie Text in unsere Benutzer muss die `ProductName` Textfeld nur aus der Bearbeitung abbrechen.

Die Schaltfläche, LinkButton und ImageButton Glücklicherweise haben eine [ `CausesValidation` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) anzugeben, dass können, und zwar unabhängig davon, ob Sie auf die Schaltfläche "" sollte die Validierungslogik initiieren (standardmäßig `True`). Legen Sie die Schaltfläche "Abbrechen" s `CausesValidation` Eigenschaft `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Sicherstellen der Eingaben in die UpdateCommand-Ereignishandler gültig sind

Aufgrund der clientseitiges Skript ausgegeben, die für die Validierungssteuerelemente Benutzer ungültige Eingabe ein, die Validierungssteuerelemente brechen Sie alle Postbacks durch Schaltfläche LinkButton initiiert, oder ImageButton steuert, dessen `CausesValidation` Eigenschaften sind `True` (die (Standard). Wenn ein Benutzer mit einem veralteten Browser oder eine besucht, deren JavaScript-Unterstützung deaktiviert wurde, werden die clientseitige Validierung überprüft jedoch nicht ausgeführt.

Alle der Validierungssteuerelemente ASP.NET wiederholen Sie ihre Validierungslogik nach Postback und melden die gesamte Gültigkeit der Seite "s" Eingaben über die [ `Page.IsValid` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Die Seitenfluss besteht jedoch nicht unterbrochen oder beendet wird, in einer Weise basierend auf den Wert des `Page.IsValid`. Als Entwickler ist dafür verantwortlich, um sicherzustellen, dass die `Page.IsValid` Eigenschaft hat den Wert des `True` , bevor Sie den Vorgang fortsetzen, durch Code, die gültige annimmt, um Daten einzugeben.

Wenn ein Benutzer JavaScript deaktiviert hat, besuchen Sie unsere-Seite, bearbeitet ein Produkt, geht in einen Preis zu viel Leistung beanspruchen, und klickt auf die Schaltfläche "Aktualisieren" wird die clientseitige Validierung umgangen werden, und stellen Sie ein Postback sicher ist. Beim Postback, der ASP-Seite "s" `UpdateCommand` -Ereignishandler ausgeführt wird und eine Ausnahme wird ausgelöst, wenn bei dem Versuch, zu analysieren aufwendig zu einem `Decimal`. Da wir verfügen über die Behandlung von Ausnahmen, derartige Ausnahme unauffällig behandelt werden, aber wir, dass die ungültigen Daten verhindern könnte über ursprünglich von es wird nur mit synchronen der `UpdateCommand` Ereignishandler Wenn `Page.IsValid` hat den Wert `True`.

Fügen Sie den folgenden Code am Anfang der `UpdateCommand` Ereignishandler, d. h. unmittelbar vor der `Try` blockieren:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Durch diese hinzufügen versucht das Produkt aktualisiert werden, nur, wenn die übermittelten Daten gültig ist. Die meisten Benutzer/t gewonnen werden ungültige Postbackdaten aufgrund der Validierung Steuerelemente clientseitige Skripts können Benutzer, deren Browser Verschlüsselungskennwort JavaScript unterstützt oder JavaScript unterstützt haben, deaktiviert, können jedoch die clientseitige Überprüfung umgehen und übermitteln Sie ungültige Daten.

> [!NOTE]
> Der aufmerksame Leser wird Bedenken Sie, dass beim Aktualisieren von Daten mit GridView wir explizit überprüfen müssen die `Page.IsValid` Eigenschaft in der Seite "s" Code-Behind-Klasse. Dies ist, da die GridView fragt die `Page.IsValid` -Eigenschaft für uns und nur fährt mit dem Update nur, wenn der Wert zurückgegeben `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Schritt 3: Zusammenfassung der Probleme mit der Dateneingabe

Zusätzlich zu den fünf Validierungssteuerelemente ASP.NET umfasst die [ValidationSummary Steuerelement](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), welche zeigt die `ErrorMessage` s die Validierungssteuerelemente, die ungültige Daten erkannt. Diese zusammengefassten Daten können als Text auf der Webseite oder über ein modales, clientseitige Messagebox angezeigt werden. Lassen Sie s Lernprogramm können Sie eine clientseitige Messagebox Zusammenfassung der Überprüfungsprobleme gehören zu verbessern.

Um dies zu erreichen, müssen ziehen Sie ein ValidationSummary-Steuerelement aus der Toolbox in den Designer. Der Speicherort des t ValidationSummary-Steuerelement verfügt über eine Rolle spielen, seit es re So zeigen Sie die Zusammenfassung nur als eine MessageBox-Datenbank konfigurieren möchten. Legen Sie nach dem Hinzufügen des Steuerelements an, dessen [ `ShowSummary` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) auf `False` und seine [ `ShowMessageBox` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) auf `True`. Mit dieser Zusatz Validierungsfehler in einer clientseitigen Messagebox zusammengefasst werden (siehe Abbildung 6).


[![Validierungsfehler werden in einer clientseitigen Messagebox zusammengefasst.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: die Validierungsfehler werden in eine clientseitige Messagebox zusammengefasst ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben wir gesehen, wie reduziert die Wahrscheinlichkeit von Ausnahmen mithilfe der Validierungssteuerelemente proaktiv sicherstellen, dass unsere Benutzer Eingaben gültig sind, bevor Sie versuchen, Ihre Verwendung in den Workflow für die Aktualisierung. ASP.NET verfügt über fünf Web Validierungssteuerelemente, die darauf ausgelegt sind, überprüfen Sie die eine bestimmte Web steuern s Eingabe, und melden die Gültigkeit der Eingabe s zurück. In diesem Lernprogramm verwendet es zwei dieser fünf Steuerelemente RequiredFieldValidator und CompareValidator, stellen Sie sicher, dass der Name des Produkts s angegeben wurde und dass der Preis ein Währungsformat mit einem Wert größer als oder gleich 0 (null) hat.

Hinzufügen von Validierungssteuerelementen mit dem Bearbeiten der Schnittstelle DataList-s ist so einfach wie Drag & Drop auf die `EditItemTemplate` aus der Toolbox, und eine Handvoll Eigenschaften festlegen. Standardmäßig ausgeben der Validierungssteuerelemente automatisch die clientseitige Validierung Skripts; Sie bieten auch eine serverseitige Validierung beim Postback, speichern das kumulierte Ergebnis in der `Page.IsValid` Eigenschaft. Um die clientseitige Überprüfung umgehen, wenn auf eine Schaltfläche, LinkButton oder ImageButton geklickt wird, legen Sie die Schaltfläche "s" `CausesValidation` Eigenschaft `False`. Vor dem Ausführen von Aufgaben mit den Daten, die beim Postback übermittelt, darüber hinaus sicher, dass die `Page.IsValid` -Eigenschaft gibt `True`.

Alle DataList Lernprogramme bearbeiten wir hatten bisher untersucht Ve sehr einfachen bearbeiten Schnittstellen, ein Textfeld für den Produktnamen s und eine für den Preis. Die Bearbeitung Schnittstelle darf jedoch eine Mischung aus anderen Web-Steuerelemente, z. B. DropDownLists, verwendete Kalender, Optionsfelder, Kontrollkästchen und So weiter. In unserem nächsten Lernprogramm betrachten wir erstellen eine Schnittstelle, die eine Vielzahl von Websteuerelementen verwendet.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Dennis Patterson Ken Pespisa und Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](handling-bll-and-dal-level-exceptions-vb.md)
> [Weiter](customizing-the-datalist-s-editing-interface-vb.md)
