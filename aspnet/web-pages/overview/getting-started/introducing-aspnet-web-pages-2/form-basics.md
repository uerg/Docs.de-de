---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: "Einführung in ASP.NET Web Pages - HTML-Formular-Grundlagen | Microsoft Docs"
author: tfitzmac
description: In diesem Lernprogramm wird gezeigt, die Grundlagen der wie ein input-Formular erstellen und die Eingaben des Benutzers bei der Verwendung von ASP.NET Web Pages (Razor) zu behandeln. Und jetzt, die Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 97e4a2a1794dbdccf80f0b44c1246c743fa23019
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Einführung in ASP.NET Web Pages - HTML-Formular-Grundlagen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Lernprogramm wird gezeigt, die Grundlagen der wie ein input-Formular erstellen und die Eingaben des Benutzers bei der Verwendung von ASP.NET Web Pages (Razor) zu behandeln. Und nun, da Sie eine Datenbank haben, müssen Sie Ihr Formular wissen verwenden, können Benutzer bestimmte Filme in der Datenbank gefunden. Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Einführung zum Anzeigen von Daten mithilfe von ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Lernen Sie:
> 
> - Vorgehensweise: Erstellen Sie ein Formular mit standard-HTML-Elemente.
> - Gewusst wie: Lesen Sie den Benutzer Benutzereingabe in einem Formular.
> - Vorgehensweise: eine SQL-Abfrage erstellen, selektiv Daten mithilfe einer Suche abgerufen, die der Benutzer Begriff bereitstellt.
> - Wie Sie die Felder auf der Seite "merken" was der Benutzer eingegeben haben.
>   
> 
> Features/Technologien erläutert:
> 
> - Das `Request`-Objekt.
> - Die SQL `Where` Klausel.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Im vorherigen Lernprogramm Sie eine Datenbank erstellt, Daten hinzugefügt, und dann verwendet der `WebGrid` Hilfsmethode zum Anzeigen der Daten. In diesem Lernprogramm fügen Sie ein Suchfeld, mit der Sie die von einer bestimmten "Genre" finden, oder, deren Titel enthält den Wort, das Sie eingeben. (Z. B., müssen Sie, können alle Filme, deren "Genre" ist "Aktion", oder zu ermitteln, deren Titel enthält "Harry" oder "Adventure".)

Wenn Sie mit diesem Lernprogramm fertig sind, müssen Sie eine Seite, wie diese:

![Seite "Filme" mit "Genre" "und" Title-Suche](form-basics/_static/image1.png)

Die Auflistung Teil der Seite ist dasselbe wie bei der letzten Lernprogramm &mdash; ein Raster an. Der Unterschied werden, dass im Raster nur die Filme anzeigt, dass Sie nach dem gesucht werden.

## <a name="about-html-forms"></a>Informationen zu HTML-Formularen

(Wenn Sie Erfahrung mit HTML-Formulare erstellen und mit den Unterschied zwischen haben `GET` und `POST`, Sie können diesen Abschnitt überspringen.)

Ein Formular weist benutzereingabeelemente &mdash; Textfelder, Schaltflächen, Optionsfelder, Kontrollkästchen, Dropdownlisten und So weiter. Benutzer in diesen Steuerelementen füllen oder treffen und anschließend Absenden des Formulars durch Klicken auf eine Schaltfläche.

Die grundlegende HTML-Syntax eines Formulars wird in diesem Beispiel dargestellt:

[!code-html[Main](form-basics/samples/sample1.html)]

Wenn dieses Markup auf einer Seite ausgeführt wird, erstellt es ein einfaches Formular aus, das in dieser Abbildung ähnelt:

![Grundlegende als im Browser gerenderte HTML-Formular](form-basics/_static/image2.png)

