---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 'Einführung in ASP.NET Web Pages: Anzeigen von Daten | Microsoft-Dokumentation'
author: tfitzmac
description: In diesem Tutorial erfahren Sie, wie Sie eine Datenbank in WebMatrix erstellen "und" Vorgehensweise beim Anzeigen von Daten auf einer Seite, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird davon ausgegangen, y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829574"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Einführung in ASP.NET Web Pages - Anzeige von Daten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie Sie eine Datenbank in WebMatrix erstellen "und" Vorgehensweise beim Anzeigen von Daten auf einer Seite, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird vorausgesetzt, Sie haben die Reihe über [Einführung in die Programmierung von ASP.NET Web Pages](../introducing-razor-syntax-c.md).
> 
> Sie lernen Folgendes:
> 
> - So verwenden Sie WebMatrix-Tools, um Datenbanken und Tabellen zu erstellen.
> - So verwenden Sie WebMatrix-Tools zum Hinzufügen von Daten in einer Datenbank.
> - Die Vorgehensweise beim Anzeigen von Daten aus der Datenbank auf einer Seite.
> - Informationen zum Ausführen von SQL-Befehlen in ASP.NET Web Pages.
> - Gewusst wie: Anpassen der `WebGrid` Hilfsmethode, die die Datenanzeige ändern und Hinzufügen von Paginierung und Sortierung.
>   
> 
> Features/Technologien erläutert:
> 
> - WebMatrix-Datenbanktools.
> - `WebGrid` -Hilfsprogramm.


## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie für ASP.NET Web Pages eingeführt wurden (*.cshtml* Dateien), auf die Grundlagen der Razor-Syntax und -Hilfsprogramme. In diesem Tutorial werden Sie beginnen, erstellen die eigentliche Webanwendung, die Sie für den Rest der Reihe verwenden. Die app ist eine einfache Movie-Anwendung, mit dem Sie die anzeigen, hinzufügen, ändern und Löschen von Informationen über Filme.

Wenn Sie mit diesem Lernprogramm fertig sind, werden Sie möglicherweise eine Movie-Liste anzeigen, die diese Seite ähnelt:

![WebGrid-Anzeige, die CSS-Klassennamen festgelegten Parametern enthält.](displaying-data/_static/image1.png)

Aber damit beginnen, müssen Sie eine Datenbank zu erstellen.

## <a name="a-very-brief-introduction-to-databases"></a>Eine sehr kurze Einführung in Datenbanken

In diesem Tutorial wird nur die briefest Einführung in Datenbanken bereit. Wenn Sie Datenbank-Erfahrung haben, können Sie in diesem kurzen Abschnitt überspringen.

Eine Datenbank enthält eine oder mehrere Tabellen, die Informationen enthalten &mdash; z. B. Tabellen, für Kunden, Bestellungen und Anbietern oder für Schüler/Studenten, Lehrer, Klassen und Noten. Strukturell gesehen ähnelt eine Datenbanktabelle ein Arbeitsblatt ein. Angenommen Sie, ein typisches Adressbuch. Für jeden Eintrag im Adressbuch (d. h. für jede Person) Sie haben verschiedene Angaben wie z. B. Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

![Beispiel-Datenbanktabelle als ein einfaches Raster](displaying-data/_static/image2.png)

(Zeilen werden manchmal als *Datensätze*, und Spalten werden manchmal als *Felder*.)

Für die meisten-Datenbanktabellen enthalten sind muss die Tabelle eine Spalte verwenden, die einen eindeutigen Wert ein, z. B. eine Kundennummer, Kontonummer und So weiter enthält. Dieser Wert wird als der Tabelle bezeichnet *Primärschlüssel*, und verwenden Sie zum Identifizieren jeder Zeile in der Tabelle. Im Beispiel ist die ID-Spalte der Primärschlüssel für das Adressbuch, die im vorherigen Beispiel gezeigt.

