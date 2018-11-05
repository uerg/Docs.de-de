---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 'Einführung in ASP.NET Web Pages: Aktualisieren von DatenbankenDaten | Microsoft-Dokumentation'
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie (ändern) einer vorhandenen Datenbank-Eintrag aktualisiert, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird vorausgesetzt, Sie haben die Reihe te...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 206d7e209857aceb3eb92c2405bb73f7ff7dbaeb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021429"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Einführung in ASP.NET Web Pages: Aktualisieren von DatenbankenDaten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie (ändern) einer vorhandenen Datenbank-Eintrag aktualisiert, bei der Verwendung von ASP.NET Web Pages (Razor). Es wird vorausgesetzt, Sie haben die Reihe über [eingeben von Daten von mithilfe von Formularen mithilfe von ASP.NET Web Pages](entering-data.md).
> 
> Sie lernen Folgendes:
> 
> - Gewusst wie: Wählen Sie einen einzelnen Datensatz in die `WebGrid` Helper.
> - So lesen Sie einen einzelnen Datensatz aus einer Datenbank.
> - Wie Sie ein Formular mit Werten aus den Datenbank-Datensatz vorab zu laden.
> - Wie Sie einen vorhandenen Datensatz in einer Datenbank zu aktualisieren.
> - Informationen zum Speichern von Informationen auf der Seite, ohne es anzuzeigen.
> - So verwenden Sie ein ausgeblendetes Feld zum Speichern von Informationen.
>   
> 
> Features/Technologien erläutert:
> 
> - Die `WebGrid` Helper.
> - Die SQL-Anweisung `Update` Befehl.
> - Die `Database.Execute` -Methode.
> - Ausgeblendete Felder (`<input type="hidden">`).


## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie gelernt, wie eine Datenbank einen Datensatz hinzufügen. Hier erfahren Sie, wie einen Datensatz für die Bearbeitung angezeigt wird. In der *Filme* Seite aktualisieren Sie die `WebGrid` Helper, sodass, dass die It anzeigt eine **bearbeiten** neben jeder Film:

![WebGrid anzuzeigen, einschließlich einen Link "Bearbeiten" für jeden Film](updating-data/_static/image1.png)

Beim Klicken auf die **bearbeiten** Link gelangen Sie zu einer anderen Seite befindet, in dem die Filminformationen bereits in einem Formular:

![Bearbeiten von Film-Seite, die mit der Film bearbeitet werden](updating-data/_static/image2.png)

Sie können einen der Werte ändern. Wenn Sie die Änderungen übermitteln, wird der Code auf der Seite aktualisiert die Datenbank, und gelangen Sie zurück auf die Movie-Auflistung.

Dieser Teil des Prozesses funktioniert fast genau so wie die *AddMovie.cshtml* Seite, die Sie im vorherigen Tutorial erstellt haben, damit viele der in diesem Tutorial vertraut werden.

Es gibt mehrere Möglichkeiten, die Sie eine Möglichkeit zum Bearbeiten eines einzelnen Films implementieren könnten. Der Ansatz gezeigt wurde gewählt, da es sich um einfach zu implementieren und leicht verständlich ist.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Die Movie-Auflistung hinzugefügt einen Link "Bearbeiten"

Um zu beginnen, führen Sie die Aktualisierung der *Filme* Seite, damit enthält jeder Film wird ebenfalls eine **bearbeiten** Link.

Öffnen der *Movies.cshtml* Datei.

Ändern Sie in den Hauptteil der Seite, die `WebGrid` Markup durch Hinzufügen einer Spalte. Dies ist die geänderte Markup:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Die neue Spalte, ist diese:

[!code-html[Main](updating-data/samples/sample2.html)]

Diese Spalte ist einen Link angezeigt (`<a>` Element), dessen Text lautet "Bearbeiten". Was wir wollen Sie eine Verknüpfung zu erstellen, die wie folgt aussieht, wenn die Seite ausgeführt wird, werden mit der `id` Wert für jeden Film:

