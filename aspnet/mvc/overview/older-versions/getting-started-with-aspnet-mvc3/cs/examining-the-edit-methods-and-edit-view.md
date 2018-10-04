---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Überprüfen der Edit-Methoden und-Ansicht (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 6ed989173f7f687e37c73b89217b1cd81e056f75
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578210"
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Überprüfen der Edit-Methoden und-Ansicht (c#)
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.
> 
> 
> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


In diesem Abschnitt müssen Sie die generierte Aktionsmethoden und Ansichten für die Movie-Controller untersuchen. Anschließend fügen Sie eine benutzerdefinierte Suchseite.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in die Adressleiste Ihres Browsers. Halten Sie den Mauszeiger über ein **bearbeiten** Link, um die URL anzuzeigen, die es verknüpft.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

Die **bearbeiten** Link wurde generiert, indem die `Html.ActionLink` -Methode in der die *Views\Movies\Index.cshtml* anzeigen:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

Die `Html` Objekt ist eine Hilfsklasse, die verfügbar, die mithilfe einer Eigenschaft gemacht wird für die `WebViewPage` Basisklasse. Die `ActionLink` Methode des Hilfsprogramms erleichtert das HTML-Links dynamisch zu generieren, die auf Aktionsmethoden in Controllern verknüpft sind. Das erste Argument für die `ActionLink` Methode ist der Text des Links zum Rendern (z. B. `<a>Edit Me</a>`). Das zweite Argument ist der Name der aufzurufenden Aktionsmethode. Das letzte Argument ist ein [anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , generiert die Routendaten (in diesem Fall wird die ID von 4).

Der generierte Link in der vorherigen Abbildung dargestellt ist `http://localhost:xxxxx/Movies/Edit/4`. Die Standardroute verwendet das URL-Muster `{controller}/{action}/{id}`. Aus diesem Grund ASP.NET übersetzt `http://localhost:xxxxx/Movies/Edit/4` in eine Anforderung an die `Edit` Action-Methode der der `Movies` Controller mit dem Parameter `ID` gleich 4.

Sie können auch die Aktionsmethodenparameter, die mithilfe einer Abfragezeichenfolge übergeben. Z. B. die URL `http://localhost:xxxxx/Movies/Edit?ID=4` übergibt außerdem den Parameter `ID` 4 die `Edit` Action-Methode der der `Movies` Controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Öffnen der `Movies` Controller. Die beiden `Edit` Aktionsmethoden werden unten angezeigt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist. Dieses Attribut gibt an, diese Überladung von der `Edit` Methode kann nur für die POST-Anforderungen aufgerufen werden. Sie können Anwenden der `HttpGet` -Attribut auf die erste Bearbeitungsmethode, aber dies ist nicht erforderlich, da dies die Standardeinstellung ist. (Wir werden den finden Sie in Aktionsmethoden, die implizit zugewiesen sind die `HttpGet` als Attribut `HttpGet` Methoden.)

Die `HttpGet` `Edit` Methode akzeptiert den Film-ID-Parameter, sucht den Film mithilfe von Entity Framework `Find` -Methode, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück. Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt. Das folgende Beispiel zeigt die Bearbeitungsansicht, die generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Beachten Sie, wie die ansichtsvorlage verfügt über eine `@model MvcMovie.Models.Movie` -Anweisung am Anfang der Datei – gibt an, dass die Ansicht erwartet, das Modell für die ansichtsvorlage dass Typ `Movie`.

Der Gerüstcode verwendet mehrere *Hilfsmethoden* , das HTML-Markup zu optimieren. Die [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Hilfsprogramm zeigt den Namen des Felds ("Title", "ReleaseDate", "Genre" oder "Price"). Die [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Hilfsprogramm zeigt eine HTML `<input>` Element. Die [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Hilfsprogramm zeigt alle validierungsnachrichten dieser Eigenschaft zugeordnet.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Klicken Sie auf einen Link **Bearbeiten**. Zeigen Sie im Browser den Quelltext für die Seite an. Der HTML-Code auf der Seite sieht wie im folgenden Beispiel aus. (Das Menü-Markup wurde aus Gründen der Übersichtlichkeit ausgeschlossen.)

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

Die `<input>` Elemente sind in einem HTML- `<form>` Element, dessen `action` -Attribut festgelegt ist, zum an den */Filme/bearbeiten* URL. Die Formulardaten werden an den Server zurückgesendet werden bei der **bearbeiten** geklickt wird.

## <a name="processing-the-post-request"></a>Verarbeiten der POST-Anforderung

Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Die modellbindung von ASP.NET Framework bereitgestellten Formularwerte und erstellt eine `Movie` -Objekt, das als übergeben wird die `movie` Parameter. Die `ModelState.IsValid` im Code wird überprüft, die im Formular übermittelten Daten verwendet werden können, so ändern Sie eine `Movie` Objekt. Wenn die Daten gültig ist, wird der Code speichert die Filmdaten zu den `Movies` Auflistung von der `MovieDBContext` Instanz. Der Code speichert dann die neuen Daten in der Datenbank durch Aufrufen der `SaveChanges` -Methode der `MovieDBContext`, die Änderungen an der Datenbank beibehalten. Nach dem Speichern der das, leitet der Code den Benutzer auf die `Index` Action-Methode der der `MoviesController` -Klasse, die bewirkt, dass den aktualisierte Film, die in der Liste von Filmen angezeigt werden.

Wenn die bereitgestellten Werte nicht gültig sind, werden sie in das Formular erneut angezeigt. Die `Html.ValidationMessageFor` -Hilfsprogramme in den *Edit.cshtml* anzeigen, die Vorlage zum Anzeigen von entsprechende Fehlermeldungen kümmern.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Hinweis zu Gebietsschemas** , wenn Sie sich normalerweise mit einem anderen Gebietsschema als Englisch arbeiten, finden Sie unter [Unterstützung von ASP.NET MVC 3-Validierung mit nicht englischen Gebietsschemas.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Gestalten Sie die Bearbeitungsmethode stabiler

Die `HttpGet` `Edit` vom System Gerüstbau generierte Methode prüft nicht, dass die ID, die an ihn übergeben wird, gültig ist. Wenn ein Benutzer das ID-Segment aus der URL entfernt (`http://localhost:xxxxx/Movies/Edit`), wird der folgende Fehler angezeigt:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Benutzer konnte auch übergeben, eine ID, die in der Datenbank, z. B. vorhanden ist `http://localhost:xxxxx/Movies/Edit/1234`. Sie können zwei Änderungen vornehmen der `HttpGet` `Edit` Action-Methode, um diese Einschränkung zu beheben. Ändern Sie zunächst die `ID` Parameter einen Standardwert von 0 (null) sind, wenn Sie explizit eine ID übergeben wird. Sie können auch überprüfen, die die `Find` Methode tatsächlich einen Film gefunden, vor der Rückgabe der Movie-Objekt, das die ansichtsvorlage. Die aktualisierte `Edit` Methode wird unten angezeigt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Wenn keine Film gefunden wird, die `HttpNotFound` Methode wird aufgerufen.

Alle der `HttpGet` Methoden folgen einem ähnlichen Muster. Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten, die im Fall von `Index`), und übergeben Sie das Modell an die Ansicht. Die `Create` Methode übergibt ein leeres Movie-Objekt, an der Ansicht "erstellen". Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode. Ändern von Daten in eine HTTP GET-Methode ein Sicherheitsrisiko dar, ist, wie in der Post-Blogeintrag beschrieben [ASP.NET MVC Tipp #46 – verwenden Sie keine Links löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Ändern von Daten in eine GET-Methode verstößt auch gegen bewährte HTTP-Methoden und das architektonische REST-Muster, das gibt an, dass die GET-Anforderungen nicht den Status Ihrer Anwendung ändern sollten. Ausführen einer GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebeneffekte hat.

## <a name="adding-a-search-method-and-search-view"></a>Hinzufügen eines Search-Methode und des Anzeigebereichs

In diesem Abschnitt fügen Sie eine `SearchIndex` Aktionsmethode, mit der Sie, suchen, Filme nach Genre oder Namen. Diese werden die verfügbaren mithilfe der */Filme/SearchIndex* URL. Die Anforderung wird ein HTML-Formular angezeigt, die input-Elemente, die ein Benutzer ausfüllen können enthält, um nach einem Film zu suchen. Wenn ein Benutzer das Formular sendet, wird die Action-Methode rufen Sie die Search-Werte, die vom Benutzer bereitgestellt und verwenden Sie die Werte, um die Datenbank zu suchen.

## <a name="displaying-the-searchindex-form"></a>Anzeigen des Formulars SearchIndex

Starten Sie durch das Hinzufügen einer `SearchIndex` Aktionsmethode mit dem vorhandenen `MoviesController` Klasse. Die Methode gibt eine Ansicht zurück, die ein HTML-Formular enthält. Hier ist der Code ein:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

Die erste Zeile der `SearchIndex` Methode erstellt die folgenden [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) Abfrage zum Auswählen der Filme:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

Die Abfrage wird an diesem Punkt definiert, aber noch nicht noch für den Datenspeicher ausgeführt wurde.

Wenn die `searchString` Parameter eine Zeichenfolge enthält, wird die filmabfrage geändert, um den Wert der Suchzeichenfolge, mit dem folgenden Code zu filtern:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

LINQ-Abfragen werden nicht ausgeführt werden, wenn sie definiert werden, oder wenn sie geändert werden, durch Aufrufen einer Methode wie z. B. `Where` oder `OrderBy`. Stattdessen wird die abfrageausführung verzögert, was bedeutet, dass die Auswertung eines Ausdrucks hinausgezögert wird, bis dessen realisierte Wert tatsächlich durchlaufen oder die [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) Methode wird aufgerufen. In der `SearchIndex` Beispiel die Abfrage in der Ansicht SearchIndex ausgeführt wird. Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](https://msdn.microsoft.com/library/bb738633.aspx).

Nachdem Sie implementieren können, die `SearchIndex` anzeigen, die das Formular für den Benutzer angezeigt werden. Klicken Sie auf auf die `SearchIndex` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**. In der **Ansicht hinzufügen** Dialogfeld geben, dass man übergeben eine `Movie` Objekt, das die ansichtsvorlage wie die Model-Klasse. In der **Gerüst Vorlage** wählen **Liste**, klicken Sie dann auf **hinzufügen**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Beim Klicken auf die **hinzufügen** Schaltfläche der *Views\Movies\SearchIndex.cshtml* ansichtsvorlage erstellt wird. Da Sie ausgewählt haben **Liste** in die **Gerüst Vorlage** aufzulisten, Visual Web Developer automatisch generiert (Gerüstbau) einige Standardinhalte in der Ansicht. Das Gerüst erstellt ein HTML-Formular. Er untersucht die `Movie` -Klasse und der erstellte Code zum Rendern `<label>` Elemente für jede Eigenschaft der Klasse. Die folgenden Liste zeigt die Ansicht "erstellen", die generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Fügen Sie eine Abfragezeichenfolge wie `?searchString=ghost` an die URL an. Die gefilterten Filme werden angezeigt.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Wenn Sie die Signatur des Ändern der `SearchIndex` Methode zu einem Parameter mit dem Namen `id`, wird die `id` Parameter entspricht der `{id}` Platzhalter für den standardmäßigen leitet Menge in der *"Global.asax"* Datei.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Die geänderte `SearchIndex` Methode würde wie folgt aussehen:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten. Nun Sie Benutzeroberfläche helfen hinzufügen, diese Filme zu filtern. Wenn Sie die Signatur des geändert haben die `SearchIndex` Methode zum Testen den routengebundenen ID-Parameter übergeben. ändern Sie ihn, damit Ihre `SearchIndex` Methode nimmt einen Zeichenfolgenparameter namens `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Öffnen der *Views\Movies\SearchIndex.cshtml* , und stellen Sie direkt nach `@Html.ActionLink("Create New", "Create")`, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

Das folgende Beispiel zeigt einen Teil der *Views\Movies\SearchIndex.cshtml* Datei hinzugefügte Markup filtern.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

Die `Html.BeginForm` Hilfsprogramm erstellt ein öffnendes `<form>` Tag. Die `Html.BeginForm` Helper wird das Formular an sich selbst zurückgesendet werden soll, wenn der Benutzer das Formular durch Klicken auf sendet die **Filter** Schaltfläche.

Führen Sie die Anwendung, und versuchen Sie es nach einem Film suchen.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Es gibt keine `HttpPost` Überladung von der `SearchIndex` Methode. Sie erforderlich nicht, da die Methode den Zustand der Anwendung ändern, ist nicht nur das Filtern von Daten.

Sie können die folgende `HttpPost SearchIndex`-Methode hinzufügen. In diesem Fall die Instanz zum Aufrufen der Aktion entsprechen der `HttpPost SearchIndex` -Methode, und die `HttpPost SearchIndex` Methode wird ausgeführt, wie in der folgenden Abbildung dargestellt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Doch selbst wenn Sie diese `HttpPost`-Version der `SearchIndex`-Methode hinzufügen, gibt es eine Einschränkung für die gesamte Implementierung. Stellen Sie sich vor, Sie möchten eine bestimmte Suche als Favorit speichern oder einen Link an Freunde senden, auf den diese klicken können, um dieselbe gefilterte Liste von Filmen anzuzeigen. Beachten Sie, dass die URL für die HTTP-POST-Anforderung die URL für die GET-Anforderung (localhost: xxxxx/Filme/SearchIndex) identisch ist – es keine Suchinformationen in der URL selbst sind. Rechts wird Informationen in der Suchzeichenfolge nun als einen Formularfeldwert an den Server gesendet. Dies bedeutet, dass Sie diese Suchinformationen Lesezeichen oder Senden an Freunde, in einer URL nicht erfassen können.

Die Lösung besteht darin, eine Überladung verwenden `BeginForm` , der angibt, dass die POST-Anforderung die Informationen für die Suche der URL hinzufügen sollte und das ist sollte weitergeleitet, auf die HttpGet-Version von der `SearchIndex` Methode. Ersetzen Sie die vorhandene parameterlosen `BeginForm` -Methode durch Folgendes:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Wenn Sie eine Suche übermitteln, enthält die URL jetzt eine Suchzeichenfolge für die Abfrage an. Das Suchen wird auch an Aktionsmethode `HttpGet SearchIndex` übertragen, auch wenn Sie eine `HttpPost SearchIndex`-Methode haben.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Hinzufügen der Suche nach Genre

Wenn Sie hinzugefügt haben die `HttpPost` Version der `SearchIndex` -Methode, löschen Sie sie jetzt.

Als Nächstes fügen Sie eine Funktion, damit Benutzer, die für Filme nach Genre suchen können. Ersetzen Sie die `SearchIndex`-Methode durch folgenden Code:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Diese Version von der `SearchIndex` Methode nimmt einen zusätzlichen Parameter, nämlich `movieGenre`. Erstellen Sie die ersten Zeilen des Codes eine `List` Objekt zum Speichern der Filmgenres aus der Datenbank.

Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

Der Code verwendet die `AddRange` Methode der generischen `List` Auflistung, die alle der unterschiedlichen Genres zur Liste hinzugefügt. (Ohne die `Distinct` Modifizierer, doppelte Genres hinzugefügt – zweimal in unserem Beispiel würde z. B. Komödie hinzugefügt werden). Der Code speichert dann die Liste der Genres in die `ViewBag` Objekt.

Der folgende Code zeigt, wie Sie überprüfen die `movieGenre` Parameter. Wenn sie nicht leer ist schränkt der Code weiter die filmabfrage, um die ausgewählten Filme, die bestimmten Genre zu beschränken.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Hinzufügen von Markup zur SearchIndex Ansicht, um die Unterstützung der Suche nach Genre

Hinzufügen einer `Html.DropDownList` Hilfsmethode zum die *Views\Movies\SearchIndex.cshtml* Datei kurz vor dem Ausführen der `TextBox` Helper. Das vollständige Markup wird unten gezeigt:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu */Filme/SearchIndex*. Versuchen Sie eine Suche nach Genre, Filmname und von beiden Kriterien.

In diesem Abschnitt überprüft Sie die CRUD-Aktionsmethoden und Ansichten, die durch das Framework generiert. Sie erstellt eine Suchmethode für Aktion und Ansicht, die Benutzern das Suchen von Filmtitel und Genre zu ermöglichen. Im nächsten Abschnitt, betrachten Sie eine Eigenschaft zum Hinzufügen der `Movie` Modell und einen Initialisierer hinzufügen, die automatisch eine Testdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](accessing-your-models-data-from-a-controller.md)
> [Weiter](adding-a-new-field.md)
