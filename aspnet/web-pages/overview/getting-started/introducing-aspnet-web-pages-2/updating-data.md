---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: "Einführung in ASP.NET Web Pages - Aktualisieren von Datenbankdaten | Microsoft Docs"
author: tfitzmac
description: "In diesem Lernprogramm wird gezeigt, wie (ändern) einer vorhandenen Datenbank-Eintrag zu aktualisieren, wenn Sie ASP.NET Web Pages (Razor) verwenden. Es wird vorausgesetzt, Sie haben die Reihe abgeschlossen th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 6fdb365c1449e6c54dfdbe492211700211f61005
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Einführung in ASP.NET Web Pages - Aktualisieren von Datenbankdaten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Lernprogramm wird gezeigt, wie (ändern) einer vorhandenen Datenbank-Eintrag zu aktualisieren, wenn Sie ASP.NET Web Pages (Razor) verwenden. Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [eingeben von Daten von mithilfe von Formularen mithilfe von ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251582).
> 
> Lernen Sie:
> 
> - Gewusst wie: auswählen ein einzelnes Datensatzes in die `WebGrid` Helper.
> - Wie Sie einen einzelnen Datensatz aus einer Datenbank zu lesen.
> - Wie Sie ein Formular mit Werten aus den Datenbankdatensatz vorab zu laden.
> - Wie Sie einen vorhandenen Datensatz in einer Datenbank zu aktualisieren.
> - Wie Sie Informationen auf der Seite zu speichern, ohne diese anzuzeigen.
> - Wie Sie ein verborgenes Feld zu verwenden, um Informationen zu speichern.
>   
> 
> Features/Technologien erläutert:
> 
> - Die `WebGrid` Helper.
> - Die SQL `Update` Befehl.
> - Die `Database.Execute`-Methode.
> - Ausgeblendete Felder (`<input type="hidden">`).


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Im vorherigen Lernprogramm haben Sie gelernt, wie beim Hinzufügen eines Datensatzes in einer Datenbank. Hier erfahren Sie, wie Sie einen Datensatz für die Bearbeitung anzeigen. In der *Filme* Seite, aktualisieren Sie die `WebGrid` Helper so, dass die It zeigt ein **bearbeiten** neben jeder Film:

![WebGrid anzuzeigen, z. B. einen Link "Bearbeiten" für jede Film](updating-data/_static/image1.png)

Beim Klicken auf die **bearbeiten** Link, es führt Sie zu einer anderen Seite ist, in dem die Informationen bereits in einem Formular:

![Bearbeiten Sie die filmdetailseite Film bearbeitet werden angezeigt](updating-data/_static/image2.png)

Sie können die Werte ändern. Wenn Sie die Änderungen übermitteln, der Code auf der Seite Datenbankupdates ausgeführt sowie gelangen Sie zur Auflistung Film zurück.

In diesem Teil des Prozesses funktioniert fast genauso wie die *AddMovie.cshtml* Seite, die Sie im vorherigen Lernprogramm erstellt haben, damit Großteil dieses Lernprogramms vertraut werden.

Es gibt mehrere Möglichkeiten, die Sie eine Möglichkeit, einen einzelne Film bearbeiten implementieren können. Der Ansatz gezeigt wurde gewählt, weil es sich um einfach zu implementieren und leicht verständlich ist.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Die Film-Auflistung hinzufügen einen Link

Um zu beginnen, aktualisieren Sie die *Filme* Seite, damit jedes Films auch auflisten enthält ein **bearbeiten** Link.

Öffnen der *Movies.cshtml* Datei.

Ändern Sie in den Text der Seite, die `WebGrid` Markup durch Hinzufügen einer Spalte. So sieht die geänderte Markup aus:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Die neue Spalte wird diese:

[!code-html[Main](updating-data/samples/sample2.html)]

Der Zeitpunkt des in dieser Spalte ist, um einen Link anzeigen (`<a>` Element), dessen Text besagt, dass "Bearbeiten". Was wir nach besteht darin, einen Link zu erstellen, die wie folgt aussieht, wenn die Seite ausgeführt wird, mit der `id` Wert für jede Film unterschiedlich:

