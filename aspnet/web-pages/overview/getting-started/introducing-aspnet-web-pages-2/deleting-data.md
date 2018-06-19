---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Einführung in ASP.NET Web Pages - Datenbankdaten löschen | Microsoft Docs
author: tfitzmac
description: In diesem Lernprogramm wird gezeigt, wie einen einzelne Datenbank-Eintrag zu löschen. Es wird davon ausgegangen, dass Sie der Reihe durch Aktualisieren der Datenbankdaten in ASP.NET Web-PA abgeschlossen haben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897415"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Einführung in ASP.NET Web Pages - Datenbankdaten löschen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Lernprogramm wird gezeigt, wie einen einzelne Datenbank-Eintrag zu löschen. Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Aktualisieren der Datenbankdaten in ASP.NET Web Pages](updating-data.md).
> 
> Lernen Sie:
> 
> - Auswählen ein einzelnes Datensatzes aus einer Liste von Datensätzen
> - Vorgehensweise: Löschen Sie einen einzelnen Datensatz aus einer Datenbank.
> - Wie Sie überprüfen, dass eine bestimmte Schaltfläche in einem Formular geklickt wurde.
>   
> 
> Features/Technologien erläutert:
> 
> - Die `WebGrid` Helper.
> - Die SQL `Delete` Befehl.
> - Die `Database.Execute` zum Ausführen einer SQL-Methode `Delete` Befehl.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Im vorherigen Lernprogramm haben Sie gelernt, wie einen vorhandene Datenbank-Datensatz aktualisiert wird. Dieses Lernprogramm ist ähnlich, außer dass anstelle der Datensatz aktualisiert, Sie sie löschen müssen. Die Prozesse sind nahezu identisch, außer dass löschen einfacher, damit dieses Lernprogramm kurz sein wird.

In der *Filme* Seite, aktualisieren Sie die `WebGrid` Helper so, dass die It zeigt eine **löschen** neben jeder Film, begleitet die **bearbeiten** verknüpfen, die Sie zuvor hinzugefügt haben.

![Filme-Seite, die mit einer Delete-Link, um jedes Films](deleting-data/_static/image1.png)

Wie bei der Bearbeitung beim Klicken auf die **löschen** Link, es führt Sie zu einer anderen Seite ist, in dem die Informationen bereits in einem Formular:

![Löschen Sie die filmdetailseite mit einen Film angezeigt](deleting-data/_static/image2.png)

Sie können dann die Schaltfläche, um den Datensatz endgültig löschen klicken.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Beim Hinzufügen einer Delete-Verknüpfung auf die Film-Auflistung

Beginnen Sie durch Hinzufügen einer **löschen** Verknüpfen mit der `WebGrid` Helper. Dieser Link ist ähnlich wie die **bearbeiten** verknüpfen, die Sie in einem vorherigen Lernprogramm hinzugefügt haben.

Öffnen der *Movies.cshtml* Datei.

Ändern der `WebGrid` Markup im Text der Seite durch Hinzufügen einer Spalte. So sieht die geänderte Markup aus:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Die neue Spalte wird diese:

[!code-html[Main](deleting-data/samples/sample2.html)]

Das Raster konfiguriert ist, wie die **bearbeiten** Spalte ganz links im Raster ist und die **löschen** Spalte ganz rechts ist. (Es ist ein Komma nach der `Year` Spalte jetzt für den Fall, dass Sie nicht feststellen.) Sind keine besonderen über diesen Linkspalten an mich, und Sie konnte sie genauso einfach nebeneinander platzieren. In diesem Fall können sie separate zu machen schwieriger zu verwechselt abrufen.

![Filme-Seite mit Links bearbeiten "und" Details markiert, um anzugeben, dass sie nicht nebeneinander sind](deleting-data/_static/image3.png)

Die neue Spalte zeigt eine Verknüpfung (`<a>` Element), dessen Text lautet "Delete". Das Ziel der Verknüpfung (dessen `href` Attribut) ist Code, der letztendlich in etwa diese URL mit aufgelöst wird der `id` Wert für jede Film unterschiedlich:

[!code-css[Main](deleting-data/samples/sample3.css)]

Dieser Link wird eine Seite mit dem Namen aufrufen *DeleteMovie* , und übergeben sie die ID des Films, die Sie ausgewählt haben.