[!code-css[Main](updating-data/samples/sample3.css)]

Dieser Link wird aufgerufen, eine Seite namens *EditMovie*, und es wird die Abfragezeichenfolge übergeben `?id=7` zu dieser Seite.

Die Syntax für die neue Spalte sieht ein wenig komplexer; Dies ist nur, weil es zusammengefügt mehrere Elemente sind. Jedes einzelnes Element ist einfach. Wenn Sie sich auf konzentrieren lediglich die `<a>` Element dieses Markup angezeigt:

[!code-html[Main](updating-data/samples/sample4.html)]

Einige Hintergrundinformationen zur Funktionsweise des Rasters: das Raster zeigt Zeilen an, eine für jeden Datensatz der Datenbank, und Spalten für jedes Feld in der Datenbankdatensatz zeigt. Während jede Zeile erstellt wird, die `item` Objekt enthält die Datenbank-Datensatz (Item) für diese Zeile. Diese Anordnung bietet Ihnen eine Möglichkeit, im Code, um die Daten für diese Zeile abzurufen. Sie sehen hier ist: der Ausdruck `item.ID` ist die ID-Wert, der das aktuelle Datenbankelement abrufen. Sie erhalten möglicherweise eines der Datenbankwerte (Title, "Genre" oder Jahr) die gleiche Weise mit `item.Title`, `item.Genre`, oder `item.Year`.

Der Ausdruck `"~/EditMovie?id=@item.ID` kombiniert den fest programmierte Teil der Ziel-URL (`~/EditMovie?id=`) dynamisch abgeleiteten ID (Sie haben gesehen, die `~` Operator im vorherigen Tutorial; es ist ein ASP.NET-Operator, der das Stammverzeichnis der aktuellen Website darstellt.)

Das Ergebnis ist, dass dieser Teil des Markups in der Spalte etwa wie das folgende Markup einfach zur Laufzeit generiert:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Natürlich ist der tatsächliche Wert der `id` wird für jede Zeile unterschiedlich sein.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Erstellen eine benutzerdefinierte Anzeige für eine Rasterspalte

Doch nun zurück zu der Spalte. Die drei Spalten mussten Sie ursprünglich der angezeigten Raster nur Datenwerte (Title, "Genre" und Jahr). Diese Anzeige durch Übergeben des Namens der Datenbankspalte angegebene &mdash; z. B. `grid.Column("Title")`.

Diese neue **bearbeiten** Link Spalte unterscheidet. Übergeben Sie einen Spaltennamen an, anstatt eine `format` Parameter. Mit diesem Parameter können Sie das Markup zu definieren, die die `WebGrid` Helper wird gerendert, zusammen mit den `item` Werts, der die Spaltendaten fett oder grün angezeigt oder in den Inhalt, den Sie formatieren möchten. Wenn den Titel gewünscht in fett angezeigt werden, konnte Sie z. B. eine Spalte, wie im folgenden Beispiel erstellen:

[!code-html[Main](updating-data/samples/sample6.html)]

(Die verschiedenen `@` Zeichen, die in angezeigt werden, die `format` Eigenschaft markieren den Übergang zwischen Markup und ein Code-Wert.)

Sobald Sie kennen die `format` -Eigenschaft, es ist einfacher zu verstehen wie das neue **bearbeiten** Spalte Link zusammengestellt werden:

[!code-html[Main](updating-data/samples/sample7.html)]

Die Spalte besteht aus *nur* der das Markup, das den Link gerendert wird, sowie einige Informationen (ID), extrahiert aus der Datenbank-Datensatz für die Zeile.