Ein Großteil der Arbeit in Webanwendungen besteht aus Lesen von Informationen aus der Datenbank und auf einer Seite anzuzeigen. Sie oft auch Informationen von Benutzern erfassen und in einer Datenbank hinzufügen oder ändern Sie Datensätze, die bereits in der Datenbank vorhanden sind. (Wir alle diese Vorgänge im Verlauf dieser tutorialreihe behandelt).

Für Datenbanken kann überaus komplex sein und kann spezielle Kenntnisse erforderlich. Für diese tutorialreihe müssen Sie jedoch nur grundlegende Konzepte, zu verstehen, die alle erläutert wird, wie Sie fortfahren.

## <a name="creating-a-database"></a>Erstellen einer Datenbank

WebMatrix enthält Tools, die zum Erstellen einer Datenbank und zum Erstellen von Tabellen in der Datenbank zu erleichtern. (Die Struktur einer Datenbank wird als der Datenbank bezeichnet *Schema*.) Für diese tutorialreihe, erstellen Sie eine Datenbank, die nur eine Tabelle enthält &mdash; Filme.

Öffnen Sie WebMatrix aus, wenn Sie dies nicht bereits getan haben, und öffnen Sie die WebPagesMovies-Website, die Sie im vorherigen Tutorial erstellt haben.

Klicken Sie im linken Bereich auf die **Datenbank** Arbeitsbereich.

![Registerkarte "WebMatrix-Datenbank-Arbeitsbereich"](displaying-data/_static/image3.png)

Die Menüband-Änderungen Aufgaben im Zusammenhang mit der Datenbank angezeigt werden soll. Klicken Sie im Menüband auf **neue Datenbank**.

![Schaltfläche "Neue Datenbank" WebMatrix-Menüband](displaying-data/_static/image4.png)

WebMatrix erstellt eine SQL Server CE-Datenbank (eine *.sdf* Datei), die den gleichen Namen hat, als Standort für die &mdash; *WebPagesMovies.sdf*. (Sie wird nicht dies hier tun, jedoch können Sie die Datei auf beliebigen, umbenennen, solange sie verfügt über eine *.sdf* Erweiterung.)

## <a name="creating-a-table"></a>Erstellen einer Tabelle

Klicken Sie im Menüband auf **neue Tabelle**. WebMatrix wird den Tabellen-Designer in einer neuen Registerkarte geöffnet. (Wenn die neue Tabellenoption nicht verfügbar ist, stellen Sie sicher, dass die neue Datenbank in der Strukturansicht auf der linken Seite ausgewählt ist.)

![Schaltfläche "Neue Tabelle" WebMatrix-Menüband](displaying-data/_static/image5.png)

Geben Sie in das Textfeld am oberen (, in dem das Wasserzeichen für "Name der Tabelle eingeben" heißt), "Movies" ein.

![Einen Tabellennamen einzugeben, in der Datenbank-Designer von WebMatrix](displaying-data/_static/image6.png)

Der Bereich unterhalb der Tabellenname ist, in dem Sie die einzelnen Spalten definieren. Für die *Filme* Tabelle in diesem Tutorial erstellen Sie nur wenige Spalten: *ID*, *Titel*, *"Genre"*, und *Jahr*.

In der **Namen** Geben Sie "ID". Alle Steuerelemente für die neue Spalte wird aktiviert, wenn Sie hier einen Wert eingeben.

Die Registerkarte der **Datentyp** aus, und wählen Sie **Int**. Dieser Wert gibt an, dass die ID-Spalte Daten für ganze Zahlen (Anzahl enthält).

> [!NOTE]
> Wird nicht nennen wir jegliche hier (wesentlich), jedoch können Sie standardmäßige Windows-tastaturgesten wechseln in diesem Raster. Beispielsweise können Sie mit der tab zwischen den Feldern, können Sie einfach beginnen mit der Eingabe, um ein Element in einer Liste auswählen und so weiter.


Letzte Registerkarte die **Standardwert** Feld (d. h. es leer lassen). TAB-Taste zu der **Primärschlüssel wird** Kontrollkästchen, und wählen Sie ihn. Diese Option wird die Datenbank angewiesen, die die *ID* Spalte enthält die Daten, die einzelne Zeilen zu identifizieren. (D. h. jede Zeile einen eindeutigen Wert in der ID-Spalte, die Sie verwenden können, um diese Zeile zu suchen müssen.)

