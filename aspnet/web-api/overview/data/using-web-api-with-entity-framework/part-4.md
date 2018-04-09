---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Behandlung von Entitätsbeziehungen | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="handling-entity-relations"></a>Behandeln von Entitätsbeziehungen
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt werden einige Details wie EF verknüpfte Entitäten lädt und Behandeln von zirkuläre Navigationseigenschaften in Ihren Modellklassen beschrieben. (Dieser Abschnitt enthält Hintergrundkenntnisse und ist nicht erforderlich, um das Lernprogramm abzuschließen. Wenn Sie dies bevorzugen, fahren Sie mit [Teil 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager Loading im Vergleich zu Lazy Loading

Wenn EF mit einer relationalen Datenbank verwenden zu können, ist es wichtig zu verstehen, wie EF verknüpfte Daten lädt.

Es ist auch nützlich, um die SQL-Abfragen anzuzeigen, die EF generiert. Um die SQL zu verfolgen, fügen Sie die folgende Codezeile, die `BookServiceContext` Konstruktor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Wenn Sie eine GET-Anforderung an /api/books senden, wird JSON wie folgt zurückgegeben:

[!code-console[Main](part-4/samples/sample2.cmd)]

Sie sehen, dass die Author-Eigenschaft null ist, obwohl das Buch eine gültige AutorID enthält. Das liegt daran EF nicht die zugehörigen Autor Entitäten geladen wird. Das Ablaufverfolgungsprotokoll der SQL-Abfrage, bestätigt dies:

[!code-console[Main](part-4/samples/sample3.sql)]

Die SELECT-Anweisung aus der Tabelle Bücher akzeptiert, und der Autor-Tabelle verweist nicht auf.

Zu Referenzzwecken hier ist die Methode in der `BooksController` Klasse, die die Liste der Bücher zurückgibt.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Sehen wir uns an, wie es den Autor als Teil der JSON-Daten zurückgeben kann. Es gibt drei Möglichkeiten zum Laden von verknüpfter Daten im Entity Framework: unverzüglichem Laden, verzögertes Laden und explizites Laden. Es gibt vor-und Nachteile mit einzelnen Techniken, daher ist es wichtig zu verstehen, wie diese funktionieren.

### <a name="eager-loading"></a>Eager Loading

Mit *unverzüglichem Laden*, EF verknüpfte Entitäten als Teil der Abfrage für die ursprüngliche Datenbank geladen. Verwenden Sie zum Ausführen von unverzüglichem Laden die **System.Data.Entity.Include** Erweiterungsmethode.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Dies teilt EF, um die Autor-Daten in die Abfrage einzuschließen. Wenn Sie diese Änderung vornehmen, und führen Sie die app, sieht die JSON-Daten nun wie folgt:

[!code-console[Main](part-4/samples/sample6.cmd)]

Das Ablaufverfolgungsprotokoll zeigt, dass EF einen Join für die Tabellen Book "und" Autor ausgeführt.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Lazy Loading

Beim lazy Loading lädt EF automatisch eine verknüpfte Entität, bei der die Navigationseigenschaft für diese Entität dereferenziert wird. Um das verzögertes Laden aktivieren, stellen Sie die Navigationseigenschaft virtuellen. Beispielsweise ist in der Book-Klasse:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Nun sehen wir uns mit den folgenden Code:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Wenn das verzögertes Laden aktiviert ist, den Zugriff auf die `Author` Eigenschaft auf `books[0]` bewirkt, dass EF zum Abfragen der Datenbank für den Autor.

Verzögertes Laden erfordert mehrere Datenbankaufrufen, da EF sendet eine Abfrage, die jeweils eine verknüpfte Entität abgerufen. In der Regel sollen lazy Loading für Objekte, die Sie serialisieren deaktiviert. Das Serialisierungsprogramm muss alle Eigenschaften für das Modell gelesen, die auslöst, laden die verknüpften Entitäten. Hier sind z. B. auch die SQL-Abfragen ein EF die Liste der Bücher mit lazy Loading aktiviert serialisiert. Sie können sehen, dass EF drei separate Abfragen für die drei Autoren macht.

[!code-console[Main](part-4/samples/sample10.sql)]

Es sind noch Mal, wenn Sie verzögertes Laden verwenden möchten. Unverzüglichem Laden kann dazu führen, dass EF, um eine sehr komplexe Verknüpfung zu generieren. Oder Sie möglicherweise verknüpfte Entitäten für eine kleine Teilmenge der Daten und verzögertes Laden wäre eine effizientere.

Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung wird zum Serialisieren von datenübertragungsobjekte (DTOs) anstelle von Entitätsobjekten. Dieser Ansatz zeige ich später in diesem Artikel.

### <a name="explicit-loading"></a>Explizites Laden

Explizites Laden ähnelt lazy loading, außer dass Sie die verwandten Daten explizit in Code auftritt; Es ist nicht automatisch ausgeführt, wenn Sie Zugriff auf eine Navigationseigenschaft. Explizites Laden bietet Ihnen mehr Kontrolle darüber, wann zum Laden von verknüpfter Daten, jedoch ist zusätzlicher Code erforderlich. Weitere Informationen zum expliziten Laden finden Sie unter [Laden von verknüpften Entitäten](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Navigationseigenschaften und Zirkuläre Verweise

Wenn ich die Modelle Book "und" Autor definiert ist, definierte ich eine Navigationseigenschaft auf die `Book` Klasse für die Buch-Autor-Beziehung, aber ich wurde nicht Navigationseigenschaft definiert, in der anderen Richtung.

Was geschieht, wenn Sie die entsprechende Navigationseigenschaft zum Hinzufügen der `Author` Klasse?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Leider ergibt sich ein Problem auf, wenn Sie die Modelle serialisieren. Wenn Sie die verwandten Daten laden, wird eine zirkuläre Objektdiagramm erstellt.

![](part-4/_static/image1.png)

Wenn der JSON oder XML-Formatierer versucht, das Diagramm zu serialisieren, wird eine Ausnahme ausgelöst. Die zwei Formatierungsprogrammen lösen verschiedene ausnahmemeldungen. Hier ist ein Beispiel für den JSON-Formatierer:

[!code-console[Main](part-4/samples/sample12.cmd)]

So sieht das XML-Formatierer aus:

[!code-xml[Main](part-4/samples/sample13.xml)]

Eine Lösung ist die Verwendung von DTOs, denen ich im nächsten Abschnitt beschrieben werden. Alternativ können Sie die JSON und XML-Formatierer zum Behandeln von Graph Zyklen konfigurieren. Weitere Informationen finden Sie unter [Behandlung von Objekt-Zirkelverweise](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Für dieses Lernprogramm müssen Sie nicht die `Author.Book` Navigationseigenschaft, damit Sie es lassen können.

> [!div class="step-by-step"]
> [Zurück](part-3.md)
> [Weiter](part-5.md)
