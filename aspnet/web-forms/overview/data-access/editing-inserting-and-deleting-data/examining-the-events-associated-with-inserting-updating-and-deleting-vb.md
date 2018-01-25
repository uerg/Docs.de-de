---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: "Überprüfen Sie die Ereignisse zugeordneten einfügen, aktualisieren und löschen (VB) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm untersuchenden unter Verwendung der Ereignisse, die vor, während und nach einer Einfügung auftreten, aktualisieren oder Löschvorgang eines Daten-Websteuerelement von ASP.NET. W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 88f6beb3f3514c6a9784d4cb936a5b779ce75ae1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Untersuchen die Ereignisse im Zusammenhang mit einfügen, aktualisieren und löschen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) oder [PDF herunterladen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> In diesem Lernprogramm untersuchenden unter Verwendung der Ereignisse, die vor, während und nach einer Einfügung auftreten, aktualisieren oder Löschvorgang eines Daten-Websteuerelement von ASP.NET. Wir sehen auch zum Anpassen der Bearbeitung-Schnittstelle, um nur eine Teilmenge der Felder Product aktualisieren.


## <a name="introduction"></a>Einführung

Wenn Sie die integrierte einfügen, bearbeiten oder Löschen von Features der GridView, DetailsView oder FormView-Steuerelemente verwenden, eine Vielzahl von Schritten ablaufen müssen nach Abschluss des Vorgangs einen neuen Datensatz hinzufügen oder aktualisieren oder Löschen eines vorhandenen Datensatzes durch den Endbenutzer. Wie in erläutert die [vorherigen Lernprogramm](an-overview-of-inserting-updating-and-deleting-data-vb.md), wenn eine Zeile in der GridView die Schaltfläche "Bearbeiten bearbeitet wird" wird durch die Schaltflächen "Abbrechen" und "Update" und aktivieren Sie die BoundFields in Textfelder ein ersetzt. Nachdem der Endbenutzer die Daten aktualisiert und Update klickt, werden die folgenden Schritte beim Postback ausgeführt:

1. Die GridView füllt die ObjectDataSource `UpdateParameters` mit eindeutigen identifizierenden Felder für den geänderten Datensatz (über die `DataKeyNames` Eigenschaft) zusammen mit den vom Benutzer eingegebenen Werte
2. Die GridView ruft seine ObjectDataSource `Update()` -Methode, die wiederum die entsprechende Methode des zugrunde liegenden Objekts aufruft (`ProductsDAL.UpdateProduct`, in unserem vorherigen Lernprogramm)
3. Die zugrunde liegenden Daten, die jetzt die aktualisierte Änderungen enthält, werden erneut an die GridView gebunden.

Während dieser Sequenz von Schritten, eine Anzahl von Ereignissen ausgelöst werden, aktivieren uns, erstellen Sie Ereignishandler, um benutzerdefinierte Logik hinzufügen, falls erforderlich. Z. B. vor Schritt 1, die GridView `RowUpdating` Ereignis ausgelöst wird. Wir können an diesem Punkt die Update-Anforderung abbrechen, wenn einige Überprüfungsfehler aufgetreten ist. Wenn die `Update()` Methode wird aufgerufen, das ObjectDataSource `Updating` Ereignis wird ausgelöst, einnahmemöglichkeiten hinzufügen, oder passen Sie die Werte aller der `UpdateParameters`. Nachdem das ObjectDataSource zugrunde liegende-Methode des Objekts wurde abgeschlossen ausführen, das ObjectDataSource `Updated` Ereignis wird ausgelöst. Ein Ereignishandler für das `Updated` Ereignis Überprüfen der Details der Update-Vorgang, z. B. wie viele Zeilen betroffen sind, und davon, ob eine Ausnahme aufgetreten ist. Zum Schluss nach Schritt2, die GridView `RowUpdated` Ereignis wird ausgelöst, einen Ereignishandler für dieses Ereignis kann untersuchen Sie zusätzliche Informationen zu den Updatevorgang nur ausgeführt.

Abbildung 1 zeigt diese Reihe von Ereignissen und Schritte beim Aktualisieren einer GridView. Die-Ereignismuster in Abbildung 1 ist nicht eindeutig mit einer GridView aktualisieren. Einfügen, aktualisieren oder Löschen von Daten aus der GridView, precipitates DetailsView oder FormView dieselbe Sequenz von vorab und nachträglich auf Ereignisse für das Web-Steuerelement und das ObjectDataSource.