Wählen Sie die **ist Identity** Option. Diese Option weist der Datenbank, dass die nächste laufende Nummer für jede neue Zeile automatisch generiert werden sollen. (Die **ist Identity** Wirkung der option nur, wenn Sie auch ausgewählt haben die **Primärschlüssel wird** Option.)

Klicken Sie in der nächsten Zeile des Datenblatts, oder drücken Sie die Tabulatortaste zweimal, um die aktuelle Zeile fertig zu stellen. Entweder Geste speichert die aktuelle Zeile und den darauffolgenden beginnt. Beachten Sie, dass die **Standardwert** Spalte sagt jetzt **Null**. (Null ist der Standardwert für den Standardwert, sozusagen.)

Wenn Sie die Definition der neuen abgeschlossen haben **ID** Spalte der Designer sieht dieser Abbildung:

![WebMatrix-Datenbank-Designer nach dem Definieren der ID-Spalte, für die Tabelle Filme](displaying-data/_static/image7.png)

Klicken Sie zum Erstellen der nächsten Spalte in das Feld in der **Namen** Spalte. Geben Sie für die Spalte "Title", und wählen Sie dann **Nvarchar** für die **Datentyp** Wert. Der Teil "Var" **Nvarchar** weist der Datenbank an, dass es sich bei die Daten für diese Spalte eine Zeichenfolge handelt, dessen Größe von Datensatz zu Datensatz variieren. (Das Präfix "n" stellt "national", was bedeutet, dass das Feld für alle Alphabet oder ein Schreibsystem-Zeichendaten enthalten kann: das Feld enthält, wird die Unicode-Daten.)

Bei der Auswahl **Nvarchar**, einem anderen Feld wird angezeigt, in dem Sie die maximale Länge für das Feld eingeben können. Geben Sie 50 ein, unter der Annahme, dass keine Filmtitel, die Sie in diesem Tutorial verwenden werden mehr als 50 Zeichen ist.

Skip **Standardwert** und deaktivieren Sie die **NULL-Werte zulassen** Option. Sie möchten nicht die Datenbank Filme, die in der Datenbank eingegeben werden können, die einen Titel besitzen.

Wenn Sie fertig sind und auf die nächste Zeile verschieben, sieht der Designer die folgende Abbildung aus:

![WebMatrix-Datenbank-Designer nach dem Definieren der Titelspalte für die Tabelle Filme](displaying-data/_static/image8.png)

Wiederholen Sie diese Schritte zum Erstellen einer Spalte mit dem Namen "Genre", mit Ausnahme von der Länge, es zu lediglich 30.

Erstellen Sie eine andere Spalte, die mit dem Namen "Year". Wählen Sie für den Datentyp **Nchar** (nicht **Nvarchar**) und die Länge auf 4 festgelegt. Für das Jahr machen Sie 4 Ziffern wie "1995" oder "2010" verwenden, damit Sie eine Spalte mit variabler Größe nicht benötigen.

Hier ist der fertige Entwurf aussieht:

![WebMatrix-Datenbank-Designer, nachdem alle Felder für die Tabelle Filme definiert sind](displaying-data/_static/image9.png)

Drücken Sie STRG + S, oder klicken Sie auf die **speichern** die Schnellzugriff-Symbolleiste die Schaltfläche. Schließen Sie den Datenbankdesigner, schließen Sie die Registerkarte.

## <a name="adding-some-example-data"></a>Einige Beispieldaten hinzufügen

Weiter unten in diesem Tutorial erstellen Sie eine Seite, in dem Sie neue Filme in einem Formular eingeben können. Jetzt können Sie jedoch einige Beispiele für Daten hinzufügen, die Sie anschließend auf einer Seite anzeigen können.

