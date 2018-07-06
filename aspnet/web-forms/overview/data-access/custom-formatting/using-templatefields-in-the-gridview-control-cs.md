---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Verwenden von TemplateFields im GridView-Steuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Um die Flexibilität zu gewährleisten, bietet die GridView das TemplateField, das gerendert wird, mithilfe einer Vorlage. Eine Vorlage kann eine Mischung aus statischen HTML-Daten, Websteuerelemente enthalten, und...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b01d2018d4164f8db7c86226f7f1d5999743e6c2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826701"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Verwenden von TemplateFields im GridView-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) oder [PDF-Datei herunterladen](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Um die Flexibilität zu gewährleisten, bietet die GridView das TemplateField, das gerendert wird, mithilfe einer Vorlage. Eine Vorlage kann enthalten eine Kombination von statischen HTML-Web-Steuerelemente und Databinding-Syntax. In diesem Tutorial betrachten wir, wie Sie das TemplateField verwenden, um einen höheren Grad der Anpassung mit GridView-Steuerelement zu erreichen.


## <a name="introduction"></a>Einführung

Das GridView besteht aus einem Satz von Feldern, die angeben, welche Eigenschaften aus der `DataSource` eingeschlossen werden sollen, in der gerenderten Ausgabe und wie die Daten angezeigt werden. Die einfachste Art des Felds ist der BoundField, die einen Datenwert als Text angezeigt wird. Andere Feldtypen zeigen die Daten, die mit anderen HTML-Elementen an. Die CheckBoxField stellt z. B. als ein Kontrollkästchen, dessen Aktivierungszustand vom Wert eines angegebenen Felds abhängt; die ImageField rendert das Bild, dessen Quelle ein Feld für die angegebenen Daten basiert. Links und Schaltflächen, deren Zustand auf einen zugrunde liegenden Wert im Datenfeld abhängt, können die Feldtypen HyperLinkField und ButtonField gerendert werden.

Die Feldtypen CheckBoxField, ImageField, HyperLinkField und ButtonField eine alternative Ansicht der Daten zu ermöglichen, sind immer noch relativ in Bezug auf die Formatierung begrenzt. Eine CheckBoxField kann nur einem einzigen Kontrollkästchen, anzeigen, während ein ImageField nur ein einzelnes Bild anzeigen kann. Was geschieht, wenn ein bestimmtes Feld muss zum Anzeigen von Text, ein Kontrollkästchen *und* ein Bild, das alle anhand von verschiedenen Datenfeldwerte? Oder was geschieht, wenn wir die Daten mithilfe eines Websteuerelements als das Kontrollkästchen, Image, links oder Schaltfläche anzeigen möchten? Darüber hinaus schränkt die BoundField um ein einzelnes Datenfeld anzuzeigen. Was geschieht, wenn wir die Feldwerte für zwei oder mehr Daten in einer einzelnen GridView-Spalte anzeigen möchten?

Um dieses Maß an Flexibilität zu ermöglichen. die GridView bietet das TemplateField, die mit rendert eine *Vorlage*. Eine Vorlage kann enthalten eine Kombination von statischen HTML-Web-Steuerelemente und Databinding-Syntax. Darüber hinaus hat das TemplateField eine Vielzahl von Vorlagen, die die Anpassung für verschiedene Situationen verwendet werden können. Z. B. die `ItemTemplate` verwendet standardmäßig die Zelle für jede Zeile, aber die `EditItemTemplate` Vorlage zum Anpassen von der-Schnittstelle beim Bearbeiten von Daten verwendet werden kann.

In diesem Tutorial betrachten wir, wie Sie das TemplateField verwenden, um einen höheren Grad der Anpassung mit GridView-Steuerelement zu erreichen. In der [vorherigen Lernprogramm](custom-formatting-based-upon-data-cs.md) erläutert, wie zur Anpassung der Formatierung basierend auf der zugrunde liegenden Daten mithilfe der `DataBound` und `RowDataBound` -Ereignishandler. Eine weitere Möglichkeit zur Anpassung der Formatierung basierend auf den zugrunde liegenden Daten wird durch Aufrufen von Methoden in einer Vorlage formatieren. Wir werden dieses Verfahren in diesem Tutorial auch betrachten.

