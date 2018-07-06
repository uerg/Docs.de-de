---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Einführung in ASP.NET Web Pages - eingeben von Datenbankdaten mithilfe von Formularen | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Tutorial erfahren Sie, wie ein Anmeldeformular erstellen, und geben die Daten, die aus dem Formular in eine Datenbanktabelle bei Verwendung von ASP.NET Web Pages (...)
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 313fd6ff70af540d4dd749734f50e3fc24be0d29
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835760"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Einführung in ASP.NET Web Pages - eingeben von Datenbankdaten mithilfe von Formularen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie ein Anmeldeformular erstellen, und geben dann auf die Daten, die aus dem Formular in eine Datenbanktabelle bei Verwendung von ASP.NET Web Pages (Razor). Es wird vorausgesetzt, Sie haben die Reihe über [Grundlagen von HTML-Formularen in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Sie lernen Folgendes:
> 
> - Weitere Informationen zum Verarbeiten von Dateneingabeformularen.
> - Das Hinzufügen von Daten (Einfügen) in einer Datenbank.
> - Informationen, um sicherzustellen, dass Benutzer einen erforderlichen Wert in einem Formular (Gewusst wie: Überprüfen von Benutzereingaben) eingegeben haben.
> - Wie Validierungsfehler angezeigt.
> - So wechseln Sie zu einer anderen Seite von der aktuellen Seite.
>   
> 
> Features/Technologien erläutert:
> 
> - Die `Database.Execute`-Methode.
> - Die SQL-Anweisung `Insert Into` Anweisung
> - Die `Validation` Helper.
> - Die `Response.Redirect`-Methode.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial weiter oben, die Sie wurde gezeigt, wie Sie eine Datenbank zu erstellen, eingegebene Daten durch Bearbeitung der Datenbank direkt in WebMatrix, arbeiten der **Datenbank** Arbeitsbereich. In den meisten apps ist, die nicht über eine praktische Möglichkeit zum Einfügen von Daten in die Datenbank jedoch. In diesem Tutorial haben erstelle Sie eine webbasierte Schnittstelle, mit dem Sie oder jemand die Daten eingeben und speichern Sie sie in der Datenbank.

Sie erstellen eine Seite, in dem Sie neue Filme eingeben können. Die Seite enthält ein Anmeldeformular, die Felder (Textfelder) verfügt, in dem Sie einen Filmtitel, "Genre" und das Jahr eingeben können. Die Seite sieht wie auf dieser Seite aus:

![Seite im Browser "Film hinzufügen"](entering-data/_static/image1.png)

Textfelder werden HTML `<input>` Elemente, die sehen, wie dieses Markup:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Erstellen das grundlegende Eintragsformular

Erstellen Sie eine Seite mit dem Namen *AddMovie.cshtml*.

Ersetzen Sie dies, was in der Datei mit dem folgenden Markup ist. Überschreiben Sie alles, was; Sie müssen einen Codeblock in Kürze oben hinzufügen.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Dieses Beispiel zeigt die typischen HTML zum Erstellen eines Formulars. Er verwendet `<input>` Elemente für die Textfelder und für die Schaltfläche "Senden". Die Beschriftungen für die Textfelder werden mit standardmäßigen erstellt `<label>` Elemente. Die `<fieldset>` und `<legend>` Elemente fügen Sie einen guten Rahmen um das Formular herum.

Beachten Sie, dass auf dieser Seite die `<form>` -Element verwendet `post` als Wert für die `method` Attribut. Im vorherigen Tutorial haben Sie ein Formular zum erstellt die `get` Methode. Das war richtig ist, da auch das Formular Werte an den Server gesendet, die Anforderung keine Änderungen vorgenommen. Alles, was zuvor war Abrufen der Daten auf unterschiedliche Weise. Allerdings in dieser Seite *wird* Änderungen vornehmen – also Datenbank neue Datensätze hinzufügen. Daher sollte dieses Formular verwendet die `post` Methode. (Weitere Informationen zu den Unterschieden zwischen `GET` und `POST` Vorgänge, finden Sie unter den[GET, POST und HTTP-Verb Sicherheit](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) Randleiste im vorherigen Tutorial.)