> [!TIP]
> 
> **Benannte Parameter und Positionsparameter für eine Methode**
> 
> Oft Wenn Sie eine Methode aufgerufen wird und die entsprechenden Parameter übergeben haben, haben Sie einfach die Parameterwerte, die durch Kommas getrennt aufgeführt. Im Folgenden sind einige Beispiele angegeben:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Wir können das Problem nicht erwähnt, wenn Sie diesen Code zuerst gesehen haben, aber in jedem Fall Sie Parameter an die Methoden in einer bestimmten Reihenfolge übergeben &mdash; , nämlich die Reihenfolge, in der die Parameter in dieser Methode definiert sind. Für `db.Execute` und `Validation.RequireFields`, wenn Sie sich die Reihenfolge der Werte Sie übergeben kombiniert, erhalten Sie eine Fehlermeldung angezeigt, wenn die Seite ausgeführt wird, oder zumindest eine gewisse seltsame Ergebnisse. Natürlich müssen Sie die Reihenfolge, übergeben die Parameter in kennen. (In WebMatrix IntelliSense Ermitteln der Namen, Typ und Reihenfolge der Parameter Informationen helfen Ihnen.)
> 
> Als Alternative zum Übergeben von Werten in der Reihenfolge, können Sie *benannte Parameter*. (Übergeben von Parametern in der Reihenfolge genannt mit *Positionsparameter*.) Für benannte Parameter schließen Sie explizit den Namen des Parameters, wenn den Wert zu übergeben. Sie haben benannte Parameter bereits eine Anzahl von Malen in diesen Tutorials verwendet. Zum Beispiel:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> und
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Benannte Parameter sind für eine Reihe von Situationen nützlich, besonders, wenn eine Methode für viele Parameter. Eines ist, wenn nur ein oder zwei Parameter übergeben werden sollen, aber die Werte, die Sie übergeben möchten, nicht für die erste Position in der Parameterliste sind. Eine andere Situation ist, wenn Sie möchten Ihren Code besser lesbar machen, indem Sie die Parameter zu übergeben, in der Reihenfolge, die für Sie am sinnvollsten ist.
> 
> Wenn benannte Parameter verwenden möchten, müssen Sie natürlich die Namen der Parameter zu kennen. WebMatrix IntelliSense können *anzeigen* Sie die Namen, aber es kann nicht aktuell Geben sie für Sie.


## <a name="creating-the-edit-page"></a>Erstellen die Seite "Bearbeiten"

Nun können Sie erstellen die *EditMovie* Seite. Wenn der Benutzer klicken auf die **bearbeiten** Link sie zum Schluss werden auf dieser Seite.

Erstellen Sie eine Seite mit dem Namen *EditMovie.cshtml* , und Ersetzen Sie die neuerungen in der Datei mit folgendem Markup:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Dieses Markup und Code entspricht, was Sie haben der *AddMovie* Seite. Es gibt ein kleinen Unterschied in den Text für die Schaltfläche "Senden" aus. Wie bei der *AddMovie* Seite, gibt es eine `Html.ValidationSummary` Aufruf, der Validierungsfehler angezeigt wird, sofern vorhanden. Diesmal haben wir die Aufrufe von auslassen `Validation.Message`, da Fehler in der Zusammenfassung der Validierung angezeigt werden. Wie im vorherigen Tutorial erwähnt, können Sie die Zusammenfassung der Validierung und die einzelne Fehlermeldungen in verschiedenen Kombinationen verwenden.

Beachten Sie erneut, dass der `method` Attribut der `<form>` Element nastaven NA hodnotu `post`. Wie bei der *AddMovie.cshtml* Seite dieser Seite werden Änderungen an der Datenbank. Daher sollten diese Form Ausführen einer `POST` Vorgang. (Weitere Informationen zu den Unterschieden zwischen `GET` und `POST` Vorgänge, finden Sie unter den [GET, POST und HTTP-Verb Sicherheit](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) Randleiste in diesem Tutorial auf HTML-Formularen.)

