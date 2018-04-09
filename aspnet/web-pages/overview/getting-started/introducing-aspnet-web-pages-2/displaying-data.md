---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Einführung in ASP.NET Web Pages – Anzeigen von Daten | Microsoft Docs
author: tfitzmac
description: In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in WebMatrix erstellen und Datenbankdaten auf einer Seite angezeigt, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird davon ausgegangen, y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Einführung in ASP.NET Web Pages - Anzeige von Daten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in WebMatrix erstellen und Datenbankdaten auf einer Seite angezeigt, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Einführung in ASP.NET Web Pages Programmierung](../introducing-razor-syntax-c.md).
> 
> Lernen Sie:
> 
> - Wie Sie die WebMatrix-Tools verwenden, um eine Datenbank als auch die Tabellen zu erstellen.
> - Wie Sie die WebMatrix-Tools verwenden, um Daten in einer Datenbank hinzufügen.
> - Vorgehensweise beim Anzeigen von Daten aus der Datenbank auf einer Seite.
> - Wie SQL-Befehlen in ASP.NET Web Pages ausgeführt.
> - Gewusst wie: Anpassen der `WebGrid` Helper so ändern Sie die Darstellung der Daten und zum Hinzufügen von paging und sortieren.
>   
> 
> Features/Technologien erläutert:
> 
> - WebMatrix Datenbanktools.
> - `WebGrid` Hilfsmethode.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Im vorherigen Lernprogramm, die Sie für ASP.NET Web Pages eingeführt wurden (*cshtml* Dateien), die Grundlagen der Razor-Syntax und Hilfsprogramme. In diesem Lernprogramm fügen Sie beginnen, erstellen die tatsächliche Webdienst-Anwendung, die Sie für den Rest der Reihe verwenden. Die app ist eine einfache Film-Anwendung, mit dem Sie die anzeigen, hinzufügen, ändern und Löschen von Informationen über Filme.

Wenn Sie mit diesem Lernprogramm fertig sind, werden es eine Film Auflistung anzeigen, die auf dieser Seite ähnelt:

![WebGrid-anzeigen, die Parameter legen Sie auf den Namen der CSS-Klasse](displaying-data/_static/image1.png)

Aber um zu beginnen, müssen Sie eine Datenbank zu erstellen.

## <a name="a-very-brief-introduction-to-databases"></a>Eine sehr kurze Einführung in Datenbanken

Dieses Lernprogramm stellt nur die briefest Einführung in Datenbanken bereit. Wenn Sie Datenbank-Erfahrung haben, können Sie in diesem kurzen Abschnitt überspringen.

Eine Datenbank enthält eine oder mehrere Tabellen, die Informationen enthalten &mdash; z. B. Tabellen für Kunden, Bestellungen und Lieferanten oder für Studenten, Bildungseinrichtungen Lehrer, Klassen und Qualitäten. Strukturell gesehen ähnelt eine Datenbanktabelle ein Arbeitsblatt ein. Angenommen Sie, ein typisches Adressbuch. Für jeden Eintrag im Adressbuch (d. h. für jede Person) stehen Ihnen verschiedene Angaben wie z. B. Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

![Beispiel-Datenbanktabelle als ein einfaches Raster](displaying-data/_static/image2.png)

(Zeilen werden manchmal als *Datensätze*, und Spalten werden manchmal als *Felder*.)

Für die meisten-Datenbanktabellen enthalten sind muss die Tabelle eine Spalte haben, die einen eindeutigen Wert ein, z. B. eine Kundennummer, Kontonummer und So weiter enthält. Dieser Wert ist bekannt, wie der Tabelle *Primärschlüssel*, und Sie verwenden, um jede Zeile in der Tabelle zu identifizieren. Im Beispiel ist die ID-Spalte der Primärschlüssel für das Adressbuch, die im vorherigen Beispiel gezeigt.