Beachten Sie, dass jedes Textfeld einen `name` Element (`title`, `genre`, `year`). Im vorherigen Tutorial haben Sie gesehen, werden diese Namen wichtig, da müssen Sie diesen Namen haben, sodass Sie die Eingabe des Benutzers später abrufen können. Sie können beliebige Namen verwenden. Es ist hilfreich, verwenden Sie aussagekräftige Namen ein, mit denen Sie denken Sie daran, welche Daten Sie arbeiten.

Die `value` -Attribut der einzelnen `<input>` Element enthält ein wenig mit Razor-Code (z. B. `Request.Form["title"]`). Sie erfahren eine Version von diesen Trick im vorherigen Tutorial zur Beibehaltung des Werts in das Textfeld ein (sofern vorhanden) eingegeben haben, nachdem das Formular übermittelt wurde.

## <a name="getting-the-form-values"></a>Die Formularwerte abrufen

Als Nächstes fügen Sie Code, der das Formular verarbeitet. In der Gliederung müssen Sie Folgendes tun:

1. Überprüfen Sie, ob die Seite zurückgesendet wird, wird (übermittelt). Sie möchten Ihren Code nur ausgeführt, wenn Benutzer auf die Schaltfläche geklickt haben, nicht verwendet werden, wenn die Seite zuerst ausgeführt wird.
2. Die der Benutzer in die Textfelder eingegebenen Werte zu erhalten. In diesem Fall, da das Formular verwendet die `POST` -Verb, erhalten Sie die Formularwerte aus der `Request.Form` Auflistung.
3. Fügen Sie die Werte als einen neuen Eintrag in der *Filme* Datenbanktabelle.

Fügen Sie am Anfang der Datei den folgenden Code hinzu:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Die ersten Zeilen erstellen Sie Variablen (`title`, `genre`, und `year`) zum Speichern der Werte aus den Textfeldern. Die Zeile `if(IsPost)` stellt sicher, dass die Variablen festgelegt werden *nur* Benutzer beim Klicken auf die **Film hinzufügen** Schaltfläche – d. h., wenn das Formular gesendet wurde.

In einem früheren Tutorial haben Sie gesehen, Sie erhalten den Wert eines Textfelds mithilfe eines Ausdrucks wie `Request.Form["name"]`, wobei *Namen* ist der Name des der `<input>` Element.

Die Namen der Variablen (`title`, `genre`, und `year`) sind willkürlich. Wie Sie die Namen, die Sie zuweisen `<input>` Elemente, man kann anrufen beliebigen. (Die Namen der Variablen nicht entsprechend die Attributen Name der haben `<input>` Elemente auf dem Formular.) Aber wie bei der `<input>` Elemente, es ist eine gute Idee, Variablennamen verwenden, die die Daten enthalten, die sie enthalten. Wenn Sie Code schreiben, erleichtern konsistente Namen für die Sie Daten speichern, mit dem Sie arbeiten.

## <a name="adding-data-to-the-database"></a>Hinzufügen von Daten in der Datenbank

Im Code blockieren Sie die soeben hinzugefügte, nur die *in* die schließende geschweifte Klammer ( `}` ) von der `if` -Block (nicht nur in den Codeblock), fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

In diesem Beispiel ähnelt der Code, den Sie in einem vorherigen Tutorial zum Abrufen und Anzeigen von Daten verwendet. Die Zeile, die mit beginnt `db =` geöffnet, die die Datenbank, z. B. vor, und die nächste Zeile definiert eine SQL-Anweisung erneut als Sie gesehen haben, bevor Sie. Dieses Mal es definiert jedoch eine SQL `Insert Into` Anweisung. Das folgende Beispiel zeigt die allgemeine Syntax von der `Insert Into` Anweisung:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Das heißt, geben Sie die Tabelle einzufügen, führen Sie dann die Spalten zum Einfügen in und Liste klicken Sie dann auf die einzufügenden Werte. (Wie bereits erwähnt, ist SQL nicht Groß-/Kleinschreibung beachtet, aber manche profitieren, die Schlüsselwörter, um es zu vereinfachen, lesen den Befehl.)

