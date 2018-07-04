---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Einführung in ASP.NET Web Pages - HTML-Formular-Grundlagen | Microsoft-Dokumentation
author: tfitzmac
description: 'In diesem Tutorial erfahren Sie, die Grundlagen der Vorgehensweise: Erstellen Sie ein Eingabeformular an und wie die Eingabe des Benutzers verarbeitet, bei der Verwendung von ASP.NET Web Pages (Razor). Und nachdem Sie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f5f7c9813041c443675f4e443822a81c1c508c20
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391990"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Einführung in ASP.NET Web Pages - HTML-Formular-Grundlagen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, die Grundlagen der Vorgehensweise: Erstellen Sie ein Eingabeformular an und wie die Eingabe des Benutzers verarbeitet, bei der Verwendung von ASP.NET Web Pages (Razor). Und nun, da Sie eine Datenbank haben, verwenden Sie Ihre Kenntnisse Formular können Benutzer bestimmte Filme in der Datenbank gefunden. Es wird vorausgesetzt, Sie haben die Reihe über [Einführung zum Anzeigen von Daten mithilfe von ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Sie lernen Folgendes:
> 
> - Vorgehensweise: Erstellen Sie ein Formular mit standard-HTML-Elemente.
> - Gewusst wie: Lesen Sie den Benutzer Benutzereingabe in einem Formular.
> - Wie Sie eine SQL-Abfrage erstellen, selektiv Daten mithilfe einer Suche abgerufen, die der Benutzer Begriff bereitstellt.
> - So, dass Felder, die auf der Seite "erinnert" Eingabe des Benutzers.
>   
> 
> Features/Technologien erläutert:
> 
> - Das `Request`-Objekt.
> - Die SQL-Anweisung `Where` Klausel.


## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie eine Datenbank erstellt, Daten hinzugefügt, und dann verwendet der `WebGrid` Hilfsmethode zum Anzeigen der Daten. In diesem Tutorial fügen Sie ein Suchfeld, mit der Sie Filme, die von einer bestimmten "Genre" suchen oder, deren Titel enthält den eingegebenen Wort. (Z. B. Sie Lage finden alle Filme, deren "Genre" "Action" ist oder mit dem Titel enthält "Paul" oder "Adventure".)

Wenn Sie mit diesem Lernprogramm fertig sind, müssen Sie eine Seite wie diese:

![Seite "Movies", mit der Suche nach Genre und Titel](form-basics/_static/image1.png)

Der Angebot Teil der Seite wird der gleiche wie in das letzte Tutorial &mdash; ein Raster. Die Unterschiede werden im Raster nur die Filme anzeigt, dass Sie gesucht.

## <a name="about-html-forms"></a>Informationen zu HTML-Formularen

(Wenn Sie Erfahrung mit HTML-Formulare erstellen und mit den Unterschied zwischen haben `GET` und `POST`, können Sie diesen Abschnitt überspringen.)

Ein Formular hat die benutzereingabeelemente &mdash; Textfelder, Schaltflächen, Optionsfelder, Kontrollkästchen, Dropdownlisten und So weiter. Benutzer geben Sie in diesen Steuerelementen oder Optionen und klicken Sie dann das Formular durch Klicken auf eine Schaltfläche zu übermitteln.

In diesem Beispiel wird ein Formular grundlegende HTML-Syntax veranschaulicht:

[!code-html[Main](form-basics/samples/sample1.html)]

Wenn dieses Markup auf einer Seite ausgeführt wird, wird ein einfaches Formular, das wie die folgende Abbildung aussieht:

![Grundlegende als im Browser gerenderte HTML-Formular](form-basics/_static/image2.png)