Ein Großteil der Arbeit in Webanwendungen besteht Lesen von Informationen aus der Datenbank sowie die Anzeige auf einer Seite. Sie häufig auch Sammeln von Informationen vom Benutzer und eine Datenbank hinzufügen oder bearbeiten Sie Datensätze, die bereits in der Datenbank befinden. (Müssen alle diese Vorgänge im Verlauf dieses Lernprogramms Satz eingegangen.)

Datenbank-Arbeit kann enorm komplex sein und kann keine speziellen Kenntnisse erfordern. Für dieses Lernprogramm gesetzt ist müssen Sie jedoch nur grundlegende Konzepte zu verstehen, die alle erläutert wird, wie Sie fortfahren.

## <a name="creating-a-database"></a>Erstellen einer Datenbank

WebMatrix enthält Tools, die zum Erstellen einer Datenbank und zum Erstellen von Tabellen in der Datenbank erleichtern. (Die Struktur einer Datenbank wird als der Datenbank bezeichnet *Schema*.) Für dieses Lernprogramm gesetzt ist, erstellen Sie eine Datenbank, die nur eine Tabelle enthält &mdash; Filme.

Öffnen Sie WebMatrix aus, wenn Sie dies nicht bereits getan haben, und öffnen Sie die WebPagesMovies-Website, die Sie im vorherigen Lernprogramm erstellt haben.

Klicken Sie im linken Bereich auf die **Datenbank** Arbeitsbereich.

![Registerkarte "Datenbank WebMatrix-Arbeitsbereich"](displaying-data/_static/image3.png)

Die Menüband-Änderungen Aufgaben im Zusammenhang mit der Datenbank angezeigt. Klicken Sie im Menüband auf **neue Datenbank**.

![Schaltfläche "Neue Datenbank" WebMatrix-Menüband](displaying-data/_static/image4.png)

WebMatrix erstellt eine SQL Server CE-Datenbank (eine *.sdf* Datei) hat, die den gleichen Namen wie Ihr Standort &mdash; *WebPagesMovies.sdf*. (Wird nicht Sie dies hier tun, aber Sie können die Datei in beliebig, umbenennen, solange es wurde ein *.sdf* Erweiterung.)

## <a name="creating-a-table"></a>Erstellen einer Tabelle

Klicken Sie im Menüband auf **neue Tabelle**. WebMatrix wird den Tabellen-Designer in einer neuen Registerkarte geöffnet. (Wenn die neue Tabellenoption nicht verfügbar ist, stellen Sie sicher, dass die neue Datenbank in der Strukturansicht auf der linken Seite ausgewählt ist.)

![Schaltfläche "Neue Tabelle" WebMatrix-Menüband](displaying-data/_static/image5.png)

Geben Sie in das Textfeld am oberen (wobei das Wasserzeichen "EINGABETASTE Tabellenname" besagt, dass), "Videos" aus.

![Einen Tabellennamen einzugeben, in der Datenbank-Designer für WebMatrix](displaying-data/_static/image6.png)

Der Bereich unterhalb der Tabellenname ist, in dem Sie die einzelnen Spalten definieren. Für die *Filme* Tabelle in diesem Lernprogramm erstellen Sie nur wenige Spalten: *ID*, *Titel*, *"Genre"*, und *Jahr*.

In der **Namen** Geben Sie "ID". Hier einen Wert eingeben, wird allen Steuerelementen für die neue Spalte aktiviert.

Die Registerkarte der **Datentyp** aus, und wählen Sie **Int**. Dieser Wert gibt an, dass die ID-Spalte Ganzzahldaten (Anzahl) enthält.

> [!NOTE]
> Wird nicht nennen wir es jegliche hier (viel), aber Sie können standardmäßige Windows-Gesten Tastatur zum Navigieren in diesem Raster. Beispielsweise können Sie mit der tab zwischen den Feldern, können Sie nur beginnen mit der Eingabe, um ein Element in einer Liste auswählen und so weiter.


Registerkarte hinter der **Standardwert** Feld (d. h. er leer lassen). Die Registerkarte der **Primärschlüssel ist** Kontrollkästchen, und wählen Sie sie. Diese Option wird der Datenbank mitgeteilt, dass die *ID* Spalte enthält die Daten, die einzelne Zeilen zu identifizieren. (D. h. jede Zeile einen eindeutigen Wert in der ID-Spalte, die Sie verwenden können, suchen Sie nach dieser Zeile müssen.)