Die Spalten, die Sie in einfügen bereits im Befehl angegeben werden – `(Title, Genre, Year)`. Der interessante Teil befindet, wie Sie die Werte aus den Textfeldern in Abrufen der `VALUES` Teil des Befehls. Anstelle der tatsächlichen Werte angezeigt `@0`, `@1`, und `@2`, das sind natürlich Platzhalter. Wenn Sie den Befehl ausführen (auf der `db.Execute` Zeile), übergeben Sie die Werte, die Sie aus den Textfeldern erhalten haben.

**Wichtig** Beachten Sie, dass die einzige Möglichkeit, Sie sollte jemals enthalten Daten, die online von einem Benutzer in einer SQL-Anweisung eingegeben ist die Verwendung von Platzhaltern, wie Sie hier sehen (`VALUES(@0, @1, @2)`). Wenn Sie auf der Benutzereingabe in eine SQL-Anweisung verketten, Sie öffnen, selbst für eine SQL-Injection-Angriff, wie unter [Form-Grundlagen in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (das vorherige Lernprogramm).

Immer noch innerhalb der `if` blockieren, die folgende Zeile nach dem Hinzufügen der `db.Execute` Zeile:

[!code-css[Main](entering-data/samples/sample4.css)]

Nachdem der neue Film in die Datenbank eingefügt worden ist, springt dieser Zeile Sie (Umleitung) an die *Filme* Seite, damit Sie den Film sehen können, die Sie gerade eingegeben haben. Die `~` Operator bedeutet "Root der Website". (Die `~` Operator funktioniert nur in ASP.NET-Seiten nicht in HTML-Code in der Regel.)

Der Block der vollständige Code sieht wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testen des Insert-Befehls (bisher)

Sie sind noch nicht fertig, aber jetzt ist ein guter Zeitpunkt, zu testen.

In der Strukturansicht der Dateien in WebMatrix, mit der Maustaste der *AddMovie.cshtml* Seite, und klicken Sie dann auf **in Browser starten**.

![Seite im Browser "Film hinzufügen"](entering-data/_static/image2.png)

(Wenn Sie sich mit einer anderen Seite im Browser beenden, stellen Sie sicher, dass die URL `http://localhost:nnnnn/AddMovie`), wobei *Nnnnn* ist die Portnummer, die Sie verwenden.)

Haben Sie eine Fehlerseite erhalten? Wenn dies der Fall ist, lesen Sie sie sorgfältig, und stellen Sie sicher, dass der Code genau wie weiter oben aufgeführt wurde.

Geben Sie einen Film in der Form &mdash; verwenden, z. B. "Citizen Kane", "Drama" und "1941". (Oder was auch immer.) Klicken Sie dann auf **Film hinzufügen**.

Wenn alles gut geht, werden Sie umgeleitet, um die *Filme* Seite. Stellen Sie sicher, dass Ihre neue Film aufgeführt ist.

