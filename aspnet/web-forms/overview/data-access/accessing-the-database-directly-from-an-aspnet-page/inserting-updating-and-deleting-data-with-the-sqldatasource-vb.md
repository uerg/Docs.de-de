---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Einfügen, aktualisieren und Löschen von Daten mit SqlDataSource-(VB) | Microsoft Docs
author: rick-anderson
description: In vorherigen Lernprogrammen haben wir gelernt, wie das ObjectDataSource-Steuerelement für die einfügen, aktualisieren und Löschen von Daten zulässig. SqlDataSource-Steuerelement unterstützt t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 92d195c3e1e349cd82e0625cf9a6c5a82644b5db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Einfügen, aktualisieren und Löschen von Daten mit SqlDataSource-(VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) oder [PDF herunterladen](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> In vorherigen Lernprogrammen haben wir gelernt, wie das ObjectDataSource-Steuerelement für die einfügen, aktualisieren und Löschen von Daten zulässig. SqlDataSource-Steuerelement unterstützt die gleichen Vorgänge jedoch der Ansatz ist anders, und dieses Lernprogramm zeigt, wie die SqlDataSource zum Einfügen, aktualisieren und Löschen von Daten konfigurieren.


## <a name="introduction"></a>Einführung

Entsprechend der Anleitung unter [eine Übersicht über die von einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)des GridView-Steuerelements enthält integrierte aktualisieren und Löschen von Funktionen, während die, DetailsView und FormView-Steuerelemente einfügen umfassen unterstützt zusammen mit Bearbeiten und Löschen von Funktionen. Diese Möglichkeiten zur Datenänderung können direkt an ein Datenquellen-Steuerelement ohne eine Codezeile geschrieben werden müssen angeschlossen werden. [Eine Übersicht der einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) überprüft das ObjectDataSource zum Einfügen, aktualisieren und Löschen mit GridView, DetailsView und FormView erleichtern. Alternativ kann die SqlDataSource anstelle der ObjectDataSource verwendet werden.

Denken Sie daran, dass zum Einfügen, aktualisieren und löschen, mit der ObjectDataSource wir erforderlich sind, geben Sie die Layer-Objektmethoden aufzurufenden zum Ausführen von INSERT-, Update- oder delete Aktion zu unterstützen. Bei der SqlDataSource müssen wir bereitstellen `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen (oder gespeicherte Prozeduren) ausgeführt. Wie wir in diesem Lernprogramm sehen werden, werden diese Anweisungen können manuell erstellt werden oder automatisch vom SqlDataSource s Konfigurieren von Datenquellen-Assistenten generiert werden.

> [!NOTE]
> Seit wir besprochen haben bereits einfügen, bearbeiten und Löschen von Funktionen der GridView, DetailsView, und steuert die FormView, konzentriert sich dieses Lernprogramm zum Konfigurieren der SqlDataSource-Steuerelement, um diese Vorgänge zu unterstützen. Wenn Sie zum Implementieren dieser Funktionen in der Rückgabe GridView, DetailsView und FormView, um die Lernprogramme bearbeiten, einfügen und Löschen von Daten-Kenntnisse auffrischen möchten, beginnend mit [eine Übersicht über die von einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Schritt 1: Angeben von`INSERT`,`UPDATE`, und`DELETE`Anweisungen

In den letzten zwei Lernprogramme zum Abrufen von Daten von einem SqlDataSource-Steuerelement zum benötigten gesehen haben zwei Eigenschaften festgelegt werden wie wir:

1. `ConnectionString`, der angibt, was Datenbank senden Sie die Abfrage, und
2. `SelectCommand`, die angibt, die Ad-hoc-SQL-Anweisung oder der Name der gespeicherten Prozedur ausgeführt wird, um die Ergebnisse zurückzugeben.

Für `SelectCommand` Werte mit den Parametern, die Werte, über die SqlDataSource-s angegeben werden-Parameter `SelectParameters` Auflistung und zählen hartcodierte Werte, die allgemeine Parameter Quelle (Querystring-Felder, Sitzungsvariablen, Web Steuerelementwerte, und usw.), oder programmgesteuert zugewiesen werden kann. Wenn die SqlDataSource-Steuerelement s `Select()` Methode wird aufgerufen, entweder programmgesteuert oder automatisch von einem Webserver-Steuerelement eine Verbindung mit der Datenbank hergestellt wird, werden die Parameterwerte an die Abfrage zugewiesen und der Befehl ist auf weitergeleitet der die Datenbank. Die Ergebnisse werden dann als DataSet oder DataReader zurückgegeben, abhängig vom Wert des Steuerelements s `DataSourceMode` Eigenschaft.

Zusammen mit der Auswahl von Daten, SqlDataSource-Steuerelement verwendet werden kann, zum Einfügen, aktualisieren und Löschen von Daten durch die Angabe `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen im großen und ganzen genauso. Weisen Sie einfach die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften der `INSERT`, `UPDATE`, und `DELETE` auszuführenden SQL-Anweisungen. Wenn die Anweisungen Parameter haben (wie sie die meisten immer), fügen Sie sie in der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen.

Sobald ein `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Wert angegeben wurde, die Option "Einfügen aktivieren, aktivieren Sie bearbeiten oder löschen aktivieren" in den entsprechenden Daten Web Control s Smarttag zur Verfügung gestellt. Um dies zu veranschaulichen, nehmen Sie Let s ein Beispiel aus der `Querying.aspx` Seite, die wir erstellt, in haben der [Abfragen von Daten mit SqlDataSource-Steuerelement](querying-data-with-the-sqldatasource-control-vb.md) Lernprogramm und ergänzen, die sie enthalten Funktionen löschen.

Öffnen Sie zunächst die `InsertUpdateDelete.aspx` und `Querying.aspx` Protokollseiten aus der `SqlDataSource` Ordner. Im Designer auf die `Querying.aspx` Seite, wählen Sie aus dem ersten Beispiel SqlDataSource und GridView (die `ProductsDataSource` und `GridView1` Steuerelemente). Nachdem Sie die beiden Steuerelemente haben, wechseln Sie zum Menü Bearbeiten und wählen Sie kopieren (oder drücken Sie einfach STRG + C). Gehen Sie anschließend auf den Designer der `InsertUpdateDelete.aspx` , und fügen Sie in den Steuerelementen. Nachdem Sie die beiden Steuerelemente zu, über verschoben haben `InsertUpdateDelete.aspx`, testen Sie die Seite in einem Browser. Daraufhin sollte die Werte der `ProductID`, `ProductName`, und `UnitPrice` Spalten für alle Datensätze in der `Products` Datenbanktabelle.


[![Alle Produkte sind aufgeführt, geordnet nach "ProductID",](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Abbildung 1**: alle Produkte sind aufgeführt, geordnet nach `ProductID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Die ': SqlDataSource-Zuordnungsvorgänge hinzufügen`DeleteCommand`und`DeleteParameters`Eigenschaften

An diesem Punkt haben wir ein SqlDataSource, die einfach alle Datensätze aus zurückgibt der `Products` Tabelle und eine GridView, die diese Daten gerendert wird. Unser Ziel ist, erweitern in diesem Beispiel für die Benutzer So löschen Sie die Produkte über die GridView zulässig. Um dies zu erreichen wir die Angabe von Werten für das ': SqlDataSource-Steuerelement s müssen `DeleteCommand` und `DeleteParameters` Eigenschaften und konfigurieren Sie dann die GridView zur Unterstützung von löschen.

Die `DeleteCommand` und `DeleteParameters` Eigenschaften können angegeben werden, eine Reihe von Möglichkeiten:

- Durch die deklarative syntax
- Im Fenster "Eigenschaften" im Designer
- Aus der benutzerdefinierten SQL-Anweisung angeben oder die gespeicherte Prozedur Bildschirm des Assistenten konfigurieren von Datenquellen
- Über die Schaltfläche "Erweitert" in der Spalten angeben, aus einer Tabelle mit den Bildschirm zum Anzeigen von in der Datenquelle konfigurieren-Assistent generiert die tatsächlich automatisch die `DELETE` SQL-Anweisung und Parameter-Auflistung verwendet werden, der `DeleteCommand` und `DeleteParameters` Eigenschaften

Untersucht, wie automatisch die `DELETE` Anweisung, die in Schritt2 erstellt haben. Jetzt können Sie s im Designer das Eigenschaftenfenster verwenden, zwar der Assistent zum Konfigurieren von Datenquellen oder deklarative Syntax Option genauso gut funktioniert.

Vom Designer in `InsertUpdateDelete.aspx`, klicken Sie auf die `ProductsDataSource` SqlDataSource und schalten Sie das Eigenschaftenfenster (klicken Sie im Menü Ansicht wählen Sie die Fenster "Eigenschaften" oder einfach drücken Sie F4). Wählen Sie die DeleteQuery-Eigenschaft, die einen Satz von Ellipsen angezeigt wird.


![Wählen Sie im Eigenschaftenfenster die DeleteQuery-Eigenschaft](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Abbildung 2**: Wählen Sie im Eigenschaftenfenster die DeleteQuery-Eigenschaft


> [!NOTE]
> T SqlDataSource verfügt über eine DeleteQuery-Eigenschaft verfügen. Stattdessen DeleteQuery ist eine Kombination der `DeleteCommand` und `DeleteParameters` Eigenschaften und wird nur im Fenster Eigenschaften aufgelistet, wenn das Fenster über den Designer anzeigen. Wenn Sie das Fenster Eigenschaften in der Datenquellensicht betrachten, finden Sie die `DeleteCommand` Eigenschaft stattdessen.


Klicken Sie auf die Auslassungszeichen in der DeleteQuery-Eigenschaft, um das Dialogfeld "Befehls- und Parameter-Editor" zu öffnen, Feld (siehe Abbildung 3). In diesem Dialogfeld Geben Sie die `DELETE` SQL-Anweisung, und geben Sie die Parameter. Geben Sie die folgende Abfrage in der `DELETE` Befehlstextfeld (entweder manuell oder über den Abfrage-Generator, falls gewünscht):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Klicken Sie anschließend auf die Schaltfläche "Parameter aktualisieren", zum Hinzufügen der `@ProductID` Parameter, um die Liste der Parameter unten.


[![Wählen Sie im Eigenschaftenfenster die DeleteQuery-Eigenschaft](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Abbildung 3**: Wählen Sie im Fenster Eigenschaften die DeleteQuery-Eigenschaft ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Führen Sie *nicht* Geben Sie einen Wert für diesen Parameter (lassen Sie der Parameter keine Quelle). Nachdem wir Löschen von Unterstützung an die GridView hinzugefügt haben, die GridView wird automatisch bereitstellen dieser Parameterwert mit dem Wert des seine `DataKeys` Auflistung für die Zeile, deren Schaltfläche "löschen" geklickt wurde.

> [!NOTE]
> Der Name des Parameters verwendet wird, der `DELETE` Abfrage *müssen* identisch mit den Namen des der `DataKeyNames` Wert in die GridView, DetailsView oder FormView. D. h. der Parameter in der `DELETE` Anweisung lautet absichtlich `@ProductID` (anstelle von, z. B. `@ID`), da die Namen der Primärschlüsselspalte in der Products-Tabelle (und daher die DataKeyNames-Wert in die GridView) wird `ProductID`.


Wenn der Name des Parameters und `DataKeyNames` Wert ist nicht t Übereinstimmung GridView kann nicht automatisch weisen Sie dem Parameter den Wert aus der `DataKeys` Auflistung.

Klicken Sie nach der Eingabe der Delete-bezogene Informationen in das Dialogfeld Befehls- und Parameter-Editor auf OK, und wechseln Sie zur Quellansicht untersuchen Sie die resultierende deklarative Markup:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Beachten Sie das Hinzufügen der `DeleteCommand` Eigenschaft als auch die `<DeleteParameters>` Abschnitt und das Parameterobjekt mit dem Namen `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurieren die GridView zum Löschen

Mit der `DeleteCommand` Eigenschaft hinzugefügt, das Smarttag für GridView s enthält jetzt die Option löschen aktivieren. Fahren Sie fort, und aktivieren Sie dieses Kontrollkästchen. Entsprechend der Anleitung unter [eine Übersicht über die von einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), dies bewirkt, dass die GridView hinzuzufügende eine CommandField mit seiner `ShowDeleteButton` -Eigenschaftensatz auf `True`. Wie Abbildung 4 zeigt, bei die Seite über einen Browser zugegriffen wird ist eine Schaltfläche "löschen" enthalten. Testen Sie diese Seite, löschen Sie alle Produkte.


[![Jede Zeile GridView enthält nun eine Schaltfläche "löschen"](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Abbildung 4**: jede Zeile GridView enthält nun eine Schaltfläche "löschen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Wenn Sie auf eine Schaltfläche "löschen" aus, ein Postback auftritt, weist die GridView der `ProductID` den Wert des Parameters der der `DataKeys` Auflistungswert für die Zeile, deren Schaltfläche "löschen" geklickt wurde, und ruft die SqlDataSource-s `Delete()` Methode. SqlDataSource-Steuerelement klicken Sie dann eine Verbindung mit der Datenbank her und führt die `DELETE` Anweisung. Die GridView bindet klicken Sie dann auf die SqlDataSource, abrufen und Anzeigen von den aktuellen Satz von Produkten (die nicht mehr den gerade gelöschten Datensatz enthält).

> [!NOTE]
> Da GridView verwendet seine `DataKeys` Auflistung zum Auffüllen der Parameter ': SqlDataSource es wichtiger s, die GridView s `DataKeyNames` Eigenschaft festgelegt werden, um die Spalte(n), die die primary key- und, bilden die SqlDataSource-s `SelectCommand` gibt Diese Spalten. Darüber hinaus führt es wichtig, dass die Parameternamen in der ': SqlDataSource s `DeleteCommand` auf festgelegt ist `@ProductID`. Wenn die `DataKeyNames` Eigenschaft nicht festgelegt ist, oder der Parameter ist nicht mit dem Namen `@ProductsID`, auf die Schaltfläche "löschen" führt dazu, dass einen Postback, aber erzielter t tatsächlich ein Datensatz gelöscht.


Abbildung 5 sind diese Interaktion grafisch dargestellt. Verweisen auf die [untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) für eine ausführlichere Erläuterung in der Kette der Ereignisse im Zusammenhang mit einfügen, aktualisieren und löschen aus einem Websteuerelement Tutorial.


![Klicken auf die Schaltfläche "löschen" in der GridView Ruft die SqlDataSource-s-Delete()-Methode](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Abbildung 5**: auf die Schaltfläche "löschen" in der GridView Ruft die SqlDataSource-s `Delete()` Methode


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Schritt 2: Automatisch generieren die`INSERT`,`UPDATE`, und`DELETE`Anweisungen

Schritt 1 überprüft `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen können über das Eigenschaftenfenster oder das Steuerelement s deklarative Syntax angegeben werden. Dieser Ansatz erfordert jedoch, dass es manuell der SQL-Anweisungen manuell schreiben die monotonen und fehleranfällig sein kann. Glücklicherweise bietet der Assistent zum Konfigurieren der Datenquelle eine Option aus, damit die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, die automatisch generiert, wenn die Spalten angeben, aus einer Tabelle mit dem Bildschirm "Ansicht" verwenden.

Lassen Sie s untersuchen diese Option für die automatische Generierung. Hinzufügen eine DetailsView in den Designer in `InsertUpdateDelete.aspx` und legen Sie dessen `ID` Eigenschaft `ManageProducts`. Wählen Sie anschließend aus dem DetailsView s smart Tag, um eine neue Datenquelle erstellen, und erstellen eine mit dem Namen ': SqlDataSource `ManageProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen ManageProductsDataSource ': SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Abbildung 6**: Erstellen einer neuen SqlDataSource namens `ManageProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


Aus dem Konfigurieren von Datenquellen-Assistenten verwenden Sie wahlweise die `NORTHWINDConnectionString` Verbindung Zeichenfolge ein, und klicken Sie auf Weiter. Lassen Sie aus dem Bildschirm Select-Anweisung Konfigurieren der Spalten angeben, über ein Optionsfeld Tabelle oder Sicht ausgewählt haben, und wählen Sie die `Products` Tabelle aus der Dropdown-Liste. Wählen Sie die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten aus der Liste das Kontrollkästchen.


[![Verwenden die Products-Tabelle, ProductID, ProductName, UnitPrice und die nicht mehr unterstützte Spalten zurückgeben](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Abbildung 7**: Verwenden der `Products` Table, Zurückgeben der `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


So generieren Sie automatisch `INSERT`, `UPDATE`, und `DELETE` Anweisungen basierend auf der ausgewählten Tabelle und Spalten, klicken Sie auf die Schaltfläche "Erweitert", und überprüfen Sie die generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen.


![Überprüfen Sie die generieren INSERT, UPDATE und DELETE-Anweisungen Kontrollkästchen](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Abbildung 8**: Überprüfen der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen


Der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen wird nur überprüfbar sein, wenn die ausgewählte Tabelle über einen Primärschlüssel verfügt und die Primärschlüsselspalte (oder Spalten) in der Liste der zurückgegebenen Spalten enthalten sind. Kontrollkästchen vollständige Parallelität verwenden, die auswählbar wird einmal generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen Punkte überprüft wurden, werden erweitert die `WHERE` Klauseln in der resultierenden `UPDATE` und `DELETE` Anweisungen, die Steuerung durch vollständige Parallelität bereitzustellen. Lassen Sie dieses Kontrollkästchen deaktiviert; in den nächsten Lernprogrammen untersuchen wir die vollständigen Parallelität mit SqlDataSource-Steuerelement.

Nach der Überprüfung der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen deaktivieren, klicken Sie auf "OK", um die Select-Anweisung konfigurieren Bildschirm zurückzukehren, klicken Sie auf Weiter und dann auf Fertig stellen, um die Datenquelle konfigurieren-Assistenten zu beenden. Nach Abschluss des Assistenten für Visual Studio BoundFields, DetailsView für fügen die `ProductID`, `ProductName`, und `UnitPrice` Spalten und eine CheckBoxField für die `Discontinued` Spalte. Aus den DetailsView s smart Tag das Kontrollkästchen Sie Paging aktivieren, damit der Benutzer Zugriff auf dieser Seite die Produkte durchlaufen kann. Auch löschen, DetailsView s `Width` und `Height` Eigenschaften.

Beachten Sie, dass das Smarttag der verfügbaren Optionen aktivieren einfügen, aktivieren Sie bearbeiten und löschen aktivieren. Dies ist, da die ': SqlDataSource für Werte enthält seine `InsertCommand`, `UpdateCommand`, und `DeleteCommand`, wie die folgende deklarative Syntax gezeigt:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Beachten Sie, wie die SqlDataSource-Steuerelement automatisch für die festgelegten Werte wurde die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Die Anzahl der Spalten, die auf die verwiesen wird der `InsertCommand` und `UpdateCommand` Eigenschaften basieren auf den in der `SELECT` Anweisung. Also anstatt *jeder* Produkte-Spalte in der `InsertCommand` und `UpdateCommand`, es werden nur die Spalten, die im angegebenen der `SelectCommand` (weniger `ProductID`, dem wird ausgelassen, da es s ein [ `IDENTITY` Spalte](http://www.sqlteam.com/item.asp?ItemID=102), dessen Wert nicht geändert werden, bearbeitet und beim Einfügen automatisch zugewiesen). Darüber hinaus für jeden Parameter in der `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften stehen die entsprechenden Parameter in der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen.

Um die DetailsView s Datenänderung Funktionen zu aktivieren, überprüfen Sie die einfügen aktivieren, Bearbeiten aktivieren und löschen aktivieren von Optionen auf das Smarttag. Dadurch wird eine CommandField mit seiner `ShowInsertButton`, `ShowEditButton`, und `ShowDeleteButton` Eigenschaften festlegen, um `True`.

Besuchen Sie die Seite in einem Browser, und notieren Sie sich die bearbeiten, löschen und neue Schaltflächen, die in der DetailsView enthalten. Klicken auf die Schaltfläche "Bearbeiten", DetailsView in den Bearbeitungsmodus wechseln, in dem jede BoundField angezeigt aktiviert, deren `ReadOnly` -Eigenschaftensatz auf `False` (Standard) als ein Textfeld, und die CheckBoxField als ein Kontrollkästchen.


[![Die DetailsView s Standard bearbeiten-Schnittstelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Abbildung 9**: The, DetailsView-s-Standard bearbeiten-Schnittstelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


Auf ähnliche Weise können Sie das aktuell ausgewählte Produkt löschen oder Hinzufügen eines neuen Produkts mit dem System. Da die `InsertCommand` Anweisung funktioniert nur mit der `ProductName`, `UnitPrice`, und `Discontinued` Spalten, die anderen Spalten haben entweder `NULL` oder gemäß dem Standardwert zugewiesen werden, indem Sie die Datenbank beim Einfügen. Ebenso wie mit der ObjectDataSource, wenn die `InsertCommand` fehlt keiner Datenbanktabelle, die Spalten, die von t Verschlüsselungskennwort zulassen `NULL` s und Don ' t über einen Standardwert verfügen, erfolgt ein SQL-Fehler beim Ausführen der `INSERT` Anweisung.

> [!NOTE]
> Die DetailsView s einfügen und Bearbeiten von Schnittstellen fehlender jegliche Art von Anpassung oder Überprüfung. Validierungssteuerelemente hinzufügen oder die Schnittstellen anzupassen, müssen Sie die BoundFields in von TemplateFields zu konvertieren. Finden Sie in der [Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) und [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Lernprogramme für Weitere Informationen.


Beachten Sie, dass zum Aktualisieren und löschen, DetailsView das aktuelle Produkt s verwendet außerdem, dass `DataKey` Wert, der nur vorhanden ist, wenn die `DataKeyNames` Eigenschaft so konfiguriert ist. Wenn angezeigt wird, keine Auswirkungen, bearbeiten oder löschen, stellen Sie sicher, dass die `DataKeyNames` festgelegt wird.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Einschränkungen für das automatische Generieren von SQL-Anweisungen

Seit der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen-Option ist nur verfügbar, wenn der Entnahme von Spalten aus einer Tabelle je komplexer Abfragen Sie benötigen ein eigenes schreiben `INSERT`, `UPDATE`, und `DELETE` Anweisungen, wie in Schritt 1. Häufig, SQL `SELECT` -Anweisungen `JOIN` s, um Daten aus einem oder mehreren Nachschlagetabellen für Anzeigezwecke wiederherzustellen (z. B. wieder Zurückholen der `Categories` Tabelle s `CategoryName` Feld beim Anzeigen von Produktinformationen). Zur gleichen Zeit Being ermöglicht dem Benutzer bearbeiten, aktualisieren oder Einfügen von Daten in der Tabelle Core (`Products`, in diesem Fall).

Während der `INSERT`, `UPDATE`, und `DELETE` Anweisungen können manuell eingegeben werden, sollten Sie die folgenden sparen Sie Zeit. Setup zunächst die SqlDataSource-, damit sie wieder Daten nur Abrufen der `Products` Tabelle. Verwenden Sie die Spalten aus einer Tabelle oder Sicht Bildschirm Konfigurieren von Datenquellen-Assistenten s angeben, damit Sie automatisch generieren, können die `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Wählen Sie dann nach Abschluss des Assistenten zum Konfigurieren der SelectQuery über das Eigenschaftenfenster (oder alternativ, wechseln Sie zurück an den Assistent zum Konfigurieren von Datenquellen, aber verwenden, geben Sie eine benutzerdefinierte SQL­Anweisung oder gespeicherte Prozedur-Option). Aktualisieren Sie dann die `SELECT` -Anweisung zum Einschließen der `JOIN` Syntax. Diese Technik bietet die Vorteile Zeit sparen, der automatisch generierten SQL-Anweisungen und ermöglicht, einer stärker angepassten `SELECT` Anweisung.

Eine weitere Einschränkung der automatischen farbgenerierung die `INSERT`, `UPDATE`, und `DELETE` Anweisungen besteht, die aus den Spalten in der `INSERT` und `UPDATE` Anweisungen werden basierend auf den Spalten, die zurückgegeben werden, indem die `SELECT` Anweisung. Wir müssen möglicherweise aktualisieren oder Einfügen von jedoch mehr oder weniger Felder. Im Beispiel aus Schritt2, vielleicht wir z. B. haben die `UnitPrice` BoundField schreibgeschützt sein. In diesem Fall es t sollte nicht angezeigt werden, der `UpdateCommand`. Oder wir möchten legen Sie den Wert eines Tabellenfelds, die nicht angezeigt wird, in der GridView. Z. B. das Hinzufügen einer neuen Datensatz möglicherweise möchten wir die `QuantityPerUnit` Wert auf TODO festgelegt.

Wenn solche Anpassungen erforderlich sind, müssen Sie sie manuell, entweder über das Eigenschaftenfenster, das benutzerdefinierte SQL-Anweisung angeben oder die gespeicherte Prozedur-Option im Assistenten oder über die deklarative Syntax machen.

> [!NOTE]
> Beim Hinzufügen von Parametern, die nicht über die entsprechenden Felder in den Daten verfügen Steuerelement Web-, sollten Sie bedenken, die diese Parameterwerte müssen Werte auf eine Weise zugewiesen werden. Diese Werte sind möglich: hartcodierte direkt in die `InsertCommand` oder `UpdateCommand`; können aus einer vordefinierten Quelle (die Abfragezeichenfolge, Sitzungszustand, Websteuerelemente auf der Seite "usw.); stammen oder können programmgesteuert zugewiesen werden, wie wir gesehen, in dem vorherigen Lernprogramm haben.


## <a name="summary"></a>Zusammenfassung

Damit die Daten, dass websteuerungselemente, um ihre integrierte einfügen, bearbeiten und Löschen von Funktionen zu nutzen muss das Datenquellensteuerelement an das, dem Sie gebunden sind, solche Funktionen bieten. Für die SqlDataSource bedeutet dies, dass `INSERT`, `UPDATE`, und `DELETE` SQL-Anweisungen müssen zugewiesen werden, die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Diese Eigenschaften und die entsprechenden Parameter-Auflistungen werden manuell hinzugefügt oder über das Konfigurieren von Datenquellen-Assistenten automatisch generiert. In diesem Lernprogramm untersucht wir beide Verfahren.

ObjectDataSource vollständige Parallelität mit untersucht die [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Lernprogramm. SqlDataSource-Steuerelement unterstützt außerdem optimistische Parallelität. Wie in Schritt2 notiert haben, automatisch der Generierung der `INSERT`, `UPDATE`, und `DELETE` -Anweisungen, die der Assistent bietet einer Option zum Verwenden einer Verletzung der vollständigen Parallelität. Wie wir im nächsten Lernprogramm sehen werden, ändert die SqlDataSource vollständige Parallelität mit der `WHERE` Klauseln in der `UPDATE` und `DELETE` Anweisungen, um sicherzustellen, dass die Werte für die anderen Spalten geändert werden unklar, da die Daten entsprechend letzten seiner t auf der Seite angezeigt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Weiter](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