Wählen Sie die **ist Identity** Option. Diese Option weist der Datenbank, dass die nächste laufende Nummer für jede neue Zeile automatisch generiert werden sollen. (Die **ist Identity** option funktioniert nur, wenn Sie auch ausgewählt haben die **Primärschlüssel ist** Option.)

Klicken Sie in der nächsten Rasterzeile auf, oder drücken Sie Tab, zweimal, um die aktuelle Zeile fertig zu stellen. Entweder Geste speichert die aktuelle Zeile und startet die nächste Datei. Beachten Sie, dass die **Standardwert** Spalte jetzt sagt **Null**. (Null ist der Standardwert für den Standardwert gleichsam auf.)

Sobald Sie abgeschlossen haben, definieren die neue **ID** Spalte Designer sieht in dieser Abbildung:

![WebMatrix-Datenbank-Designer nach der Definition der Spalte-ID für die Tabelle Filme](displaying-data/_static/image7.png)

Klicken Sie zum Erstellen der nächsten Spalte in das Feld in der **Namen** Spalte. Geben Sie für die Spalte "Title", und wählen Sie dann **Nvarchar** für die **Datentyp** Wert. Der Teil "Var" **Nvarchar** weist der Datenbank, dass die Daten für diese Spalte eine Zeichenfolge, deren Größe von Datensatz zu Datensatz variieren. (Das Präfix "n" stellt "national" gibt an, dass das Feld Zeichendaten für alle Alphabet oder ein Schreibsystem aufnehmen kann – d. h. das Feld Unicode-Daten enthält.)

Bei Auswahl **Nvarchar**, einem anderen Feld angezeigt wird, in dem Sie die maximale Länge für das Feld eingeben können. Geben Sie 50 ein, unter der Annahme, dass keine Filmtitel, die Sie in diesem Lernprogramm verwenden müssen mehr als 50 Zeichen enthalten kann.

Skip **Standardwert** , und deaktivieren Sie die **NULL-Werte zulassen** Option. Sie möchten schließlich nicht die Datenbank ermöglichen, die über keinen Titel keine Filme in die Datenbank eingegeben werden.

Wenn Sie fertig sind und zur nächsten Zeile bewegen, sieht der Designer in dieser Abbildung:

![WebMatrix-Datenbank-Designer nach der Definition der Spalte Titel für die Tabelle Filme](displaying-data/_static/image8.png)

Wiederholen Sie diese Schritte, um eine Spalte mit dem Namen "Genre", mit Ausnahme der Länge zu erstellen, der nur 30 festgelegt.

Erstellen Sie eine andere Spalte mit dem Namen "Year". Wählen Sie für den Datentyp **Nchar** (nicht **Nvarchar**) und die Länge auf 4 festgelegt. Für das Jahr benötigen Sie eine Zahl mit 4-Ziffern wie "1995" oder "2010" verwenden, damit Sie eine Spalte mit variabler Größe nicht erforderlich ist.

Hier ist der fertige Entwurf aussieht:

![WebMatrix Datenbank-Designer, nachdem alle Felder für die Tabelle Filme definiert sind](displaying-data/_static/image9.png)

Drücken Sie STRG + S, oder klicken Sie auf die **speichern** Schaltfläche in der Symbolleiste für den Schnellzugriff. Schließen Sie den Datenbankdesigner durch Schließen der Registerkarte.

## <a name="adding-some-example-data"></a>Einige Beispieldaten hinzufügen

Weiter unten in diesem Lernprogramm erstellen Sie eine Seite, in dem Sie neue Kinofilmen in ein Formular eingeben können. Jetzt können Sie einige Beispieldaten hinzufügen, die Sie dann auf einer Seite anzeigen können.

