---
uid: web-pages/overview/data/5-working-with-data
title: Einführung in die Arbeit mit einer Datenbank in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird beschrieben, wie Sie Zugriff auf Daten aus einer Datenbank, und mithilfe von ASP.NET Web Pages anzeigen wird.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: a688761c87376aa93463c13eaa07858d3acb9dc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021689"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Einführung in die Arbeit mit einer Datenbank in der ASP.NET Web Pages (Razor) Sites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie auf Microsoft WebMatrix-Tools verwenden, um eine Datenbank in einer ASP.NET Web Pages (Razor)-Website erstellen und wie Sie die Seiten zu erstellen, mit denen Sie die anzeigen, hinzufügen, bearbeiten und Löschen von Daten.
> 
> **Sie lernen Folgendes:** 
> 
> - Vorgehensweise: erstellen eine Datenbank.
> - Informationen zum Herstellen einer datenbankverbindung.
> - Informationen zum Anzeigen von Daten auf einer Webseite.
> - Informationen zum Einfügen, aktualisieren und Löschen von Datenbankdatensätzen.
> 
> Dies sind die Funktionen, die in diesem Artikel:
> 
> - Arbeiten mit einer Microsoft SQL Server Compact Edition-Datenbank.
> - Arbeiten mit SQL-Abfragen.
> - Der `Database`-Klasse.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> In diesem Tutorial funktioniert auch mit WebMatrix 3. Sie können ASP.NET Web Pages-3 und Visual Studio 2013 (oder Visual Studio Express 2013 für Web) verwenden. die Benutzeroberfläche wird jedoch abweichen.


## <a name="introduction-to-databases"></a>Einführung in Datenbanken

Angenommen Sie, ein typisches Adressbuch. Für jeden Eintrag im Adressbuch (d. h. für jede Person) Sie haben verschiedene Angaben wie z. B. Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

Eine typische Herangehensweise an die Daten wie folgt Grafik ist als eine Tabelle mit Zeilen und Spalten. In der Datenbanksprache wird jede Zeile häufig als Datensatz bezeichnet. Jede Spalte (auch als Felder bezeichnet) enthält einen Wert für jede Art von Daten: Vorname, letzte Name und So weiter.

| **ID** | **Vorname** | **"LastName"** | **Adresse** | **E-Mail** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Oliver | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Für die meisten-Datenbanktabellen enthalten sind muss die Tabelle eine Spalte haben, die einen eindeutigen Bezeichner, z. B. eine Kundennummer, Kontonummer usw. enthält. Dies bezeichnet man als der tabellenspezifischen *Primärschlüssel*, und verwenden Sie zum Identifizieren jeder Zeile in der Tabelle. Im Beispiel ist die ID-Spalte der Primärschlüssel für das Adressbuch.

Mit diesem Überblick über Datenbanken können Sie erfahren, wie Sie eine einfache Datenbank erstellen und Vorgänge wie hinzufügen, ändern und Löschen von Daten.

> [!TIP] 
> 
> **Relationale Datenbanken**
> 
> Sie können Daten auf viele weisen, schließt auch Textdateien und Tabellen speichern. Für die meisten Unternehmen verwendet werden Daten dagegen in einer relationalen Datenbank gespeichert.
> 
> In diesem Artikel geht nicht sehr tief in Datenbanken. Allerdings finden Sie es möglicherweise hilfreich, ein wenig über diese zu verstehen. In einer relationalen Datenbank ist die Informationen logisch in separaten Tabellen unterteilt. Beispielsweise kann eine Datenbank für eine Schule separate Tabellen für Schüler/Studenten und Klasse-Angebote enthalten. Die Software (z. B. SQL Server) unterstützt leistungsstarke Datenbankbefehle, mit denen Sie dynamisch richten Sie Beziehungen zwischen den Tabellen. Beispielsweise können Sie die relationale Datenbank, um eine logische Beziehung zwischen Klassen und Schüler/Studenten herzustellen, um einen Zeitplan zu erstellen. Speichern von Daten in separaten Tabellen reduziert die Komplexität der Tabellenstruktur und verringert den Bedarf an redundante Daten in Tabellen speichern.


## <a name="creating-a-database"></a>Erstellen einer Datenbank

