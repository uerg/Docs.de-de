---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: "Prüfen der Methoden bearbeiten und die Bearbeitungsansicht (VB) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 25ba5887a9fd179e75a45d4e140592d0ea66184a
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Prüfen der Methoden bearbeiten und die Bearbeitungsansicht (VB)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/examining-the-edit-methods-and-edit-view.md) dieses Lernprogramm.


In diesem Abschnitt fügen Sie der generierten Aktionsmethoden und Ansichten für den Controller Film untersuchen. Anschließend fügen Sie eine benutzerdefinierte Suchseite.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers. Halten Sie den Mauszeiger über ein **bearbeiten** Link, um die URL anzuzeigen, die es verknüpft.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Die **bearbeiten** Link wurde generiert, indem die `Html.ActionLink` Methode in der *Views\Movies\Index.vbhtml* anzeigen:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

Die `Html` Objekt ist ein Hilfsprogramm, das verfügbar, die mit einer Eigenschaft gemacht wird für die `WebViewPage` Basisklasse. Die `ActionLink` Methode des Hilfsprogramms ganz einfach zum dynamischen Generieren von HTML-Links, die mit Aktionsmethoden im Controller verknüpft. Das erste Argument für die `ActionLink` Methode ist der Text des Links zum Rendern (z. B. `<a>Edit Me</a>`). Das zweite Argument ist der Name der aufzurufenden Aktionsmethode. Das letzte Argument ist ein [anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , generiert die Routendaten (in diesem Fall wird die ID des 4).

Der generierte Link in der vorherigen Abbildung dargestellt ist `http://localhost:xxxxx/Movies/Edit/4`. Die Standardroute akzeptiert das URL-Muster `{controller}/{action}/{id}`. Aus diesem Grund ASP.NET übersetzt `http://localhost:xxxxx/Movies/Edit/4` in eine Anforderung an die `Edit` Aktionsmethode des der `Movies` Controller mit dem Parameter `ID` gleich 4.

Sie können auch die Aktionsmethodenparameter über eine Abfragezeichenfolge übergeben. Beispielsweise die URL `http://localhost:xxxxx/Movies/Edit?ID=4` übergibt außerdem den Parameter `ID` 4 die `Edit` Aktionsmethode von der `Movies` Controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Öffnen der `Movies` Controller. Die beiden `Edit` Aktionsmethoden unten angezeigt.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist. Dieses Attribut gibt an, diese Überladung von der `Edit` Methode kann nur für die POST-Anforderungen aufgerufen werden. Sie können Werte anwenden der `HttpGet` Attribut mit dem ersten bearbeiten Methode, aber dies ist nicht erforderlich, da dies die Standardeinstellung ist. (Verweisen wir auf Aktionsmethoden, die implizit zugewiesen sind die `HttpGet` Attribut `HttpGet` Methoden.)

Die `HttpGet` `Edit` Methode nimmt die Film-ID-Parameter, mit dem Entity Framework Film sucht `Find` -Methode, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück. Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt. Das folgende Beispiel zeigt die Bearbeitungsansicht, die generiert wurde:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Beachten Sie, wie die Vorlage für die Sicht hat eine `@ModelType MvcMovie.Models.Movie` -Anweisung am Anfang der Datei – Dies gibt an, dass die Sicht erwartet, das Modell für die Vorlage Sicht vom Typ dass `Movie`.

Der scaffolded Code verwendet mehrere *Hilfsmethoden* um das HTML-Markup zu optimieren. Die [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Hilfsprogramm zeigt den Namen des Felds (&quot;Titel&quot;, &quot;ReleaseDate&quot;, &quot;"Genre"&quot;, oder &quot;Preis &quot;). Die [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Hilfsprogramm zeigt eine HTML `<input>` Element. Die [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Hilfsprogramm zeigt Fehlermeldungen Validierung dieser Eigenschaft zugeordnet.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Klicken Sie auf einen Link **Bearbeiten**. Zeigen Sie im Browser den Quelltext für die Seite an. Der HTML-Code auf der Seite sieht wie im folgenden Beispiel. (Das Menü Markup wurde aus Gründen der Übersichtlichkeit ausgeschlossen.)

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

Die `<input>` Elemente sind in einem HTML- `<form>` Element, dessen `action` -Attribut festgelegt ist, an die */Filme/bearbeiten* URL. Die Formulardaten werden an den Server gesendet werden bei der **bearbeiten** geklickt wird.

## <a name="processing-the-post-request"></a>Verarbeiten der POST-Anforderung

Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Die modellbindung von ASP.NET Framework nimmt die übermittelte Formularwerte und erstellt eine `Movie` -Objekt, das als übergeben wird, die `movie` Parameter. Die `ModelState.IsValid` im Code wird überprüft, dass die Daten in das Formular übermittelt verwendet werden können, so ändern Sie eine `Movie` Objekt. Wenn die Daten gültig ist, wird der Code speichert die Filmdaten auf die `Movies` Auflistung von der `MovieDBContext` Instanz. Der Code speichert dann die neue Filmdaten in der Datenbank durch Aufrufen der `SaveChanges` Methode `MovieDBContext`, die weiterhin Änderungen an der Datenbank besteht. Nach dem Speichern der Daten an, der Code leitet den Benutzer die `Index` Aktionsmethode von der `MoviesController` -Klasse, die bewirkt, dass den aktualisierte Film, die in der Liste von Filmen angezeigt werden.

Wenn die bereitgestellten Werte nicht gültig sind, werden sie in das Formular erneut angezeigt. Die `Html.ValidationMessageFor` Hilfen in den *Edit.vbhtml* anzeigen, die Vorlage zum Anzeigen von entsprechenden Fehlermeldungen achten.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Beachten Sie, Informationen zu Gebietsschemas** Sie normalerweise mit einem anderen Gebietsschema als Englisch arbeiten, finden Sie unter [Unterstützung von ASP.NET MVC 3-Validierung mit nicht englischen Gebietsschemas.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Die Bearbeitungsmethode machen unter Umständen stabiler

Die `HttpGet` `Edit` Methode, die vom System Gerüstbau generiert prüft nicht, dass die ID, die an sie übergeben wird, gültig ist. Wenn ein Benutzer das ID-Segment aus der URL entfernt (`http://localhost:xxxxx/Movies/Edit`), wird die folgende Fehlermeldung angezeigt:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Benutzer konnte auch übergeben, eine ID, die in der Datenbank, z. B. vorhanden ist `http://localhost:xxxxx/Movies/Edit/1234`. Sie können zwei Änderungen, die `HttpGet` `Edit` Aktionsmethode Überwindung dieser Einschränkung. Ändern Sie zunächst die `ID` Parameter einen Standardwert von 0 (null) sind, wenn Sie explizit eine ID übergeben wird. Sie können auch überprüfen, die die `Find` Methode gefunden tatsächlich einen Film vor der Rückgabe des Movie-Objekts an der Vorlage anzeigen. Die aktualisierte `Edit` Methode wird unten gezeigt.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Wenn keine Film gefunden wird, die `HttpNotFound` -Methode aufgerufen wird.

Alle der `HttpGet` Methoden einem ähnliches Muster folgen. Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten, in der Groß-/Kleinschreibung `Index`), und übergeben Sie das Modell zur Ansicht. Die `Create` Methode übergibt ein leeres Movie-Objekt an die Erstellungsansicht. Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode. Ändern von Daten in einer HTTP GET-Methode ist ein Sicherheitsrisiko, gemäß der in der Post-Blogeintrag [ASP.NET MVC Tipp #46 – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Ändern von Daten in einer GET-Methode verletzt HTTP bewährte Methoden und die REST-Architekturschema, der angibt, dass die GET-Anforderungen sollten nicht den Status Ihrer Anwendung ändern. Ein GET-Vorgang sollte also einen sicheren Operation sein, die keine Nebeneffekte hat.

## <a name="adding-a-search-method-and-search-view"></a>Hinzufügen eines Search-Methode und des Anzeigebereichs

In diesem Abschnitt fügen Sie eine `SearchIndex` Aktionsmethode, mit dem Sie suchen, Filme nach Genre oder Namen. Dieser Wert ist, mithilfe der */Filme/SearchIndex* URL. Die Anforderung wird ein HTML-Formular angezeigt, die Eingabe Elemente, die ein Benutzer ausfüllen kann enthält, um einen Film zu suchen. Wenn ein Benutzer das Formular sendet, wird die Aktionsmethode rufen Sie die Search-Werte, die vom Benutzer bereitgestellt und verwenden Sie die Werte, um die Datenbank zu suchen.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Anzeigen des Formulars SearchIndex

Starten Sie durch Hinzufügen einer `SearchIndex` Aktionsmethode zur vorhandenen `MoviesController` Klasse. Die Methode gibt eine Ansicht zurück, die einem HTML-Formular enthält. Hier ist der Code ein:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

Die erste Zeile der `SearchIndex` -Methode erstellt die folgenden [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) Abfrage Filme auswählen:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

Die Abfrage wird an diesem Punkt definiert, jedoch noch nicht für den Datenspeicher noch ausgeführt.

Wenn die `searchString` -Parameters enthält eine Zeichenfolge, die Filme Abfrage wurde geändert, um auf den Wert der Suchzeichenfolge, mithilfe des folgenden Codes zu filtern:

Wenn dies nicht der String.IsNullOrEmpty(searchString) dann   
 Filme = Filme. Wobei (ausgeschlossener s.Title.Contains(searchString))   
 Beenden, wenn

LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert sind oder wenn sie geändert werden, durch Aufrufen einer Methode wie z. B. `Where` oder `OrderBy`. Stattdessen Ausführung der Abfrage wird verzögert, was bedeutet, dass die Auswertung eines Ausdrucks verzögert wird, bis der realisierte Wert tatsächlich in einer Schleife durchlaufen wird oder die [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) -Methode aufgerufen wird. In der `SearchIndex` Sample, die Abfrage in der Ansicht SearchIndex ausgeführt wird. Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](https://msdn.microsoft.com/library/bb738633.aspx).

Nachdem Sie implementieren können, die `SearchIndex` anzeigen, die der Benutzer das Formular angezeigt wird. Mit der rechten Maustaste innerhalb der `SearchIndex` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**. In der **Ansicht hinzufügen** Dialogfeld geben, dass Sie die offensichtlichen übergeben ein `Movie` Objekt, das die der Ansichtenvorlage als der Modellklasse. In der **Gerüst Vorlage** wählen **Liste**, klicken Sie dann auf **hinzufügen**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Beim Klicken auf die **hinzufügen** Schaltfläche, die *Views\Movies\SearchIndex.vbhtml* Ansichtenvorlage erstellt wurde. Da Sie ausgewählt haben **Liste** in der **Gerüst Vorlage** aufzulisten, Visual Web Developer automatisch generiert (Gerüstbau) einige Standardinhalt in der Ansicht. Das Gerüst erstellt eine HTML-Formular. Er überprüft die `Movie` Klasse und erstellte Code zum Rendern `<label>` Elemente für jede Eigenschaft der Klasse. Die Liste unten zeigt die Ansicht für das Erstellen, das generiert wurde:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Fügen Sie eine Abfragezeichenfolge wie `?searchString=ghost` an die URL an. Die gefilterten Filme werden angezeigt.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Wenn Sie die Signatur der Ändern der `SearchIndex` Methode zu einem Parameter mit dem Namen `id`, `id` Parameter entspricht der `{id}` Platzhalter für den standardmäßigen leitet Menge in der *"Global.asax"* Datei.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Das geänderte `SearchIndex` Methode würde wie folgt aussehen:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten. Ja, nachdem Sie Hilfe der Benutzeroberfläche hinzufügen, sie filtern, Filme. Wenn Sie die Signatur der geändert und die `SearchIndex` IMM Methode zum Testen wie die Route datengebundenen-ID-Parameter übergeben, damit Ihre `SearchIndex` Methode nimmt einen Zeichenfolgenparameter, der mit dem Namen `searchString`:

Öffnen der *Views\Movies\SearchIndex.vbhtml* , und stellen Sie direkt nach `@Html.ActionLink("Create New", "Create")`, fügen Sie Folgendes hinzu:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

Die `Html.BeginForm` Hilfsprogramm erstellt ein öffnendes `<form>` Tag. Die `Html.BeginForm` Helper bewirkt, dass das Formular an sich selbst zu senden, wenn der Benutzer durch Klicken auf das Formular sendet die **Filter** Schaltfläche.

Führen Sie die Anwendung, und versuchen Sie es für einen Film suchen.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Es ist keine `HttpPost` Überladung von der `SearchIndex` Methode. Nicht erforderlich, da die Methode ändern den Status der Anwendung ist nicht nur das Filtern von Daten. Wenn Sie die folgenden hinzugefügt `HttpPost` `SearchIndex` -Methode, die Instanz zum Aufrufen der Aktion entsprechen den `HttpPost` `SearchIndex` -Methode, und die `HttpPost` `SearchIndex` Methode würde ausgeführt, wie in der folgenden Abbildung dargestellt.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Suche nach "Genre" hinzufügen

Wenn Sie hinzugefügt haben die `HttpPost` Version der `SearchIndex` -Methode, löschen Sie es jetzt.

Als Nächstes fügen Sie eine Funktion zum Benutzer, die für Filme nach Genre suchen können. Ersetzen Sie die `SearchIndex`-Methode durch folgenden Code:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Diese Version von den `SearchIndex` Methode nimmt einen zusätzlichen Parameter, nämlich `movieGenre`. Erstellen Sie die ersten Zeilen des Codes eine `List` Objekt um Filmgenres aus der Datenbank zu speichern.

Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Der Code verwendet die `AddRange` der generischen Methode `List` Auflistung, die unterschiedliche Genres zur Liste hinzugefügt werden. (Ohne die `Distinct` Modifizierer, doppelte Genres würde hinzugefügt – zweimal in unserem Beispiel würde z. B. Comedy hinzugefügt werden). Der Code speichert dann die Liste von Genres in die `ViewBag` Objekt.

Der folgende Code zeigt die Vorgehensweise beim Überprüfen der `movieGenre` Parameter. Wenn er nicht leer ist schränkt der Code weiter Filme Abfrage ausgewählten Filme an der angegebenen "Genre" beschränken.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Die Ansicht SearchIndex zur Unterstützung der Suche nach "Genre" Markup hinzufügen

Hinzufügen einer `Html.DropDownList` Hilfsmethode zum der *Views\Movies\SearchIndex.vbhtml* Datei, direkt vor der `TextBox` Helper. Das vollständige Markup wird unten gezeigt:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Versuchen Sie eine Suche durch "Genre" Movie namentlich und von beiden Kriterien.

In diesem Abschnitt untersuchen Sie die CRUD-Aktionsmethoden und Ansichten, die vom Framework generierten. Sie erstellt eine Aktionsmethode suchen und anzeigen, mit denen Benutzer anhand der Filmtitel und die "Genre" suchen kann. Im nächsten Abschnitt, betrachten Sie zum Hinzufügen einer Eigenschaft, um die `Movie` Modell und wie Sie einen Initialisierer hinzufügen, die automatisch eine Testdatenbank erstellen.

>[!div class="step-by-step"]
[Zurück](accessing-your-models-data-from-a-controller.md)
[Weiter](adding-a-new-field.md)
