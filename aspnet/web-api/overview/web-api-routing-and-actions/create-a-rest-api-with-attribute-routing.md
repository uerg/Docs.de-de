---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Erstellen Sie eine REST-API mit Attributrouting in der ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 191452204d4347396b1d339d9b82d583a2ce9f3c
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795513"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Erstellen Sie eine REST-API mit Attributrouting in der ASP.NET-Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2 unterstützt einen neuen Typ Namens des Routings, *attributrouting*. Eine allgemeine Übersicht über das attributrouting, finden Sie unter [Attributrouting in der Web-API 2](attribute-routing-in-web-api-2.md). In diesem Tutorial verwenden Sie das attributrouting eine REST-API für eine Sammlung von Büchern zu erstellen. Die API unterstützt die folgenden Aktionen:

| Aktion | Beispiel-URI |
| --- | --- |
| Ruft eine Liste aller Bücher. | / api/Bücher |
| Erhalten Sie ein Buch anhand der ID. | /api/books/1 |
| Rufen Sie die Details eines Buchs ein. | /API/Books/1/Details |
| Abrufen einer Liste der Bücher "Genre". | /API/Books/Fantasy |
| Abrufen einer Liste von Büchern Veröffentlichungsdatum. | /api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form) |
| Ruft eine Liste von Büchern eines bestimmten Autors an. | /api/authors/1/books |

Alle Methoden sind schreibgeschützte (HTTP GET-Anforderungen).

Für die Datenschicht verwenden wir die Entity Framework. Buchdatensätze verfügen die folgenden Felder:

- Id
- Titel
- "Genre"
- Veröffentlichungsdatum
- Preis
- Beschreibung
- AutorID (Fremdschlüssel einer Autoren-Tabelle)

Für die meisten Anforderungen wird die API jedoch eine Teilmenge dieser Daten (Titel, Autor und "Genre") zurück. Um den vollständigen Datensatz, der Client Anforderungen erhalten `/api/books/{id}/details`.

## <a name="prerequisites"></a>Vorraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition.

## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

Starten Sie durch Ausführen von Visual Studio. Von der **Datei** , wählen Sie im Menü **neu** und wählen Sie dann **Projekt**.

Erweitern Sie die **installiert** > **Visual C#-** Kategorie. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Unter "Hinzufügen von Ordnern und kernreferenzen für" Wählen Sie die **Web-API-** Kontrollkästchen. Klicken Sie auf **erstellen Projekt**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Dies erstellt ein Projektgerüst, die für die Web-API-Funktionalität konfiguriert ist.

### <a name="domain-models"></a>Domänenmodelle

Fügen Sie anschließend die Klassen für Domänenmodelle. Klicken Sie im Projektmappen-Explorer den Ordner "Models". Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**. Nennen Sie die Klasse `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Ersetzen Sie den Code in Author.cs Folgendes ein:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Fügen Sie nun eine weitere Klasse namens `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu.

In diesem Schritt fügen wir einen Web-API-Controller, der Entity Framework als Datenschicht verwendet.

Drücken Sie STRG+UMSCHALT+B, um das Projekt zu erstellen. Entitätsframework verwendet Reflektion, um die Eigenschaften der Modelle, zu ermitteln, deshalb sie eine kompilierte Assembly ein, um das Datenbankschema zu erstellen müssen.

Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Lese-/schreibaktionen, mithilfe von Entitätsframework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

In der **Controller hinzufügen** im Dialogfeld für **Controllername**, geben Sie &quot;BooksController&quot;. Wählen Sie die &quot;Async-Controlleraktionen verwenden&quot; Kontrollkästchen. Für **Modellklasse**Option &quot;Buch&quot;. (Wenn Sie nicht sehen die `Book` Klasse, die in der Dropdownliste aufgeführt sind, stellen Sie sicher, dass Sie das Projekt erstellt.) Klicken Sie dann auf die Schaltfläche "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Klicken Sie auf **hinzufügen** in die **neuer Datenkontext** Dialogfeld.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Klicken Sie auf **hinzufügen** in die **Controller hinzufügen** Dialogfeld. Das Gerüst Fügt eine Klasse, die mit dem Namen `BooksController` , API-Controller definiert. Es fügt auch eine Klasse, die mit dem Namen `BooksAPIContext` im Ordner "Models", das den Datenkontext für Entity Framework definiert.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Seeding für die Datenbank

Wählen Sie im Menü Extras **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Dieser Befehl erstellt einen Ordner "Migrations" und fügt eine neue Codedatei Configuration.cs benannt. Öffnen Sie diese Datei, und fügen Sie den folgenden Code der `Configuration.Seed` Methode.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Diese Befehle erstellen eine lokale Datenbank und rufen Sie die Seed-Methode, um die Datenbank zu füllen.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Hinzufügen von DTO-Klassen

