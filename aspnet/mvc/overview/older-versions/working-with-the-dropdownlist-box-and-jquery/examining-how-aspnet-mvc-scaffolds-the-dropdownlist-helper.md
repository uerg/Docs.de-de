---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Überprüfen, wie ASP.NET MVC die DropDownList-Hilfsprogramms erstellt das Gerüst für | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ab45a14c4eda9e7552af4831851396af3c13dce1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826728"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Überprüfen, wie ASP.NET MVC die DropDownList-Hilfsprogramms erstellt das Gerüst für
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**. Nennen Sie den Controller **StoreManagerController**. Festlegen der Optionen für die **Controller hinzufügen** Dialogfeld wie in der folgenden Abbildung dargestellt.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Bearbeiten der *StoreManager\Index.cshtml* anzeigen und entfernen `AlbumArtUrl`. Entfernen von `AlbumArtUrl` veranlasst die Präsentation besser lesbar. Der fertige Code wird nachfolgend dargestellt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `Index` Methode. Hinzufügen der `OrderBy` Klausel, damit die Alben nach Preis sortiert werden soll. Der vollständige Code wird unten angezeigt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Sortieren nach Preis wird zum Testen von Änderungen an der Datenbank erleichtern. Beim Testen der Bearbeitung und Methoden erstellen, können Sie einen niedrigen Preis, damit die gespeicherten Daten zuerst angezeigt werden.

Öffnen der *StoreManager\Edit.cshtml* Datei. Fügen Sie die folgende Zeile direkt nach dem Tag der Legende an.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Der folgende Code zeigt die im Rahmen dieser Änderung:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Die `AlbumId` ist erforderlich, um einen Datensatz von Album zu ändern.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie diese Option die **Admin** verknüpfen, und wählen Sie dann die **neu erstellen** Link, um ein neues Album zu erstellen. Stellen Sie sicher, dass die Albuminformationen gespeichert wurde. Bearbeiten Sie ein Album, und stellen Sie sicher, dass die vorgenommenen Änderungen gespeichert werden.

### <a name="the-album-schema"></a>Das Album-Schema

Die `StoreManager` Controller, die von der MVC-Gerüstbau erstellten CRUD (Create, Read, Update, Delete), auf die Alben ermöglicht, in der Music Store-Datenbank. Das Schema für Albuminformationen ist unten dargestellt:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

Die `Albums` Tabelle speichert nicht das Album "Genre" und eine Beschreibung, die sie speichert einen Fremdschlüssel für die `Genres` Tabelle. Die `Genres` Tabelle enthält, "Genre" Name und Beschreibung. Ebenso die `Albums` Tabelle nicht enthalten, der Name des Albums-Künstler, jedoch einen Fremdschlüssel für die `Artists` Tabelle. Die `Artists` Tabelle enthält den Namen des Interpreten. Wenn Sie überprüfen, dass die Daten in die `Albums` Tabelle sehen, da Sie jede Zeile enthält einen Fremdschlüssel für die `Genres` Tabelle und einen Fremdschlüssel für die `Artists` Tabelle. Die folgende Abbildung zeigen einige Daten aus der Tabelle die `Albums` Tabelle.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Wählen Sie HTML-Tags

