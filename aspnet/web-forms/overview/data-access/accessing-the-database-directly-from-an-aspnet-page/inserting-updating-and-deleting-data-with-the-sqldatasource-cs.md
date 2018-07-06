---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Einfügen, aktualisieren und Löschen von Daten mit dem SqlDataSource-Steuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In vorherigen Tutorials haben Sie erfahren, wie das ObjectDataSource-Steuerelement, die für das Einfügen, aktualisieren und Löschen von Daten zulässig. Das SqlDataSource-Steuerelement unterstützt t...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: d2958cb9cb0dd5ed89988a969663022a920a3dca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812846"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Einfügen, aktualisieren und Löschen von Daten mit dem SqlDataSource-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) oder [PDF-Datei herunterladen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> In vorherigen Tutorials haben Sie erfahren, wie das ObjectDataSource-Steuerelement, die für das Einfügen, aktualisieren und Löschen von Daten zulässig. Das SqlDataSource-Steuerelement unterstützt die gleichen Vorgänge, aber der Ansatz ist anders, und in diesem Tutorial wird gezeigt, wie dem SqlDataSource-Steuerelement zum Einfügen, aktualisieren und Löschen von Daten konfigurieren.


## <a name="introduction"></a>Einführung

Siehe [eine Übersicht der einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), GridView-Steuerelement bietet integrierte aktualisieren und Löschen von Funktionen, während die DetailsView und FormView-Steuerelemente einfügen enthalten zu unterstützen, zusammen mit Bearbeiten und Löschen von Funktionen. Diese Möglichkeiten zur Datenänderung können direkt in ein Datenquellen-Steuerelement ohne eine einzige Zeile Code geschrieben werden müssen, angeschlossen werden. [Eine Übersicht der einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) untersucht, mit dem ObjectDataSource-Steuerelement um zu ermöglichen, einfügen, aktualisieren und Löschen mit der GridView, DetailsView oder FormView-Steuerelemente. Alternativ kann dem SqlDataSource-Steuerelement statt dem ObjectDataSource-Steuerelement verwendet werden.

Denken Sie daran, die zur Unterstützung von einfügen, aktualisieren und löschen, mit dem ObjectDataSource-Steuerelement wir benötigt an die Methoden des Objekts Ebene aufrufen, die zum Ausführen von INSERT-, update oder delete-Aktion. Mit dem SqlDataSource-Steuerelement, ich benötige `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen (oder gespeicherte Prozeduren) ausgeführt. In diesem Tutorial sehen, werden diese Anweisungen können manuell erstellt werden oder automatisch vom SqlDataSource s Konfigurieren von Datenquellen-Assistenten generiert werden.

> [!NOTE]
> Seit wir haben bereits erläutert das Einfügen, bearbeiten und löschen die Funktionen von GridView, DetailsView und FormView-Steuerelemente, dieses Tutorial konzentriert sich auf das SqlDataSource-Steuerelement, um die Unterstützung dieser Vorgänge zu konfigurieren. Bei Bedarf zu auffrischen zur Implementierung dieser Funktionen in der Rückgabe GridView, DetailsView und FormView-Steuerelement, in den Tutorials bearbeiten, einfügen und Löschen von Daten, beginnend mit [eine Übersicht der einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Schritt 1: Angeben von`INSERT`,`UPDATE`, und`DELETE`Anweisungen

Als wir festgelegt haben, finden Sie in den letzten beiden Tutorials zum Abrufen von Daten von einem SqlDataSource-Steuerelement müssen wir zwei Eigenschaften:

1. `ConnectionString`, die angibt, welche Datenbank zum Senden der Abfrage, und
2. `SelectCommand`, der angibt, die Ad-hoc-SQL-Anweisung oder der Name der gespeicherten Prozedur, die ausgeführt werden, um die Ergebnisse zurückgeben.

Für `SelectCommand` Werte mit Parametern, die den Parameter Werte, über die SqlDataSource s angegeben werden `SelectParameters` Auflistung und hart kodierte Werte häufig verwendete Parameter Quellwerte enthalten (Querystring-Felder, Sitzungsvariablen, Web Control-Werte, und usw.), oder programmgesteuert zugewiesen werden kann. Wenn die SqlDataSource-Steuerelement s `Select()` Methode wird aufgerufen, entweder programmgesteuert oder automatisch aus einem Websteuerelement eine Verbindung mit der Datenbank hergestellt wird, werden die Parameterwerte für die Abfrage zugewiesen und der Befehl ist deaktiviert übermittelt der die Datenbank. Die Ergebnisse werden anschließend als DataSet oder DataReader, zurückgegeben, abhängig vom Wert des Steuerelements s `DataSourceMode` Eigenschaft.

Zusammen mit der Auswahl von Daten, das SqlDataSource-Steuerelement verwendet werden kann, zum Einfügen, aktualisieren und Löschen von Daten durch Angabe `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen in die gleiche Weise. Weisen Sie einfach die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften der `INSERT`, `UPDATE`, und `DELETE` auszuführenden SQL-Anweisungen. Wenn die Anweisungen über Parameter (wie sie die meisten immer), fügen Sie sie in der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen.