In der **Datenbank** Arbeitsbereich in WebMatrix, beachten Sie, dass es eine Struktur, die Aufschluss über die *.sdf* Datei, die Sie zuvor erstellt haben. Öffnen Sie den Knoten für Ihre neue *.sdf* Datei, und öffnen Sie dann die **Tabellen** Knoten.

![Arbeitsbereich "WebMatrix-Datenbank" Struktur Filme Tabelle öffnen](displaying-data/_static/image10.png)

Mit der rechten Maustaste die **Filme** Knoten und wählen Sie dann **Daten**. WebMatrix öffnet ein Raster, in dem Sie Daten eingeben können, die *Filme* Tabelle:

![Datenbank-Eintrag-Raster in WebMatrix (leer)](displaying-data/_static/image11.png)

Klicken Sie auf die **Titel** Spalte, und geben Sie "Bei der Harry erfüllt Sally". Verschieben Sie in der **"Genre"** Spalte (Sie können die Tab-Taste verwenden), und geben Sie "Romantisch Komödie". Verschieben Sie in der **Jahr** Spalte, und geben Sie "1989":

![Datenbank-Eintrag-Raster in WebMatrix mit einem Datensatz](displaying-data/_static/image12.png)

Drücken Sie die EINGABETASTE, und WebMatrix wird den neuen Film gespeichert. Beachten Sie, dass die **ID** Spalte ausgefüllt wurde.

![Datenbank-Eintrag-Raster in WebMatrix mit einem Datensatz und automatisch generierte ID](displaying-data/_static/image13.png)

Geben Sie einen anderen Film (z. B. "nicht mehr mit der Wind", "Drama", "1939"). Die ID-Spalte wird erneut ausgefüllt:

![Datenbank-Eintrag-Raster in WebMatrix mit zwei Datensätze und automatisch generierten IDs](displaying-data/_static/image14.png)

Geben Sie einen dritten Film (z. B. "Ghostbusters", "Komödie"). Lassen Sie zum Experimentieren die **Jahr** Spalte leer, und drücken Sie dann die EINGABETASTE. Da Sie nicht ausgewählt. die **NULL-Werte zulassen** option, die Datenbank wird ein Fehler angezeigt:

!["Ungültige Daten"-Fehler angezeigt, wenn ein erforderliche Spalte kein Wert angegeben ist](displaying-data/_static/image15.png)

Klicken Sie auf **OK** zurückgehen und beheben Sie den Eintrag (das Jahr für "Ghostbusters" ist 1984) und dann die EINGABETASTE drücken.

Geben Sie mehrere Filme bis 8 sind, oder damit. (8 eingeben erleichtert es funktioniert mit paging später noch mal. Aber wenn dies auf zu viele ist, geben Sie ein paar vorerst.) Die tatsächlichen Daten spielt keine Rolle.

![Datenbank-Eintrag-Raster in WebMatrix mit beiden Einträgen](displaying-data/_static/image16.png)

Wenn Sie alle Filme fehlerfrei eingegeben haben, sind die ID-Werte sequenziell. Wenn Sie versucht, einen unvollständige Movie-Datensatz zu speichern, können die ID-Nummern nicht sequenziell sein. Wenn dies der Fall ist, ist das kein Problem. Die Zahlen müssen keine Bedeutungen behaftet, und das einzige, die wichtig ist, dass sie in eindeutig sind die *Filme* Tabelle.

Schließen Sie die Registerkarte, die den Datenbank-Designer enthält.

Jetzt können Sie zum Anzeigen der Daten auf einer Webseite aktivieren.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Anzeigen von Daten auf einer Seite mit der WebGrid-Hilfsprogramm

Um Daten auf einer Seite anzuzeigen, also verwenden Sie die `WebGrid` Helper. Dieses Hilfsprogramm erzeugt eine Anzeige in einem Raster oder einer Tabelle (Zeilen und Spalten). Wie Sie sehen werden, werden Sie können kooperiert das Raster mit Formatierung und andere Features sein.

Um das Raster auszuführen, müssen Sie ein paar Zeilen Code schreiben. Diese Zeilen werden als eine Art von Muster für fast alle den Datenzugriff dienen, die Sie in diesem Tutorial.