In einem früheren Tutorial haben Sie gesehen die `value` Attribute der Textfelder werden mit Razor-Code festgelegt wird, um sie vorab zu laden. Dieses Mal jedoch, Sie verwenden Variablen wie `title` und `genre` für diese Aufgabe anstelle von `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Wie wird vor, dieses Markup Werte des Text mit den Werten der Film vorab zu laden. Sehen Sie in einen Moment, warum es nützlich ist, die Variablen dieses Mal verwenden, anstatt, die `Request` Objekt.

Es gibt auch eine `<input type="hidden">` Element auf dieser Seite. Dieses Element werden die Film-ID gespeichert, ohne dass sie auf der Seite angezeigt. Die ID wird anfangs auf der Seite mit übergeben Wert einer Abfragezeichenfolge (`?id=7` oder in der URL ähnlich). Indem Sie den ID-Wert in ein verborgenes Feld einfügen, Sie können sicherstellen, dass es verfügbar ist. wenn das Formular übermittelt wird, auch wenn Sie nicht mehr auf die ursprüngliche URL zugreifen, die mit die Seite aufgerufen wurde.

Im Gegensatz zu den *AddMovie* Seite, den Code für die *EditMovie* Seite verfügt über zwei verschiedene Funktionen. Die erste Funktion ist, die, wenn die Seite zum ersten Mal angezeigt wird (und *nur* dann), der Code Ruft die Film-ID aus der Abfragezeichenfolge ab. Der Code nutzt dann die ID den entsprechenden Film aus der Datenbank lesen und anzeigen (vorab zu laden) es in die Textfelder ein.

Die zweite Funktion ist, die klickt der Benutzer die **Änderungen Absenden** Schaltfläche, der Code verfügt lesen die Werte der Textfelder, und überprüfen. Der Code hat auch den Datenbank-Artikel mit den neuen Werten aktualisiert. Diese Technik ist vergleichbar mit dem Hinzufügen eines Eintrags, wie in *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zum Lesen eines einzelnen Films

Um die erste Funktion auszuführen, fügen Sie folgenden Code am Anfang der Seite:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Der größte Teil dieses Codes wird in einem Block, der beginnt `if(!IsPost)`. Die `!` Operator "not", damit der Ausdruck bedeutet, dass bedeutet *ist diese Anforderung keiner Post-Übermittlung*, d.h. ein indirektes Verfahren zu sagen *Wenn die Anforderung beim ersten ist, die auf dieser Seite ausgeführt wurde*. Wie bereits erwähnt, sollte dieser Code ausgeführt *nur* ersten Mal die Seite ausgeführt wird. Wenn Sie nicht, dass den Code in einschließen `if(!IsPost)`, sie würden jedes Mal, wenn die Seite, gibt an, ob das erste Mal aufgerufen wird oder als Reaktion auf eine Schaltfläche auf.

Beachten Sie, die der Code enthält ein `else` diesmal blockieren. Wie bereits erwähnt, wenn wir eingeführt `if` Blöcke, manchmal alternativen Code ausgeführt wird, wenn die Bedingung, die Sie testen nicht "true" ist werden soll. Dies ist hier der Fall. Wenn die Bedingung erfüllt (d.h., wenn die ID, die an die Seite in Ordnung ist), können Sie eine Zeile aus der Datenbank gelesen. Jedoch, wenn die Bedingung nicht erfolgreich abgeschlossen, die `else` Block ausgeführt wird und im Code wird eine Fehlermeldung angezeigt.

## <a name="validating-a-value-passed-to-the-page"></a>Überprüfen einen Wert an die Seite übergeben

Der Code verwendet `Request.QueryString["id"]` zum Abrufen der ID, die die Seite übergeben werden. Der Code wird sichergestellt, der tatsächlich übergeben eines Werts für die ID. Wenn kein Wert übergeben wurde, legt der Code einen Validierungsfehler.

Dieser Code zeigt eine andere Möglichkeit, Informationen zu überprüfen. Im vorherigen Tutorial, mit denen Sie gearbeitet der `Validation` Helper. Sie registriert die zu überprüfenden Felder ASP.NET automatisch und die Überprüfung wurde mit Fehlern angezeigt `Html.ValidationMessage` und `Html.ValidationSummary`. In diesem Fall sind jedoch Sie nicht wirklich Benutzereingaben werden überprüft. Stattdessen können Sie einen Wert überprüfen, der von einer anderen Stelle auf der Seite übergeben wurde. Die `Validation` Hilfsprogramm nicht, die für Sie.

Aus diesem Grund Sie den Wert selbst überprüfen, testen Sie es mit `if(!Request.QueryString["ID"].IsEmpty()`). Wenn ein Problem vorliegt, können Sie den Fehler anzeigen, mit `Html.ValidationSummary`, wie Sie mit der `Validation` Helper. Zu diesem Zweck rufen Sie `Validation.AddFormError` , und übergeben sie eine anzuzeigende Meldung. `Validation.AddFormError` ist eine integrierte Methode, mit dem Sie benutzerdefinierte Meldungen zu definieren, die sich das Überprüfungssystem zu verknüpfen, die Sie bereits kennen. (Später in diesem Lernprogramm befassen wir uns dazu, wie Sie diesen Validierungsprozess etwas stabiler machen mit.)

Stellen Sie sicher, dass eine für den Film-ID vorhanden ist, liest der Code für die Datenbank ab und sucht für nur eine einzeldatenbank-Element. (Das allgemeine Muster für Datenbankvorgänge haben wahrscheinlich bemerkt: Öffnen Sie die Datenbank, eine SQL­Anweisung definieren, und führen Sie die Anweisung.) Dieses Mal SQL `Select` -Anweisung schließt `WHERE ID = @0`. Da die ID eindeutig ist, kann nur ein Datensatz zurückgegeben werden.

Die Abfrage erfolgt über `db.QuerySingle` (nicht `db.Query`, da Sie für die Movie-Auflistung verwendet), und der Code legt das Ergebnis in der `row` Variable. Der Name `row` ist ein beliebiger; Sie können die Variablen name ist beliebig. Die Variablen initialisiert, die am Anfang werden dann mit den Filmdetails gefüllt, damit diese Werte in den Textfeldern angezeigt werden können.

## <a name="testing-the-edit-page-so-far"></a>Testen die Seite "Bearbeiten" (bisher)

Wenn Sie die Seite testen möchten, führen Sie die *Filme* Seite jetzt, und klicken Sie auf eine **bearbeiten** neben jedem Film. Sehen Sie die *EditMovie* Seite mit den Details für den ausgewählten Film ausgefüllt:

![Bearbeiten von Film-Seite, die mit der Film bearbeitet werden](updating-data/_static/image3.png)

Beachten Sie, dass die URL der Seite etwa enthält `?id=10` (oder eine andere Nummer). Bisher haben Sie getestet, die **bearbeiten** verknüpft die *Film* Seite arbeiten, die Seite wird die ID aus der Abfragezeichenfolge gelesen, und, dass die Datenbank eine zum Abrufen eines einzelnen Films Datensatzes Abfrage funktioniert.

Sie können die Filminformationen ändern, aber die nichts geschieht, wenn Sie auf **Änderungen Absenden**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Hinzufügen von Code zum Aktualisieren des Films mit die Änderungen des Benutzers

In der *EditMovie.cshtml* Datei, um die zweite Funktion (Änderungen speichern) zu implementieren, fügen Sie folgenden Code innerhalb der schließenden Klammer der der `@` Block. (Wenn Sie nicht sicher, genau, wo sich der Code platziert sind, sehen Sie sich die [vollständige codeauflistung für die Seite bearbeiten Film](#Complete_Page_Listing_for_EditMovie) , das am Ende dieses Tutorials angezeigt wird.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

In diesem Fall dieses Markup und Code ähnelt dem Code in *AddMovie*. Der Code befindet sich in einem `if(IsPost)` blockiert werden, da dieser Code ausgeführt wird, nur, wenn der Benutzer klickt der **Änderungen Absenden** Schaltfläche &mdash; , also wenn (und nur wenn) das Formular gesendet wurde. In diesem Fall verwenden Sie keinen Test wie `if(IsPost && Validation.IsValid())`– d. h., Sie sind nicht kombinieren von beide Tests unter Verwendung and. Auf dieser Seite Sie zuerst ermitteln, ob es eine formularübertragung (`if(IsPost)`), und nur die Felder für die Überprüfung registrieren. Und dann die Ergebnisse, die Sie testen (`if(Validation.IsValid()`). Der Flow ist etwas anders als in der *AddMovie.cshtml* Seite, jedoch der Effekt ist identisch.

Erhalten Sie die Werte der Textfelder mit `Request.Form["title"]` und ähnlichen Code für die anderen `<input>` Elemente. Beachten Sie, dass dieses Mal mit dem Code die Film-ID aus dem ausgeblendeten Feld abgerufen (`<input type="hidden">`). Wenn die Seite erstmalig ausgeführt haben, hier der Code die ID aus der Abfragezeichenfolge an. Sie erhalten den Wert aus dem ausgeblendeten Feld, um sicherzustellen, dass Sie die ID des Films, die ursprünglich angezeigt wurde, optimal nutzen, für den Fall, dass die Abfragezeichenfolge irgendwie seither geändert wurde.

Der wirklich wichtige Unterschied zwischen der *AddMovie* und dieser Code ist, dass in diesem Code die Verwendung von SQL `Update` -Anweisung anstelle der `Insert Into` Anweisung. Das folgende Beispiel zeigt die Syntax des SQL- `Update` Anweisung:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Sie können alle Spalten in einer beliebigen Reihenfolge angeben und Sie müssen nicht unbedingt jede Spalte beim Aktualisieren einer `Update` Vorgang. (Die ID selbst kann nicht aktualisiert werden, da, die den Datensatz in Kraft, als einen neuen Datensatz speichern würde und, nicht für zulässig ist einen `Update` Vorgang.)

> [!NOTE] 
> 
> **Wichtige** der `Where` -Klausel mit der ID ist sehr wichtig, denn das ist wie die Datenbank bekannt ist, welche Datenbank Datensätze aktualisiert werden soll. Wenn Sie aufgehört haben vorher die `Where` -Klausel, die Datenbank wird aktualisiert *jeder* Datensatz in der Datenbank. In den meisten Fällen wäre, die einem Notfall.


Im Code werden die Werte aktualisieren für die SQL-Anweisung übergeben, mit Platzhaltern. Wiederholen, was wir erwähnt habe: aus Sicherheitsgründen *nur* Platzhalter verwenden, um Werte an eine SQL-Anweisung übergeben.

Nachdem der Code verwendet `db.Execute` zum Ausführen der `Update` -Anweisung, die es wieder auf die Angebotsseite, hier Sie die Änderungen sehen leitet.

> [!TIP] 
> 
> **Andere SQL-Anweisungen, verschiedene Methoden**
> 
> Möglicherweise haben Sie bemerkt, dass Sie andere Methoden verwenden, um verschiedene SQL-Anweisungen auszuführen. Zum Ausführen einer `Select` Abfragen gibt, die ein potenzielles mehrere Datensätze, die Sie verwenden die `Query` Methode. Zum Ausführen einer `Select` Abfrage, die Sie kennen nur ein Datenbankelement zurück, die Sie verwenden die `QuerySingle` Methode. Führen Sie Befehle, die Änderungen vornehmen, jedoch nicht, die Datenbankelemente zurückgeben, die Sie verwenden die `Execute` Methode.
> 
> Sie müssen verschiedene Methoden haben, da jeweils unterschiedliche Ergebnisse zurückgibt, wie Sie bereits gesehen, den Unterschied zwischen haben `Query` und `QuerySingle`. (Die `Execute` Methode eigentlich gibt einen Wert auch &mdash; , nämlich die Anzahl von Datenbankzeilen, die mit dem Befehl betroffen waren &mdash; , aber Sie haben wurde ignoriert, die bisher.)
> 
> Natürlich die `Query` Methode kann nur eine Datenbankzeile zurück. ASP.NET behandelt, jedoch immer die Ergebnisse der `Query` Methode als eine Auflistung. Auch wenn die Methode nur eine Zeile zurückgibt, müssen Sie in dieser Zeile aus der Auflistung zu extrahieren. Aus diesem Grund in Situationen, in dem Sie *wissen* erhalten Sie einige nur eine Zeile ist dies ein angenehmer zu wenig `QuerySingle`.
> 
> Es gibt einigen andere Methoden, die bestimmte Typen von Datenbankvorgänge ausführen. Sie finden eine Liste der datenbankmethoden in der [ASP.NET Web Pages-API-Kurzübersicht](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Überprüfung für die ID mehr machen robuster

Beim ersten, die die Seite ausgeführt wird, erhalten Sie die Film-ID aus der Abfragezeichenfolge, damit Sie zugreifen können, Film aus der Datenbank abzurufen. Sie haben sichergestellt, dass es sich bei gab es ein Wert zu suchen, die Verwendung dieser Code tatsächlich:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Sie diesen Code verwendet, um sicherzustellen, dass wenn ein Benutzer auf die *EditMovies* Seite, ohne zuvor einen Film in den *Filme* Seite die Seite würde eine benutzerfreundliche Fehlermeldung angezeigt. (Andernfalls würde Benutzer eine Fehlermeldung, die wahrscheinlich nur diese verwechselt wird angezeigt.)

Diese Überprüfung ist jedoch nicht sehr zuverlässig. Die Seite kann auch mit diesen Fehlern aufgerufen werden:

- Die ID ist keine Zahl. Beispielsweise könnte die Seite aufgerufen werden, mit einer URL wie `http://localhost:nnnnn/EditMovie?id=abc`.
- Die ID ist eine Zahl, aber sie verweist, auf einen Film, die nicht vorhanden (z. B. `http://localhost:nnnnn/EditMovie?id=100934`).

Wenn Sie wissen möchten, die Fehler angezeigt, die aus diesen URLs, führen Sie die *Filme* Seite. Wählen Sie einen Film zu bearbeiten, und ändern Sie die URL der *EditMovie* Seite zu einer URL, die ein alphabetisches enthält, ID oder die ID eines Films nicht vorhanden.

Daher sollten Sie vorgehen? Die erste Lösung besteht darin sicherzustellen, dass, dass nicht nur eine ID auf der Seite übergeben wird, aber die ID, eine ganze Zahl ist. Ändern Sie den Code für die `!IsPost` Test aus, um wie im folgenden Beispiel aussehen:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Sie hinzugefügt haben, eine zweite Bedingung, die `IsEmpty` Test ausgeführt wird, verknüpft mit `&&` (logisches AND):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Erinnern Sie sich über die [Einführung in die Programmierung von ASP.NET Web Pages](../introducing-razor-syntax-c.md) Tutorial, in dem Methoden wie `AsBool` ein `AsInt` konvertieren eine Zeichenfolge in einen anderen Datentyp. Die `IsInt` Methode (und andere wie `IsBool` und `IsDateTime`) sind ähnlich. Sie testen allerdings nur, ob Sie *können* konvertieren die Zeichenfolge ohne die Konvertierung ausführen. Hier können Sie im Wesentlichen besagt *Wenn Abfragezeichenfolgen-Werts in eine ganze Zahl konvertiert werden kann...* .

Das andere mögliche Problem aufgetreten ist nach einem Film suchen, die nicht vorhanden. Der Code zum Abrufen eines Films sieht wie im folgenden Code:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Übergeben einer `movieId` Wert der `QuerySingle` -Methode, die keinen tatsächlichen Film entsprechen nicht, wird nichts zurückgegeben und die Anweisungen folgen (z. B. `title=row.Title`) zu Fehlern führen.

Es ist leicht zu beheben. Wenn die `db.QuerySingle` Methode keine Ergebnisse zurückgibt, die `row` Variable wird null sein. Damit Sie überprüfen können, ob die `row` Variable null ist, bevor Sie versuchen, die von ihr Werte zu erhalten. Der folgende Code Fügt eine `if` -Block um die Anweisungen, die die Werte von Abrufen der `row` Objekt:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Zwei zusätzliche Validierung ausführen wird die Seite mehr hundertprozentig. Der vollständige Code für die `!IsPost` Branch sieht nun wie im folgenden Beispiel:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Wir werden noch einmal Beachten Sie, dass diese Aufgabe eine gute Verwendung für ist ein `else` Block. Wenn die Tests übergeben, die `else` Standardresultset Fehlermeldungen.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Hinzufügen eines Links auf der Seite "Movies" zurückgegeben.

Ein Detail endgültig und hilfreich ist, fügen einen Link zurück an die *Filme* Seite. In der normale Fluss von Ereignissen, Benutzer beginnt mit der *Filme* Seite, und klicken Sie auf eine **bearbeiten** Link. Somit kommen sie zu der *EditMovie* Seite, in dem Bearbeiten des Films und klicken Sie auf die Schaltfläche. Nachdem der Code die Änderung verarbeitet hat, leitet Sie an der *Filme* Seite.

Dabei gilt jedoch Folgendes:

- Benutzer können keine Änderungen vornehmen.
- Der Benutzer kann zu dieser Seite erhalten haben, ohne Klicken auf eine **bearbeiten** -link in der *Filme* Seite.

In beiden Fällen möchten Sie sie auf die Auflistung der wichtigsten zurückzugebenden erleichtern. Es ist eine einfache Lösung &mdash; fügen Sie das folgende Markup direkt nach dem schließenden `</form>` Tags im Markup:

[!code-html[Main](updating-data/samples/sample19.html)]

Dieses Markup verwendet die gleiche Syntax wie für eine `<a>` -Element, das Sie an anderer Stelle gesehen haben. Die URL enthält `~` bedeutet "Root der Website".

## <a name="testing-the-movie-update-process"></a>Testen des Prozesses der Movie-Update

Jetzt können Sie testen. Führen Sie die *Filme* Seite, und klicken Sie auf **bearbeiten** neben einen Film. Wenn die *EditMovie* Seite angezeigt wird, nehmen Sie Änderungen an den Film und auf **Änderungen Absenden**. Wenn die Movie-Liste angezeigt wird, stellen Sie sicher, dass Ihre Änderungen angezeigt werden.

Um sicherzustellen, dass die Validierung funktioniert, klicken Sie auf **bearbeiten** für einen anderen Film. Wenn man an die *EditMovie* Deaktivieren der **"Genre"** Feld (oder **Jahr** Feld oder beides), und versuchen, Ihre Änderungen zu übernehmen. Einen Fehler angezeigt, wenn Sie erwarten:

![Bearbeiten Sie (Seite) Film Validierungsfehler anzeigt](updating-data/_static/image4.png)

Klicken Sie auf die **zurück zum Movie-Angebot** Link, um Ihre Änderungen verwerfen und zurück zu den *Filme* Seite.

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial sehen Sie, wie Sie einen Film-Datensatz zu löschen.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Vollständige Liste für Movie-Seite (mit Bearbeitungslinks aktualisiert)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Führen Sie die Seite Listeneintrag Movie-Seite "Bearbeiten"

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](../../getting-started/introducing-razor-syntax-c.md)
- [SQL-UPDATE-Anweisung](http://www.w3schools.com/sql/sql_update.asp) auf der Website W3Schools

> [!div class="step-by-step"]
> [Zurück](entering-data.md)
> [Weiter](deleting-data.md)