Sobald ein `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Wert angegeben wurde, die Option "Einfügen aktivieren, aktivieren Sie bearbeiten oder löschen aktivieren" in die entsprechenden Daten smart Tag des Websteuerelements s zur Verfügung stehen. Um dies zu veranschaulichen, nehmen können s ein Beispiel aus der `Querying.aspx` Seite, die wir erstellt, in haben der [Abfragen von Daten mit dem SqlDataSource-Steuerelement](querying-data-with-the-sqldatasource-control-cs.md) Tutorial, und ergänzen es sollen die Funktionen löschen.

Öffnen Sie zunächst die `InsertUpdateDelete.aspx` und `Querying.aspx` Seiten aus der `SqlDataSource` Ordner. Im Designer auf die `Querying.aspx` Seite, wählen Sie aus dem ersten Beispiel SqlDataSource-Steuerelement und GridView (die `ProductsDataSource` und `GridView1` Steuerelemente). Finden Sie nachdem Sie die beiden Steuerelemente haben unter dem Menü "Bearbeiten" und wählen Sie kopieren (oder drücken Sie STRG + C nur). Navigieren Sie anschließend auf den Designer der `InsertUpdateDelete.aspx` , und fügen Sie in den Steuerelementen. Nachdem Sie auf die beiden Steuerelemente, über verschoben haben `InsertUpdateDelete.aspx`, testen Sie die Seite in einem Browser. Daraufhin sollte die Werte der `ProductID`, `ProductName`, und `UnitPrice` Spalten für alle Datensätze in der `Products` Datenbanktabelle.


[![Alle Produkte aufgeführt sind, sortiert nach "ProductID",](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Abbildung 1**: alle Produkte aufgeführt sind, sortiert nach `ProductID` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Die Zuordnungsvorgänge SqlDataSource-Steuerelement hinzufügen`DeleteCommand`und`DeleteParameters`Eigenschaften

An diesem Punkt haben wir ein SqlDataSource-Steuerelement, die einfach alle Datensätze aus zurückgibt der `Products` Tabelle und einer GridView-Ansicht, die diese Daten rendert. Unser Ziel ist in diesem Beispiel für den Benutzer zum Löschen von Produkten über die GridView zu erweitern. Um dies zu erreichen, wir geben Sie Werte für die SqlDataSource-Steuerelement s müssen `DeleteCommand` und `DeleteParameters` Eigenschaften und konfigurieren Sie dann auf die GridView, um das Löschen zu unterstützen.

Die `DeleteCommand` und `DeleteParameters` Eigenschaften können angegeben werden, in eine Reihe von Möglichkeiten:

- Mithilfe der deklarativen syntax
- Im Eigenschaftenfenster im Designer
- Klicken Sie auf der Specify Bildschirm eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur im Konfigurieren von Datenquellen-Assistenten
- Über die Schaltfläche "Erweitert" in die Spalten von einer Tabelle der Bildschirm zum Anzeigen von im Konfigurieren von Datenquellen-Assistenten angeben, generiert die tatsächlich automatisch die `DELETE` in verwendete Auflistung für SQL-Anweisung und die Parameter der `DeleteCommand` und `DeleteParameters` Eigenschaften

Untersucht, wie Sie automatisch die `DELETE` Anweisung, die in Schritt2 erstellt haben. Jetzt können Sie s, die das Fenster "Eigenschaften" im Designer verwenden, jedoch die Assistenten zum Konfigurieren von Datenquellen oder die deklarative syntaxoption genauso gut funktionieren würde.

Vom Designer in `InsertUpdateDelete.aspx`, klicken Sie auf die `ProductsDataSource` SqlDataSource-Steuerelement, und klicken Sie dann machen Sie das Fenster "Eigenschaften" (klicken Sie im Menü "Ansicht" Fenster "Eigenschaften" auswählen, oder einfach drücken Sie F4). Wählen Sie die DeleteQuery-Eigenschaft, die einen Satz von Ellipsen angezeigt wird.


![Wählen Sie die DeleteQuery-Eigenschaft im Eigenschaftenfenster](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Abbildung 2**: Wählen Sie die DeleteQuery-Eigenschaft im Eigenschaftenfenster


> [!NOTE]
> Die SqlDataSource-Steuerelement wurden einer DeleteQuery-Eigenschaft. Stattdessen DeleteQuery ist eine Kombination der `DeleteCommand` und `DeleteParameters` Eigenschaften und wird nur im Fenster Eigenschaften aufgelistet, wenn Sie das Fenster mit dem Designer anzeigen. Wenn Sie im Fenster Eigenschaften in der Datenquellensicht angezeigt wird, finden Sie die `DeleteCommand` Eigenschaft stattdessen.


Klicken Sie auf die Auslassungspunkte in der DeleteQuery-Eigenschaft, um das Dialogfeld "Befehls- und Parameter-Editor" zu öffnen, Feld (siehe Abbildung 3). In diesem Dialogfeld können Sie angeben der `DELETE` SQL-Anweisung, und geben Sie die Parameter. Geben Sie die folgende Abfrage in der `DELETE` Befehlstextfeld ein (entweder manuell oder über den Abfrage-Generator, falls gewünscht):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Klicken Sie dann auf die Schaltfläche "Parameter aktualisieren", Hinzufügen der `@ProductID` Parameter, um die Liste der folgenden Parameter.


[![Wählen Sie die DeleteQuery-Eigenschaft im Eigenschaftenfenster](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Abbildung 3**: Wählen Sie die DeleteQuery-Eigenschaft im Eigenschaftenfenster ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Führen Sie *nicht* Geben Sie einen Wert für diesen Parameter (lassen, die als Parameter an keine Quelle). Nachdem wir Unterstützung für das Löschen von an die GridView hinzugefügt haben, die GridView wird automatisch Bereitstellen der Wert dieses Parameters, mit dem Wert des der `DataKeys` Sammlung für die Zeile, deren löschen-Schaltfläche geklickt wurde.

> [!NOTE]
> Der Parametername, der verwendet wird, der `DELETE` Abfrage *muss* übereinstimmen, den der Name des der `DataKeyNames` Wert in der GridView, DetailsView oder FormView-Steuerelement. D. h. der Parameter in der `DELETE` Anweisung ist absichtlich mit dem Namen `@ProductID` (anstelle von, z. B. `@ID`), da der Name der Primärschlüsselspalte in der Products-Tabelle (und daher die DataKeyNames-Wert in den GridView-Ansicht) ist `ProductID`.


Wenn der Name des Parameters und `DataKeyNames` Wert t Match, GridView kann nicht automatisch weisen Sie dem Parameter den Wert aus der `DataKeys` Auflistung.

Klicken Sie nach dem Eingeben der Delete-bezogene Informationen in das Befehls- und Parameter-Editor-Dialogfeld klicken Sie auf OK, und wechseln Sie zur Quellansicht, das sich ergebende deklarative Markup zu untersuchen:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Beachten Sie das Hinzufügen der `DeleteCommand` Eigenschaft als auch die `<DeleteParameters>` Abschnitt und das Parameterobjekt mit dem Namen `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurieren der GridView für das Löschen

