---
title: Aktualisieren der generierten Seiten
author: rick-anderson
description: Aktualisieren der generierten Seiten mit besserer Anzeige.
keywords: ASP.NET Core, Razor-Seiten
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 8b2018bbf83cbb4b5c9139605fbb97d1c5be959f
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2017
---
# <a name="updating-the-generated-pages"></a>Aktualisieren der generierten Seiten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualisieren des generierten Codes

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie > **Schnellaktionen und Refactorings**.

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)


Wählen Sie `using System.ComponentModel.DataAnnotations;`

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.

Wir behandeln [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) im nächsten Tutorial. Das [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“). Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)-Attribut gibt den Typ der Daten (Date) an, damit die im Feld gespeicherten Uhrzeitinformationen nicht angezeigt werden.

Navigieren Sie zu „Pages/Movies“, und bewegen Sie den Mauszeiger über dem Link **Bearbeiten**, um die Ziel-URL anzuzeigen.

![Browserfenster mit Maus über dem Link „Bearbeiten“ und der Link-URL „http://localhost:1234/Movies/Edit/5“](da1/edit7.png)

Die Links **Edit**, **Details** und **Delete** werden mithilfe des [Hilfsprogramms für Ankertags](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) in der Datei *Pages/Movies/Index.cshtml* generiert.

[!code-cshtml[Main](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]

[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien. Im vorangehenden Code generiert das `AnchorTagHelper` dynamisch den Wert des HTML-Attributs `href` auf der Razor-Seite (die Route ist relativ), das `asp-page`-Element und die Routen-ID (`asp-route-id`). Weitere Informationen finden Sie unter [URL-Generierung für Seiten](xref:mvc/razor-pages/index#url-generation-for-pages).

Rufen Sie in Ihrem bevorzugten Browser **Quelltext anzeigen** auf, um das generierte Markup zu untersuchen. Ein Teil des generierten HTML-Codes wird unten gezeigt:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>

```

Die dynamisch generierten Links übergeben die Film-ID mit einer Abfragezeichenfolge (z B. `http://localhost:5000/Movies/Details?id=2`). 

Aktualisieren Sie die Razor-Seiten „Edit“, „Details“ und „Delete“ so, dass die Routenvorlage „{id:int}“ verwendet wird. Ändern Sie die „page“-Direktive für jede dieser Seiten in `@page "{id:int}"`. Führen Sie die App aus, und zeigen Sie dann den Quelltext an. Der generierte HTML-Code fügt die ID dem Pfadteil der URL hinzu:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Eine Anforderung an die Seite mit der Routenvorlage „{id:int}“, die **nicht** die ganze Zahl enthält, gibt den HTTP-Fehler 404 (Nicht gefunden) zurück. `http://localhost:5000/Movies/Details` gibt beispielsweise den Fehler 404 zurück. Um die ID optional zu machen, fügen Sie `?` an die Routeneinschränkung an:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Aktualisieren der Behandlung von Ausnahmen bei vollständiger Parallelität

Aktualisieren Sie die `OnPostAsync`-Methode in der Datei *Pages/Movies/Edit.cshtml.cs*. Im folgenden markierten Code sind alle Änderungen dargestellt:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]

Der vorherige Code erkennt nur Ausnahmen bei vollständiger Parallelität, wenn der erste gleichzeitige Client den Film löscht und der zweite gleichzeitige Client Änderungen am Film vornimmt.

So testen Sie den Block `catch`

* Legen Sie einen Haltepunkt bei `catch (DbUpdateConcurrencyException)` fest.
* Bearbeiten Sie einen Film.
* Klicken Sie in einem anderen Browserfenster auf den Link **Delete** für denselben Film, und löschen Sie dann den Film.
* Übermitteln Sie im vorherigen Browserfenster Änderungen am Film.

Code in Produktionsumgebungen erkennt normalerweise Parallelitätskonflikte, wenn zwei oder mehrere Clients einen Datensatz gleichzeitig aktualisiert haben. Weitere Informationen finden Sie unter [Behandlung von Parallelitätskonflikten](xref:data/ef-mvc/concurrency).

### <a name="posting-and-binding-review"></a>Überprüfen der Bereitstellung und Bindung

Untersuchen Sie die Datei *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]

Wenn eine HTTP GET-Anforderung an die Seite „Movies/Edit“ gerichtet wird (z.B. `http://localhost:5000/Movies/Edit/2`):

* Die `OnGetAsync`-Methode ruft den Film aus der Datenbank ab und gibt die `Page`-Methode zurück. 
* Die `Page`-Methode rendert die Razor-Seite *Pages/Movies/Edit.cshtml*. Die Datei *Pages/Movies/Edit.cshtml* enthält die Modelldirektive (`@model RazorPagesMovie.Pages.Movies.EditModel`), die das Filmmodell auf der Seite verfügbar macht.
* Das Bearbeitungsformular wird mit den Werten aus dem Film angezeigt.

Wenn die Seite „Filme/Bearbeiten“ bereitgestellt wird:

* Die Formularwerte auf der Seite sind an die `Movie`-Eigenschaft gebunden. Das `[BindProperty]`-Attribut ermöglicht die [Modellbindung](xref:mvc/models/model-binding).

```csharp
[BindProperty]
public Movie Movie { get; set; }
```

* Bei Fehlern beim Modellstatus (Beispiel: `ReleaseDate` kann nicht in ein Datum konvertiert werden), wird das Formular mit den übermittelten Werten erneut bereitgestellt.
* Wenn keine Modellfehler vorhanden sind, wird der Film gespeichert.

Die HTTP GET-Methoden auf den Razor-Seiten „Index“, „Create“ und „Delete“ folgen einem ähnlichen Muster. Die HTTP-POST-Methode `OnPostAsync` auf der Razor-Seite „Create“ folgt einem ähnlichen Muster wie die `OnPostAsync`-Methode auf der Razor-Seite „Edit“.

Die Suche wird im nächsten Tutorial hinzugefügt.

>[!div class="step-by-step"]
[Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Hinzufügen der Suche](xref:tutorials/razor-pages/search)
