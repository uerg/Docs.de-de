---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Erstellen Sie eine REST-API mit Routing-Attribut in der ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "30223261"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Erstellen Sie eine REST-API mit Routing in ASP.NET Web API 2-Attribut
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2 unterstützt einen neuen Typ wird aufgerufen, Routing, *routing-Attribut*. Eine allgemeine Übersicht über routing-Attribut, finden Sie unter [Routing-Attribut in der Web-API 2](attribute-routing-in-web-api-2.md). In diesem Lernprogramm verwenden Sie routing-Attribut eine REST-API für eine Sammlung von Büchern zu erstellen. Die API unterstützt die folgenden Aktionen:

| Aktion | Beispiel-URI |
| --- | --- |
| Ruft eine Liste aller Bücher. | / api/Bücher |
| Ein Buch-ID abrufen | /api/books/1 |
| Abrufen von Details eines Buchs. | /API/Books/1/Details |
| Abrufen einer Liste von Büchern von "Genre". | /API/Books/Fantasy |
| Abrufen einer Liste von Büchern nach Veröffentlichungsdatum. | /api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form) |
| Ruft eine Liste von Büchern von einem bestimmten Autor. | /api/authors/1/books |

Alle Methoden sind ohne Schreibzugriff (HTTP GET-Anforderungen).

Für die Datenschicht verwenden wir Entity Framework. Datensätze müssen die folgenden Felder:

- Id
- Titel
- "Genre"
- Veröffentlichungsdatum
- Preis
- Beschreibung
- AutorID (Fremdschlüssel mit einer Autorentabelle)

Für die meisten Anforderungen gibt die API jedoch eine Teilmenge dieser Daten (Title, Author und "Genre") zurück. Die vollständigen Datensatz, der Client Anforderungen abrufen `/api/books/{id}/details`.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Visual Studio-2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition.

## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

Starten von Visual Studio ausgeführt wird. Aus der **Datei** klicken Sie im Menü **neu** und wählen Sie dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Klicken Sie unter "Hinzufügen von Ordnern und Verweise für core" Wählen Sie die **Web-API** Kontrollkästchen. Klicken Sie auf **erstellen Projekt**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Dies erstellt ein Skeleton-Projekt, das für die Web-API-Funktionalität konfiguriert ist.

### <a name="domain-models"></a>Domänenmodelle

Als Nächstes fügen Sie Klassen für Domänenmodelle hinzu. Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models. Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**. Nennen Sie die Klasse `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Ersetzen Sie den Code in Author.cs durch Folgendes:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Fügen Sie jetzt eine andere Klasse, die mit dem Namen `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu

In diesem Schritt fügen wir einen Web-API-Controller, der als die Datenschicht Entity Framework verwendet.

Drücken Sie STRG+UMSCHALT+B, um das Projekt zu erstellen. Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle ermitteln, daher ist eine kompilierte Assembly So erstellen das Schema der Datenbank erforderlich.

Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Lese-/schreibaktionen unter Verwendung von Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

In der **Controller hinzufügen** im Dialogfeld für **Controllernamen**, geben Sie &quot;BooksController&quot;. Wählen Sie die &quot;asynchrone Controlleraktionen verwenden&quot; Kontrollkästchen. Für **Modellschemas**Option &quot;Buch&quot;. (Wenn Sie sehen die `Book` Klasse, die in der Dropdownliste aufgeführt sind, stellen Sie sicher, dass Sie das Projekt erstellt.) Klicken Sie dann auf die Schaltfläche "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Klicken Sie auf **hinzufügen** in der **neuen Datenkontext** Dialogfeld.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Klicken Sie auf **hinzufügen** in der **Controller hinzufügen** Dialogfeld. Das Gerüst Fügt eine Klasse namens `BooksController` , die die API-Controller definiert. Sie fügt außerdem eine Klasse namens `BooksAPIContext` im Ordner Models, der den Datenkontext für Entity Framework definiert.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Ausgangswert für die Datenbank

Wählen Sie im Menü Extras **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Dieser Befehl erstellt einen Ordner Migrationen und fügt eine neue Codedatei mit dem Namen "Configuration.cs". Öffnen Sie diese Datei, und fügen Sie folgenden Code zu der `Configuration.Seed` Methode.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Geben Sie die folgenden Befehle aus, klicken Sie im Paket-Manager-Konsole.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Diese Befehle eine lokale Datenbank erstellen und Aufrufen der Seed-Methode, um die Datenbank aufzufüllen.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Fügen Sie DTO-Klassen

