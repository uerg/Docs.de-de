---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Prüfen der Methoden bearbeiten und die Bearbeitungsansicht | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a3baa8e9af572d4c21813218ba394715a6db65cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Prüfen der Methoden bearbeiten und die Bearbeitungsansicht
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In diesem Abschnitt werden Sie untersucht den generierten `Edit` Aktionsmethoden und Ansichten für den Film-Controller. Doch zunächst dauert eine kurze Abzweigung zu dem Veröffentlichungsdatum zu optimieren. Öffnen der *Models\Movie.cs* Datei, und fügen Sie den folgenden hervorgehobenen Zeilen hinzu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Sie können auch die Datumskultur wie folgt bestimmte vornehmen:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Wir behandeln [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) im nächsten Tutorial. Das [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“). Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribut gibt den Typ der Daten, die in diesem Fall ist es ein Datum ist, damit die Informationen gespeichert werden, in das Feld nicht angezeigt wird. Die [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut ist erforderlich, für einen Programmfehler in Chrome-Browser, die Datums-und Uhrzeitformate falsch gerendert wird.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller. Halten Sie den Mauszeiger über ein **bearbeiten** Link, um die URL anzuzeigen, die es verknüpft.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Die **bearbeiten** Link wurde generiert, indem die `Html.ActionLink` Methode in der *Views\Movies\Index.cshtml* anzeigen:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Die `Html` Objekt ist ein Hilfsprogramm, das verfügbar, die mit einer Eigenschaft gemacht wird für die [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) Basisklasse. Die `ActionLink` Methode des Hilfsprogramms ganz einfach zum dynamischen Generieren von HTML-Links, die mit Aktionsmethoden im Controller verknüpft. Das erste Argument für die `ActionLink` Methode ist der Text des Links zum Rendern (z. B. `<a>Edit Me</a>`). Das zweite Argument ist der Name der aufzurufenden Aktionsmethode (In diesem Fall die `Edit` Aktion). Das letzte Argument ist ein [anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , generiert die Routendaten (in diesem Fall wird die ID des 4).

Der generierte Link in der vorherigen Abbildung dargestellt ist `http://localhost:1234/Movies/Edit/4`. Die Standardroute (in vielen Branchen *App\_Start\RouteConfig.cs*) nimmt das URL-Muster `{controller}/{action}/{id}`. Aus diesem Grund ASP.NET übersetzt `http://localhost:1234/Movies/Edit/4` in eine Anforderung an die `Edit` Aktionsmethode des der `Movies` Controller mit dem Parameter `ID` gleich 4. Überprüfen Sie den folgenden Code aus der *App\_Start\RouteConfig.cs* Datei. Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) Methode wird verwendet, um HTTP-Anforderungen an die richtige Methode der Controller- und weiterleiten, und geben Sie den optionalen ID-Parameter. Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) Methode dient auch durch die [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) wie z. B. `ActionLink` zum Generieren von URLs anhand des Controllers, Aktionsmethode und keine Routendaten.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Sie können auch die Aktionsmethodenparameter über eine Abfragezeichenfolge übergeben. Beispielsweise die URL `http://localhost:1234/Movies/Edit?ID=3` übergibt außerdem den Parameter `ID` 3 der `Edit` Aktionsmethode von der `Movies` Controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Öffnen der `Movies` Controller. Die beiden `Edit` Aktionsmethoden unten angezeigt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist. Dieses Attribut gibt an, dass die Überladung von der `Edit` Methode kann nur für die POST-Anforderungen aufgerufen werden. Sie können Werte anwenden der `HttpGet` Attribut mit dem ersten bearbeiten Methode, aber dies ist nicht erforderlich, da dies die Standardeinstellung ist. (Verweisen wir auf Aktionsmethoden, die implizit zugewiesen sind die `HttpGet` Attribut `HttpGet` Methoden.) Die [binden](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) Attribut ist eine andere wichtige Sicherheitsmechanismus, der verhindert, dass Hacker zu stark Senden von Daten mit dem Modell. Sie sollten nur Eigenschaften in der Bind-Attribut enthalten, die Sie ändern möchten. Informieren Sie sich über Overposting und die Bind-Attribut in meinem [overposting Sicherheitshinweis](https://go.microsoft.com/fwlink/?LinkId=317598). In dem einfachen Wiederherstellungsmodell, die in diesem Lernprogramm verwendet wird werden alle Daten im Modell gebunden werden. Die [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) Attribut wird verwendet, um zu verhindern, dass Fälschung einer Anforderung und wird mit-Rückrufvertrag `@Html.AntiForgeryToken()` in die Ansichtsdatei bearbeiten (*Views\Movies\Edit.cshtml*), ein Teil wird unten gezeigt:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` generiert eine verborgene antifälschungs-Token, das im übereinstimmen muss die `Edit` Methode der `Movies` Controller. Erfahren Sie mehr über Cross-Site request Fälschung (auch bekannt als XSRF oder FORGERY) in meinem Lernprogramm [XSRF/vom CSRF-Schutz in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Die `HttpGet` `Edit` Methode nimmt die Film-ID-Parameter, mit dem Entity Framework Film sucht `Find` -Methode, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück. Wenn Sie ein Film nicht gefunden werden kann, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) wird zurückgegeben. Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt. Das folgende Beispiel zeigt die Bearbeitungsansicht, die von der visual Studio-Gerüstbau-System generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Beachten Sie, wie die Vorlage für die Sicht hat eine `@model MvcMovie.Models.Movie` -Anweisung am Anfang der Datei – Dies gibt an, dass die Sicht erwartet, das Modell für die Vorlage Sicht vom Typ dass `Movie`.

Der scaffolded Code verwendet mehrere *Hilfsmethoden* um das HTML-Markup zu optimieren. Die [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Hilfsprogramm zeigt den Namen des Felds (&quot;Titel&quot;, &quot;ReleaseDate&quot;, &quot;"Genre"&quot;, oder &quot;Preis &quot;). Die [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) -Hilfsmethode rendert ein HTML `<input>` Element. Die [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Hilfsprogramm zeigt Fehlermeldungen Validierung dieser Eigenschaft zugeordnet.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Klicken Sie auf einen Link **Bearbeiten**. Zeigen Sie im Browser den Quelltext für die Seite an. Der HTML-Code für das Form-Element wird unten gezeigt.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Die `<input>` Elemente sind in einem HTML- `<form>` Element, dessen `action` -Attribut festgelegt ist, an die */Filme/bearbeiten* URL. Die Formulardaten werden an den Server gesendet werden bei der **speichern** geklickt wird. Die zweite Linie zeigt die ausgeblendeten [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generierte Token durch die `@Html.AntiForgeryToken()` aufrufen.

## <a name="processing-the-post-request"></a>Verarbeiten der POST-Anforderung

Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Die [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) Attribut überprüft die [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generierte Token durch die `@Html.AntiForgeryToken()` in der Ansicht aufrufen.

Die [ASP.NET MVC-modellbindung](https://msdn.microsoft.com/library/dd410405.aspx) nimmt die übermittelte Formularwerte und erstellt eine `Movie` -Objekt, das als übergeben wird, die `movie` Parameter. Die `ModelState.IsValid`-Methode stellt sicher, dass die im Formular übermittelten Daten verwendet werden können, um ein `Movie`-Objekt zu ändern (bearbeiten oder aktualisieren). Wenn die Daten gültig ist, wird die Filmdaten gespeichert, auf die `Movies` Auflistung von der `db(MovieDBContext` Instanz). Die neue Filmdaten in der Datenbank gespeichert ist, durch Aufrufen der `SaveChanges` Methode `MovieDBContext`. Nach dem Speichern der Daten leitet der Code den Benutzer an die `Index`-Aktionsmethode der `MoviesController`-Klasse weiter, die die Filmsammlung einschließlich der gerade vorgenommenen Änderungen anzeigt.

Sobald die clientseitige Validierung, dass die Werte eines Felds nicht gültig sind feststellt, wird eine Fehlermeldung angezeigt. Wenn Sie JavaScript deaktivieren, Sie erhalten keinen clientseitige Validierung, aber der Server erkennt die bereitgestellten Werte sind ungültig und werden wieder die Formularwerte mit Fehlermeldungen angezeigt. Später in diesem Lernprogramm untersuchen wir die Überprüfung im Detail.

Die `Html.ValidationMessageFor` Hilfen in den *Edit.cshtml* anzeigen, die Vorlage zum Anzeigen von entsprechenden Fehlermeldungen achten.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Alle der `HttpGet` Methoden einem ähnliches Muster folgen. Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten, in der Groß-/Kleinschreibung `Index`), und übergeben Sie das Modell zur Ansicht. Die `Create` Methode übergibt ein leeres Movie-Objekt an die Erstellungsansicht. Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode. Ändern von Daten in einer HTTP GET-Methode ist ein Sicherheitsrisiko, gemäß der in der Post-Blogeintrag [ASP.NET MVC Tipp #46 – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Ändern von Daten in einer GET-Methode auch verletzt, bewährte Methoden für HTTP und das architektonische [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) Muster, der angibt, dass die GET-Anforderungen sollten nicht den Status Ihrer Anwendung ändern. Die Durchführung eines GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebenwirkungen hat und die permanenten Daten nicht ändert.

Wenn Sie einen US-Englisch-Computer verwenden, können Sie überspringen Sie diesen Abschnitt und wechseln Sie zum nächsten Lernprogramm. Sie können die Globalize Version dieses Lernprogramms herunterladen [hier](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Eine hervorragende auf Internationalisierung zweiteiligen-Lernprogramm, finden Sie unter [Nadeems ASP.NET MVC 5-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> darin jQuery-Validierung bei nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht US-englischen Datums-und Uhrzeitformate, enthalten Sie *globalize.js* und Ihre spezifischen  *Cultures/globalize.Cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Sie können die jQuery-Validierung nicht englischen von NuGet abrufen. (Nicht installieren Globalize bei Verwendung von einem englischen Gebietsschema.)


1. Aus der **Tools** klicken Sie im Menü **NuGetLibrary Package Manager**, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Wählen Sie im linken Bereich <strong>Durchsuchen*.</strong>* (Siehe folgende Abbildung).
3. Geben Sie in das Eingabefeld * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Wählen Sie `jQuery.Validation.Globalize`, wählen Sie `MvcMovie` , und klicken Sie auf **installieren**. Die *Scripts\jquery.globalize\globalize.js* Datei wird dem Projekt hinzugefügt werden. Die *Scripts\jquery.globalize\cultures\* Ordner viele Kultur JavaScript-Dateien enthält. Beachten Sie, dass es beim Installieren dieses Pakets fünf Minuten dauern.

   Der folgende Code zeigt die Änderungen an der Datei Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Um zu vermeiden, wiederholen diesen Code in jeder Ansicht bearbeiten, können Sie es auf die Layoutdatei verschieben. Um das Skript herunterladen zu optimieren, finden Sie unter meinem Lernprogramm [Bündelung und Minimierung](../../performance/bundling-and-minification.md).

Weitere Informationen finden Sie unter [ASP.NET MVC 3-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx) und [ASP.NET MVC 3-Internationalisierung – Teil 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Als temporäres Problem behoben Wenn Überprüfung arbeiten in Ihrem Gebietsschema kann nicht angezeigt werden, Sie können erzwingen, dass Ihre Computer auf Englisch (USA) verwenden oder Deaktivieren von JavaScript in Ihrem Browser. Um Ihren Computer auf Englisch (USA) verwenden zu erzwingen, können Sie das Globalisierung-Element hinzufügen, in das Stammverzeichnis Projekte *"Web.config"* Datei. Der folgende Code zeigt die Globalisierung-Element mit der Kultur Englisch (USA) festgelegt.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> In den nächsten Lernprogrammen implementieren wir Suchfunktion.

> [!div class="step-by-step"]
> [Zurück](accessing-your-models-data-from-a-controller.md)
> [Weiter](adding-search.md)