Die `<form>` Element umschließt HTML-Elementen zur übermittelt werden. (Eine einfache Fehler vornehmen wird auf der Seite Elemente hinzufügen, aber vergessen Sie Einsatzort innerhalb einer `<form>` Element. In diesem Fall wird keine Aktion gesendet.) Die `method` Attribut teilt dem Browser, wie die Benutzereingabe zu übermitteln. Sie festlegen, um `post` , wenn Sie ein Update auf dem Server oder auf durchführen, `get` , wenn Sie nur Daten vom Server abgerufen werden.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST und HTTP-Verb für Sicherheit**
> 
> HTTP, das Protokoll, Browsern und Servern verwenden, für den Austausch von Informationen, ist erstaunlich einfach in seiner grundlegenden Vorgänge. Browsern werden nur einige Verben verwenden, damit Anforderungen an den Server senden. Beim Schreiben von Code für das Web ist es hilfreich zu verstehen, diese Verben und dem Browser und dem Server wie verwenden. Bei weitem sich dabei um die am häufigsten verwendeten Verben:
> 
> - `GET`. Der Browser verwendet das Verb etwas vom Server abgerufen. Z. B. Wenn Sie eine URL in Ihren Browser eingeben, die der Browser führt eine `GET` Vorgang die Seite angefordert werden sollen. Wenn die Seite Grafiken enthält, führt der Browser zusätzliche `GET` Operations um die Images abzurufen. Wenn die `GET` -Vorgang bleibt, Informationen an den Server zu übergeben, die Informationen als Teil der URL in der Abfragezeichenfolge übergeben wird.
> - `POST`. Der Browser sendet eine `POST` Anforderung an, um das Senden von Daten, die hinzugefügt oder auf dem Server geändert werden. Z. B. die `POST` Verb wird verwendet, um Datensätze in einer Datenbank erstellen oder vorhandene ändern. In den meisten Fällen, wenn Sie ein Formular auszufüllen, und klicken Sie auf die Schaltfläche "Absenden", um der Browser führt eine `POST` Vorgang. In einem `POST` -Operation, die Daten an den Server weitergegeben werden im Text der Seite ist.
> 
> Ein wichtiger Unterschied zwischen diesen Verben ist, die eine `GET` Vorgang sollte nicht ändert nichts auf dem Server – oder auf eine etwas geringere abstrakte Weise ausgedrückt ein `GET` Vorgang führt nicht zu einer Änderung auf dem Server. Können Sie Ausführen einer `GET` Vorgang auf die gleichen Ressourcen wie oft und solange Ihnen gefällt, und diese Ressourcen nicht ändern. (Ein `GET` Operation wird häufig als "sicheren" sein, oder um einen technischen Begriff zu verwenden ist *Idempotent*.) Im Gegensatz dazu sind natürlich eine `POST` Anforderung ändert sich etwas auf dem Server jedes Mal, führen Sie den Vorgang.
> 
> Zwei Beispiele für lassen sich diese Unterscheidung zu veranschaulichen. Beim Ausführen einer Suche mit einem Modul wie Bing oder Google, füllen Sie ein Formular, das von einem Textfeld besteht aus, und klicken Sie dann Sie auf die Schaltfläche "Suchen". Der Browser führt eine `GET` Vorgang, mit dem Wert, die Sie eingegeben, in das Feld, das als Teil der URL übergeben haben. Mit einem `GET` Vorgang für diese Art von Formular Ordnung, ist, da es sich bei ein Suchvorgang keine Ressourcen auf dem Server ändern, es nur Informationen abruft.
> 
> Nun sehen wir uns mit des Prozess etwas online bestellen. Sie füllen die Auftragsdetails enthält, und klicken Sie dann auf die Schaltfläche "Absenden". Dieser Vorgang wird eine `POST` anfordern, da der Vorgang Änderungen auf dem Server, z. B. einen neuen Auftragsdatensatz, eine Änderung in Ihre Kontoinformationen und möglicherweise viele weitere Änderungen führt. Im Gegensatz zu den `GET` -Vorgang können nicht Sie wiederholt Ihre `POST` Anforderung –, wenn Sie Sie, jedes Mal haben Sie erneut die Anforderung übermittelt würden Sie einen neuen Auftrag auf dem Server generieren. (In solchen Fällen Websites häufig gewarnt. Sie nicht auf eine Schaltfläche "Absenden" mehr als einmal klicken, oder wird die Schaltfläche "Absenden" deaktivieren, sodass Sie das Formular versehentlich erneut nicht.)
> 
> Im Verlauf dieses Lernprogramms verwenden Sie sowohl eine `GET` Vorgang und ein `POST` Vorgang zur Bearbeitung von HTML-Formularen. Wir erläutern in jeder Groß-/Kleinschreibung verdeutlichen, warum das Verb an, das Sie verwenden die entsprechenden handelt.
> 
> (Weitere Informationen zu HTTP-Verben finden Sie unter der [Methodendefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) Artikel auf der W3C-Website.)


Die meisten benutzereingabeelemente sind HTML `<input>` Elemente. Sie sehen, wie `<input type="type" name="name">,` , in denen *Typ* gibt die Art der Eingabe Benutzersteuerelement werden sollen. Diese Elemente sind die gängigsten Einträge:

- Textfeld:`<input type="text">`
- Kontrollkästchen:`<input type="check">`
- Optionsfeld:`<input type="radio">`
- Schaltfläche:`<input type="button">`
- Schaltfläche zum Absenden:`<input type="submit">`