In der **Datenbank** Arbeitsbereich in WebMatrix, beachten Sie, dass es eine Struktur, die Aufschluss über die *.sdf* Datei, die Sie zuvor erstellt haben. Öffnen Sie den Knoten für Ihre neue *.sdf* Datei, und öffnen Sie dann die **Tabellen** Knoten.

![WebMatrix-Datenbank-Arbeitsbereich mit Struktur Filme Tabelle öffnen](displaying-data/_static/image10.png)

Mit der rechten Maustaste die **Filme** Knoten und wählen Sie dann **Daten**. WebMatrix öffnet ein Raster, in dem Sie Daten für die eingeben können, die *Filme* Tabelle:

![Eintrag Datenbankraster in WebMatrix (leer)](displaying-data/_static/image11.png)

Klicken Sie auf die **Titel** Spalte, und geben Sie "Wenn Harry erfüllt Sally". Zum Verschieben der **"Genre"** Spalte (Sie können die Tab-Taste), und geben Sie "Romantisch Comedy". Zum Verschieben der **Jahr** Spalte, und geben Sie "1989":

![Eintrag Datenbankraster in WebMatrix mit einem Datensatz](displaying-data/_static/image12.png)

Drücken Sie die EINGABETASTE, und WebMatrix speichert die neuen Film. Beachten Sie, dass die **ID** Spalte ausgefüllt.

![Eintrag Datenbankraster in WebMatrix mit einem Datensatz und automatisch generierte ID](displaying-data/_static/image13.png)

Geben Sie einen anderen Film (z. B. "sich nicht mit der Wind", "Filmen", "1939"). Die ID-Spalte wird erneut ausgefüllt:

![Eintrag Datenbankraster in WebMatrix mit zwei Datensätze und automatisch generierte-IDs](displaying-data/_static/image14.png)

Geben Sie einen dritten Film (z. B. "Ghostbusters", "Comedy"). Als ein Experiment zu belassen, die **Jahr** Spalte leer, und drücken Sie dann die EINGABETASTE. Da Sie diese Option nicht ausgewählt der **NULL-Werte zulassen** option, die Datenbank wird ein Fehler angezeigt:

![Fehler "Ungültige Daten" angezeigt, wenn ein erforderliche Spalte kein Wert angegeben ist](displaying-data/_static/image15.png)

Klicken Sie auf **OK** zurückgehen und korrigieren Sie den Eintrag (das Jahr für "Ghostbusters" ist 1984) und drücken dann die EINGABETASTE.

Geben Sie mehrere Filme bis 8 erstellt wurde oder damit. (8 eingeben erleichtert es, arbeiten mit paging weiter unten. Aber wenn dies zu viele ist, geben Sie nur wenige vorläufig.) Die tatsächlichen Daten spielt keine Rolle.

![Eintrag Datenbankraster in WebMatrix mit beiden Datensätze](displaying-data/_static/image16.png)

Wenn Sie alle Filme ohne Fehler eingegeben haben, sind die ID-Werte sequenziell. Die ID-Nummern sind möglicherweise nicht sequenziell, würden Sie versuchen, einen unvollständige Film-Eintrag zu speichern. Wenn dies der Fall ist, ist das kein Problem. Die Zahlen keine inhärente Bedeutung und das einzige, die wichtig ist, dass sie eindeutig sind die *Filme* Tabelle.

Schließen Sie die Registerkarte mit der Datenbank-Designer.

Jetzt können Sie zum Anzeigen der Daten auf einer Webseite deaktivieren.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Anzeigen von Daten in eine Seite mit den WebGrid-Hilfsprogramm

Damit Daten auf einer Seite angezeigt, Sie verwenden nun den `WebGrid` Helper. Dieses Hilfsprogramm erzeugt eine Anzeige in einem Raster oder die Tabelle (Zeilen und Spalten). Wie Sie sehen, werden Sie können verfeinern Sie das Raster mit Formatierung und andere Funktionen.

Um das Raster auszuführen, müssen Sie ein paar Zeilen Code schreiben. Diese Zeilen werden als eine Art von Muster für fast alle der Datenzugriffs-dienen, die Sie in diesem Lernprogramm.