Dieses Verfahren zeigt, wie Sie eine Datenbank mit Namen SmallBakery mit dem SQL Server Compact-Datenbank-Entwurfstools, die in WebMatrix enthalten ist. Obwohl Sie eine Datenbank mithilfe von Code erstellen können, ist es üblicher, die Datenbank und die Datenbanktabellen, die mit einem Designtool wie WebMatrix erstellen.

1. Starten Sie WebMatrix, und klicken Sie auf der Seite "Schnellstart" auf **Websitevorlage aus**.
2. Wählen Sie **leere Website**, und klicken Sie in der **Standortname** Feld Geben Sie "SmallBakery", und klicken Sie dann auf **OK**. Die Website wird erstellt und in WebMatrix angezeigt.
3. Klicken Sie im linken Bereich auf die **Datenbanken** Arbeitsbereich.
4. Klicken Sie im Menüband auf **neue Datenbank**. Eine leere Datenbank wird mit dem gleichen Namen wie Ihre Website erstellt.
5. Erweitern Sie im linken Bereich die **SmallBakery.sdf** Knoten, und klicken Sie dann auf **Tabellen**.
6. Klicken Sie im Menüband auf **neue Tabelle**. WebMatrix wird den Tabellen-Designer geöffnet.

    ![[Image]](5-working-with-data/_static/image1.jpg)
7. Klicken Sie in der **Namen** Spalte, und geben Sie &quot;Id&quot;.
8. In der **Datentyp** Spalte **Int**.
9. Festlegen der **ist der Primärschlüssel?** und **identifizieren ist?** Optionen aus, **Ja**.

    Wie der Name schon sagt, **Primärschlüssel wird** teilt mit, dass es sich bei dieser Primärschlüssel der Tabelle werden der Datenbank. **Ist Identität** weist die Datenbank aus, um automatisch eine ID-Nummer für jeden neuen Datensatz zu erstellen und die nächste laufende Nummer (beginnend mit 1) zuweisen.
