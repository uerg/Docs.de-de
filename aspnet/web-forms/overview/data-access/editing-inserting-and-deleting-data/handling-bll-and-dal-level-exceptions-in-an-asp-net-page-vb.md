---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Behandeln von Ausnahmen BLL und DAL-Ebene, auf einer ASP.NET-Seite (VB) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm wird gezeigt, wie mit eine benutzerfreundlichen, informative Fehlermeldung anzuzeigen, eine Ausnahme während einer INSERT-, Update- oder Delete-Vorgang der erfolgen soll..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 2269458cbc41fd3a483aaade0f07288ee805bdd1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Behandeln von Ausnahmen BLL und DAL-Ebene, auf einer ASP.NET-Seite (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) oder [PDF herunterladen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> In diesem Lernprogramm wird gezeigt, wie mit eine benutzerfreundlichen, informative Fehlermeldung angezeigt, eine Ausnahme während einer INSERT-, Update- oder Löschvorgang eines ASP.NET Daten Websteuerelement erfolgen soll.


## <a name="introduction"></a>Einführung

Arbeiten mit Daten aus einer ASP.NET Web-Anwendung mithilfe einer Anwendung mit mehreren Ebenen-Architektur umfasst die folgenden drei allgemeine Schritte aus:

1. Bestimmen Sie, welche Methode für die Business Logic Layer muss aufgerufen werden und welche Parameterwerte, um es zu übergeben. Parameterwerte können hartcodierten, programmgesteuert zugewiesen sein oder Eingaben vom Benutzer eingegeben werden.
2. Rufen Sie die Methode auf.
3. Die Ergebnisse zu verarbeiten. Beim Aufrufen einer BLL-Methode, die Daten zurückgibt, kann dies die Binden der Daten in eine Datendatei Websteuerelement umfassen. Für BLL-Methoden, die Daten ändern, kann dies enthalten einige Aktion basierend auf einen Rückgabewert oder ordnungsgemäß Behandeln von Ausnahmen, die in Schritt2 entstanden ist.

Wie wir gesehen, in haben der [vorherigen Lernprogramm](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), das ObjectDataSource und die Daten Websteuerelemente bieten Erweiterungspunkte für die Schritte 1 und 3. Beispielsweise löst die GridView seine `RowUpdating` Ereignis vor dessen Feldwerte zuweisen, dessen ObjectDataSource `UpdateParameters` Auflistung; die `RowUpdated` Ereignis wird ausgelöst, nachdem das ObjectDataSource der Vorgang abgeschlossen wurde.

Wir haben bereits die Ereignisse überprüft werden, die ausgelöst werden, während Schritt 1 und haben gesehen, wie sie verwendet werden können, um die Eingabeparameter anpassen, oder brechen Sie den Vorgang. In diesem Lernprogramm aktivieren wir unsere Aufmerksamkeit auf die Ereignisse, die ausgelöst werden, nachdem der Vorgang abgeschlossen wurde. Mit dieser nach der Ebene, die wir unter anderem können, Ereignishandler zu bestimmen Sie, ob eine Ausnahme während des Vorgangs aufgetreten ist und handhaben Sie kann es, eine benutzerfreundlichen, informative Fehlermeldung auf dem Bildschirm anzeigen, statt der standardmäßigen ASP.NET direktionales Seite "Ausnahme.

Zum Arbeiten mit diese Ereignisse nach der Ebene zu veranschaulichen, erstellen Sie eine Seite, die der Produkte in einem bearbeitbaren GridView aufgeführt sind. Wenn ein Produkt aktualisiert werden, wenn eine Ausnahme ist unsere ASP.NET ausgelöst Seite zeigt eine kurze Nachricht über die GridView erläutert wird, dass ein Problem aufgetreten ist. Fangen wir an!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Schritt 1: Erstellen einer bearbeitbaren GridView von Produkten

Im vorherigen Lernprogramm erstellt es einen bearbeitbaren GridView mit nur zwei Felder `ProductName` und `UnitPrice`. Diese Einstellung ist erforderlich erstellen eine weitere Überladung für die `ProductsBLL` Klasse `UpdateProduct` -Methode, ein, die nur drei Eingabeparameter (der Product Name, Einzelpreis und ID) wie einzurichten einen Parameter für jedes Feld "Product" akzeptiert. Üben Sie für dieses Lernprogramm nehmen wir diese Technik erneut aus, erstellen eine bearbeitbare GridView, die zeigt den Namen des Produkts, Menge pro Einheit, Einzelpreis und Einheiten im Lager, aber nur der Name, den Einzelpreis und die Einheiten im Lager bearbeitet werden kann.

Für dieses Szenario benötigen wir eine andere Überladung der `UpdateProduct` -Methode, die vier Parameter akzeptiert: den Namen des Produkts, Preis pro Einheit, Einheiten im Bestand und -ID. Fügen Sie die folgende Methode, die `ProductsBLL` Klasse:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Mit dieser Methode abgeschlossen ist können wir die ASP.NET-Seite zu erstellen, die es ermöglicht die Bearbeitung dieser vier bestimmtes Produkt Felder. Öffnen der `ErrorHandling.aspx` auf der Seite der `EditInsertDelete` Ordner und die Seite mithilfe des Designers eine GridView hinzuzufügen. GridView zu binden, um eine neue ObjectDataSource Zuordnung der `Select()` Methode, um die `ProductsBLL` Klasse `GetProducts()` Methode und die `Update()` Methode, um die `UpdateProduct` Überladung, die gerade erstellt haben.


[![Verwenden Sie die UpdateProduct Methodenüberladung, die vier Eingabeparameter akzeptiert.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Abbildung 1**: Verwenden der `UpdateProduct` -Methode zu überladen, akzeptiert vier Eingabeparameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Dadurch wird ein ObjectDataSource mit erstellt eine `UpdateParameters` Auflistung mit vier Parametern und einer GridView mit einem Feld für jedes der Felder Product. Das ObjectDataSource deklarative Markup weist die `OldValuesParameterFormatString` Eigenschaft den Wert `original_{0}`, dem wird eine Ausnahme ausgelöst, da unsere BLL Klasse einen Eingabeparameter mit dem Namen unerwarteterweise `original_productID` übergeben werden muss. Vergessen Sie nicht, entfernen Sie diese Einstellung vollständig vom deklarative Syntax (oder legen Sie sie auf den Standardwert `{0}`).

Als Nächstes die GridView nur einschließen Kürzen der `ProductName`, `QuantityPerUnit`, `UnitPrice`, und `UnitsInStock` BoundFields. Darüber hinaus können Sie alle Feldebene Formatierung aus, die Sie für erforderlich halten, anwenden (z. B. das Ändern der `HeaderText` Eigenschaften).

Im vorherigen Lernprogramm erläutert, wie Sie formatieren die `UnitPrice` BoundField als Währung in nur-Lese-Modus und Bearbeitungsmodus. Führen wir die gleichen hier ein. Beachten Sie, dass dies erforderlich Festlegen der BoundField `DataFormatString` Eigenschaft, um `{0:c}`, dessen `HtmlEncode` Eigenschaft, um `false`, und die zugehörige `ApplyFormatInEditMode` auf `true`, wie in Abbildung 2 dargestellt.


[![Konfigurieren Sie die UnitPrice BoundField-anzuzeigende als Währung](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Abbildung 2**: Konfigurieren der `UnitPrice` BoundField anzuzeigende als Währung ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Formatieren der `UnitPrice` als Währung in der Bearbeitung Schnittstelle erfordert, erstellen einen Ereignishandler für der GridView `RowUpdating` Ereignis, das analysiert die Currency-formatierte Zeichenfolge in eine `decimal` Wert. Bedenken Sie, dass die `RowUpdating` -Ereignishandler aus dem letzten Lernprogramm auch überprüft, um sicherzustellen, dass vom Benutzer angegebenen ein `UnitPrice` Wert. Allerdings für dieses Lernprogramm nehmen wir ermöglicht dem Benutzer, um den Preis zu unterdrücken.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Unsere GridView umfasst eine `QuantityPerUnit` BoundField-, aber diese BoundField-sollte nur zu Anzeigezwecken sein und sollte nicht vom Benutzer bearbeitet werden. Um dies zu sortieren, legen Sie einfach die BoundFields' `ReadOnly` Eigenschaft `true`.


[![Aktivieren des Schreibschutzes für die QuantityPerUnit BoundField-](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Abbildung 3**: Stellen Sie die `QuantityPerUnit` BoundField schreibgeschützt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Zum Schluss das Kontrollkästchen Sie Bearbeiten aktivieren aus der GridView Smarttag. Nach Abschluss dieser Schritte der `ErrorHandling.aspx` Seite Designer sollte ähnlich wie in Abbildung 4 aussehen.


[![Entfernen Sie alle bis auf die erforderliche BoundFields und Kontrollkästchen bearbeiten das Kontrollkästchen aktivieren](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Abbildung 4**: Entfernen Sie alle außer der benötigt BoundFields und aktivieren Sie das Kontrollkästchen aktivieren bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


An diesem Punkt haben wir eine Liste aller Produkte `ProductName`, `QuantityPerUnit`, `UnitPrice`, und `UnitsInStock` Felder; allerdings nur die `ProductName`, `UnitPrice`, und `UnitsInStock` Felder können bearbeitet werden.


[![Benutzer können nun problemlos Produkte Namen, Preise und Units In Stock Felder bearbeiten](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Abbildung 5**: Benutzer können nun problemlos bearbeiten Produkte Namen, Preise und Units In Stock Felder ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Schritt 2: Ordnungsgemäß Ausnahmebehandlung DAL-Ebene

Unsere bearbeitbaren GridView wunderbar funktioniert, zwar beim Benutzer zulässigen Werte für die bearbeitete Product Name, Preis und Einheiten im Lager eingeben wird eine Ausnahme ausgelöst ungültige Werte eingeben. Z. B. Auslassen der `ProductName` Werts wird eine [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) ausgelöst wird, seit der `ProductName` Eigenschaft in der `ProdcutsRow` -Klasse verfügt über seine `AllowDBNull` -Eigenschaftensatz auf `false`; Wenn der Datenbank betriebsbereit ist, eine `SqlException` von TableAdapter ausgelöst wird, beim Verbinden mit der Datenbank. Ohne weitere Aktionen durchzuführen, diese Ausnahmen Blase Einrichten von der Datenzugriffsebene, die Geschäftslogikschicht und anschließend auf der ASP.NET-Seite und schließlich auf die ASP.NET-Laufzeit.

Je nach Konfiguration Ihrer Webanwendung und davon, ob die Anwendung von besuchte `localhost`, eine nicht behandelte Ausnahme kann dazu führen, entweder eine generische Fehlerseite des Servers, einer ausführlichen Fehlerbericht oder eine benutzerfreundliche Webseite. Finden Sie unter [Web Application Error Handling in ASP.NET](http://www.15seconds.com/issue/030102.htm) und [CustomErrors Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) detaillierte Informationen wie die ASP.NET-Laufzeit auf eine nicht abgefangene Ausnahme reagiert.

Abbildung 6 zeigt den Bildschirm aufgetreten beim Versuch, ein Produkt zu aktualisieren, ohne die `ProductName` Wert. Dies ist die Standardeinstellung, die ausführliche Fehlermeldung-Bericht angezeigt, wenn kommen `localhost`.


[![Das Weglassen der Product Name wird Anzeige Ausnahmedetails:](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Abbildung 6**: Auslassen der Product Name wird Anzeige Ausnahmedetails ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Während solche Ausnahmedetails hilfreich sind, wenn eine Anwendung zu testen, eignet sich einen Endbenutzer Bildschirm bei einer Ausnahme vorlegen kleiner als. Ein Endbenutzer, die wahrscheinlich nicht wissen, was eine `NoNullAllowedException` wird oder warum sie verursacht wurde. Ein besserer Ansatz ist, stehen dem Benutzer eine benutzerfreundlichere Meldung darauf hingewiesen, dass beim Versuch, das Produkt zu aktualisieren sind Probleme aufgetreten.

Wenn eine Ausnahme tritt auf, wenn der Vorgang ausführt, geben Sie die Ereignisse nach der Ebene in der ObjectDataSource und das Webserver-Steuerelement eine Möglichkeit zum Erkennen und die Ausnahme von bis zu die ASP.NET-Laufzeit bubbling Abbrechen. In unserem Beispiel erstellen Sie nun einen Ereignishandler für der GridView `RowUpdated` Ereignis, das bestimmt, ob eine Ausnahme ausgelöst wurde, und wenn dies der Fall ist, die Details der Ausnahme in einem Label-Steuerelement zeigt.

Starten, indem Sie eine Bezeichnung auf der ASP.NET-Seite festlegen seiner `ID` Eigenschaft `ExceptionDetails` und gelöscht wird, seine `Text` Eigenschaft. Um sollten Sie die der Benutzer auf diese Meldung zu zeichnen, legen Sie seine `CssClass` Eigenschaft, um `Warning`, ist eine CSS-Klasse, die wir, um hinzugefügt die `Styles.css` Datei im vorherigen Lernprogramm. Beachten Sie, dass diese CSS-Klasse bewirkt, dass die Bezeichnung, die in Rot, kursiv, fett, Extragroß Schriftart anzuzeigende Text.


[![Fügen Sie ein Websteuerelement Bezeichnung auf der Seite "](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Abbildung 7**: Fügen Sie ein Websteuerelement Bezeichnung auf der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Eine Ausnahme ist aufgetreten, da diese Bezeichnung Websteuerelement sichtbar nur unmittelbar nach dem sein soll, legen Sie dessen `Visible` Eigenschaft auf "false" in der `Page_Load` Ereignishandler:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Mit diesem Code wird auf dem ersten Besuch der Seite und die nachfolgenden Postbacks der `ExceptionDetails` -Steuerelement seine `Visible` -Eigenschaftensatz auf `false`. Bei einem DAL-BLL Level Ausnahme aus, die in der GridView erkannt werden kann `RowUpdated` Ereignishandler, d. h. legen wir den `ExceptionDetails` des Steuerelements `Visible` Eigenschaft auf "true". Da Web Control-Ereignishandler nach dem Auftreten der `Page_Load` -Ereignishandler im Lebenszyklus Seite die Bezeichnung wird angezeigt. Allerdings auf das nächste Postback der `Page_Load` Ereignishandler zurückversetzt der `Visible` -Eigenschaft zurück auf `false`, erneut aus der Ansicht ausgeblendet.

> [!NOTE]
> Alternativ können wir die Notwendigkeit für die Einstellung Entfernen der `ExceptionDetails` des Steuerelements `Visible` Eigenschaft im `Page_Load` durch Zuweisen der `Visible` Eigenschaft `false` in die deklarative Syntax und Deaktivieren von seinen Ansichtszustand (Festlegen der `EnableViewState` Eigenschaft `false`). Wir verwenden diese alternativen Ansatz in einem späteren Lernprogramm.


Mit dem Label-Steuerelement hinzugefügt, wird im nächsten Schritt erstellen Sie den Ereignishandler für der GridView `RowUpdated` Ereignis. Wählen Sie im Designer die GridView, wechseln Sie zum Fenster Eigenschaften und klicken Sie auf das Blitzsymbol Auflisten der GridView-Ereignisse. Es bereits darf es ein Eintrag für der GridView `RowUpdating` Ereignis, wie wir einen Ereignishandler für dieses Ereignis weiter oben in diesem Lernprogramm erstellt haben. Erstellen Sie einen Ereignishandler für das `RowUpdated` Ereignis sowie.


![Erstellen Sie einen Ereignishandler für der GridView RowUpdated-Ereignis](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Abbildung 8**: Erstellen Sie einen Ereignishandler für der GridView `RowUpdated` Ereignis


> [!NOTE]
> Sie können auch den Ereignishandler über die Dropdownlisten am oberen Rand der Code-Behind-Klassendatei erstellen. Wählen Sie aus der Dropdown-Liste auf der linken Seite die GridView und die `RowUpdated` Ereignis von der auf der rechten Seite.


Durch das Erstellen dieser Ereignishandler wird den folgenden Code auf der ASP.NET-Seite Code-Behind-Klasse hinzugefügt:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Dieser Ereignishandler zweiten Eingabeparameter ist ein Objekt des Typs [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), dem verfügt über drei Eigenschaften für das Behandeln von Ausnahmen von Bedeutung sind:

- `Exception`Ein Verweis auf die ausgelöste Ausnahme Wenn keine Ausnahme ausgelöst wurde, wird diese Eigenschaft einen Wert aufweisen.`null`
- `ExceptionHandled`Ein boolescher Wert, der angibt, ob die Ausnahme behandelt wurde, in der `RowUpdated` Ereignishandler; Wenn `false` (Standard), die Ausnahme wird erneut ausgelöst, bis zu die ASP.NET-Laufzeit durchsickert
- `KeepInEditMode`Wenn auf festgelegt `true` bearbeitete GridView Zeile bleibt im Bearbeitungsmodus befindet; Wenn `false` (Standard), die GridView-Zeile, die nur-Lese-Modus wiederhergestellt

Getesteten Codes, prüfen Sie dann, sollte Wenn `Exception` nicht `null`, was bedeutet, dass eine Ausnahme, beim Ausführen des Vorgangs ausgelöst wurde. Wenn dies der Fall ist, möchten wir:

- Eine benutzerfreundliche Meldung in die `ExceptionDetails` Bezeichnung
- Um anzugeben Sie, dass die Ausnahme behandelt wurde
- Halten Sie die GridView-Zeile in den Bearbeitungsmodus

Der folgende Code führt diese Ziele zu erreichen:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Dieser Ereignishandler beginnt, indem überprüft wird, finden Sie unter `e.Exception` ist `null`. Ist dies nicht der Fall, die `ExceptionDetails` Bezeichnungsfelds `Visible` -Eigenschaftensatz auf `true` und seine `Text` -Eigenschaft auf "Es wurde ein Problem mit dem Aktualisieren des Produkts." Die Details der tatsächliche Ausnahme, die ausgelöst wurde, befinden sich in der `e.Exception` des Objekts `InnerException` Eigenschaft. Diese Beschreibung der eingeschlossenen Ausnahme überprüft wird und wenn es einen bestimmten Typ handelt, an eine zusätzliche, hilfreiche-Nachricht angefügt der `ExceptionDetails` Bezeichnungsfelds `Text` Eigenschaft. Abschließend wird die `ExceptionHandled` und `KeepInEditMode` Eigenschaften festgelegt `true`.

Abbildung 9 zeigt ein Screenshot, der auf dieser Seite aus, wenn der Name des Produkts auslassen; Abbildung 10 zeigt die Ergebnisse bei der Eingabe eine unzulässige `UnitPrice` Wert (-50).


[![Die ProductName BoundField-muss einen Wert enthalten.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Abbildung 9**: die `ProductName` BoundField muss einen Wert enthalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Negative UnitPrice-Werte sind nicht zulässig.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Abbildung 10**: Negative `UnitPrice` Werte sind nicht zulässig ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Durch Festlegen der `e.ExceptionHandled` Eigenschaft `true`, `RowUpdated` -Ereignishandler hat festgelegt, dass sie die Ausnahme verarbeitet wurde. Die Ausnahme wird nicht aus diesem Grund bis zu die ASP.NET-Laufzeit weitergeleitet.

> [!NOTE]
> Abbildung 9 und 10 zeigen eine ordnungsgemäße Möglichkeit zum Behandeln von Ausnahmen, die aufgrund von ungültigen Benutzereingaben ausgelöst werden. Im Idealfall jedoch solche ungültige Eingabe wird nie Reichweite der Geschäftslogikschicht ursprünglich, wie die ASP.NET-Seite sicherstellen sollten, dass Eingaben des Benutzers gültig, vor dem Aufrufen sind der `ProductsBLL` Klasse `UpdateProduct` Methode. In unserem nächsten Lernprogramm zum Hinzufügen von Steuerelementen für die Validierung zu die bearbeiten und Einfügen von Schnittstellen, um sicherzustellen, dass die Daten übermittelt, die Business Logic Layer eingehendem entspricht der Geschäftsregeln. Die Validierungssteuerelemente nicht nur zu verhindern, dass den Aufruf der `UpdateProduct` Methode, bis die vom Benutzer bereitgestellten Daten gültig ist, aber auch eine umfangreichere benutzererfahrung, zum Identifizieren der Probleme mit der Dateneingabe bieten.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Schritt 3: Ordnungsgemäß Ausnahmebehandlung BLL auf Dokumentebene

Beim Einfügen von möglicherweise aktualisieren oder Löschen von Daten, die Datenzugriffsebene eine Ausnahme im ein Daten-bezogener Fehler ausgelöst. Die Datenbank ist möglicherweise offline, eine erforderlichen Datenbanktabellen-Spalte kann einen angegebenen Wert nicht hatten, oder eine Einschränkung auf Tabellenebene wurde verletzt. Zusätzlich zu den Ausnahmen streng datenbezogene können die Geschäftslogikschicht Ausnahmen verwenden, um anzugeben, wann Geschäftsregeln verletzt wurden. In der [erstellen eine Geschäftslogikschicht](../introduction/creating-a-business-logic-layer-vb.md) Lernprogramm, z. B. wir eine regelüberprüfung Business hinzugefügt, mit dem ursprünglichen `UpdateProduct` überladen. Insbesondere, wenn der Benutzer ein Produkt kennzeichnen wurde Sie als nicht mehr unterstützt, erforderlich sind wir, dass das Produkt nicht die einzige von seiner Lieferanten bereitgestellt werden. Wenn diese Bedingung gegen die verstoßen wurde, ein `ApplicationException` ausgelöst wurde.

Für die `UpdateProduct` Überladung, die in diesem Lernprogramm erstellt haben, fügen Sie eine Geschäftsregel, die verhindert, dass die `UnitPrice` festgelegt wird, um einen neuen Wert, der mehr als zweimal die ursprüngliche Feld `UnitPrice` Wert. Anpassen, um dies zu erreichen, die `UpdateProduct` überladen, sodass er führt diese Überprüfung aus, und löst eine `ApplicationException` , wenn die Regel verletzt wird. Die aktualisierte Methode lautet folgendermaßen:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Durch diese Änderung alle Preis Update, das mehr als doppelt vorhandene Preis führt dazu, dass ein `ApplicationException` ausgelöst wird. Ebenso wie die Ausnahme, die ausgelöst wird, von der DAL, die diesem BLL-ausgelöst `ApplicationException` erkannt und behandelt der GridView `RowUpdated` -Ereignishandler. In der Tat die `RowUpdated` --Ereignishandler Code geschrieben, ordnungsgemäß erkennt diese Ausnahme und zeigt die `ApplicationException`des `Message` Eigenschaftswert. Abbildung 11 zeigt einen Screenshot, wenn ein Benutzer versucht, aktualisieren Sie den Preis Chai auf 50,00 $, also mehr als das Doppelte der aktuellen Preis des 19,95 $.


[![Die Geschäftsregeln unterbinden, Preis erhöht, die mehr als doppelt so Preis eines Produkts](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Abbildung 11**: die Geschäftsregel Disallow Preis erhöht, die mehr als doppelt so Preis eines Produkts ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> Im Idealfall würden unsere Geschäftslogikregeln umgestaltet werden, von der `UpdateProduct` Überladungen der Methode und in eine gängige Methode. Dies wird als Übung für den Leser belassen.


## <a name="summary"></a>Zusammenfassung

Beim Einfügen, aktualisieren und Löschen von Vorgängen das Websteuerelement Daten und die Beteiligten ObjectDataSource vor und nach dem Kompatibilitätsgrad Ereignisse auslösen, Buchstütze den aktuellen Vorgang. Wie in diesem Lernprogramm und der vorherigen Abfrage, wurde erläutert, bei der Arbeit mit einer bearbeitbaren GridView der GridView `RowUpdating` Ereignis wird ausgelöst, gefolgt von der ObjectDataSource `Updating` an diesem Punkt der Updatebefehl, um das ObjectDataSource ausgelöst wird-Ereignis zugrunde liegende Objekt. Nachdem der Vorgang abgeschlossen wurde, wird das ObjectDataSource `Updated` Ereignis wird ausgelöst, gefolgt von der GridView `RowUpdated` Ereignis.

Wir können Ereignishandler für die Ereignisse vorab Ebene, um die Eingabeparameter anpassen oder die Ereignisse nach der Ebene, um zu überprüfen und reagieren auf Ergebnisse(n) der Vorgang erstellen. Nach der Servicelevel-Ereignishandler werden am häufigsten verwendet, um festzustellen, ob eine Ausnahme während des Vorgangs aufgetreten ist. Bei einer Ausnahme können diese nach der Ebene Ereignishandler optional die Ausnahme für ihre eigenen behandeln. In diesem Lernprogramm wurde erläutert, wie solche eine Ausnahme zu behandeln, indem eine benutzerfreundliche Fehlermeldung angezeigt.

In den nächsten Lernprogrammen erfahren wir, wie Sie verringern die Wahrscheinlichkeit von Ausnahmen, die aus der datenformatierung Probleme (z. B. eine Negative eingeben `UnitPrice`). Insbesondere sehen wir uns zum Hinzufügen von Steuerelementen für die Validierung zu bearbeiten und Einfügen von Schnittstellen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
[Weiter](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