In diesem Lernprogramm wird nicht näher wie diesen Link erstellt wird, da sie fast identisch mit ist der **bearbeiten** Link aus dem vorherigen Lernprogramm ([Aktualisieren der Datenbankdaten in ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Erstellen die Seite "löschen"

Nun können Sie die Seite erstellen, die das Ziel für die **löschen** Link im Raster.

> [!NOTE] 
> 
> **Wichtige** die Technik der zuerst wählen Sie einen Datensatz löschen und dann mithilfe einer separaten Seite und einer Schaltfläche Überprüfen, ob den Prozess ist äußerst wichtig für die Sicherheit. Wie Sie in vorherigen Lernprogrammen gelesen haben, sodass *alle* Art von Änderung an Ihrer Website sollte *immer* erfolgen über ein Formular &mdash; , also mithilfe eines HTTP POST-Vorgangs. Wenn es vorgenommenen möglich, den Standort zu ändern, indem Sie einfach einen Link (die mithilfe einen GET-Vorgang), konnte Personen stellen einfache Anforderungen an Ihre Website und Ihre Daten zu löschen. Auch ein Suchmaschinen-Crawler, der die Indizierung konnte nur über Links versehentlich Daten löschen.
> 
> Wenn Ihre app Personen, die einen Datensatz ändern kann, müssen Sie den Datensatz für den Benutzer anzuzeigen, für die Bearbeitung trotzdem. Aber Sie möchten, überspringen Sie diesen Schritt für das Löschen eines Datensatzes sein. Überspringen Sie diesen Schritt jedoch nicht. (Es ist auch nützlich für Benutzer finden Sie unter den Datensatz und bestätigen Sie, dass sie den Datensatz löschen, den sie für den gedacht.)
> 
> In einem späteren Lernprogramm Satz sehen Sie, wie Anmeldenamen-Funktionen hinzugefügt, damit ein Benutzer melden Sie sich vor dem Löschen eines Datensatzes müssten.


Erstellen Sie eine Seite mit dem Namen *DeleteMovie.cshtml* , und Ersetzen Sie die Neuigkeiten in der Datei durch Folgendes Markup:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Dieses Markup ähnelt der *EditMovie* Seiten, außer dass anstelle von Textfeldern (`<input type="text">`), das Markup enthält `<span>` Elemente. Es ist nichts hier bearbeiten. Alles, was ist die Filmdetails angezeigt werden, damit die Benutzer können Sie sicher, dass sie den richtigen Film löschen.

Das Markup enthält bereits einen Link, der dem Benutzer, die auf der Seite "Movie-Angebot" zurückgeben kann.

Wie in der *EditMovie* Seite, die ID des ausgewählten Films in einem ausgeblendeten Feld gespeichert wird. (Es wird auf der Seite ursprünglich als ein Wert der Abfragezeichenfolge übergeben.) Es ist ein `Html.ValidationSummary` aufrufen, die Validierungsfehler angezeigt werden. In diesem Fall möglicherweise der Fehler, dass keine Film-ID auf der Seite "übergeben wurde oder die Film-ID ungültig ist. Diese Situation kann eintreten, wenn jemand auf dieser Seite ausgeführt wurde, ohne zuvor einen Film in der *Filme* Seite.

Die Beschriftung der Schaltfläche wird **löschen Film**, und ihr Namensattribut wird festgelegt, um `buttonDelete`. Die `name` Attribut wird im Code verwendet werden, um die Schaltfläche zu identifizieren, die das Formular gesendet.

Sie müssen Code schreiben, um die (1) die Filmdetails gelesen, wenn die Seite erstmals angezeigt wird und den Film 2) tatsächlich gelöscht werden, wenn der Benutzer die Schaltfläche klickt.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zu einen einzigen Film lesen

Am oberen Rand der *DeleteMovie.cshtml* Seite, fügen Sie den folgenden Codeblock:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Dieses Markup ist identisch mit den entsprechenden Code in der *EditMovie* Seite. Er ruft die Film-ID aus der Abfragezeichenfolge ab und verwendet die ID, um einen Datensatz aus der Datenbank zu lesen. Der Code enthält die Validierungstest (`IsInt()` und `row != null`) um sicherzustellen, dass die Film-ID übergeben wird, um die Seite ungültig ist.

Denken Sie daran, dass dieser Code nur beim ersten ausgeführt werden soll, die die Seite ausgeführt wird. Sie möchten nicht die Film-Eintrag aus der Datenbank erneut zu lesen, klickt der Benutzer die **löschen Film** Schaltfläche. Deshalb zum Lesen der Film befindet sich innerhalb eines Tests, die besagt, dass code `if(!IsPost)` &mdash; , also *ist die Anforderung keinen Post-Vorgang (Formularübermittlung)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Hinzufügen von Code zum Löschen des ausgewählten Films

Um den Film klickt der Benutzer die Schaltfläche "" zu löschen, fügen Sie den folgenden Code innerhalb der schließenden Klammer der der `@` blockieren:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Dieser Code ist ähnlich wie der Code für einen vorhandenen Datensatz aktualisieren, aber einfacher. Der Code führt im Wesentlichen eine SQL `Delete` Anweisung.

 Wie in der *EditMovie* Seite der Code befindet sich in einem `if(IsPost)` Block. Dieses Mal die `if()` Bedingung ist etwas komplizierter: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Es gibt zwei Bedingungen hier ein. Die erste ist, dass die Seite übermittelt wird, wie Sie vor dem gesehen haben &mdash; `if(IsPost)`.

Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`, was bedeutet, dass die Anforderung ein Objekt mit dem Namen verfügt über `buttonDelete`. Zugegeben, ist es ein indirektes Verfahren dar, auf welche Schaltfläche Senden des Formulars testen. Wenn ein Formular mehrere Schaltflächen "Senden" enthält, wird nur der Namen der Schaltfläche, auf die geklickt wurde, in der Anforderung. Aus diesem Grund, logisch, wenn der Name einer bestimmten Schaltfläche wird, in der Anforderung angezeigt &mdash; oder wie im Code angegeben werden, wenn diese Schaltfläche nicht leer ist &mdash; , die die Schaltfläche, die das Formular gesendet wird.

Die `&&` Operator bedeutet "und" (logisches AND). Daher die gesamte `if` Bedingung ist...

*Diese Anforderung wird einer POST-Anforderung (keine Anforderung zum ersten Mal)*  
  
 UND  
  
*Die* `buttonDelete` *Schaltfläche wurde die Schaltfläche, die das Formular gesendet.*

Dieses Formular (in tatsächlich auf dieser Seite) enthält nur ein Optionsfeld, sodass die zusätzliche Tests für `buttonDelete` ist technisch nicht erforderlich. Dennoch können Sie einen Vorgang ausführen, der Daten dauerhaft entfernt. Sie sollten sich daher so sicher wie möglich sein, dass Sie den Vorgang durchführen, nur, wenn der Benutzer explizit angefordert hat. Nehmen wir beispielsweise an, dass Sie diese Seite später erweitert und andere Schaltflächen hinzugefügt. Der Code, der den Film löscht wird selbst dann ausgeführt, wenn die `buttonDelete` Schaltfläche geklickt wurde.

Wie in der *EditMovie* Seite, die ID aus dem ausgeblendeten Feld abrufen und führen Sie den SQL-Befehl. Die Syntax für die `Delete` -Anweisung ist:

`DELETE FROM table WHERE ID = value`

Schließen Sie unbedingt die `WHERE` -Klausel und die-ID. Wenn Sie die WHERE-Klausel auslassen *alle Datensätze in der Tabelle gelöscht werden*. Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl, indem Sie mithilfe eines Platzhalters.

## <a name="testing-the-movie-delete-process"></a>Testen des Prozesses der Film löschen

Sie können jetzt testen. Führen Sie die *Filme* Seite, und klicken Sie auf **löschen** neben einen Film. Wenn die *DeleteMovie* Seite angezeigt wird, klicken Sie auf **löschen Film**.

![Löschen von filmdetailseite mit Schaltfläche "Löschen Film" hervorgehoben](deleting-data/_static/image4.png)

Wenn Sie die Schaltfläche klicken, wird der Code löscht die Filme und zur Auflistung Film gibt. Sie können es für den gelöschten Film suchen, und bestätigen Sie, dass sie gelöscht wurde, wird.

## <a name="coming-up-next"></a>Als Nächstes kommen

Der nächste Lernprogrammen wird gezeigt, wie eine allgemeine Darstellung und Layout alle Seiten auf Ihrer Website zu gewähren.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Vollständige Auflistung für Filmdetailseite (inklusive löschen Links)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Vollständige Auflistung für die Seite "DeleteMovie"

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](../introducing-razor-syntax-c.md)
- [DELETE-Anweisung von SQL](http://www.w3schools.com/sql/sql_delete.asp) auf der Website W3Schools

> [!div class="step-by-step"]
> [Zurück](updating-data.md)
> [Weiter](layouts.md)