10. Klicken Sie auf die nächste Zeile. Der Editor wird gestartet, eine neuen Spaltendefinition.
11. Geben Sie für die Name-Wert, &quot;Namen&quot;.
12. Für **Datentyp**, wählen Sie &quot;Nvarchar&quot; und die Länge auf 50 festgelegt. Die *Var* Teil `nvarchar` weist der Datenbank an, dass es sich bei die Daten für diese Spalte eine Zeichenfolge handelt, dessen Größe von Datensatz zu Datensatz variieren. (Die *n* Präfix stellt *national*, gibt an, dass das Feld Zeichendaten enthalten kann, die Buchstaben darstellt oder ein Schreibsystem &#8212; , also, dass das Feld Unicode-Daten enthält.)
13. Legen Sie die **NULL-Werte zulassen** option **keine**. Dadurch wird erzwungen, die die *Namen* Spalte ist nicht leer.
14. Erstellen Sie mit der gleiche Vorgang, eine Spalte namens *Beschreibung*. Legen Sie **Datentyp** "Nvarchar" und 50 für die Länge und **NULL-Werte zulassen** auf "false".
15. Erstellen Sie eine Spalte mit dem Namen *Preis*. Legen Sie **Datentyp "Money"** und **NULL-Werte zulassen** auf "false".
16. Benennen Sie im Feld oben auf die Tabelle &quot;Produkt&quot;.

    Wenn Sie fertig sind, wird die Definition wie folgt aussehen:

    ![[Image]](5-working-with-data/_static/image2.jpg)
17. Drücken Sie STRG + S zum Speichern der Tabelle ein.

## <a name="adding-data-to-the-database"></a>Hinzufügen von Daten in der Datenbank

Jetzt können Sie einige Beispieldaten zu Ihrer Datenbank hinzufügen, die Sie später in diesem Artikel verwenden werden.

1. Erweitern Sie im linken Bereich die **SmallBakery.sdf** Knoten, und klicken Sie dann auf **Tabellen**.
2. Mit der rechten Maustaste in der Product-Tabelle, und klicken Sie dann auf **Daten**.
3. Geben Sie im Bearbeitungsbereich die folgenden Einträge aus:

    | **Name** | **Beschreibung** | **Preis** |
    | --- | --- | --- |
    | Ein | Dank der minutengenauen sauber jeden Tag. | 2.99 |
    | Strawberry Shortcake | Aus unseren Garten mit organische Erdbeeren ausgeführt wurden. | 9.99 |
    | Apple-Kreisdiagramm | Der zweite nur zum Ihrer Mutter Kreis. | 12.99 |
    | Pecan Kreis | Wenn Sie Pecans zufrieden sind, ist dies für Sie. | 10.99 |
    | Zitronen-Kreisdiagramm | Mit der besten Zitronen in der ganzen Welt ausgeführt wurden. | 11.99 |
    | Cupcakes | Ihre Kinder und "Kid" Sie werden diese lieben. | 7.99 |

    Beachten Sie, dass Sie keine Eingabe vornehmen, für die *Id* Spalte. Bei der Erstellung der *Id* Spalte festlegen seiner **ist Identity** Eigenschaft auf "true", wodurch sie automatisch gefüllt werden soll.

    Wenn Sie die Dateneingabe abgeschlossen sind, wird der Tabellen-Designer wie folgt aussehen:

    ![[Image]](5-working-with-data/_static/image3.jpg)
4. Schließen Sie die Registerkarte, die Daten in der Datenbank enthält.

## <a name="displaying-data-from-a-database"></a>Anzeigen von Daten aus einer Datenbank

Nachdem Sie eine Datenbank mit Daten darin haben, können Sie die Daten in einer ASP.NET-Webseite anzeigen. Um die Zeilen der Tabelle anzuzeigenden auszuwählen, verwenden Sie eine SQL-Anweisung, um einen Befehl handelt, den Sie in der Datenbank zu übergeben.

1. Klicken Sie im linken Bereich auf die **Dateien** Arbeitsbereich.
2. Erstellen Sie im Stammverzeichnis der Website, eine neue CSHTML-Seite namens *ListProducts.cshtml*.
3. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Öffnen Sie in der ersten Codeblock, der *SmallBakery.sdf* -Datei (Datenbank), die Sie zuvor erstellt haben. Die `Database.Open` Methode setzt voraus, dass die *.sdf* Datei befindet sich in Ihrer Website *App\_Daten* Ordner. (Beachten Sie, die Sie nicht angeben müssen die *.sdf* Erweiterung &#8212; in der Tat, wenn Sie dies tun, die `Open` Methode funktioniert nicht.)

    > [!NOTE]
    > Die *App\_Daten* Ordner ist ein spezieller Ordner in ASP.NET, das zum Speichern von Datendateien verwendet wird. Weitere Informationen finden Sie unter [Verbindung mit einer Datenbank](#SB_ConnectingToADatabase) weiter unten in diesem Artikel.

    Stellen Sie dann eine Anforderung zum Abfragen der Datenbank, indem Sie den folgenden SQL `Select` Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    In der Anweisung `Product` identifiziert die Tabelle auf Abfrage. Die `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können auch Spalten einzeln auflisten durch Kommas getrennt, wenn Sie nur einige der Spalten finden Sie unter.) Die `Order By` -Klausel gibt an, wie die Daten sortiert werden soll &#8212; in diesem Fall durch die *Namen* Spalte. Dies bedeutet, dass die Daten alphabetisch basierend auf dem Wert sortiert werden die *Namen* Spalte für jede Zeile.

    In den Text der Seite erstellt das Markup für eine HTML-Tabelle, die zum Anzeigen der Daten verwendet werden. In der `<tbody>` -Element, Sie verwenden eine `foreach` Schleife um einzeln auf jede Datenzeile abzurufen, die von der Abfrage zurückgegeben wird. Für jede Datenzeile, die Sie Erstellen einer Tabellenzeile HTML (`<tr>` Element). Anschließend erstellen Sie die HTML-Tabellenzellen (`<td>` Elemente) für jede Spalte. Jedes Mal, die Sie der Schleife wird durchlaufen die nächste verfügbare Zeile aus der Datenbank ist der `row` Variable (dies richten Sie der `foreach` Anweisung). Um eine einzelne Spalte aus der Zeile zu erhalten, können Sie `row.Name` oder `row.Description` oder alle der Name der Spalte ist Sie.
4. Führen Sie die Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite zeigt eine Liste wie folgt:

    ![[Image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Strukturierte Abfragesprache (SQL)**
> 
> SQL ist eine Sprache, die in den meisten relationalen Datenbanken für die Verwaltung von Daten in einer Datenbank verwendet wird. Es enthält Befehle, mit denen Sie Daten abrufen und aktualisieren und, mit denen Sie erstellen, ändern und Verwalten von Datenbanktabellen. SQL unterscheidet sich von einer Programmiersprache (z. B. die Version, die Sie in WebMatrix verwenden), da mit SQL die Idee ist, dass Sie der Datenbank mitteilen, was Sie möchten, und es Aufgabe der Datenbank ist, um zu ermitteln, wie Sie die Daten abzurufen, oder führen Sie die Aufgabe. Hier sind Beispiele für einige SQL-Befehle und was sie tun:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Abruft, dies die *Id*, *Namen*, und *Preis* Spalten aus der Datensätze in der *Produkt* Tabelle, wenn der Wert des *Preis* mehr als 10 ist, und gibt die Ergebnisse zurück, in alphabetischer Reihenfolge anhand der Werte von der *Namen* Spalte. Dieser Befehl gibt ein Resultset zurück, die die Datensätze, die die Kriterien oder eine leere Menge zu erfüllen enthält, wenn keine Datensätze entsprechen.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Dies fügt einen neuen Datensatz in die *Produkt* Tabelle Festlegen der *Namen* Spalte &quot;Croissant&quot;, *Beschreibung* Spalte &quot; Ein unzuverlässiger begeistern&quot;, und der Preis, 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Dieser Befehl löscht Datensätze in der *Produkt* Tabelle, deren Ablauf Datumsspalte älter als der 1. Januar 2008 ist. (Dies setzt voraus, dass die *Produkt* Tabelle besitzt eine solche Spalte natürlich.) Das Datum wird hier im Format MM/TT/JJJJ eingegeben, aber es muss angegeben werden, in dem Format, das für Ihr Gebietsschema verwendet wird.
> 
> Die `Insert Into` und `Delete` Befehle keine Resultsets zurückgeben. Stattdessen geben sie eine Zahl, die Aufschluss darüber gibt, wie viele Datensätze betroffen sind, durch den Befehl zurück.
> 
> Für einige dieser Vorgänge (wie einfügen und Löschen von Datensätzen) wird der Prozess, der den Vorgang angefordert wird, die entsprechenden Berechtigungen verfügen, in der Datenbank aufweist. Dies ist deshalb für Produktionsdatenbanken, häufig müssen Benutzername und Kennwort angeben, wenn Sie eine Verbindung mit der Datenbank herstellen.
> 
> Es gibt Dutzende von SQL-Befehle, aber sie alle folgen einem Muster wie folgt. Sie können SQL-Befehlen erstellen Sie die Datenbanktabellen, die Anzahl der Datensätze in einer Tabelle, Preise berechnen und viele weitere Vorgänge ausführen.


## <a name="inserting-data-in-a-database"></a>Einfügen von Daten in einer Datenbank

In diesem Abschnitt wird gezeigt, wie Sie eine Seite erstellen, die Benutzer auf ein neues Produkt hinzufügen können die *Produkt* Datenbanktabelle. Nachdem Sie ein neuen Produktdatensatz eingefügt wird, auf die Seite zeigt die aktualisierte Tabelle mithilfe der *ListProducts.cshtml* Seite, die Sie im vorherigen Abschnitt erstellt haben.

Die Seite enthält die Überprüfung, um sicherzustellen, dass die Daten, die der Benutzer gibt für die Datenbank gültig ist. Code auf der Seite wird beispielsweise sichergestellt, dass es sich bei ein Wert für alle erforderlichen Spalten eingegeben wurde.

1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *InsertProducts.cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Der Hauptteil der Seite enthält eine HTML-Formular mit drei Textfelder, mit denen Benutzer geben Sie einen Namen, Beschreibung und Preis. Wenn der Benutzer klicken auf die **einfügen** Schaltfläche am oberen Rand der Seite Code öffnet eine Verbindung mit der *SmallBakery.sdf* Datenbank. Sie rufen dann die Werte, die der Benutzer mit übermittelt hat die `Request` Objekt aus, und weisen Sie diese Werte zu lokalen Variablen.

    Um zu überprüfen, dass der Benutzer einen Wert für jede erforderliche Spalte eingegeben haben, registrieren Sie jede `<input>` -Element, das Sie überprüfen möchten:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Die `Validation` Helper überprüft, ob es ein Wert in jedes der Felder, die Sie registriert haben. Sie können testen, ob alle Felder anhand validiert `Validation.IsValid()`, die Sie in der Regel tun, bevor Sie die Informationen verarbeiten, Sie vom Benutzer erhalten:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Die `&&` bedeutet, dass Operator und – bei diesem Test wird *ist dies eine formularübertragung und alle Felder haben die Überprüfung bestanden*.)

    Wenn alle Spalten überprüft (keine war leer), Sie fahren Sie fort, und erstellen Sie eine SQL-Anweisung zum Einfügen der Daten, und führen sie dann, wie im folgenden gezeigt:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Für die einzufügenden Werte, enthalten Sie Platzhalter für Parameter (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Als Sicherheitsmaßnahme immer übergeben Sie Werte an eine SQL-Anweisung mit Parametern, wie Sie im vorherigen Beispiel sehen. Dies bietet Ihnen die Möglichkeit, die Daten des Benutzers zu überprüfen, und es schützt vor versuchen, schädliche Befehle mit der Datenbank (auch als SQL Injection-Angriffe bezeichnet) zu senden.

    Um die Abfrage auszuführen, verwenden Sie diese Anweisung, übergibt Sie an die Variablen, die die Werte als Ersatz für die Platzhalter enthalten:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Nach der `Insert Into` -Anweisung ausgeführt wurde, die Sie zur Seite, die die Produkte, die über diese Befehlszeile führt den Benutzer senden:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Wenn die Überprüfung nicht erfolgreich war, überspringen Sie die Einfügung. Stattdessen müssen Sie eine Hilfsprogramm, auf der Seite, die die akkumulierten Fehlermeldungen (falls vorhanden) angezeigt werden kann:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Beachten Sie, dass die Style-Block in das Markup mit dem Namen eine CSS-Klassendefinition enthält `.validation-summary-errors`. Dies ist der Name der CSS-Klasse, mit dem Standardwert für die `<div>` -Element, das sämtliche Validierungsfehler enthält. In diesem Fall die CSS-Klasse gibt, dass zusammenfassende Validierungsfehler in Rot angezeigt werden und in Fettdruck, aber Sie definieren können, die `.validation-summary-errors` Klasse, um anzuzeigen, alle Formatierungen, die Ihnen gefällt.

### <a name="testing-the-insert-page"></a>Testen der Seite "Einfügen"

1. Die Seite in einem Browser anzeigen. Die Seite zeigt ein Formular, das dem ähnelt, die in der folgenden Abbildung angezeigt wird.

    ![[Image]](5-working-with-data/_static/image5.jpg)
2. Geben Sie Werte für alle Spalten, aber stellen Sie sicher, dass Sie belassen die *Preis* Spalte leer.
3. Klicken Sie auf **einfügen**. Die Seite zeigt eine Fehlermeldung an, wie in der folgenden Abbildung dargestellt. (Es wird kein neuer Datensatz erstellt.)

    ![[Image]](5-working-with-data/_static/image6.jpg)
4. Füllen Sie das Formular vollständig, und klicken Sie dann auf **einfügen**. Dieses Mal die *ListProducts.cshtml* Seite wird angezeigt, und zeigt Sie den neuen Datensatz.

## <a name="updating-data-in-a-database"></a>Aktualisieren von Daten in einer Datenbank

Nachdem die Daten in eine Tabelle eingegeben wurde, müssen Sie es aktualisieren. Dieses Verfahren zeigt, wie Sie zwei Seiten zu erstellen, die denen ähneln, die Sie zum Einfügen von Daten zuvor erstellt haben. Die erste Seite zeigt Produkte und ermöglicht Benutzern, die eine ändern auswählen. Die zweite Seite können die Benutzer tatsächlich die Bearbeitungen vornehmen und speichern Sie sie.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite, Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Weitere Informationen über das Einrichten der Mitgliedschaft und zu Möglichkeiten, um Benutzern das Ausführen von Aufgaben auf der Website zu autorisieren, finden Sie unter [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *EditProducts.cshtml*.
2. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Der einzige Unterschied zwischen dieser Seite und die *ListProducts.cshtml* Seite zuvor besteht, dass die HTML-Tabelle auf dieser Seite eine zusätzliche Spalte enthält, die zeigt eine **bearbeiten** Link. Wenn Sie diesen Link klicken, gelangen Sie zum dem *UpdateProducts.cshtml* -Seite (die Sie als Nächstes erstellen) in dem Sie den ausgewählten Datensatz bearbeiten können.

    Sehen Sie sich den Code, erstellt die **bearbeiten** Link:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Dadurch wird eine HTML erstellt `<a>` Element, dessen `href` Attribut dynamisch festgelegt ist. Die `href` Attribut gibt an, die Seite angezeigt wird, wenn der Benutzer auf den Link klickt. Sie übergibt außerdem die `Id` Wert der aktuellen Zeile, um den Link. Wenn die Seite ausgeführt wird, könnte den Quellcode der Seite Links wie diesen enthalten:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Beachten Sie, dass die `href` -Attributsatz auf `UpdateProducts/n`, wobei *n* ist eine Produkt-Zahl. Wenn ein Benutzer einen dieser Links klickt, wird die resultierende URL etwa wie folgt aussehen:

    `http://localhost:18816/UpdateProducts/6`

    Das heißt, werden die Produktnummer, die bearbeitet werden, in der URL übergeben.
3. Die Seite in einem Browser anzeigen. Die Seite zeigt die Daten in einem Format wie folgt:

    ![[Image]](5-working-with-data/_static/image7.jpg)

    Als Nächstes erstellen Sie die Seite, die Benutzer tatsächlich die Daten aktualisieren können. Die Update-Seite umfasst eine Überprüfung der zum Überprüfen der Daten, die der Benutzer eingibt. Code auf der Seite wird beispielsweise sichergestellt, dass es sich bei ein Wert für alle erforderlichen Spalten eingegeben wurde.
4. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *UpdateProducts.cshtml*.
5. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Der Hauptteil der Seite enthält ein HTML-Formular, in denen ein Produkt angezeigt wird und in dem Sie Benutzer bearbeiten können. Rufen Sie das Produkt aus, um anzuzeigen, verwenden Sie diese SQL-Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Dies wird wählen Sie das Produkt, mit der ID entspricht den Wert, der übergeben wird, die `@0` Parameter. (Da *Id* ist die primary key- und aus diesem Grund muss eindeutig sein, kann nur eine Produktdatensatz jemals auf diese Weise ausgewählt werden.) Zum Abrufen der ID-Wert für die Übergabe dieser `Select` -Anweisung können Sie den Wert, der auf der Seite als Teil der URL übergeben wird, mit der folgenden Syntax lesen:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Um den Datensatz tatsächlich zu abzurufen, verwenden Sie die `QuerySingle` -Methode, die nur ein Datensatz zurückgeben:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Die einzelne Zeile zurückgegeben wird, in der `row` Variable. Sie können Abrufen von Daten aus jeder Spalte, und weisen sie Sie lokale Variablen wie folgt:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Im Markup für das Formular werden diese Werte automatisch in einzelne Textfelder angezeigt, mit eingebettetem Code wie folgt:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Dieser Teil des Codes zeigt den Datensatz aktualisiert werden. Nachdem der Datensatz angezeigt wurde, kann der Benutzer die einzelne Spalten bearbeiten.

    Wenn der Benutzer das Formular übermittelt, indem Sie auf die **Update** Schaltfläche den Code in die `if(IsPost)` -Block ausgeführt. Hiermit wird der Benutzer Werte aus der `Request` -Objekt, werden die Werte in Variablen gespeichert und validiert, dass jede Spalte gefüllt wurde. Wenn die Validierung erfolgreich war, erstellt der Code die folgende SQL-Update-Anweisung aus:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In einer SQL `Update` -Anweisung, die jede Spalte zu aktualisieren und den Wert auf festgelegt ist, dass Angabe. In diesem Code werden die Werte angegeben, über die Parameterplatzhalter `@0`, `@1`, `@2`und so weiter. (Wie bereits erwähnt aus Sicherheitsgründen sollten Sie immer Werte für eine SQL-Anweisung übergeben mithilfe von Parametern.)

    Beim Aufrufen der `db.Execute` -Methode übergeben Sie die Variablen, die die Werte in der Reihenfolge enthalten, die die Parameter in der SQL-Anweisung entspricht:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Nach der `Update` Anweisung ausgeführt wurde, rufen Sie die folgende Methode, um den Benutzer zurück an die Seite "Bearbeiten" umleiten:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Der Effekt ist, dass der Benutzer eine aktualisierte Liste der Daten in der Datenbank sieht und ein anderes Produkt bearbeiten kann.
6. Speichern Sie die Seite.
7. Führen Sie die *EditProducts.cshtml* Seite (nicht der Update-Seite), und klicken Sie dann auf **bearbeiten** ein Produkts zum Bearbeiten auswählen. Die *UpdateProducts.cshtml* angezeigt wird, den ausgewählte Datensatz angezeigt.

    ![[Image]](5-working-with-data/_static/image8.jpg)
8. Eine Änderung vornehmen, und klicken Sie auf **Update**. Die Liste der Produkte ist mit den aktualisierten Daten erneut angezeigt.

## <a name="deleting-data-in-a-database"></a>Löschen von Daten in einer Datenbank

In diesem Abschnitt wird gezeigt, wie Benutzer ein Produkt aus löschen lassen die *Produkt* Datenbanktabelle. Das Beispiel besteht aus zwei Seiten. Wählen Sie in der ersten Seite Benutzer einen Datensatz zu löschen. Der zu löschenden Datensatz wird in einer zweiten Seite angezeigt, mit dem sie bestätigen, dass sie den Datensatz löschen möchten.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite, Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Weitere Informationen über das Einrichten der Mitgliedschaft und zu Möglichkeiten, Benutzer zur Ausführung von Aufgaben auf der Website zu autorisieren, finden Sie unter [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *ListProductsForDelete.cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Diese Seite ähnelt der *EditProducts.cshtml* Seite aus. Aber anstatt einer **bearbeiten** Link für jedes Produkt, es zeigt eine **löschen** Link. Die **löschen** Link wird erstellt, mit dem folgenden eingebetteten Code in das Markup:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Erstellt eine URL, die wie folgt aussieht, wenn Benutzer auf den Link klicken:

    `http://<server>/DeleteProduct/4`

    Die URL aufruft, eine Seite namens *DeleteProduct.cshtml* (die Sie als Nächstes erstellen) und übergibt sie die ID des Produkts zu löschenden (in diesem Fall 4).
3. Speichern Sie die Datei, aber lassen Sie es offen.
4. Erstellen Sie eine andere CHTML-Datei, die mit dem Namen *DeleteProduct.cshtml*. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Auf dieser Seite wird aufgerufen, indem *ListProductsForDelete.cshtml* und ermöglicht Benutzern, die bestätigen, dass sie ein Produkt löschen möchten. Um das Produkt zu löschenden aufzulisten, erhalten Sie die ID des Produkts aus der URL löschen mit dem folgenden Code:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Die Seite muss der Benutzer auf eine Schaltfläche, um den Datensatz zu löschen. Dies ist eine wichtige Maßnahme: bei der Sie sicherheitsrelevante Vorgänge in Ihre Website aktualisieren oder Löschen von Daten ausführen, diese Vorgänge sollte immer erfolgen, verwenden einen POST-Vorgang, einen GET-Vorgang. Wenn Ihre Website festgelegt ist, damit Sie ein Löschvorgang mithilfe einer GET-Operation ausgeführt werden kann, kann jeder Benutzer eine URL wie übergeben `http://<server>/DeleteProduct/4` und beliebige Änderungen aus der Datenbank zu löschen. Hinzufügen von der Bestätigung, und Programmieren die Seite, damit der Löschvorgang ausgeführt werden kann, nur mithilfe eines BEITRAGS, fügen Sie ein Maß an Sicherheit zu Ihrer Website.

    Mit dem folgenden Code, der zuerst bestätigt, dass dies eine Post-Vorgang ist und die ID nicht leer ist, ist der eigentliche Löschvorgang erfolgt:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Der Code ausgeführt wird, eine SQL­Anweisung, die löscht den angegebenen Datensatz und leitet Sie dann den Benutzer zurück auf die Angebotsseite.
5. Führen Sie *ListProductsForDelete.cshtml* in einem Browser.

    ![[Image]](5-working-with-data/_static/image9.jpg)
6. Klicken Sie auf die **löschen** Link für eines der Produkte. Die *DeleteProduct.cshtml* Seite wird angezeigt, um sicherzustellen, dass Sie diesen Datensatz löschen möchten.
7. Klicken Sie auf die **löschen** Schaltfläche. Der Produktdatensatz gelöscht, und die Seite mit einer aktualisierten Produktliste aktualisiert wird.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Herstellen einer Verbindung mit einer Datenbank
> 
> Sie können in einer Datenbank auf zwei Arten verbinden. Die erste ist die Verwendung der `Database.Open` Methode und den Namen der Datenbankdatei angeben (weniger die *.sdf* Erweiterung):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Die `Open` Methode setzt voraus, dass die. *Sdf* Datei befindet sich in der Website *App\_Daten* Ordner. Dieser Ordner dient speziell für die Aufnahme von Daten. Beispielsweise kann die erforderlichen Berechtigungen zum Zulassen der Website zu lesen und Schreiben von Daten, und als Sicherheitsmaßnahme WebMatrix lässt keinen Zugriff auf Dateien aus diesem Ordner.
> 
> Die zweite Möglichkeit ist die Verwendung eine Verbindungszeichenfolge. Eine Verbindungszeichenfolge enthält Informationen zum Herstellen einer Verbindung mit einer Datenbank. Dies kann einen Dateipfad einschließen, oder er kann den Namen einer SQL Server-Datenbank auf einem lokalen oder remote-Server, zusammen mit einem Benutzernamen und Kennwort für die Verbindung zu diesem Server enthalten. (Wenn Sie Daten in einer zentral verwalteten Version von SQL Server speichern, verwenden z. B. am Standort eines Hostinganbieters, Sie immer eine Verbindungszeichenfolge auf die Datenbank-Verbindungsinformationen angeben.)
> 
> In WebMatrix werden Verbindungszeichenfolgen in der Regel in einer XML-Datei mit dem Namen gespeichert *"Web.config"*. Wie der Name schon sagt, können Sie eine *"Web.config"* Datei im Stammverzeichnis Ihrer Website in der Website-Konfigurationsinformationen, einschließlich Verbindungszeichenfolgen, die Ihre Site muss möglicherweise zu speichern. Ein Beispiel für eine Verbindungszeichenfolge in einer *"Web.config"* Datei könnte folgendermaßen aussehen:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Im Beispiel die Verbindungszeichenfolge verweist, in einer Datenbank in einer Instanz von SQL Server, die auf einem Server an einer beliebigen Stelle ausgeführt wird (im Gegensatz zu einer lokalen *.sdf* Datei). Müssen Sie die entsprechenden Namen für ersetzen `myServer` und `myDatabase`, und geben Sie Werte von SQL Server-Anmeldung für `username` und `password`. (Die Werte für Benutzername und Kennwort sind nicht unbedingt gleich als Ihre Windows-Anmeldeinformationen oder die Werte, die Ihr Hostinganbieter Sie zum Anmelden an ihren Servern erhalten haben. Überprüfen Sie mit dem Administrator die genauen Werte, die Sie benötigen.)
> 
> Die `Database.Open` Methode ist flexibel, da sie entweder den Namen einer Datenbank übergeben können *.sdf* Datei oder den Namen einer Verbindungszeichenfolge, die in gespeichert ist die *"Web.config"* Datei. Im folgenden Beispiel wird das Herstellen einer Verbindung mit der Datenbank mithilfe der Verbindungszeichenfolge, die im vorherigen Beispiel gezeigt:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Wie bereits erwähnt, die `Database.Open` Methode können Sie entweder ein Datenbankname oder eine Verbindungszeichenfolge übergeben und es werden ermitteln, was Sie verwenden. Dies ist sehr nützlich, bei der Bereitstellung (veröffentlichen) Ihrer Website. Können Sie eine *.sdf* Datei die *App\_Daten* Ordners beim Entwickeln und Testen Ihrer Website. Wenn Sie Ihre Website auf einem Produktionsserver verschieben, können Sie eine Verbindungszeichenfolge im Verwenden der *"Web.config"* -Datei mit dem gleichen Namen wie Ihre *.sdf* Datei, aber verweist auf des Hostinganbieters &#8212;ohne den Code ändern müssen.
> 
> Abschließend sollten Sie direkt mit einer Verbindungszeichenfolge zu arbeiten, können rufen Sie die `Database.OpenConnectionString` Methode und übergeben sie die tatsächliche Verbindungszeichenfolge anstatt nur der Name eines solchen Objekts in der *"Web.config"* Datei. Dies ist möglicherweise hilfreich in Situationen, in dem aus irgendeinem Grund Sie keinen Zugriff auf die Verbindungszeichenfolge (oder Werte, z. B. die *.sdf* Dateiname), bis die Seite ausgeführt wird. Allerdings in den meisten Fällen können Sie `Database.Open` wie in diesem Artikel beschrieben.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Herstellen einer Verbindung mit einer SQL Server- oder MySQL-Datenbank in WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