[!code-css[Main](updating-data/samples/sample3.css)]

Dieser Link wird eine Seite mit dem Namen aufrufen *EditMovie*, und die Abfragezeichenfolge unverarbeitet `?id=7` zu dieser Seite.

Die Syntax für die neue Spalte kann etwas komplexer aussehen, aber das ist nur möglich, weil es mehrere Elemente zusammen versetzt. Jedes einzelnes Element ist einfach. Wenn Sie sich auf konzentrieren nur die `<a>` Element dieses Markup angezeigt:

[!code-html[Main](updating-data/samples/sample4.html)]

Hintergrundinformationen zur Funktionsweise des Rasters: das Raster zeigt die Zeilen, eine für jeden Datensatz der Datenbank, und Spalten für jedes Feld in den Datenbankdatensatz angezeigt. Während jede Zeile des Datenblatts erstellt wird, die `item` Objekt enthält die Datenbank-Datensatz (Item) für diese Zeile. Diese Anordnung bietet Ihnen eine Möglichkeit, im Code, an die Daten für diese Zeile abzurufen. Hier sehen Sie also: der Ausdruck `item.ID` nur noch den ID-Wert des aktuellen Elements der Datenbank. Sie können abrufen, die Datenbankwerte (Titel, "Genre" oder Jahr) die gleiche Weise mit `item.Title`, `item.Genre`, oder `item.Year`.

Der Ausdruck `"~/EditMovie?id=@item.ID` kombiniert den hartcodierten Teil der Ziel-URL (`~/EditMovie?id=`) dynamisch abgeleiteten ID (Sie gesehen haben die `~` Operator im vorherigen Lernprogramm; es ist ein ASP.NET-Operator, der den Stamm der aktuellen Website darstellt.)

Das Ergebnis ist, dass in diesem Teil des Markups in der Spalte etwa wie das folgende Markup einfach zur Laufzeit generiert:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Natürlich ist der tatsächliche Wert des `id` wird für jede Zeile unterschiedlich sein.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Erstellen eine benutzerdefinierte Anzeige für eine Spalte eines Datenblatts

Sichern Sie jetzt in der Spalte. Die drei Spalten mussten Sie ursprünglich in der angezeigten Raster nur Datenwerten (Title, "Genre" und Jahr). Diese Anzeige wird übergeben Sie den Namen der Datenbankspalte angegebene &mdash; z. B. `grid.Column("Title")`.

Diese neue **bearbeiten** Link-Spalte unterscheidet. Statt einen Spaltennamen angeben Sie übergeben ein `format` Parameter. Dieser Parameter ermöglicht Ihnen das Definieren von Markup, das `WebGrid` Helper wird gerendert, zusammen mit den `item` Wert, der die Spaltendaten fett oder grün angezeigt oder in jeder beliebigen, die Sie formatieren möchten. Mussten Sie den Titel in Fettschrift angezeigt werden, konnte Sie z. B. eine Spalte wie in diesem Beispiel erstellen:

[!code-html[Main](updating-data/samples/sample6.html)]

(Die verschiedenen `@` Zeichen, die in angezeigt werden, die `format` Eigenschaft markieren Sie den Übergang zwischen Markup und Code-Wert.)

Sobald Sie wissen der `format` -Eigenschaft, es ist einfacher zu verstehen, wie die neue **bearbeiten** Spalte Link zusammengestellt:

[!code-html[Main](updating-data/samples/sample7.html)]

Die Spalte besteht aus *nur* der das Markup, das den Link gerendert wird, sowie einige Informationen (ID), die extrahiert aus dem Datenbankdatensatz für die Zeile.