> [!NOTE]
> Sie haben viele Optionen zum Anzeigen von Daten auf einer Seite; die `WebGrid` Helper ist nur eine. Wir haben sie für dieses Lernprogramm, da es die einfachste Möglichkeit zum Anzeigen von Daten ist und es ausreichend flexibel ist. In den nächsten Lernprogramm sehen Sie, wie Sie eine weitere "manual" Möglichkeit zum Arbeiten mit Daten in der Seite ", wodurch Sie mehr direkte Kontrolle über die Vorgehensweise beim Anzeigen von Daten erhalten.


Klicken Sie im linken Bereich in WebMatrix auf die **Dateien** Arbeitsbereich.

Die neue Datenbank, die Sie erstellt haben, ist in der *App\_Daten* Ordner. Wenn der Ordner nicht vorhanden, von WebMatrix für die neue Datenbank erstellt. (Der Ordner möglicherweise Waren haben, hätten Sie zuvor Hilfsprogramme installiert.)

Wählen Sie in der Strukturansicht den Stamm der Website ein. Sie müssen das Stammverzeichnis der Website auswählen; Andernfalls kann die neue Datei hinzugefügt werden, für die App\_Datenordner.

Klicken Sie im Menüband auf **neu**. In der **wählen Sie einen Dateityp** wählen **CSHTML**.

In der **Namen** benennen Sie die neue Seite "Movies.cshtml":

![Anzeigen der Seite "Filme" 'Eines Dateityps auswählen' (Dialogfeld)](displaying-data/_static/image17.png)

Klicken Sie auf die **OK** Schaltfläche. WebMatrix öffnet eine neue Datei mit einigen Skelett Elemente. Sie schreiben zuerst Code gehen die Daten aus der Datenbank abzurufen. Anschließend fügen Sie Markup zur Seite, um die Daten tatsächlich anzuzeigen.

### <a name="writing-the-data-query-code"></a>Der Abfragecode Daten schreiben

