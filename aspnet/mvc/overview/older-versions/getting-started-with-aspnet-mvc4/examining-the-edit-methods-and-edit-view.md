---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: "Prüfen der Methoden bearbeiten und die Bearbeitungsansicht | Microsoft Docs"
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 315914056c0a666fdf23cf82a314a999e03114b6
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Prüfen der Methoden bearbeiten und die Bearbeitungsansicht
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt fügen Sie der generierten Aktionsmethoden und Ansichten für den Controller Film untersuchen. Anschließend fügen Sie eine benutzerdefinierte Suchseite.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers. Halten Sie den Mauszeiger über ein **bearbeiten** Link, um die URL anzuzeigen, die es verknüpft.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Die **bearbeiten** Link wurde generiert, indem die `Html.ActionLink` Methode in der *Views\Movies\Index.cshtml* anzeigen:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Die `Html` Objekt ist ein Hilfsprogramm, das verfügbar, die mit einer Eigenschaft gemacht wird für die [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) Basisklasse. Die `ActionLink` Methode des Hilfsprogramms ganz einfach zum dynamischen Generieren von HTML-Links, die mit Aktionsmethoden im Controller verknüpft. Das erste Argument für die `ActionLink` Methode ist der Text des Links zum Rendern (z. B. `<a>Edit Me</a>`). Das zweite Argument ist der Name der aufzurufenden Aktionsmethode. Das letzte Argument ist ein [anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , generiert die Routendaten (in diesem Fall wird die ID des 4).

Der generierte Link in der vorherigen Abbildung dargestellt ist `http://localhost:xxxxx/Movies/Edit/4`. Die Standardroute (in vielen Branchen *App\_Start\RouteConfig.cs*) nimmt das URL-Muster `{controller}/{action}/{id}`. Aus diesem Grund ASP.NET übersetzt `http://localhost:xxxxx/Movies/Edit/4` in eine Anforderung an die `Edit` Aktionsmethode des der `Movies` Controller mit dem Parameter `ID` gleich 4. Überprüfen Sie den folgenden Code aus der *App\_Start\RouteConfig.cs* Datei.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Sie können auch die Aktionsmethodenparameter über eine Abfragezeichenfolge übergeben. Beispielsweise die URL `http://localhost:xxxxx/Movies/Edit?ID=4` übergibt außerdem den Parameter `ID` 4 die `Edit` Aktionsmethode von der `Movies` Controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Öffnen der `Movies` Controller. Die beiden `Edit` Aktionsmethoden unten angezeigt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist. Dieses Attribut gibt an, diese Überladung von der `Edit` Methode kann nur für die POST-Anforderungen aufgerufen werden. Sie können Werte anwenden der `HttpGet` Attribut mit dem ersten bearbeiten Methode, aber dies ist nicht erforderlich, da dies die Standardeinstellung ist. (Verweisen wir auf Aktionsmethoden, die implizit zugewiesen sind die `HttpGet` Attribut `HttpGet` Methoden.)

Die `HttpGet` `Edit` Methode nimmt die Film-ID-Parameter, mit dem Entity Framework Film sucht `Find` -Methode, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück. Gibt die ID-Parameter an eine [Standardwert](https://msdn.microsoft.com/library/dd264739.aspx) 0 (null) zurück, wenn die `Edit` Methode wird aufgerufen, ohne einen Parameter. Wenn Sie ein Film nicht gefunden werden kann, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) wird zurückgegeben. Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt. Das folgende Beispiel zeigt die Bearbeitungsansicht, die generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Beachten Sie, wie die Vorlage für die Sicht hat eine `@model MvcMovie.Models.Movie` -Anweisung am Anfang der Datei – Dies gibt an, dass die Sicht erwartet, das Modell für die Vorlage Sicht vom Typ dass `Movie`.

Der scaffolded Code verwendet mehrere *Hilfsmethoden* um das HTML-Markup zu optimieren. Die [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Hilfsprogramm zeigt den Namen des Felds (&quot;Titel&quot;, &quot;ReleaseDate&quot;, &quot;"Genre"&quot;, oder &quot;Preis &quot;). Die [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) -Hilfsmethode rendert ein HTML `<input>` Element. Die [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Hilfsprogramm zeigt Fehlermeldungen Validierung dieser Eigenschaft zugeordnet.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Klicken Sie auf einen Link **Bearbeiten**. Zeigen Sie im Browser den Quelltext für die Seite an. Der HTML-Code für das Form-Element wird unten gezeigt.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Die `<input>` Elemente sind in einem HTML- `<form>` Element, dessen `action` -Attribut festgelegt ist, an die */Filme/bearbeiten* URL. Die Formulardaten werden an den Server gesendet werden bei der **bearbeiten** geklickt wird.

## <a name="processing-the-post-request"></a>Verarbeiten der POST-Anforderung

Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Die [ASP.NET MVC-modellbindung](https://msdn.microsoft.com/magazine/hh781022.aspx) nimmt die übermittelte Formularwerte und erstellt eine `Movie` -Objekt, das als übergeben wird, die `movie` Parameter. Die `ModelState.IsValid`-Methode stellt sicher, dass die im Formular übermittelten Daten verwendet werden können, um ein `Movie`-Objekt zu ändern (bearbeiten oder aktualisieren). Wenn die Daten gültig ist, wird die Filmdaten gespeichert, auf die `Movies` Auflistung von der `db(MovieDBContext` Instanz). Die neue Filmdaten in der Datenbank gespeichert ist, durch Aufrufen der `SaveChanges` Methode `MovieDBContext`. Nach dem Speichern der Daten an, der Code leitet den Benutzer die `Index` Aktionsmethode des der `MoviesController` Klasse, welche zeigt die Film-Auflistung, einschließlich der gerade vorgenommenen Änderungen.

Wenn die bereitgestellten Werte nicht gültig sind, werden sie in das Formular erneut angezeigt. Die `Html.ValidationMessageFor` Hilfen in den *Edit.cshtml* anzeigen, die Vorlage zum Anzeigen von entsprechenden Fehlermeldungen achten.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> auf die jQuery-Validierung für nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) Sie müssen für ein Dezimaltrennzeichen einschließen *globalize.js* und Ihre spezifischen *cultures/globalize.cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Der folgende Code zeigt die Änderungen an der Views\Movies\Edit.cshtml-Datei zur Bearbeitung der &quot;fr-FR&quot; Kultur:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Decimal Feld möglicherweise ein Komma und keinem Dezimaltrennzeichen. Als temporäre Korrektur können Sie die Globalisierung-Element an der Stammdatei "Web.config" Projekte hinzufügen. Der folgende Code zeigt die Globalisierung-Element mit der Kultur Englisch (USA) festgelegt.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Alle der `HttpGet` Methoden einem ähnliches Muster folgen. Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten, in der Groß-/Kleinschreibung `Index`), und übergeben Sie das Modell zur Ansicht. Die `Create` Methode übergibt ein leeres Movie-Objekt an die Erstellungsansicht. Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode. Ändern von Daten in einer HTTP GET-Methode ist ein Sicherheitsrisiko, gemäß der in der Post-Blogeintrag [ASP.NET MVC Tipp #46 – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Ändern von Daten in einer GET-Methode auch verletzt, bewährte Methoden für HTTP und das architektonische [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) Muster, der angibt, dass die GET-Anforderungen sollten nicht den Status Ihrer Anwendung ändern. Die Durchführung eines GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebenwirkungen hat und die permanenten Daten nicht ändert.

## <a name="adding-a-search-method-and-search-view"></a>Hinzufügen eines Search-Methode und des Anzeigebereichs

In diesem Abschnitt fügen Sie eine `SearchIndex` Aktionsmethode, mit dem Sie suchen, Filme nach Genre oder Namen. Dieser Wert ist, mithilfe der */Filme/SearchIndex* URL. Die Anforderung wird ein HTML-Formular angezeigt, die Eingabe Elemente, die ein Benutzer eingeben kann enthält, um einen Film zu suchen. Wenn ein Benutzer das Formular sendet, wird die Aktionsmethode rufen Sie die Search-Werte, die vom Benutzer bereitgestellt und verwenden Sie die Werte, um die Datenbank zu suchen.

## <a name="displaying-the-searchindex-form"></a>Anzeigen des Formulars SearchIndex

Starten Sie durch Hinzufügen einer `SearchIndex` Aktionsmethode zur vorhandenen `MoviesController` Klasse. Die Methode gibt eine Ansicht zurück, die einem HTML-Formular enthält. Hier ist der Code ein:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Die erste Zeile der `SearchIndex` -Methode erstellt die folgenden [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) Abfrage Filme auswählen:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Die Abfrage wird an diesem Punkt definiert, jedoch noch nicht für den Datenspeicher noch ausgeführt.

Wenn die `searchString` -Parameters enthält eine Zeichenfolge, die Filme Abfrage wurde geändert, um auf den Wert der Suchzeichenfolge, mithilfe des folgenden Codes zu filtern:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Der Code `s => s.Title` oben ist ein [Lambdaausdruck](https://msdn.microsoft.com/library/bb397687.aspx). Lambdas werden in methodenbasierten verwendet [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) -Abfragen als Argumente für Standardabfrageoperator-Methoden wie z. B. die [, in denen](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Methode, die im obigen Code verwendet. LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert sind oder wenn sie geändert werden, durch Aufrufen einer Methode wie z. B. `Where` oder `OrderBy`. Stattdessen Ausführung der Abfrage wird verzögert, was bedeutet, dass die Auswertung eines Ausdrucks verzögert wird, bis der realisierte Wert tatsächlich in einer Schleife durchlaufen wird oder die [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) -Methode aufgerufen wird. In der `SearchIndex` Sample, die Abfrage in der Ansicht SearchIndex ausgeführt wird. Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](https://msdn.microsoft.com/library/bb738633.aspx).

Nachdem Sie implementieren können, die `SearchIndex` anzeigen, die der Benutzer das Formular angezeigt wird. Mit der rechten Maustaste innerhalb der `SearchIndex` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**. In der **Ansicht hinzufügen** Dialogfeld geben, dass Sie die offensichtlichen übergeben ein `Movie` Objekt, das die der Ansichtenvorlage als der Modellklasse. In der **Gerüst Vorlage** wählen **Liste**, klicken Sie dann auf **hinzufügen**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Beim Klicken auf die **hinzufügen** Schaltfläche, die *Views\Movies\SearchIndex.cshtml* Ansichtenvorlage erstellt wurde. Da Sie ausgewählt haben **Liste** in der **Gerüst Vorlage** aufzulisten, Visual Studio automatisch generiert (Gerüstbau) einige Standard-Markup in der Ansicht. Das Gerüst erstellt eine HTML-Formular. Er überprüft die `Movie` Klasse und erstellte Code zum Rendern `<label>` Elemente für jede Eigenschaft der Klasse. Die Liste unten zeigt die Ansicht für das Erstellen, das generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Fügen Sie eine Abfragezeichenfolge wie `?searchString=ghost` an die URL an. Die gefilterten Filme werden angezeigt.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Wenn Sie die Signatur der Ändern der `SearchIndex` Methode zu einem Parameter mit dem Namen `id`, `id` Parameter entspricht der `{id}` Platzhalter für den standardmäßigen leitet Menge in der *"Global.asax"* Datei.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Die ursprüngliche `SearchIndex` -Methode sieht folgendermaßen aus:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Das geänderte `SearchIndex` Methode würde wie folgt aussehen:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten. Ja, nachdem Sie Hilfe der Benutzeroberfläche hinzufügen, sie filtern, Filme. Wenn Sie die Signatur der geändert und die `SearchIndex` IMM Methode zum Testen wie die Route datengebundenen-ID-Parameter übergeben, damit Ihre `SearchIndex` Methode nimmt einen Zeichenfolgenparameter, der mit dem Namen `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Öffnen der *Views\Movies\SearchIndex.cshtml* , und stellen Sie direkt nach `@Html.ActionLink("Create New", "Create")`, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Das folgende Beispiel zeigt einen Teil der *Views\Movies\SearchIndex.cshtml* Datei mit dem hinzugefügten Filtern Markup.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

Die `Html.BeginForm` Hilfsprogramm erstellt ein öffnendes `<form>` Tag. Die `Html.BeginForm` Helper bewirkt, dass das Formular an sich selbst zu senden, wenn der Benutzer durch Klicken auf das Formular sendet die **Filter** Schaltfläche.

Führen Sie die Anwendung, und versuchen Sie es für einen Film suchen.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Es ist keine `HttpPost` Überladung von der `SearchIndex` Methode. Nicht erforderlich, da die Methode ändern den Status der Anwendung ist nicht nur das Filtern von Daten.

Sie können die folgende `HttpPost SearchIndex`-Methode hinzufügen. In diesem Fall würde die Instanz zum Aufrufen der Aktion übereinstimmen der `HttpPost SearchIndex` -Methode, und die `HttpPost SearchIndex` Methode würde ausgeführt, wie in der folgenden Abbildung dargestellt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Doch selbst wenn Sie diese `HttpPost`-Version der `SearchIndex`-Methode hinzufügen, gibt es eine Einschränkung für die gesamte Implementierung. Stellen Sie sich vor, Sie möchten eine bestimmte Suche als Favorit speichern oder einen Link an Freunde senden, auf den diese klicken können, um dieselbe gefilterte Liste von Filmen anzuzeigen. Beachten Sie, dass die URL für die HTTP POST-Anforderung identisch mit der URL für die GET-Anforderung (Localhost:xxxxx/Filme/SearchIndex ist) – es keine Suchlaufinformationen Sie in der URL selbst sind. Rechts wird nun Informationen für die Suche an den Server als Wert eines Formulars gesendet. Dies bedeutet, dass Sie erfasst werden können, Suchinformationen Lesezeichen oder zum Hinzufügen von Freunden in einer URL senden.

Die Lösung besteht darin, eine Überladung der `BeginForm` , der angibt, dass die POST-Anforderung die Suchinformationen URL hinzugefügt werden soll, und dass er die Version HttpGet weitergeleitet werden soll die `SearchIndex` Methode. Ersetzen Sie die vorhandene parameterlosen `BeginForm` -Methode durch Folgendes:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Wenn Sie eine Suche übermitteln, enthält die URL nun eine Abfrage zu suchende Zeichenfolge. Das Suchen wird auch an Aktionsmethode `HttpGet SearchIndex` übertragen, auch wenn Sie eine `HttpPost SearchIndex`-Methode haben.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Suche nach "Genre" hinzufügen

Wenn Sie hinzugefügt haben die `HttpPost` Version der `SearchIndex` -Methode, löschen Sie es jetzt.

Als Nächstes fügen Sie eine Funktion zum Benutzer, die für Filme nach Genre suchen können. Ersetzen Sie die `SearchIndex`-Methode durch folgenden Code:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Diese Version von den `SearchIndex` Methode nimmt einen zusätzlichen Parameter, nämlich `movieGenre`. Erstellen Sie die ersten Zeilen des Codes eine `List` Objekt um Filmgenres aus der Datenbank zu speichern.

Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Der Code verwendet die `AddRange` der generischen Methode `List` Auflistung, die unterschiedliche Genres zur Liste hinzugefügt werden. (Ohne die `Distinct` Modifizierer, doppelte Genres würde hinzugefügt – zweimal in unserem Beispiel würde z. B. Comedy hinzugefügt werden). Der Code speichert dann die Liste von Genres in die `ViewBag` Objekt.

Der folgende Code zeigt die Vorgehensweise beim Überprüfen der `movieGenre` Parameter. Wenn er nicht leer ist, schränkt der Code weiter Filme Abfrage ausgewählten Filme an der angegebenen "Genre" beschränken.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Die Ansicht SearchIndex zur Unterstützung der Suche nach "Genre" Markup hinzufügen

Hinzufügen einer `Html.DropDownList` Hilfsmethode zum der *Views\Movies\SearchIndex.cshtml* Datei, direkt vor der `TextBox` Helper. Das vollständige Markup wird unten gezeigt:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Versuchen Sie eine Suche durch "Genre" Movie namentlich und von beiden Kriterien.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

In diesem Abschnitt untersuchen Sie die CRUD-Aktionsmethoden und Ansichten, die vom Framework generierten. Sie erstellt eine Aktionsmethode suchen und anzeigen, mit denen Benutzer anhand der Filmtitel und die "Genre" suchen kann. Im nächsten Abschnitt, betrachten Sie zum Hinzufügen einer Eigenschaft, um die `Movie` Modell und wie Sie einen Initialisierer hinzufügen, die automatisch eine Testdatenbank erstellen.

>[!div class="step-by-step"]
[Zurück](accessing-your-models-data-from-a-controller.md)
[Weiter](adding-a-new-field-to-the-movie-model-and-table.md)
