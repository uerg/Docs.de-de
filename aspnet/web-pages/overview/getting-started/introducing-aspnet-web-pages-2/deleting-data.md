---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Einführung in ASP.NET Web Pages: Löschen von DatenbankenDaten | Microsoft-Dokumentation'
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie einen Eintrag für die einzelnen Datenbank zu löschen. Es wird davon ausgegangen, dass Sie der Reihe über das Aktualisieren von Datenbank-Daten in ASP.NET Web-PA abgeschlossen haben...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020441"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Einführung in ASP.NET Web Pages: Löschen von DatenbankenDaten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie Sie einen Eintrag für die einzelnen Datenbank zu löschen. Es wird vorausgesetzt, Sie haben die Reihe über [Aktualisieren von Daten in ASP.NET Web Pages](updating-data.md).
> 
> Sie lernen Folgendes:
> 
> - So wählen Sie einen einzelnen Datensatz in einer Auflistung der Datensätze.
> - Vorgehensweise: Löschen ein einzelnes Datensatzes aus einer Datenbank.
> - So überprüfen Sie, dass eine bestimmte Schaltfläche in einem Formular geklickt wurde.
>   
> 
> Features/Technologien erläutert:
> 
> - Die `WebGrid` Helper.
> - Die SQL-Anweisung `Delete` Befehl.
> - Die `Database.Execute` Methode für die Ausführung einer SQL `Delete` Befehl.


## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie gelernt, wie einen vorhandener Datenbankdatensatz zu aktualisieren. Dieses Lernprogramm ist ähnlich, außer dass statt den Datensatz aktualisieren Sie sie löschen müssen. Die Prozesse sind nahezu identisch, außer dass löschen einfacher ist, damit in diesem Tutorial kurz werden.

In der *Filme* Seite, die Sie aktualisieren müssen die `WebGrid` Helper, sodass, dass die It anzeigt eine **löschen** neben jeder Film zur Ergänzung der **bearbeiten** Verknüpfung, die Sie zuvor hinzugefügt haben.

![Seite "Movies" mit einem Link "löschen" für jeden Film](deleting-data/_static/image1.png)

Wie bei der Bearbeitung beim Klicken auf die **löschen** Link gelangen Sie zu einer anderen Seite befindet, in dem die Filminformationen bereits in einem Formular:

![Löschen Sie Seite "Movie" mit einem Film angezeigt](deleting-data/_static/image2.png)

Sie können dann die Schaltfläche, um den Datensatz dauerhaft zu löschen klicken.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Die Movie-Auflistung hinzugefügt einen Link "löschen"

Beginnen Sie durch das Hinzufügen einer **löschen** Verknüpfen mit der `WebGrid` Helper. Dieser Link ist ähnlich wie die **bearbeiten** Verknüpfung, die Sie in einem vorherigen Tutorial hinzugefügt.

Öffnen der *Movies.cshtml* Datei.

Ändern der `WebGrid` Markup im Hauptteil der Seite durch Hinzufügen einer Spalte. Dies ist die geänderte Markup:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Die neue Spalte, ist diese:

[!code-html[Main](deleting-data/samples/sample2.html)]

Die Möglichkeit, die das Raster konfiguriert ist, die **bearbeiten** Spalte ganz links im Raster ist und die **löschen** Spalte ganz rechts ist. (Es ist ein Komma nach dem die `Year` Spalte, für den Fall, dass Sie nicht feststellen.) Es ist nichts Besonderes, diese Linkspalten wo, und Sie könnten einfach platzieren Sie sie nebeneinander. In diesem Fall sind separate, um sie vermischt wird schwieriger zu machen.