Die `<form>` Element einschließt, HTML-Elemente übermittelt werden. (Eine einfache Fehler vornehmen besteht darin, die Elemente auf der Seite hinzufügen, aber vergessen Sie Einsatzort innerhalb einer `<form>` Element. In diesem Fall ist "nothing" übermittelt.) Die `method` Attribut teilt dem Browser mit, wie Sie die Benutzereingabe zu übermitteln. Sie legen `post` Wenn Sie ein Update auf dem Server oder auf durchführen `get` , wenn Sie nur Daten vom Server abgerufen werden.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST und HTTP-Verb-Sicherheit**
> 
> HTTP, das Protokoll, Browser und Server zu verwenden, um Informationen auszutauschen, ist die grundlegende Vorgänge überraschend einfach. Browser verwenden nur ein paar Verben, um Anforderungen an Server senden. Beim Schreiben von Code für das Web, ist es hilfreich zu verstehen, diese Verben und wie Browser und Server sie verwenden. Bei weitem sind die am häufigsten verwendeten Verben dies die:
> 
> - `GET` Der Browser verwendet dieses Verb etwas vom Server abgerufen. Z. B. Wenn Sie eine URL in Ihren Browser eingeben, die der Browser führt eine `GET` Vorgang auf die Seite "anfordern, werden sollen. Wenn die Seite Grafiken enthält, führt der Browser zusätzliche `GET` Vorgänge, um die Bilder zu erhalten. Wenn die `GET` -Vorgang bleibt, Informationen an den Server übergeben, die Informationen als Teil der URL in der Abfragezeichenfolge übergeben wird.
> - `POST` Der Browser sendet eine `POST` Anforderung zum Senden von Daten hinzugefügt oder auf dem Server geändert werden. Z. B. die `POST` Verb wird verwendet, um Datensätze in einer Datenbank erstellen oder vorhandene ändern. In den meisten Fällen, wenn Sie ein Formular auszufüllen, und klicken Sie auf die Schaltfläche "Senden", die der Browser führt eine `POST` Vorgang. In einem `POST` -Vorgang an den Server übergebenen Daten werden im Text der Seite.
> 
> Ein wichtiger Unterschied zwischen dieser Verben ist, eine `GET` Vorgang darf nicht mehr ändern, Elemente auf dem Server – oder auf etwas abstraktere Weise ausgedrückt eine `GET` Vorgang führt nicht zu einer Änderung in den Zustand auf dem Server. Durchführen können eine `GET` -Vorgang für die gleichen Ressourcen wie oft und solange aus, und ändern Sie diese Ressourcen nicht. (Ein `GET` Vorgang wird oft betont, "sicher", oder um einen technischen Begriff, zu verwenden ist *Idempotent*.) Im Gegensatz dazu sind natürlich eine `POST` Anforderung ändert sich etwas auf dem Server jedes Mal, die Sie den Vorgang ausführen.
> 
> Zwei Beispiele hilft diese Unterscheidung veranschaulichen. Beim Durchführen einer Suche verwenden ein Modul wie Bing oder Google, füllen Sie ein Formular, das aus einem Textfeld besteht, und klicken Sie dann Sie auf die Schaltfläche "Suchen". Der Browser führt eine `GET` Vorgang, mit dem Wert, der Sie eingegeben, in das Feld, das als Teil der URL übergeben haben. Mit einem `GET` Vorgang für diese Art von Form in Ordnung ist, ist, da es sich bei ein Suchvorgang alle Ressourcen auf dem Server nicht ändert, es nur Informationen abruft.
> 
> Betrachten Sie nun den Prozess etwas online zu bestellen. Sie geben Sie in den Details der Bestellung, und klicken Sie dann auf die Schaltfläche "Senden". Dieser Vorgang wird eine `POST` anfordern, da Änderungen auf dem Server, z. B. einen neuen Datensatz für die Reihenfolge, eine Änderung der Kontoinformationen und möglicherweise viele weitere Änderungen der Vorgang führt. Im Gegensatz zu den `GET` -Operation, Sie können nicht wiederholt werden Ihre `POST` Anforderung – Wenn, jedes Mal haben Sie Sie die Anforderung erneut, würden Sie einen neuen Auftrag auf dem Server generieren. (In solchen Fällen Websites häufig warnt Sie nicht mehr als einmal auf eine Sendeschaltfläche klicken, oder die Schaltfläche "Senden" wird deaktiviert, so, dass Sie versehentlich nicht erneut das Formular übermitteln.)
> 
> Bei diesem Tutorial verwenden Sie sowohl eine `GET` Vorgang und ein `POST` HTML-Formularen funktioniert. Erläutert in jeden Fall daher das Verb an, die, das Sie verwenden, das geeignet ist.
> 
> (Weitere Informationen zu HTTP-Verben finden Sie unter den [Methodendefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) Artikel auf der W3C-Website.)


Die meisten benutzereingabeelemente sind HTML- `<input>` Elemente. Sie sehen, wie `<input type="type" name="name">,` , in denen *Typ* gibt die Art von Steuerelement mit Benutzereingabe werden sollen. Diese Elemente sind die gängigsten Einträge:

- Textfeld: `<input type="text">`
- Aktivieren Sie das Kontrollkästchen: `<input type="check">`
- Optionsfeld ":". `<input type="radio">`
- Schaltfläche: `<input type="button">`
- Senden Sie Schaltfläche "": `<input type="submit">`