Wenn Sie die Anwendung jetzt ausführen und senden eine GET-Anforderung an /api/books/1, sieht die Antwort ähnlich der folgenden. (Ich hinzugefügt Einzug zur besseren Lesbarkeit.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Stattdessen werden diese Anforderung, die eine Teilmenge der Felder zurückgeben soll. Darüber hinaus möchte ich den Namen des Autors, anstatt die Autor-ID zurückgegeben. Zu diesem Zweck ändern wir die Controllermethoden zurückzugebenden eine *Datenübertragungsobjekt* (DTO) anstelle des EF-Modells. Ein DTO ist ein Objekt, das entworfen wurde, nur für Daten ausführen.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen** | **neuer Ordner**. Benennen Sie den Ordner &quot;DTOs&quot;. Fügen Sie eine Klasse mit dem Namen `BookDto` in den Ordner DTOs mit folgender Definition:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Fügen Sie eine andere Klasse, die mit dem Namen `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Aktualisieren Sie als Nächstes die `BooksController` Klasse zurückzugebenden `BookDto` Instanzen. Wir verwenden die [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) Methode Projekt `Book` -Instanzen `BookDto` Instanzen. Hier ist der aktualisierte Code für die Controllerklasse.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Ich gelöscht, die `PutBook`, `PostBook`, und `DeleteBook` Methoden, da sie nicht für dieses Lernprogramm benötigt werden.


Wenn Sie die Anwendung auszuführen und /api/books/1 anfordern, sollte der Antworttext jetzt wie folgt aussehen:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Hinzufügen von Routenattributen

Konvertieren Sie als Nächstes den Controller, um das routing-Attribut verwenden. Fügen Sie zunächst eine **Routenpräfix** -Attribut auf den Controller. Dieses Attribut definiert die anfänglichen URI-Segmente für alle Methoden auf diesem Controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Fügen Sie dann **[Route]** Attribute für die Controlleraktionen, wie folgt:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Die routenvorlage für jede Controllermethode ist das Präfix sowie der angegebenen Zeichenfolge der **Route** Attribut. Für die `GetBook` -Methode, die routenvorlage enthält, die parametrisierte Zeichenfolge &quot;{-Id: Int}&quot;, dem entspricht, sofern das URI-Segment einen ganzzahligen Wert enthält.

| Methode | Routenvorlage | Beispiel-URI |
| --- | --- | --- |
| `GetBooks` | "-api/Books" | `http://localhost/api/books` |
| `GetBook` | "-api/Books / {Id: Int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Abrufen von Details für das Offlineadressbuch

Um Buchdetails zu erhalten, der Client sendet eine GET-Anforderung `/api/books/{id}/details`, wobei *{Id}* ist die ID des Buchs.

Fügen Sie der `BooksController`-Klasse die folgende Methode hinzu.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Wenn Sie anfordern `/api/books/1/details`, die Antwort sieht wie folgt aus:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Abrufen von Büchern "Genre"

Um eine Liste von Büchern in einer bestimmten "Genre" abzurufen, der Client sendet eine GET-Anforderung `/api/books/genre`, wobei *"Genre"* ist der Name des der "Genre". (Beispiel: `/api/books/fantasy`)

Fügen Sie die folgende Methode hinzu `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Hier werden wir eine Route definieren, die einen {"Genre"}-Parameter in der URI-Vorlage enthält. Beachten Sie, dass Web-API kann diese zwei URIs unterscheiden und zu den verschiedenen Methoden weiterleiten:

`/api/books/1`

`/api/books/fantasy`

Grund hierfür ist die `GetBook` Methode enthält eine Einschränkung, dass das Segment "Id" als ganze Zahl sein muss:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Wenn Sie /api/books/fantasy anfordern, sieht die Antwort wie folgt:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Abrufen von Büchern vom Autor

Um eine Liste von einem Büchern für einen bestimmten Autor zu erhalten, der Client sendet eine GET-Anforderung `/api/authors/id/books`, wobei *Id* ist die ID des Autors.

Fügen Sie die folgende Methode hinzu `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

In diesem Beispiel ist interessant, da &quot;Bücher&quot; ist eine untergeordnete Ressource behandelt &quot;Autoren&quot;. Dieses Vorgehen ist üblich, das in Rest-APIs.

Die Tilde (~) in der routenvorlage überschreibt das Routenpräfix in der **Routenpräfix** Attribut.

## <a name="get-books-by-publication-date"></a>Abrufen von Büchern nach Veröffentlichungsdatum

Um eine Liste von Büchern nach Veröffentlichungsdatum zu erhalten, der Client sendet eine GET-Anforderung `/api/books/date/yyyy-mm-dd`, wobei *jjjj-mm-tt* ist das Datum.

Hier ist eine Möglichkeit hierfür ein:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Die `{pubdate:datetime}` Parameter entsprechend eingeschränkt ist ein **"DateTime"** Wert. Dies funktioniert, allerdings handelt es sich tatsächlich schwächer einschränkend sein als wir möchten. Diese URIs werden z. B. auch die Route entspricht:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Es ist nichts falsch mit dem diese URIs. Allerdings können Sie die Route auf ein bestimmtes Format beschränken, indem Sie eine Einschränkung für reguläre Ausdrücke an die routenvorlage:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Jetzt nur Datumsangaben in der Form &quot;jjjj-mm-tt&quot; entspricht. Beachten Sie, dass wir nicht den regulären Ausdruck verwenden, um sicherzustellen, dass wir ein echtes Datum haben. Die behandelt wird, wenn die Web-API wird versucht, konvertieren das URI-Segment in einem **"DateTime"** Instanz. Ein ungültiges Datum z. B. "2012-47-99' kann nicht konvertiert werden, und der Client erhält einen 404-Fehler.

Sie können auch eine Schrägstrich-Trennzeichen unterstützen (`/api/books/date/yyyy/mm/dd`) durch Hinzufügen eines anderen **[Route]** Attribut mit einem anderen regulären Ausdruck.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Es ist hier ein feinen, aber wichtige Detail. Die zweite routenvorlage hat ein Platzhalterzeichen (\*) am Anfang der {Pubdate}-Parameter:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Dies weist dem Routingmodul, dass diese {Pubdate} der Rest des URI übereinstimmt. Standardmäßig entspricht ein Vorlagenparameter ein einzelnes URI-Segment. In diesem Fall möchten wir {Pubdate}, die mehrere URI-Segmenten umfassen:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Controllercode

Hier ist der vollständige Code für die BooksController-Klasse.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Zusammenfassung

Routing-Attribut bietet Ihnen mehr Kontrolle und Flexibilität beim Entwerfen der URIs für Ihre API.