Der HTML-Code `<select>` Element (von der HTML-Code erstellt [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Helper) wird verwendet, um eine vollständige Liste der Werte (z. B. die Liste der Genres) angezeigt. Bearbeiten von Formularen Wenn der aktuelle Wert bekannt ist, kann die select-Liste den aktuellen Wert anzeigen. Erläutert, diesem zuvor Wenn wir den ausgewählten Wert festzulegen, um **Komödie**. Die select-Liste ist ideal für das Anzeigen von Kategorie- oder foreign Key-Daten. Die `<select>` -Element für den Fremdschlüssel "Genre" zeigt die Liste der möglichen genrenamen, aber wenn Sie das Formular speichern wird die Eigenschaft "Genre" mit "Genre" Fremdschlüsselwert, nicht den Namen der angezeigten "Genre" aktualisiert. In der folgenden Abbildung wird das ausgewählte Genre **Disco** und Interpreten **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Untersuchen von ASP.NET MVC, Gerüst Code

Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `HTTP GET Create` Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Die `Create` Methode fügt zwei [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) Objekte die `ViewBag`, "Genre" Informationen enthalten, und die Interpret-Daten enthalten. Die [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Konstruktorüberladung, die oben verwendeten akzeptiert drei Argumente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *Elemente*: ein ["IEnumerable"](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) mit den Elementen in der Liste. Im obigen Beispiel wird die Liste der Genres von zurückgegebenen `db.Genres`.
2. *DataValueField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die den Schlüsselwert enthält. Im obigen Beispiel `GenreId` und `ArtistId`.
3. *DataTextField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die die Informationen für die Anzeige enthält. In den Interpreten und Genre-Tabelle die `name` Feld verwendet wird.

Öffnen der *Views\StoreManager\Create.cshtml* Datei, und sehen die `Html.DropDownList` Helper-Markup für das Feld "Genre".

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Die erste Zeile zeigt, dass die Ansicht "erstellen" wird ein `Album` Modell. In der `Create` oben gezeigten Methode, es wurde kein Modell übergeben, damit die Ansicht Ruft eine **null** `Album` Modell. An diesem Punkt erstellt wird, ein neues Album wir noch keinen haben `Album` dafür Daten.

Die [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Überladung, die oben gezeigte nimmt den Namen des Felds an, für das Modell gebunden. Darüber hinaus verwendet diesen Namen auf die Suche nach einem **"ViewBag"** Objekt mit einer [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Objekt. Verwenden diese Überladung, Sie sind erforderlich, um den Namen der **ViewBag SelectList** Objekt `GenreId`. Der zweite Parameter (`String.Empty`) ist der Text, der angezeigt wird, wenn kein Element ausgewählt ist. Dies ist genau das, was wir wollen, wenn Sie ein neues Album zu erstellen. Wenn Sie den zweiten Parameter entfernt und den folgenden Code verwendet:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Das erste Element bzw. der Rock würde die select-Liste in diesem Beispiel standardmäßig.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Untersuchen der `HTTP POST Create` Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Diese Überladung von der `Create` -Methode übernimmt eine `album` Objekt, das durch das ASP.NET MVC-modellbindungssystem aus die Formularwerte gebucht erstellt. Wenn Sie ein neues Album, senden, wenn Modellstatus gültig ist und keine Datenbankfehler vorliegen, wird das neue Album die Datenbank hinzugefügt. Die folgende Abbildung zeigt die Erstellung eines neuen Albums.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Sie können die [Fiddler-Tool](http://www.fiddler2.com/fiddler2/) untersuchen Sie die bereitgestellten Formularwerte, ASP.NET MVC-modellbindung verwendet werden, um das Album-Objekt zu erstellen.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactoring "ViewBag" SelectList erstellen

Sowohl die `Edit` Methoden und die `HTTP POST Create` Methode haben identischen Code zum Einrichten der **SelectList** in die **"ViewBag"**. Ganz im Geiste der [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), wir werden diesen Code Umgestalten. Wir erstellen mithilfe dieses Refactoring Code später noch mal.

Erstellen Sie eine neue Methode zum Hinzufügen einer "Genre" und Interpreten **SelectList** auf die **"ViewBag"**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Ersetzen Sie die zwei Zeilen mit dem Festlegen der `ViewBag` in jedem der `Create` und `Edit` Methoden mit einem Aufruf von der `SetGenreArtistViewBag` Methode. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Übergeben die SelectList explizit zu DropDownList

Die bearbeitungs- und änderungsansichten erstellt, durch die Verwendung der ASP.NET MVC-Gerüstbau Folgendes **DropDownList** überladen:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Die `DropDownList` Markup für die Ansicht "erstellen" wird unten angezeigt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Da die `ViewBag` -Eigenschaft für die `SelectList` heißt `GenreId`, **DropDownList** Helfer verwendet die `GenreId` **SelectList** in die **"ViewBag"** . In der folgenden **DropDownList** überladen, die `SelectList` explizit übergeben.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Öffnen der *Views\StoreManager\Edit.cshtml* Datei, und ändern Sie die **DropDownList** Aufruf explizit übergeben die **SelectList**, mithilfe der Überladung, die oben genannten. Dies gilt für die Kategorie "Genre". Der vollständige Code wird unten gezeigt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Führen Sie die Anwendung, und klicken Sie auf die **Admin** zu verknüpfen, und klicken Sie dann ein Album Jazz navigieren, und wählen die **bearbeiten** Link.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Anstelle Jazz als das derzeit ausgewählte Genre, wird die Rock angezeigt. Wenn das Argument "String" (die Eigenschaft zum Binden) und die **SelectList** Objekt den gleichen Namen haben, wird der ausgewählte Wert nicht verwendet. Wenn es kein ausgewählten Wert angegeben ist, Browser standardmäßig das erste Element in der **SelectList**(d.h. **Rock** im obigen Beispiel). Dies ist eine bekannte Einschränkung der **DropDownList** Helper.

Öffnen der *Controllers\StoreManagerController.cs* und Ändern der **SelectList** Objektnamen, `Genres` und `Artists`. Der vollständige Code wird unten gezeigt:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Die Namen, die Genres und Künstler sind bessere Namen für die Kategorien aus, da sie mehr als nur die ID der einzelnen Kategorien enthalten. Das refactoring, die, das wir zuvor, gelohnt. Ändern, sondern die **"ViewBag"** wurden unsere Änderungen in vier Methoden, auf die `SetGenreArtistViewBag` Methode.

Ändern der **DropDownList** rufen Sie in das Erstellen und Bearbeiten von Ansichten, mit denen die Verwendung der neuen **SelectList** Namen. Das neue Markup für die Bearbeitungsansicht ist unten dargestellt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Ansicht "erstellen" ist erforderlich, eine leere Zeichenfolge ein, um zu verhindern, dass das erste Element in der SelectList angezeigt wird.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren. Testen Sie den Code bearbeiten, indem Sie die ein Album mit einer "Genre" als Rock auswählen.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Verwenden ein Ansichtsmodell mit dem DropDownList-Hilfsprogramms

Erstellen Sie eine neue Klasse im Ordner "ViewModels" mit dem Namen `AlbumSelectListViewModel`. Ersetzen Sie den Code in die `AlbumSelectListViewModel` Klasse durch Folgendes:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Die `AlbumSelectListViewModel` -Konstruktor akzeptiert ein Album und eine Liste mit Interpreten und Genres und erstellt ein Objekt mit dem Album und `SelectList` für Genres und Künstler.

Erstellen Sie das Projekt daher `AlbumSelectListViewModel` ist verfügbar, wenn wir im nächsten Schritt eine Ansicht erstellen.

Hinzufügen einer `EditVM` Methode, um die `StoreManagerController`. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Klicken Sie mit der rechten Maustaste auf `AlbumSelectListViewModel`Option **beheben**, klicken Sie dann **mit MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternativ können Sie hinzufügen, die folgenden using-Anweisung:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Klicken Sie mit der rechten Maustaste auf `EditVM` , und wählen Sie **Ansicht hinzufügen**. Verwenden Sie die nachfolgend aufgeführten Optionen.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wählen Sie **hinzufügen**, ersetzen Sie den Inhalt von der *Views\StoreManager\EditVM.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Die `EditVM` Markup ist sehr ähnlich, mit dem Original `Edit` Markup mit den folgenden Ausnahmen.

- Modellieren von Eigenschaften in der `Edit` Anzeigen des Formulars werden `model.property`(z. B. `model.Title` ). Modellieren von Eigenschaften in der `EditVm` Anzeigen des Formulars werden `model.Album.property`(z. B. `model.Album.Title`). Der Grund dafür ist die `EditVM` Ansicht übergeben wird einen Container für eine `Album`und keine `Album` wie in der `Edit` anzeigen.
- Die **DropDownList** zweiter Parameter stammt aus dem Ansichtsmodell, nicht die **"ViewBag"**.
- Die **BeginForm** -Hilfsmethode in den `EditVM` explizit Ansicht sendet eine Rückmeldung an den `Edit` Aktionsmethode. Durch Ausgeben von zurück an die `Edit` Aktion, die wir geschrieben haben kein `HTTP POST EditVM` Aktion. Außerdem können die `HTTP POST` `Edit` Aktion.

Führen Sie die Anwendung, und bearbeiten Sie ein Album. Ändern Sie die URL mit `EditVM`. Ändern Sie ein Feld, und drücken Sie die **speichern** Schaltfläche, um zu überprüfen, ob der Code funktioniert.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Welcher Ansatz sollten Sie verwenden?

Alle drei Ansätze, die angezeigt werden Überwachungsdatensätze. Viele Entwickler bevorzugen Explictily übergeben die `SelectList` auf die `DropDownList` mithilfe der `ViewBag`. Dieser Ansatz hat den zusätzlichen Vorteil bietet Ihnen die Flexibilität, einen besser geeigneten Namen für die Sammlung an. Ein Nachteil ist, Sie können nicht den Namen der `ViewBag SelectList` Objekt den gleichen Namen wie der Modelleigenschaft.

Einige Entwickler bevorzugen die ViewModel-Ansatz. Andere sollten Sie die ausführlichere Markup und generiert HTML-Code von der ViewModel-Ansatz einen Nachteil.

In diesem Abschnitt haben wir drei Ansätze zum Verwenden der **DropDownList** mit Kategoriedaten. Im nächsten Abschnitt zeigen wir, wie Sie eine neue Kategorie hinzuzufügen.

> [!div class="step-by-step"]
> [Zurück](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Weiter](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