In diesem Tutorial verwenden wir von TemplateFields anpassen die Darstellung einer Liste von Mitarbeitern. Insbesondere wir alle Mitarbeiter führen jedoch zeigt des Mitarbeiters ersten und letzten Namen in eine Spalte, deren Einstellungsdatum in einem Monatskalender-Steuerelement und eine Statusspalte, der angibt, wie viele Tage sie im Unternehmen entwickelt haben.


[![Drei von TemplateFields werden verwendet, um die Anzeige anpassen](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Abbildung 1**: drei von TemplateFields werden verwendet, um die Anzeige anpassen ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Schritt 1: Binden der Daten an die GridView

In Szenarien, in dem Sie von TemplateFields zu verwenden, um die Darstellung anpassen, müssen, reporting ich finde es am einfachsten, Sie beginnen mit dem ein GridView-Steuerelement, das nur BoundFields zuerst enthält erstellen und dann zum Hinzufügen von neuen von TemplateFields oder konvertieren die vorhandenen BoundFields auf Von TemplateFields, je nach Bedarf. Aus diesem Grund beginnen wir in diesem Tutorial durch Hinzufügen einer GridView-Ansicht auf der Seite mithilfe des Designers, und binden sie an einer ObjectDataSource gegeben, die die Liste der Mitarbeiter zurückgibt. Diese Schritte werden einer GridView-Ansicht mit BoundFields für jeden die Employee-Felder erstellt.

Öffnen der `GridViewTemplateField.aspx` Seite und einer GridView-Ansicht aus der Toolbox in den Designer ziehen. GridView Smarttag auswählen, um ein neues ObjectDataSource-Steuerelement hinzuzufügen, die aufruft, die `EmployeesBLL` Klasse `GetEmployees()` Methode.


[![Fügen Sie ein neues ObjectDataSource-Steuerelement, das die GetEmployees()-Methode aufruft](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie ein neues ObjectDataSource-Steuerelement, Invokes der `GetEmployees()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Binden des GridView auf diese Weise wird automatisch ein BoundField für jede der Eigenschaften der Mitarbeiter hinzugefügt: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, und `Country`. Für diesen Bericht lassen Sie uns nicht kümmern Sie sich Anzeigen der `EmployeeID`, `ReportsTo`, oder `Country` Eigenschaften. So entfernen Sie diese BoundFields können Sie folgende Aktionen ausführen:

- Verwenden Sie die Felder im Dialogfeld auf auf den Link "Spalten bearbeiten" aus den GridView Smarttag, um das Dialogfeld zu öffnen. Schaltfläche "Weiter," Wählen Sie die BoundFields aus der linken unteren Ecke aus, und klicken Sie auf das rote X, um die BoundField zu entfernen.
- Deklarativen Syntax des GridView aus der Datenquellensicht manuell zu bearbeiten, löschen Sie die `<asp:BoundField>` -Element für die BoundField, die Sie entfernen möchten.

Nachdem Sie entfernt haben die `EmployeeID`, `ReportsTo`, und `Country` BoundFields, sollte Ihre GridView Markup wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser anzuzeigen. An diesem Punkt sehen Sie eine Tabelle mit einem Eintrag für jeden Mitarbeiter und vier Spalten: eine für des Mitarbeiters Nachnamen, einen für ihren Vornamen, eine für ihre Titel und eine Einstellungsdatum.


[![Die LastName, FirstName, Titel und HireDate-Felder werden für jede Mitarbeiter angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Abbildung 3**: die `LastName`, `FirstName`, `Title`, und `HireDate` Felder für jedes Mitarbeiters angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Schritt 2: Die Anzeige der ersten und letzten Namen in einer einzelnen Spalte

Jeder Mitarbeiter derzeit den vor- und Nachnamen in einer separaten Spalte angezeigt werden. Gut, diese stattdessen in einer einzelnen Spalte zusammenfassen möglicherweise. Zu diesem Zweck müssen wir ein TemplateField verwenden. Wir können entweder ein neues TemplateField hinzufügen, fügen Sie das erforderliche Markup und die Databinding-Syntax hinzu und löschen Sie dann die `FirstName` und `LastName` BoundFields, oder wir können konvertieren die `FirstName` BoundField in ein TemplateField, bearbeiten Sie das TemplateField einschließen die `LastName` Wert ein, und entfernen Sie die `LastName` BoundField.

Beide Ansätze net das gleiche Ergebnis, aber persönlich ich gern BoundFields in von TemplateFields möglichst konvertiert werden, da es sich bei die Konvertierung automatisch hinzugefügt. eine `ItemTemplate` und `EditItemTemplate` mit Websteuerelementen und Databinding-Syntax, um die Darstellung zu imitieren. und die Funktionalität von der BoundField. Der Vorteil ist, müssen wir jedoch weniger Arbeitsschritte mit der TemplateField während des Konvertierungsvorgangs Teil der Arbeit für uns ausgeführt haben wird.

Um eine vorhandene BoundField in ein TemplateField konvertieren möchten, klicken Sie auf den Link "Spalten bearbeiten" aus den GridView Smarttag, schalten das Dialogfeld "Felder". Wählen Sie die BoundField konvertieren aus der Liste in der unteren linken Ecke, und klicken Sie auf den Link "Convert dieses Feld in ein TemplateField" in der unteren rechten Ecke aus.


[![Konvertieren einer BoundField in ein TemplateField aus der Felder (Dialogfeld)](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Abbildung 4**: ein BoundField-in ein TemplateField konvertieren, in die Felder (Dialogfeld) ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Fahren Sie fort, und konvertieren die `FirstName` BoundField in ein TemplateField. Nach dieser Änderung besteht kein scharfsinnigen Unterschied im Designer zur Verfügung. Dies ist die BoundField in ein TemplateField konvertieren ein TemplateField erstellt werden, der das Aussehen und Verhalten von der BoundField verwaltet. Trotz gibt es keine visuellen Unterschied an diesem Punkt im Designer, wurde diesem Konvertierungsprozess ersetzt die BoundField-s deklarativen Syntax - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` – mit der folgenden TemplateField-Syntax:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Wie Sie sehen können, das TemplateField umfasst zwei Vorlagen ein `ItemTemplate` hat, die die Bezeichnung, deren `Text` -Eigenschaftensatz auf den Wert der der `FirstName` Feld "Daten", und ein `EditItemTemplate` mit einem Textfeld-Steuerelement, dessen `Text` Eigenschaft wird auch festgelegt werden. um die `FirstName` Feld "Daten". Die Datenbindungssyntax - `<%# Bind("fieldName") %>` – gibt an, dass das Datenfeld *`fieldName`* an der angegebenen Eigenschaft für die Web-Steuerelements gebunden ist.

Hinzufügen der `LastName` Datenfeld Wert, der diese TemplateField, wir eine andere Bezeichnung Websteuerelement in müssen, der `ItemTemplate` und binden die `Text` Eigenschaft, um `LastName`. Dies kann entweder manuell oder mithilfe des Designers erreicht werden. Um per hand erledigt werden, einfach die entsprechenden deklarative Syntax zum Hinzufügen der `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Um sie über den Designer hinzuzufügen, klicken Sie auf die Vorlagen bearbeiten von GridView Smarttag. GridView vorlagenschnittstelle bearbeiten wird angezeigt. In dieser Schnittstelle Smarttag ist eine Liste der Vorlagen in den GridView-Ansicht. Da wir ein TemplateField nur an diesem Punkt verfügen, die nur in der Dropdown-Liste aufgeführt sind die Vorlagen für die `FirstName` TemplateField zusammen mit den `EmptyDataTemplate` und `PagerTemplate`. Die `EmptyDataTemplate` Vorlage, wenn angegeben, wird zum Rendern von GridView Ausgabe, wenn keine Ergebnisse, in den Daten, die an die GridView; gebunden vorliegen verwendet die `PagerTemplate`, sofern angegeben, wird verwendet, um die Paging-Schnittstelle für einer GridView-Ansicht zu rendern, der Pagingvorgänge unterstützt.


[![GridView Vorlagen können über den Designer bearbeitet werden](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Abbildung 5**: der GridView Vorlagen können werden bearbeitet über das Verwenden des Designers ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Auch Anzeigen der `LastName` in die `FirstName` TemplateField ziehen Sie das Label-Steuerelement aus der Toolbox in die `FirstName` TemplateFields `ItemTemplate` in den GridView-Ansicht der Vorlage bearbeiten-Schnittstelle.


[![Die FirstName TemplateFields ItemTemplate-Element ein Label-Steuerelement hinzugefügt](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen eines Websteuerelements Bezeichnung, um die `FirstName` TemplateFields ItemTemplate ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


Das Label-Steuerelement hinzugefügt, das TemplateField an diesem Punkt verfügt die `Text` -Eigenschaft auf "Label" festgelegt. Wir müssen geändert werden, damit diese Eigenschaft gebunden ist, auf den Wert des der `LastName` Daten stattdessen das Feld. Klicken Sie auf das Label-Steuerelement smart Tag zu erreichen, und wählen Sie die Option DataBindings bearbeiten.


[![Wählen Sie die Option "DataBindings bearbeiten" aus der Bezeichnung Smarttag](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Abbildung 7**: Wählen Sie die Option der DataBindings bearbeiten der Bezeichnung Smarttag aus ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Das Dialogfeld "DataBindings" wird angezeigt. Von hier aus können Sie wählen aus, um teilnehmen in Datenbindung aus der Liste auf der linken Seite, und wählen Sie das Feld aus, um die Daten aus der Dropdown Liste auf der rechten Seite zu binden. Wählen Sie die `Text` Eigenschaft aus der linken Seite und die `LastName` -Feld aus der rechten Seite, und klicken Sie auf OK.


[![Binden Sie der Text-Eigenschaft auf das Feld "LastName" Daten](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Abbildung 8**: Binden der `Text` Eigenschaft, um die `LastName` Feld "Daten" ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Das Dialogfeld "DataBindings" können Sie an, ob die bidirektionale Datenbindung ausgeführt. Wenn Sie dieses Kontrollkästchen deaktiviert ist, lassen die Datenbindungssyntax `<%# Eval("LastName")%>` verwendet werden, anstelle von `<%# Bind("LastName")%>`. Beide Vorgehensweisen ist für dieses Tutorial in Ordnung. Bidirektionale Datenbindung ist wichtig, beim Einfügen und Bearbeiten von Daten. Zum Anzeigen von einfach Daten aus, jedoch funktioniert beide Ansätze gleichermaßen gut. Wir erläutere die bidirektionale Datenbindung im Detail in zukünftigen Lernprogrammen.


Nehmen Sie einen Moment Zeit, zum Anzeigen dieser Seite über einen Browser ein. Wie Sie sehen, enthält die GridView trotzdem vier Spalten; allerdings die `FirstName` Spalte nun aufgeführt *sowohl* der `FirstName` und `LastName` -Datenfeldwerte.


[![Sowohl das FirstName und LastName-Werte werden in einer einzelnen Spalte angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Abbildung 9**: sowohl die `FirstName` und `LastName` Werte werden in einer einzelnen Spalte angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Um diesem ersten Schritt abgeschlossen haben, entfernen Sie die `LastName` BoundField, und benennen Sie die `FirstName` TemplateFields `HeaderText` Eigenschaft "Name". Nach diesen Änderungen sollte den GridView deklarative Markup wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Erste und Nachnamen des Mitarbeiters werden in einer Spalte angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Abbildung 10**: ersten und den Nachnamen jedes Mitarbeiters werden in einer Spalte angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Schritt 3: Verwenden das Kalender-Steuerelement zum Anzeigen der`HiredDate`Feld

Einen Datenfeldwert als Text in einer GridView-Ansicht ist so einfach wie die Verwendung einer BoundField. Für bestimmte Szenarios ist jedoch die Daten am besten ausgedrückt mit der ein bestimmtes Steuerelement anstelle von nur-Text. Es ist eine solche Anpassung der Anzeige von Daten mit von TemplateFields möglich. Z. B. stattdessen als das Einstellungsdatum als Text anzuzeigen, wir konnte einen Kalender anzeigen (mit [das Kalender-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) mit Einstellungsdatum hervorgehoben.

Um dies zu erreichen, starten Sie durch die Konvertierung der `HiredDate` BoundField in ein TemplateField. Einfach wechseln Sie zu den GridView Smarttag, und klicken Sie auf die Spalten bearbeiten-Link, schalten Sie die Felder (Dialogfeld). Wählen Sie die `HiredDate` BoundField- und klicken Sie auf "dieses Feld konvertieren in ein TemplateField."


[![Die HiredDate BoundField-in ein TemplateField konvertieren](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Abbildung 11**: Konvertieren der `HiredDate` BoundField-in ein TemplateField ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Wie in Schritt2 beschrieben, wird dadurch die BoundField ersetzen Sie durch ein TemplateField, die enthält ein `ItemTemplate` und `EditItemTemplate` mit einer Bezeichnung und ein Textfeld, dessen `Text` Eigenschaften gebunden sind, um die `HiredDate` Wert mithilfe der Datenbindungssyntax `<%# Bind("HiredDate")%>`.

Um den Text mit einem Kalendersteuerelement zu ersetzen, bearbeiten Sie die Vorlage, indem Sie die Bezeichnung ein Kalender-Steuerelement. Klicken Sie im Designer wählen Sie aus den GridView Smarttag Vorlagen bearbeiten und dann die `HireDate` TemplateFields `ItemTemplate` aus der Dropdown-Liste. Klicken Sie dann löschen Sie das Label-Steuerelement, und ziehen Sie ein Kalender-Steuerelement aus der Toolbox in die Vorlage bearbeiten-Schnittstelle.


[![Ein Kalendersteuerelement, das HireDate-TemplateField die ItemTemplate-Element](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Abbildung 12**: ein Kalendersteuerelement auf die `HireDate` TemplateFields `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


An diesem Punkt enthält jede Zeile in der GridView ein Kalender-Steuerelement in seiner `HiredDate` TemplateField. Allerdings der Mitarbeiter des tatsächlichen `HiredDate` Wert ist nicht an einer beliebigen Stelle im Kalender-Steuerelement, verursacht jedes Kalender-Steuerelement standardmäßig mit den aktuellen Monat und Datum festgelegt. Um dies zu beheben, müssen wir jedes Mitarbeiters zuweisen `HiredDate` auf der Kalender-Steuerelement ["SelectedDate"](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) und ["VisibleDate"](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) Eigenschaften.

Wählen Sie das Kalendersteuerelement Smarttag DataBindings bearbeiten. Als Nächstes binden Sie beide `SelectedDate` und `VisibleDate` Eigenschaften, die die `HiredDate` Feld "Daten".


[![Binden Sie "SelectedDate" und "VisibleDate" Eigenschaften für das Datenfeld HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Abbildung 13**: Binden der `SelectedDate` und `VisibleDate` Eigenschaften, die die `HiredDate` Feld "Daten" ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Das Kalender-Steuerelement ausgewählte Datum muss nicht notwendigerweise sichtbar werden. Z. B. möglicherweise ein Kalender 1. August<sup>St</sup>, 1999 als das ausgewählte Datum, aber den aktuellen Monat und Jahr angezeigt werden. Das ausgewählte Datum und die sichtbares Datum werden durch des Kalender-Steuerelements angegeben `SelectedDate` und `VisibleDate` Eigenschaften. Da beide Wählen des Mitarbeiters sollten `HiredDate` und stellen Sie sicher, dass es angezeigt wird, müssen wir beide diese Eigenschaften zu binden der `HireDate` Feld "Daten".


Wenn Sie die Seite in einem Browser anzeigen zu können, wird der Kalender jetzt zeigt den Monat des Mitarbeiters Einstellungsdatum und dieses Datum ausgewählt.


[![Des Mitarbeiters HiredDate wird im Kalendersteuerelement angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Abbildung 14**: die Mitarbeiters `HiredDate` sehen Sie in das Kalender-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Im Gegensatz zu allen Beispielen wir, bisher gesehen haben in diesem Tutorial haben wir *nicht* festgelegt `EnableViewState` Eigenschaft `false` für diese GridView. Der Grund für diese Entscheidung ist da bewirkt, ein Postback handeln dass, der im Kalender ausgewählte Datum festlegen, auf das Datum, die einfach auf die Daten des Kalender-Steuerelements. Wenn die GridView Ansichtszustand deaktiviert ist, jedoch bei jedem Postback GridView Daten werden erneut gebunden an die zugrunde liegende Datenquelle, die bewirkt, dass der im Kalender ausgewählte Datum festgelegt werden *wieder* , des Mitarbeiters `HireDate`, überschreiben das Datum, die vom Benutzer ausgewählt wird.


Für dieses Tutorial ist es sich hierbei um eine völlig irrelevant Diskussion da der Benutzer nicht zum Aktualisieren des Mitarbeiters kann `HireDate`. Es wäre wahrscheinlich am besten, das Kalender-Steuerelement so konfigurieren, dass die Daten nicht ausgewählt werden. Unabhängig davon, zeigt das Lernprogramm an, dass in einigen Fällen Ansichtszustand aktiviert werden muss, um bestimmte Funktionen bereitzustellen.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Schritt 4: Zeigt der Anzahl der Tage des Mitarbeiters für das Unternehmen beschäftigt

Bisher haben Sie zwei Anwendungen von TemplateFields erfahren:

- Kombinieren von zwei oder mehr Datenfeldwerte in einer Spalte, und
- Einen Wert im Datenfeld ein Websteuerelement anstelle von Text mit Ausdrücken

Eine dritte Verwendung der von TemplateFields liegt beim Anzeigen von Metadaten über des GridView Daten zugrunde. Neben Einstellungsdatum der Mitarbeiter wird angezeigt, möglicherweise z. B. wir möchten auch eine Spalte haben, die gesamte Anzahl der Tage anzeigt, sie bei der Arbeit gewesen.

Noch entsteht eine weitere Verwendungsmöglichkeit der von TemplateFields in Szenarien, wenn es sich bei müssen die zugrunde liegenden Daten im Bericht Webseite unterschiedlich angezeigt werden, als im Format in der Datenbank gespeichert ist. Stellen Sie sich vor der `Employees` -Tabelle eine `Gender` Feld, das das Zeichen gespeichert `M` oder `F` das Geschlecht des Mitarbeiters an. Wenn diese Informationen auf einer Webseite anzeigen könnten wir das Geschlecht anzeigen, entweder als "Männlich" oder "Weiblich", statt nur "M" oder "F".

Beide Szenarien verarbeitet werden können, durch das Erstellen einer *Formatierungsmethode* in die ASP.NET-Seite Code-Behind-Klasse (oder in einer separaten Klassenbibliothek, die als implementiert eine `static` Methode), die aus der Vorlage aufgerufen wird. Solche eine Formatierungsmethode wird aus der Vorlage, die mit der gleichen Databinding-Syntax, die bereits aufgerufen. Die Formatierungsmethode dauert in einer beliebigen Anzahl von Parametern, aber es muss eine Zeichenfolge zurückgeben. Diese zurückgegebene Zeichenfolge ist der HTML-Code, der in der Vorlage eingefügt wird.

Um dieses Konzept zu veranschaulichen, erweitern wir unser Tutorial zum Einblenden einer Spalte, in der die Gesamtzahl der Tage aufgeführt, die ein Mitarbeiter für den Auftrag war ein. Diese Methode zur Formatierung übernimmt eine `Northwind.EmployeesRow` Objekt aus, und die Anzahl der Tage, die der Mitarbeiter wurde, als Zeichenfolge eingesetzt hat zurück. Diese Methode kann auf der ASP.NET-Seite CodeBehind-Klasse hinzugefügt werden, aber *müssen* gekennzeichnet werden `protected` oder `public` um aus der Vorlage zugegriffen werden.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Da die `HiredDate` Feld darf `NULL` Datenbank Werte, die wir zunächst sicherstellen müssen, dass der Wert nicht `NULL` vor dem Fortfahren mit der Berechnung. Wenn die `HiredDate` Wert `NULL`, wir einfach die Zeichenfolge "Unknown"; zurückzugeben, ist dies nicht `NULL`, wir berechnen den Unterschied zwischen der aktuellen Zeit und die `HiredDate` Wert ein, und geben die Anzahl von Tagen zurück.

Diese Methode nutzen können, wir es in ein TemplateField in den GridView-Ansicht mit der Datenbindungssyntax aufgerufen müssen. Starten Sie durch das Hinzufügen einer neuen TemplateField an die GridView durch Klicken auf den Link "Spalten bearbeiten" in den GridView Smarttag und eine neue TemplateField hinzufügen.


[![Fügen Sie ein neues TemplateField an die GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Abbildung 15**: Hinzufügen einer neuen TemplateField an die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Legen Sie diese neue TemplateField des `HeaderText` "Tage auf des Auftrags"-Eigenschaft und die zugehörige `ItemStyle`des `HorizontalAlign` Eigenschaft `Center`. Zum Aufrufen der `DisplayDaysOnJob` Methode aus der Vorlage Hinzufügen einer `ItemTemplate` und verwenden Sie die folgenden Databinding-Syntax:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Gibt eine `DataRowView` Objekt entspricht der `DataSource` Datensatz gebunden, um die `GridViewRow`. Die `Row` Eigenschaft gibt den stark typisierten `Northwind.EmployeesRow`, der übergeben wird, um die `DisplayDaysOnJob` Methode. Diese Syntax Databinding stehen direkt in der `ItemTemplate` (siehe die deklarative Syntax weiter unten) oder zugewiesen werden kann, um die `Text` Eigenschaft ein Label-Steuerelement.

> [!NOTE]
> Auch nicht in einer `EmployeesRow` -Instanz kann nur übergeben wir den `HireDate` -Wert mithilfe `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Jedoch der `Eval` Methodenrückgabe ein `object`, daher müssten wir ändern unsere `DisplayDaysOnJob` Methodensignatur akzeptieren einen Eingabeparameter vom Typ `object`stattdessen. Es kann nicht blind umgewandelt der `Eval("HireDate")` aufrufen, um eine `DateTime` da die `HireDate` -Spalte in der `Employees` Tabelle darf `NULL` Werte. Aus diesem Grund müssen wir akzeptieren ein `object` als Eingabeparameter für die `DisplayDaysOnJob` -Methode, die überprüfen, ob es sich um eine Datenbank wurde `NULL` Wert (das kann mithilfe von durchgeführt werden `Convert.IsDBNull(objectToCheck)`), und führen Sie anschließend entsprechend.


Aufgrund dieser feinheiten, ich haben sich entschieden, um die gesamte übergeben `EmployeesRow` Instanz. Im nächsten Tutorial sehen, dass ein weitere Anpassung-Beispiel für die Verwendung der `Eval("columnName")` Syntax für einen Eingabeparameter in einer Formatierungsmethode übergeben.

Im folgenden wird die deklarative Syntax für unsere GridView gezeigt, nachdem das TemplateField hinzugefügt wurde und die `DisplayDaysOnJob` Methode mit dem Namen aus der `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Abbildung 16 zeigt das Lernprogramm abgeschlossene, wenn Sie über einen Browser angezeigt.


[![Die Anzahl von-Tagen, die die Mitarbeiter für den Auftrag wurde wird angezeigt.](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Abbildung 16**: die Anzahl von Tagen der Mitarbeiter wurde für den Auftrag angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Zusammenfassung

Das TemplateField im GridView-Steuerelement ermöglicht einen höheren Grad an Flexibilität beim Anzeigen von Daten als bei anderen feldsteuerungen verfügbar ist. Von TemplateFields eignen sich ideal für Situationen, in denen:

- Mehrere Datenfelder, die in einer GridView-Spalte angezeigt werden müssen
- Die Daten ist mithilfe von eine Websteuerelement anstelle von nur-Text am besten ausgedrückt.
- Die Ausgabe hängt von der zugrunde liegenden Daten, z. B. das Anzeigen von Metadaten oder neuformatierung der das

Zusätzlich zum Anpassen der Anzeige von Daten, werden auch von TemplateFields verwendet, für die Benutzeroberflächen, die zum Bearbeiten und Einfügen von Daten verwendet werden, in zukünftigen Lernprogrammen sehen anpassen.

Die nächsten beiden Lernprogramme kennenzulernen Vorlagen, beginnend mit einem Blick auf das Verwenden von TemplateFields in einem DetailsView. Danach werden wir auf die FormView aktivieren, die Vorlagen anstelle von Feldern verwendet werden, um größere Flexibilität bei der das Layout und Struktur der Daten zu ermöglichen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Dan Jagers. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](custom-formatting-based-upon-data-cs.md)
> [Weiter](using-templatefields-in-the-detailsview-control-cs.md)