![Seite "Movies" mit neu hinzugefügt Film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

Wechseln Sie zurück zu den *AddMovie* Seite, oder führen Sie es noch mal. Geben Sie einen anderen Film, aber dieses Mal ein, geben Sie nur den Titel &mdash; Geben Sie beispielsweise "Einloggen ' in der Rain". Klicken Sie dann auf **Film hinzufügen**.

Sie werden umgeleitet, um die *Filme* Seite erneut. Sie finden den neuen Film, jedoch ist es unvollständig.

![Filme Seite mit neuen Film, die einige Werte fehlen](entering-data/_static/image4.png)

Bei der Erstellung der *Filme* Tabelle Sie explizit angegeben, dass keines der Felder null sein kann. Hier haben Sie ein Anmeldeformular für neue Filme, und Sie können Felder leer lassen. Dies ist ein Fehler.

In diesem Fall nicht die Datenbank tatsächlich auslösen (oder *auslösen*) einen Fehler. Sie nicht angegeben haben einen "Genre" oder ein Jahr, also den Code in die *AddMovie* Seite behandelt diese Werte als so genannten *leere Zeichenfolgen*. Wenn die SQL-Anweisung `Insert Into` -Befehl ausgeführt haben, der Felder für "Genre" und das Jahr nicht nützliche Daten darin enthalten, aber sie waren nicht null.

Natürlich möchten keine Benutzer, die Hälfte leere Filminformationen in die Datenbank eingeben können. Die Lösung ist die Eingabe des Benutzers zu überprüfen. Zunächst die Überprüfung stellt einfach sicher, dass der Benutzer einen Wert für alle Felder eingegeben hat (d. h. keine davon enthält eine leere Zeichenfolge).

> [!TIP]
> 
> **NULL und leere Zeichenfolgen**
> 
> Bei der Programmierung wird unterschieden zwischen verschiedenen Arten von "kein Wert." Ein Wert in der Regel ist *null* Wenn nicht festgelegt oder in keiner Weise initialisiert wurde. Im Gegensatz dazu kann eine Variable, die Zeichendaten (Zeichenfolgen) erwartet festgelegt werden, um eine *eine leere Zeichenfolge*. In diesem Fall ist der Wert nicht null; Es wird nur explizit auf eine Zeichenfolge mit Zeichen festgelegt wurde, dessen Länge 0 (null) ist. Diese beiden Anweisungen zeigen die Unterschiede:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Es wurde ein wenig komplizierter als die, aber der wichtigste Punkt ist, die `null` eine Art unbestimmten Zustand darstellt.
> 
> Und wieder ist es wichtig zu wissen genau, wenn ein Wert null ist und wenn es nur eine leere Zeichenfolge ist. Im Code für die *AddMovie* Seite erhalten Sie die Werte der Textfelder mit `Request.Form["title"]` und so weiter. Wenn die Seite zuerst ausgeführt wird, (bevor Sie die Schaltfläche klicken), wird der Wert des `Request.Form["title"]` ist null. Aber wenn Sie das Formular übermitteln `Request.Form["title"]` Ruft den Wert des der `title` Textfeld. Ist es nicht offensichtlich, aber ein leeres Textfeld ist nicht null; Es enthält nur eine leere Zeichenfolge. Klicken Sie daher beim Ausführen des Codes als Reaktion auf die Schaltfläche auf, `Request.Form["title"]` verfügt über eine leere Zeichenfolge.
> 
> Warum ist diese Unterscheidung wichtig? Bei der Erstellung der *Filme* Tabelle Sie explizit angegeben, dass keines der Felder null sein kann. Aber hier haben Sie ein Anmeldeformular für neue Filme, und Sie können Felder leer lassen. Sie erwarten einigermaßen die Datenbank, sich zu beschweren, bei dem Versuch neuer Filme zu speichern, die Werte für "Genre" oder ein Jahr nicht hatten. Aber darum geht es &mdash; , auch wenn Sie diese Textfelder leer lassen, nicht die Werte null sind; sie leere Zeichenfolgen sind. Sie sind daher Speichern neuer Filme auf die Datenbank mit diesen Spalten leer &mdash; jedoch nicht null. &mdash;-Werte sind. Aus diesem Grund müssen Sie sicherstellen, dass Benutzer keine leere Zeichenfolge ist,, dies können Sie senden durch die Eingabe des Benutzers überprüfen.


### <a name="the-validation-helper"></a>Der Überprüfungshelfer

ASP.NET Web Pages enthält eine Hilfsprogramm &mdash; der `Validation` Helper &mdash; , Sie verwenden können, um sicherzustellen, dass Benutzer Daten eingeben, die Ihren Anforderungen entspricht. Die `Validation` Hilfsprogramm ist eine der Hilfsprogramme, die für ASP.NET Web Pages, sodass Sie keine installieren Sie es als ein Paket mithilfe von NuGet, die Möglichkeit, die Installation der Gravatar-Hilfe in einem früheren Tutorial enthalten ist.

Um die Eingabe des Benutzers zu überprüfen, müssen Sie Folgendes tun:

- Verwenden Sie Code, um anzugeben, dass die Werte in die Textfelder auf der Seite erforderlich sein soll.
- Fügen Sie einen Test in den Code, damit die Filminformationen in der Datenbank hinzugefügt wird, nur dann, wenn alles ordnungsgemäß überprüft.
- Fügen Sie Code in das Markup zum Anzeigen von Fehlermeldungen an.

In den Codeblock in die *AddMovie* Seite, fügen Sie oben rechts oben Variablendeklarationen, bevor Sie den folgenden Code:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Rufen Sie `Validation.RequireField` einmal für jedes Feld (`<input>` Element), in dem einen Eintrag erstellt werden sollen. Sie können auch eine benutzerdefinierte Fehlermeldung für jeden Aufruf, wie Sie hier hinzufügen. (Wir verschiedene Nachrichten nur an, dass Sie einfügen können beliebig vorhanden.)

Wenn ein Problem vorliegt, möchten Sie verhindern, dass der neue Filminformationen in der Datenbank eingefügt wird. In der `if(IsPost)` -Block `&&` (logisches AND), um eine andere Bedingung hinzuzufügen, die testet `Validation.IsValid()`. Wenn Sie fertig sind, die gesamte `if(IsPost)` Block sieht aus wie im folgenden Code:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Wenn ein Validierungsfehler mit einem der Felder, die Sie registriert den `Validation` Hilfsprogramms, des der `Validation.IsValid` Methode gibt false zurück. Und in diesem Fall kein Teil des Codes in diesen Block wird ausgeführt, damit keine ungültigen Movie-Einträge in der Datenbank eingefügt werden. Und Sie sind natürlich nicht weitergeleitet die *Filme* Seite.

Der vollständigen Code-Block, einschließlich des Validierungscodes, sieht nun wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Anzeigen von Überprüfungsfehlern

Im letzte Schritt werden alle Fehlermeldungen angezeigt. Können Sie einzelne Nachrichten für jede Validierungsfehler anzeigen, oder Sie können eine Zusammenfassung oder beides anzeigen. Für dieses Tutorial müssen Sie beide Methoden verwenden, damit Sie sehen, wie es funktioniert.

Neben den jeweiligen `<input>` -Element, das Sie überprüfen können, rufen die `Html.ValidationMessage` Methode und übergeben sie den Namen der der `<input>` Element, die Sie überprüfen können. Sie fügen die `Html.ValidationMessage` Methode rechts, in dem die Fehlermeldung angezeigt werden sollen. Wenn die Seite ausgeführt wird, wird die `Html.ValidationMessage` -Methode rendert ein `<span>` Element, in dem der Validierungsfehler geht. (Wenn kein Fehler vorliegt, die `<span>` Element gerendert wird, aber es ist kein Text darin.)

Ändern Sie das Markup auf der Seite, sodass es enthält eine `Html.ValidationMessage` -Methode für jede der drei `<input>` Elemente auf der Seite, wie im folgenden Beispiel:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Um die Funktionsweise der Zusammenfassung angezeigt wird, außerdem fügen Sie das folgende Markup und code direkt nach der `<h1>Add a Movie</h1>` Element auf der Seite:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

In der Standardeinstellung die `Html.ValidationSummary` Methode zeigt alle Nachrichten für die Validierung in einer Liste (ein `<ul>` Elements in einer `<div>` Element). Wie bei der `Html.ValidationMessage` -Methode, das Markup für die Zusammenfassung der Validierung wird immer gerendert; Wenn keine Fehler vorliegen, werden keine Listenelemente gerendert.

Die Zusammenfassung kann eine alternative Möglichkeit zum validierungsmeldungen werden nicht angezeigt werden die `Html.ValidationMessage` Methode, um jedes Feld-spezifischer Fehler anzuzeigen. Sie können auch sowohl eine Zusammenfassung als auch die Details. Sie können auch die `Html.ValidationSummary` Methode, um eine generische Fehlermeldung angezeigt, und verwenden Sie dann auf einzelne `Html.ValidationMessage` aufrufen, um Details anzuzeigen.

Auf der Seite abgeschlossen sieht nun wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Das ist alles. Sie können die Seite jetzt testen, durch das Hinzufügen eines Films, aber eine oder mehrere Felder auslassen. Wenn Sie dies tun, sehen Sie den folgenden Fehler angezeigt:

![Hinzufügen von Film-Seite, die mit der Überprüfung Fehlermeldungen](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Die Validierungsfehlermeldungen formatieren

Sie können sehen, dass Fehlermeldungen, aber sie nicht wirklich sehr gut hervorstechen. Es ist eine einfache Möglichkeit, die Fehlermeldungen, jedoch zu formatieren.

So formatieren Sie die einzelne Fehlermeldungen, die angezeigt werden `Html.ValidationMessage`, erstellen Sie eine CSS-Formatklasse `field-validation-error`. Um das Aussehen für die Zusammenfassung der Validierung zu definieren, erstellen Sie eine CSS-Formatklasse `validation-summary-errors`.

Um zu sehen, wie dieses Verfahren funktioniert, fügen einen `<style>` Element innerhalb der `<head>` Abschnitt der Seite. Definieren Sie dann die Style-Klassen, die mit dem Namen `field-validation-error` und `validation-summary-errors` , enthalten die folgenden Regeln:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalerweise legen Sie wahrscheinlich Formatinformationen in einer separaten *CSS* -Datei, aber aus Gründen der Einfachheit Sie können sie auf der Seite jetzt. (Weiter unten in diesem Tutorial festgelegt, verschieben Sie die CSS-Regeln in einer separaten *CSS* Datei.)

Wenn ein Validierungsfehler, die `Html.ValidationMessage` -Methode rendert ein `<span>` -Element, das umfasst `class="field-validation-error"`. Durch Hinzufügen einer Formatdefinition für diese Klasse, können Sie konfigurieren, wie die Nachricht aussieht. Wenn Fehler vorliegen, die `ValidationSummary` Methode entsprechend dynamisch rendert das Attribut `class="validation-summary-errors"`.

Führen Sie die Seite erneut aus, und lassen Sie absichtlich ein Paar Felder aus. Die Fehler sind jetzt auffälliger. (In der Tat sie overdone sind, aber das ist um zu zeigen, was Sie tun können.)

![Anzeigen von Validierungsfehlern, die Formatvorlage wurde Film-Seite hinzufügen](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Hinzufügen eines Links zu der Seite "Movies"

Ein letzter Schritt ist, es praktisch sein, die zum Abrufen der *AddMovie* Seite aus dem ursprünglichen Movie-Angebot.

Öffnen der *Filme* Seite erneut. Nach dem schließenden `</div>` Tag, das folgt der `WebGrid` Helper, fügen Sie das folgende Markup hinzu:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Wie Sie bereits gesehen haben, ASP.NET interpretiert die `~` Operator als Stamm der Website. Sie müssen keine verwenden die `~` Operator ist; können Sie das Markup `<a href="./AddMovie">Add a movie</a>` oder eine andere Möglichkeit, den Pfad zu definieren, die HTML versteht. Aber die `~` Operator ist ein guter allgemeiner Ansatz bei der Erstellung von Links für Razor-Seiten, da die Website eine flexiblere ist – Wenn Sie die aktuelle Seite in einen Unterordner verschieben, wechseln weiterhin der Link zur der *AddMovie* Seite. (Beachten Sie, dass die `~` Operator funktioniert nur in *.cshtml* Seiten. ASP.NET verstanden werden, aber es ist keine standard-HTML.)

Wenn Sie fertig sind, führen Sie die *Filme* Seite. Es sieht so aus wie auf dieser Seite:

![Seite "Movies" mit Link zur Seite "Movies hinzufügen" "](entering-data/_static/image7.png)

Klicken Sie auf die **fügen Sie einen Film** Link, um sicherzustellen, dass darin auf die *AddMovie* Seite.

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial erfahren Sie, wie, damit Benutzer Daten zu bearbeiten, die bereits in der Datenbank vorhanden ist.

## <a name="complete-listing-for-addmovie-page"></a>Vollständige Liste für AddMovie-Seite

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO-Anweisung](http://www.w3schools.com/sql/sql_insert.asp) auf der Website W3Schools
- [Überprüfen der Benutzereingabe in der ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002). Weitere Informationen zum Arbeiten mit der `Validation` Helper.

> [!div class="step-by-step"]
> [Zurück](form-basics.md)
> [Weiter](updating-data.md)