![Seite "Movies" mit Links "Bearbeiten" und "Details markiert, um anzugeben, dass sie sich nicht nebeneinander befinden](deleting-data/_static/image3.png)

Die neue Spalte zeigt eine Verknüpfung (`<a>` Element), dessen Text lautet "Löschen". Das Ziel des Links (die `href` Attribut) wird der Code, der letztendlich löst z. B. diese URL, mit der `id` Wert für jeden Film:

[!code-css[Main](deleting-data/samples/sample3.css)]

Dieser Link wird aufgerufen, eine Seite namens *DeleteMovie* , und übergeben sie die ID des Films, die Sie ausgewählt haben.

In diesem Tutorial Gehe nicht ausführlich, wie dieser Link erstellt wird, da sie fast identisch mit ist der **bearbeiten** Link aus dem vorherigen Lernprogramm ([Aktualisieren von Daten in ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Erstellen die Seite "löschen"

Nun können Sie die Seite erstellen, die das Ziel für die **löschen** -Link in das Raster.

> [!NOTE] 
> 
> **Wichtige** die Technik zunächst auswählen einen Datensatz zu löschen, und klicken Sie dann mit einer separaten Seite und einer Schaltfläche Überprüfen, ob den Vorgang ist äußerst wichtig, für die Sicherheit. Wie Sie in vorherigen Tutorials gelesen haben, sodass *alle* Art von Änderung an Ihrer Website sollte *immer* erfolgen mit einem Formular &mdash; , also mithilfe eines HTTP POST-Vorgangs. Wenn Sie es wurde ermöglicht, ändern Sie die Website nur über einen Link (die mithilfe einen GET-Vorgang), könnten Personen einfache Anforderungen an Ihre Website vornehmen und löschen Sie Ihre Daten. Selbst ein Suchmaschinen-Crawler, der Ihre Website Indizierung ist konnte Daten nur über Links versehentlich gelöscht werden.
> 
> Wenn Ihre app auf Personen, die einen Datensatz zu ändern kann, müssen Sie den Datensatz für dem Benutzer vorhanden, für die Bearbeitung trotzdem. Aber Sie könnten versucht sein, überspringen Sie diesen Schritt für das Löschen eines Datensatzes. Überspringen Sie diesen Schritt, jedoch nicht. (Es ist auch hilfreich für Benutzer finden Sie unter dem Datensatz, und bestätigen Sie, dass sie den Datensatz löschen, den sie vorgesehen.)
> 
> In einer nachfolgenden Tutorial Menge sehen Sie, wie Anmeldename-Funktionalität hinzufügen, damit ein Benutzer sich anmelden, bevor das Löschen eines Datensatzes müssten.


Erstellen Sie eine Seite mit dem Namen *DeleteMovie.cshtml* , und Ersetzen Sie die neuerungen in der Datei mit folgendem Markup:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Dieses Markup ist, wie die *EditMovie* -Seiten, außer dass anstelle von Textfeldern (`<input type="text">`), das Markup enthält `<span>` Elemente. Es gibt hier nichts zu bearbeiten. Müssen Sie lediglich die Movie-Details anzuzeigen, damit die Benutzer können Sie sicher, dass sie den richtigen Film löschen.

Das Markup enthält bereits einen Link, der der Benutzer auf die Movie-Angebotsseite zurückzukehren.

Wie in der *EditMovie* Seite, die den ausgewählten Film-ID wird in einem ausgeblendeten Feld gespeichert. (Es wird auf der Seite im vornherein als Wert einer Abfragezeichenfolge übergeben.) Es gibt eine `Html.ValidationSummary` Aufruf, der Validierungsfehler angezeigt wird. In diesem Fall möglicherweise der Fehler, dass keine Film-ID an die Seite übergeben wurde oder die Film-ID ungültig ist. Diese Situation kann eintreten, wenn ein Benutzer auf dieser Seite ausgeführt wurde, ohne zuvor einen Film in den *Filme* Seite.

Die Beschriftung der Schaltfläche ist **löschen Film**, und dessen Namensattribut nastaven NA hodnotu `buttonDelete`. Die `name` Attribut wird im Code verwendet werden, auf um die Schaltfläche zu identifizieren, die das Formular übermittelt.

Sie müssen Code schreiben, um die (1) die Filmdetails lesen, wenn die Seite zum ersten Mal angezeigt wird, und löschen (2) tatsächlich den Film aus, klickt der Benutzer die Schaltfläche.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zum Lesen eines einzelnen Films

Am oberen Rand der *DeleteMovie.cshtml* Seite, fügen Sie den folgenden Codeblock:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Dieses Markup immer das gleiche wie der entsprechende Code in die *EditMovie* Seite. Es ruft die Film-ID aus der Abfragezeichenfolge ab und verwendet die ID, um einen Datensatz aus der Datenbank zu lesen. Der Code enthält den Test zur Überprüfung (`IsInt()` und `row != null`) sicherstellen, dass die Film-ID übergeben wird, auf der Seite gültig ist.

Denken Sie daran, dass dieser Code sollte nur zum ersten Mal ausführen, die die Seite ausgeführt wird. Sie möchten die Movie-Datensatz aus der Datenbank erneut zu lesen, klickt der Benutzer die **löschen Film** Schaltfläche. Aus diesem Grund code zum Lesen des Films befindet sich innerhalb eines Tests, die besagt, `if(!IsPost)` &mdash; , also *ist die Anforderung keinen Post-Vorgang (Formularübermittlung)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Hinzufügen von Code, um den ausgewählten Film zu löschen.

Um den Film löschen, wenn der Benutzer auf die Schaltfläche klickt, fügen Sie den folgenden Code innerhalb der schließenden Klammer der der `@` blockieren:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Dieser Code ist mit dem Code zum Aktualisieren eines vorhandenen Datensatzes ähnlich, aber einfacher. Der Code führt im Grunde eine SQL `Delete` Anweisung.

 Wie in der *EditMovie* Seite der Code befindet sich in einem `if(IsPost)` Block. Dieses Mal die `if()` Bedingung ist ein wenig komplizierter: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Es gibt hier zwei Bedingungen. Die erste ist, dass die Seite übermittelt wird, ist, wie Sie vor dem gesehen haben &mdash; `if(IsPost)`.

Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`, was bedeutet, dass die Anforderung ein Objekt namens hat `buttonDelete`. Zugegeben, ist es ein indirektes Verfahren dar, testen, auf welche Schaltfläche auf das Formular übermittelt. Wenn ein Formular mehrere Submit-Schaltflächen enthält, wird nur der Name der Schaltfläche, auf die geklickt wurde, in der Anforderung angezeigt. Aus diesem Grund, logisch, wenn der Name einer bestimmten Schaltfläche angezeigt, in der Anforderung wird &mdash; oder wie im Code angegeben werden, wenn diese Schaltfläche nicht leer ist &mdash; , das die Schaltfläche, die das Formular übermittelt wird.

Die `&&` Operator bedeutet, dass "und" (logisches AND). Aus diesem Grund für die gesamte `if` Bedingung...

*Diese Anforderung ist ein Beitrag (keine Anforderung zum ersten Mal)*  
  
 UND  
  
*Die* `buttonDelete` *die Schaltfläche, die das Formular übermittelt wurde.*

Dieses Formular (genauer gesagt auf dieser Seite) enthält nur ein Optionsfeld, also die zusätzliche Tests für `buttonDelete` ist technisch nicht erforderlich. Dennoch können Sie einen Vorgang ausführen, der Daten dauerhaft entfernt. Sie möchten also so sicher wie möglich sein, dass Sie den Vorgang durchführen, nur, wenn der Benutzer ausdrücklich angefordert hat. Nehmen wir beispielsweise an, dass Sie diese Seite später erweitert, und weitere Schaltflächen hinzugefügt. Der Code, der den Film löscht wird auch dann ausgeführt, wenn nur die `buttonDelete` Schaltfläche geklickt wurde.

Wie in der *EditMovie* Seite, Sie rufen Sie die ID aus dem ausgeblendeten Feld und führen Sie den SQL-Befehl. Die Syntax für die `Delete` -Anweisung ist:

`DELETE FROM table WHERE ID = value`

Es ist wichtig, enthalten die `WHERE` -Klausel und die-ID. Wenn Sie der WHERE-Klausel auslassen *alle Datensätze in der Tabelle gelöscht werden*. Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl mit einem Platzhalter.

## <a name="testing-the-movie-delete-process"></a>Testen des Prozesses der Film löschen

Jetzt können Sie testen. Führen Sie die *Filme* Seite, und klicken Sie auf **löschen** neben einen Film. Wenn die *DeleteMovie* klicken Sie mit der Seite angezeigt wird, auf **löschen Film**.

![Löschen Sie Seite "Movie" mit hervorgehobener Schaltfläche "Löschen Film"](deleting-data/_static/image4.png)

Wenn Sie die Schaltfläche klicken, wird der Code löscht die Filme und gibt Sie zurück auf die Movie-Auflistung. Sie können es für den gelöschten Film suchen und vergewissern Sie sich, dass es gelöscht wird.

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächste Tutorial erfahren Sie, wie Sie alle Seiten auf Ihrer Website zu geben, eine allgemeine Aussehen und Layout.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Vollständige Liste für Movie-Seite (Aktualisierung mit Links "löschen")

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Vollständige Liste für DeleteMovie-Seite

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](../introducing-razor-syntax-c.md)
- [SQL-DELETE-Anweisung](http://www.w3schools.com/sql/sql_delete.asp) auf der Website W3Schools

> [!div class="step-by-step"]
> [Zurück](updating-data.md)
> [Weiter](layouts.md)