Am oberen Rand der Seite zwischen dem `@{` und `}` Zeichen, geben Sie den folgenden Code. (Stellen Sie sicher, dass Sie diesen Code zwischen den öffnenden und schließenden geschweiften Klammern eingeben.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

Die erste Zeile öffnet die Datenbank, die Sie zuvor erstellt haben, also immer der erste Schritt vor der Aktionen, die mit der Datenbank. Sie erkennen die `Database.Open` Methodennamen der Datenbank zu öffnen. Beachten Sie, dass Sie die enthalten *.sdf* im Namen. Der `Open` Methode wird davon ausgegangen, dass sie für schaut ist ein *sdf* Datei (, also *WebPagesMovies.sdf*) und der *sdf* Datei befindet sich in der *App\_ Daten* Ordner. (Wir erwähnt, die die *App\_Daten* Ordner ist reserviert; dieses Szenario ist eine der Stellen, in dem ASP.NET zu diesem Namen Annahmen.)

Wenn die Datenbank geöffnet wird, wird ein Verweis darauf in der benannten Variablen eingefügt `db`. (Die nichts mit dem Namen werden konnte.) Die `db` Variable ist, wie Sie enden mit der Datenbank interagieren müssen.

Die zweite Zeile ruft tatsächlich Daten in der Datenbank mithilfe der `Query` Methode. Beachten Sie, wie dieser Code funktioniert: die `db` Variable enthält einen Verweis auf die geöffnete Datenbank, und rufen Sie die `Query` Methode mithilfe der `db` Variable (`db.Query`).

Die Abfrage selbst ist ein SQL `Select` Anweisung. (Einige Hintergrundinformationen zur SQL finden Sie in der Erläuterung weiter unten.) In der Anweisung `Movies` identifiziert die Tabelle, Abfrage. Die `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können auch Spalten einzeln auflisten durch Kommas getrennt.)

Die Ergebnisse der Abfrage, sofern vorhanden, werden zurückgegeben und zur Verfügung gestellt in die `selectedData` Variable. In diesem Fall konnte die Variable nichts benannt werden.

Schließlich in der dritten Zeile Weise wird ASP.NET informiert, dass eine Instanz von verwendet werden sollen die `WebGrid` Helper. Sie erstellen (*instanziieren*) das Hilfsobjekt mithilfe der `new` Schlüsselwort, und übergeben sie die Ergebnisse der Abfrage über die `selectedData` Variable. Die neue `WebGrid` Objekt, zusammen mit den Ergebnissen der Datenbankabfrage, stehen in der `grid` Variable. Sie benötigen dieses Ergebnis in einen Moment Zeit, um die Daten tatsächlich auf der Seite anzuzeigen.

Zu diesem Zeitpunkt ist die Datenbank geöffnet wurde, haben Sie die Daten erhalten soll, und Sie vorbereitet haben die `WebGrid` Helper mit diesen Daten. Als Nächstes wird das Markup der Seite zu erstellen.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL ist eine Programmiersprache, die in den meisten relationalen Datenbanken verwendet wird, um Daten in einer Datenbank zu verwalten. Es enthält Befehle, mit deren Hilfe Sie die Daten abzurufen und zu aktualisieren, und, mit deren Hilfe Sie erstellen, ändern und Verwalten von Daten in Datenbanktabellen. SQL unterscheidet sich von einer Programmiersprache Ihrer Wahl (z. B. c#). Mit "SQL" oder Teilen der Datenbank erwünscht, und es ist Aufgabe der Datenbank, um zu erfahren, wie die Daten abgerufen und die Ausführung der Aufgabe. Nachfolgend sind Beispiele für einige SQL-Befehle und was sie tun:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Die erste `Select` Anweisung ruft alle Spalten (gemäß `*`) aus der *Filme* Tabelle.
> 
> Die zweite `Select` Anweisung ruft die Spalten-ID, Name und Preis von Datensätzen in der *Produkt* Tabelle, deren Preis für die Spalte mehr als 10 ist. Der Befehl gibt die Ergebnisse in alphabetischer Reihenfolge anhand der Werte der Spalte zurück. Wenn keine Datensätze auf die Preis Kriterien entsprechen, gibt der Befehl eine leere Menge zurück.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Dieser Befehl fügt einen neuen Datensatz in die *Produkt* Tabelle, die die Spalte "Name" an "Croissant" Position der Spalte "Beschreibung", "A arbeitende erfreuen" und der Preis auf 1.99 festlegen.
> 
> Beachten Sie, dass wenn Sie einen nicht numerischen Wert angeben, wird der Wert in einfache Anführungszeichen (keine doppelten Anführungszeichen, wie in c#) eingeschlossen. Sie verwenden diese Anführungszeichen um Text oder Datumswerte, jedoch nicht um Zahlen.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Dieser Befehl löscht Einträge in der *Produkt* Tabelle, deren Ablauf des Date-Spalte vor dem 1. Januar 2008 liegt. (Der Befehl setzt voraus, dass die *Produkt* Tabelle besitzt eine Spalte, natürlich.) Das Datum wird im Format MM/TT/JJJJ hier eingegeben, aber es eingegeben werden in das Format, das für Ihr Gebietsschema verwendet wird.
> 
> Die `Insert` und `Delete` Befehle keine Resultsets zurückgeben. Stattdessen geben sie eine Zahl, die Sie informiert, wie viele Datensätze mit dem Befehl betroffen sind zurück.
> 
> Für einige dieser Vorgänge (wie einfügen und Löschen von Datensätzen) muss der Prozess, der den Vorgang anfordert, wird in der Datenbank berechtigt. Warum für Produktionsdatenbanken, die Sie häufig aufweisen, geben Sie einen Benutzernamen und ein Kennwort ein, bei der Herstellung einer Verbindung mit der Datenbank ist.
> 
> Es gibt Dutzende von SQL-Befehlen, alle folgen jedoch ein Muster wie die Befehle, die Sie hier angezeigt. Sie können SQL-Befehle erstellen Sie die Datenbanktabellen, die Anzahl der Datensätze in einer Tabelle, Preise berechnen und viele weitere Vorgänge ausführen.


### <a name="adding-markup-to-display-the-data"></a>Hinzufügen von Markup zum Anzeigen von Daten

Innerhalb der `<head>` Element, der Inhalt der `<title>` Element "Filme":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Innerhalb der `<body>` -Element der Seite, fügen Sie Folgendes hinzu:

[!code-html[Main](displaying-data/samples/sample3.html)]

Das ist alles. Die `grid` Variable ist der Wert, der Sie erstellt, beim Erstellen haben der `WebGrid` Objekt im Code weiter oben.

Klicken Sie in der Strukturansicht WebMatrix Maustaste auf die Seite, und wählen **in Browser starten**. Sie sehen in etwa auf dieser Seite:

![Standardausgabe WebGrid-Hilfsprogramm aus der Tabelle Filme](displaying-data/_static/image18.png)

Klicken Sie auf eine Spaltenüberschrift-Link, um nach dieser Spalte zu sortieren. Durch Klicken auf eine Spaltenüberschrift sortieren ist eine Funktion, die in integrierten der **WebGrid** Helper.

Die `GetHtml` -Methode, wie der Name andeutet, generiert Markup, das die Daten anzeigt. Wird standardmäßig die `GetHtml` Methode generiert ein HTML `<table>` Element. (Wenn Sie möchten, können Sie das Rendering überprüfen, durch einen Blick auf die Quelle der Seite im Browser.)

## <a name="modifying-the-look-of-the-grid"></a>Ändern das Aussehen des Rasters

Mithilfe der `WebGrid` Helper, wie Sie gerade getan haben, ist einfach, aber angezeigt wird. Die `WebGrid` Hilfsprogramm hat alle Arten von Optionen, mit denen Sie steuern, wie die Daten angezeigt werden. Vorhanden sind, die in diesem Lernprogramm nachgehen viel zu viele, aber in diesem Abschnitt erhalten Sie einen Überblick über einige dieser Optionen. Einige zusätzliche Optionen werden in späteren Lernprogrammen in dieser Serie behandelt.

### <a name="specifying-individual-columns-to-display"></a>Angeben der einzelne Spalten zum Anzeigen

Um zu starten, können Sie angeben, dass nur bestimmte Spalten angezeigt werden soll. Standardmäßig, wie Sie gesehen haben, im Raster werden alle vier Spalten aus der *Filme* Tabelle.

In der *Movies.cshtml* Datei, ersetzen Sie die `@grid.GetHtml()` Markup, das Sie gerade hinzugefügt haben, durch Folgendes:

[!code-css[Main](displaying-data/samples/sample4.css)]

Um dem Hilfsobjekt mitzuteilen, welche Spalten angezeigt, Sie enthalten eine `columns` -Parameter für die `GetHtml` -Methode und übergeben Sie eine Auflistung von Spalten. In der Auflistung geben Sie jede Spalte einschließen. Geben Sie eine einzelne Spalte zum Anzeigen von einschließlich einer `grid.Column` Objekts, und übergeben Sie den Namen der Datenspalte werden sollen. (Diese Spalten enthalten sein müssen, in die Ergebnisse der SQL-Abfrage – die `WebGrid` Hilfsprogramm kann nicht angezeigt werden Spalten, die nicht von der Abfrage zurückgegeben wurden.)

Starten Sie die *Movies.cshtml* Seite im Browser erneut, und dieses Mal erhalten Sie eine Anzeige wie den folgenden Ausdruck (Beachten Sie, dass keine ID-Spalte angezeigt wird):

![WebGrid-Anzeige, die nur die ausgewählten Spalten mit](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Ändern des Aussehens des Rasters

Es gibt ein paar weitere Optionen für die Anzeige von Spalten, von die einige in späteren Lernprogrammen in dieser Gruppe beschrieben wird. Jetzt werden in diesem Abschnitt Möglichkeiten vorgestellt in dem Sie das Raster als Ganzes formatieren können.

Innerhalb der `<head>` Abschnitt der Seite unmittelbar vor dem schließenden `</head>` zu kennzeichnen, fügen Sie die folgenden `<style>` Element:

[!code-css[Main](displaying-data/samples/sample5.css)]

Mit diesem CSS-Markup definiert Klassen, die mit dem Namen `grid`, `head`und so weiter. Diese Definitionen konnte auch abgelegt werden, in einer separaten *CSS* Datei und eine Verknüpfung zu der Seite. (Tatsächlich müssen Sie, die weiter unten in diesem Lernprogramm Satz ausführen.) Um Elemente für dieses Lernprogramm zu erleichtern, sind aber innerhalb der gleichen Seite, die die Daten anzeigt.

Nutzen Sie jetzt die `WebGrid` Hilfsmethode zum diese Formatklassen verwenden. Das Hilfsprogramm hat eine Reihe von Eigenschaften (z. B. `tableStyle`) für diesen Zweck – Sie weisen einen Klassennamen der CSS-Stil auf diese, und diese Klassennamen wird als Teil das Markup, das durch das Hilfsprogramm gerendert wird gerendert.

Ändern der `grid.GetHtml` Markup, sodass jetzt wie dieser Code aussieht:

[!code-css[Main](displaying-data/samples/sample6.css)]

Hier Neuigkeiten ist, dass Sie hinzugefügt haben `tableStyle`, `headerStyle`, und `alternatingRowStyle` Parameter für die `GetHtml` Methode. Diese Parameter haben den Namen der CSS-Formatvorlagen festgelegt wurde, die Sie eben hinzugefügt.

Führen Sie die Seite, und diesmal sehen Sie ein Raster aus, die wesentlich geringer als vor plain aussieht:

![WebGrid-anzeigen, die Parameter legen Sie auf den Namen der CSS-Klasse](displaying-data/_static/image20.png)

Festzustellen, was die `GetHtml` Methode, die generiert werden, können Sie an der Quelle der Seite im Browser ansehen. Es wird nicht ins Detail gehen, aber der wichtige Punkt ist, die durch Angeben von Parametern wie `tableStyle`, veranlasst das Raster zum Generieren von HTML-Tags wie folgt:

`<table class="grid">`

Die `<table>` Tag hatte ein `class` Attribut hinzugefügt wird, die auf eine der CSS-Regeln verweist, die Sie zuvor hinzugefügt haben. Dieser Code zeigt das grundlegende Muster &mdash; verschiedene Parameter für die `GetHtml` -können Sie Methodenverweis CSS-Klassen, dass die Methode dann zusammen mit dem Markup generiert. Was Sie mit der CSS-Klassen machen liegt bei Ihnen.

## <a name="adding-paging"></a>Hinzufügen von Paging

Im letzten Task für dieses Lernprogramm fügen Sie dem Raster Paging. Derzeit ist kein Problem, um alle Filme gleichzeitig anzuzeigen. Aber wenn Sie Hunderte von Filmen hinzugefügt haben, würden die angezeigte Seite lang abrufen.

Ändern Sie in den Seitencode die Zeile, die erstellt die `WebGrid` Objekt in den folgenden Code:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Bevor Sie der einzige Unterschied ist, dass Sie hinzugefügt haben eine `rowsPerPage` Parameter, der auf 3 festgelegt ist.

Führen Sie die Seite. Das Raster zeigt 3 Zeilen an eine Zeit plus der Navigationslinks, mit denen Sie die Seite über die Filme in Ihrer Datenbank können:

![WebGrid-Anzeige mit paging](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Als Nächstes kommen

In den nächsten Lernprogrammen erfahren Sie, wie mit Razor und C#-Code zum Abrufen von Benutzereingaben in einem Formular. Ein Suchfeld fügen auf der Seite "Filme" Sie, damit Sie Filme nach Titel oder "Genre" erhalten.

## <a name="complete-listing-for-movies-page"></a>Vollständige Auflistung für Filme-Seite

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Zurück](intro-to-web-pages-programming.md)
> [Weiter](form-basics.md)