> [!NOTE]
> Sie müssen den tatsächlich viele Optionen zum Anzeigen von Daten auf einer Seite; die `WebGrid` Helper ist nur eine. Wir gewählt, der für dieses Tutorial, da es die einfachste Möglichkeit zum Anzeigen von Daten ist und es ausreichend flexibel ist. In den nächsten Tutorial sehen Sie, wie Sie eine weitere "manuell" Möglichkeit zur Verwendung von Daten auf der Seite, wodurch Sie die bessere Kontrolle über die Vorgehensweise beim Anzeigen von Daten verwenden.


Klicken Sie im linken Bereich in WebMatrix auf die **Dateien** Arbeitsbereich.

Die neue Datenbank, die Sie erstellt haben, ist in der *App\_Daten* Ordner. Wenn der Ordner nicht vorhanden, von WebMatrix für Ihre neue Datenbank erstellt. (Der Ordner möglicherweise vorhanden waren, wenn Sie die Hilfsprogramme zuvor installiert hatten.)

Wählen Sie in der Strukturansicht den Stamm der Website ein. Sie müssen den Stamm der Website auswählen; Andernfalls kann die neue Datei hinzugefügt werden, für die App\_Datenordner.

Klicken Sie im Menüband auf **neu**. In der **wählen Sie einen Dateityp** wählen **CSHTML**.

In der **Namen** benennen Sie die neue Seite "Movies.cshtml":

!["Wählen Sie einen Dateityp" Dialogfeld Anzeigen der Seite "Movies"](displaying-data/_static/image17.png)

Klicken Sie auf die **OK** Schaltfläche. WebMatrix öffnet eine neue Datei mit einigen Skelett Elementen darin. Zunächst schreiben Sie Code zum wechseln, die Daten aus der Datenbank abzurufen. Anschließend fügen Sie Markup zum Anzeigen der Daten auf der Seite.

### <a name="writing-the-data-query-code"></a>Der Code der Abfrage Daten schreiben

