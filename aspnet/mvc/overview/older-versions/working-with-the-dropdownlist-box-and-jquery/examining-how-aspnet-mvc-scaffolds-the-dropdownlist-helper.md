---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Untersuchen, wie ASP.NET MVC der DropDownList-Hilfsmethode scaffolds | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: abd9b5c09e942b966eb3eaaebe1b315c30b8e0c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Überprüfen, wie ASP.NET MVC der DropDownList-Hilfsmethode scaffolds
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**. Nennen Sie den Controller **StoreManagerController**. Festlegen der Optionen für die **Controller hinzufügen** Dialogfeld wie in der folgenden Abbildung dargestellt.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Bearbeiten der *StoreManager\Index.cshtml* anzeigen und Entfernen von `AlbumArtUrl`. Entfernen von `AlbumArtUrl` stellen die Präsentation besser lesbar. Der fertige Code wird nachfolgend dargestellt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `Index` Methode. Hinzufügen der `OrderBy` Klausel, damit die Alben nach dem Preis sortiert werden. Der vollständige Code wird unten gezeigt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Sortieren nach dem Preis wird Änderungen an der Datenbank testen vereinfachen. Wenn Sie die Bearbeitung testen und Methoden erstellen, können Sie eine Tiefstkurs verwenden, damit die gespeicherten Daten zuerst angezeigt werden.

Öffnen der *StoreManager\Edit.cshtml* Datei. Fügen Sie die folgende Zeile direkt nach dem Tag der Legende an.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Der folgende Code zeigt den Kontext dieser Änderung:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Die `AlbumId` ist erforderlich, um ein Album Datensatz zu ändern.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie diese Option die **Admin** verknüpfen, und wählen Sie dann die **neu erstellen** Link, um ein neues Album zu erstellen. Stellen Sie sicher, dass die Albuminformationen gespeichert wurde. Bearbeiten Sie ein Album, und stellen Sie sicher, dass die vorgenommenen Änderungen beibehalten werden.

### <a name="the-album-schema"></a>Das Schema Album

Die `StoreManager` von der MVC-Gerüstbau erstellten Controller ermöglicht CRUD (Create, Read, Update, Delete) Zugriff auf die Alben in die Musik-Informationsspeicher-Datenbank. Das Schema für Albuminformationen finden Sie weiter unten:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

Die `Albums` Tabelle speichert keine Album "Genre" und die Beschreibung des, speichert einen Fremdschlüssel für die `Genres` Tabelle. Die `Genres` Tabelle enthält die "Genre" Name und Beschreibung. Entsprechend der `Albums` Tabelle nicht enthalten, der Name des Albums Künstler, sondern ein Fremdschlüssel für die `Artists` Tabelle. Die `Artists` Tabelle enthält den Namen der Interpreten. Wenn Sie überprüfen, dass die Daten in der `Albums` Tabelle sehen, da Sie jede Zeile enthält einen Fremdschlüssel für die `Genres` Tabelle und einem Fremdschlüssel für die `Artists` Tabelle. Die folgende Abbildung zeigen einige Daten aus der Tabelle der `Albums` Tabelle.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Wählen Sie HTML-Tag