Können Sie auch die `<textarea>` Element um ein mehrzeiliges Textfeld zu erstellen und die `<select>` Element zum Erstellen einer Dropdown-Liste oder einer bildlauffähigen Liste. (Weitere Informationen zu HTML Elemente bilden, finden Sie unter [HTML-Formularen und Eingabe](http://www.w3schools.com/html/html_forms.asp) auf der Website W3Schools.)

Die `name` Attribut ist sehr wichtig, da der Name ist wie den Wert der Elements höher erhalten, wie Sie bald sehen werden.

Der interessante Teil befindet, wie Sie, den Seitenentwickler, die Eingabe des Benutzers verwenden. Es gibt keine integrierten Verhalten dieser Elemente. Stattdessen müssen Sie zum Abrufen der Werte, die der Benutzer eingegeben oder ausgewählt haben und etwas mit ihnen. Das ist in diesem Tutorial lernen Sie.

> [!TIP] 
> 
> **HTML5 und Eingabeformulare**
> 
> Wie Sie wissen vielleicht, wird HTML befindet sich im Übergangsstadium, und die neueste Version (HTML5) bietet Unterstützung für intuitiver Möglichkeiten für Benutzer Informationen eingeben. HTML5 können Sie (der Seitenentwickler) z. B. der Seite erkennen, dass der Benutzer ein Datum eingeben soll. Der Browser können Sie automatisch einen Kalender, statt den Benutzer manuell eingeben, ein Datum anzeigen. Allerdings wird HTML5 ist neu und wird noch nicht unterstützt in allen Browsern.
> 
> ASP.NET Web Pages unterstützt HTML5 eingeben, wenn der Browser des Benutzers ist. Für einen Überblick über die neuen Attribute für die `<input>` Element HTML5 finden Sie unter [HTML &lt;Eingabe&gt; type-Attribut](http://www.w3schools.com/html/html_form_input_types.asp) auf der Website W3Schools.


## <a name="creating-the-form"></a>Erstellen des Formulars

In WebMatrix in die **Dateien** öffnen Sie im Arbeitsbereich, den *Movies.cshtml* Seite.

Nach dem schließenden `</h1>` Tag und vor dem öffnenden `<div>` Tag die `grid.GetHtml` aufzurufen, fügen Sie das folgende Markup hinzu:

[!code-html[Main](form-basics/samples/sample2.html)]

Dieses Markup erstellt ein Formular, das ein mit dem Namen Textfeld `searchGenre` sowie eine Senden-Schaltfläche. Der Text ein, und übermitteln Sie die Schaltfläche in eingeschlossen sind ein `<form>` Element, dessen `method` -Attributsatz auf `get`. (Beachten Sie, dass, wenn Sie nicht im Textfeld platzieren und Schaltfläche in "Senden" eine `<form>` Element nichts wird gesendet werden, wenn Sie die Schaltfläche klicken.) Sie verwenden die `GET` Verb hier, da Sie ein Formular erstellen, das macht keine Änderungen auf dem Server – er einfach eine Suche ergibt. (Im vorherigen Tutorial haben Sie verwendet eine `post` -Methode, die ist, wie Sie Änderungen an den Server übermitteln. Sie müssen, die im nächsten Tutorial erneut sehen.)

Führen Sie die Seite. Obwohl Sie jedes Verhalten für das Formular definiert haben, können Sie sehen, wie es aussieht:

![Seite "Movies", mit dem Suchfeld für "Genre"](form-basics/_static/image3.png)

Geben Sie einen Wert in das Textfeld ein, z. B. "Komödie." Klicken Sie dann auf **Search "Genre"**.

Notieren Sie sich die URL der Seite. Da Sie festlegen, die `<form>` des Elements `method` Attribut `get`, eingegebene Wert ist jetzt Teil der Abfragezeichenfolge in der URL wie folgt:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Formularwerte lesen

Diese Seite enthält bereits Code, der die Datenbankdaten abruft und die Ergebnisse in einem Raster angezeigt. Sie haben jetzt fügen Sie Code hinzu, der den Wert des Textfelds liest, sodass Sie eine SQL-Abfrage ausführen können, die den gesuchten Begriff enthält.

Da Sie die-Methode des Formulars, um festlegen `get`, Sie können den Wert, der in das Textfeld eingegeben wurde, mithilfe von Code wie folgt lesen:

`var searchTerm = Request.QueryString["searchGenre"];`

Die `Request.QueryString` Objekt (das `QueryString` Eigenschaft der `Request` Objekt) enthält die Werte der Elemente, die als Teil des gesendet wurden die `GET` Vorgang. Die `Request.QueryString` Eigenschaft enthält eine *Auflistung* (Liste) der Werte, die in das Formular übermittelt werden. Um einen einzelnen Wert zu erhalten, geben Sie den Namen des Elements, das Sie möchten. Daher enthalten sind ein `name` -Attribut für die `<input>` Element (`searchTerm`), erstellt das Textfeld ein. (Weitere Informationen zu den `Request` Objekt, finden Sie unter der [Randleiste](#BKMK_TheRequestObject) weiter unten.)

Es ist einfach genug, um den Wert des Textfelds zu lesen. Aber wenn der Benutzer nicht eingeben, was überhaupt in das Textfeld geklickt **Suche** jedenfalls können ignoriert werden, klicken Sie auf, da es nichts gibt zu suchen.

Der folgende Code ist ein Beispiel, das zeigt, wie Sie diese Bedingungen zu implementieren. (Sie müssen diesen Code hinzufügen, die noch keine; Sie hierfür das weiter unten.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Auf diese Weise wird der Test:

- Ruft den Wert der `Request.QueryString["searchGenre"]`, d. h. der Wert, der in eingegeben wurde die `<input>` Element mit dem Namen `searchGenre`.
- Erfahren Sie, wenn sie mithilfe von leer ist die `IsEmpty` Methode. Diese Methode ist die Standardmethode zum bestimmen, ob ein (z. B. ein Formularelement) einen Wert enthält. Aber wirklich nur, wenn sie gewünschte *nicht* leer, daher...
- Hinzufügen der `!` Operator vor dem `IsEmpty` testen. (Die `!` Operator bedeutet Logisches NOT).

In einfachen Worten, die gesamte `if` Bedingung in den folgenden übersetzt: *ist SearchGenre-Element des Formulars nicht leer ist, klicken Sie dann...*

Dieser Block legt fest, die als Grundlage für das Erstellen einer Abfrage, die den gesuchten Begriff verwendet wird. Haben Sie Gelegenheit, die im nächsten Abschnitt.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Das Request-Objekt**
> 
> Die `Request` Objekt enthält alle Informationen, die der Browser zu Ihrer Anwendung sendet, wenn eine Seite angefordert oder abgesendet wird. Dieses Objekt enthält alle Informationen, die der Benutzer gibt, wie z. B. Textfeldwerte oder eine Datei zum Hochladen. Es enthält auch alle möglichen zusätzlichen Informationen, wie Cookies, Werte in der URL-Abfragezeichenfolge (sofern vorhanden), den Dateipfad der Seite, die den Typ des Browsers, die der Benutzer verwendet wird, die Liste der Sprachen ausgeführt wird, die im Browser festgelegt werden , und vieles mehr.
> 
> Die `Request` Objekt ist ein *Auflistung* (Liste) von Werten. Sie erhalten einen einzelnen Wert aus der Auflistung durch Angabe seines Namens:
> 
> `var someValue = Request["name"];`
> 
> Die `Request` Objekt tatsächlich stellt verschiedene Teilmengen. Zum Beispiel:
> 
> - `Request.Form` enthält Werte von Elementen innerhalb der übermittelten `<form>` Element, wenn die Anforderung ist ein `POST` Anforderung.
> - `Request.QueryString` erhalten Sie nur die Werte in der URL-Abfragezeichenfolge. (In einer URL wie `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` Teil der URL ist die Abfragezeichenfolge.)
> - `Request.Cookies` Sammlung erhalten Sie Zugriff auf Cookies, die der Browser gesendet hat.
> 
> Einen Wert abgerufen, die Sie kennen das übermittelte Formular wird, können Sie mit `Request["name"]`. Alternativ können Sie die spezifischeren Versionen `Request.Form["name"]` (für `POST` Anforderungen) oder `Request.QueryString["name"]` (für `GET` Anforderungen). Natürlich *Namen* ist der Name des abzurufenden Elements.
> 
> Der Name des Elements, die Sie abrufen möchten muss innerhalb der Auflistung eindeutig sein, die Sie verwenden. Deshalb die `Request` Objekt enthält die Teilmengen wie `Request.Form` und `Request.QueryString`. Nehmen wir an, dass Ihre Seite ein Formularelement mit dem Namen enthält `userName` und *auch* enthält ein Cookie mit dem Namen `userName`. Wenn man `Request["userName"]`, es ist nicht eindeutig, ob der Formularwert oder das Cookie soll. Aber wenn man `Request.Form["userName"]` oder `Request.Cookie["userName"]`, Sie sind explizit der Wert abgerufen wird.
> 
> Es hat sich bewährt, machen Sie genaue Angaben, und verwenden die Teilmenge der `Request` , dass Sie, wie interessieren `Request.Form` oder `Request.QueryString`. Für die einfache Seiten, die Sie in diesem Tutorial erstellen, erleichtern nicht wahrscheinlich wirklich einen Unterschied. Beim Erstellen komplexere Seiten verwenden jedoch die explizite Version `Request.Form` oder `Request.QueryString` ermöglicht Ihnen, Probleme zu vermeiden, die auftreten können, wenn die Seite enthält, ein Formular (oder mehrere Formulare), Cookies, Abfragezeichenfolgen-Werte und So weiter.


## <a name="creating-a-query-by-using-a-search-term"></a>Erstellen einer Abfrage mithilfe eines Suchbegriffs

Nun, dass Sie wissen, wie den Suchbegriff abgerufen, den vom Benutzer eingegebene, können Sie eine Abfrage erstellen, die verwendet wird. Denken Sie daran, dass die um alle Elemente der Film aus der Datenbank zu erhalten, Sie eine SQL-Abfrage verwenden, die diese Anweisung ähnelt:

`SELECT * FROM Movies`

Um nur bestimmte Filme zu erhalten, müssen Sie eine Abfrage zu verwenden, enthält eine `Where` Klausel. Diese Klausel können Sie eine Bedingung festlegen, auf der von der Abfrage Zeilen zurückgegeben werden. Im Folgenden ein Beispiel:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Das Standardformat lautet `WHERE column = value`. Andere Operatoren können neben nur `=`, z. B. `>` (größer als), `<` (kleiner als), `<>` (ungleich), `<=` (kleiner als oder gleich), usw., je nachdem, was Sie suchen.

Falls Sie sich wundern, SQL-Anweisungen sind keine Groß-/ Kleinschreibung &mdash; `SELECT` ist identisch mit `Select` (oder sogar `select`). Allerdings Personen häufig profitieren Schlüsselwörter in einer SQL-Anweisung, wie z. B. `SELECT` und `WHERE`, um ihn leichter lesbar zu machen.

### <a name="passing-the-search-term-as-a-parameter"></a>Den Suchbegriff übergeben als Parameter.

Suchen nach einem bestimmten Genre recht einfach ist (`WHERE Genre = 'Action'`), aber Sie möchten in der Lage, die alle Genres suchen, die der Benutzer eingibt. Zu diesem Zweck erstellen Sie als SQL-Abfrage, die enthält einen Platzhalter für den Wert zu suchen. Es sieht so aus wie mit diesem Befehl:

`SELECT * FROM Movies WHERE Genre = @0`

Der Platzhalter die `@` Zeichens, gefolgt von 0 (null). Wie Sie sich denken können, eine Abfrage kann mehrere Platzhalter enthalten, und sie den Namen `@0`, `@1`, `@2`usw.

Informationen zum Einrichten von der Abfrage ein, und übergeben Sie sie tatsächlich den Wert, verwenden Sie den Code wie folgt aus:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Dieser Code ist ähnlich, was Sie bereits durchgeführt haben, um Daten im Raster anzuzeigen. Der einzige Unterschied besteht:

- Die Abfrage enthält einen Platzhalter (`WHERE Genre = @0"`).
- Die Abfrage wird in einer Variablen eingefügt (`selectCommand`), bevor Sie die Abfrage direkt zu übergeben, die `db.Query` Methode.
- Beim Aufrufen der `db.Query` -Methode, übergeben Sie die Abfrage und der Wert, der für den Platzhalter verwenden. (Wenn die Abfrage mehrere Platzhalter enthält, übergeben Sie sie als unterschiedliche Werte für die Methode.)

Wenn Sie all diese Elemente zusammengestellt, erhalten Sie den folgenden Code:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Wichtig** Mithilfe von Platzhaltern (z. B. `@0`) Werte an einen SQL-Befehl übergeben wird *äußerst wichtig* für die Sicherheit. Die Möglichkeit, die Sie es hier mit Platzhaltern für Variable Daten sehen, ist die einzige Möglichkeit, die Sie die SQL-Befehlen erstellen soll.
> 
> Erstellen Sie eine SQL-Anweisung nie durch das Erstellen (verketten) Literaltext und Werte, die Sie vom Benutzer zu erhalten. Verkettung von Benutzereingaben in einer SQL-Anweisung öffnet die Website eine *SQL Injection-Angriff* , in denen ein böswilliger Benutzer Werte auf der Seite, die Ihre Datenbank hack übermittelt. (Erfahren Sie mehr in diesem Artikel [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) der MSDN-Website.)


## <a name="updating-the-movies-page-with-search-code"></a>Aktualisieren die Seite "Movies" mit Code durchsuchen

Nachdem Sie den Code aktualisieren, können die *Movies.cshtml* Datei. Ersetzen Sie zunächst den Code im Codeblock am oberen Rand der Seite durch den folgenden Code ein:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Der Unterschied besteht darin, dass Sie die Abfrage in erstellt haben die `selectCommand` Variable, die Sie übergeben `db.Query` später noch mal. Platzieren die SQL-Anweisung in einer Variablen können Sie die Anweisung ändern, was Sie tun müssen, um die Suche durchgeführt wird.

Sie haben auch diese beiden Zeilen und entfernt, die Sie später wieder ein werden:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Sie nicht möchten, führen Sie die Abfrage noch (d. h. Aufrufen `db.Query`) und nicht initialisiert werden soll die `WebGrid` Helper noch eine. Sie müssen diese Dinge tun, nachdem Sie ermittelt haben, das die SQL-Anweisung werden ausgeführt muss.

Nachdem dieses Blocks umgeschriebene können Sie die neue Logik zur Behandlung von der Suche hinzufügen. Der fertige Code sieht wie folgt. Aktualisieren Sie den Code auf der Seite, damit sie in diesem Beispiel entspricht:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Die Seite funktioniert jetzt wie folgt. Jedes Mal, wenn die Seite ausgeführt wird, wird der Code öffnet die Datenbank und die `selectCommand` Variable wird festgelegt, auf die SQL-Anweisung, die alle Datensätze aus erhält die `Movies` Tabelle. Der Code initialisiert, außerdem die `searchTerm` Variable.

Jedoch die aktuelle Anforderung enthält einen Wert für die `searchGenre` -Element, legt der Code `selectCommand` auf eine andere Abfrage – nämlich, beinhaltet die `Where` -Klausel, um ein Genre gesucht. Außerdem wird `searchTerm` , was für das Suchfeld übergeben wurde (die "nothing" sein kann).

Unabhängig davon, welche SQL-Anweisung liegt im `selectCommand`, der Code ruft dann `db.Query` zum Ausführen der Abfrage, und übergeben sie den befindet sich in `searchTerm`. Wenn es keine Werte `searchTerm`, es spielt keine Rolle, da in diesem Fall gibt es keinen Parameter, der den Wert zu übergeben, ist `selectCommand` trotzdem.

Der Code schließlich initialisiert der `WebGrid` Hilfe mit den Abfrageergebnissen wie.

Sie können sehen, indem Sie die SQL-Anweisung einfügen und den Suchbegriff in Variablen, haben Sie Flexibilität auf den Code hinzugefügt. Wie Sie später in diesem Lernprogramm sehen werden, können Sie dieses grundlegende Framework und behalten Sie die Logik für die verschiedenen Typen von Suchvorgängen hinzufügen.

## <a name="testing-the-search-by-genre-feature"></a>Testen die Suche von "Genre"-Funktion

Führen Sie in WebMatrix die *Movies.cshtml* Seite. Die Seite mit dem Textfeld für "Genre" angezeigt.

Geben Sie ein Genre, die für eine Ihrer Test-Datensätze eingegeben haben, und klicken Sie dann **Suche**. Dieses Mal sehen Sie eine Liste der einfach Filme, die dieses Genres entsprechen:

![Seite "Movies" nach der Suche nach Genre "Comedies" auflisten](form-basics/_static/image4.png)

Geben Sie einen anderen "Genre" und die Suche wiederholen. Versuchen Sie, das Genre mithilfe von allen klein- oder Großbuchstaben Buchstaben, damit Sie sehen, dass es sich bei der Suche nicht Groß-/Kleinschreibung beachtet werden.

## <a name="remembering-what-the-user-entered"></a>"Speichern" Eingabe des Benutzers

Sie haben vielleicht bemerkt, die nach dem Sie ein Genre eingegeben, und klicken auf **Search "Genre"**, Sie haben gesehen, eine Liste für dieses Genres. Das Suchfeld ein war jedoch leer &mdash; also nicht merken, was Sie eingegeben haben.

Es ist wichtig zu verstehen, warum dieses Verhalten tritt auf. Wenn Sie eine Seite senden, sendet der Browser eine Anforderung an den Webserver. Wenn ASP.NET die Anforderung erhält, erstellt eine völlig neue Instanz der Seite, führt den Code in der sie und rendert dann die Seite an den Browser erneut. Aktiviert ist, wissen nicht jedoch die Seite, dass Sie nur mit einer früheren Version von sich selbst arbeiteten. Alle bekannt ist, dass die It eine Anforderung erhalten wird, die einige Formulardaten darin.

Jedes Mal, wenn Sie eine Seite anfordern &mdash; angibt, ob zum ersten Mal, oder Senden des Updategrams &mdash; bekommen Sie eine neue Seite. Der Webserver verfügt über kein Speicher Ihrer letzten Anforderung. Wird weder auf ASP.NET und ist weder des Browsers. Die einzige Verbindung zwischen diesen separaten Instanzen der Seite ist, alle Daten, die Sie zwischen diesen übertragen. Wenn Sie eine Seite senden, kann die neue Seiteninstanz z. B. Daten aus dem Formular abrufen, die von der früheren Instanz gesendet wurde. (Eine weitere Möglichkeit zur Datenübergabe zwischen Seiten ist Cookies verwendet.)

Eine formale zum Beschreiben dieser Situation besteht darin, sagen, dass Webseiten sind *zustandslosen*. Webserver und den Seiten selbst und die Elemente auf der Seite beibehalten keine Informationen zu den vorherigen Zustand einer Seite. Das Web wurde so entwickelt, da das Beibehalten des Zustands für einzelne Anforderungen schnell erschöpfen würde die Ressourcen der Webserver, die häufig Tausende, vielleicht verarbeiten sogar Hunderte von Tausenden von Anforderungen pro Sekunde.

Warum ist das Textfeld leer war. Nachdem Sie die Seite übermittelt, wird von ASP.NET erstellt eine neue Instanz der Seite und über Code und Markup ausgeführt wurde. Gab es keine anderen in diesem Code, der sagte ASP.NET, einen Wert in das Textfeld platzieren. Also ASP.NET nicht nichts ein, und das Textfeld wurde ohne einen Wert darin gerendert.

Es ist tatsächlich eine einfache Möglichkeit zum Umgehen dieses Problems. Das Genre, die Sie in das Textfeld eingegeben *ist* im Code für Sie verfügbaren &mdash; befindet sich im `Request.QueryString["searchGenre"]`.

Aktualisieren Sie das Markup für das Textfeld ein, damit die `value` Attribut Ruft den Wert von `searchTerm`, wie im folgenden Beispiel:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Auf dieser Seite, Sie haben auch Festlegen der `value` -Attribut auf die `searchTerm` Variable, da diese Variable auch das Genre enthält Sie eingegeben haben. Während mit den `Request` Objekt zum Festlegen der `value` Attribut wie gezeigt, hier ist das Standardverfahren zum Ausführen dieser Aufgabe. (Vorausgesetzt, Sie möchten auch dazu &mdash; in einigen Fällen sollten Sie zum Rendern der Seite *ohne* Werte in den Feldern. Es hängt alles was mit Ihrer Anwendung vor sich geht.)

> [!NOTE]
> Sie können nicht "den Wert eines Textfelds, die verwendet wird, für die Kennwörter merken". Es wäre eine Sicherheitslücke Personen mithilfe von Code in ein Kennwortfeld zu füllen können.


Führen Sie die Seite erneut aus, geben Sie ein Genre, und klicken Sie auf **Search "Genre"**. Dieses Mal nicht nur sehen Sie die Ergebnisse der Suche, aber das Textfeld gespeichert, was letzten Besuch eingegeben haben:

![Seite mit, dass das Textfeld "des vorherigen Eintrags gespeichert hat"](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Suchen nach einem beliebigen Wort im Titel

Sie können jetzt für alle "Genre" suchen, aber Sie können auch einen Titel suchen möchten. Es ist schwierig, einen Titel, ganz genau zu erhalten, bei der Suche stattdessen Sie nach einem Wort suchen können, die einen Titel an einer beliebigen Stelle angezeigt wird. Zu diesem Zweck in SQL, die Sie verwenden die `LIKE` Operator und die Syntax wie folgt aus:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Dieser Befehl ruft alle Filme, deren Namen "Adventure" enthalten. Bei Verwendung der `LIKE` -Operator, enthalten Sie das Platzhalterzeichen `%` als Teil des Suchbegriffs. Die Suche `LIKE 'adventure%'` bedeutet "beginnend mit 'Adventure'". (Technisch gesehen bedeutet dies "Die Zeichenfolge 'Adventure' gefolgt von etwas".) Auf ähnliche Weise den gesuchten Begriff `LIKE '%adventure'` bedeutet "Alles gefolgt von der Zeichenfolge 'Adventure'", d.h., dass eine andere Möglichkeit, sagen Sie "bis hin zu 'Adventure'".

Der Suchbegriff `LIKE '%adventure%'` bedeutet daher "mit"Adventure' eine beliebige Stelle im Titel." (Technisch gesehen "alles im Titel, gefolgt von 'Adventure' ist, gefolgt von der nichts.")

In der `<form>` -Element, fügen Sie das folgende Markup direkt unter dem schließenden `</div>` Tag für die Suche für "Genre" (direkt vor dem abschließenden `</form>` Element):

[!code-html[Main](form-basics/samples/sample10.html)]

Der Code zum Behandeln dieser Suche ist ähnelt dem Code für die Suche für "Genre", mit dem Unterschied, dass Sie Zusammenstellen der `LIKE` suchen. Fügen Sie den Codeblock am oberen Rand der Seite, diese `if` blockieren direkt nach der `if` Block für die Suche für "Genre":

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Dieser Code verwendet die gleiche Logik, die Sie zuvor gesehen haben, mit dem Unterschied, dass die Suche verwendet einen `LIKE` Operator und der Code setzt "`%`" vor und nach dem Suchbegriff.

Beachten Sie, wie sie mühelos eine neue Suche auf der Seite hinzugefügt wurde. Alles, was Sie tun musste wurde:

- Erstellen Sie eine `if` blockieren, die getestet werden, um festzustellen, ob das relevante Suchfeld einen Wert verfügte.
- Legen Sie die `selectCommand` -Variable an eine neue SQL-Anweisung.
- Legen Sie die `searchTerm` Variable auf den Wert für die Abfrage übergeben.

Hier ist der vollständige Code-Block, enthält die Logik die neue für eine Suche nach Softwaretiteln:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Hier ist eine Zusammenfassung der Funktionsweise dieses Codes:

- Die Variablen `searchTerm` und `selectCommand` oben initialisiert werden. Diese Variablen mit dem entsprechenden Suchbegriff (sofern vorhanden) festgelegt werden soll, und entsprechende SQL-Befehl basierend auf der Funktionsweise des Benutzers auf der Seite. Die Standardsuche ist ein einfacher Vorgang alle Filme aus der Datenbank abrufen.
- In den Tests für `searchGenre` und `searchTitle`, legt der Code `searchTerm` auf den Wert, der Sie suchen möchten. Außerdem legen Sie diese Codeblöcke `selectCommand` auf einen entsprechenden SQL‑Befehl für die Suche.
- Die `db.Query` Methode wird aufgerufen, nur einmal verwendet den SQL-Befehl in `selectedCommand` und einen beliebigen Wert in `searchTerm`. Es ist kein Suchbegriff (keine "Genre" und kein Titel Wort) den Wert der `searchTerm` ist eine leere Zeichenfolge. Allerdings ist, die unerheblich, da in diesem Fall die Abfrage keine Parameter erfordert.

## <a name="testing-the-title-search-feature"></a>Testen die Suchfunktion für den Titel

Jetzt können Sie Ihre abgeschlossenen Suchseite testen. Führen Sie *Movies.cshtml*.

Geben Sie ein Genre, und klicken Sie auf **Search "Genre"**. Das Raster zeigt Filme, die von diesem "Genre", wie vor.

Geben Sie ein Wort Titel, und klicken Sie auf **Suchtitel**. Das Raster zeigt Filme, die das Wort im Titel haben.

![Seite "Movies" nach der Suche für "Der" im Titel auflisten](form-basics/_static/image6.png)

Lassen Sie beide Felder leer, und klicken Sie auf eine Schaltfläche. Das Raster zeigt alle Filme.

## <a name="combining-the-queries"></a>Kombinieren von Abfragen

Sie werden feststellen, dass die suchen, die Sie ausführen können ausgeschlossen werden. Sie können nicht den Titel und das Genre zur gleichen Zeit, suchen, selbst wenn beide Suchfelder Werte enthalten. Sie können z. B., können nicht für alle Aktion Filme suchen, deren Titel "Adventure" enthält. (Wie die Seite jetzt codiert ist, wenn Sie Werte für "Genre" und Titel eingeben, ruft der Suche nach Softwaretiteln Vorrang.) Um eine Suche zu erstellen, die die Bedingungen kombiniert, müssten Sie eine SQL-Abfrage zu erstellen, die Syntax folgendermaßen:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Und Sie müssten zum Ausführen der Abfrage mit einer Anweisung wie folgt (etwa "Sprache"):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Erstellen Logik, um die Anzahl der möglichen Permutationen von Suchkriterien zulassen kann etwas beteiligt sind, abrufen, wie Sie sehen können. Aus diesem Grund werden wir hier beenden.

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial erstellen Sie eine Seite, die einem Formular verwendet wird, um Benutzer der Datenbank Filme hinzugefügt.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Vollständige Liste für Movie-Seite (mit der Suche aktualisiert)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL-WHERE-Klausel](http://www.w3schools.com/sql/sql_where.asp) auf der Website W3Schools
- [Methodendefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) Artikel auf der W3C-Website

> [!div class="step-by-step"]
> [Zurück](displaying-data.md)
> [Weiter](entering-data.md)
