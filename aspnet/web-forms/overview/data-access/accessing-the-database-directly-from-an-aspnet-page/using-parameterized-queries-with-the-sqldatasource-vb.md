---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Verwenden von parametrisierten Abfragen mit dem SqlDataSource-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial stellen wir weiterhin unsere Betrachtung der SqlDataSource-Steuerelement und erfahren Sie, wie parametrisierte Abfragen zu definieren. Die Parameter können angegeben werden beide deklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: fd6361d7b8981b7caee996ff3af165416af860a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395485"
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Verwenden von parametrisierten Abfragen mit dem SqlDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) oder [PDF-Datei herunterladen](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> In diesem Tutorial stellen wir weiterhin unsere Betrachtung der SqlDataSource-Steuerelement und erfahren Sie, wie parametrisierte Abfragen zu definieren. Die Parameter können sowohl deklarativ als auch programmgesteuert angegeben werden, und können von einer Anzahl von Standorten, z. B. der Abfragezeichenfolge, Sitzung Zustand, andere Steuerelemente und vieles mehr abgerufen werden.


## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde erläutert, wie das SqlDataSource-Steuerelement zu verwenden, um Daten direkt aus einer Datenbank abzurufen. Mit dem Konfigurieren von Datenquellen-Assistenten können wir die Datenbank aus, und klicken Sie dann entweder auswählen: Wählen Sie die Spalten aus einer Tabelle oder Ansicht zurückgeben Geben Sie eine benutzerdefinierte SQL­Anweisung aus: oder verwenden Sie eine gespeicherte Prozedur. Ob die Spalten aus einer Tabelle oder Sicht ausgewählt oder eine benutzerdefinierte SQL­Anweisung eingeben, dem SqlDataSource-Steuerelement s steuern `SelectCommand` Eigenschaft erhält die Ad-hoc-SQL `SELECT` -Anweisung, und es ist dies `SELECT` Anweisung, die ausgeführt wird, wenn die SqlDataSource-Steuerelement s `Select()` Methode aufgerufen wird (entweder programmgesteuert oder automatisch aus einem Websteuerelement).

Die SQL-Anweisung `SELECT` Anweisungen, die im vorherigen Tutorial s Demos fehlten verwendet `WHERE` Klauseln. In einem `SELECT` -Anweisung, die `WHERE` -Klausel kann verwendet werden, um die zurückgegebenen Ergebnisse zu beschränken. Um die Namen von Produkten, die mehr als 50,00 $ Kosten anzuzeigen, können wir beispielsweise die folgende Abfrage verwenden:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

In der Regel die Werte in einer `WHERE` Klausel werden von einer externen Quelle, z. B. eine Querystring-Wert, eine Sitzungsvariable oder Benutzereingaben in ein Steuerelement auf der Seite zu bestimmen. Im Idealfall werden diese Eingaben angegeben, durch die Verwendung von *Parameter*. Mit Microsoft SQL Server, werden Parameter angegeben, mit `@parameterName`, z. B.:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

Dem SqlDataSource-Steuerelement unterstützt die parametrisierte Abfragen, die sowohl für `SELECT` Anweisungen und `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Darüber hinaus die Parameterwerte können automatisch abgerufen werden aus einer Vielzahl von Quellen die Abfragezeichenfolge, Sitzungszustand, Steuerelemente auf der Seite usw. und programmgesteuert zugewiesen werden können. In diesem Tutorial sehen wir, wie Sie auch parametrisierte Abfragen zu definieren, wie zum Angeben des Parameters sowohl deklarativ als auch programmgesteuert Werte.

> [!NOTE]
> Im vorherigen Tutorial verglichen wir dem ObjectDataSource-Steuerelement, unser Tool Ihrer Wahl über den ersten 46 Tutorials mit dem SqlDataSource-Steuerelement, beachten die ähnlichkeiten zwischen die konzeptionellen Diensten wurde. Diese ähnlichkeiten erstrecken sich auch auf Parameter. Die "ObjectDataSource"-s-Parameter zugeordnet, die Eingabeparameter für die Methoden in der Geschäftslogikebene. Mit dem SqlDataSource-Steuerelement werden die Parameter direkt in die SQL-Abfrage definiert. Beide Steuerelemente sind Auflistungen von Parametern für ihren `Select()`, `Insert()`, `Update()`, und `Delete()` Methoden und beide können diese Parameterwerte aus vordefinierten Quellen (Querystring-Werte, Sitzungsvariablen und So weiter aufgefüllt haben ) oder programmgesteuert zugewiesen.


## <a name="creating-a-parameterized-query"></a>Erstellen einer parametrisierten Abfrage

S SqlDataSource-Steuerelement-konfigurieren von Datenquellen-Assistent bietet drei Möglichkeiten zum Definieren des Befehls ausgeführt wird, um Datenbank-Datensätzen abzurufen:

- Indem Sie auswählen, die Spalten aus einer vorhandenen Tabelle oder Ansicht,
- Durch Eingabe einer benutzerdefinierten SQL-Anweisung oder
- Durch Auswahl einer gespeicherten Prozedur

Bei Auswahl von Spalten aus einer vorhandenen Tabelle oder Sicht, die Parameter für die `WHERE` -Klausel muss angegeben werden, über hinzufügen `WHERE` Dialogfeld-Klausel. Wenn Sie eine benutzerdefinierte SQL­Anweisung zu erstellen, aber Sie können Geben Sie die Parameter direkt in die `WHERE` Klausel (mit `@parameterName` um jeden Parameter anzugeben). Ein [gespeicherte Prozedur](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) besteht aus mindestens einer SQL-Anweisung, und diese Anweisungen parametrisiert werden können. Die in die SQL-Anweisungen verwendeten Parameter müssen allerdings in als Eingabeparameter an die gespeicherte Prozedur übergeben werden.

Da eine parametrisierte Abfrage erstellen, das wie abhängt SqlDataSource s `SelectCommand` wird angegeben, Let s sehen Sie alle drei Ansätze. Öffnen Sie zum Einstieg die `ParameterizedQueries.aspx` auf der Seite die `SqlDataSource` , ein SqlDataSource-Steuerelement aus der Toolbox in den Designer ziehen, und legen Sie dessen `ID` zu `Products25BucksAndUnderDataSource`. Klicken Sie anschließend auf die Datenquelle konfigurieren-Link aus dem Steuerelement-s-Smarttag. Wählen Sie die zu verwendende Datenbank (`NORTHWINDConnectionString`), und klicken Sie auf Weiter.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Schritt 1: Hinzufügen einer`WHERE`-Klausel, wenn die Spalten aus einer Tabelle oder Sicht auswählen

Wenn Sie die Daten wieder aus der Datenbank mit dem SqlDataSource-Steuerelement auswählen, ermöglicht der Konfigurieren von Datenquellen-Assistenten, wählen Sie einfach die Spalten aus einer vorhandenen Tabelle zurückzugeben oder angezeigt werden (siehe Abbildung 1). SQL erstellt dies automatisch `SELECT` -Anweisung, die ist die Datenbank gesendeten beim SqlDataSource s `Select()` -Methode wird aufgerufen. Wie im vorherigen Tutorial, wählen Sie die Products-Tabelle, aus der Dropdown-Liste, und Überprüfen der `ProductID`, `ProductName`, und `UnitPrice` Spalten.


[![Wählen Sie die Spalten aus einer Tabelle oder Sicht zurückgegeben.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Abbildung 1**: Wählen Sie die Spalten auf die Rückgabe aus einer Tabelle oder Sicht ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


Einschließen einer `WHERE` -Klausel in der `SELECT` -Anweisung, klicken Sie auf die `WHERE` Schaltfläche hinzufügen bringt `WHERE` Klausel Dialogfeld (siehe Abbildung 2). Zum Hinzufügen eines Parameters, um die Ergebnisse zu begrenzen die `SELECT` abzufragen, wählen Sie zuerst die Spalte zum Filtern der Daten durch. Wählen Sie als Nächstes den Operator zum Filtern zu verwendende (=, &lt;, &lt;= &gt;und so weiter). Wählen Sie abschließend die Quelle des s Wert des Parameters, wie z. B. vom Status Abfragezeichenfolge oder der Sitzung. Nach dem Konfigurieren des Parameters, klicken Sie auf die Schaltfläche "hinzufügen" Einbeziehung in die `SELECT` Abfrage.

In diesem Beispiel können s nur die Ergebnisse zurückgeben, in denen die `UnitPrice` Wert ist kleiner als oder gleich 25,00. Wählen Sie aus diesem Grund `UnitPrice` aus der Dropdown-Liste von Spalte und &lt;= aus der Dropdown-Liste ' Operator '. Bei Verwendung von hartcodierten Parameterwert (z. B. 25,00), oder wenn der Parameterwert ist programmgesteuert angegeben werden, wählen Sie keine aus der Quelle Dropdown-Liste aus. Als Nächstes geben Sie den hartcodierten Parameterwert in das Textfeld Wert 25,00, und schließen Sie den Prozess, indem Sie auf die Schaltfläche "hinzufügen".


[![Begrenzen Sie die Ergebnisse zurückgegeben, die von der WHERE hinzufügen Dialogfeld-Klausel](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Abbildung 2**: Beschränken von Ergebnissen aus dem Hinzufügen `WHERE` Dialogfeld-Klausel ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


Klicken Sie nachdem der Parameter hinzugefügt haben auf OK, um zum Konfigurieren von Datenquellen-Assistenten zurückzukehren. Die `SELECT` -Anweisung am Ende des Assistenten enthalten sollte nun eine `WHERE` -Klausel mit einem Parameter mit dem Namen `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Wenn Sie angeben, dass mehrere Bedingungen in der `WHERE` -Klausel aus den hinzufügen `WHERE` Dialogfeld-Klausel, der Assistent verknüpft sie mit der `AND` Operator. Wenn Sie einschließen möchten ein `OR` in der `WHERE` Klausel (wie z. B. `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) müssen Sie zum Erstellen der `SELECT` Anweisung mithilfe des Bildschirms zum benutzerdefinierten SQL-Anweisung.


Abschließen Sie der Konfiguration dem SqlDataSource-Steuerelement (klicken Sie auf Weiter und beenden), und untersuchen Sie SqlDataSource s deklarative Markup zu. Das Markup enthält jetzt eine `<SelectParameters>` -Auflistung, die beschreibt, die Quellen für die Parameter in der `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Wenn s SqlDataSource-Steuerelement `Select()` Methode wird aufgerufen, die `UnitPrice` Parameterwert (25,00) gilt, an die `@UnitPrice` Parameter in der `SelectCommand` vor dem Senden an die Datenbank. Das Ergebnis ist, dass nur die Produkte, die kleiner oder gleich 25,00 werden zurückgegeben, aus der `Products` Tabelle. Vergewissern Sie sich, Hinzufügen einer GridView-Ansicht auf der Seite, an diese Datenquelle binden Sie, und zeigen Sie die Seite über einen Browser. Diese Produkte aufgeführt, die kleiner oder gleich 25,00, wie die Abbildung 3 wird bestätigt, sollte nur angezeigt werden.


[![Nur die Produkte kleiner als oder gleich dem 25,00 werden angezeigt.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Abbildung 3**: nur die Produkte kleiner als oder gleich dem 25,00 werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Schritt 2: Hinzufügen von Parametern für eine benutzerdefinierte SQL-Anweisung

Beim Hinzufügen einer benutzerdefinierten SQL-Anweisung können Sie eingeben der `WHERE` Klausel explizit, oder geben Sie einen Wert in der Zelle Filtern der Abfrage-Generator. Um dies zu demonstrieren, lassen Sie s, die nur für diese Produkte in einer GridView anzeigen, deren Preis, die kleiner als einen bestimmten Schwellenwert sind. Starten, indem ein Textfeld zum Hinzufügen der `ParameterizedQueries.aspx` Seite, um diesen Schwellenwert vom Benutzer gesammelt werden. Legen Sie das Textfeld s `ID` Eigenschaft `MaxPrice`. Fügen Sie ein Steuerelement Schaltfläche hinzu, und legen Sie dessen `Text` Eigenschaft, um übereinstimmende Produkte angezeigt.

Als Nächstes einer GridView-Ansicht auf die Seite ziehen und sein Smarttag auswählen, um das Erstellen einer neuen SqlDataSource-Steuerelement mit dem Namen `ProductsFilteredByPriceDataSource`. Fahren Sie aus dem Konfigurieren von Datenquellen-Assistenten, mit der Specify benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Bildschirm (siehe Abbildung 4), und geben Sie die folgende Abfrage:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Klicken Sie nachdem Sie die Abfrage eingegeben haben (entweder manuell oder über den Abfrage-Generator) auf "Weiter".


[![Nur solche Produkte zurück auf einen Parameterwert kleiner oder gleich](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Abbildung 4**: Zurückgeben nur diejenigen Produkte kleiner als oder gleich dem Parameterwert ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


Da die Abfrage Parameter enthält, verlangt der nächste Bildschirm des Assistenten für die Quelle der Parameterwerte. Wählen Sie aus der Parameterliste der Quelle Dropdown-Steuerelement und `MaxPrice` (das TextBox-Steuerelement s `ID` Wert) aus der ControlID Dropdown-Liste. Sie können auch einen Optionaler Standardwert in die Groß-/Kleinschreibung verwendet, in denen der Benutzer verfügt über einen beliebigen Text in nicht eingegeben, Eingeben der `MaxPrice` Textfeld. Geben Sie für die zurzeit keinen Standardwert ein.


[![Die MaxPrice TextBox-s-Text-Eigenschaft wird als Quelle für Parameter verwendet.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Abbildung 5**: die `MaxPrice` Textfeld s `Text` Eigenschaft dient als Quelle-Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


Schließen Sie den Assistenten für die Datenquelle konfigurieren, indem Sie als Nächstes auf, und schließen. Deklarative Markup für die GridView, Textfeld, Schaltfläche und SqlDataSource-Steuerelement folgt:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Beachten Sie, dass der Parameter in der SqlDataSource s `<SelectParameters>` Abschnitt ist eine `ControlParameter`, darunter zusätzliche Eigenschaften wie `ControlID` und `PropertyName`. Wenn die SqlDataSource-s `Select()` Methode wird aufgerufen, die `ControlParameter` Ruft den Wert aus der angegebenen Eigenschaft für die Web-Steuerelements, und weist es der entsprechende Parameter im der `SelectCommand`. In diesem Beispiel die `MaxPrice` s Text-Eigenschaft dient als die `@MaxPrice` Parameterwert.

Kurz zum Anzeigen dieser Seite über einen Browser. Wenn die Seite zuerst besuchen oder die `MaxPrice` Textfeld verfügt nicht über einen Wert, der keine Datensätze in den GridView-Ansicht angezeigt werden.


[![Keine Datensätze werden angezeigt, wenn die MaxPrice Textfeld leer ist.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Abbildung 6**: keine Datensätze werden angezeigt, wenn die `MaxPrice` Textfeld ist leer ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


Der Grund sind keine Produkte dargestellt ist, standardmäßig eine leere Zeichenfolge für einen Parameterwert in einer Datenbank konvertiert wird `NULL` Wert. Da der Vergleich `[UnitPrice] <= NULL` ergibt immer als "False" werden keine Ergebnisse zurückgegeben.

Geben Sie einen Wert in das Textfeld ein, z. B. 5,00, und klicken Sie auf die Schaltfläche mit den übereinstimmenden Produkte angezeigt. Beim Postback informiert dem SqlDataSource-Steuerelement an, dass die GridView, dass eine der Quellen Parameter geändert wurde. Die Bindung folglich GridView an dem SqlDataSource-Steuerelement, diese Produkte kleiner als oder gleich auf $5.00 anzeigen.


[![Produkte kleiner als oder gleich dem $5.00 werden angezeigt.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Abbildung 7**: Produkte kleiner als oder gleich dem $5.00 werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Zunächst anzeigen alle Produkte

Anstatt keine Produkte anzuzeigen, wenn die Seite zuerst geladen wird, möchten wir vielleicht anzeigen *alle* Produkte. Eine Möglichkeit zum Auflisten aller Produkte immer die `MaxPrice` Textfeld leer ist. ist für einige unglaublich hohen Wert festlegen, der Standardwert des Parameters s festzulegen, wie 1000000, da sie 1.000.000 $vorliegt unwahrscheinlich, dass Northwind Traders wird ständig Stückpreis inventarisiert haben %(threshold)s überschreitet. Dieser Ansatz wird jedoch kurzsichtig ist und funktioniert möglicherweise nicht in anderen Situationen.

In vorherigen Tutorials - [deklarative Parameter](../basic-reporting/declarative-parameters-vb.md) und [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) wir mit einem ähnlichen Problem konfrontiert waren. Es unsere Lösung bestand darin, diese Logik in der Geschäftslogikebene eingefügt. Insbesondere die BLL untersucht den eingehenden Wert und, falls es war `NULL` oder einige reservierte Wert, der Anruf weitergeleitet wurde, an die DAL-Methode, die alle Datensätze zurückgegeben. Wenn der eingehende Wert einer normalen Filterwert war, eine Puffermethode wurde an die DAL-Methode, die eine SQL-Anweisung ausgeführt, die eine parametrisierte verwendet `WHERE` -Klausel mit dem angegebenen Wert.

Leider können umgehen wir die Architektur beim mit dem SqlDataSource-Steuerelement. Stattdessen müssen wir anpassen, die SQL-Anweisung, um alle Datensätze auf intelligente Weise beziehen Sie bei der `@MaximumPrice` Parameter `NULL` oder einen reservierten Wert. Für diese Übung können s haben sie also, dass bei der `@MaximumPrice` -Parameter ist gleich `-1.0`, klicken Sie dann *alle* der Datensätze sind, zurückgegeben werden (`-1.0` arbeitet als ein reservierter Wert, da kein Produkt eine Negative aufweisenkann`UnitPrice`Wert). Um dies zu erreichen, können wir die folgende SQL-Anweisung verwenden:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Dies `WHERE` Klausel gibt *alle* aufgezeichnet, wenn die `@MaximumPrice` entspricht `-1.0`. Der Wert des Parameters ist nicht `-1.0`, nur die Produkte, deren `UnitPrice` ist kleiner als oder gleich der `@MaximumPrice` Parameterwert werden zurückgegeben. Durch Festlegen des Standardwerts der `@MaximumPrice` Parameter, um `-1.0`, auf dem ersten Laden einer Seite (oder jedes Mal, wenn die `MaxPrice` Textfeld leer ist), `@MaximumPrice` Wert der `-1.0` und alle Produkte angezeigt.


[![Jetzt alle Produkte werden ist angezeigt, wenn die MaxPrice Textfeld leer](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Abbildung 8**: jetzt alle Produkte sind, angezeigt, wenn die `MaxPrice` Textfeld ist leer ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


Es gibt eine Reihe von Einschränkungen bei dieser Vorgehensweise beachten. Zunächst feststellen, dass die s-Parameterdatentyp davon abgeleitet ist s-Nutzung in der SQL-Abfrage. Wenn Sie ändern die `WHERE` -Klausel aus `@MaximumPrice = -1.0` zu `@MaximumPrice = -1`, die Laufzeit behandelt die Parameter als ganze Zahl. Wenn Sie dann versuchen, Sie weisen die `MaxPrice` Textfeld in einen Dezimalwert (z. B. 5,00) ein Fehler tritt auf, da er 5,00 in eine ganze Zahl konvertieren kann. Zur Behebung des Problems, machen Sie sich, dass Sie `@MaximumPrice = -1.0` in die `WHERE` -Klausel oder besser noch, legen Sie die `ControlParameter` s-Objekt `Type` Eigenschaft in eine Dezimalzahl.

Zweitens, durch das Hinzufügen der `OR @MaximumPrice = -1.0` auf die `WHERE` -Klausel die Abfrage-Engine kann nicht verwenden, einen Index auf `UnitPrice` (sofern eine vorhanden), bei einem Tabellenscan Ergebnis. Dies kann die Leistung beeinträchtigen, wenn es eine ausreichend große Zahl von Datensätzen in gibt der `Products` Tabelle. Ein besserer Ansatz wäre, diese Logik in einer gespeicherten Prozedur zu verschieben, in denen ein `IF` Anweisung würde entweder auszuführen. eine `SELECT` Abfrage aus der `Products` Tabelle ohne einen `WHERE` -Klausel, wenn alle Datensätze zurückgegeben werden müssen oder eine, deren `WHERE` -Klausel enthält nur die `UnitPrice` Kriterien, so, dass ein Index verwendet werden kann.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Schritt 3: Erstellen und verwenden Sie parametrisierte gespeicherte Prozeduren

Gespeicherte Prozeduren können einen Satz von Eingabeparametern, die verwendet werden können einschließen, in der SQL-Anweisungen, die in der gespeicherten Prozedur definiert. Beim Konfigurieren von dem SqlDataSource-Steuerelement um eine gespeicherte Prozedur verwenden, die Eingabeparameter akzeptiert, können diese Parameterwerte mit den gleichen Verfahren wie bei Ad-hoc-SQL-Anweisungen angegeben werden.

Zur Veranschaulichung der Verwendung von gespeicherten Prozeduren in dem SqlDataSource-Steuerelement können s, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank, die mit dem Namen `GetProductsByCategory`, akzeptiert einen Parameter namens `@CategoryID` und gibt alle Spalten der Produkte zurück, dessen `CategoryID` entspricht der Spalte `@CategoryID`. Um eine gespeicherte Prozedur zu erstellen, wechseln Sie zu dem Server-Explorer, und einen Drilldown der `NORTHWND.MDF` Datenbank. (Wenn Sie Ich möchte den Server-Explorer angezeigt wird, machen sie Sie durch Navigieren zum Menü anzeigen und die Server-Explorer-Option auswählen.)

Von der `NORTHWND.MDF` -Datenbank mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, wählen Sie die neue gespeicherte Prozedur hinzufügen, und geben Sie die folgende Syntax:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Klicken Sie auf die speichern-Symbol (oder STRG + S) um die gespeicherte Prozedur zu speichern. Sie können die gespeicherte Prozedur testen, indem Sie mit der rechten Maustaste im Ordner gespeicherte Prozeduren und "ausführen" wählen. Dadurch werden Sie aufgefordert, für die gespeicherte Prozedur s-Parameter (`@CategoryID`, in diesem Fall), nach denen die Ergebnisse im Ausgabefenster angezeigt wird.


[![Die GetProductsByCategory gespeicherte Prozedur, die bei der Ausführung einer @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Abbildung 9**: die `GetProductsByCategory` gespeicherte Prozedur bei der Ausführung einer `@CategoryID` 1 ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


Lassen Sie s, die diese gespeicherte Prozedur zu verwenden, um alle Produkte in der Kategorie "Getränke" in einer GridView-Ansicht anzeigen. Hinzufügen einer neuen GridView-Ansicht auf der Seite, und ihn mit einer neuen SqlDataSource-Steuerelement mit dem Namen `BeverageProductsDataSource`. Weiterhin der Specify benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Bildschirm, aktivieren Sie das Optionsfeld der gespeicherten Prozedur, und wählen Sie die `GetProductsByCategory` gespeicherte Prozedur aus der Dropdown-Liste.


[![Wählen Sie die GetProductsByCategory gespeicherte Prozedur aus der Dropdown-Liste](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Abbildung 10**: Wählen Sie die `GetProductsByCategory` gespeicherte Prozedur aus der Dropdown-Liste ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


Da die gespeicherte Prozedur einen Eingabeparameter akzeptiert (`@CategoryID`), klicken weiter an die Quelle für den Wert dieses Parameters s verlangt. Die Getränke `CategoryID` 1 ist, also keine Dropdownliste für die Parameter-Quelle behalten, und geben Sie 1 in das Textfeld "DefaultValue" ein.


[![Verwenden Sie einen hartcodierten Wert 1, um die Produkte in der Kategorie "Getränke" zurückgegeben](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Abbildung 11**: Verwenden Sie Hard-Coded Wert 1, um die Produkte in der Kategorie "Getränke" zurückgegeben ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


Wie im folgenden deklaratives Markup dargestellt, wenn Sie eine gespeicherte Prozedur, die SqlDataSource-s verwenden `SelectCommand` -Eigenschaftensatz auf den Namen der gespeicherten Prozedur und die [ `SelectCommandType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) nastaven NA hodnotu `StoredProcedure`, um anzugeben, die die `SelectCommand` ist der Name einer gespeicherten Prozedur, statt eine Ad-hoc-SQL-Anweisung.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Testen Sie die Seite in einem Browser. Nur die Produkte, die die Kategorie "Getränke" angehören, werden angezeigt, obwohl *alle* des Produkts Felder angezeigt werden, da die `GetProductsByCategory` gespeicherte Prozedur gibt alle Spalten aus der `Products` Tabelle. Wir könnten natürlich beschränken oder passen Sie die Felder in den GridView-Ansicht aus dem GridView s Spalten bearbeiten-Dialogfeld angezeigt.


[![Alle von Getränken werden angezeigt](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Abbildung 12**: alle von Getränken werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Schritt 4: Programmgesteuerten Aufrufen von einem SqlDataSource s`Select()`Anweisung

In den Beispielen wir haben bislang in der vorherigen und dieses Lernprogramms haben SqlDataSource-Steuerelemente direkt an ein GridView gebunden. Die SqlDataSource-Steuerelement s-Daten, jedoch können werden programmgesteuert zugegriffen und im Code aufgelistet. Dies kann besonders nützlich sein, wenn Sie zum Abfragen von Daten benötigen, um es zu überprüfen, aber ich möchte es angezeigt werden müssen. Anstatt alle ADO.NET Codebausteine, die eine Verbindung mit der Datenbank herstellen, geben Sie den Befehl und Abrufen der Ergebnisse schreiben, können Sie dem SqlDataSource-Steuerelement diese monotonen Code behandeln lassen.

Zur Veranschaulichung der Arbeit mit dem SqlDataSource-s stellen Sie sich vor Daten programmgesteuert, dass es sich bei Ihrem Chef erreicht wurde Sie mit einer Anforderung an eine Webseite erstellen, die den Namen des eine zufällig ausgewählte Kategorie und die zugehörigen Produkte anzeigt. D. h. wenn ein Benutzer diese Seite besucht, wir möchten nach dem Zufallsprinzip wählen Sie eine Kategorie aus der `Categories` Tabellen, die den Kategorienamen und die Liste klicken Sie dann auf die Produkte, die zu dieser Kategorie gehören.

Um dies zu erreichen, die wir benötigen zwei SqlDataSource-Steuerelemente ein, ziehen Sie eine zufällige Kategorie aus der `Categories` Tabelle und eine andere Kategorie s Produkte abgerufen. Wir erstellen dem SqlDataSource-Steuerelement, die einen zufälligen Kategoriedatensatz in diesem Schritt abruft; Schritt 5 untersucht die Erstellung von dem SqlDataSource-Steuerelement, das die Kategorie-s-Produkte abruft.

Durch das Hinzufügen von einem SqlDataSource-Steuerelement zum Starten `ParameterizedQueries.aspx` und legen Sie seine `ID` zu `RandomCategoryDataSource`. Konfigurieren Sie es so, dass sie die folgende SQL-Abfrage verwendet:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` Gibt zurück, die Datensätze in zufälliger Reihenfolge sortiert (finden Sie unter [Using `NEWID()` , nach dem Zufallsprinzip Sortieren von Datensätzen](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Gibt den ersten Datensatz aus dem Resultset zurück. All diese Abfrage gibt die `CategoryID` und `CategoryName` Spaltenwerte aus einer einzelnen, zufällig ausgewählten Kategorie.

Zum Anzeigen der Kategorie "s" `CategoryName` Wert, der Seite ein Label-Steuerelement hinzufügen, legen Sie seine `ID` Eigenschaft `CategoryNameLabel`, und deaktivieren Sie die `Text` Eigenschaft. Um die Daten von einem SqlDataSource-Steuerelement programmgesteuert abzurufen, müssen wir rufen die `Select()` Methode. Die [ `Select()` Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) erwartet einen einzelnen Eingabeparameter vom Typ [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), der angibt, wie die Daten vor der Rückgabe Nachrichten werden sollte. Dies kann die Anweisungen zum Sortieren und Filtern von Daten enthalten und wird von den Daten, die Websteuerelemente beim Sortieren oder durch die Daten von einem SqlDataSource-Steuerelement paging verwendet. In unserem Beispiel jedoch wir Einbau zusätzlichen t müssen die Daten geändert werden, bevor Sie zurückgegeben wird und daher wird übergeben, die `DataSourceSelectArguments.Empty` Objekt.

Die `Select()` -Methode gibt ein Objekt, das implementiert `IEnumerable`. Der genaue Typ zurückgegeben, hängt vom Wert von dem SqlDataSource-Steuerelement s [ `DataSourceMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Wie im vorherigen Tutorial erwähnt, kann diese Eigenschaft festgelegt werden, auf den Wert entweder `DataSet` oder `DataReader`. Wenn auf festgelegt `DataSet`, wird die `Select()` Methode gibt eine ["DataView"](https://msdn.microsoft.com/library/01s96x0z.aspx) Objekt; Wenn Sie auf `DataReader`, gibt ein Objekt, das implementiert [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Da die `RandomCategoryDataSource` SqlDataSource-Steuerelement verfügt über seine `DataSourceMode` -Eigenschaftensatz auf `DataSet` (Standardeinstellung), wir arbeiten mit einem DataView-Objekt.

Der folgende Code zeigt, wie Sie zum Abrufen der Datensätze aus der `RandomCategoryDataSource` SqlDataSource-Steuerelement als einer "DataView" sowie Gewusst wie: Lesen der `CategoryName` Spaltenwert in der ersten Zeile von "DataView":


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` Gibt das erste `DataRowView` in das DataView-Steuerelement. `randomCategoryView(0)("CategoryName")` Gibt den Wert des der `CategoryName` Spalte in dieser ersten Zeile. Beachten Sie, dass das DataView-Steuerelement lose typbindung. Um ein bestimmter Spaltenwert verweisen müssen wir den Namen der Spalte als Zeichenfolge ("CategoryName", in diesem Fall) zu übergeben. In Abbildung 13 wird die Meldung angezeigt, der `CategoryNameLabel` beim Anzeigen der Seite. Natürlich wird der tatsächliche Kategorie Anzeigenamen nach dem Zufallsprinzip ausgewählt, die `RandomCategoryDataSource` SqlDataSource-Steuerelement auf jedem Besuch der Seite (einschließlich Postbacks).


[![Der Name angezeigt wird nach dem Zufallsprinzip ausgewählten Kategorie s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Abbildung 13**: Name angezeigt wird, der nach dem Zufallsprinzip ausgewählten Kategorie-s ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> Wenn die SqlDataSource-, s Steuerelement `DataSourceMode` Eigenschaft Wert `DataReader`, den Rückgabewert der `Select()` Methode würde in umgewandelt werden mussten `IDataReader`. Zum Lesen der `CategoryName` Spaltenwert in der ersten Zeile, wir d verwenden Sie Code wie:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Mit dem SqlDataSource-Steuerelement nach dem Zufallsprinzip durch Auswahl einer Kategorie, es erneut bereit, die GridView hinzufügen, die die-s-Kategorieprodukte aufgeführt sind.

> [!NOTE]
> Statt ein Label-Steuerelement zum Anzeigen der Namen der Kategorie s, wir könnten eine FormView oder DetailsView haben auf der Seite hinzugefügt an dem SqlDataSource-Steuerelement gebunden wird. Verwenden die Bezeichnung, jedoch konnten wir untersuchen die s SqlDataSource-Steuerelement programmgesteuert aufrufen `Select()` -Anweisung und die Arbeit mit den sich ergebenden Daten im Code.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Schritt 5: Zuweisen von Parameterwerten programmgesteuert

Alle Beispiele wir haben bislang in diesem Tutorial verwendet haben, entweder einen hartcodierten Parameterwert oder eine stammt aus einer der vordefinierten Parameter Quellen (eine Querystring-Wert, ein Steuerelement auf der Seite, und So weiter). Die SqlDataSource-Steuerelement-s-Parameter können jedoch auch programmgesteuert festgelegt werden. Um die aktuellen Beispiel abzuschließen, benötigen wir ein SqlDataSource-Steuerelement, das alle Produkte, die zu einer angegebenen Kategorie gehören zurückgibt. Dieser SqlDataSource-Steuerelement hat eine `CategoryID` Parameter, dessen Wert festgelegt werden muss, basierend auf der `CategoryID` durch zurückgegebene Wert für die `RandomCategoryDataSource` SqlDataSource-Steuerelement in der `Page_Load` -Ereignishandler.

Starten Sie durch das Hinzufügen einer GridView-Ansicht auf der Seite, und binden Sie es an eine neue SqlDataSource-Steuerelement mit dem Namen `ProductsByCategoryDataSource`. Wie haben wir in Schritt 3, dem SqlDataSource-Steuerelement so konfigurieren, dass sie ruft die `GetProductsByCategory` gespeicherte Prozedur. Behalten Sie die Parameter Quelle die Dropdown-Liste auf "None", aber geben Sie keine Standardwert zugewiesen wurde, da wir diesen Wert programmgesteuert festgelegt werden.


[![Geben Sie keine Parameterquelle oder Standardwert](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Abbildung 14**: Geben Sie keine Parameter-Quelle oder Default Value ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


Nach dem Fertigstellen des Assistenten für SqlDataSource-Steuerelement sollte das daraus resultierende deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Weisen wir die `DefaultValue` von der `CategoryID` Parameter programmgesteuert in die `Page_Load` -Ereignishandler:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Mit folgender Ergänzung enthält die Seite einer GridView-Ansicht, die die Produkte, die nach dem Zufallsprinzip ausgewählten Kategorie zugeordnet wird.


[![Geben Sie keine Parameterquelle oder Standardwert](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Abbildung 15**: Geben Sie keine Parameter-Quelle oder Default Value ([klicken Sie, um das Bild in voller Größe anzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Dem SqlDataSource-Steuerelement ermöglicht es dem Seitenentwickler um parametrisierte Abfragen zu definieren, deren Parameterwerte fest programmierte, mithilfe von Pull aus vordefinierten Parameter Quellen oder programmgesteuert zugewiesen werden können. In diesem Tutorial wurde erläutert, wie eine parametrisierte Abfrage aus dem Konfigurieren von Datenquellen-Assistenten für Ad-hoc-SQL-Abfragen und gespeicherte Prozeduren zu erstellen. Wir haben uns auch unter Verwendung hartcodierter Parameter Quellen, ein Steuerelement als Parameterquelle, und der Wert des Parameters programmgesteuert festlegen.

Wie bietet mit dem ObjectDataSource-Steuerelement, dem SqlDataSource-Steuerelement auch Funktionen für die zugrunde liegenden Daten ändern. Im nächsten Tutorial betrachten wir das Definieren des `INSERT`, `UPDATE`, und `DELETE` -Anweisungen mit dem SqlDataSource-Steuerelement. Nachdem Sie diese Anweisungen hinzugefügt wurden, können wir das integrierte einfügen, bearbeiten und Löschen von Features der an die GridView, DetailsView oder FormView-Steuerelemente nutzen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Scott Clyde Randell Schmidt und Ken Pespisa. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](querying-data-with-the-sqldatasource-control-vb.md)
> [Weiter](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