> [!TIP] 
> 
> **Benannte Parameter und Positionsparameter für eine Methode**
> 
> Oft, wenn Sie eine Methode aufgerufen und Parameter an sie übergeben haben, haben Sie einfach die Parameterwerte, die durch Kommas getrennt aufgeführt. Im Folgenden sind einige Beispiele angegeben:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Wir haben nicht das Problem erwähnt, wenn Sie diesen Code zuerst gesehen haben, aber in jedem Fall Sie von Parametern an die Methoden in einer bestimmten Reihenfolge Übergabe haben &mdash; , nämlich die Reihenfolge, in der die Parameter in der Methode definiert sind. Für `db.Execute` und `Validation.RequireFields`, wenn Sie die Reihenfolge der Werte Sie übergeben kombiniert, erhalten Sie eine Fehlermeldung angezeigt, wenn die Seite ausgeführt wird, oder zumindest eine gewisse unerwartete Ergebnisse. Es ist offensichtlich, müssen Sie wissen zu übergeben, die Parameter in die Reihenfolge. (In WebMatrix, kann IntelliSense Sie lernen, ermitteln Sie den Namen, den Typ und die Reihenfolge der Parameter zum.)
> 
> Als Alternative zum Übergeben von Werten in der Reihenfolge, können Sie *benannte Parameter*. (Das Übergeben von Parametern in der Reihenfolge genannt mit *Positionsparameter*.) Für benannte Parameter fügen Sie explizit den Namen des Parameters, wenn dessen Wert zu übergeben. Sie haben benannte Parameter bereits eine Anzahl von Malen in diesen Lernprogrammen verwendet. Zum Beispiel:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> und
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Benannte Parameter sind für verschiedene Situationen praktisch, insbesondere, wenn eine Methode viele Parameter annimmt. Eine ist, wenn nur ein oder zwei Parameter übergeben werden sollen, aber die Werte, die Sie übergeben möchten, nicht zwischen der ersten Position in der Parameterliste sind. Eine andere Situation ist, wenn Sie Code besser lesbar machen, indem Sie die Parameter in der Reihenfolge, die Ihnen am sinnvollsten übergeben möchten.
> 
> Natürlich müssen benannte Parameter verwenden, Sie die Namen der Parameter zu kennen. WebMatrix IntelliSense können *anzeigen* Sie die Namen, aber sie können keine derzeit auszufüllen für Sie.


## <a name="creating-the-edit-page"></a>Erstellen die Seite "Bearbeiten"

Nun können Sie erstellen die *EditMovie* Seite. Wenn der Benutzer klicken auf die **bearbeiten** Link, sie müssen am Ende auf dieser Seite.

Erstellen Sie eine Seite mit dem Namen *EditMovie.cshtml* , und Ersetzen Sie die Neuigkeiten in der Datei durch Folgendes Markup:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Dieses Markup und Code ist ähnlich, dass in der *AddMovie* Seite. Es gibt ein kleiner Unterschied in den Text für die Schaltfläche "Absenden". Wie bei der *AddMovie* Seite, gibt es eine `Html.ValidationSummary` aufrufen, die Validierungsfehler angezeigt werden, sofern vorhanden. Dieses Mal werden wir die Aufrufe eingepasst `Validation.Message`, da der Fehler in der validierungszusammenfassung angezeigt werden. Wie im vorherigen Lernprogramm erwähnt, können Sie die Zusammenfassung der Validierung und die einzelne Fehlermeldungen in verschiedenen Kombinationen verwenden.

Beachten Sie erneut, die die `method` Attribut von der `<form>` -Elementgruppe ist `post`. Wie bei der *AddMovie.cshtml* Seite dieser Seite werden Änderungen an der Datenbank. Daher sollten diese Form Ausführen einer `POST` Vorgang. (Weitere Informationen zu den Unterschieden zwischen `GET` und `POST` Vorgänge, finden Sie unter der [GET, POST und HTTP-Verb für Sicherheit](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) Randleiste in das Lernprogramm für HTML-Formularen.)