Der HTML-Code `<select>` Element (erstellt von HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Helper) wird verwendet, um eine vollständige Liste der Werte (z. B. die Liste von Genres) angezeigt. Für Formulare bearbeiten Wenn der aktuelle Wert bekannt ist, kann die select-Liste den aktuellen Wert anzeigen. Filtermenü diesem zuvor Wenn wir den ausgewählten Wert festgelegt, um **Comedy**. Die select-Liste ist ideal zum Anzeigen von Kategorie- oder foreign Key-Daten. Die `<select>` -Element für den Fremdschlüssel "Genre" zeigt die Liste der möglichen "Genre" Namen, aber wenn Sie das Formular zu speichern wird die Eigenschaft "Genre" mit der "Genre" Fremdschlüsselwert, nicht den Namen der angezeigten "Genre" aktualisiert. In der folgenden Abbildung wird die ausgewählte "Genre" **Disco** und Künstler **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Untersuchen von ASP.NET MVC Gerüstbau Code

Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `HTTP GET Create` Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Die `Create` Methode fügt zwei [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) -Objekte und die `ViewBag`, "Genre" Informationen enthalten, und der Interpreteninformationen enthalten. Die [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Konstruktorüberladung oben verwendeten akzeptiert drei Argumente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *Elemente*: ein [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) mit den Elementen in der Liste. Im obigen Beispiel die Liste von Genres zurückgegebenes `db.Genres`.
2. *DataValueField*: der Name der Eigenschaft in der **IEnumerable** Liste, die den Schlüsselwert enthält. Im Beispiel oben `GenreId` und `ArtistId`.
3. *DataTextField*: der Name der Eigenschaft in der **IEnumerable** Liste, die zur Darstellung der Informationen enthält. In den Künstler und "Genre"-Tabelle die `name` Feld verwendet wird.

Öffnen der *Views\StoreManager\Create.cshtml* Datei, und überprüfen Sie die `Html.DropDownList` Helper-Markup für das Feld "Genre".

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Die erste Zeile zeigt, dass die Erstellungsansicht akzeptiert ein `Album` Modell. In der `Create` oben gezeigten Methode, es wurde kein Modell übergeben, damit die Ansicht Ruft eine **null** `Album` Modell. An diesem Punkt sind wir ein neues Album erstellen, damit wir nicht über verfügen `Album` Daten für sie.

Die [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) oben gezeigten Überladung nimmt den Namen des Felds, das an das Modell binden. Darüber hinaus verwendet diesen Namen, der gesucht eine **ViewBag** mit einem [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Objekt. Verwenden diese Überladung, Sie sind erforderlich, um den Namen der **ViewBag SelectList** Objekt `GenreId`. Der zweite Parameter (`String.Empty`) ist der Text angezeigt, wenn kein Element ausgewählt ist. Dies ist genau das, was wir soll, wenn ein neues Album zu erstellen. Wenn Sie den zweiten Parameter entfernt, und den folgenden Code verwendet:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Die select-Liste standardmäßig würde in diesem Beispiel auf das erste Element oder Rock.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Untersuchen der `HTTP POST Create` Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Diese Überladung von der `Create` -Methode übernimmt ein `album` mit ASP.NET MVC-Modell Bindungssystems aus der Formularwerte, gebucht erstellte-Objekt. Wenn Sie ein neues Album übermitteln, wenn Modellstatus gültig ist und keine Datenbankfehler vorliegen, wird das neue Album die Datenbank hinzugefügt. Die folgende Abbildung zeigt die Erstellung ein neues Album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Sie können die [Fiddler-Tool](http://www.fiddler2.com/fiddler2/) die übermittelte Formularwerte untersuchen, ASP.NET MVC-modellbindung verwendet, um das Album-Objekt zu erstellen.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Umgestaltung der ViewBag SelectList-Erstellung

Sowohl die `Edit` Methoden und die `HTTP POST Create` Methode verfügen identischen Code zum Einrichten der **SelectList** in der **ViewBag**. Gemäß dem Prinzip der [TROCKENEN](http://en.wikipedia.org/wiki/Don't_repeat_yourself), gestalten wir diesen Code. Wir stellen mithilfe dieses Code später umgestaltet.

Erstellen einer neuen Methode ein "Genre" und Interpreten hinzufügen **SelectList** auf die **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Ersetzen Sie die zwei Zeilen Festlegen der `ViewBag` in jedem der `Create` und `Edit` mit einem Aufruf von Methoden der `SetGenreArtistViewBag` Methode. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Übergeben die SelectList explizit an der DropDownList

Erstellen und Bearbeiten von Ansichten erstellt, durch die Verwendung der ASP.NET MVC-Gerüstbau Folgendes **DropDownList** überladen:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Die `DropDownList` Markup für die Erstellungsansicht wird unten gezeigt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Da die `ViewBag` -Eigenschaft für die `SelectList` lautet `GenreId`, die **DropDownList** Hilfsobjekt verwenden, wird die `GenreId` **SelectList** in die **ViewBag** . Im folgenden **DropDownList** überladen, die `SelectList` explizit übergeben.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Öffnen der *Views\StoreManager\Edit.cshtml* Datei, und ändern Sie die **DropDownList** Aufruf explizit übergeben der **SelectList**, mithilfe der Überladung, die oben genannten. Dies gilt für die Kategorie "Genre". Der vollständige Code wird unten gezeigt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Führen Sie die Anwendung, und klicken Sie auf die **Admin** verknüpfen, und klicken Sie dann ein Album Jazz navigieren und wählen Sie die **bearbeiten** Link.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Anstelle von Jazz als den aktuell ausgewählten "Genre", wird die Rock angezeigt. Wenn das Zeichenfolgenargument (die Eigenschaft binden) und die **SelectList** Objekt den gleichen Namen aufweisen, wird der ausgewählte Wert nicht verwendet. Wird kein ausgewählten Wert angegeben, Browser standardmäßig das erste Element in der **SelectList**(also **Rock** im obigen Beispiel). Dies ist eine bekannte Einschränkung bei der die **DropDownList** Helper.

Öffnen der *Controllers\StoreManagerController.cs* Datei und ändern Sie die **SelectList** Objektnamen, `Genres` und `Artists`. Der vollständige Code wird unten gezeigt:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Die Namen Genres und Künstler sind besser Namen für die Kategorien, da sie mehr als nur die ID der einzelnen Kategorien enthalten. Die Umgestaltung früher haben ausgezahlt. Anstatt zu ändern, die **ViewBag** in vier Methoden waren unsere Änderungen nur im Zusammenhang mit der `SetGenreArtistViewBag` Methode.

Ändern der **DropDownList** rufen Sie in das Erstellen und Bearbeiten von Ansichten Verwendung des neuen **SelectList** Namen. Das neue Markup für die Bearbeitungsansicht ist unten dargestellt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Die Erstellungsansicht erfordert eine leere Zeichenfolge zu verhindern, dass das erste Element in der Auswahlliste angezeigt werden.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren. Testen Sie den Code bearbeiten, indem Sie ein Album mit einem "Genre" als Rock auswählen.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Verwenden ein Modell anzeigen, mit der DropDownList-Hilfsmethode

Erstellen Sie eine neue Klasse im Ordner "ViewModels" mit dem Namen `AlbumSelectListViewModel`. Ersetzen Sie den Code in der `AlbumSelectListViewModel` Klasse durch Folgendes:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Die `AlbumSelectListViewModel` Konstruktor akzeptiert ein Album und eine Liste der Künstler und Genres und erstellt ein Objekt, das Album enthält und eine `SelectList` für Genres und Künstler.

Erstellen Sie das Projekt daher `AlbumSelectListViewModel` ist verfügbar, wenn wir im nächsten Schritt eine Ansicht erstellen.

Hinzufügen einer `EditVM` Methode, um die `StoreManagerController`. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Klicken Sie mit der rechten Maustaste auf `AlbumSelectListViewModel`Option **beheben**, klicken Sie dann **mit MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternativ können Sie hinzufügen, die folgende Anweisung:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Klicken Sie mit der rechten Maustaste auf `EditVM` , und wählen Sie **Ansicht hinzufügen**. Mithilfe der Optionen unten angezeigt.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wählen Sie **hinzufügen**, ersetzen Sie den Inhalt von der *Views\StoreManager\EditVM.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Die `EditVM` Markup ist sehr ähnlich, mit dem ursprünglichen `Edit` Markup mit folgenden Ausnahmen.

- Modellieren von Eigenschaften in der `Edit` Ansicht des Formulars sind `model.property`(z. B. `model.Title` ). Modellieren von Eigenschaften in der `EditVm` Ansicht des Formulars sind `model.Album.property`(z. B. `model.Album.Title`). Liegt darin, dass die `EditVM` Ansicht ist einen Container für übergeben ein `Album`, sondern ein `Album` wie in der `Edit` anzeigen.
- Die **DropDownList** zweiter Parameter stammt nicht aus dem Modell anzeigen, die **ViewBag**.
- Die **BeginForm** Hilfsprogramm in der `EditVM` explizit Ansicht sendet eine Rückmeldung an die `Edit` Aktionsmethode. Durch das Bereitstellen von zurück an die `Edit` Aktion, liegen keine Schreiben einer `HTTP POST EditVM` Aktion und wiederverwenden können die `HTTP POST` `Edit` Aktion.

Führen Sie die Anwendung, und bearbeiten Sie ein Album. Ändern Sie die zu verwendende URL `EditVM`. Ändern eines Felds, und drücken Sie die **speichern** Schaltfläche, um zu überprüfen, ob der Code funktioniert.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Welcher Ansatz sollten Sie verwenden?

Alle drei Ansätze, die angezeigt werden akzeptiert. Viele Entwickler bevorzugen Explictily Durchlauf der `SelectList` auf die `DropDownList` mithilfe der `ViewBag`. Dieser Ansatz hat den zusätzlichen Vorteil bietet Ihnen die Flexibilität, verwenden einen geeigneteren Namen für die Sammlung an. Ein Nachteil ist Sie können nicht den Namen der `ViewBag SelectList` -Objekt den gleichen Namen wie die Modelleigenschaft.

Einige Entwickler bevorzugen das ViewModel-Ansatz. Andere sollten Sie eine detailliertere Markup und generiert HTML des Ansatzes ViewModel einen Nachteil.

In diesem Abschnitt haben wir drei Ansätze zum Verwenden der **DropDownList** mit Kategoriedaten. Im nächsten Abschnitt zeigen wir, wie Sie eine neue Kategorie hinzufügen.

>[!div class="step-by-step"]
[Zurück](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Weiter](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
