---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Behandeln von Ausnahmen auf BLL- und DAL-Ebene, auf einer ASP.NET-Seite (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie mit eine benutzerfreundlichen, aussagekräftigen Fehlermeldung anzeigen Auftreten einer Ausnahme während einer Insert, Update oder Delete-Vorgang, der...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c971b69605055e587aefe92fb4fc755176e6b53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393111"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Behandeln von Ausnahmen auf BLL- und DAL-Ebene, auf einer ASP.NET-Seite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) oder [PDF-Datei herunterladen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> In diesem Tutorial wird gezeigt, wie mit eine benutzerfreundlichen, aussagekräftigen Fehlermeldung anzeigen Auftreten einer Ausnahme während einer INSERT-, Update-, oder Delete-Vorgang, der ASP.NET-Daten in einem Web-Steuerelement.


## <a name="introduction"></a>Einführung

Arbeiten mit Daten aus einer ASP.NET-Webanwendung mithilfe einer Anwendung mit mehreren Ebenen-Architektur umfasst die folgenden drei allgemeine Schritte:

1. Bestimmen Sie, welche Methode für die Geschäftslogikschicht muss aufgerufen werden und welche Parameterwerte, um es zu übergeben. Die Werte der Parameter können hart codierten, programmgesteuert zugewiesen oder Eingaben vom Benutzer eingegeben werden.
2. Rufen Sie die Methode auf.
3. Die Ergebnisse zu verarbeiten. Beim Aufrufen einer BLL-Methode, die Daten zurückgibt, kann dies das Binden der Daten in einen Daten-Websteuerelement umfassen. Für die BLL-Methoden, die Daten ändern, kann dies umfassen, Aktionen basierend auf einen Rückgabewert oder ordnungsgemäß behandeln jede Ausnahme, die in Schritt2 entstanden ist.

Wie wir, in gesehen der [vorherigen Tutorial](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), stellen Sie Erweiterbarkeitspunkte sowohl dem ObjectDataSource-Steuerelement auch die Web-Steuerelemente bereit, für die Schritte 1 und 3. Beispielsweise löst der GridView seine `RowUpdating` Ereignis vor dem Zuweisen von die Feldwerte, die "ObjectDataSource" `UpdateParameters` Auflistung, dessen `RowUpdated` Ereignis wird ausgelöst, nachdem das "ObjectDataSource" der Vorgang abgeschlossen ist.

Wir haben bereits die Ereignisse untersucht, die in Schritt 1 ausgelöst werden und haben gesehen, wie sie zum Anpassen von die Eingabeparameter, oder brechen Sie den Vorgang verwendet werden können. In diesem Tutorial werden wir unsere Aufmerksamkeit auf die Ereignisse aktivieren, die ausgelöst werden, nachdem der Vorgang abgeschlossen ist. Bei diesen auf beitragsebene Ereignishandlern, die wir, die unter anderem können, zu bestimmen Sie, ob eine Ausnahme während des Vorgangs aufgetreten, und behandeln Sie diese ordnungsgemäß, als das ASP.NET-STANDARDATTRIBUT statt eine benutzerfreundlichen, aussagekräftigen Fehlermeldung auf dem Bildschirm angezeigt Seite mit Ausnahmen für.

Erstellen Sie zum Arbeiten mit diesen Ereignissen auf beitragsebene zu veranschaulichen, lassen Sie uns eine Seite, die die Produkte in einem bearbeitbaren GridView-Ansicht aufgelistet werden. Wenn ein Produkt aktualisiert, wenn eine Ausnahme ist unser ASP.NET ausgelöst Seite zeigt eine kurze Nachricht über die GridView, dass ein Problem aufgetreten ist. Fangen wir an!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Schritt 1: Erstellen einer bearbeitbaren GridView-Produkte

Im vorherigen Tutorial erstellt es eine bearbeitbare GridView mit nur zwei Felder, `ProductName` und `UnitPrice`. Dieser erforderliche erstellen zusätzlichen Verarbeitungsaufwand für die `ProductsBLL` Klasse `UpdateProduct` -Methode, eine, die nur drei Parameter (des Produkts Name, Preis pro Einheit und ID) wie entgegengesetzter einen Parameter für jedes Feld "Product" akzeptiert. Üben Sie in diesem Tutorial lassen Sie uns dieses Verfahren in diesem Fall erstellen eine bearbeitbare GridView, die zeigt den Namen des Produkts, Menge pro Einheit, Preis pro Einheit und Einheiten auf Lager, jedoch kann nur der Name, den Preis pro Einheit und die Einheiten auf Lager bearbeitet werden.

Für dieses Szenario benötigen wir eine andere Überladung der `UpdateProduct` -Methode, eine, die akzeptiert vier Parameter: den Namen des Produkts, Preis, Einheiten im Lager und -ID. Fügen Sie die folgende Methode der `ProductsBLL` Klasse:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Mit dieser Methode abgeschlossen ist sind wir bereit, die ASP.NET-Seite zu erstellen, die es ermöglicht die Bearbeitung dieser vier Felder für bestimmtes Produkt. Öffnen der `ErrorHandling.aspx` auf der Seite die `EditInsertDelete` Ordner und Hinzufügen einer GridView-Ansicht auf der Seite über den Designer. GridView zu binden, um eine neue "ObjectDataSource", Zuordnung der `Select()` Methode, um die `ProductsBLL` Klasse `GetProducts()` Methode und die `Update()` Methode, um die `UpdateProduct` Überladung, die gerade erstellt haben.


[![Verwenden Sie die Überladung von UpdateProduct-Methode, die vier Eingabeparameter akzeptiert.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Abbildung 1**: Verwenden der `UpdateProduct` Methode überladen, akzeptiert vier Eingabeparameter ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Dadurch entsteht ein ObjectDataSource-Steuerelement mit einem `UpdateParameters` Sammlung mit vier Parametern und einer GridView-Ansicht mit einem Feld für jedes der Felder Product. Weist dem ObjectDataSource-Steuerelement deklarative Markup der `OldValuesParameterFormatString` Eigenschaft ist der Wert `original_{0}`, das wird eine Ausnahme ausgelöst, da unsere BLL-Klasse einen Eingabeparameter namens erwarten nicht `original_productID` übergeben werden. Vergessen Sie nicht, entfernen Sie diese Einstellung vollständig aus der deklarativen Syntax (oder legen ihn auf den Standardwert `{0}`).

Kürzen Sie als Nächstes die GridView, die nur die `ProductName`, `QuantityPerUnit`, `UnitPrice`, und `UnitsInStock` BoundFields. Darüber hinaus können Sie alle Feldebene Formatierung, die Sie für erforderlich halten, anwenden (z. B. das Ändern der `HeaderText` Eigenschaften).

Im vorherigen Tutorial erläutert, wie Sie das format der `UnitPrice` BoundField als Währung in nur-Lese Modus und den Bearbeitungsmodus. Führen wir hier ein. Denken Sie daran, dass dies erforderlich, Festlegen der BoundField des `DataFormatString` Eigenschaft `{0:c}`, dessen `HtmlEncode` Eigenschaft, um `false`, und die zugehörige `ApplyFormatInEditMode` zu `true`, wie in Abbildung 2 dargestellt.


[![Konfigurieren Sie die UnitPrice-BoundField-Anzeige als Währung](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Abbildung 2**: Konfigurieren der `UnitPrice` BoundField Anzeige als Währung ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formatieren der `UnitPrice` wie eine Währung, in die Bearbeitungsschnittstelle erfordert, erstellen einen Ereignishandler für der GridView `RowUpdating` -Ereignis, das analysiert die Currency-formatierte Zeichenfolge in eine `decimal` Wert. Bedenken Sie, dass die `RowUpdating` -Ereignishandler aus dem letzten Tutorial auch überprüft werden, um sicherzustellen, dass den angegebene Benutzer ein `UnitPrice` Wert. Allerdings in diesem Tutorial lassen wir für den Benutzer, um den Preis zu unterdrücken.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Unsere GridView umfasst eine `QuantityPerUnit` BoundField-, aber diese BoundField-sollte nur zu Anzeigezwecken sein und sollte nicht vom Benutzer bearbeitet werden. Um dies zu sortieren, legen Sie einfach die BoundFields' `ReadOnly` Eigenschaft `true`.


[![Aktivieren des Schreibschutzes für die QuantityPerUnit BoundField-](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Abbildung 3**: Stellen die `QuantityPerUnit` BoundField Read-Only ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Zum Schluss das Kontrollkästchen Sie bearbeiten Aktivieren von GridView Smarttag. Nach Abschluss dieser Schritte den `ErrorHandling.aspx` -Designer die Seite sollte ähnlich wie in Abbildung 4 aussehen.


[![Entfernen Sie alle bis auf die erforderliche BoundFields und Kontrollkästchen bearbeiten das Kontrollkästchen aktivieren](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Abbildung 4**: Entfernen Sie alle außer der erforderlichen BoundFields und aktivieren Sie das Kontrollkästchen aktivieren bearbeiten ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


An diesem Punkt haben wir eine Liste aller Produkte `ProductName`, `QuantityPerUnit`, `UnitPrice`, und `UnitsInStock` Felder; allerdings nur die `ProductName`, `UnitPrice`, und `UnitsInStock` Felder bearbeitet werden können.


[![Benutzer können jetzt ganz einfach Produkte Namen, Preise und Einheiten In vordefinierte Felder bearbeiten](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Abbildung 5**: Benutzer können jetzt ganz einfach bearbeiten Produkte Namen, Preise und Einheiten im Lager Felder ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Schritt 2: Ordnungsgemäße Ausnahmebehandlung auf DAL-Ebene

Unsere bearbeitbaren GridView einwandfrei funktioniert, zwar beim Benutzer gültige Werte für die bearbeitete Product Name, Preis und im Lager vorrätigen Stückzahl eingeben löst eine Ausnahme unzulässige Werte eingeben. Z. B. das Auslassen der `ProductName` Wert bewirkt, dass eine [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) seit ausgelöst werden die `ProductName` -Eigenschaft in der `ProdcutsRow` -Klasse verfügt über seine `AllowDBNull` -Eigenschaft auf festgelegt `false`; Wenn die Datenbank betriebsbereit ist, eine `SqlException` beim Versuch, eine Verbindung mit der Datenbank von der TableAdapter ausgelöst. Ohne dass eine Aktion an, diese Ausnahmen übergeben aus der Datenzugriffsebene, die Geschäftslogikebene und anschließend auf der ASP.NET-Seite und schließlich an die ASP.NET-Laufzeitumgebung.

Je nachdem, wie Ihre Web-Anwendung konfiguriert wird und davon, ob die Anwendung von besuchte `localhost`, eine nicht behandelte Ausnahme kann dazu führen, entweder eine generische Fehlerseite des Servers, einer detaillierten Fehlerbericht oder eine benutzerfreundliche Webseite. Finden Sie unter [Web Application Error Handling in ASP.NET](http://www.15seconds.com/issue/030102.htm) und [CustomErrors-Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) um mehr über die ASP.NET-Laufzeit wie auf eine nicht abgefangene Ausnahme zu reagieren.

Abbildung 6 zeigt den Bildschirm, der aufgetreten beim Versuch, ein Produkt zu aktualisieren, ohne die `ProductName` Wert. Dies ist die Standardeinstellung, die ausführliche Fehlermeldung-Bericht angezeigt, wenn der eingehenden `localhost`.


[![Das Auslassen des Produkts Name wird Anzeige Ausnahmedetails](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Abbildung 6**: das Auslassen des Produkts Name wird Anzeige Ausnahmedetails ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Auch die Details für eine solche Ausnahme beim Testen einer Anwendung hilfreich sind, ist es bietet ein Endbenutzer mit solcher Bildschirm bei einer Ausnahme nicht ideal. Ein Endbenutzer, die wahrscheinlich nicht wissen, was eine `NoNullAllowedException` ist oder warum es verursacht wurde. Ein besserer Ansatz ist, stehen dem Benutzer eine benutzerfreundliche Meldung angezeigt, dass beim Versuch, das Produkt zu aktualisieren sind Probleme aufgetreten.

Wenn eine Ausnahme tritt auf, wenn es sich bei den Vorgang ausführt, bereitgestellt werden die Ereignisse auf beitragsebene, in dem ObjectDataSource-Steuerelement und das Web-Steuerelement erkannt und die Ausnahme von bis zu der ASP.NET-Laufzeit bubbling Abbrechen. In unserem Beispiel erstellen Sie lassen Sie uns einen Ereignishandler für der GridView `RowUpdated` -Ereignis, das bestimmt, ob eine Ausnahme ausgelöst hat, und, wenn dies der Fall ist, werden die Details der Ausnahme in ein Label-Steuerelement angezeigt.

Starten, indem Sie eine Bezeichnung hinzufügen, in die ASP.NET-Seite, Festlegen der `ID` Eigenschaft `ExceptionDetails` und Beseitigen der `Text` Eigenschaft. Um Eye für den Benutzer auf diese Nachricht zu zeichnen, legen die `CssClass` Eigenschaft `Warning`, dies ist eine CSS-Klasse, die wir hinzugefügt, die `Styles.css` Datei im vorherigen Tutorial. Denken Sie daran, dass diese CSS-Klasse bewirkt, dass der Bezeichnungstext, der in Rot, kursiv, fett, sehr große Schriftart angezeigt werden.


[![Fügen Sie ein Label-Steuerelement auf der Seite hinzu.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Abbildung 7**: Fügen Sie ein Label-Steuerelement auf der Seite ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Eine Ausnahme ist aufgetreten, da diese Bezeichnung Websteuerelement nur unmittelbar nach dem sichtbar werden soll, legen Sie dessen `Visible` Eigenschaft auf "false", in der `Page_Load` -Ereignishandler:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Mit diesem Code wird auf der Seite zum ersten Mal besuchen und die nachfolgenden Postbacks der `ExceptionDetails` Steuerelement hat seine `Visible` -Eigenschaftensatz auf `false`. Bei einer DAL oder BLL auf - Ausnahme aus, die erkennen, in des GridView `RowUpdated` -Ereignishandler legen wir die `ExceptionDetails` des Steuerelements `Visible` Eigenschaft auf "true". Da Ereignishandler für Web-Steuerelements nach dem Auftreten der `Page_Load` -Ereignishandler im Lebenszyklus Seite die Bezeichnung wird angezeigt. Auf das nächste Postback, jedoch die `Page_Load` -Ereignishandler wird zurückgesetzt. die `Visible` -Eigenschaft zurück auf `false`, erneut aus der Ansicht ausgeblendet.

> [!NOTE]
> Alternativ können wir die Notwendigkeit für die Einstellung Entfernen der `ExceptionDetails` des Steuerelements `Visible` -Eigenschaft in `Page_Load` durch Zuweisen der `Visible` Eigenschaft `false` in der deklarativen Syntax und seinen Ansichtszustand (Festlegen der deaktivieren`EnableViewState` Eigenschaft `false`). In einem späteren Tutorial verwenden wir diese alternative Methode.


Mit dem Label-Steuerelement hinzugefügt, wird im nächsten Schritt zum Erstellen von des ereignishandlers für der GridView `RowUpdated` Ereignis. Wählen Sie im Designer die GridView, wechseln Sie zu dem Fenster "Eigenschaften", und klicken Sie auf das Blitzsymbol, GridView Ereignisse auflisten. Sollte bereits ein Eintrag vorhanden sein gibt es für des GridView `RowUpdating` -Ereignis, wie wir einen Ereignishandler für dieses Ereignis weiter oben in diesem Tutorial erstellt haben. Erstellen Sie einen Ereignishandler für die `RowUpdated` Ereignis sowie.


![Erstellen Sie einen Ereignishandler für der GridView RowUpdated-Ereignis](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Abbildung 8**: Erstellen Sie einen Ereignishandler für der GridView `RowUpdated` Ereignis


> [!NOTE]
> Sie können auch den Ereignishandler über die Dropdownlisten am Anfang der CodeBehind-Klassendatei erstellen. Wählen Sie aus der Dropdown-Liste auf der linken Seite die GridView und die `RowUpdated` Ereignis von der auf der rechten Seite.


Erstellen diesen Ereignishandler wird den folgenden Code die ASP.NET-Seite Code-Behind-Klasse hinzugefügt:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Dieser Ereignishandler der zweite Eingabeparameter ist ein Objekt des Typs [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), die über drei Eigenschaften von Interesse sind, für die Behandlung von Ausnahmen verfügt:

- `Exception` Ein Verweis auf die ausgelöste Ausnahme Wenn keine Ausnahme ausgelöst wurde, wird diese Eigenschaft den Wert verfügen. `null`
- `ExceptionHandled` Ein boolescher Wert, der angibt, und zwar unabhängig davon, ob die Ausnahme behandelt wurde, in der `RowUpdated` Ereignishandler; Wenn `false` (Standardeinstellung), die Ausnahme wird erneut ausgelöst, bis zu der ASP.NET-Laufzeit durchsickert
- `KeepInEditMode` Wenn auf festgelegt `true` die bearbeitete GridView-Zeile bleibt im Bearbeitungsmodus befindet, wenn `false` (Standardeinstellung), die GridView-Zeile wird auf den nur-Lese Modus zurückgesetzt

Unser Code, dann sollten überprüfen, finden Sie unter `Exception` nicht `null`, was bedeutet, dass eine Ausnahme beim Ausführen des Vorgangs ausgelöst wurde. Wenn dies der Fall ist, möchten wir:

- Zeigt eine benutzerfreundliche Meldung in die `ExceptionDetails` Bezeichnung
- Anzugeben Sie, dass die Ausnahme behandelt wurde
- Belassen Sie die GridView-Zeile im Bearbeitungsmodus.

Der folgende Code führt diese Ziele zu erreichen:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Dieser Ereignishandler startet, indem überprüft wird, finden Sie unter `e.Exception` ist `null`. Ist dies nicht der Fall, die `ExceptionDetails` Bezeichnungsfelds `Visible` -Eigenschaftensatz auf `true` und die zugehörige `Text` Eigenschaft auf "Gab es ein Problem mit dem Aktualisieren des Produkts." Die Details der tatsächliche Ausnahme, die ausgelöst wurde, befinden sich in der `e.Exception` des Objekts `InnerException` Eigenschaft. Diese innere Ausnahme wird untersucht, und, wenn es einen bestimmten Typ handelt, wird an eine zusätzliche, hilfreiche-Nachricht angefügt der `ExceptionDetails` Bezeichnungsfelds `Text` Eigenschaft. Und schließlich die `ExceptionHandled` und `KeepInEditMode` Eigenschaften auf festlegen `true`.

Abbildung 9 zeigt einen Screenshot der Seite aus, wenn der Name des Produkts auslassen; Abbildung 10 zeigt die Ergebnisse bei der Eingabe eine unzulässige `UnitPrice` Wert (-50).


[![Die ProductName BoundField-muss einen Wert enthalten.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Abbildung 9**: die `ProductName` BoundField muss einen Wert enthalten ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Negative UnitPrice-Werte sind nicht zulässig](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Abbildung 10**: Negative `UnitPrice` Werte sind nicht zulässig ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Durch Festlegen der `e.ExceptionHandled` Eigenschaft `true`, `RowUpdated` -Ereignishandler hat festgelegt, dass es die Ausnahme behandelt hat. Aus diesem Grund wird nicht die Ausnahme, die ASP.NET Laufzeit weitergeleitet.

> [!NOTE]
> Abbildung 9 und 10 zeigen eine ordnungsgemäße Möglichkeit zum Verarbeiten von Ausnahmen, die aufgrund von ungültigen Benutzereingaben ausgelöst. Im Idealfall jedoch solche ungültige Eingabe wird nie Reichweite der Geschäftslogikschicht im vornherein, wie die ASP.NET-Seite sichergestellt wird, dass der Benutzer Eingaben gültig, vor dem Aufrufen sind der `ProductsBLL` Klasse `UpdateProduct` Methode. In unserem nächsten Tutorial wir das sehen Hinzufügen von Steuerelementen zur gültigkeitsprüfung den bearbeiten und Einfügen von Schnittstellen, um sicherzustellen, dass die Daten an die Geschäftslogikschicht übermittelt entspricht die Geschäftsregeln. Steuerelemente zur gültigkeitsprüfung nicht nur zu verhindern, dass der Aufruf der `UpdateProduct` Methode, bis die vom Benutzer bereitgestellten Daten gültig ist, aber auch eine informativere benutzerfreundlichkeit, zum Identifizieren der Probleme mit der Dateneingabe bieten.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Schritt 3: Ordnungsgemäße Behandlung von Ausnahmen auf BLL--Ebene

Beim Einfügen, möglicherweise aktualisieren oder Löschen von Daten, die Datenzugriffsebene eine Ausnahme bei ein Daten-bezogener Fehler auslösen. Die Datenbank ist möglicherweise offline, eine erforderlichen Datenbanktabellen-Spalte nicht war vielleicht einen Wert angegeben oder eine Einschränkung auf Tabellenebene wurde verletzt. Zusätzlich zu streng datenbezogene Ausnahmen können die Geschäftslogikschicht Ausnahmen Sie angeben, wann Business Regeln verletzt wurden. In der [Erstellen einer Geschäftslogikebene](../introduction/creating-a-business-logic-layer-cs.md) Tutorial, z. B. wir eine regelüberprüfung Business hinzugefügt, mit dem Original `UpdateProduct` überladen. Wenn der Benutzer ein Produkt markieren wurde, da nicht mehr unterstützt, müssen wir insbesondere, dass das Produkt nicht die einzige von zugehörigen Lieferanten bereitgestellt werden. Wenn diese Bedingung gegen die verstoßen wurde, ein `ApplicationException` ausgelöst wurde.

Für die `UpdateProduct` Überladung, die in diesem Tutorial erstellt haben, fügen Sie eine Geschäftsregel, die verhindert, dass die `UnitPrice` Feld festgelegt wird, um einen neuen Wert, der mehr als zweimal in der ursprünglichen `UnitPrice` Wert. Um dies zu erreichen, passen Sie die `UpdateProduct` überladen, sodass er diese Überprüfung führt und es wird ein `ApplicationException` wenn gegen diese Regel verstoßen wird. Die aktualisierte Methode folgt:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Durch diese Änderung bewirkt ein Preis-Update, das mehr als zweimal in der vorhandenen Preis ist ein `ApplicationException` ausgelöst wird. Ebenso wie die Ausnahme wird ausgelöst, von der DAL, die diesem BLL-ausgelöst `ApplicationException` ermittelt und in des GridView behandelt `RowUpdated` -Ereignishandler. In der Tat die `RowUpdated` Ereignishandler Code geschrieben wird, ordnungsgemäß erkennt diese Ausnahme und zeigt die `ApplicationException`des `Message` -Eigenschaftswert. Abbildung 11 zeigt einen Screenshot, wenn ein Benutzer versucht, aktualisieren Sie den Preis des Chai auf 50,00 $, d.h., dass mehr als das Doppelte der aktuellen Preis der 19,95 $.


[![Die Geschäftsregeln verweigert Preiserhöhungen, die mehr als doppelt so den Preis eines Produkts](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Abbildung 11**: die Geschäftsregel Disallow Preiserhöhungen, die mehr als doppelt so den Preis eines Produkts ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> Im Idealfall würde unsere Geschäftslogikregeln umgestaltet werden, von der `UpdateProduct` Überladungen der Methode in eine gängige Methode. Dies wird für den Leser als Übung übernommen.


## <a name="summary"></a>Zusammenfassung

Beim Einfügen, aktualisieren und Löschen von Vorgängen sowohl das Datensteuerelement für das Web als auch dem ObjectDataSource-Steuerelement beteiligt vorab und nachträglich auf Ereignisse auslösen, Stütze für den eigentlichen Vorgang. Wie wir in diesem Tutorial und dem vorherigen Beispiel, bei der Arbeit mit einer bearbeitbaren GridView GridView `RowUpdating` Ereignis wird ausgelöst, gefolgt von dem ObjectDataSource-Steuerelement `Updating` Ereignis, das an diesem Punkt der Update-Befehl, um dem ObjectDataSource-Steuerelement ausgelöst wird zugrunde liegende Objekt. Nachdem der Vorgang abgeschlossen wurde, wird dem ObjectDataSource-Steuerelement `Updated` Ereignis wird ausgelöst, gefolgt von des GridView `RowUpdated` Ereignis.

Ereignishandler für die vorab auf Ereignisse, um die Eingabeparameter anpassen oder für die Ereignisse auf beitragsebene, um zu überprüfen und reagieren auf der Vorgangsergebnisse Sie können erstellen. Auf beitragsebene-Ereignishandler werden am häufigsten verwendet, um festzustellen, ob eine Ausnahme während des Vorgangs aufgetreten. Bei einer Ausnahme können diese auf beitragsebene Ereignishandler optional selbstständig behandeln. In diesem Tutorial wurde erläutert, wie eine Ausnahme zu behandeln, indem eine benutzerfreundliche Fehlermeldung angezeigt.

Im nächsten Tutorial erfahren Sie, wie zum Verringern der Wahrscheinlichkeit von Ausnahmen, die aus Daten, die Formatierungsprobleme (wie z.B. die Eingabe einer negatives `UnitPrice`). Genauer gesagt betrachten wir das Hinzufügen von Validierungssteuerelementen zu bearbeiten und Einfügen von Schnittstellen ein.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [Weiter](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