In einer früheren Lernprogramm haben Sie gesehen der `value` Attribute der Textfelder werden mit Razor-Code festgelegt wird, um sie vorab zu laden. Dieses Mal jedoch verwendeten Variablen wie `title` und `genre` für diese Aufgabe anstelle von `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Als wird vor, diese Markup der Textfeldwerte mit den Werten Film vorab zu laden. Sehen Sie in wenigen Augenblicken warum es praktisch ist, Variablen verwenden Sie dieses Mal anstatt der `Request` Objekt.

Es gibt auch eine `<input type="hidden">` Element auf dieser Seite. Dieses Element speichert die Film-ID, ohne dass es auf der Seite sichtbar. Die ID wird anfänglich übergeben zur Seite "von" mit einem Wert der Abfragezeichenfolge (`?id=7` oder ähnlich wie in der URL). Verlegen Sie den ID-Wert in ein verborgenes Feld, Sie können sicherstellen, dass es verfügbar ist. wenn das Formular gesendet wird, selbst wenn Sie nicht mehr auf die ursprüngliche URL zugreifen, die mit die Seite aufgerufen wurde.

Im Gegensatz zu den *AddMovie* Seite den Code für die *EditMovie* Seite verfügt über zwei verschiedene Funktionen. Die erste Funktion sich ergibt, wenn die Seite zum ersten Mal angezeigt wird (und *nur* dann), der Code Ruft die Film-ID aus der Abfragezeichenfolge ab. Der Code dann mithilfe der ID den entsprechenden Film aus der Datenbank lesen und Anzeigen von (vorab laden) es in die Textfelder ein.

Die zweite Funktion sich ergibt, wenn der Benutzer klickt auf die **Änderungen Absenden** -Schaltfläche im Code verfügt, liest die Werte der Textfelder, und überprüfen Sie diese. Außerdem muss der Code das Datenbankelement mit den neuen Werten zu aktualisieren. Dieses Verfahren ähnelt Hinzufügen eines Datensatzes im haben Sie gesehen *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zu einen einzigen Film lesen

Um die erste Funktion auszuführen, fügen Sie diesen Code am Anfang der Seite "ein:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Die meisten dieser Code wird in einem Block, der beginnt `if(!IsPost)`. Die `!` Operator "not" bedeutet, damit der Ausdruck bedeutet, dass *ist diese Anforderung keine Post-Übermittlung*, also eine indirekte Möglichkeit, dies zu sagen *, wenn diese Anforderung das erste Mal ist, die auf dieser Seite ausgeführt wurde*. Wie bereits erwähnt, sollte dieser Code ausgeführt *nur* der ersten Ausführung die Seite. Wenn Sie den Code in Klammern einschließen, haben nicht `if(!IsPost)`, es würde jedes Mal, wenn die Seite, gibt an, ob das erste Mal aufgerufen wird oder als Antwort auf eine Schaltfläche auf.

Beachten Sie, die den Code enthält eine `else` diesmal blockieren. Wie bereits erwähnt, wenn wir eingeführt `if` Blöcke, manchmal alternativen Code ausführen, wenn die Bedingung, die Sie testen "true" wird nicht verwendet werden sollen. Also wird die Groß-/Kleinschreibung. Wenn die Bedingung erfolgreich (d. h., wenn die ID, die an die Seite übergeben ok ist), wird eine Zeile aus der Datenbank gelesen. Jedoch, wenn die Bedingung nicht erfüllt, die `else` Block ausgeführt wird und mit dem Code wird eine Fehlermeldung angezeigt.

## <a name="validating-a-value-passed-to-the-page"></a>Einen auf der Seite "übergebenen Wert überprüfen

Der Code verwendet `Request.QueryString["id"]` beim Abrufen der ID, die an die Seite übergeben wird. Der Code stellt sicher, dass ein Wert tatsächlich für die-ID übergeben wurde Wenn kein Wert übergeben wurde, legt der Code einen Validierungsfehler.

Dieser Code zeigt eine andere Möglichkeit, Informationen zu überprüfen. Im vorherigen Lernprogramm, mit denen Sie gearbeitet der `Validation` Helper. Sie die zu überprüfenden Felder registriert ASP.NET automatisch und hat die Überprüfung Fehler angezeigt, indem `Html.ValidationMessage` und `Html.ValidationSummary`. In diesem Fall sind jedoch nicht wirklich Benutzereingaben überprüften. Stattdessen können Sie einen Wert überprüfen, der von einer anderen Stelle auf der Seite "übergeben wurde. Die `Validation` Hilfsprogramm nicht weiter, die für Sie.

Aus diesem Grund Sie den Wert selbst überprüfen, durch das Testen mit `if(!Request.QueryString["ID"].IsEmpty()`). Wenn ein Problem vorliegt, können Sie den Fehler anzeigen, indem Sie mit `Html.ValidationSummary`, wie Sie mit der `Validation` Helper. Zu diesem Zweck rufen Sie `Validation.AddFormError` und übergibt ihn dann eine anzuzeigende Meldung. `Validation.AddFormError`ist eine integrierte Methode, mit dem Sie die benutzerdefinierte Nachrichten zu definieren, die sich mit den Überprüfungssystem binden, die Sie bereits vertraut sind. (Später in diesem Lernprogramm stellen Informationen zu diesen Validierungsprozess etwas stabiler zu gestalten wir.)

Stellen Sie sicher, dass eine ID für den Film vorhanden ist, liest der Code für die Datenbank ab und sucht für nur eine einzelne Datenbank-Element. (Ihnen wahrscheinlich aufgefallen das allgemeine Muster für Datenbankvorgänge: Öffnen Sie die Datenbank, definieren Sie eine SQL-Anweisung, und führen Sie die Anweisung.) Dieses Mal SQL `Select` -Anweisung enthält `WHERE ID = @0`. Da die ID eindeutig ist, kann nur ein Datensatz zurückgegeben werden.

Die Abfrage erfolgt über `db.QuerySingle` (nicht `db.Query`, wie Sie für die Film-Auflistung verwendet), und durch den Code wird das Ergebnis in der `row` Variable. Der Name `row` ist willkürlich; Sie können die Variablen beliebig benennen. Die Variablen, die am oberen initialisiert werden dann mit den Filmdetails gefüllt, damit diese Werte in den Textfeldern angezeigt werden können.

## <a name="testing-the-edit-page-so-far"></a>Testen der bearbeiten (Seite) (bisher)

Wenn Sie die Seite testen möchten, führen Sie die *Filme* jetzt Seite, und klicken Sie auf eine **bearbeiten** neben jedem Film. Sehen Sie die *EditMovie* Seite mit den Details für den ausgewählten Film ausgefüllt:

![Bearbeiten Sie die filmdetailseite Film bearbeitet werden angezeigt](updating-data/_static/image3.png)

Beachten Sie, dass die URL der Seite etwa enthält `?id=10` (oder einer anderen Zahl). Sie haben bisher getestet, die **bearbeiten** links in der *Film* Seite Arbeit, die Seite ist die ID aus der Abfragezeichenfolge lesen, und funktioniert, dass die Datenbank abgefragt, um einen einzelnen Film-Eintrag zu erhalten.

Sie können die Informationen ändern, aber nichts geschieht, wenn Sie klicken Sie auf **Änderungen Absenden**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Hinzufügen von Code zum Aktualisieren des Films des Benutzers

In der *EditMovie.cshtml* Datei, um die zweite Funktion (Änderungen speichern) zu implementieren, fügen Sie den folgenden Code innerhalb der schließenden Klammer der der `@` Block. (Wenn Sie nicht sicher, genau, wo sich der Code platziert sind, können Sie sehen Sie sich die [vollständige codeauflistung für die bearbeiten filmdetailseite](#Complete_Page_Listing_for_EditMovie) , am Ende dieses Lernprogramms angezeigt wird.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Erneut, diese Markup und Code ähnelt der Code in *AddMovie*. Der Code befindet sich in einer `if(IsPost)` blockiert werden, da dieser Code ausgeführt wird, nur, wenn der Benutzer klickt auf die **Änderungen Absenden** Schaltfläche &mdash; , also wenn (und nur wenn) Form gebucht wurde. In diesem Fall verwenden Sie keinen Test wie `if(IsPost && Validation.IsValid())`– d. h., Sie sind nicht Kombination beide Tests mit and. Auf dieser Seite Sie zuerst bestimmen, ob es eine Formularübergabe (`if(IsPost)`), und registrieren Sie die Felder für die Überprüfung nur dann. Dann Sie die Überprüfungsergebnisse testen können (`if(Validation.IsValid()`). Der Ablauf ist etwas anders als in der *AddMovie.cshtml* Seite, aber der Effekt ist identisch.

Sie rufen Sie die Werte der Textfelder mit `Request.Form["title"]` und ähnlichen Code für die anderen `<input>` Elemente. Beachten Sie, dass der Code bei diesmal die Film-ID aus dem ausgeblendeten Feld erhält (`<input type="hidden">`). Die Seite das erste Mal ausgeführt haben, hat der Code die ID aus der Abfragezeichenfolge. Sie erhalten den Wert aus dem ausgeblendeten Feld um sicherzustellen, dass Sie die ID des Films, die ursprünglich angezeigt wurde, Information erhalten, für den Fall, dass die Abfragezeichenfolge aus irgendeinem Grund seitdem geändert wurde.

Die wirklich wichtigen Unterschied zwischen der *AddMovie* und dieser Code ist, dass in diesem Code die Verwendung von SQL `Update` -Anweisung anstelle der `Insert Into` Anweisung. Das folgende Beispiel zeigt die Syntax der SQL- `Update` Anweisung:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Sie können alle Spalten in beliebiger Reihenfolge angeben, und Sie müssen nicht unbedingt jede Spalte während der Aktualisierung einer `Update` Vorgang. (Die ID selbst kann nicht aktualisiert werden, da die, die würde den Datensatz faktisch speichern, als ein neuer Datensatz und, ist nicht zulässig für eine `Update` Vorgang.)

> [!NOTE] 
> 
> **Wichtige** der `Where` -Klausel mit der ID ist sehr wichtig, da dies ist wie die Datenbank bekannt ist, welche Datenbank aufzuzeichnen, das Sie aktualisieren möchten. Wenn Sie aufgehört haben die `Where` -Klausel, die Datenbank würde aktualisieren *jeder* Datensatz in der Datenbank. In den meisten Fällen wäre, die einem Notfall.


Im Code werden die Werte aktualisieren mithilfe von Platzhaltern für die SQL-Anweisung übergeben. Wiederholen, was wir vor dem gesagt haben: aus Sicherheitsgründen *nur* Platzhalter verwenden, um Werte an eine SQL-Anweisung übergeben.

Nachdem der Code verwendet `db.Execute` zum Ausführen der `Update` Anweisung leitet an die Angebotsseite, in dem Sie die Änderungen sehen können.

> [!TIP] 
> 
> **Andere SQL-Anweisungen, andere Methoden**
> 
> Möglicherweise haben Sie bemerkt, dass Sie andere Methoden verwenden, um verschiedene SQL-Anweisungen auszuführen. Zum Ausführen einer `Select` Abfragen gibt, möglicherweise mehrere Datensätze, die Sie verwenden die `Query` Methode. Zum Ausführen einer `Select` Abfrage, die Sie kennen nur ein Datenbankelement zurück, die Sie verwenden die `QuerySingle` Methode. Führen Sie Befehle, die Änderungen vornehmen, jedoch nicht, die Datenbankelemente zurückgibt, verwenden Sie die `Execute` Methode.
> 
> Verschiedene Methoden verfügen, da jede von ihnen unterschiedliche Ergebnisse zurückgibt, wie Sie bereits gesehen, den Unterschied zwischen haben `Query` und `QuerySingle`. (Die `Execute` Methode tatsächlich gibt einen Wert auch &mdash; , nämlich die Anzahl von Datenbankzeilen, die mit dem Befehl betroffen sind &mdash; , aber Sie haben wurde ignoriert, die bisher.)
> 
> Natürlich die `Query` Methode kann nur eine Datenbankzeile zurück. ASP.NET behandelt jedoch immer die Ergebnisse der `Query` Methode als eine Auflistung. Auch wenn die Methode nur eine Zeile zurückgibt, müssen Sie in dieser Zeile aus der Auflistung zu extrahieren. Aus diesem Grund in Situationen, in denen Sie *wissen* erhalten Sie wieder nur eine Zeile, es ist ein bequemer verwenden Bit `QuerySingle`.
> 
> Es gibt mehrere Methoden, die bestimmte Arten von Datenbankvorgängen durchgeführt. Sie finden eine Liste der datenbankmethoden in der [ASP.NET Web Pages-API-Kurzübersicht](https://go.microsoft.com/fwlink/?LinkID=202907#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Überprüfung für die ID mehr machen robuster

Beim ersten, der die Seite ausgeführt wird, erhalten Sie die Film-ID aus der Abfragezeichenfolge, sodass Sie wechseln können Film aus der Datenbank abrufen. Sie sicherstellen, dass tatsächlich ein Wert zu suchen, die Verwendung dieser Code aufgetreten:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Sie diesen Code verwendet, um sicherzustellen, dass wenn ein Benutzer, ruft der *EditMovies* Seite ohne Auswahl eines Films in der *Filme* Seite, die Seite ist eine benutzerfreundliche Fehlermeldung anzuzeigen. (Andernfalls würde Benutzer ein Fehler, der u. u. lediglich diese verwirren würde angezeigt.)

Diese Überprüfung ist jedoch nicht sehr robuste. Die Seite kann auch mit diesen Fehlern aufgerufen werden:

- Die ID ist keine Zahl. Beispielsweise könnte die Seite aufgerufen werden, mit einer URL wie `http://localhost:nnnnn/EditMovie?id=abc`.
- Die ID ist eine Zahl, aber er verweist auf einen Film, die nicht vorhanden ist (z. B. `http://localhost:nnnnn/EditMovie?id=100934`).

Wenn Sie wissen möchten, die Fehler angezeigt, die aus dieser URLs, führen Sie die *Filme* Seite. Wählen Sie einen Film zu bearbeiten, und ändern Sie dann die URL des der *EditMovie* Seite zu einer URL, die ein alphabetisches enthält, oder -ID für eine nicht existierende Film.

Daher sollten Sie vorgehen? Um sicherzustellen, dass nicht nur auf der Seite "eine ID übergeben wird, aber die ID eine ganze Zahl ist, ist die erste Lösung. Ändern Sie den Code für die `!IsPost` Test in diesem Beispiel aussehen:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Sie hinzugefügt haben, eine zweite Bedingung, die `IsEmpty` Test, verknüpft mit `&&` (logisches AND):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Sie möglicherweise wissen, aus der [Einführung in ASP.NET Web Pages Programmierung](../introducing-razor-syntax-c.md) Lernprogramm, in dem Methoden wie `AsBool` eine `AsInt` eine Zeichenfolge in einen anderen Datentyp zu konvertieren. Die `IsInt` Methode (und andere, z. B. `IsBool` und `IsDateTime`) sind ähnlich. Sie testen allerdings nur, ob Sie *können* konvertieren die Zeichenfolge ohne die Konvertierung ausführen. Hier haben wir also Sie sind im Wesentlichen besagt *, wenn der Wert der Abfragezeichenfolge in eine ganze Zahl konvertiert werden kann...* .

Das andere mögliche Problem aufgetreten ist für einen Film suchen, die nicht vorhanden ist. Der Code, der einen Film sieht wie folgt diesem Code:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Wenn Sie übergeben ein `movieId` -Wert an die `QuerySingle` -Methode, die einen tatsächlichen Film nicht entsprechen, keine zurückgegeben wird und die Anweisungen folgen (z. B. `title=row.Title`) zu Fehlern führen.

Es ist eine einfache Lösung. Wenn die `db.QuerySingle` Methodenrückgabe keine Ergebnisse zurückgegeben, die `row` Variable wird null sein. Damit Sie überprüfen können, ob die `row` Variable null ist, bevor Sie versuchen, Werte aus ihm abzurufen. Der folgende Code Fügt eine `if` -Block um die Anweisungen, die die Werte von erhalten die `row` Objekt:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Mit diesen zwei zusätzliche Validierungstests wird die Seite mehr absolut. Der vollständige Code für die `!IsPost` Verzweigung sieht wie Folgendes Beispiel:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Wir müssen einmal Beachten Sie, dass diese Aufgabe eine gute Verwendung für ist ein `else` Block. Wenn die Tests nicht erfolgreich sind, die `else` Fehlermeldungen festgelegt werden.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Beim Hinzufügen einer Verknüpfung auf der Seite "Filme" zurückgegeben.

Ein Detail endgültigen und hilfreich ist, einen Link hinzuzufügen zurück an die *Filme* Seite. In den normalen Fluss von Ereignissen, Benutzer am Starten der *Filme* Seite, und klicken Sie auf eine **bearbeiten** Link. Ermöglicht, die sie mit der *EditMovie* Seite, auf dem den Film bearbeiten und klicken Sie auf die Schaltfläche "". Nachdem die Änderung des Codes verarbeitet hat, leitet Sie an der *Filme* Seite.

Jedoch:

- Der Benutzer ggf. nicht ändert nichts.
- Der Benutzer kann auf dieser Seite gelangt haben, ohne Klicken auf eine **bearbeiten** wiederherstellungsverknüpfung in der *Filme* Seite.

In beiden Fällen möchten Sie sie auf die Auflistung der wichtigsten zurückzugebenden erleichtern. Es ist eine einfache Lösung &mdash; fügen Sie das folgende Markup direkt hinter das abschließende `</form>` Tag im Markup:

[!code-html[Main](updating-data/samples/sample19.html)]

Dieses Markup verwendet dieselbe Syntax für eine `<a>` -Element, das Sie an anderer Stelle gesehen haben. Die URL enthält `~` bedeutet "Root der Website".

## <a name="testing-the-movie-update-process"></a>Testen des Prozesses der Film-Update

Sie können jetzt testen. Führen Sie die *Filme* Seite, und klicken Sie auf **bearbeiten** neben einen Film. Wenn die *EditMovie* Seite angezeigt wird, nehmen Sie Änderungen an der Film und auf **Änderungen Absenden**. Wenn die Film-Liste angezeigt wird, stellen Sie sicher, dass die Änderungen angezeigt werden.

Um sicherzustellen, dass die Validierung funktioniert, klicken Sie auf **bearbeiten** für einen anderen Film. Wenn erhalten Sie die *EditMovie* Seite löschen der **"Genre"** Feld (oder **Jahr** Feld oder beides), und wiederholen Sie die Änderungen zu übermitteln. Sie sehen einen Fehler, wie zu erwarten:

![Bearbeiten Sie die filmdetailseite mit Validierungsfehlern](updating-data/_static/image4.png)

Klicken Sie auf die **zurück zum Film Angebot** Link, um Ihre Änderungen verwerfen und zum Zurückgeben der *Filme* Seite.

## <a name="coming-up-next"></a>Als Nächstes kommen

In den nächsten Lernprogrammen sehen Sie, wie einen Movie-Datensatz gelöscht.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Vollständige Auflistung für Filmdetailseite (inklusive Bearbeitungslinks)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Seite "-Angebot für Film Bearbeitungsseite fertig

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL-UPDATE-Anweisung](http://www.w3schools.com/sql/sql_update.asp) auf der Website W3Schools

>[!div class="step-by-step"]
[Zurück](entering-data.md)
[Weiter](deleting-data.md)