Sie können auch die `<textarea>` Element, um ein mehrzeiliges Textfeld erstellen und die `<select>` Element, um eine Dropdown-Liste oder einer bildlauffähigen Liste erstellen. (Weitere Informationen zu HTML Elemente bilden, finden Sie unter [HTML-Formularen und Eingabe](http://www.w3schools.com/html/html_forms.asp) auf der Website W3Schools.)

Die `name` Attribut ist sehr wichtig, da der Name wie den Wert des Elements höher erhalten, ist wie Sie in Kürze sehen.

Ist das interessante daran, was Ihnen als Seitenentwickler tun, mit der Eingabe des Benutzers. Es ist kein integriertes Verhalten, die diesen Elementen zugeordnet. Stattdessen müssen Sie rufen Sie die Werte, die der Benutzer ausgewählt oder wurde eingegeben und etwas mit ihnen. Dies ist in diesem Lernprogramm lernen Sie.

> [!TIP] 
> 
> **HTML5- und als Eingabeformulare**
> 
> Wie Sie wissen vielleicht, HTML befindet sich im Übergangsstadium, und die neueste Version (HTML5) bietet Unterstützung für intuitiver Möglichkeiten für Benutzer Informationen eingeben. Beispielsweise können in HTML5, (der Entwickler der Seite ") der Seite Aufschluss darüber, dass der Benutzer ein Datum eingeben soll. Browser können Sie automatisch einen Kalender, anstatt dass der Benutzer ein Datum manuell eingeben anzeigen. Allerdings HTML5 ist neu und wird nicht in allen Browsern noch unterstützt.
> 
> ASP.NET Web Pages unterstützt HTML5 Benutzereingaben, wenn der Browser des Benutzers ist. Für einen Überblick über die neuen Attribute für die `<input>` Element in HTML5, finden Sie unter [HTML &lt;input&gt; -Attributtyp](http://www.w3schools.com/html/html_form_input_types.asp) auf der Website W3Schools.


## <a name="creating-the-form"></a>Erstellen des Formulars

In WebMatrix in der **Dateien** Arbeitsbereich, öffnen die *Movies.cshtml* Seite.

Nach dem schließenden `</h1>` Tag und vor dem Öffnen `<div>` Tag die `grid.GetHtml` aufrufen, fügen Sie das folgende Markup hinzu:

[!code-html[Main](form-basics/samples/sample2.html)]

Dieses Markup erstellt ein Formular, das ein mit dem Namen Textfeld `searchGenre` und eine Schaltfläche "Absenden". Den Text ein, und senden Sie die Schaltfläche in eingeschlossen sind ein `<form>` Element, dessen `method` -Attributsatz zur `get`. (Beachten Sie, dass, wenn Sie das Textfeld einfügen, und übermitteln die Schaltfläche ein `<form>` -Element, wird nichts übermittelt werden, wenn Sie die Schaltfläche klicken.) Sie verwenden die `GET` Verb hier, da Sie ein Formular erstellen, die Sie nimmt keine Änderungen auf dem Server, führt dies nur bei einer Suche. (Im vorherigen Lernprogramm Sie verwendet eine `post` Methode, die ist, wie Sie Änderungen an den Server übermitteln. Die in den nächsten Lernprogrammen erneut angezeigt.)

Führen Sie die Seite. Obwohl Sie jedes Verhalten für das Formular definiert haben, können Sie sehen, wie es aussieht:

![Suchfeld für "Genre" Filme Seite](form-basics/_static/image3.png)

Geben Sie einen Wert in das Textfeld ein, z. B. "Comedy." Klicken Sie dann auf **Suche "Genre"**.

Notieren Sie die URL der Seite. Da Sie festlegen, die `<form>` des Elements `method` -Attribut auf `get`, der eingegebene Wert ist jetzt Teil der Abfragezeichenfolge in der URL wie folgt:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Formularwerte lesen

Diese Seite enthält bereits Code, der die Datenbankdaten abruft und die Ergebnisse in einem Raster angezeigt. Jetzt müssen Sie Code hinzufügen, der den Wert des Textfelds liest, damit Sie eine SQL-Abfrage ausführen können, die den Suchbegriff enthält.

Da die-Methode des Formulars festgelegt werden, um `get`, Sie können den Wert, der in das Textfeld eingegeben wurde, mithilfe von Code wie folgt lesen:

`var searchTerm = Request.QueryString["searchGenre"];`

Die `Request.QueryString` Objekt (die `QueryString` Eigenschaft von der `Request` Objekt) enthält die Werte von Elementen, die im Rahmen des gesendet wurden die `GET` Vorgang. Die `Request.QueryString` Eigenschaft enthält eine *Auflistung* (eine Liste) von den Werten, die in das Formular übermittelt werden. Um einen einzelnen Wert zu erhalten, geben Sie den Namen des Elements, das Sie möchten. Daher verfügen eine `name` -Attribut auf die `<input>` Element (`searchTerm`), erstellt das Textfeld. (Weitere Informationen zu den `Request` Objekt, finden Sie unter der [Randleiste](#BKMK_TheRequestObject) höher.)

Es ist einfach genug, um den Wert des Textfelds zu lesen. Aber wenn der Benutzer nichts überhaupt in das Textfeld eingeben hat nicht jedoch geklickt **Suche** trotzdem, können Sie dass klicken ignorieren, da es nichts zum Suchen ist.

Der folgende Code ist ein Beispiel, das zeigt, wie Sie diese Bedingungen zu implementieren. (Müssen Sie diesen Code hinzufügen, die noch nicht; Sie erledigen, die Sie in wenigen Augenblicken.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Der Test wird auf diese Weise unterteilt:

- Rufen Sie den Wert der `Request.QueryString["searchGenre"]`, d. h. der Wert, der eingegeben wurde die `<input>` Element mit dem Namen `searchGenre`.
- Herausfinden, ob sie mithilfe von "leer" ist die `IsEmpty` Methode. Diese Methode ist das Standardverfahren, um festzustellen, ob ein Element (z. B. ein Formularelement) einen Wert enthält. Aber wirklich, können Sie nur dann, wenn es wichtig *nicht* leer, daher...
- Hinzufügen der `!` Operator vor dem `IsEmpty` testen. (Die `!` Operator bedeutet Logisches NOT).

Im Klartext, die gesamte `if` Bedingung übersetzt werden, in der folgenden: *Wenn das Formular SearchGenre Element nicht leer ist, klicken Sie dann...*

Dieser Block wird die Phase zum Erstellen einer Abfrage, die den Suchbegriff verwendet. Sie erledigen, die Sie im nächsten Abschnitt.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Das Request-Objekt**
> 
> Die `Request` Objekt enthält alle Informationen, die der Browser für Ihre Anwendung sendet, wenn eine Seite angefordert oder gesendet wird. Dieses Objekt enthält alle Informationen, die der Benutzer eingibt, wie z. B. Textfeldwerte oder eine Datei zum Hochladen. Es enthält auch alle möglichen zusätzliche Informationen, wie Cookies, Werte in der URL-Abfragezeichenfolge (sofern vorhanden), den Dateipfad der Seite, die, den Typ des Browsers, die der Benutzer verwendet wird, die Liste der Sprachen ausgeführt wird, die im Browser festgelegt werden, , und vieles mehr.
> 
> Die `Request` Objekt ist ein *Auflistung* (Liste) von Werten. Sie erhalten einen einzelnen Wert aus der Auflistung durch Angabe ihres Namens:
> 
> `var someValue = Request["name"];`
> 
> Die `Request` Objekt macht tatsächlich verschiedene Teilmengen. Zum Beispiel:
> 
> - `Request.Form`Gibt Werte von Elementen in der übermittelten `<form>` Element, wenn die Anforderung ist eine `POST` Anforderung.
> - `Request.QueryString`bietet Ihnen nur die Werte in der URL-Abfragezeichenfolge. (In einer URL wie `http://mysite/myapp/page?searchGenre=action&page=2`die `?searchGenre=action&page=2` Abschnitt der URL ist die Abfragezeichenfolge.)
> - `Request.Cookies`Sammlung erhalten Sie Zugriff auf Cookies, die der Browser gesendet wurde.
> 
> Einen Wert abgerufen, von denen Sie wissen in der übermittelten Form ist, können Sie `Request["name"]`. Alternativ können Sie spezifischeren Versionen `Request.Form["name"]` (für `POST` Anfragen) oder `Request.QueryString["name"]` (für `GET` Anforderungen). Natürlich *Namen* ist der Name des abzurufenden Elements.
> 
> Der Name des Elements, die Sie abrufen möchten, muss innerhalb der Sammlung eindeutig sein, das Sie verwenden. Deshalb ist die `Request` Objekt bietet die Teilmengen wie `Request.Form` und `Request.QueryString`. Nehmen wir an, dass Ihre Seite ein Formularelement mit dem Namen enthält `userName` und *auch* enthält ein Cookie mit dem Namen `userName`. Wenn Sie erhalten `Request["userName"]`, er ist mehrdeutig, ob der Formularwert oder das Cookie soll. Jedoch, wenn Sie erhalten `Request.Form["userName"]` oder `Request.Cookie["userName"]`, Sie sind explizit der Wert abgerufen wird.
> 
> Es wird empfohlen, spezifisch und verwenden die Teilmenge der `Request` , dass Sie interessiert, z. B. sind `Request.Form` oder `Request.QueryString`. Für die einfache Seiten, die Sie in diesem Lernprogramm erstellen, wird nicht wahrscheinlich wirklich jeglicher Unterschied erleichtern. Allerdings wie komplexere Seiten erstellen, unter Verwendung der expliziten Version `Request.Form` oder `Request.QueryString` können Sie die Probleme zu vermeiden, die auftreten können, wenn die Seite ein Formular (oder mehrere Formen) enthält Cookies, Abfragezeichenfolgenwerten und So weiter.


## <a name="creating-a-query-by-using-a-search-term"></a>Erstellen einer Abfrage mithilfe eines Suchbegriffs

Nun, dass Sie wissen, wie den Suchbegriff abgerufen, den der Benutzer eingegeben haben, können Sie eine Abfrage erstellen, die verwendet werden. Beachten Sie, um die Film-Elemente aus der Datenbank zu erhalten Sie eine SQL-Abfrage verwenden, die diese Anweisung ähnelt:

`SELECT * FROM Movies`

Um nur bestimmte Filme zu erhalten, müssen Sie eine Abfrage verwenden, enthält eine `Where` Klausel. Diese Klausel können Sie eine Bedingung für die Zeilen von der Abfrage zurückgegeben werden. Im Folgenden ein Beispiel:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Das Grundformat lautet `WHERE column = value`. Verschiedene Operatoren können neben gerade `=`, z. B. `>` (größer als), `<` (kleiner als), `<>` (ungleich), `<=` (kleiner als oder gleich), usw., je nachdem, was Sie suchen.

Für den Fall, dass Sie sich wundern, sind SQL-Anweisungen nicht Groß-/Kleinschreibung beachtet &mdash; `SELECT` ist identisch mit `Select` (oder sogar `select`). Allerdings Personen häufig Großbuchstaben Schlüsselwörter in einer SQL-Anweisung, wie z. B. `SELECT` und `WHERE`, um das Lesen zu erleichtern.

### <a name="passing-the-search-term-as-a-parameter"></a>Den Suchbegriff wird als Parameter übergeben.

Suchen nach einer bestimmten "Genre" einfach ist (`WHERE Genre = 'Action'`), aber Sie möchten in der Lage, alle "Genre" gesucht, die der Benutzer eingibt. Zu diesem Zweck erstellen Sie als SQL-Abfrage, die einen Platzhalter für den Wert zu suchen. Es wird mit diesem Befehl formuliert werden:

`SELECT * FROM Movies WHERE Genre = @0`

Der Platzhalter ist der `@` Zeichen, gefolgt von 0 (null). Wie Sie denken möglicherweise, eine Abfrage kann mehrere Platzhalter enthalten, und sie den Namen `@0`, `@1`, `@2`usw.

Um richten Sie die Abfrage, und der Wert tatsächlich übergeben, verwenden Sie den Code wie folgt:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Dieser Code ist ähnlich, was Sie bereits durchgeführt haben, um Daten im Raster anzuzeigen. Die einzigen Unterschiede sind:

- Die Abfrage enthält einen Platzhalter (`WHERE Genre = @0"`).
- Die Abfrage wird in einer Variablen eingefügt (`selectCommand`), bevor Sie die Abfrage direkt zu übergeben der `db.Query` Methode.
- Beim Aufrufen der `db.Query` -Methode, übergeben Sie die Abfrage und der Wert, der für den Platzhalter verwenden. (Wenn die Abfrage mehrere Platzhalter hatten, übergeben Sie sie als unterschiedliche Werte für die Methode.)

Wenn Sie alle diese Elemente zusammengestellt, erhalten Sie den folgenden Code:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Wichtig!** Mithilfe von Platzhaltern (z. B. `@0`) werden zum Übergeben von Werten an einen SQL-Befehl *äußerst wichtig* für die Sicherheit. Die hier mit Platzhaltern für Variablendaten, sehen besteht die einzige Möglichkeit, die SQL-Befehlen erstellt werden soll.
> 
> Erstellen Sie nie eine SQL-Anweisung, indem zusammenstellen (verketten) Literaltext und Werte, die Sie vom Benutzer erhalten. Verketten von Benutzereingaben in einer SQL-Anweisung Öffnet Ihre Website in einem *SQL-Injection-Angriff* ein böswilliger Benutzer übermittelt, in denen Werte auf die Seite, die Ihre Datenbank hack. (Erfahren Sie mehr im Artikel [SQL Injection](https://msdn.microsoft.com/en-us/library/ms161953.aspx) der MSDN-Website.)


## <a name="updating-the-movies-page-with-search-code"></a>Aktualisieren die Seite "Filme" mit Code suchen

Jetzt kann das Aktualisieren des Codes in der *Movies.cshtml* Datei. Um zu beginnen, ersetzen Sie den Code in den Codeblock am oberen Rand der Seite mit dem folgenden Code ein:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Der Unterschied besteht darin, dass Sie die Abfrage in gesteckt haben die `selectCommand` Variablen, die Sie übergeben müssen `db.Query` später erneut. Platzieren die SQL-Anweisung in einer Variablen können Sie die Anweisung ändern, die was Sie tun müssen, um die Suche auszuführen ist.

Sie haben auch diese beiden Zeilen entfernt, die Sie später in zurückgestellt werden:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Nicht noch die Abfrage ausgeführt werden soll (d. h. Aufrufen `db.Query`) und nicht initialisiert werden soll die `WebGrid` Helper noch entweder. Nachdem Sie ermittelt haben, welche SQL-Anweisung ausgeführt wurde, müssen Sie diese Schritte ausführen.

Nach dieser umgeschriebene Block können Sie die neue Logik zum Behandeln von die Suche hinzufügen. Der vollständige Code sieht wie folgt. Aktualisieren Sie den Code auf der Seite, damit sie in diesem Beispiel entspricht:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Die Seite funktioniert jetzt wie folgt. Jedes Mal, wenn die Seite ausgeführt wird, wird der Code öffnet die Datenbank und die `selectCommand` Variable wird festgelegt, auf die SQL-Anweisung, die alle Datensätze von abruft, die `Movies` Tabelle. Auch im Code initialisiert das `searchTerm` Variable.

Allerdings enthält die aktuelle Anforderung einen Wert für die `searchGenre` -Element, legt der Code `selectCommand` auf eine andere Abfrage – nämlich, enthält die `Where` -Klausel, um eine "Genre" suchen. Außerdem wird `searchTerm` nach Belieben für das Suchfeld übergeben wurde (die ' Nothing ' sein kann).

Unabhängig davon, welche SQL-Anweisung liegt im `selectCommand`, der Code ruft dann `db.Query` um die Abfrage auszuführen, und übergeben sie den befindet sich in `searchTerm`. Wenn es nichts `searchTerm`, es spielt keine Rolle, da in diesem Fall kein Parameter besteht um den Wert zu übergeben `selectCommand` trotzdem.

Schließlich im Code initialisiert das `WebGrid` Helper mit den Abfrageergebnissen, genauso wie zuvor.

Sie können sehen, indem Sie die SQL-Anweisung einfügen und den Suchbegriff in Variablen, haben Sie Flexibilität zum Code hinzugefügt. Wie Sie weiter unten in diesem Lernprogramm sehen werden, Sie können dieses grundlegende Framework und Logik für die verschiedenen Typen von Suchvorgängen hinzufügen zu halten.

## <a name="testing-the-search-by-genre-feature"></a>Testen die Suche nach "Genre"-Funktion

Führen Sie in WebMatrix die *Movies.cshtml* Seite. Die Seite mit dem Textfeld für "Genre" wird angezeigt.

Geben Sie eine "Genre", die Sie für einen der Datensätze Test eingegeben haben, und klicken Sie auf **Suche**. Dieses Mal sehen Sie eine Liste der soeben Filme, die dieses Genres entsprechen:

![Filme-Seite, die nach der Suche nach "Genre""Comedies" auflisten](form-basics/_static/image4.png)

Geben Sie einen anderen "Genre" und die Suche wiederholen. Versuchen Sie, die "Genre" mithilfe von klein- oder Großbuchstaben Großbuchstaben, damit Sie sehen, dass es sich bei der Suche nicht berücksichtigt Groß-/Kleinschreibung beachtet wird.

## <a name="remembering-what-the-user-entered"></a>"Merken", was der Benutzer eingegeben hat.

Möglicherweise haben Sie bemerkt, die nach einem "Genre" eingegeben und geklickt **Suche "Genre"**, einen Eintrag für dieses Genres gelernt. Das Suchfeld ein war jedoch leer &mdash; also nicht merken eingegebene hatte.

Es ist wichtig zu verstehen, warum dieses Verhalten tritt auf. Wenn Sie eine Seite senden, sendet der Browser eine Anforderung an den Webserver. Wenn ASP.NET die Anforderung weitergeleitet wird, erstellt eine völlig neue Instanz der Seite, führt den Code im sie und rendert die Seite an den Browser erneut. Aktiviert ist, aber weiß die Seite nicht, dass Sie nur mit einer früheren Version von sich selbst arbeiteten. Alle bekannt ist, dass die It eine Anforderung erhalten wird, die einige mussten Formulardaten darin.

Jedes Mal, wenn Sie eine Seite anfordern &mdash; angibt, ob zum ersten Mal oder Senden des Updategrams &mdash; können Sie eine neue Seite abrufen. Der Webserver verfügt über kein Speicher Ihrer letzten Anforderung. Wird keins ASP.NET und ist weder den Browser. Die einzige Verbindung zwischen diesen separaten Instanzen von der Seite "ist keine Daten, mit denen Sie zwischen ihnen zu übertragen. Wenn Sie auf eine Seite übermitteln, kann die neue Seiteninstanz z. B. die Formulardaten abrufen, die von der früheren Instanz gesendet wurde. (Eine weitere Möglichkeit zum Übergeben von Daten zwischen Seiten werden Cookies verwendet.)

Eine formale zur Beschreibung dieser Situation besteht darin, sagen, dass Webseiten sind *zustandslose*. Webserver und den Seiten selbst und die Elemente auf der Seite beibehalten Informationen zu den vorherigen Zustand einer Seite nicht. Im Web wurde so entwickelt werden, da Zustandsverwaltung Informationen zu einzelnen Anforderungen schnell ausgeschöpft würde die Ressourcen von Webservern, die häufig Tausende, vielleicht behandeln sogar Hunderte von Tausenden von Anforderungen pro Sekunde.

Daher das Textfeld leer war. Nachdem Sie die Seite übermittelt, ASP.NET erstellt eine neue Instanz der Seite, und Sie werden über den Code und Markup ausgeführt. Es wurde nichts in diesen Code, die ASP.NET für den einen Wert in das Textfeld eingefügt mitgeteilt. Daher ASP.NET haben nicht keinerlei, und das Textfeld wurde ohne einen Wert darin gerendert.

Es ist eigentlich eine einfache Möglichkeit, dieses Problem zu umgehen. Die "Genre", die Sie in das Textfeld eingegeben *ist* im Code für Sie verfügbaren &mdash; befindet sich im `Request.QueryString["searchGenre"]`.

Das Markup für das Textfeld zu aktualisieren, damit die `value` Attribut Ruft ab, deren Werte von `searchTerm`, wie im folgenden Beispiel:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Auf dieser Seite, Sie haben auch Festlegen der `value` -Attribut auf die `searchTerm` eingegebene Variable, da die Variable auch die "Genre" enthält. Während mit der `Request` Objekt, das Festlegen der `value` -Attribut an, wie die hier gezeigte der Standardmethode zur Ausführung dieser Aufgabe. (Vorausgesetzt, Sie möchten auch dazu &mdash; in einigen Situationen sollten Sie zum Rendern der Seite *ohne* Werte in den Feldern. Es hängt alles mit der app was.)

> [!NOTE]
> Sie können keine "den Wert eines Textfelds für die Kennwörter verwendeten merken". Es wäre eine Sicherheitslücke Personen mithilfe von Code ein Kennwortfeld ausfüllen können.


Führen Sie die Seite erneut aus, geben Sie eine "Genre", und klicken Sie auf **Suche "Genre"**. Dieses Mal nicht nur sehen Sie die Ergebnisse der Suche, aber das Textfeld speichert Sie zuletzt eingegeben haben:

![Seite "zeigt, dass das Textfeld 'den vorherigen Eintrag gespeichert ist"](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Suchen nach einem beliebigen Wort im Titel

Sie können jetzt für alle "Genre" suchen, aber Sie können auch einen Titel suchen möchten. Es ist schwierig, einen Titel genau richtig machen, bei der Suche stattdessen Sie ein Wort suchen können, die an einer beliebigen Stelle innerhalb eines Titels angezeigt wird. Verwenden Sie hierzu in SQL die `LIKE` Operator und die Syntax wie folgt:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Dieser Befehl ruft alle Filme, deren Namen "Adventure" enthalten. Bei Verwendung der `LIKE` -Operator, schließen Sie das Platzhalterzeichen `%` als Teil der Suchbegriff. Die Suche `LIKE 'adventure%'` bedeutet "beginnend mit 'Adventure'". (Technisch gesehen bedeutet dies "Die Zeichenfolge 'Adventure' gefolgt von nichts") Auf ähnliche Weise den Suchbegriff `LIKE '%adventure'` bedeutet, dass "nichts gefolgt von der Zeichenfolge 'Adventure'", die eine andere Möglichkeit zum Ansagen "endet mit 'Adventure'".

Der Suchbegriff `LIKE '%adventure%'` aus diesem Grund bedeutet "mit"Adventure' an einer beliebigen Stelle in der Titelleiste." (Technisch gesehen "Alles, was im Titel, gefolgt von 'Adventure', gefolgt von etwas.")

Innerhalb der `<form>` Element, fügen Sie das folgende Markup direkt unter dem schließenden `</div>` Tag für die Suche "Genre" (unmittelbar vor dem schließenden `</form>` Element):

[!code-html[Main](form-basics/samples/sample10.html)]

Der Code zum behandeln diese Suche ist ähnlich wie der Code für die Suche "Genre", mit dem Unterschied, dass zusammenstellen stehen Ihnen die `LIKE` suchen. In den Codeblock am oberen Rand der Seite, fügen Sie Folgendes `if` blockieren direkt nach der `if` Block für die Suche "Genre":

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Dieser Code verwendet die gleiche Logik, die Sie zuvor gesehen haben, mit dem Unterschied, dass die Suche verwendet einen `LIKE` Operator und der Code setzt "`%`" vor und nach dem Suchbegriff.

Beachten Sie, wie sie können ganz einfach eine andere Suche auf der Seite hinzugefügt wurde. Alles, was Sie musste wurde:

- Erstellen einer `if` Block, der getestet werden, um festzustellen, ob die relevanten Suchfeld einen Wert hat.
- Legen Sie die `selectCommand` -Variablen an eine neue SQL-Anweisung.
- Legen Sie die `searchTerm` Variable auf den Wert an die Abfrage übergeben.

Hier wird der vollständige Codeblock, der Logik die neue für eine Suche nach Softwaretiteln enthält:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Hier wird ein Überblick darüber, was bewirkt, dass dieser Code ein:

- Die Variablen `searchTerm` und `selectCommand` oben initialisiert werden. Sie sind im Begriff, diese Variablen auf den entsprechenden Suchbegriff (sofern vorhanden) festgelegt und die entsprechenden SQL-Befehl basierend auf der Wirkungsweise des Benutzers auf der Seite. Die Standardsuche ist der einfache Groß-/Kleinschreibung aller Filme aus der Datenbank abrufen.
- In den Tests für `searchGenre` und `searchTitle`, legt der Code `searchTerm` auf den Wert, der Sie suchen möchten. Auch festlegen, die Codeblöcke `selectCommand` auf einen entsprechenden SQL‑Befehl für die Suche.
- Die `db.Query` Methode ist nur einmal aufgerufen, mit der SQL-Befehl wird `selectedCommand` und der Wert in `searchTerm`. Es ist kein Suchbegriff (keine "Genre" und kein Titel Wort) den Wert des `searchTerm` ist eine leere Zeichenfolge. Allerdings ist, die unerheblich, da in diesem Fall die Abfrage keine Parameter erfordert.

## <a name="testing-the-title-search-feature"></a>Testen die Suchfunktion für den Titel

Jetzt können Sie Ihre abgeschlossenen Suchseite testen. Führen Sie *Movies.cshtml*.

Geben Sie eine "Genre", und klicken Sie auf **Suche "Genre"**. Das Raster zeigt dieses Genres Filme wie vor.

Geben Sie ein Wort Titel, und klicken Sie auf **Titelsuche**. Das Raster zeigt Filme, die das Wort im Titel haben.

![Filme-Seite, die nach der Suche für die"im Titel auflisten](form-basics/_static/image6.png)

Lassen Sie beide Textfelder leer, und klicken Sie auf eine Schaltfläche. Das Raster zeigt alle Filme.

## <a name="combining-the-queries"></a>Kombinieren von Abfragen

Sie können feststellen, die suchen, die Sie ausführen können exklusiv sind. Sie können nicht am Titel und an die "Genre" zur gleichen Zeit suchen, selbst wenn beide Suchfelder Werte enthalten. Sie können z. B. können nicht für alle Aktion Filme suchen, deren Titel "Adventure" enthält. (Wie die Seite nun codiert ist, wenn Sie Werte für "Genre" und Titel eingeben, ruft die Suche nach Softwaretiteln Vorrang.) Um eine Suche zu erstellen, die die Bedingungen kombiniert, müssten Sie eine SQL-Abfrage erstellen, die wie die folgende Syntax verfügt:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Sie müssten zum Ausführen der Abfrage mit einer Anweisung wie folgt (ungefähr sprechen):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Erstellen Logik, um viele Variationen von Suchkriterien zulassen kann etwas beteiligt sind, abrufen, wie Sie sehen können. Aus diesem Grund müssen wir hier beenden.

## <a name="coming-up-next"></a>Als Nächstes kommen

In den nächsten Lernprogrammen erstellen Sie eine Seite, die einem Formular verwendet wird, können Benutzer der Datenbank Filme hinzugefügt.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Vollständige Auflistung für Filmdetailseite (inklusive Suche)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL-WHERE-Klausel](http://www.w3schools.com/sql/sql_where.asp) auf der Website W3Schools
- [Methodendefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) Artikel auf der W3C-Website

>[!div class="step-by-step"]
[Zurück](displaying-data.md)
[Weiter](entering-data.md)
