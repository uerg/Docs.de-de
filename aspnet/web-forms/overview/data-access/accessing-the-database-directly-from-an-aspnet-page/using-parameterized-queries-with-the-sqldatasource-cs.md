---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Verwenden parametrisierte Abfragen mit SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm haben wir unsere Betrachtung der SqlDataSource-Steuerelement fortfahren und erfahren Sie, wie parametrisierte Abfragen zu definieren. Die Parameter können angegeben werden, beide Decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b66c68b8306b905a800465ab0ed720ae6f9d16b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Verwenden parametrisierte Abfragen mit SqlDataSource (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) oder [PDF herunterladen](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> In diesem Lernprogramm haben wir unsere Betrachtung der SqlDataSource-Steuerelement fortfahren und erfahren Sie, wie parametrisierte Abfragen zu definieren. Der Parameter können sowohl deklarativ und programmgesteuert angegeben werden und können von mehreren Standorten wie die Abfragezeichenfolge, Session State, andere Steuerelemente und vieles mehr abgerufen werden.


## <a name="introduction"></a>Einführung

Im vorherigen Lernprogramm wurde erläutert, wie die SqlDataSource-Steuerelement verwenden, um Daten direkt aus einer Datenbank abzurufen. Mit dem Konfigurieren von Datenquellen-Assistenten können wir die Datenbank aus, und klicken Sie dann entweder auswählen: Wählen Sie die Spalten aus einer Tabelle oder Sicht; zurückgegeben. Geben Sie eine benutzerdefinierte SQL­Anweisung aus. oder verwenden Sie eine gespeicherte Prozedur. Ob die Auswahl von Spalten aus einer Tabelle oder Sicht, oder Sie eine benutzerdefinierte SQL­Anweisung eingeben, die SqlDataSource s steuern `SelectCommand` Eigenschaft erhält die resultierende Ad-hoc-SQL `SELECT` -Anweisung, und es ist dies `SELECT` Anweisung, die ausgeführt wird, wenn die SqlDataSource s `Select()` Methode aufgerufen wird (entweder programmgesteuert oder automatisch aus einem Websteuerelement).

Die SQL `SELECT` -Anweisungen in der vorherigen Lernprogramm s Demos fehlte `WHERE` Klauseln. In einem `SELECT` -Anweisung, die `WHERE` -Klausel kann verwendet werden, um die zurückgegebenen Ergebnisse zu begrenzen. Um die Namen von Produkten, die mehr als 50,00 $ Kostenberechnungen anzuzeigen, könnten wir z. B. die folgende Abfrage verwenden:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

In der Regel die Werte, die in verwendet eine `WHERE` Klausel werden von einer externen Quelle, z. B. eine Querystring-Wert, Sitzungsvariablen oder eine Benutzereingabe aus einem Webserver-Steuerelement auf der Seite zu bestimmen. Im Idealfall werden diese Eingaben mithilfe des angegeben *Parameter*. Mit Microsoft SQL Server, werden Parameter angegeben, mit `@parameterName`, z. B.:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

Die ': SqlDataSource unterstützt parametrisierte Abfragen, sowohl für `SELECT` Anweisungen und `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Darüber hinaus die Parameterwerte können automatisch abgerufen werden aus einer Vielzahl von Quellen die Abfragezeichenfolge, Sitzungszustand, Steuerelemente auf der Seite usw. und programmgesteuert zugewiesen werden können. In diesem Lernprogramm sehen wir, wie parametrisierte Abfragen auch definiert, wie zum Angeben des Parameters deklarativ und programmgesteuert Werte.

> [!NOTE]
> Im vorherigen Lernprogramm verglichen wir das ObjectDataSource, unserem Werkzeug Wahl über die ersten 46 Lernprogramme mit dem SqlDataSource, beachten Sie die konzeptionellen Ähnlichkeit wurde. Diese ähnlichkeiten erweitern auch Parameter. Die ObjectDataSource s-Parameter, die Eingabeparameter für Methoden in der Business Logic Layer zugeordnet ist. Die Parameter sind mit den SqlDataSource direkt in die SQL-Abfrage definiert. Beide Steuerelemente sind Auflistungen von Parametern für ihre `Select()`, `Insert()`, `Update()`, und `Delete()` Methoden, und beide können diese Parameterwerte aus vordefinierten Quellen (Querystring-Werte, Sitzungsvariablen usw. aufgefüllt haben ) oder programmgesteuert zugewiesen.


## <a name="creating-a-parameterized-query"></a>Erstellen einer parametrisierten Abfrage

Die ': SqlDataSource-Steuerelement-Assistent zum Konfigurieren von Datenquellen der s bietet drei Möglichkeiten zum Definieren des Befehls zum Abrufen von Datenbankdatensätzen ausführen:

- Durch Entnahme von Spalten aus einer vorhandenen Tabelle oder Sicht,
- Indem Sie eine benutzerdefinierte SQL­Anweisung eingeben oder
- Durch Auswahl einer gespeicherten Prozedur

Wenn Spalten aus einer vorhandenen Tabelle oder Sicht, die Parameter für die Entnahme der `WHERE` -Klausel muss angegeben werden, durch das Hinzufügen `WHERE` Dialogfeld '-Klausel. Wenn Sie eine benutzerdefinierte SQL­Anweisung zu erstellen, aber Sie können Geben Sie die Parameter direkt in die `WHERE` -Klausel (mit `@parameterName` um jeden Parameter zu bezeichnen). Ein [gespeicherte Prozedur](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) besteht aus einem oder mehreren SQL-Anweisungen, und diese Anweisungen können parametrisiert sein. Die in der SQL-Anweisung verwendeten Parameter müssen allerdings in als Eingabeparameter an die gespeicherte Prozedur übergeben werden.

Da erstellen eine parametrisierte Abfrage wie voraussetzt SqlDataSource s `SelectCommand` wird angegeben, so können s sehen Sie alle drei Ansätze. Öffnen Sie zum Einstieg die `ParameterizedQueries.aspx` auf der Seite der `SqlDataSource` Ordner ein SqlDataSource-Steuerelement aus der Toolbox in den Designer ziehen, und legen Sie dessen `ID` auf `Products25BucksAndUnderDataSource`. Klicken Sie dann auf den Link "Datenquelle konfigurieren" aus dem Steuerelement s Smarttag. Wählen Sie die zu verwendende Datenbank (`NORTHWINDConnectionString`), und klicken Sie auf Weiter.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Schritt 1: Hinzufügen einer`WHERE`-Klausel, wenn die Spalten aus einer Tabelle oder Sicht auswählen

Bei der Auswahl der Daten mit SqlDataSource-Steuerelement aus der Datenbank zurückgeben, ermöglicht das Konfigurieren von Datenquellen-Assistenten uns, wählen Sie einfach die Spalten aus einer vorhandenen Tabelle zurückzugeben oder anzeigen (siehe Abbildung 1). Auf diese Weise automatisch erstellt, eine SQL `SELECT` -Anweisung, die die Datenbank gesendeten ist bei der SqlDataSource-s `Select()` Methode wird aufgerufen. Wie wir im vorherigen Lernprogramm ausgeführt haben, wählen Sie die Products-Tabelle aus der Dropdown-Liste, und Überprüfen der `ProductID`, `ProductName`, und `UnitPrice` Spalten.


[![Wählen Sie die Spalten aus einer Tabelle oder Sicht zurückgegeben.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Abbildung 1**: die Spalten auswählen, um die Rückgabe aus einer Tabelle oder Sicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Einschließen einer `WHERE` -Klausel in der `SELECT` -Anweisung, klicken Sie auf die `WHERE` Schaltfläche untergebracht oben hinzufügen `WHERE` Klausel Dialogfeld (siehe Abbildung 2). Hinzufügen ein Parameters, um die von der zurückgegebenen Ergebnisse zu begrenzen der `SELECT` abzufragen, wählen Sie zuerst die Spalte, die die Daten nach filtern. Als nächstes wählen Sie den Operator zum Filtern (=, &lt;, &lt;=, &gt;usw.). Wählen Sie abschließend die Quelle des Parameterwerts s wie z. B. aus dem Zustand Querystring oder Sitzung. Klicken Sie nach dem Konfigurieren des Parameters an, auf die Schaltfläche "hinzufügen" Einbeziehung in die `SELECT` Abfrage.

In diesem Beispiel können s nur diese Ergebnisse zurückgeben, in denen die `UnitPrice` Wert ist kleiner als oder gleich 25,00. Wählen Sie aus diesem Grund `UnitPrice` aus der Dropdown-Spaltenliste und &lt;= aus der Dropdownliste Operator. Bei Verwendung von hartcodierten Parameterwert (z. B. 25,00), oder wenn der Parameterwert ist programmgesteuert angegeben werden, wählen Sie keine aus der Dropdown-Quellliste. Als Nächstes geben Sie den hartcodierten Parameterwert im Textfeld Wert 25,00 aus, und schließen Sie den Prozess, indem Sie auf die Schaltfläche "hinzufügen".


[![Begrenzen der zurückgegebenen Ergebnisse der hinzufügen WHERE-Klausel (Dialogfeld)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Abbildung 2**: Einschränken der Ergebnisse zurückgegeben hinzufügen `WHERE` -Klausel (Dialogfeld) ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Klicken Sie nachdem der Parameter hinzugefügt wurde auf OK, um beim Konfigurieren von Datenquellen-Assistenten zurückzukehren. Die `SELECT` -Anweisung am Ende des Assistenten enthalten sollte nun eine `WHERE` -Klausel mit einem Parameter namens `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Wenn Sie angeben, dass mehrere Bedingungen in der `WHERE` Klausel hinzufügen `WHERE` Dialogfeld '-Klausel, um der Assistenten verknüpft sie mit der `AND` Operator. Wenn Sie müssen eine `OR` in der `WHERE` -Klausel (z. B. `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) müssen Sie erstellen die `SELECT` -Anweisung über den Bildschirm der benutzerdefinierten SQL-Anweisung.


Abzuschließen Sie die SqlDataSource (klicken Sie auf Weiter und beenden) Konfiguration, und überprüfen Sie dann das deklarative SqlDataSource-s-Markup. Das Markup enthält jetzt eine `<SelectParameters>` -Auflistung, die ausgeschrieben Quellen für die Parameter in der `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Wenn die SqlDataSource-s `Select()` Methode wird aufgerufen, die `UnitPrice` Parameterwert (25,00) wird angewendet, um die `@UnitPrice` Parameter in der `SelectCommand` vor dem Senden an die Datenbank. Das Ergebnis ist, dass nur die Produkte, die kleiner oder gleich 25,00 werden zurückgegeben, aus der `Products` Tabelle. Vergewissern Sie sich hierzu hinzuzufügen GridView auf der Seite, auf diese Datenquelle zu binden Sie und zeigen Sie dann auf der Seite über einen Browser. Sie sollten nur aufgeführten Produkte angezeigt, die kleiner oder gleich 25,00, wie in Abbildung 3 bestätigt werden.


[![Nur solche Produkte kleiner oder gleich dem 25,00 werden angezeigt.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Abbildung 3**: nur solche Produkte kleiner oder gleich dem 25,00 angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Schritt 2: Hinzufügen von Parametern an eine benutzerdefinierte SQL-Anweisung

Beim Hinzufügen einer benutzerdefinierten SQL-Anweisung können Sie geben die `WHERE` Klausel explizit, oder geben Sie einen Wert in der Zelle Filter den Abfrage-Generator. Um dies zu demonstrieren, können Sie s, die nur die Produkte in einer GridView anzeigen, deren Preis unter einen bestimmten Schwellenwert liegen. Durch Hinzufügen eines Textfelds zum Starten der `ParameterizedQueries.aspx` Seite, um diesen Schwellenwert vom Benutzer zu erfassen. Legen Sie das Textfeld s `ID` Eigenschaft `MaxPrice`. Fügen Sie ein Websteuerelement Schaltfläche hinzu, und legen Sie dessen `Text` Eigenschaft, um übereinstimmende Produkte angezeigt.

Als Nächstes eine GridView auf die Seite ziehen, und wählen Sie das Smarttag, erstellen Sie eine neue, mit dem Namen ': SqlDataSource `ProductsFilteredByPriceDataSource`. Aus dem Konfigurieren von Datenquellen-Assistenten fortfahren die angeben, an eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur Bildschirm (siehe Abbildung 4), und geben Sie die folgende Abfrage:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Klicken Sie nachdem Sie die Abfrage eingegeben haben (entweder manuell oder über den Abfrage-Generator) auf "Weiter".


[![Nur solche Produkte zurück kleiner oder gleich einem Parameterwert](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Abbildung 4**: Zurückgeben nur solche Produkte kleiner oder gleich dem Parameterwert ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Da die Abfrage Parameter enthält, werden aufgefordert, dem nächsten Bildschirm des Assistenten uns für die Quelle der Parameterwerte. Wählen Sie aus der Parameterliste für Quelle Dropdown-Steuerelement und `MaxPrice` (dem TextBox-Steuerelement s `ID` Wert) aus der ControlID Dropdown-Liste. Sie können auch einen Optionaler Standardwert in die Groß-/Kleinschreibung verwendet, in denen der Benutzer verfügt über Text in nicht eingegeben, Eingeben der `MaxPrice` Textfeld. Geben Sie für die zurzeit keinen Standardwert ein.


[![Die MaxPrice TextBox-s-Text-Eigenschaft wird als Quelle für Parameter verwendet.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Abbildung 5**: die `MaxPrice` Textfeld s `Text` Eigenschaft dient als Quelle für Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Schließen Sie den Assistenten für die Datenquelle konfigurieren, indem Sie auf Weiter, dann auf Fertig stellen. Das deklarative Markup für die GridView, Textfeld, Schaltfläche und SqlDataSource lautet folgendermaßen:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Beachten Sie, dass dem Parameter innerhalb der SqlDataSource s `<SelectParameters>` Abschnitt ist eine `ControlParameter`, darunter zusätzliche Eigenschaften wie `ControlID` und `PropertyName`. Wenn die SqlDataSource-s `Select()` Methode wird aufgerufen, die `ControlParameter` grabs den Wert aus der angegebenen Eigenschaft für die Web-Steuerelements und weist ihn dem entsprechenden Parameter in der `SelectCommand`. In diesem Beispiel wird die `MaxPrice` Text-Eigenschaft wird verwendet, als die `@MaxPrice` Parameterwert.

Nehmen Sie zum Anzeigen dieser Seite über einen Browser an. Beim ersten Seite besuchen, oder wenn die `MaxPrice` TextBox verfügt nicht über einen Wert, der keine Datensätze in der GridView angezeigt werden.


[![Sind keine Datensätze angezeigt, wenn die MaxPrice Textfeld leer ist.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Abbildung 6**: keine Datensätze werden angezeigt, wenn die `MaxPrice` Textfeld ist leer ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Der Grund sind keine Produkte angezeigt liegt daran, dass standardmäßig eine leere Zeichenfolge für einen Parameterwert in einer Datenbank konvertiert wird `NULL` Wert. Da der Vergleich von `[UnitPrice] <= NULL` ergibt immer als "false", werden keine Ergebnisse zurückgegeben.

Geben Sie einen Wert in das Textfeld ein, z. B. 5,00, und klicken Sie auf die Schaltfläche mit den übereinstimmenden Produkte angezeigt. Beim Postback informiert die SqlDataSource an die GridView, dass eine der Quellen Parameter geändert wurde. Folglich bindet die GridView, die ': SqlDataSource dieser Produkte kleiner oder gleich $5,00 angezeigt.


[![Produkte kleiner oder gleich dem $5,00 werden angezeigt.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Abbildung 7**: Produkte kleiner oder gleich $5,00 angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Anfänglich anzeigen alle Produkte

Anstatt zum Anzeigen von keine Produkte beim ersten Laden der Seite "wird möglicherweise angezeigt werden soll *alle* Produkte. Eine Möglichkeit zum Auflisten aller Produkte immer die `MaxPrice` Textfeld leer ist der Standardwert des Parameters s auf einigen können, ausgesprochen hohen Wert festgelegt wie 1000000, da es 1.000.000 $ s unwahrscheinlich, dass Northwind Traders wird jemals, deren Preis pro Einheit inventarisiert wurden überschreitet. Dieser Ansatz ist Ansichtsweise allerdings und funktionieren ggf. nicht in anderen Situationen.

In vorherigen Lernprogrammen - [deklarative Parameter](../basic-reporting/declarative-parameters-cs.md) und [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) wir ein ähnliches Problem Kundenangebote wurden. Unsere Lösung vorhanden war, diese Logik in der Business Logic Layer eingefügt werden soll. Insbesondere die BLL den eingehenden Wert überprüft und, wenn es war `NULL` oder einige reservierte Wert, der Aufruf an die DAL-Methode, die alle Datensätze zurückgegeben weitergeleitet wurde. Wenn der eingehende Wert einer normalen Filterwert war, eine Puffermethode wurde an die DAL-Methode, die eine SQL-Anweisung ausgeführt, die eine parametrisierte verwendet `WHERE` -Klausel mit den angegebenen Wert.

Leider umgehen wir die Architektur auf, wenn die SqlDataSource verwenden. Stattdessen müssen wir anpassen, die SQL-Anweisung aus, um alle Datensätze und beziehen Sie bei der `@MaximumPrice` Parameter ist `NULL` oder einige reservierten Wert. Für diese Übung Let s haben sie daher, auch wenn die `@MaximumPrice` -Parameters `-1.0`, klicken Sie dann *alle* Datensätze zurückgegeben werden sollen (`-1.0` fungiert als ein reservierter Wert an, da kein Produkt eine Negative aufweisenkann`UnitPrice`Wert). Um dies zu erreichen, können wir die folgende SQL-Anweisung verwenden:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Dies `WHERE` -Klausel zurück *alle* aufgezeichnet, wenn die `@MaximumPrice` entspricht `-1.0`. Der Parameterwert ist nicht `-1.0`, nur die Produkte, deren `UnitPrice` ist kleiner als oder gleich der `@MaximumPrice` Parameterwert zurückgegeben werden. Durch Festlegen des Standardwerts der `@MaximumPrice` Parameter `-1.0`, beim ersten Laden der Seite (oder wenn die `MaxPrice` Textfeld leer ist), `@MaximumPrice` ein Wert von `-1.0` und alle Produkte angezeigt.


[![Nun alle Produkte werden ist angezeigt, wenn die MaxPrice Textfeld leer.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Abbildung 8**: jetzt alle Produkte werden angezeigt, wenn die `MaxPrice` Textfeld ist leer ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Es gibt eine Reihe von Vorsichtsmaßnahmen beachten Sie bei diesem Ansatz. Beachten Sie zunächst, dass die s-Parameterdatentyp von ihr abgeleitet wird s-Verwendung in der SQL-Abfrage. Wenn Sie ändern die `WHERE` -Klausel aus `@MaximumPrice = -1.0` zu `@MaximumPrice = -1`, behandelt die Runtime den Parameter als ganze Zahl. Wenn Sie dann versuchen, weisen Sie die `MaxPrice` Textfeld in einen Dezimalwert (z. B. 5,00), da er 5,00 in eine ganze Zahl konvertieren kann, wird eine Fehlermeldung angezeigt. Zur Behebung des Problems, nehmen Sie sich, dass Sie `@MaximumPrice = -1.0` in der `WHERE` -Klausel oder besser noch, legen Sie die `ControlParameter` s-Objekt `Type` Eigenschaft in eine Dezimalzahl.

Zweitens können durch Hinzufügen der `OR @MaximumPrice = -1.0` auf die `WHERE` -Klausel das Abfragemodul kann nicht verwenden, einen Index auf `UnitPrice` (sofern eine vorhanden), wodurch Tabellensuche. Dadurch kann die Leistung beeinträchtigen, wenn es eine ausreichend große Anzahl von Datensätzen in gibt der `Products` Tabelle. Ein besserer Ansatz wäre, diese Logik in einer gespeicherten Prozedur zu verschieben, in dem ein `IF` Anweisung würde entweder auszuführen. eine `SELECT` Abfrage aus der `Products` Tabelle ohne einen `WHERE` -Klausel, wenn alle Datensätze zurückgegeben werden müssen oder eine, deren `WHERE` -Klausel enthält nur die `UnitPrice` Kriterien, so, dass ein Index verwendet werden kann.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Schritt 3: Erstellen und verwenden parametrisierte gespeicherte Prozeduren

Gespeicherte Prozeduren beinhalten einen Satz von Eingabeparametern, die dann verwendet werden können, in die SQL-Anweisungen, die in der gespeicherten Prozedur definiert. Beim Konfigurieren der SqlDataSource um eine gespeicherte Prozedur verwenden, die Eingabeparameter akzeptiert, können diese Parameterwerte mit den gleichen Verfahren wie bei Ad-hoc-SQL-Anweisungen angegeben werden.

Zur Veranschaulichung der Verwendung von gespeicherten Prozeduren in der ': SqlDataSource Let s, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank mit dem Namen `GetProductsByCategory`, der akzeptiert eines Parameters namens `@CategoryID` und gibt alle Spalten der Produkte zurück, dessen `CategoryID` entspricht der Spalte `@CategoryID`. Um eine gespeicherte Prozedur zu erstellen, wechseln Sie zu dem Server-Explorer und Drilldown in die `NORTHWND.MDF` Datenbank. (Wenn Sie Ich möchte den Server-Explorer angezeigt wird, schalten Sie ihn von indem Sie im Menü "Ansicht" aufrufen und dabei die Option Server-Explorer.)

Aus der `NORTHWND.MDF` -Datenbank mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, wählen Sie die neue gespeicherte Prozedur hinzufügen, und geben Sie die folgende Syntax:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Klicken Sie auf die speichern-Symbol (oder STRG + S) zum Speichern der gespeicherten Prozedur. Sie können die gespeicherte Prozedur testen, indem Sie mit der rechten Maustaste in den Ordner gespeicherte Prozeduren und Execute. Daraufhin werden Sie aufgefordert, für die gespeicherte Prozedur s-Parameter (`@CategoryID`, in dieser Instanz), nach dem die Ergebnisse im Ausgabefenster angezeigt.


[![Die GetProductsByCategory gespeicherte Prozedur bei der Ausführung einer @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Abbildung 9**: die `GetProductsByCategory` gespeicherte Prozedur bei der Ausführung einer `@CategoryID` 1 ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Lassen Sie s, die diese gespeicherte Prozedur zu verwenden, um alle Produkte in der Kategorie "Getränke" in einem GridView anzuzeigen. Die Seite neue GridView hinzu, und binden es an eine neue, mit dem Namen ': SqlDataSource `BeverageProductsDataSource`. Geben Sie eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur Bildschirm weiterhin, wählen Sie das Optionsfeld für gespeicherte Prozedur, und wählen die `GetProductsByCategory` gespeicherten Prozedur aus der Dropdown Liste.


[![Wählen Sie die GetProductsByCategory gespeicherten Prozedur aus der Dropdown Liste](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Abbildung 10**: Wählen Sie die `GetProductsByCategory` gespeicherte Prozedur aus der Dropdown-Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Da die gespeicherte Prozedur einen Eingabeparameter akzeptiert (`@CategoryID`), Sie auf Weiter, fordert Sie uns die Quelle für diesen Parameter-s-Wert angegeben. Die Getränke `CategoryID` 1 ist, so behalten Sie für keine der Quellliste Dropdown-Parameter, und geben Sie 1 in das Textfeld "DefaultValue".


[![Verwenden Sie den hartcodierten Wert 1 die Produkte in der Kategorie "Getränke" zurückgegeben](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Abbildung 11**: Verwenden Sie die Produkte in der Kategorie "Getränke" zurückgegeben Hard-Coded Wert 1 ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Wie im folgenden deklarativem Markup gezeigt, bei Verwendung einer gespeicherten Prozedur, die ': SqlDataSource-s `SelectCommand` Eigenschaft auf den Namen der gespeicherten Prozedur festgelegt ist und die [ `SelectCommandType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) festgelegt ist, um `StoredProcedure`gibt an, die die `SelectCommand` ist der Name einer gespeicherten Prozedur, statt eine Ad-hoc-SQL-Anweisung.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Testen Sie die Seite in einem Browser aus. Nur die Produkte, die zur Kategorie "Getränke" gehören angezeigt, obwohl *alle* des Produkts werden Felder angezeigt, da die `GetProductsByCategory` gespeicherte Prozedur gibt alle Spalten aus der `Products` Tabelle. Wir können natürlich einschränken oder passen Sie die Felder in der GridView aus dem GridView s Spalten bearbeiten-Dialogfeld angezeigt.


[![Alle von Getränken werden angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Abbildung 12**: alle von Getränken werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Schritt 4: Programmgesteuerten Aufrufen SqlDataSource s`Select()`Anweisung

In den Beispielen wird in der vorherigen und dieses Lernprogramms bisher vorgefundener Ve SqlDataSource-Steuerelemente direkt an die GridView gebunden haben. Die SqlDataSource-Steuerelement s-Daten, jedoch können programmgesteuert zugegriffen und aufgelistet, die im Code. Dies kann besonders hilfreich sein, wenn Sie zum Abfragen von Daten benötigen, um es zu überprüfen, aber Don ' t müssen, um diese anzuzeigen. Anstatt aller der Textbaustein ADO.NET-Code zum Herstellen einer Verbindung mit der Datenbank und geben Sie den Befehl Abrufen der Ergebnisse zu schreiben, können Sie die SqlDataSource dieser monotonen Code behandeln lassen.

Zur Veranschaulichung der Arbeit mit dem SqlDataSource-s stellen Sie sich vor Daten programmgesteuert, dass Ihr Chef Sie mit einer Anforderung Tabellenpartitionierung wurde an eine Webseite erstellen, die den Namen einer zufällig ausgewählten Kategorie und seine zugehörigen Produkte anzeigt. D. h., wenn ein Benutzer auf dieser Seite besucht, wir nach dem Zufallsprinzip eine Kategorie aus auswählen möchten die `Categories` Tabelle, zeigen Sie den Namen der Kategorie und Listen Sie die Produkte, die zu dieser Kategorie gehören.

Um dies zu erreichen, die wir benötigen zwei SqlDataSource Steuerelemente in einem auf eine zufällige Kategorie aus der `Categories` Tabelle und eine andere Kategorie Produkte abgerufen. Wir müssen die SqlDataSource zu erstellen, einen zufälligen Kategoriedatensatz in diesem Schritt abruft; Schritt 5 prüft Objekt die SqlDataSource, die die Kategorie s Produkte abruft.

Durch Hinzufügen von einem SqlDataSource zum Starten `ParameterizedQueries.aspx` und legen Sie dessen `ID` auf `RandomCategoryDataSource`. Konfigurieren Sie es so, dass sie die folgende SQL-Abfrage verwendet:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()`Gibt die Datensätze in zufälliger Reihenfolge sortiert (finden Sie unter [Using `NEWID()` , nach dem Zufallsprinzip Sortieren von Datensätzen](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`Gibt den ersten Datensatz aus dem Resultset zurück. Zusammengestellt, die diese Abfrage gibt die `CategoryID` und `CategoryName` Spaltenwerte aus einer einzelnen, zufällig ausgewählten Kategorie.

Die Kategorie s angezeigt `CategoryName` -Wert, der Seite ein Label-Websteuerelement hinzugefügt haben, legen Sie seine `ID` Eigenschaft, um `CategoryNameLabel`, und löschen, dessen `Text` Eigenschaft. Um die Daten von einem SqlDataSource-Steuerelement programmgesteuert abzurufen, müssen wir Aufrufen seiner `Select()` Methode. Die [ `Select()` Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) erwartet einen einzelnen Eingabeparameter vom Typ [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), der angibt, wie die Daten vor der Rückgabe Nachrichten werden sollte. Dies kann die Anweisungen zum Sortieren und Filtern der Daten enthalten und wird durch die Daten, die Websteuerelemente beim Sortieren oder paging durch die Daten von einem SqlDataSource-Steuerelement verwendet. In unserem Beispiel jedoch wir Verschlüsselungskennwort t müssen die Daten vor der Rückgabe geändert werden, und daher wird auf die `DataSourceSelectArguments.Empty` Objekt.

Die `Select()` Methode gibt ein Objekt, das implementiert `IEnumerable`. Der genaue Typ zurückgegeben, hängt vom Wert des SqlDataSource-Steuerelement s [ `DataSourceMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Wie im vorherigen Lernprogramm erläutert wird, kann diese Eigenschaft festgelegt werden, auf den Wert entweder `DataSet` oder `DataReader`. Wenn auf festgelegt `DataSet`, die `Select()` Methode gibt ein ["DataView"](https://msdn.microsoft.com/library/01s96x0z.aspx) Objekt; gesetzt `DataReader`, es gibt ein Objekt, das implementiert [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Da die `RandomCategoryDataSource` SqlDataSource hat seine `DataSourceMode` -Eigenschaftensatz auf `DataSet` (Standard), wir arbeiten mit einer "DataView"-Objekt.

Im folgende Code wird veranschaulicht, wie die Datensätze aus Abrufen der `RandomCategoryDataSource` SqlDataSource als einer "DataView" sowie zum Lesen der `CategoryName` Spaltenwert aus der ersten Zeile von "DataView":


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]`Gibt das erste `DataRowView` in der DataView. `randomCategoryView[0]["CategoryName"]`Gibt den Wert der `CategoryName` Spalte in dieser ersten Zeile. Beachten Sie, dass die DataView lose typisierten ist. Einen bestimmter Spaltenwert verweisen muss den Namen der Spalte als Zeichenfolge (in diesem Fall CategoryName) übergeben werden. Abbildung 13 zeigt die Meldung angezeigt, der `CategoryNameLabel` beim Anzeigen der Seite ". Natürlich ist die tatsächliche Kategorienamen angezeigt nach dem Zufallsprinzip ausgewählt, die `RandomCategoryDataSource` SqlDataSource auf jeden Besuch der Seite "(einschließlich Postbacks).


[![Nach dem Zufallsprinzip ausgewählte Kategorie-s, auf die Namen angezeigt wird](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Abbildung 13**: Name angezeigt wird, der nach dem Zufallsprinzip ausgewählte Kategorie-s ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Wenn die SqlDataSource-, s Steuerelement `DataSourceMode` Eigenschaft festgelegt wurde `DataReader`, der Rückgabewert der `Select()` Methode würde in umgewandelt werden mussten `IDataReader`. Zum Lesen der `CategoryName` Spaltenwert aus der ersten Zeile, wir d verwenden Sie Code wie:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Mit dem SqlDataSource zufällig durch Auswahl einer Kategorie, wir re kann jetzt die GridView hinzugefügt, die der s Kategorieprodukte aufgeführt sind.

> [!NOTE]
> Statt ein Websteuerelement Bezeichnung der Kategorie-s-Name angezeigt, es konnte eine FormView oder DetailsView haben auf der Seite hinzugefügt binden es an die SqlDataSource. Verwenden die Bezeichnung ein, uns, wie Sie programmgesteuert aufrufen, die SqlDataSource s untersuchen jedoch zulässig `Select()` -Anweisung und zum Arbeiten mit der resultierenden Daten im Code.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Schritt 5: Zuweisen von Parameterwerten programmgesteuert

Alle Beispiele wir haben bisher vorgefundener in diesem Lernprogramm verwendet einen hartcodierten Parameterwert oder eine entnommen, bis eine der vordefinierten Parameter Quellen (Querystring-Wert, ein Websteuerelement auf der Seite "usw.). SqlDataSource-Steuerelement-s-Parameter können jedoch auch programmgesteuert festgelegt werden. Zum Beispiel mit aktuellen abschließen zu können, benötigen wir SqlDataSource, die alle Produkte, die auf eine angegebene Kategorie gehören zurückgibt. Diesen SqlDataSource müssen eine `CategoryID` Parameter, dessen Wert festgelegt werden muss, basierend auf der `CategoryID` von zurückgegebene Wert für die `RandomCategoryDataSource` SqlDataSource in der `Page_Load` -Ereignishandler.

Starten, indem eine GridView zur Seite hinzufügen und binden es an eine neue, mit dem Namen ': SqlDataSource `ProductsByCategoryDataSource`. Wie wir es in Schritt 3 getan haben, die SqlDataSource so konfigurieren, dass er ruft die `GetProductsByCategory` gespeicherte Prozedur. Behalten Sie die Parameter Quelle die Dropdown-Liste auf None, aber geben Sie einen Standardwert nicht auf, da wir diesen Wert programmgesteuert festgelegt wird.


[![Geben Sie keine Parameterquelle oder Standardwert](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Abbildung 14**: Geben Sie keine Parameterquelle oder Standardwert ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Nach Abschluss des Assistenten SqlDataSource sollte die resultierende deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Wir können Zuweisen der `DefaultValue` von der `CategoryID` Parameter programmgesteuert in die `Page_Load` Ereignishandler:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Durch diese hinzufügen enthält die Seite eine GridView, die die Produkte, die nach dem Zufallsprinzip ausgewählte Kategorie zugeordnet wird.


[![Geben Sie keine Parameterquelle oder Standardwert](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Abbildung 15**: Geben Sie keine Parameterquelle oder Standardwert ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Die ': SqlDataSource ermöglicht Entwicklern parametrisierte Abfragen definieren, deren Parameterwerte fest programmiert, aus vordefinierten Parameter Quellen abgerufen oder programmgesteuert zugewiesen werden können. In diesem Lernprogramm wurde erläutert, wie eine parametrisierte Abfrage aus der Datenquelle konfigurieren-Assistent für Ad-hoc-SQL-Abfragen und gespeicherte Prozeduren zu erstellen. Wir haben uns auch hartcodierter Parameter Quellen, ein Websteuerelement als Parameterquelle zu verwenden und den Parameterwert programmgesteuert festlegen.

Bietet genau wie mit der ObjectDataSource die SqlDataSource auch Funktionen, um die zugrunde liegenden Daten ändern. In den nächsten Lernprogrammen betrachten wir zum Definieren `INSERT`, `UPDATE`, und `DELETE` -Anweisungen mit der SqlDataSource. Sobald diese Anweisungen hinzugefügt wurden, können wir die integrierte einfügen, bearbeiten und Löschen von Features der GridView, DetailsView und FormView nutzen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Scott Clyde Randell Schmidt und Ken Pespisa. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](querying-data-with-the-sqldatasource-control-cs.md)
[Weiter](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
