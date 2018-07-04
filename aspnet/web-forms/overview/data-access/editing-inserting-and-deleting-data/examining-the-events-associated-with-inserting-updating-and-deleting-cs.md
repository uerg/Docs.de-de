---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Überprüfen von Ereignissen im Zusammenhang mit einfügen, aktualisieren und löschen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial untersuchen wir werden mit den Ereignissen, die vor, während und nach einer Einfügung auftreten, aktualisieren oder Löschen von ASP.NET-Daten in einem Web-Steuerelement. W...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: cec25809038cbe6a114b0e36ef7265d84bbe9b92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363299"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Untersuchen von Ereignissen im Zusammenhang mit einfügen, aktualisieren und löschen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) oder [PDF-Datei herunterladen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> In diesem Tutorial untersuchen wir werden mit den Ereignissen, die vor, während und nach einer Einfügung auftreten, aktualisieren oder Löschen von ASP.NET-Daten in einem Web-Steuerelement. Wir sehen auch die Bearbeitung-Schnittstelle, um nur eine Teilmenge der Felder Product aktualisieren anpassen.


## <a name="introduction"></a>Einführung

Wenn Sie die integrierten einfügen, bearbeiten oder löschen die Features der GridView, DetailsView oder FormView-Steuerelemente verwenden, die eine Vielzahl von Schritten eintritt, nach Abschluss des Prozesses einen neuen Datensatz hinzufügen, aktualisieren oder Löschen eines vorhandenen Datensatzes durch den Endbenutzer. Wie wir unter den [vorherigen Tutorial](an-overview-of-inserting-updating-and-deleting-data-cs.md), wenn eine Zeile in der GridView-Ansicht die Schaltfläche "Bearbeiten bearbeitet wird" wird ersetzt durch Schaltflächen "Update" und "Abbrechen", und die BoundFields aktivieren in Textfelder ein. Nachdem der Endbenutzer die Daten aktualisiert und Update klickt, werden die folgenden Schritte beim Postback ausgeführt:

1. Das GridView füllt die "ObjectDataSource" `UpdateParameters` mit den geänderten Datensatz eindeutig identifizierenden Felder (über die `DataKeyNames` Eigenschaft) zusammen mit den Werten, die vom Benutzer eingegebene
2. GridView ruft seine "ObjectDataSource" `Update()` -Methode, die wiederum die entsprechende Methode des zugrunde liegenden Objekts aufruft (`ProductsDAL.UpdateProduct`, in unserem vorherigen Lernprogramm)
3. Die zugrunde liegenden Daten, die nun den aktualisierten Änderungen enthält, werden erneut an die GridView gebunden.

Während dieser Sequenz von Schritten wird eine Anzahl von Ereignissen ausgelöst werden, sodass-Ereignishandler, um das Hinzufügen benutzerdefinierter Logik zu erstellen, falls erforderlich. Z. B. vor Schritt 1, der GridView `RowUpdating` -Ereignis ausgelöst wird. An diesem Punkt können wir die aktualisierungsanforderung Abbrechen, wenn einige Überprüfungsfehler vorliegt. Wenn die `Update()` Methode wird aufgerufen, dem ObjectDataSource-Steuerelement `Updating` Ereignis wird ausgelöst, so hinzufügen, oder passen Sie die Werte aller der `UpdateParameters`. Nach dem ObjectDataSource-Steuerelement zugrunde liegende-Methode des Objekts wurde ausgeführt, das "ObjectDataSource" `Updated` Ereignis wird ausgelöst. Ein Ereignishandler für die `Updated` untersuchen Ereignisses kann die Details über den Updatevorgang, z. B. wie viele Zeilen betroffen sind und ob eine Ausnahme aufgetreten ist. Nach Schritt2 der GridView `RowUpdated` Ereignis wird ausgelöst, einen Ereignishandler für dieses Ereignis kann, überprüfen Sie weitere Informationen zu den Updatevorgang nur ausgeführt.

Abbildung 1 zeigt diese Reihe von Ereignissen und Schritte bei der Aktualisierung einer GridView-Ansicht. Das Ereignismuster in Abbildung 1 ist nicht eindeutig, zum Aktualisieren von mit einer GridView-Ansicht. Einfügen, aktualisieren oder Löschen von Daten aus der GridView, precipitates DetailsView oder FormView-Steuerelement die gleiche Sequenz von vorab und nachträglich auf Ereignisse für das Web-Steuerelement und dem ObjectDataSource-Steuerelement.


