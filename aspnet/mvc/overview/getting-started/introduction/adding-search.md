---
uid: mvc/overview/getting-started/introduction/adding-search
title: Suche | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 8afa72d4dbc4695e7d26c6ef4052be08a7c69080
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="search"></a>Suchen
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Hinzufügen eines Search-Methode und des Anzeigebereichs

In diesem Abschnitt fügen Sie die Suchfunktion auf der `Index` Aktionsmethode, mit dem Sie suchen, Filme nach Genre oder Namen.

## <a name="updating-the-index-form"></a>Die Index-Formular aktualisieren

Starten mit dem Aktualisieren der `Index` Aktionsmethode zur vorhandenen `MoviesController` Klasse. Hier ist der Code ein:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

Die erste Zeile der `Index` -Methode erstellt die folgenden [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) Abfrage Filme auswählen:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Die Abfrage wird an diesem Punkt definiert, jedoch noch nicht für die Datenbank noch ausgeführt.

Wenn die `searchString` -Parameters enthält eine Zeichenfolge, die Filme Abfrage wurde geändert, um auf den Wert der Suchzeichenfolge, mithilfe des folgenden Codes zu filtern:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Der Code `s => s.Title` oben ist ein [Lambdaausdruck](https://msdn.microsoft.com/library/bb397687.aspx). Lambdas werden in methodenbasierten verwendet [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) -Abfragen als Argumente für Standardabfrageoperator-Methoden wie z. B. die [, in denen](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Methode, die im obigen Code verwendet. LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert sind oder wenn sie geändert werden, durch Aufrufen einer Methode wie z. B. `Where` oder `OrderBy`. Stattdessen Ausführung der Abfrage wird verzögert, was bedeutet, dass die Auswertung eines Ausdrucks verzögert wird, bis der realisierte Wert tatsächlich in einer Schleife durchlaufen wird oder die [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) -Methode aufgerufen wird. In der `Search` Beispiel wird die Abfrage wird ausgeführt, der *Index.cshtml* anzeigen. Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> Die [Contains](https://msdn.microsoft.com/library/bb155125.aspx) -Methode für die Datenbank, nicht der c#-Code oben ausgeführt wird. Für die Datenbank [Contains](https://msdn.microsoft.com/library/bb155125.aspx) ordnet [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), die Groß-/Kleinschreibung beachtet wird.

Jetzt können Sie aktualisieren die `Index` anzeigen, die der Benutzer das Formular angezeigt wird.

Führen Sie die Anwendung, und navigieren Sie zu */Filme/Index*. Fügen Sie eine Abfragezeichenfolge wie `?searchString=ghost` an die URL an. Die gefilterten Filme werden angezeigt.

![SearchQryStr](adding-search/_static/image1.png)

Wenn Sie die Signatur der Ändern der `Index` Methode zu einem Parameter mit dem Namen `id`, `id` Parameter entspricht der `{id}` Platzhalter für den standardmäßigen leitet Menge in der *App\_Start RouteConfig.cs* Datei.

[!code-json[Main](adding-search/samples/sample4.json)]

Die ursprüngliche `Index` -Methode sieht folgendermaßen aus:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Das geänderte `Index` Methode würde wie folgt aussehen:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.

![](adding-search/_static/image2.png)

Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten. Ja, nachdem Sie Hilfe der Benutzeroberfläche hinzufügen, sie filtern, Filme. Wenn Sie die Signatur der geändert und die `Index` IMM Methode zum Testen wie die Route datengebundenen-ID-Parameter übergeben, damit Ihre `Index` Methode nimmt einen Zeichenfolgenparameter, der mit dem Namen `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Öffnen der *Views\Movies\Index.cshtml* , und stellen Sie direkt nach `@Html.ActionLink("Create New", "Create")`, fügen Sie die hervorgehobenen Formularmarkup hinzu:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

Die `Html.BeginForm` Hilfsprogramm erstellt ein öffnendes `<form>` Tag. Die `Html.BeginForm` Helper bewirkt, dass das Formular an sich selbst zu senden, wenn der Benutzer durch Klicken auf das Formular sendet die **Filter** Schaltfläche.

Visual Studio 2013 ist eine gute Verbesserung bei der Anzeige und Bearbeitung von Dateien anzeigen. Wenn Sie die Anwendung mit einer Ansichtsdatei Öffnen ausführen, ruft Visual Studio 2013 die richtige Controlleraktionsmethode um die Ansicht anzuzeigen.

![](adding-search/_static/image3.png)

Klicken Sie mit die Indexansicht in Visual Studio öffnen Sie, (wie in der Abbildung oben gezeigt), und tippen Sie auf Ctr F5 oder F5, um die Anwendung auszuführen, und wiederholen dann einen Film gesucht.

![](adding-search/_static/image4.png)

Es ist keine `HttpPost` Überladung von der `Index` Methode. Nicht erforderlich, da die Methode ändern den Status der Anwendung ist nicht nur das Filtern von Daten.

Sie können die folgende `HttpPost Index`-Methode hinzufügen. In diesem Fall würde die Instanz zum Aufrufen der Aktion übereinstimmen der `HttpPost Index` -Methode, und die `HttpPost Index` Methode würde ausgeführt, wie in der folgenden Abbildung dargestellt.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Doch selbst wenn Sie diese `HttpPost`-Version der `Index`-Methode hinzufügen, gibt es eine Einschränkung für die gesamte Implementierung. Stellen Sie sich vor, Sie möchten eine bestimmte Suche als Favorit speichern oder einen Link an Freunde senden, auf den diese klicken können, um dieselbe gefilterte Liste von Filmen anzuzeigen. Beachten Sie, dass die URL für die HTTP POST-Anforderung identisch mit der URL für die GET-Anforderung (Localhost:xxxxx/Filme/Index ist) – es keine Suchlaufinformationen Sie in der URL selbst sind. Rechts wird nun Informationen für die Suche an den Server als Wert eines Formulars gesendet. Dies bedeutet, dass Sie erfasst werden können, Suchinformationen Lesezeichen oder zum Hinzufügen von Freunden in einer URL senden.

Die Lösung besteht darin, eine Überladung der `BeginForm` , der angibt, dass die POST-Anforderung die Suchinformationen URL hinzugefügt werden soll, und dass sie weitergeleitet werden soll die `HttpGet` Version der `Index` Methode. Ersetzen Sie die vorhandene parameterlosen `BeginForm` -Methode durch Folgendes Markup:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Wenn Sie eine Suche übermitteln, enthält die URL nun eine Abfrage zu suchende Zeichenfolge. Das Suchen wird auch an Aktionsmethode `HttpGet Index` übertragen, auch wenn Sie eine `HttpPost Index`-Methode haben.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Suche nach "Genre" hinzufügen

Wenn Sie hinzugefügt haben die `HttpPost` Version der `Index` -Methode, löschen Sie es jetzt.

Als Nächstes fügen Sie eine Funktion zum Benutzer, die für Filme nach Genre suchen können. Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Diese Version von den `Index` Methode nimmt einen zusätzlichen Parameter, nämlich `movieGenre`. Erstellen Sie die ersten Zeilen des Codes eine `List` Objekt um Filmgenres aus der Datenbank zu speichern.

Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Der Code verwendet die `AddRange` der generischen Methode `List` Auflistung, die unterschiedliche Genres zur Liste hinzugefügt werden. (Ohne die `Distinct` Modifizierer, doppelte Genres würde hinzugefügt – zweimal in unserem Beispiel würde z. B. Comedy hinzugefügt werden). Der Code speichert dann die Liste von Genres in die `ViewBag.MovieGenre` Objekt. Speichern von Daten (solche einen Film "Genre" des) als Kategorie ein [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) -Objekt in ein `ViewBag`, und klicken Sie dann den Zugriff auf die Daten der Kategorie in einem Dropdown-Listenfeld ein typischer Ansatz für MVC-Anwendungen ist.

Der folgende Code zeigt die Vorgehensweise beim Überprüfen der `movieGenre` Parameter. Wenn er nicht leer ist, schränkt der Code weiter Filme Abfrage ausgewählten Filme an der angegebenen "Genre" beschränken.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Wie bereits erwähnt, die Abfrage wird nicht ausgeführt auf der Basis der Daten, bis die Filmliste in einer Schleife durchlaufen wird (das geschieht in der Ansicht nach der `Index` Aktionsmethode zurückgegeben).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Die Indexansicht zur Unterstützung der Suche nach "Genre" Markup hinzufügen

Hinzufügen einer `Html.DropDownList` Hilfsmethode zum der *Views\Movies\Index.cshtml* Datei, direkt vor der `TextBox` Helper. Das vollständige Markup wird unten gezeigt:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Im folgenden Code:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Der Parameter "MovieGenre" stellt den Schlüssel für die `DropDownList` Hilfsmethode zum Suchen einer `IEnumerable<SelectListItem>` in der `ViewBag`. Die `ViewBag` wurde in der Aktionsmethode aufgefüllt:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Der Parameter "All" stellt eine optionsbezeichnung bereit. Wenn Sie diese Auswahl in Ihrem Browser überprüfen, sehen Sie sich, dass ihr Attribut "Value" leer ist. Da unser Controller nur filtert `if` die Zeichenfolge ist kein `null` oder leer ist, senden einen leeren Wert `movieGenre` zeigt alle Genres.

Sie können auch eine Option in der Standardeinstellung ausgewählt werden festlegen. Wenn Sie "Comedy" als Standardwert verwenden möchten, ändern Sie den Code im Controller wie folgt:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/Index*. Versuchen Sie eine Suche durch "Genre" Movie namentlich und von beiden Kriterien.

![](adding-search/_static/image8.png)

In diesem Abschnitt erstellt Sie eine Aktionsmethode suchen und anzeigen, mit denen Benutzer anhand der Filmtitel und die "Genre" suchen kann. Im nächsten Abschnitt, betrachten Sie zum Hinzufügen einer Eigenschaft, um die `Movie` Modell und wie Sie einen Initialisierer hinzufügen, die automatisch eine Testdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-a-new-field.md)