Mit der `DeleteCommand` Eigenschaft hinzugefügt, das GridView-s-Smarttag enthält jetzt die Option löschen aktivieren. Fahren Sie fort, und aktivieren Sie dieses Kontrollkästchen. Siehe [eine Übersicht der einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), dies bewirkt, dass die GridView eine CommandField mit Hinzufügen der `ShowDeleteButton` -Eigenschaft auf festgelegt `true`. Abbildung 4 zeigt, wenn die Seite über einen Browser zugegriffen wird, an denen eine Löschen-Schaltfläche enthalten ist. Testen Sie diese Seite, löschen Sie einige Produkte.


[![Jede Zeile GridView enthält jetzt eine Schaltfläche "löschen"](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Abbildung 4**: jede GridView-Zeile enthält jetzt eine Löschen-Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Nach dem Klicken auf eine Schaltfläche "löschen", ein Postback auftritt, weist der GridView der `ProductID` Parameter den Wert von der `DataKeys` -Wert für die Zeile, deren Schaltfläche "löschen" geklickt wurde, und ruft die SqlDataSource s `Delete()` Methode. Das SqlDataSource-Steuerelement klicken Sie dann eine Verbindung mit der Datenbank her und führt die `DELETE` Anweisung. GridView bindet dann erneut, auf dem SqlDataSource-Steuerelement, abrufen und Anzeigen von den aktuellen Satz von Produkten (einschließlich nicht mehr den Datensatz einfach gelöscht).

> [!NOTE]
> Da GridView verwendet die `DataKeys` Auflistung zum Auffüllen der SqlDataSource-Parameter, es wichtige s, der GridView-s `DataKeyNames` Eigenschaft festgelegt werden, um die Spalten, die den Primärschlüssel und die bilden s SqlDataSource-Steuerelement `SelectCommand` zurückgibt Diese Spalten. Darüber hinaus es wichtig, dass der Parameter in der SqlDataSource s Namen s `DeleteCommand` nastaven NA hodnotu `@ProductID`. Wenn die `DataKeyNames` Eigenschaft nicht festgelegt ist, oder der Parameter ist nicht mit dem Namen `@ProductsID`, klicken Sie auf die Schaltfläche "löschen" führt dazu, dass einen Postback, aber gewonnenem t tatsächlich einen Datensatz gelöscht.


Abbildung 5 zeigt diese Interaktion grafisch an. Verweisen zurück auf die [Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) Tutorial für eine ausführlichere Erläuterung in der Kette der Ereignisse im Zusammenhang mit einfügen, aktualisieren und löschen aus einem Websteuerelement.


![Klicken Sie auf die Schaltfläche "löschen" in der GridView, ruft die SqlDataSource-s-Delete()-Methode](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Abbildung 5**: Klicken Sie auf die Schaltfläche "löschen" in den GridView-Ansicht die s SqlDataSource-Steuerelement ruft `Delete()` Methode


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Schritt 2: Wird automatisch generiert. die`INSERT`,`UPDATE`, und`DELETE`Anweisungen

Schritt 1 untersucht `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen können über das Fenster "Eigenschaften" oder die deklarative Syntax des Steuerelements s angegeben werden. Dieser Ansatz erfordert jedoch, dass manuell, die SQL-Anweisungen manuell schreiben wir die monotone und fehleranfällig sein können. Glücklicherweise bietet der Assistent zum Konfigurieren der Datenquelle eine Option aus, damit die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, die automatisch generiert, wenn die Spalten angeben, aus einer Tabelle mit dem Bildschirm "Ansicht" verwenden.

Lassen Sie s, die diese Option für die automatische Generierung nutzen. Hinzufügen des Designers in einem DetailsView `InsertUpdateDelete.aspx` und legen Sie seine `ID` Eigenschaft `ManageProducts`. Von DetailsView s Smarttags, wählen Sie als Nächstes erstellen eine neue Datenquelle, und erstellen ein SqlDataSource-Steuerelement mit dem Namen `ManageProductsDataSource`.


[![Erstellen Sie eine neue SqlDataSource-Steuerelement mit dem Namen ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Abbildung 6**: Erstellen einer neuen SqlDataSource-Steuerelement mit dem Namen `ManageProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Deaktivieren Sie im Konfigurieren von Datenquellen-Assistenten werden Sie zum Verwenden der `NORTHWINDConnectionString` Verbindung Zeichenfolge ein, und klicken Sie auf Weiter. Aus dem Bildschirm für die Select-Anweisung konfigurieren, lassen Sie die Spalten angeben, aus einer Tabelle oder Sicht Optionsfeld ausgewählt, und wählen Sie die `Products` Tabelle aus der Dropdown-Liste. Wählen Sie die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten aus der Liste das Kontrollkästchen.


[![Verwenden die Products-Tabelle, die ProductID, ProductName, UnitPrice und nicht mehr unterstützte Spalten zurückgeben](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Abbildung 7**: mithilfe der `Products` Table, Zurückgeben der `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Zum automatischen Generieren von `INSERT`, `UPDATE`, und `DELETE` Anweisungen basierend auf der ausgewählten Tabellen- und Spalten, klicken Sie auf die Schaltfläche "Erweitert", und überprüfen Sie die generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen.


![Überprüfen Sie die generieren INSERT, UPDATE und DELETE-Anweisungen Kontrollkästchen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Abbildung 8**: Überprüfen der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen


Der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen wird nur überprüfbar sein, wenn die ausgewählte Tabelle über einen Primärschlüssel verfügt, und die Primärschlüsselspalte (oder Spalten) in der Liste der zurückgegebenen Spalten enthalten sind. Kontrollkästchen vollständige Parallelität verwenden, das ausgewählt wird, nach der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen überprüft wurden, erweitern, wird die `WHERE` Klauseln in der resultierenden `UPDATE` und `DELETE` Anweisungen, die Steuerung durch vollständige Parallelität bereitzustellen. Jetzt lassen Sie dieses Kontrollkästchen deaktiviert wird; im nächsten Tutorial untersuchen wir die vollständigen Parallelität mit dem SqlDataSource-Steuerelement.

Nach der Überprüfung der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen deaktivieren, klicken Sie auf "OK", um die Select-Anweisung konfigurieren Bildschirm zurückzukehren, klicken Sie auf Weiter, und schließen, um das Konfigurieren von Datenquellen-Assistenten zu beenden. Nach Abschluss des Assistenten, Visual Studio fügt BoundFields zur DetailsView für das `ProductID`, `ProductName`, und `UnitPrice` Spalten und eine CheckBoxField für die `Discontinued` Spalte. Aktivieren Sie vom DetailsView s Smarttag Paging aktivieren die Option, damit der Benutzer, die auf dieser Seite können die Produkte durchlaufen kann. Löschen Sie auch, das DetailsView-s `Width` und `Height` Eigenschaften.

Beachten Sie, dass das Smarttag der verfügbaren Optionen einfügen aktivieren, aktivieren Sie bearbeiten und löschen aktivieren. Dies ist, da es sich bei dem SqlDataSource-Steuerelement für Werte enthält die `InsertCommand`, `UpdateCommand`, und `DeleteCommand`, wie die folgende deklarative Syntax gezeigt:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Beachten Sie, wie das SqlDataSource-Steuerelement automatisch für die festgelegten Werte wurden die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Die Anzahl der Spalten, die auf die verwiesen wird der `InsertCommand` und `UpdateCommand` Eigenschaften basieren auf den in der `SELECT` Anweisung. Also statt *jeder* Produkte-Spalte in der `InsertCommand` und `UpdateCommand`, es gibt nur die Spalten, die im angegebenen die `SelectCommand` (weniger `ProductID`, die wird ausgelassen, da es s ein [ `IDENTITY` Spalte](http://www.sqlteam.com/item.asp?ItemID=102), dessen Wert nicht geändert werden, wenn bearbeitet, und beim Einfügen automatisch zugewiesen). Darüber hinaus für jeden Parameter in der `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften vorhanden entsprechende Parameter in sind der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen.

Überprüfen Sie zum Aktivieren der DetailsView s Datenfeatures Änderung der einfügen aktivieren, Bearbeiten aktivieren, und löschen aktivieren Optionen auf sein Smarttag. Dadurch wird eine CommandField mit seiner `ShowInsertButton`, `ShowEditButton`, und `ShowDeleteButton` Eigenschaften festgelegt, um `true`.

Besuchen Sie die Seite in einem Browser, und notieren Sie sich, das Bearbeiten, löschen und neue Schaltflächen in DetailsView enthalten. Klicken Sie auf die Schaltfläche "Bearbeiten" DetailsView in den Bearbeitungsmodus, in dem jede BoundField angezeigt wird, dessen `ReadOnly` -Eigenschaftensatz auf `false` (Standard) als ein Textfeld, und die CheckBoxField als Kontrollkästchen.


[![Das DetailsView-s-Standard Bearbeitungsschnittstelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Abbildung 9**: das DetailsView-s-Standard bearbeiten-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Auf ähnliche Weise können Sie das derzeit ausgewählte Produkt zu löschen oder Hinzufügen eines neuen Produkts mit dem System. Da die `InsertCommand` Anweisung funktioniert nur mit der `ProductName`, `UnitPrice`, und `Discontinued` Spalten, die anderen Spalten haben entweder `NULL` oder ihren Standardwert zugewiesen, die von der Datenbank beim Einfügen. Ebenso wie mit dem ObjectDataSource-Steuerelement, wenn die `InsertCommand` fehlt einer Datenbanktabelle, die Spalten, die nicht von t vorgenommen `NULL` s "und" Don ' t über einen Standardwert verfügen, ein SQL-Fehler auftreten, wird beim Ausführen der `INSERT` Anweisung.

> [!NOTE]
> Das DetailsView s einfügen und Bearbeiten von Schnittstellen verfügen nicht über ein beliebiges Anpassung oder Überprüfung aus. Hinzufügen von Steuerelementen zur gültigkeitsprüfung oder anpassen, die Schnittstellen, müssen Sie die BoundFields in von TemplateFields zu konvertieren. Finden Sie in der [Hinzufügen von Steuerelementen zur gültigkeitsprüfung zum Bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) und [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Lernprogramme für Weitere Informationen.


Beachten Sie, dass für das Aktualisieren und löschen, DetailsView das aktuelle Produkt s verwendet außerdem, dass `DataKey` -Wert, der nur vorhanden ist, wenn die `DataKeyNames` Eigenschaft so konfiguriert ist. Wenn das Bearbeiten oder löschen angezeigt wird, keine Auswirkung haben, stellen Sie sicher, dass die `DataKeyNames` festgelegt wird.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Einschränkungen von SQL-Anweisungen wird automatisch generiert.

Seit der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen-Option ist nur verfügbar, wenn Spalten aus einer Tabelle auswählen, für komplexere, Sie Abfragen Ihren eigenen schreiben muss `INSERT`, `UPDATE`, und `DELETE` Anweisungen, wie in Schritt 1. Häufig, SQL `SELECT` -Anweisungen `JOIN` s zum Wiederherstellen von Daten aus einem oder mehreren Nachschlagetabellen für Anzeigezwecke (z. B. Onlineschaltung des wieder die `Categories` Tabelle s `CategoryName` Feld beim Anzeigen von Produktinformationen). Zur gleichen Zeit, wir möchten ermöglicht dem Benutzer, bearbeiten, aktualisieren oder Einfügen von Daten in der Tabelle "Core" (`Products`, in diesem Fall).

Während der `INSERT`, `UPDATE`, und `DELETE` Anweisungen können manuell eingegeben werden, sollten Sie die folgenden sparen Sie Zeit. Setup-zunächst dem SqlDataSource-Steuerelement, damit sie wieder Daten einfach Abrufen der `Products` Tabelle. Verwenden Sie die Spalten aus einer Tabelle oder Sicht Bildschirm Konfigurieren von Datenquellen-Assistenten s angeben, damit Sie automatisch generieren können die `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Wählen Sie dann nach Abschluss des Assistenten zum Konfigurieren der SelectQuery aus dem Fenster "Eigenschaften" (oder alternativ, wechseln Sie zurück an den Assistent zum Konfigurieren von Datenquellen, aber verwenden Sie die benutzerdefinierte SQL-Anweisung angeben oder die gespeicherte Prozedur-Option). Aktualisieren Sie dann die `SELECT` -Anweisung zum Einschließen der `JOIN` Syntax. Diese Technik bietet zeitsparende Vorteile der automatisch generierten SQL-Anweisungen und ermöglicht eine stärker angepassten `SELECT` Anweisung.

Eine weitere Einschränkung der automatischen farbgenerierung die `INSERT`, `UPDATE`, und `DELETE` Anweisungen besteht, die aus den Spalten in der `INSERT` und `UPDATE` Anweisungen werden auf Grundlage der Spalten, die vom der `SELECT` Anweisung. Wir müssen möglicherweise aktualisieren oder Einfügen von mehr oder weniger Felder jedoch. Angenommen, in dem Beispiel aus Schritt2, vielleicht möchten wir haben die `UnitPrice` BoundField schreibgeschützt sein. In diesem Fall es treten normalerweise t angezeigt, der `UpdateCommand`. Oder wir können möchten, legen Sie den Wert eines Tabellenfelds, die nicht angezeigt wird, in den GridView-Ansicht. Z. B. das Hinzufügen einer neuen Datensatz möglicherweise möchten wir die `QuantityPerUnit` Wert auf TODO festgelegt.

Wenn solche Anpassungen erforderlich sind, müssen Sie sie manuell über das Fenster "Eigenschaften", die benutzerdefinierte SQL-Anweisung angeben oder die gespeicherte Prozedur eine Option im Assistenten oder über die deklarative Syntax machen.

> [!NOTE]
> Beim Hinzufügen von Parametern, die entsprechenden Felder in den Daten noch keine-Steuerelement Web, sollten Sie bedenken, die diesen Parameterwerten Werte auf irgendeine Weise zugewiesen werden müssen. Diese Werte sind möglich: hartcodierte direkt in die `InsertCommand` oder `UpdateCommand`; können aus einer vordefinierten Quelle (die Abfragezeichenfolge, Sitzungszustand, Websteuerelemente auf der Seite, und So weiter); stammen oder können programmgesteuert zugewiesen werden, wie im vorherigen Tutorial beschrieben.


## <a name="summary"></a>Zusammenfassung

Damit die Daten, dass websteuerungselemente, um ihre integrierte einfügen, bearbeiten und Löschen von Funktionen zu nutzen muss das Datenquellen-Steuerelement an das, dem Sie gebunden sind, solche Funktionen bieten. Für dem SqlDataSource-Steuerelement, das bedeutet, dass `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen müssen zugewiesen werden, um die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Diese Eigenschaften und die entsprechenden Parameter Sammlungen enthalten, können manuell hinzugefügt oder mit dem Konfigurieren von Datenquellen-Assistenten automatisch generiert. In diesem Tutorial untersucht wir beide Verfahren.

Untersuchten wir zu "ObjectDataSource" vollständige Parallelität mit dem [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Tutorial. Das SqlDataSource-Steuerelement unterstützt außerdem optimistische Parallelität. Wie in Schritt2 erwähnt, bei der automatischen farbgenerierung die `INSERT`, `UPDATE`, und `DELETE` -Anweisungen, die der Assistent bietet einer vollständige Parallelität-Option verwenden. Im nächsten Tutorial sehen, verwenden optimistischen Parallelität, mit dem SqlDataSource-Steuerelement ändert die `WHERE` Klauseln in der `UPDATE` und `DELETE` Anweisungen, um sicherzustellen, dass die Werte für die anderen Spalten geändert wurde, seit die Daten entsprechend letzten seiner t auf der Seite angezeigt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Weiter](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