[![Eine Reihe von vor und nach der Ereignisse auslösen, beim Aktualisieren von Daten in einem GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Abbildung 1**: eine Reihe von vor und nach der Ereignisse auslösen beim Aktualisieren von Daten in eine GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


In diesem Lernprogramm untersuchenden erweitern die integrierte einfügen mithilfe von diesen Ereignissen steuert aktualisieren und löschen die Funktionen des ausgewählten ASP.NET Web. Wir sehen auch zum Anpassen der Bearbeitung-Schnittstelle, um nur eine Teilmenge der Felder Product aktualisieren.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Schritt 1: Aktualisieren eines Produkts`ProductName`und`UnitPrice`Felder

In den Bearbeitung Schnittstellen aus dem vorherigen Lernprogramm *alle* Produkt-Felder, die nicht ohne Schreibzugriff waren hatte eingeschlossen werden sollen. Angenommen, wäre es ein Feld aus der GridView - entfernen `QuantityPerUnit` – beim Aktualisieren der Daten des Websteuerelements Daten nicht das ObjectDataSource festgelegt würden `QuantityPerUnit` `UpdateParameters` Wert. Das ObjectDataSource würde dann übergeben den Wert der `Nothing` in der `UpdateProduct` (Business Logic Layer, BLL)-Methode, die der bearbeiteten Datenbankeintrag geändert würde `QuantityPerUnit` Spalte eine `NULL` Wert. Auf ähnliche Weise, wenn ein erforderliches Feld, z. B. `ProductName`, entfernt wird von der Bearbeitung-Schnittstelle, das Update erzeugt einen Fehler mit einer "*Spaltenname 'ProductName' lässt keine NULL-Werte*" Ausnahme. Der Grund für dieses Verhalten wurde, da das ObjectDataSource konfiguriert wurde, rufen Sie auf der `ProductsBLL` Klasse `UpdateProduct` -Methode, die Eingabeparameter für jedes der Felder Product erwartet. Aus diesem Grund die ObjectDataSources `UpdateParameters` enthält einen Parameter für jede der Methode Eingabeparameter des.

Wenn ein Websteuerelement-Daten zu schaffen, die Endbenutzer nur eine Teilmenge der Felder aktualisieren erlaubt, werden soll, müssen wir entweder programmgesteuert die fehlende festgelegt `UpdateParameters` Werte in der ObjectDataSource `Updating` Ereignishandler oder erstellen und Aufrufen einer Methode BLL, erwartet, dass nur eine Teilmenge der Felder. Lassen Sie uns diese zweite Ansatz.

Insbesondere erstellen wir eine Seite, die zeigt nur die `ProductName` und `UnitPrice` Felder in einem bearbeitbaren GridView. Bearbeitung dieser GridView-Schnittstelle ermöglicht dem Benutzer, die zwei angezeigten Felder aktualisieren nur `ProductName` und `UnitPrice`. Da diese Bearbeitung Schnittstelle nur eine Teilmenge von Feldern für ein Produkt bereitstellt, wir entweder ein ObjectDataSource erstellen müssen, die der vorhandenen BLL verwendet `UpdateProduct` Methode und die fehlenden Werte der Product-Feld festgelegt programmgesteuert in ihre `Updating` Ereignis Handler, oder es müssen zum Erstellen einer neuen BLL-Methode, die nur die Teilmenge von Feldern, definiert in der GridView erwartet. Für dieses Lernprogramm nehmen wir verwenden Sie letztere Option und erstellen Sie eine Überladung der `UpdateProduct` -Methode, ein, die in nur drei Parameter akzeptiert: `productName`, `unitPrice`, und `productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Wie die ursprüngliche `UpdateProduct` Methode, diese Überladung wird gestartet, indem überprüft wird, um festzustellen, ob ein Produkt in der Datenbank mit dem angegebenen `ProductID`. Wenn nicht der Fall, es gibt `False`, der angibt, die Fehler bei der Anforderung, die Produktinformationen zu aktualisieren. Andernfalls aktualisiert die vorhandenen Produktdatensatz `ProductName` und `UnitPrice` entsprechend Felder und führt einen Commit für die Aktualisierung durch Aufrufen der TableAdpater `Update()` -Methode auf und übergibt die `ProductsRow` Instanz.

Mit diesem zusätzlich zu unserem `ProductsBLL` -Klasse, wir jetzt können Sie die vereinfachte GridView-Schnittstelle zu erstellen. Öffnen der `DataModificationEvents.aspx` in den `EditInsertDelete` Ordner und die Seite eine GridView hinzuzufügen. Eine neue ObjectDataSource erstellen und konfigurieren sie mithilfe der `ProductsBLL` -Klasse mit seiner `Select()` Mapping-Methode, um `GetProducts` und seine `Update()` Mapping-Methode, um die `UpdateProduct` Überladung, die nur in der `productName`, `unitPrice`, und `productID` Eingabeparameter. Abbildung 2 zeigt der Assistent zum Erstellen der Datenquelle aus, bei der Zuordnung der ObjectDataSource `Update()` Methode, um die `ProductsBLL` Klasse des neuen `UpdateProduct` -methodenüberladung.


[![Map-Methode für das ObjectDataSource-Update() an die neue UpdateProduct Überladung](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Abbildung 2**: Zuordnen der ObjectDataSource `Update()` Methode, um das neue `UpdateProduct` überladen ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


Da unser Beispiel zunächst nur die Möglichkeit zum Bearbeiten von Daten, aber nicht das Einfügen oder Löschen von Datensätzen benötigt, nehmen einen Moment Zeit, um explizit anzugeben, dass das ObjectDataSource `Insert()` und `Delete()` Methoden sollten nicht zugeordnet werden, an ein beliebiges der `ProductsBLL` Klasse-Methoden, indem Sie die Registerkarten INSERT- und DELETE aufrufen und (keine) aus der Dropdown-Liste auswählen.


[![Wählen Sie (keine) aus der Dropdown Liste für die INSERT- und DELETE-Registerkarten](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie (keine) aus der Dropdown-Liste für die einfügen und Löschen von Registerkarten ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


Nach Abschluss dieses Assistenten das Kontrollkästchen Sie Bearbeiten aktivieren aus der GridView Smarttag.

Mit dem Abschluss des Assistenten Datenquelle erstellen und binden, die an die GridView hat Visual Studio die deklarative Syntax für beide Steuerelemente erstellt. Wechseln Sie zur Quellansicht zu das ObjectDataSource deklarative Markup, überprüfen Sie die unten angezeigt wird:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Da es sich um keine Zuordnungen für das ObjectDataSource `Insert()` und `Delete()` Methoden, es gibt keine `InsertParameters` oder `DeleteParameters` Abschnitte. Darüber hinaus seit der `Update()` -Methode zugeordnet ist die `UpdateProduct` methodenüberladung, die drei Eingabeparameter nur akzeptiert den `UpdateParameters` Abschnitt verfügt über nur drei `Parameter` Instanzen.

Beachten Sie, dass das ObjectDataSource `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`. Diese Eigenschaft ist mit dem Konfigurieren von Datenquellen-Assistenten von Visual Studio festgelegt. Aber da unsere BLL Methoden erwarten, dass die ursprüngliche nicht `ProductID` Wert übergeben werden, vollständig entfernen dieser Eigenschaft die Zuweisung über das ObjectDataSource-Deklarationssyntax.

> [!NOTE]
> Wenn Sie einfach löschen, die `OldValuesParameterFormatString` Eigenschaftswert im Eigenschaftenfenster in der Entwurfsansicht die Eigenschaft auch weiterhin in der deklarativen Syntax vorhanden, aber auf eine leere Zeichenfolge festgelegt werden. Entfernen Sie entweder die Eigenschaft vollständig vom deklarative Syntax oder über das Eigenschaftenfenster, legen Sie den Wert auf den Standardwert `{0}`.


Während der ObjectDataSource nur `UpdateParameters` für die Product Name, Preis und ID, Visual Studio verfügt über zusätzliche eine BoundField- oder CheckBoxField in die GridView für jedes der Felder für das Produkt.


[![Die GridView enthält für jedes der Felder für das Produkt ein BoundField- oder CheckBoxField](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Abbildung 4**: GridView enthält für jedes der Felder für das Produkt ein BoundField- oder CheckBoxField ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


Wenn der Endbenutzer ein Produkt bearbeitet und klickt auf die Schaltfläche "Aktualisieren", listet die GridView diese Felder, die nicht ohne Schreibzugriff waren. Dann wird der Wert des entsprechenden Parameters in der ObjectDataSource `UpdateParameters` -Auflistung, um den Wert, der vom Benutzer eingegeben werden. Wenn keine entsprechenden Parameter vorhanden ist, fügt die GridView der Auflistung hinzu. Aus diesem Grund enthält unsere GridView BoundFields und CheckBoxFields für alle Felder für das Produkt, das ObjectDataSource letztlich Aufrufen der `UpdateProduct` Überladung, die an alle diese Parameter, trotz der Tatsache annimmt, die das ObjectDataSource deklaratives Markup gibt nur drei Eingabeparameter (siehe Abbildung 5). Auf ähnliche Weise, wenn aus eine Kombination von nicht schreibgeschützte besteht Produkt Felder in der GridView, die die Eingabeparameter für entsprechen keine `UpdateProduct` überladen, eine Ausnahme ausgelöst, wenn Sie versuchen zu aktualisieren.


[![Die GridView wird das ObjectDataSource UpdateParameters Auflistung Parameter hinzufügen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Abbildung 5**: die GridView wird Hinzufügen von Parametern zu der ObjectDataSource `UpdateParameters` Auflistung ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


Um sicherzustellen, dass das ObjectDataSource ruft die `UpdateProduct` Überladung, die in nur des Produkts, Preis, und ID, akzeptiert müssen wir die GridView zu beschränken, mit dem Fehlen eines editierbaren Felder für lediglich das `ProductName` und `UnitPrice`. Dies geschieht durch das Entfernen von anderen BoundFields und CheckBoxFields, durch Festlegen von den anderen Feldern `ReadOnly` Eigenschaft `True`, oder durch eine Kombination aus beidem. Für dieses Lernprogramm nehmen wir entfernen Sie einfach alle GridView Felder mit Ausnahme der `ProductName` und `UnitPrice` BoundFields, wonach die GridView deklarative Markup wird wie folgt aussehen:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Obwohl die `UpdateProduct` Überladung erwartet drei Eingabeparameter, wir nur zwei BoundFields in unserer GridView aufweisen. Grund hierfür ist die `productID` Eingabeparameter wird ein Primärschlüsselwert und übergeben den Wert des der `DataKeyNames` -Eigenschaft für der bearbeiteten Zeile.

Unsere GridView zusammen mit den `UpdateProduct` überladen, können Benutzer nur der Name und der Preis eines Produkts zu bearbeiten, ohne die anderen Felder des Produkts verloren.


[![Die Schnittstelle ermöglicht es, nur der Product Name und der Preis bearbeiten](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Abbildung 6**: die Schnittstelle ermöglicht es, die nur der Product Name und der Preis bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> Wie im vorherigen Lernprogramm erläutert wird, ist es äußerst wichtig, dass die GridView Ansichtszustand s werden (das Standardverhalten) aktiviert. Wenn Sie festlegen, dass die GridView s `EnableViewState` Eigenschaft `false`, laufen Sie Gefahr, dass gleichzeitige Benutzer versehentlich löschen oder Bearbeiten von Datensätzen. Finden Sie unter [Warnung: Parallelität Problem mit ASP.NET 2.0 GridViews/DetailsView/FormViews, Bearbeitung unterstützen und/oder löschen und, deren Ansichtszustand deaktiviert](http://scottonwriting.net/sowblog/posts/10054.aspx) für Weitere Informationen.


## <a name="improving-theunitpriceformatting"></a>Verbessern der`UnitPrice`Formatierung

GridView dargestellten Beispiel in Abbildung 6 arbeitet, während die `UnitPrice` Feld überhaupt nicht formatiert wird, einen Preis anzeigen, die alle Währung verfügt nicht über Symbole und verfügt über vier Dezimalstellen. Um eine Währung Formatierung für die Zeilen nicht bearbeitbaren zu übernehmen, legen Sie einfach die `UnitPrice` BoundFields `DataFormatString` Eigenschaft, um `{0:c}` und dessen `HtmlEncode` Eigenschaft `False`.


[![Festlegen Sie die UnitPrice DataFormatString und HtmlEncode Eigenschaften entsprechend](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Abbildung 7**: Legen Sie die `UnitPrice`des `DataFormatString` und `HtmlEncode` Eigenschaften entsprechend ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


Durch diese Änderung-Format nicht bearbeitbare Zeilen als Währung den Preis die bearbeitete Zeile zeigt jedoch weiterhin den Wert ohne das Währungssymbol und vier Dezimalstellen.


[![Die nicht bearbeitet werden Zeilen sind als Währungswerte jetzt formatiert](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Abbildung 8**: die nicht bearbeitbare Zeilen sind jetzt als Währungswerte formatiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


Die Formatierung Anweisungen, die der `DataFormatString` Eigenschaft kann auf die Bearbeitung Schnittstelle angewendet werden, durch Festlegen der BoundField `ApplyFormatInEditMode` Eigenschaft `True` (die Standardeinstellung ist `False`). Können Sie diese Eigenschaft festgelegt wird, um `True`.


[![Legen Sie die UnitPrice BoundField ApplyFormatInEditMode-Eigenschaft auf "true"](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Abbildung 9**: Festlegen der `UnitPrice` BoundFields `ApplyFormatInEditMode` Eigenschaft `True` ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


Mit dieser Änderung wird der Wert, der die `UnitPrice` angezeigt, in der bearbeiteten Zeile wird auch als Währung formatiert.


[![Die bearbeitete Zeilenwert UnitPrice ist jetzt formatiert als Währung](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Abbildung 10**: die Bearbeitung der Zeile `UnitPrice` Wert ist jetzt als Währung formatiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


Ein Produkt jedoch mit dem Währungssymbol in das Textfeld zu aktualisieren, wie z. B. $19.00 löst eine `FormatException`. Wenn GridView versucht, die vom Benutzer angegebenen Werte die ObjectDataSource zuweisen `UpdateParameters` Sammlung wird, kann nicht konvertiert die `UnitPrice` -Zeichenfolge "$19.00" in der `Decimal` durch den Parameter erforderlich (siehe Abbildung 11). Zur Umgehung des Problems erstellen wir einen Ereignishandler für der GridView `RowUpdating` Ereignis und analysiert die vom Benutzer bereitgestellte `UnitPrice` als eine Währung formatiert `Decimal`.

Der GridView `RowUpdating` Ereignis akzeptiert als zweiten Parameter ein Objekt des Typs [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), darunter eine `NewValues` Wörterbuch als eine seiner Eigenschaften, die die bereit für die benutzerdefinierte Werte enthält. das ObjectDataSource zugewiesene `UpdateParameters` Auflistung. Wir können überschreibt das vorhandene `UnitPrice` Wert in der `NewValues` Auflistung mit einem Dezimalwert analysiert die folgenden Codezeilen in das Währungsformat mit der `RowUpdating` Ereignishandler:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Wenn der Benutzer angegeben hat eine `UnitPrice` Wert (z. B. "$19.00"), wird dieser Wert mit dem dezimalen Wert berechnet, indem überschrieben [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analysieren den Wert als Währung. Dies Dezimaltrennzeichen ordnungsgemäß im Falle alle Währungssymbole, Kommas, Dezimaltrennzeichen und So weiter analysiert und verwendet die [NumberStyles Enumeration](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) in der [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) Namespace.

Abbildung 11 zeigt sowohl das Problem durch Währungssymbole in den vom Benutzer bereitgestellten verursacht `UnitPrice`, zusammen mit wie der GridView `RowUpdating` Ereignishandler kann verwendet werden, um diese Eingabe wird ordnungsgemäß analysiert.


[![Die bearbeitete Zeilenwert UnitPrice ist jetzt formatiert als Währung](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Abbildung 11**: die Bearbeitung der Zeile `UnitPrice` Wert ist jetzt als Währung formatiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Schritt 2: Verbot`NULL UnitPrices`

Während die Datenbank konfiguriert ist, um zuzulassen `NULL` Werte in der `Products` tabellenspezifischen `UnitPrice` Spalte sollten wir die verhindern, dass Benutzer, die Zugriff auf diese Seite von der Angabe einer `NULL` `UnitPrice` Wert. D. h., wenn ein Benutzer zur Eingabe einer `UnitPrice` Wert, wenn eine Produktzeile zu bearbeiten, anstatt Sie zu speichern der Ergebnisse in der Datenbank, die eine Meldung, die den Benutzer darüber informiert, die über diese Seite alle bearbeiteten Produkte aus, die einen Preis angegeben benötigen angezeigt werden soll.

Die `GridViewUpdateEventArgs` Objekt an der GridView übergeben `RowUpdating` Ereignishandler enthält eine `Cancel` Eigenschaft, wenn auf festgelegt `True`, beendet den Prozess aktualisieren. Erweitern wir die `RowUpdating` Ereignishandler festlegen `e.Cancel` auf `True` und zeigt folgende Meldung Erklärung, wenn die `UnitPrice` Wert in der `NewValues` Auflistung hat den Wert `Nothing`.

Starten, indem Sie die Seite mit dem Namen einer Bezeichnung Websteuerelement hinzugefügt `MustProvideUnitPriceMessage`. Diese Label-Steuerelement wird angezeigt, wenn der Benutzer nicht an einen `UnitPrice` Wert, der beim Aktualisieren eines Produkts. Legen Sie die Bezeichnung `Text` -Eigenschaft auf "müssen Sie einen Preis für das Produkt bereitstellen." Ich erstellt habe auch eine neue CSS-Klasse in `Styles.css` mit dem Namen `Warning` mit folgender Definition:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Legen Sie schließlich der Bezeichnung `CssClass` Eigenschaft `Warning`. An diesem Punkt sollte der Designer zeigt, wie Abbildung 12 die Warnmeldung in einem roten, fett, kursiv, Extragroß Schriftgrad oben GridView.


[![Eine Bezeichnung wurde über die GridView hinzugefügt.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Abbildung 12**: eine Bezeichnung wurde wurde hinzugefügt oberhalb der GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


Standardmäßig diese Bezeichnung ausgeblendet werden sollen, legen Sie daher dessen `Visible` Eigenschaft `False` in die `Page_Load` Ereignishandler:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Wenn der Benutzer versucht, ein Produkt zu aktualisieren, ohne die `UnitPrice`, möchten wir das Update Abbrechen und die Warnung Bezeichnung angezeigt. Erweitern Sie der GridView `RowUpdating` Ereignishandler wie folgt:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Wenn ein Benutzer versucht, ohne einen Preis ein Produkts zu speichern, das Update wird abgebrochen und eine hilfreiche Meldung angezeigt. Während die Datenbank (und Geschäftslogik) ermöglicht `NULL` `UnitPrice` s, diese bestimmten ASP.NET-Seite hingegen nicht.


[![Ein Benutzer kann nicht UnitPrice leer lassen.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Abbildung 13**: ein Benutzer kann nicht verlassen `UnitPrice` leer ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


Bisher haben wir gesehen, wie Sie der GridView `RowUpdating` Ereignis, um die Parameterwerte der ObjectDataSource zugewiesene programmgesteuert alter `UpdateParameters` Sammlung wie auf "Abbrechen" die Aktualisierung vollständig verarbeiten. Diese Konzepte übertragen, DetailsView und FormView-Steuerelemente und gelten auch für das Einfügen und löschen.

Diese Aufgaben können auch über die Ereignishandler für die Ebene der ObjectDataSource erfolgen dessen `Inserting`, `Updating`, und `Deleting` Ereignisse. Diese Ereignisse ausgelöst werden, bevor die zugeordnete Methode des zugrunde liegenden Objekts aufgerufen wird, und geben Sie eine Gelegenheit letzte Chance, auf die Auflistung der Eingabeparameter zu ändern, oder brechen Sie den Vorgang sofort. Die Ereignishandler für diese drei Ereignisse sind ein Objekt des Typs übergeben [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , das über zwei wichtige Eigenschaften verfügt:

- ["Abbrechen"](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), die, wenn auf festgelegt `True`, bricht den Vorgang ausgeführt wird
- [Eingabeparameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), also in die Auflistung der `InsertParameters`, `UpdateParameters`, oder `DeleteParameters`, abhängig davon, ob der Ereignishandler wird der `Inserting`, `Updating`, oder `Deleting` Ereignis

Zum Arbeiten mit den Parameterwerten auf der Ebene ObjectDataSource zu veranschaulichen, enthalten Sie eine DetailsView wir auf unserer Seite, die der Benutzer ein neues Produkt hinzufügen können. Diese DetailsView wird verwendet werden, um eine Schnittstelle für das schnelle Hinzufügen von einem neuen Produkt mit der Datenbank bereitzustellen. Um eine einheitliche Benutzeroberfläche zu behalten, wenn Sie ein neues Produkt wir hinzufügen können Benutzer nur Geben Sie Werte für die `ProductName` und `UnitPrice` Felder. Standardmäßig werden die Werte, die in der DetailsView einfügen-Schnittstelle angegeben werden nicht auf festgelegt werden eine `NULL` Datenbank-Wert. Wir können jedoch das ObjectDataSource `Inserting` Ereignis, um unterschiedliche Standardwerte einzufügen, wie es in Kürze sehen.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Schritt 3: Stellt eine Schnittstelle zum Hinzufügen neuer Produkte bereit.

Ziehen Sie eine DetailsView aus der Toolbox auf den Designer über die GridView löschen seine `Height` und `Width` Eigenschaften und binden es das ObjectDataSource auf der Seite "bereits vorhanden. Dadurch wird eine BoundField- oder CheckBoxField für jedes der Felder für das Produkt hinzugefügt. Da wir diese DetailsView zu verwenden, um neue Produkte hinzufügen möchten, müssen wir die Option einfügen aktivieren aus dem Smarttag aktivieren. allerdings keine solche Option vorhanden ist, da das ObjectDataSource `Insert()` -Methode nicht zugeordnet ist, an eine Methode in der `ProductsBLL` Klasse (Beachten Sie, dass wir diese Zuordnung (keine), setzen Wenn die Datenquelle konfiguriert, siehe Abbildung 3).

Um das ObjectDataSource zu konfigurieren, wählen Sie die Datenquelle konfigurieren-Link aus seiner Smarttag, und Starten des Assistenten aus. Der erste Bildschirm können Sie das zugrunde liegende Objekt zu ändern, dem das ObjectDataSource gebunden ist; Lassen Sie festgelegt ist, dass `ProductsBLL`. Im nächste Bildschirm sind die Zuordnungen von das ObjectDataSource-Methoden auf die zugrunde liegende Objekt aufgelistet. Obwohl es explizit, die angegeben die `Insert()` und `Delete()` Methoden sollten nicht auf alle Methoden zugeordnet werden, wenn Sie den INSERT- und DELETE-Registerkarten fortfahren sehen Sie, dass eine Zuordnung vorhanden ist. Grund hierfür ist die `ProductsBLL`des `AddProduct` und `DeleteProduct` Methoden verwenden, die `DataObjectMethodAttribute` Attribut, um anzugeben, dass sie die Standardmethoden für sind `Insert()` und `Delete()`bzw. Daher wählt das ObjectDataSource-Assistent diese jedes Mal, die Sie den Assistenten ausführen, es sei denn, es ein anderer Wert explizit angegeben wird.

Lassen Sie die `Insert()` Methode, die auf die `AddProduct` -Methode, aber die DELETE-Registerkarte Dropdown-Liste (None) nicht erneut festgelegt.


[![Legen Sie die Registerkarte "Einfügen" des Dropdown-Liste auf die AddProduct-Methode](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Abbildung 14**: Legen Sie auf der Registerkarte Einfügen des Dropdown-Liste der `AddProduct` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![Legen Sie die DELETE-Registerkarte Dropdown-Liste (keine)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Abbildung 15**: Festlegen der löschen Registerkarte Dropdown-Liste (None) ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


Nachdem diese Änderungen vorgenommen wurden, wird das ObjectDataSource-Deklarationssyntax erweitert werden und enthalten eine `InsertParameters` -Auflistung, wie unten dargestellt:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Erneutes Ausführen des Assistenten wieder hinzugefügt der `OldValuesParameterFormatString` Eigenschaft. Nehmen Sie einen Moment Zeit, um diese Eigenschaft zu löschen, indem Sie es auf den Standardwert festlegen (`{0}`) oder entfernen Sie sie vollständig aus der deklarativen Syntax.

Mit der ObjectDataSource Einfügefunktionen bereitstellen enthalten die DetailsView Smarttag nun das Kontrollkästchen aktivieren einfügen; Kehren Sie zum Designer zurück, und aktivieren Sie diese Option aus. Als Nächstes, DetailsView kürzen, damit, dass es nur zwei BoundFields - `ProductName` und `UnitPrice` - und die CommandField. An diesem Punkt sollte die DetailsView deklarative Syntax formuliert werden:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Abbildung 16 zeigt diese Seite, wenn Sie über einen Browser an diesem Punkt angezeigt. Wie Sie sehen können, DetailsView aufgeführt, den Namen und den Preis des ersten Produkts (Chai). Was soll, ist jedoch eine einfügen-Schnittstelle, die eine Möglichkeit für den Benutzer zum schnellen Hinzufügen ein neues Produkts zu der Datenbank enthält.


[![DetailsView ist zurzeit im schreibgeschützten Modus gerendert.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Abbildung 16**: The, DetailsView ist zurzeit im schreibgeschützten Modus gerendert ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


Um DetailsView in seiner Einfügemodus anzuzeigen, wir festlegen müssen, der `DefaultMode` Eigenschaft `Inserting`. Rendert die DetailsView im Einfügemodus beim ersten Mal besucht hat, und hält dort nach dem Einfügen eines neuen Datensatzes. Wie in Abbildung 17 gezeigt, bietet eine solche DetailsView eine schnelle Oberfläche zum Hinzufügen eines neuen Datensatzes.


[![DetailsView stellt eine Schnittstelle für ein neues Produkt schnell hinzufügen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Abbildung 17**: DetailsView stellt eine Schnittstelle für das schnelle Hinzufügen von einem neuen Produkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


Wenn der Benutzer einfügen klickt und einen Produktnamen und Preis (z. B. "Water Acme" und 1.99, wie in Abbildung 17 gibt), ein Postback erfolgt, und das Einfügen von Workflow beginnt mit einer neuen Produktdatensatz mit der Datenbank hinzugefügt wird. DetailsView verwaltet die einfügende-Schnittstelle und die GridView wird automatisch neu zu seiner Datenquelle um das neue Produkt enthalten wie in Abbildung 18 dargestellt.


![Das Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Abbildung 18**: das Produkt "Acme Wasser" in der Datenbank hinzugefügt wurde


Während die GridView in Abbildung 18, die Produkt-Felder fehlen von DetailsView-Schnittstelle nicht angezeigt `CategoryID`, `SupplierID`, `QuantityPerUnit`usw. zugewiesen `NULL` Datenbank-Werte. Sie können dies sehen, indem Sie die folgenden Schritte ausführen:

1. Wechseln Sie zu dem Server-Explorer in Visual Studio
2. Erweitern der `NORTHWND.MDF` Datenbankknoten
3. Mit der rechten Maustaste auf die `Products` Datenbankknoten-Tabelle
4. Wählen Sie die Tabellendaten anzeigen

Dies wird Auflisten aller Datensätze in der `Products` Tabelle. Wie in Abbildung 19 gezeigt, alle Spalten für unser neues Produkt außer `ProductID`, `ProductName`, und `UnitPrice` haben `NULL` Werte.


[![Das Produkt Felder nicht in bereitgestellten DetailsView werden NULL-Werte zugewiesen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Abbildung 19**: das Produkt Felder nicht in bereitgestellten DetailsView zugewiesen sind `NULL` Werte ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


Wir möchten einen Standardwert anzugeben, außer `NULL` für eine oder mehrere dieser Spaltenwerte, entweder weil `NULL` ist nicht die beste Option, oder weil die Datenbankspalte selbst nicht zugelassen wird `NULL` s. Um dies zu erreichen wir die Werte der Parameter für die DetailsView programmgesteuert festgelegt `InputParameters` Auflistung. Diese Zuordnung kann erfolgen entweder im Handler für das DetailsView `ItemInserting` Ereignis oder das ObjectDataSource `Inserting` Ereignis. Da wir bereits gesucht haben an die Ereignisse vor und nach der Ebene auf die Web-Daten mit steuern Sie Zugriffsebene, untersuchen Sie wir die Verwendung der ObjectDataSource-Ereignissen diesmal.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Schritt 4: Zuweisen von Werten zu den`CategoryID`und`SupplierID`Parameter

Für dieses Lernprogramm nehmen wir, dass für die Anwendung beim Hinzufügen eines neuen Produkts über diese Schnittstelle die zugewiesen werden soll eine `CategoryID` und `SupplierID` Wert 1. Wie bereits erwähnt, verfügt über das ObjectDataSource ein Paar von vorab und nachträglich auf Ereignissen, Feuer während des Änderungsvorgangs Daten. Wenn seine `Insert()` Methode aufgerufen wird, löst Sie zuerst das ObjectDataSource seine `Inserting` Ereignis ruft dann die Methode, die seine `Insert()` Methode zugeordnet wurde, und schließlich löst die `Inserted` Ereignis. Die `Inserting` Ereignishandler anwendungsübergreifend uns eine letzte Möglichkeit zum Optimieren der Eingabeparameter, oder brechen Sie den Vorgang sofort.

> [!NOTE]
> In einer realen Anwendung würde möchten Sie wahrscheinlich entweder mit der Benutzer, geben Sie die Kategorie und Lieferanten, oder möchten, wählen Sie diesen Wert für diesen basierend auf bestimmte Kriterien oder Business Logic (statt Blind Auswählen einer ID von 1). Veranschaulicht dieses Beispiel unabhängig davon, wie Sie den Wert eines Eingabeparameters aus der ObjectDataSource vorab Ebene Ereignis programmgesteuert festlegen.


So erstellen Sie einen Ereignishandler für das ObjectDataSource erkundet `Inserting` Ereignis. Beachten Sie, dass der zweite Eingabeparameter für den Ereignishandler ein Objekt des Typs ist `ObjectDataSourceMethodEventArgs`, dem verfügt über eine Eigenschaft zum Zugriff auf die Parameterauflistung (`InputParameters`) und eine Eigenschaft, die das Abbrechen des Vorgangs (`Cancel`).


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

An diesem Punkt der `InputParameters` Eigenschaft enthält die ObjectDataSource `InsertParameters` Auflistung mit den Werten, DetailsView zugewiesen. Um den Wert einer der folgenden Parameter ändern möchten, verwenden Sie einfach: `e.InputParameters("paramName") = value`. Aus diesem Grund festzulegende der `CategoryID` und `SupplierID` anpassen, um die Werte 1, der `Inserting` -Ereignishandler hinzu wie folgt aussehen:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Dieses Mal beim Hinzufügen eines neuen Produkts (z. B. Acme Soda), die `CategoryID` und `SupplierID` Spalten des neuen Produkts auf 1 festgelegt werden (siehe Abbildung 20).


[![Neue Produkte verfügen jetzt über ihre CategoryID und SupplierID Werte festlegen 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Abbildung 20**: neue Produkte jetzt haben ihre `CategoryID` und `SupplierID` Werte auf 1 festgelegt ([klicken Sie hier, um das Bild in voller Größe angezeigt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Während der Bearbeitung, einfügen und Löschen von Prozess fahren Sie das Webserver-Steuerelement und das ObjectDataSource durch eine Reihe von Ereignisse vor und nach der Ebene fort. In diesem Lernprogramm werden untersucht die vorab-Ereignissen und wurde erläutert, wie verwenden Sie diese anpassen, die Eingabeparameter, oder brechen Sie den Änderungsvorgang von Daten vollständig sowohl aus der Web-Datensteuerelement und ObjectDataSource Ereignisse. Betrachten wir in den nächsten Lernprogrammen erstellen und verwenden Ereignishandler für die Ereignisse nach der Ebene.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Jackie Goor und Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](an-overview-of-inserting-updating-and-deleting-data-vb.md)
[Weiter](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