Am oberen Rand der Seite zwischen dem `@{` und `}` Zeichen, geben Sie den folgenden Code. (Stellen Sie sicher, dass Sie diesen Code zwischen den öffnenden und schließenden geschweiften Klammern eingeben.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

Die erste Zeile wird die Datenbank, die Sie zuvor erstellt haben. das ist immer der erste Schritt vor dem nachgehen mit der Datenbank geöffnet. Sie erkennen die `Database.Open` Methodennamen der Datenbank zu öffnen. Beachten Sie, die Sie nicht einschließen *.sdf* im Namen. Die `Open` Methode wird davon ausgegangen, dass es gesuchten ein *.sdf* Datei (, also *WebPagesMovies.sdf*) und der *.sdf* Datei befindet sich in der *App\_ Daten* Ordner. (Wir erwähnt, die die *App\_Daten* Ordner ist reserviert, dieses Szenario ist eine der Quellen, in dem ASP.NET zu diesem Namen Annahmen.)

Wenn die Datenbank geöffnet wird, wird ein Verweis darauf in der benannten Variablen eingefügt `db`. (Das könnte etwas benannt werden.) Die `db` Variable ist, wie Sie ansonsten mit der Datenbank interagieren.

Die zweite Zeile ruft tatsächlich Daten in der Datenbank mithilfe der `Query` Methode. Beachten Sie, wie dieser Code funktioniert: die `db` Variable enthält einen Verweis auf die Datenbank geöffnet, und rufen Sie die `Query` Methode mithilfe der `db` Variable (`db.Query`).

Die Abfrage selbst ist eine SQL `Select` Anweisung. (Ein paar Hintergrundinformationen zur SQL, finden Sie auf die Erläuterung weiter unten). In der Anweisung `Movies` identifiziert die Tabelle auf Abfrage. Die `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können auch Spalten einzeln auflisten durch Kommas getrennt.)

Die Ergebnisse der Abfrage, sofern vorhanden, werden zurückgegeben und in zur Verfügung gestellt der `selectedData` Variable. In diesem Fall kann die Variable Namen.

Die dritte Zeile weist ASP.NET schließlich, dass Sie eine Instanz der verwenden möchten die `WebGrid` Helper. Sie erstellen (*instanziieren*) das Hilfsobjekt mithilfe der `new` Schlüsselwort, und übergeben sie die Ergebnisse der Abfrage über die `selectedData` Variable. Die neue `WebGrid` Objekt zusammen mit den Ergebnissen der Datenbankabfrage, stehen in der `grid` Variable. Sie benötigen dieses Ergebnis in einen Moment Zeit, um die Daten tatsächlich auf der Seite anzuzeigen.

In dieser Phase wird die Datenbank geöffnet worden ist, haben Sie die Daten abgerufen werden sollen, und Sie vorbereitet haben die `WebGrid` Helper mit diesen Daten. Als Nächstes wird das Markup auf der Seite zu erstellen.

> [!TIP] 
> 
> **Strukturierte Abfragesprache (SQL)**
> 
> SQL ist eine Sprache, die in den meisten relationalen Datenbanken für die Verwaltung von Daten in einer Datenbank verwendet wird. Es enthält Befehle, mit denen Sie Daten abrufen und aktualisieren und, mit denen Sie erstellen, ändern und Verwalten von Daten in Datenbanktabellen. SQL ist eine Programmiersprache (z. B. C#)) unterscheidet. Mit SQL Sie legen fest, die Datenbank Was soll, und es ist Aufgabe der Datenbank, um zu ermitteln, wie Sie die Daten abzurufen, oder führen Sie die Aufgabe. Hier sind Beispiele für einige SQL-Befehle und was sie tun:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Die erste `Select` Anweisung werden alle Spalten (angegeben durch `*`) aus der *Filme* Tabelle.
> 
> Die zweite `Select` Anweisung ruft die Spalten-ID, Name und Preis von Datensätzen in der *Produkt* Tabelle, deren Spaltenwerte Preis mehr als 10 ist. Die Ergebnisse zurückgegeben in alphabetischer Reihenfolge anhand der Werte der Spalte "Name". Wenn keine Datensätze auf die Preis Kriterien entsprechen, gibt der Befehl eine leere Menge zurück.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Dieser Befehl fügt einen neuen Datensatz in die *Produkt* Tabelle die Spalte "Name", "Croissant", die Spalte "Beschreibung", "A-unzuverlässigen begeistern" und der Preis auf 1,99 festlegen.
> 
> Beachten Sie, dass bei der Angabe eines nicht numerischen Wert, der Wert in einfache Anführungszeichen (keine doppelten Anführungszeichen, wie in c#) eingeschlossen ist. Sie verwenden diese Anführungszeichen ein, um Text oder Datumswerte, aber nicht um Zahlen.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Dieser Befehl löscht Datensätze in der *Produkt* Tabelle, deren Ablauf Datumsspalte älter als der 1. Januar 2008 ist. (Der Befehl setzt voraus, dass die *Produkt* Tabelle besitzt eine solche Spalte natürlich.) Das Datum wird hier im Format MM/TT/JJJJ eingegeben, aber es muss angegeben werden, in dem Format, das für Ihr Gebietsschema verwendet wird.
> 
> Die `Insert` und `Delete` Befehle keine Resultsets zurückgeben. Stattdessen geben sie eine Zahl, die Aufschluss darüber gibt, wie viele Datensätze betroffen sind, durch den Befehl zurück.
> 
> Für einige dieser Vorgänge (wie einfügen und Löschen von Datensätzen) wird der Prozess, der den Vorgang angefordert wird, die entsprechenden Berechtigungen verfügen, in der Datenbank aufweist. Dies ist deshalb für Produktionsdatenbanken, die Sie häufig einen Benutzernamen und ein Kennwort angeben, wenn Sie eine Verbindung mit der Datenbank herstellen müssen.
> 
> Es gibt Dutzende von SQL-Befehle, aber sie alle folgen einem Muster wie die Befehle, die Sie hier sehen. Sie können SQL-Befehlen erstellen Sie die Datenbanktabellen, die Anzahl der Datensätze in einer Tabelle, Preise berechnen und viele weitere Vorgänge ausführen.


### <a name="adding-markup-to-display-the-data"></a>Hinzufügen von Markup zum Anzeigen der Daten

In der `<head>` festlegen-Inhalt des Elements der `<title>` Element zu "Movies":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

In der `<body>` -Element der Seite, fügen Sie Folgendes hinzu:

[!code-html[Main](displaying-data/samples/sample3.html)]

Das ist alles. Die `grid` Variablen ist der Wert, der Sie erstellt haben, bei der Erstellung der `WebGrid` -Objekt im Code oben.

Klicken Sie in der WebMatrix-Strukturansicht mit der rechten Maustaste in der Seite, und wählen **in Browser starten**. Sie sehen etwa wie auf dieser Seite:

![Standardausgabe WebGrid-Hilfsprogramm aus der Tabelle Filme](displaying-data/_static/image18.png)

Klicken Sie auf Link einer Spaltenüberschrift nach dieser Spalte sortiert. Durch Klicken auf eine Spaltenüberschrift sortieren kann, ist ein Feature, das in integriert ist die **WebGrid** Helper.

Die `GetHtml` -Methode, wie der Name schon sagt, generiert das Markup, das Daten anzeigt. In der Standardeinstellung die `GetHtml` Methode generiert ein HTML-Code `<table>` Element. (Wenn Sie möchten, können Sie das Rendering überprüfen, indem Sie den Quellcode der Seite im Browser ansehen.)

## <a name="modifying-the-look-of-the-grid"></a>Ändern das Aussehen des Rasters

Mithilfe der `WebGrid` Hilfsprogramm wie eben durchgeführt ist einfach, aber die resultierende Anzeige ist einfacher. Die `WebGrid` Hilfsprogramm bietet Optionen, mit denen Sie steuern, wie die Daten angezeigt werden können. Es gibt viel zu viele in diesem Tutorial untersuchen, aber in diesem Abschnitt erhalten Sie einen Überblick über einige dieser Optionen. Einige zusätzliche Optionen werden in späteren Tutorials dieser Serie behandelt.

### <a name="specifying-individual-columns-to-display"></a>Angeben der einzelne Spalten zum Anzeigen

Um zu starten, können Sie angeben, dass nur bestimmte Spalten angezeigt werden soll. Standardmäßig lässt sich sagen, wie Sie gesehen haben, im Raster werden alle vier Spalten aus der *Filme* Tabelle.

In der *Movies.cshtml* Datei, ersetzen Sie die `@grid.GetHtml()` Markup, das Sie gerade hinzugefügt, durch Folgendes haben:

[!code-css[Main](displaying-data/samples/sample4.css)]

Um dem Hilfsobjekt die anzuzeigenden Spalten zu sagen, Sie enthalten eine `columns` -Parameter für die `GetHtml` -Methode und übergeben Sie eine Auflistung von Spalten. In der Auflistung geben Sie jede Spalte einschließen. Geben Sie eine einzelne Spalte zum Anzeigen von einschließlich einer `grid.Column` Objekt, und übergeben Sie den Namen der Datenspalte. (Diese Spalten müssen in die Ergebnisse der SQL-Abfrage enthalten sein, die `WebGrid` Hilfsprogramm kann nicht angezeigt werden Spalten, die nicht von der Abfrage zurückgegeben wurden.)

Starten Sie den *Movies.cshtml* Seite im Browser erneut, und dieses Mal erhalten Sie eine Anzeige wie folgt (Beachten Sie, dass kein ID-Spalte angezeigt wird):

![WebGrid-angezeigt werden, nur ausgewählte Spalten](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Ändern des Aussehens des Rasters

Es gibt einigen Weitere Optionen für die Anzeige von Spalten, von die einige in späteren Tutorials in dieser Gruppe beschrieben wird. Jetzt werden in diesem Abschnitt Möglichkeiten vorgestellt in dem Sie das Raster als Ganzes formatieren können.

In der `<head>` Abschnitt der Seite unmittelbar vor dem `</head>` markieren, fügen Sie die folgenden `<style>` Element:

[!code-css[Main](displaying-data/samples/sample5.css)]

Dieser CSS-Markup definiert Klassen, die mit dem Namen `grid`, `head`und so weiter. Sie können auch diese Stildefinitionen einfügen, in einer separaten *CSS* Datei, und verknüpfen, die auf der Seite. (In der Tat müssen Sie das weiter unten in dieser tutorialreihe tun.) Aber um die Dinge für dieses Tutorial zu vereinfachen, in der gleichen Seite, die die Daten anzeigt.

Nutzen Sie jetzt die `WebGrid` Hilfsprogramm mithilfe dieser Stil-Klassen. Das Hilfsprogramm hat eine Reihe von Eigenschaften (z. B. `tableStyle`) für genau diesen Zweck, Sie einen Klassennamen der CSS-Stil auf diese zuweisen und Klassennamens gerendert wird, im Rahmen von Markup, das vom Hilfsprogramm gerendert wird.

Ändern der `grid.GetHtml` Markup so, dass die It nun wie im folgenden Code sieht:

[!code-css[Main](displaying-data/samples/sample6.css)]

Was neu ist, dass Sie hinzugefügt haben `tableStyle`, `headerStyle`, und `alternatingRowStyle` Parameter für die `GetHtml` Methode. Diese Parameter haben den Namen der CSS-Formate festgelegt wurde, die Sie vorhin hinzugefügt.

Führen Sie die Seite, und dieses Mal sehen Sie ein Raster, das wesentlich einfacher als vor dem aussieht:

![WebGrid-Anzeige, die CSS-Klassennamen festgelegten Parametern enthält.](displaying-data/_static/image20.png)

Festzustellen, was die `GetHtml` Methode, die generiert werden, sehen Sie sich den Quellcode der Seite im Browser. Wir gehen hier nicht, aber der wichtigste Punkt ist, die durch Angabe von Parametern wie `tableStyle`, Sie verursacht das Raster zum Generieren von HTML-Tags wie folgt:

`<table class="grid">`

Die `<table>` Tag hatte eine `class` Attribut hinzugefügt wird, die auf eine CSS-Regeln verweist, die Sie zuvor hinzugefügt haben. Dieser Code zeigt das grundlegende Muster &mdash; unterschiedliche Parameter für die `GetHtml` können Sie ein Verweis auf CSS-Klassen, die die Methode dann zusammen mit dem Markup generiert. Wie Sie die CSS-Klassen verwenden, bleibt Ihnen überlassen.

## <a name="adding-paging"></a>Hinzufügen von Paging

Als der letzten Aufgabe in diesem Tutorial fügen Sie Paging in das Raster. Zurzeit ist kein Problem, um alle Ihre Filme auf einmal anzuzeigen. Aber wenn Sie Hunderte von Filmen hinzugefügt haben, würden die angezeigte Seite lang erhalten.

Ändern Sie im Seitencode, die Zeile, die erstellt die `WebGrid` Objekt in den folgenden Code:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Bevor Sie der einzige Unterschied ist, dass Sie hinzugefügt haben eine `rowsPerPage` Parameter, der auf 3 festgelegt ist.

Führen Sie die Seite. Das Raster zeigt 3 Zeilen an eine Zeit plus der Navigationslinks, die auf Ihrer Seite über den Filmen in Ihrer Datenbank zu ermöglichen:

![WebGrid-Anzeige mit auslagerungen](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial erfahren Sie, wie Razor und c#-Code verwenden, um Benutzereingaben in einem Formular abgerufen werden. Sie müssen ein Suchfeld auf der Seite "Movies" hinzufügen, damit Sie Filme nach Titel oder "Genre" finden können.

## <a name="complete-listing-for-movies-page"></a>Vollständige Liste für die Seite "Movies"

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Zurück](intro-to-web-pages-programming.md)
> [Weiter](form-basics.md)