[![Eine Reihe von vor und nach der Ereignisse auslösen, beim Aktualisieren von Daten in einer GridView-Ansicht](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Abbildung 1**: eine Reihe von vor und nach der Ereignisse auslösen beim Aktualisieren von Daten in einer GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


Steuert, in diesem Tutorial wir diese Ereignisse verwenden untersuchen werden, erweitern Sie die integrierten einfügen, aktualisieren und löschen die Funktionen des ausgewählten ASP.NET Web. Wir sehen auch die Bearbeitung-Schnittstelle, um nur eine Teilmenge der Felder Product aktualisieren anpassen.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Schritt 1: Aktualisieren eines Produkts`ProductName`und`UnitPrice`Felder

In den Bearbeitungsmodus Schnittstellen aus dem vorherigen Lernprogramm *alle* Product-Felder, die nicht schreibgeschützt waren, musste der enthalten sein. Angenommen, wenn wir zum Entfernen eines Felds aus der GridView waren - `QuantityPerUnit` – beim Aktualisieren der Daten das Websteuerelement Daten nicht dem ObjectDataSource-Steuerelement festgelegt wäre `QuantityPerUnit` `UpdateParameters` Wert. Dem ObjectDataSource-Steuerelement würde dann übergebe eine `null` -Wert in der `UpdateProduct` Geschäftslogikschicht (Business Logic Layer, BLL)-Methode, die des bearbeiteten Datenbankdatensatzes ändern würde `QuantityPerUnit` Spalte eine `NULL` Wert. Auf ähnliche Weise, wenn ein erforderliches Feld, z. B. `ProductName`, entfernt von der Bearbeitung-Schnittstelle, schlägt das Update mit einer "*Spaltenname 'ProductName' lässt keine NULL-Werte*" Ausnahme. Der Grund für dieses Verhalten wurde, da dem ObjectDataSource-Steuerelement, zum Aufrufen konfiguriert wurde der `ProductsBLL` Klasse `UpdateProduct` -Methode, die Eingabeparameter für jedes der Felder Product erwartet. Aus diesem Grund der ObjectDataSource `UpdateParameters` enthält einen Parameter für jede der Methode Eingabeparameter des.

Wenn ein Daten-Websteuerelement bereit, die dem Benutzer nur eine Teilmenge der Felder aktualisieren können soll, müssen wir entweder programmgesteuert die fehlende festlegen `UpdateParameters` Werte in dem ObjectDataSource-Steuerelement `Updating` -Ereignishandler oder erstellen und Aufrufen eine Methode BLL, erwartet, dass nur eine Teilmenge der Felder. Sehen wir uns dieser zweite Ansatz.

Insbesondere erstellen wir eine Seite, die zeigt nur die `ProductName` und `UnitPrice` Felder in einem bearbeitbaren GridView-Ansicht. Dieser GridView Bearbeitungsoberfläche lässt sich nur auf den Benutzer, die zwei angezeigten Felder aktualisieren `ProductName` und `UnitPrice`. Da diese Bearbeitung Schnittstelle nur eine Teilmenge von Feldern für ein Produkt des bereitstellt, wir müssen entweder ein ObjectDataSource-Steuerelement zu erstellen, die der vorhandenen Geschäftslogikschicht verwendet `UpdateProduct` Methode und die fehlenden Werte der Product-Feld festgelegt programmgesteuert in die `Updating` Ereignis Handler, oder wir müssen eine neue BLL-Methode erstellen, die nur die Teilmenge der Felder in den GridView-Ansicht definiert erwartet. Für dieses Tutorial lassen Sie uns die zweite Option verwenden und erstellen Sie eine Überladung von der `UpdateProduct` -Methode, eine, die in nur drei Parameter akzeptiert: `productName`, `unitPrice`, und `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Wie die ursprüngliche `UpdateProduct` Methode diese Überladung wird gestartet, indem überprüft wird, festzustellen, ob ein Produkt in der Datenbank mit dem angegebenen `ProductID`. Wenn nicht der Fall, gibt `false`, informiert, dass die Fehler bei der Anforderung zum Aktualisieren der Produktinformationen. Andernfalls aktualisiert die vorhandene Produktdatensatz `ProductName` und `UnitPrice` Felder entsprechend, und führt einen Commit für die Aktualisierung durch Aufrufen der TableAdpater des `Update()` Methode und übergeben die `ProductsRow` Instanz.

Mit dieser Ergänzung unserer `ProductsBLL` -Klasse, sind wir bereit, die vereinfachte GridView-Schnittstelle zu erstellen. Öffnen der `DataModificationEvents.aspx` in die `EditInsertDelete` Ordner und Hinzufügen einer GridView-Ansicht auf der Seite. Erstellen Sie eine neue "ObjectDataSource", und konfigurieren Sie ihn zur Verwendung der `ProductsBLL` -Klasse mit der `Select()` Mapping-Methode, um `GetProducts` und die zugehörige `Update()` Mapping-Methode, um die `UpdateProduct` Überladung, die nur in der `productName`, `unitPrice`, und `productID` Eingabeparameter. Abbildung 2 zeigt den Assistent zum Erstellen der Datenquelle aus, bei der Zuordnung von dem ObjectDataSource-Steuerelement `Update()` Methode, um die `ProductsBLL` Klasse der neuen `UpdateProduct` -methodenüberladung.


[![Map-Methode für das ObjectDataSource-Steuerelement-Update() an die neue UpdateProduct-Überladung](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Abbildung 2**: Ordnen Sie dem ObjectDataSource-Steuerelement `Update()` Methode, um neue `UpdateProduct` überladen ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Da es sich bei unserem Beispiel zunächst nur die Möglichkeit zum Bearbeiten von Daten, aber nicht zum Einfügen oder Löschen von Datensätzen benötigt, können Sie explizit angeben, dass das "ObjectDataSource" `Insert()` und `Delete()` Methoden sollten nicht zugeordnet werden, auf die `ProductsBLL` die Klasse, Methoden die INSERT- und DELETE-Registerkarten und (keine) aus der Dropdown-Liste auswählen.


[![Wählen Sie aus der Dropdown-Liste für die INSERT- und DELETE-Registerkarten (keine)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Abbildung 3**: Wählen Sie (keine) aus der Dropdown-Liste für die einfügen und Löschen von Registerkarten ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Nach Abschluss dieses Assistenten das Kontrollkästchen Sie bearbeiten Aktivieren von GridView Smarttag.

Mit dem Abschluss des Assistenten Create Data Source "und", die an die GridView zu binden hat Visual Studio die deklarative Syntax für beide Steuerelemente erstellt. Wechseln Sie zur Quellansicht, deklarative Markup dem ObjectDataSource-Steuerelement, überprüfen Sie die folgenden ist:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Da es sich um keine Zuordnungen für das "ObjectDataSource" `Insert()` und `Delete()` Methoden gibt es keine `InsertParameters` oder `DeleteParameters` Abschnitte. Darüber hinaus, da die `Update()` Methode zugeordnet ist die `UpdateProduct` methodenüberladung, die nur drei Eingabeparameter akzeptiert die `UpdateParameters` Abschnitt verfügt über nur drei `Parameter` Instanzen.

Beachten Sie, dass das "ObjectDataSource" `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`. Diese Eigenschaft wird von Visual Studio automatisch festgelegt, mit dem Konfigurieren von Datenquellen-Assistenten. Aber da unsere Methoden der Geschäftslogikschicht erwarten, dass die ursprüngliche nicht `ProductID` Wert übergeben werden, das Entfernen dieser Eigenschaft die Zuweisung aus dem ObjectDataSource-Steuerelements deklarative Syntax.

> [!NOTE]
> Wenn Sie einfach löschen, die `OldValuesParameterFormatString` Eigenschaftswert im Eigenschaftenfenster in der Entwurfsansicht, die Eigenschaft in der deklarativen Syntax auch weiterhin vorhanden, jedoch wird auf eine leere Zeichenfolge festgelegt werden. Entfernen Sie entweder die Eigenschaft vollständig aus der deklarativen Syntax oder, im Eigenschaftenfenster den Wert auf den Standardwert festgelegt, `{0}`.


Während dem ObjectDataSource-Steuerelement nur hat `UpdateParameters` des Produkts Name, Preis und ID, Visual Studio wurde hinzugefügt eine BoundField- oder CheckBoxField in den GridView-Ansicht für jedes der Felder des Produkts.


[![GridView enthält eine BoundField- oder CheckBoxField für jedes der Felder für den Produktanforderungen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Abbildung 4**: GridView enthält eine BoundField- oder CheckBoxField für jedes der Felder des Produkts ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Wenn der Endbenutzer ein Produkts bearbeitet und klickt auf die Schaltfläche "Aktualisieren", listet die GridView Felder, die nicht schreibgeschützt sind. Dann wird den Wert des entsprechenden Parameters in der "ObjectDataSource" `UpdateParameters` -Auflistung, um den Wert, der vom Benutzer eingegeben werden. Wenn kein entsprechender Parameter vorhanden ist, fügt der Auflistung von GridView hinzu. Aus diesem Grund enthält unsere GridView BoundFields und CheckBoxFields für alle Felder des Produkts, dem ObjectDataSource-Steuerelement wird am Ende Aufrufen der `UpdateProduct` Überladung, die in alle diese Parameter, trotz der Tatsache akzeptiert, die dem ObjectDataSource-Steuerelement deklaratives Markup gibt nur drei Parameter (siehe Abbildung 5). Auf ähnliche Weise, es ist eine Kombination nicht schreibgeschützten Produkt Felder in den GridView-Ansicht, die die Eingabeparameter für entsprechen nicht, eine `UpdateProduct` überladen, eine Ausnahme ausgelöst, wenn der Versuch, zu aktualisieren.


[![Das GridView-wird dem ObjectDataSource-Steuerelement UpdateParameters Auflistung Parameter hinzufügen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Abbildung 5**: die GridView wird Hinzufügen von Parametern zu dem ObjectDataSource-Steuerelement `UpdateParameters` Auflistung ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Um sicherzustellen, dass es sich bei dem ObjectDataSource-Steuerelement ruft die `UpdateProduct` -Überladung mit nur des produktanforderungen den Namen, Preis und-ID, müssen wir die GridView zu beschränken, mit dem editierbaren Felder für nur den `ProductName` und `UnitPrice`. Dies kann erreicht werden, durch das Entfernen von den anderen BoundFields und CheckBoxFields, durch Festlegen von den anderen Feldern `ReadOnly` Eigenschaft `true`, oder durch eine Kombination aus beidem. Für dieses Tutorial lassen Sie uns einfach entfernen Sie alle GridView Felder mit Ausnahme der `ProductName` und `UnitPrice` BoundFields, nach der GridView deklarative Markup wird wie folgt aussehen:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Obwohl die `UpdateProduct` Überladung erwartet drei Parameter lassen wir nur zwei BoundFields in unserer GridView-Ansicht. Grund hierfür ist die `productID` Eingabeparameter den Wert eines Primärschlüssels und über den Wert des übergebenen der `DataKeyNames` -Eigenschaft für die bearbeitete Zeile.

Unsere GridView, zusammen mit den `UpdateProduct` überladen, ermöglicht es einem Benutzer nur der Name und der Preis eines Produkts zu bearbeiten, ohne die anderen Felder des Produkts verloren.


[![Die Schnittstelle ermöglicht das Bearbeiten von nur des Produkts Name und Preis](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Abbildung 6**: die Schnittstelle ermöglicht es, Bearbeiten nur des Produktanforderungen Name und Preis ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Wie im vorherigen Tutorial erwähnt, ist es äußerst wichtig, dass die GridView Ansichtszustand s werden (Standardverhalten) aktiviert. Setzen Sie das GridView-s `EnableViewState` Eigenschaft `false`, führen Sie das Risiko gleichzeitiger Benutzer versehentlich löschen oder zu bearbeiten zeichnet. Finden Sie unter [Warnung: Problem der Parallelität mit ASP.NET 2.0 GridViews/DetailsView/FormViews diese Unterstützung zu bearbeiten bzw. löschen und, deren Ansichtszustand deaktiviert ist](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) für Weitere Informationen.


## <a name="improving-theunitpriceformatting"></a>Verbessern der`UnitPrice`Formatierung

Das GridView-Beispiel in Abbildung 6 funktioniert, während die `UnitPrice` Feld überhaupt nicht formatiert wird, im Preis anzuzeigen, die alle Währung verfügt nicht über die Symbole und verfügt über vier Dezimalstellen. Um eine währungsformatierung für die Zeilen nicht bearbeitbare anzuwenden, legen Sie einfach die `UnitPrice` BoundFields `DataFormatString` Eigenschaft `{0:c}` und die zugehörige `HtmlEncode` Eigenschaft `false`.


[![Festlegen Sie die UnitPrice DataFormatString und das "HTMLEncode"-Eigenschaften entsprechend](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Abbildung 7**: Legen Sie die `UnitPrice`des `DataFormatString` und `HtmlEncode` Eigenschaften entsprechend ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Durch diese Änderung formatieren die nicht-bearbeitbaren Zeilen den Preis als Währung; die bearbeitete Zeile wird jedoch immer noch den Wert ohne das Währungssymbol und mit vier Dezimalstellen.


[![Nicht-bearbeitbaren Zeilen sind jetzt formatiert, als Currency-Werte](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Abbildung 8**: nicht-bearbeitbaren Zeilen sind jetzt als Währungswerte formatiert ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Die formatierungsanweisungen, angegeben der `DataFormatString` Eigenschaft kann auf die Bearbeitungsschnittstelle angewendet werden, durch Festlegen der BoundField des `ApplyFormatInEditMode` Eigenschaft `true` (der Standardwert ist `false`). Können Sie diese Eigenschaft auf festgelegt `true`.


[![Legen Sie die UnitPrice BoundFields ApplyFormatInEditMode-Eigenschaft auf "true"](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Abbildung 9**: Legen Sie die `UnitPrice` BoundFields `ApplyFormatInEditMode` Eigenschaft `true` ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Mit dieser Änderung wird der Wert, der die `UnitPrice` angezeigt, in der bearbeiteten Zeile wird auch als Währung formatiert.


[![Bearbeiteter der Wert der Zeile UnitPrice ist jetzt formatiert, als Währung](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Abbildung 10**: The bearbeitet Zeile `UnitPrice` Wert ist jetzt als Währung formatiert ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Ein Produkt jedoch mit dem Währungssymbol im Textfeld für die zu aktualisieren, wie z. B. $19.00 löst eine `FormatException`. Wenn die GridView versucht, dem ObjectDataSource-Steuerelement die vom Benutzer angegebenen Werte zuweisen `UpdateParameters` Auflistung kann nicht konvertiert werden kann die `UnitPrice` "$19.00" in eine Zeichenfolge der `decimal` erforderlich, durch den Parameter (siehe Abbildung 11). Zur Umgehung des Problems erstellen wir einen Ereignishandler für der GridView `RowUpdating` Ereignis und den vom Benutzer bereitgestellten analysieren `UnitPrice` als eine Währung formatierten `decimal`.

GridView `RowUpdating` Ereignis akzeptiert als zweiten Parameter ein Objekt vom Typ [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), wozu ein `NewValues` Wörterbuch als eine seiner Eigenschaften, die die benutzerdefinierten Werte werden enthält. dem ObjectDataSource-Steuerelement zugewiesen `UpdateParameters` Auflistung. Wir können die vorhandenen überschreiben `UnitPrice` Wert in der `NewValues` Auflistung mit einem Dezimalwert analysiert die folgenden Codezeilen in das Währungsformat mit der `RowUpdating` -Ereignishandler:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Wenn der Benutzer angegeben hat eine `UnitPrice` Wert (z. B. "$19.00"), wird dieser Wert mit der decimal-Wert berechnet, indem überschrieben [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analysieren den Wert als Währung. Dieser Dezimaltrennzeichen ordnungsgemäß analysiert wird, im Falle alle Währungssymbole, Kommas, Dezimalstellen und So weiter, und verwendet die [NumberStyles-Enumeration](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) in die [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) Namespace.

Abbildung 11 zeigt sowohl das Problem zurückzuführen Währungssymbole in den vom Benutzer bereitgestellten `UnitPrice`, sowie wie GridView `RowUpdating` -Ereignishandler kann verwendet werden, um derartige Eingabe ordnungsgemäß zu analysieren.


[![Bearbeiteter der Wert der Zeile UnitPrice ist jetzt formatiert, als Währung](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Abbildung 11**: The bearbeitet Zeile `UnitPrice` Wert ist jetzt als Währung formatiert ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Schritt 2: Verbot`NULL UnitPrices`

Während die Datenbank konfiguriert ist, um zu ermöglichen `NULL` Werte in der `Products` Tabelle `UnitPrice` Spalte, wir könnten verhindern, dass Benutzer, die Zugriff auf diese Seite von der Angabe einer `NULL` `UnitPrice` Wert. D. h. wenn ein Benutzer nicht zur Eingabe einer `UnitPrice` Wert, wenn eine Produktzeile bearbeiten, anstatt Sie zu speichern der Ergebnisse in der Datenbank, die wir eine Nachricht, die den Benutzer darüber informiert, dass bearbeitete Produkte über diese Seite, einen Preis angegeben benötigen angezeigt werden soll.

Die `GridViewUpdateEventArgs` -Objekt übergeben, in des GridView `RowUpdating` Ereignishandler enthält eine `Cancel` Eigenschaft, wenn auf festgelegt `true`, beendet den Prozess aktualisierten. Jetzt erweitern wir die `RowUpdating` Ereignishandler festlegen `e.Cancel` zu `true` und zeigt eine Meldung, die Erklärung, wenn die `UnitPrice` Wert in der `NewValues` Auflistung `null`.

Starten, indem ein Label-Steuerelement hinzufügen, um die Seite mit dem Namen `MustProvideUnitPriceMessage`. Diesen Label-Steuerelement wird angezeigt, wenn der Benutzer nicht an eine `UnitPrice` Wert, wenn ein Produkt aktualisiert. Legen Sie die Bezeichnung des `Text` Eigenschaft auf "müssen Sie einen Preis für das Produkt bereitstellen." Ich habe auch eine neue CSS-Klasse im erstellt `Styles.css` mit dem Namen `Warning` mit folgender Definition:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Legen Sie schließlich die Bezeichnung des `CssClass` Eigenschaft `Warning`. An diesem Punkt sollte der Designer angezeigt werden wie in Abbildung 12 dargestellt die Warnmeldung in einem roten, fett, kursiv, sehr große Schriftgrad über GridView.


[![Eine Bezeichnung wurde hinzugefügt, über die GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Abbildung 12**: eine Bezeichnung wurde wurde hinzugefügt über die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Standardmäßig diesen Label ausgeblendet werden soll, legen Sie daher die `Visible` Eigenschaft `false` in die `Page_Load` -Ereignishandler:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Wenn der Benutzer versucht, ein Produkt zu aktualisieren, ohne die `UnitPrice`, wir möchten die Aktualisierung abzubrechen und zeigen Sie den Text der Warnung. Erweitern Sie des GridView `RowUpdating` Ereignishandler wie folgt:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Wenn ein Benutzer versucht, ein Produkt zu speichern, ohne einen Preis, das Update wird abgebrochen, und eine hilfreiche Meldung wird angezeigt. Während die Datenbank (und die Geschäftslogik) ermöglicht die `NULL` `UnitPrice` s, diese bestimmte ASP.NET-Seite hingegen nicht.


[![Ein Benutzer kann nicht UnitPrice leer lassen.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Abbildung 13**: ein Benutzer kann nicht verlassen `UnitPrice` leer ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Bisher haben wir gesehen, wie mit GridView `RowUpdating` Ereignis, um die Werte der Parameter zugewiesen werden, auf dem ObjectDataSource-Steuerelement programmgesteuert ändern `UpdateParameters` Auflistung sowie zum Abbrechen die Aktualisierung vollständig verarbeiten. Diese Konzepte werden dabei auf die DetailsView und FormView-Steuerelemente und gelten auch für das Einfügen und Löschen von.

Diese Aufgaben können auch ausgeführt werden, auf der Ebene "ObjectDataSource" über den Ereignishandler für die `Inserting`, `Updating`, und `Deleting` Ereignisse. Diese Ereignisse ausgelöst werden, bevor die zugeordnete Methode des zugrunde liegenden Objekts aufgerufen wird, und geben Sie eine letzte Gelegenheit-Gelegenheit, um die Auflistung der Eingabeparameter zu ändern, oder brechen Sie den Vorgang 30-tägiges. Die Ereignishandler für diese drei Ereignisse sind ein Objekt des Typs übergeben [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , die zwei wichtige Eigenschaften hat:

- [Abbrechen](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), die, wenn auf festgelegt `true`, bricht den Vorgang
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), d.h., dass die Auflistung der `InsertParameters`, `UpdateParameters`, oder `DeleteParameters`, abhängig davon, ob der Ereignishandler wird die `Inserting`, `Updating`, oder `Deleting` Ereignis

Zum Arbeiten mit den Parameterwerten auf der Ebene "ObjectDataSource" zu veranschaulichen, enthalten Sie eine DetailsView lassen Sie uns auf unserer Seite, die dem Benutzer ein neues Produkt hinzufügen können. Bereitstellen eine Schnittstelle zum schnellen Hinzufügen ein neues Produkt in der Datenbank, wird dieser DetailsView verwendet werden. Eine einheitliche Benutzeroberfläche zu, wenn wir kein neues Produkt hinzugefügt, kann der Benutzer nur die Werte für geben die `ProductName` und `UnitPrice` Felder. Standardmäßig werden die Werte, die in DetailsViews-einfügen-Schnittstelle angegeben sind nicht festgelegt werden eine `NULL` Datenbank-Wert. Wir können jedoch dem ObjectDataSource-Steuerelement `Inserting` Ereignis, um unterschiedliche Standardwerte aufweisen, einfügen, wie wir gleich sehen werden.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Schritt 3: Bereitstellen einer Schnittstelle zum Hinzufügen neuer Produkte

Ziehen Sie eine DetailsView aus der Toolbox in den Designer über die GridView, löschen Sie die `Height` und `Width` Eigenschaften und Bindung, die es dem ObjectDataSource-Steuerelement auf der Seite bereits vorhanden. Dadurch wird eine BoundField- oder CheckBoxField für jedes der Felder des Produkts hinzugefügt. Da wir diese DetailsView zu verwenden, um die neuen Produkte hinzufügen möchten, müssen wir überprüfen Sie die Option "Einfügen aktivieren" das Smarttag; ist jedoch keine Option dieser Art vorhanden, da das "ObjectDataSource" `Insert()` Methode ist nicht zugeordnet, auf eine Methode in der `ProductsBLL` Klasse (Beachten Sie, dass wir diese Zuordnung (keine) festgelegt, wenn die Konfiguration der Datenquelle finden Sie unter Abbildung 3).

Um dem ObjectDataSource-Steuerelement konfigurieren zu können, wählen Sie die Datenquelle konfigurieren-Verknüpfung von sein Smarttag, den Assistenten zu starten. Im erste Bildschirm können Sie das zugrunde liegende Objekt zu ändern, an dem ObjectDataSource-Steuerelement gebunden ist; Lassen sie die Option `ProductsBLL`. Der nächste Bildschirm enthält die Zuordnungen von Methoden mit dem ObjectDataSource-Steuerelement auf die zugrunde liegende Objekt. Obwohl wir explizit, die angegeben die `Insert()` und `Delete()` Methoden sollten nicht auf alle Methoden zugeordnet werden, wenn man den INSERT- und DELETE-Registerkarten sehen Sie, dass eine Zuordnung vorhanden ist. Grund hierfür ist die `ProductsBLL`des `AddProduct` und `DeleteProduct` Methoden verwenden die `DataObjectMethodAttribute` Attribut, um anzugeben, dass sie die Standardmethoden für sind `Insert()` und `Delete()`bzw. Daher wählt der Assistent "ObjectDataSource" diese jedes Mal, wenn Sie den Assistenten ausführen, es sei denn, es einen anderen Wert explizit angegeben gibt.

Lassen Sie die `Insert()` Methode, die auf die `AddProduct` -Methode, aber erneut die löschen-Registerkarte Dropdown-Liste auf (keine) festgelegt.


[![Legen Sie die Registerkarte "Einfügen" des Dropdown-Liste auf die AddProduct-Methode](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Abbildung 14**: Legen Sie die Registerkarte "Einfügen" des Dropdown-Listenfeld auf der `AddProduct` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Legen Sie die DELETE-Registerkarte Dropdown-Liste auf (keine)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Abbildung 15**: die löschen-Registerkarte Dropdown-Liste (keine) festgelegt ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Nach diesen Änderungen durchführen, deklarative Syntax des dem ObjectDataSource-Steuerelement wird um erweitert werden ein `InsertParameters` -Auflistung, wie unten dargestellt:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Erneutes Ausführen des Assistenten, dem erneuten Hinzufügen der der `OldValuesParameterFormatString` Eigenschaft. Können Sie diese Eigenschaft zu löschen, indem Sie sie auf den Standardwert festlegen (`{0}`) oder entfernen Sie sie vollständig aus der deklarativen Syntax.

Mit dem ObjectDataSource-Steuerelement einfügen zur Verfügung stehen enthält DetailsViews-Smarttag jetzt das Kontrollkästchen einfügen aktivieren; Kehren Sie zum Designer zurück, und aktivieren Sie diese Option aus. Als Nächstes DetailsView kürzen, sodass sie nur zwei BoundFields - `ProductName` und `UnitPrice` - und die CommandField. An diesem Punkt sollte die deklarative Syntax des DetailsView wie aussehen:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

Abbildung 16 zeigt diese Seite, wenn Sie über einen Browser an diesem Punkt angezeigt. Wie Sie sehen können, führt DetailsView, den Namen und den Preis für das erste Produkt (Chai entspricht). Wir möchten, ist jedoch eine einfügen-Schnittstelle, die eine Möglichkeit für den Benutzer der Datenbank schnell ein neues Produkt hinzufügen.


[![Die DetailsView ist zurzeit im schreibgeschützten Modus gerendert.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Abbildung 16**: das DetailsView ist zurzeit im schreibgeschützten Modus gerendert ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Um DetailsView in der Einfügemodus anzuzeigen, wir legen müssen, die `DefaultMode` Eigenschaft `Inserting`. Dies DetailsView im Einfügemodus befindet, bei der ersten Anzeige gerendert und anschließend nach dem Einfügen eines neuen Datensatzes verfolgt. Wie in Abbildung 17 zeigt, bietet eine solche DetailsView eine schnelle Schnittstelle für einen neuen Datensatz hinzufügen.


[![Die DetailsView stellt eine Schnittstelle zum schnellen Hinzufügen ein neues Produkt bereit.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Abbildung 17**: DetailsView stellt eine Schnittstelle zum schnellen Hinzufügen eines neuen Produkts ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Wenn der Benutzer einen Produktnamen und Preis (z. B. "Acme Kate" und 1,99, wie in Abbildung 17 gibt) und Insert klickt, erfolgt ein Postback und Einfügen von Workflows beginnt, einen neuen Produktdatensatz in der Datenbank hinzugefügt wird verbessert. Die DetailsView verwaltet die einfügen-Schnittstelle und die GridView wird automatisch erneut gebunden mit der Datenquelle um das neue Produkt enthalten, wie in Abbildung 18 dargestellt.


![Das Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Abbildung 18**: das Produkt "Acme Kate" in der Datenbank hinzugefügt wurde


Während die GridView in Abbildung 18, die Produkt-Felder fehlen, für die von der DetailsView-Schnittstelle nicht angezeigt wird `CategoryID`, `SupplierID`, `QuantityPerUnit`usw. zugewiesen `NULL` Datenbank Werte. Sie können dies sehen, indem Sie die folgenden Schritte ausführen:

1. Wechseln Sie zu dem Server-Explorer in Visual Studio
2. Erweitern der `NORTHWND.MDF` Datenbankknoten
3. Mit der rechten Maustaste auf die `Products` Knoten der Datenbank-Tabelle
4. Wählen Sie die Tabellendaten anzeigen

Dadurch werden aufgelistet, alle Datensätze in der `Products` Tabelle. Wie in Abbildung 19 gezeigt, alle Spalten für unser neues Produkt außer `ProductID`, `ProductName`, und `UnitPrice` haben `NULL` Werte.


[![Das Produkt Felder nicht in bereitgestellten DetailsView werden NULL-Werte zugewiesen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Abbildung 19**: das Produkt Felder nicht in bereitgestellten DetailsView zugewiesen `NULL` Werte ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Wir möchten einen Standardwert angeben, außer `NULL` für eine oder mehrere der folgenden Spalte fest, entweder weil `NULL` ist nicht die beste Standardoption oder da die Datenbankspalte selbst nicht zugelassen wird `NULL` s. Um dies zu erreichen wir die Werte der Parameter für die DetailsView programmgesteuert festlegen können `InputParameters` Auflistung. Diese Zuweisung erfolgt entweder im Handler für die DetailsView `ItemInserting` Ereignis oder dem ObjectDataSource-Steuerelement `Inserting` Ereignis. Da wir bereits angesehen haben die Ereignisse vor und nach der Ebene auf die Web-Daten mit steuern Sie, sehen Sie wir uns mit dem ObjectDataSource-Steuerelement-Ereignisse dieses Mal.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Schritt 4: Zuweisen von Werten zu den`CategoryID`und`SupplierID`Parameter

Für dieses Tutorial nehmen wir, dass für die Anwendung, die beim Hinzufügen eines neuen Produkts über diese Schnittstelle die zugewiesen werden soll eine `CategoryID` und `SupplierID` Wert 1. Wie bereits erwähnt, verfügt über dem ObjectDataSource-Steuerelement ein Paar von vorab und nachträglich auf Ereignissen, Feuer während des Änderungsvorgangs Daten. Bei der `Insert()` Methode aufgerufen wird, löst Sie zunächst dem ObjectDataSource-Steuerelement seine `Inserting` Ereignis ruft dann die Methode, die die `Insert()` -Methode zugeordnet wurde, und schließlich löst die `Inserted` Ereignis. Die `Inserting` -Ereignishandler bietet uns eine letzte Gelegenheit, um die Eingabeparameter optimieren oder 30-tägiges abzubrechen.

> [!NOTE]
> In einer echten Anwendung möchten Sie wahrscheinlich jedoch, ob Sie mit der Benutzer geben die Kategorie und Lieferanten, oder wählen Sie diesen Wert würden für diese Grundlage einiger Kriterien oder Business Logic (und nicht blind Auswählen einer ID von 1). Das Beispiel veranschaulicht unabhängig davon, wie den Wert eines Eingabeparameters programmgesteuert aus vorab auf dem ObjectDataSource-Steuerelement-Ereignis festgelegt.


Erstellen Sie einen Ereignishandler für das ObjectDataSource-Steuerelement in Ruhe `Inserting` Ereignis. Beachten Sie, dass der zweite Eingabeparameter für den Ereignishandler ein Objekt des Typs ist `ObjectDataSourceMethodEventArgs`, das hat es sich um eine Eigenschaft auf die Parameters-Auflistung (`InputParameters`) und eine Eigenschaft, um den Vorgang abzubrechen (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

An diesem Punkt die `InputParameters` Eigenschaft enthält die "ObjectDataSource" `InsertParameters` Auflistung mit den Werten aus der DetailsView zugewiesen. Um den Wert von einer der folgenden Parameter zu ändern, verwenden Sie einfach: `e.InputParameters["paramName"] = value`. Aus diesem Grund Festlegen der `CategoryID` und `SupplierID` anpassen, um die Werte von 1, die `Inserting` Ereignishandler wie folgt aussehen:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Diese Zeit beim Hinzufügen eines neuen Produkts (z. B. Acme Soda), die `CategoryID` und `SupplierID` für Spalten des neuen Produkts werden auf 1 festgelegt (siehe Abbildung 20).


[![Neue Produkte können die CategoryID und SupplierID Werte von Reihe 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Abbildung 20**: neue Produkte jetzt haben deren `CategoryID` und `SupplierID` Werte auf 1 festgelegt ([klicken Sie, um das Bild in voller Größe anzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Während der bearbeiten, einfügen und Löschen von Prozess fortgesetzt werden sowohl das Datensteuerelement für das Web als auch dem ObjectDataSource-Steuerelement durch eine Reihe von vorab und nachträglich auf Ereignisse. In diesem Tutorial haben wir untersucht, die bereits auf Ereignisse und gesehen, wie verwenden Sie diese zum Anpassen der Eingabeparameter oder zum Abbrechen der datenänderungsvorgang vollständig sowohl aus dem Datensteuerelement für das Web und ObjectDataSource-Ereignissen. Betrachten wir im nächsten Tutorial erstellen und Verwenden von Ereignishandlern für die Ereignisse nach der Ebene.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Jackie Goor und Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Weiter](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