Wenn Sie die Anwendung jetzt ausführen und senden eine GET-Anforderung an /api/books/1, lautet die Antwort ähnelt dem folgenden. (Ich hinzugefügt Einzug zur besseren Lesbarkeit.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Stattdessen möchte ich diese Anforderung um eine Teilmenge der Felder zurückzugeben. Außerdem möchte ich den Namen des Autors, anstatt die Autor-ID zurückgegeben. Um dies zu erreichen, ändern wir die Controllermethoden zurückzugebenden eine *Datenübertragungsobjekt* (DTO) anstelle des EF-Modells. Ein DTO ist ein Objekt, das nur für das Übertragen von Daten konzipiert ist.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **hinzufügen** | **neuer Ordner**. Nennen Sie den Ordner &quot;DTOs&quot;. Fügen Sie eine Klasse, die mit dem Namen `BookDto` zum Ordner DTOs, mit der folgenden Definition:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Fügen Sie eine andere Klasse, die mit dem Namen `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Aktualisieren Sie als Nächstes die `BooksController` Klasse die Rückgabe `BookDto` Instanzen. Wir verwenden die [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) Methode, um das Projekt `Book` -Instanzen `BookDto` Instanzen. Hier ist der aktualisierte Code für die Controllerklasse.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Ich gelöscht, die `PutBook`, `PostBook`, und `DeleteBook` Methoden, da sie für dieses Tutorial benötigt werden.


Wenn Sie die Anwendung auszuführen und /api/books/1 anfordern, sollte der Antworttext jetzt wie folgt aussehen:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Hinzufügen von Routenattributen

Als Nächstes konvertieren wir den Controller, um die Attribut-routing verwendet. Fügen Sie zunächst eine **RoutePrefix** Attribut zum Controller. Dieses Attribut definiert die anfänglichen URI-Segmente für alle Methoden auf diesem Controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Fügen Sie dann **[Route]** Attribute für die Controlleraktionen, wie folgt:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Die routenvorlage für jede Controllermethode ist das Präfix sowie der angegebenen Zeichenfolge die **Route** Attribut. Für die `GetBook` -Methode, die routenvorlage enthält, die parametrisierte Zeichenfolge &quot;{Id: Int}&quot;, wenn das URI-Segment, einen ganzzahliger Wert enthält, der mit übereinstimmt.

| Methode | Routenvorlage | Beispiel-URI |
| --- | --- | --- |
| `GetBooks` | "api/Books" | `http://localhost/api/books` |
| `GetBook` | "api/Books / {Id: Int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Abrufen von Book-Details

Um Book-Details erhalten, der Client sendet eine GET-Anforderung an `/api/books/{id}/details`, wobei *{Id}* ist die ID des Buchs.

Fügen Sie der `BooksController`-Klasse die folgende Methode hinzu.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Wenn Sie anfordern, `/api/books/1/details`, die Antwort sieht wie folgt aus:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Bücher von "Genre" Abrufen

Rufen Sie eine Liste von Büchern in einem bestimmten Genre der Client sendet eine GET-Anforderung an `/api/books/genre`, wobei *"Genre"* ist der Name des Genres. (Beispiel: `/api/books/fantasy`)

Fügen Sie die folgende Methode `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Hier definieren wir eine Route, die einen Parameter "{" Genre "}" in der URI-Vorlage enthält. Beachten Sie, dass Web-API kann diese zwei URIs unterscheiden und zu den verschiedenen Methoden weitergeleitet:

`/api/books/1`

`/api/books/fantasy`

Der Grund dafür ist die `GetBook` Methode enthält eine Einschränkung, dass das Segment "Id" eine ganze Zahl sein muss:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Wenn Sie /api/books/fantasy angefordert haben, sieht die Antwort folgendermaßen aus:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Bücher von Autor abrufen

Rufen Sie eine Liste von einem Büchern für einen bestimmten Autor der Client sendet eine GET-Anforderung an `/api/authors/id/books`, wobei *Id* ist die ID des Autors.

Fügen Sie die folgende Methode `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

In diesem Beispiel ist interessant, da &quot;Bücher&quot; ist eine untergeordnete Ressource behandelt &quot;Autoren&quot;. Dieses Muster ist in der RESTful-APIs üblich.

Die Tilde (~) in der routenvorlage überschreibt das Routenpräfix in die **RoutePrefix** Attribut.

## <a name="get-books-by-publication-date"></a>Datum der Veröffentlichung Bücher abrufen

Um eine Liste von Büchern nach Veröffentlichungsdatum zu abzurufen, sendet der Client eine GET-Anforderung an `/api/books/date/yyyy-mm-dd`, wobei *jjjj-mm-tt* ist das Datum.

Hier ist eine Möglichkeit, dies zu tun:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Die `{pubdate:datetime}` Parameter entsprechend eingeschränkt ist eine **"DateTime"** Wert. Dies funktioniert, aber es ist tatsächlich schwächer einschränkend sein, als wir möchten. Diese URIs entspricht z. B. auch die Route:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Es gibt nichts auszusetzen, sodass diese URIs. Jedoch können Sie die Route auf ein bestimmtes Format beschränken, indem Sie eine Einschränkung für reguläre Ausdrücke die routenvorlage hinzufügen:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Derzeit wird nur Datumsangaben im Format &quot;jjjj-mm-tt&quot; entspricht. Beachten Sie, dass wir nicht dem regulären Ausdruck verwenden, um sicherzustellen, dass wir ein reales Datum haben. Dies geschieht bei der Web-API wird versucht, konvertieren die URI-Segment in einem **"DateTime"** Instanz. Ein ungültiges Datum z. B. "2012-47-99' kann nicht konvertiert werden, und der Client erhält einen 404-Fehler.

Sie können auch eine Schrägstrich Trennzeichen unterstützen (`/api/books/date/yyyy/mm/dd`) durch Hinzufügen eines anderen **[Route]** Attribut mit einem anderen regulären Ausdruck.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Es gibt hier ein kniffliges, aber wichtiges Detail. Die zweite Vorlage ist ein Platzhalterzeichen (\*) am Anfang des Parameters {Pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Dies sagt der routing-Engine, {Pubdate} den verbleibenden Teil des URI entsprechen muss. Standardmäßig entspricht ein Vorlagenparameter ein einzelnes URI-Segment an. In diesem Fall möchten wir {Pubdate} mehrere URI-Segmente umfassen:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Controller-Codes

Hier ist der vollständige Code für die BooksController-Klasse.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Zusammenfassung

Attributrouting gibt Ihnen mehr steuerungsmöglichkeiten und größere Flexibilität beim Entwerfen der URIs für Ihre API.
