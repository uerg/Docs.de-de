---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Verwenden von TemplateFields in des GridView-Steuerelements (c#) | Microsoft Docs
author: rick-anderson
description: "Um die Flexibilität, bietet die GridView der TemplateField, die mithilfe einer Vorlage rendert. Eine Vorlage kann eine Mischung aus statischem HTML-Code, Web Controls, einschließen und..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 004f1450937cc6543cb728e01586e3c3529a57d0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Verwenden von TemplateFields in des GridView-Steuerelements (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) oder [PDF herunterladen](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Um die Flexibilität, bietet die GridView der TemplateField, die mithilfe einer Vorlage rendert. Eine Vorlage kann eine Mischung aus einschließen Web statische HTML-Steuerelemente und Databinding-Syntax. In diesem Lernprogramm untersuchen wir wie die TemplateField zu verwenden, um eine bessere Anpassung mithilfe des GridView-Steuerelements zu erzielen.


## <a name="introduction"></a>Einführung

Die GridView besteht aus einem Satz von Feldern, die angeben, welche Eigenschaften aus der `DataSource` eingeschlossen werden sollen, in der gerenderten Ausgabe zusammen mit, wie die Daten angezeigt werden sollen. Der einfachste Typ des Felds ist der BoundField, das einen Datenwert als Text anzeigt. Andere Feldtypen anzuzeigen, die Daten mit anderen HTML-Elementen. Die CheckBoxField stellt z. B. als ein Kontrollkästchen, die den Wert eines angegebenen Felds, dessen Aktivierungszustand abhängig; die ImageField rendert ein Bild, deren Bildquelle auf ein Feld für die angegebenen Daten basiert. Links und Schaltflächen, deren Zustand auf einen zugrunde liegenden Datenfeldwert abhängt können die Feldtypen HyperLinkField und ButtonField gerendert werden.

Während die Feldtypen CheckBoxField, ImageField HyperLinkField und ButtonField eine alternative Ansicht der Daten zu ermöglichen, sind sie immer noch relativ beschränkt, in Bezug auf die Formatierung. Eine CheckBoxField kann nur einem einzigen Kontrollkästchen angezeigt, während ein ImageField nur ein einziges Bild angezeigt werden kann. Was geschieht, wenn ein bestimmtes Feld muss zum Anzeigen von Text, ein Kontrollkästchen *und* ein Bild, das alle basierend auf unterschiedliche Feldwerte? Oder was geschieht, wenn die Daten mithilfe eines Webserver-Steuerelements als das Kontrollkästchen, Bild, einen Link oder Schaltfläche angezeigt werden sollten? Darüber hinaus schränkt die BoundField seine Anzeige für eine einzelnes Datenfeld. Was geschieht, wenn wir soll demonstriert werden, zwei oder mehr Datenfeldwerte in einer einzelnen GridView-Spalte?

Um dieses Maß an Flexibilität zu berücksichtigen GridView bietet die TemplateField, die mit rendert eine *Vorlage*. Eine Vorlage kann eine Mischung aus einschließen Web statische HTML-Steuerelemente und Databinding-Syntax. Darüber hinaus hat das TemplateField eine Vielzahl von Vorlagen, die verwendet werden kann, um das Rendering für verschiedene Situationen anzupassen. Z. B. die `ItemTemplate` wird standardmäßig verwendet, um die Zelle für jede Zeile zu rendern, aber die `EditItemTemplate` Vorlage zum Anpassen der Benutzeroberfläche, beim Bearbeiten von Daten verwendet werden kann.

In diesem Lernprogramm untersuchen wir wie die TemplateField zu verwenden, um eine bessere Anpassung mithilfe des GridView-Steuerelements zu erzielen. In der [vorherigen Lernprogramm](custom-formatting-based-upon-data-cs.md) wurde erläutert, wie zum Anpassen der Formatierung basierend auf der zugrunde liegenden Daten mithilfe der `DataBound` und `RowDataBound` -Ereignishandler. Eine weitere Möglichkeit zum Anpassen der Formatierung basierend auf den zugrunde liegenden Daten wird durch Aufrufen von Methoden in einer Vorlage formatieren. Sehen wir uns diese Technik in diesem Lernprogramm auch.

Für dieses Lernprogramm verwenden wir von TemplateFields, um die Darstellung einer Liste von Mitarbeitern anzupassen. Insbesondere der wir müssen Auflisten aller Mitarbeiter, sondern zeigt des Vorgesetzten des Mitarbeiters vor- und Nachnamen Namen in einer Spalte, deren Einstellungsdatum in einem Monatskalender-Steuerelement, und eine Statusspalte, der angibt, wie viele Tage sie in der Firma beschäftigt haben.


[![Drei von TemplateFields werden verwendet, um die Anzeige anpassen](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Abbildung 1**: drei von TemplateFields werden verwendet, um die Anzeige anpassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Schritt 1: Binden von Daten an die GridView

In Szenarien, in denen Sie von TemplateFields verwenden, um die Darstellung anzupassen reporting: Suchen von es am einfachsten zu beginnen, indem ein GridView-Steuerelement, das nur BoundFields zuvor enthält erstellen und dann zum Hinzufügen von neuen TemplateFields oder konvertieren die vorhandenen BoundFields an Von TemplateFields nach Bedarf. Daher beginnen in diesem Lernprogramm von der Seite mithilfe des Designers eine GridView hinzufügen und binden es an einem ObjectDataSource, die die Liste der Mitarbeiter zurückgibt. Diese Schritte erstellt eine GridView BoundFields für jedes der Felder Employee.

Öffnen der `GridViewTemplateField.aspx` Seite und eine GridView aus der Toolbox in den Designer ziehen. Wählen Sie die GridView Smarttag für die ein neues ObjectDataSource-Steuerelement hinzuzufügen, die aufruft der `EmployeesBLL` Klasse `GetEmployees()` Methode.


[![Fügen Sie ein neues ObjectDataSource-Steuerelement, das die GetEmployees()-Methode aufruft](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie ein neues ObjectDataSource-Steuerelement, Invokes die `GetEmployees()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Binden des GridView auf diese Weise wird automatisch ein BoundField für jeden Mitarbeiter Eigenschaften hinzugefügt: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, und `Country`. Für diesen Bericht wir nicht kümmern Anzeigen der `EmployeeID`, `ReportsTo`, oder `Country` Eigenschaften. So entfernen Sie diese BoundFields können Sie folgende Aktionen ausführen:

- Verwenden Sie im Dialogfeld "Felder" auf auf den Link "Spalten bearbeiten" aus der GridView Smarttag, um dieses Dialogfeld zu öffnen. Als Nächstes die Schaltfläche wählen die BoundFields aus der unteren linken Ecke aus, und klicken Sie auf das rote X die BoundField entfernen.
- Bearbeiten Sie die GridView deklarativen Syntax manuell aus der Datenquellensicht an, löschen Sie die `<asp:BoundField>` -Element für die BoundField, die Sie entfernen möchten.

Nachdem Sie entfernt haben die `EmployeeID`, `ReportsTo`, und `Country` BoundFields, Ihre GridView-Markup sollte wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser anzuzeigen. Nun sehen Sie eine Tabelle mit einem Datensatz für jeden Mitarbeiter und vier Spalten: eine für des Vorgesetzten des Mitarbeiters Nachnamen, eine für ihren Vornamen, einen für ihre Titel und eine Einstellungsdatum.


[![Die LastName, FirstName, Titel und HireDate-Felder werden für jede Mitarbeiter angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Abbildung 3**: die `LastName`, `FirstName`, `Title`, und `HireDate` Felder werden für jede Mitarbeiter angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Schritt 2: Die ersten und letzten Namen in einer einzelnen Spalte anzeigen

Jeder Mitarbeiter derzeit dem vor- und Nachnamen in einer separaten Spalte angezeigt werden. Es kann zu einer einzelnen Spalte zu kombinieren, die stattdessen nice sein. Um dies zu erreichen, müssen wir ein TemplateField verwenden. Wir können entweder eine neue TemplateField hinzufügen, fügen Sie die erforderlichen Markup und die Databinding-Syntax hinzu und löschen Sie dann die `FirstName` und `LastName` BoundFields oder wir können konvertieren die `FirstName` BoundField in ein TemplateField Bearbeiten der TemplateField einschließen die `LastName` Wert ein, und entfernen Sie die `LastName` BoundField.

Beide Ansätze net dasselbe Ergebnis, aber persönlich ich möchte, konvertieren BoundFields in von TemplateFields, wenn möglich, da die Konvertierung automatisch hinzugefügt ein `ItemTemplate` und `EditItemTemplate` mit Websteuerelementen und Databinding-Syntax, die Darstellung zu imitieren. und die Funktionalität von der BoundField. Der Vorteil ist, müssen wir jedoch weniger Arbeitsschritte mit dem TemplateField wie der Konvertierungsprozess Teil der Arbeit für uns ausgeführt haben wird.

Zum Konvertieren einer vorhandenen BoundField in ein TemplateField klicken Sie auf den Link Bearbeiten von Spalten aus der GridView Smarttag, schalten Sie die Felder (Dialogfeld). Wählen Sie die BoundField aus der Liste in der unteren linken Ecke umwandeln, und klicken Sie dann auf den Link "Convert dieses Feld in ein TemplateField" in der unteren rechten Ecke.


[![Konvertieren einer BoundField in ein TemplateField aus der Felder (Dialogfeld)](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Abbildung 4**: Konvertieren von BoundField-in ein TemplateField aus der Felder (Dialogfeld) ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Fahren Sie fort, und konvertieren Sie die `FirstName` BoundField in ein TemplateField. Nach dieser Änderung besteht kein perceptive Unterschied im Designer zur Verfügung. Dies ist die BoundField in ein TemplateField konvertieren einer TemplateField erstellt werden, der das Aussehen und Verhalten von der BoundField verwaltet. Trotz keinen visuellen Unterschied zu diesem Zeitpunkt im Designer, wurde im Rahmen dieses Konvertierungsprozesses ersetzt die BoundField-deklarativen Syntax - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` – mit der folgenden TemplateField-Syntax:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Wie Sie sehen können, besteht die TemplateField zwei Vorlagen für eine `ItemTemplate` hat, die die Bezeichnung, deren `Text` auf den Wert der Eigenschaft festgelegt ist die `FirstName` Feld "Daten", und ein `EditItemTemplate` mit einem Textfeld-Steuerelement, dessen `Text` -Eigenschaft ebenfalls festgelegt wird um die `FirstName` Feld "Daten". Die Syntax der Databinding - `<%# Bind("fieldName") %>` -gibt an, dass das Feld "Daten"  *`fieldName`*  an die angegebene Eigenschaft des Web-Steuerelement gebunden ist.

Hinzufügen der `LastName` Datenfeld der Spalte Wert, der diese TemplateField wir eine andere Bezeichnung Websteuerelement in müssen der `ItemTemplate` und Binden der `Text` Eigenschaft, um `LastName`. Dies kann entweder manuell oder mithilfe des Designers erreicht werden. Zu diesem Zweck per hand katalogisiert wird einfach die entsprechende deklarative Syntax zum Hinzufügen der `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Um sie durch den Designer hinzuzufügen, klicken Sie auf auf den Link Bearbeiten von Vorlagen aus der GridView Smarttag. Dadurch wird die GridView Vorlage bearbeiten Schnittstelle angezeigt. In dieser Schnittstelle Smarttag ist eine Liste der Vorlagen in die GridView. Da wir eine TemplateField nur an diesem Punkt verfügen, die nur in der Dropdown-Liste aufgeführt sind die Vorlagen für die `FirstName` TemplateField zusammen mit den `EmptyDataTemplate` und `PagerTemplate`. Die `EmptyDataTemplate` Vorlage, wenn angegeben, wird verwendet, um die GridView-Ausgabe zu rendern, wenn es keine Ergebnisse in den Daten, die an die GridView; gebunden sind die `PagerTemplate`, sofern angegeben, wird verwendet, um die Paging-Schnittstelle für eine GridView gerendert werden sollen, die Paging unterstützt.


[![Die GridView Vorlagen können durch den Designer bearbeitet werden](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Abbildung 5**: die GridView Vorlagen können werden bearbeitet über der Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Auch Anzeigen der `LastName` in der `FirstName` TemplateField ziehen Sie das Label-Steuerelement aus der Toolbox in die `FirstName` TemplateFields `ItemTemplate` in die GridView des vorlagenbearbeitung Schnittstelle.


[![Die FirstName TemplateField ItemTemplate ein Websteuerelement Bezeichnung hinzufügen](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Abbildung 6**: Fügen Sie ein Websteuerelement Bezeichnung, um die `FirstName` TemplateFields ItemTemplate ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


Das Label-Websteuerelement hinzugefügt, um die TemplateField an diesem Punkt verfügt ihre `Text` -Eigenschaft auf "Label" festgelegt. Wir müssen geändert werden, damit diese Eigenschaft gebunden ist, auf den Wert von der `LastName` Datenfeld der Spalte stattdessen. Klicken Sie auf das Label-Steuerelement smart Tag zu erreichen, und wählen die Option-Datenbindungen bearbeiten.


[![Wählen Sie die bearbeiten-DataBindings-Option die Bezeichnung Smarttag](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Abbildung 7**: Wählen Sie die Option zum Bearbeiten DataBindings aus Smarttag für der Bezeichnung ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Hierdurch wird das Dialogfeld "DataBindings" angezeigt. Von hier aus können Sie die Eigenschaft teilnehmen Databinding aus der Liste auf der linken Seite und wählen das Feld zum Binden der Daten aus der Dropdown-Liste auf der rechten Seite auswählen. Wählen Sie die `Text` Eigenschaft aus der linken Seite und die `LastName` Feld aus der rechten Seite, und klicken Sie auf OK.


[![Binden Sie die Texteigenschaft auf das Feld LastName-Daten](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Abbildung 8**: Binden der `Text` Eigenschaft, um die `LastName` Feld "Daten" ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Das Dialogfeld "DataBindings" können Sie angeben, ob die bidirektionale Datenbindung ausführen. Wenn Sie dieses Kontrollkästchen deaktiviert ist, lassen die Databinding-Syntax `<%# Eval("LastName")%>` verwendet werden, anstelle von `<%# Bind("LastName")%>`. Für dieses Lernprogramm ist in beiden Ansätze Ordnung. Bidirektionale Datenbindung ist wichtig, beim Einfügen und Bearbeiten von Daten. Zum Anzeigen der Daten einfach, allerdings funktionieren beide Vorgehensweisen ebenso gut. In zukünftigen Lernprogrammen erläutert bidirektionale Datenbindung im Detail.


Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Wie Sie sehen, umfasst die GridView weiterhin vier Spalten. allerdings die `FirstName` Spalte nun aufgeführt *beide* der `FirstName` und `LastName` Datenfeld der Spalte Werte.


[![Sowohl das FirstName und LastName-Werte werden in einer einzelnen Spalte angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Abbildung 9**: sowohl der `FirstName` und `LastName` Werte werden in einer einzelnen Spalte angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Um diesem ersten Schritt abgeschlossen haben, entfernen Sie die `LastName` BoundField und benennen Sie die `FirstName` TemplateFields `HeaderText` Eigenschaft "Name". Nach der Änderung sollte die GridView deklarative Markup wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![First und den Nachnamen des Mitarbeiters werden in einem Columnstore angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Abbildung 10**: ersten und den Nachnamen jedes Mitarbeiters werden in einem Columnstore angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Schritt 3: Verwenden das Kalendersteuerelement mit Anzeige der`HiredDate`Feld

Anzeigen von ein Datenfeldwert als Text in einem GridView ist so einfach wie mit einem BoundField. Für bestimmte Szenarien ist jedoch die Daten am besten ausgedrückt mit einem bestimmten Websteuerelement anstelle von nur-Text. Diese Anpassung der Anzeige der Daten ist mit von TemplateFields möglich. Z. B. stattdessen als Einstellungsdatum der Mitarbeiter als Text angezeigt werden, wir konnte einen Kalender anzeigen (mit [das Kalendersteuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) mit Einstellungsdatum hervorgehoben.

Um dies zu erreichen, starten Sie durch Konvertieren der `HiredDate` BoundField in ein TemplateField. Klicken Sie einfach an die GridView Smarttag wechseln Sie, und klicken Sie auf den Link Spalten bearbeiten, wie Sie die Felder (Dialogfeld). Wählen Sie die `HiredDate` BoundField- und klicken Sie auf "Konvertieren dieses Feld in ein TemplateField."


[![Die HiredDate BoundField-in ein TemplateField konvertieren](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Abbildung 11**: Konvertieren der `HiredDate` BoundField-in TemplateField ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Wie in Schritt2 veranschaulichte Filtermenü, ersetzt dieses die BoundField durch ein TemplateField, die enthält ein `ItemTemplate` und `EditItemTemplate` mit eine Bezeichnung und ein Textfeld, deren `Text` Eigenschaften gebunden sind, um die `HiredDate` Wert mit der Syntax Databinding `<%# Bind("HiredDate")%>`.

Um den Text durch Calendar-Steuerelement zu ersetzen, müssen bearbeiten Sie die Vorlage durch die Bezeichnung entfernen und das Hinzufügen eines Kalendersteuerelements. Vom Designer, wählen Sie aus der GridView Smarttag Vorlagen bearbeiten, und wählen Sie die `HireDate` TemplateFields `ItemTemplate` aus der Dropdown-Liste. Klicken Sie dann löschen Sie das Label-Steuerelement, und ziehen Sie ein Monatskalender-Steuerelement aus der Toolbox in die Vorlage bearbeiten-Schnittstelle.


[![Hinzufügen ein Kalendersteuerelements an die HireDate TemplateField ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Abbildung 12**: Hinzufügen eines Kalendersteuerelements an die `HireDate` TemplateFields `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


Zu diesem Zeitpunkt enthält jede Zeile in der GridView ein Monatskalender-Steuerelement in seiner `HiredDate` TemplateField. Allerdings die Mitarbeiter des tatsächlichen `HiredDate` Wert nicht an einer beliebigen Stelle in der Calendar-Steuerelement, verursacht jeder Monatskalender-Steuerelement an Standardeinstellung besteht im Anzeigen der aktuellen Monat und Tag festgelegt. Zur Behebung des Problems, müssen wir weisen Sie jedes Mitarbeiters `HiredDate` auf des Kalender-Steuerelements ["SelectedDate"](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) und [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) Eigenschaften.

Wählen Sie das Kalendersteuerelement Smarttag-Datenbindungen bearbeiten. Anschließend binden Sie beide `SelectedDate` und `VisibleDate` Eigenschaften der `HiredDate` Feld "Daten".


[![Binden Sie "SelectedDate" und VisibleDate Eigenschaften zum HiredDate Datenfeld](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Abbildung 13**: Binden der `SelectedDate` und `VisibleDate` Eigenschaften der `HiredDate` Feld "Daten" ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Das Kalender-Steuerelement ausgewählte Datum muss nicht unbedingt sichtbar werden. Z. B. möglicherweise ein Kalender 1. August<sup>St</sup>, 1999 als das ausgewählte Datum im aktuellen Monat und Jahr angezeigt werden. Das ausgewählte Datum und die sichtbare Datum werden angegeben, indem des Kalendersteuerelement `SelectedDate` und `VisibleDate` Eigenschaften. Da beide Wählen des Vorgesetzten des Mitarbeiters soll `HiredDate` und stellen Sie sicher, dass er angezeigt wird, müssen wir beide Eigenschaften zum Binden der `HireDate` Feld "Daten".


Wenn Sie die Seite in einem Browser anzeigen, wird der Kalender jetzt zeigt den Monat des Einstellungsdatum der Mitarbeiter und wählt diese bestimmte Datum.


[![Den Vorgesetzten des Mitarbeiters HiredDate wird in der Calendar-Steuerelement angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Abbildung 14**: der Mitarbeiter `HiredDate` in das Kalender-Steuerelement angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Im Gegensatz zur allen Beispielen bisher gelernt haben, für dieses Lernprogramm haben wir *nicht* festgelegt `EnableViewState` Eigenschaft `false` für diese GridView. Der Grund für diese Entscheidung ist auf die Daten der Calendar-Steuerelement ein Postback handeln, für die Einstellung ausgewählten Datums im Kalender das Datums, geklickt verursacht. Wenn die GridView Ansichtszustand deaktiviert ist, jedoch bei jedem Postback die GridView-Daten werden neu mit der zugrunde liegenden Datenquelle, wodurch ausgewählten Kalenderdatum festgelegt werden *wieder* , die des Mitarbeiters `HireDate`, überschreiben das Datum, die vom Benutzer ausgewählt wurde.


Für dieses Lernprogramm ist es sich hierbei um eine Diskussion hinfällig, da der Benutzer nicht zum Aktualisieren des Mitarbeiters kann `HireDate`. Es wäre wahrscheinlich am besten das Kalender-Steuerelement so konfigurieren, dass seine Daten nicht ausgewählt werden können. Unabhängig davon, zeigt das Lernprogramm an, dass unter bestimmten Umständen Ansichtszustand aktiviert werden muss, um bestimmte Funktionen bereitzustellen.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Schritt 4: Zeigt der Anzahl der Tage des Mitarbeiters des Unternehmens funktioniert hat

Bisher haben wir zwei Anwendungen von TemplateFields angezeigt:

- Kombinieren von zwei oder mehr Datenfeldwerte in einer einzelnen Spalte und
- Einen Datenfeldwert ein Websteuerelement anstelle von Text mit Ausdrücken

Eine dritte Verwendung von von TemplateFields Anzeigen von Metadaten über der GridView Daten zugrunde liegt. Zusätzlich zum Anzeigen der Mitarbeiter mitarbeiteridentifikationsnummern, z. B. möchten wir auch eine Spalte aufweisen, die Gesamtanzahl von wie vielen Tagen zeigt sie auf den Auftrag wurden.

Noch entsteht eine weitere Verwendungsmöglichkeit der von TemplateFields in Szenarien, wenn die zugrunde liegenden Daten müssen in der Webseitenbericht unterschiedlich angezeigt werden, als im Format in der Datenbank gespeichert ist. Stellen Sie sich vor, die die `Employees` Tabelle musste ein `Gender` Feld, das das Zeichen gespeichert `M` oder `F` an, dass das Geschlecht des Mitarbeiters. Wenn diese Informationen auf einer Webseite anzeigen, um das Geschlecht anzeigen, als "Männlich" oder "Weiblich", statt nur "M" oder "F möchten".

Beide Szenarien können verarbeitet werden, durch das Erstellen einer *Formatierungsmethode* in der ASP.NET-Seite Code-Behind-Klasse (oder in einer separaten Klassenbibliothek als implementiert eine `static` Methode), die aus der Vorlage aufgerufen wird. Solche eine Formatierungsmethode wird aus der Vorlage, die mit derselben Databinding-Syntax, die bereits aufgerufen. Formatierungsmethode kann in einer beliebigen Anzahl von Parametern, aber muss eine Zeichenfolge zurückgeben. Diese zurückgegebene Zeichenfolge ist der HTML-Code, der in die Vorlage eingefügt wird.

Um dieses Konzept zu veranschaulichen, ergänzen Sie wir unser Tutorial aus, um eine Spalte anzuzeigen, die die Gesamtanzahl der Tage angezeigt werden, die ein Mitarbeiter für den Auftrag wurde. Diese Formatierungsmethode gelangen eine `Northwind.EmployeesRow` Objekt, und die Anzahl der Tage, die der Mitarbeiter wurde, als Zeichenfolge eingesetzt hat zurück. Diese Methode kann auf der ASP.NET-Seite Code-Behind-Klasse hinzugefügt werden, aber *müssen* als gekennzeichnet werden `protected` oder `public` um aus der Vorlage zugegriffen werden.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Da die `HiredDate` Feld darf `NULL` Datenbank Werte, die wir zunächst sicherstellen müssen, dass der Wert nicht `NULL` vor dem Fortfahren mit der Berechnung. Wenn die `HiredDate` Wert `NULL`, die Zeichenfolge "Unknown"; wir einfach zurückzugeben, ist er nicht `NULL`, wir Berechnen der Differenz zwischen dem aktuellen Zeitpunkt und die `HiredDate` -Wert und gibt die Anzahl von Tagen zurück.

Diese Methode nutzen können, sie über ein TemplateField in die GridView unter Verwendung der Syntax Databinding aufrufen müssen. Starten Sie eine neue TemplateField an die GridView starten, indem Sie durch Klicken auf den Link "Spalten bearbeiten" in der GridView Smarttag und das Hinzufügen einer neuen TemplateField hinzufügen.


[![Hinzufügen einer neuen TemplateField an die GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Abbildung 15**: Hinzufügen einer neuen TemplateField an die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Legen Sie diese neue TemplateField `HeaderText` "Tage auf des Auftrags"-Eigenschaft und die zugehörige `ItemStyle`des `HorizontalAlign` Eigenschaft `Center`. Aufrufen der `DisplayDaysOnJob` Methode aus der Vorlage Hinzufügen einer `ItemTemplate` und verwenden Sie die folgenden Databinding-Syntax:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem`Gibt eine `DataRowView` , Objekt entspricht der `DataSource` Datensatz gebunden werden, um die `GridViewRow`. Die `Row` Eigenschaft gibt den stark typisierten `Northwind.EmployeesRow`, der übergeben wird, um die `DisplayDaysOnJob` Methode. Stehen diese Databinding-Syntax direkt in die `ItemTemplate` (wie in der folgenden deklarative Syntax gezeigt) oder zugewiesen werden kann, um die `Text` Eigenschaft von einem Label-Steuerelement.

> [!NOTE]
> Alternativ können Sie anstelle der Übergabe von einer `EmployeesRow` Instanz übergeben wir gerade die `HireDate` -Wert mit `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Allerdings die `Eval` Methode gibt ein `object`, sodass wir hätten so ändern Sie unsere `DisplayDaysOnJob` Methodensignatur akzeptieren einen Eingabeparameter vom Typ `object`, stattdessen. Wir können nicht ungeprüft umgewandelt der `Eval("HireDate")` aufrufen, um eine `DateTime` da die `HireDate` Spalte in der `Employees` Tabelle darf `NULL` Werte. Aus diesem Grund müssen wir akzeptieren ein `object` als Eingabeparameter für die `DisplayDaysOnJob` -Methode ist, überprüfen, um festzustellen, ob es sich um eine Datenbank wurde `NULL` Wert (dem können mithilfe von durchgeführt werden `Convert.IsDBNull(objectToCheck)`), und fahren Sie dann entsprechend.


Aufgrund dieser Besonderheiten ich haben sich entschieden, übergeben Sie in der gesamten `EmployeesRow` Instanz. In den nächsten Lernprogrammen sehen wir ein weitere Anpassung-Beispiel für die Verwendung der `Eval("columnName")` Syntax für einen Eingabeparameter in einer Formatierungsmethode übergeben.

Im folgenden wird die deklarative Syntax für unsere GridView gezeigt, nachdem die TemplateField hinzugefügt wurde und die `DisplayDaysOnJob` Methode mit dem Namen aus der `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Abbildung 16 zeigt das Lernprogramm abgeschlossene, wenn Sie über einen Browser angezeigt.


[![Die von Tagen für Anzahl der Mitarbeiter für den Auftrag wurde wird angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Abbildung 16**: die Anzahl von Tagen der Mitarbeiter wurde für den Auftrag angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Zusammenfassung

Die TemplateField in des GridView-Steuerelements ermöglicht ein höheres Maß an Flexibilität beim Anzeigen von Daten als mit den anderen Feldsteuerelemente verfügbar ist. Von TemplateFields eignen sich ideal für Situationen, in denen:

- Mehrere Datenfelder in einer GridView-Spalte angezeigt werden müssen
- Am besten ausgedrückt ein Websteuerelement statt nur-text
- Die Ausgabe hängt von der zugrunde liegenden Daten, z. B. das Anzeigen von Metadaten oder Formatieren von Daten

Zusätzlich zum Anpassen der Anzeige von Daten, werden von TemplateFields auch zum Anpassen von Benutzeroberflächen, die zum Bearbeiten und Einfügen von Daten, verwendet werden, wie es in zukünftigen Lernprogrammen sehen.

Die nächsten beiden Lernprogramme lernen Vorlagen, mit einem Blick auf das Verwenden von TemplateFields in einem DetailsView ab. Danach müssen wir auf die FormView aktivieren, das Vorlagen anstelle von Feldern verwendet werden, um größere Flexibilität beim Layout und Struktur der Daten zu ermöglichen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Dan Jagers. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](custom-formatting-based-upon-data-cs.md)
[Weiter](using-templatefields-in-the-detailsview-control-cs.md)
