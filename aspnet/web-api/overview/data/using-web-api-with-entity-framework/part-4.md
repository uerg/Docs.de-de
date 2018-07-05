---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Verarbeiten von Entitätsbeziehungen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 7759b828068d99f9975d56671e427ccf6e94aef6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400952"
---
<a name="handling-entity-relations"></a>Verarbeiten von Entitätsbeziehungen
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

Dieser Abschnitt beschreibt einige Details der wie EF auf verknüpfte Entitäten geladen, und das zirkuläre Navigationseigenschaften in Ihren Modellklassen zu behandeln. (Dieser Abschnitt enthält Hintergrundinformationen und ist nicht erforderlich, um das Lernprogramm abzuschließen. Wenn Sie möchten, fahren Sie mit [Teil 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Mittels Eager Load und Lazy Load geladen

Wenn Sie EF mit einer relationalen Datenbank verwenden zu können, ist es wichtig zu verstehen, wie EF zugehörige Daten lädt.

Es ist auch hilfreich, um die SQL-Abfragen anzuzeigen, die EF generiert. Um die SQL-Anweisung zu verfolgen, fügen Sie die folgende Codezeile, die `BookServiceContext` Konstruktor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Wenn Sie eine GET-Anforderung an /api/books senden, wird JSON wie folgt zurückgegeben:

[!code-console[Main](part-4/samples/sample2.cmd)]

Sie sehen, dass die Author-Eigenschaft null ist, obwohl das Buch eine gültige AutorID enthält. Das ist da EF die verknüpften Entitäten für den Autor nicht geladen werden. Das Ablaufverfolgungsprotokoll der SQL-Abfrage, bestätigt dies:

[!code-console[Main](part-4/samples/sample3.sql)]

Die SELECT-Anweisung ruft aus der Tabelle "Books", und verweist nicht auf die Tabelle erstellen.

Zu Referenzzwecken ist hier die Methode in der `BooksController` Klasse, die die Liste der Bücher zurückgibt.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Sehen wir uns an, wie wir den Autor im Rahmen der JSON-Daten zurückgeben kann. Es gibt drei Möglichkeiten zum Laden von verknüpfter Daten im Entity Framework: Eager Load und Lazy Load explizites Laden. Es gibt vor-und Nachteile mit jede Technik, daher es wichtig ist zu verstehen, wie sie funktionieren.

### <a name="eager-loading"></a>Eager Loading

Mit *Eager Load*, EF verknüpfte Entitäten als Teil der Abfrage für die ursprüngliche Datenbank geladen. Verwenden Sie zum Ausführen von Eager Load der **System.Data.Entity.Include** -Erweiterungsmethode.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Dadurch wird EF die Autor-Daten in der Abfrage enthalten. Wenn Sie diese Änderung vornehmen, führen Sie die app jetzt die JSON-Daten sieht folgendermaßen aus:

[!code-console[Main](part-4/samples/sample6.cmd)]

Das Ablaufverfolgungsprotokoll zeigt, dass EF eine Verknüpfung für die Tabellen Buch und Autor ausgeführt.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Lazy Loading

Mit lazy Loading lädt EF automatisch eine verknüpfte Entität, wenn die Navigationseigenschaft für diese Entität dereferenziert wird. Zum Aktivieren von lazy Loading machen Sie die Navigationseigenschaft virtuell. Z. B. in der Book-Klasse:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Betrachten Sie nun den folgenden Code:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Wenn lazy Loading aktiviert ist, den Zugriff auf die `Author` Eigenschaft `books[0]` bewirkt, dass EF zum Abfragen der Datenbank für den Autor.

Lazy Loading erfordert mehrere Trips zur Datenbank, da EF sendet eine Abfrage, die jedes Mal eine verknüpfte Entität abgerufen. Im Allgemeinen möchten Sie lazy Loading deaktiviert für Objekte, die Sie serialisieren. Das Serialisierungsprogramm muss alle Eigenschaften im Modell zu lesen, in dem löst die verknüpften Entitäten werden geladen. Hier sind z. B. die SQL-Abfragen, wenn EF die Liste der Bücher mit lazy Loading aktiviert serialisiert. Sie können sehen, dass EF drei separate Abfragen für die drei Autoren machen.

[!code-console[Main](part-4/samples/sample10.sql)]

Es gibt noch Mal, wenn Sie lazy Loading verwenden möchten. Eager Loading kann dazu führen, dass EF eine sehr komplexe Verknüpfung generieren. Oder möglicherweise verknüpfte Entitäten für eine kleine Teilmenge der Daten und Lazy Load wäre effizienter.

Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung ist datentransferobjekte (DTOs) anstelle von Entitätsobjekte zu serialisieren. Dieser Ansatz zeige ich später in diesem Artikel.

### <a name="explicit-loading"></a>Explizites Laden

Explizites Laden ist ähnlich wie beim verzögerten Laden, außer dass Sie die verwandten Daten explizit im Code können; Dies erfolgt nicht automatisch beim Zugriff auf eine Navigationseigenschaft. Explizites Laden gibt Ihnen mehr Kontrolle darüber, wann zum Laden von verwandter Daten, aber es ist zusätzlicher Code erforderlich. Weitere Informationen zu expliziten Laden, finden Sie unter [Laden von verknüpften Entitäten](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Navigationseigenschaften und Zirkelverweise

Wenn ich die Buch und Autor Modellen definiert, definierte ich eine Navigationseigenschaft, auf die `Book` -Klasse für den Buchautor Beziehung, aber ich eine Navigationseigenschaft nicht definiert, in die andere Richtung.

Was geschieht, wenn Sie die entsprechende Navigationseigenschaft zum Hinzufügen der `Author` Klasse?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Leider ergibt sich ein Problem auf, wenn Sie die Modelle serialisieren. Wenn Sie die verwandten Daten laden, wird ein zirkulärer Objektdiagramm erstellt.

![](part-4/_static/image1.png)

Wenn das JSON- oder XML-Formatierungsprogramm versucht, die das Diagramm zu serialisieren, wird eine Ausnahme ausgelöst. Die zwei Formatierungsprogrammen löst verschiedene ausnahmemeldungen. Hier ist ein Beispiel für das JSON-Formatierungsprogramm ein:

[!code-console[Main](part-4/samples/sample12.cmd)]

Hier ist das XML-Formatierungsprogramm ein:

[!code-xml[Main](part-4/samples/sample13.xml)]

Eine Lösung ist die Verwendung von DTOs, denen ich im nächsten Abschnitt beschrieben werden. Alternativ können Sie die JSON- und XML-Formatierer zum Behandeln von Graph-Zyklen konfigurieren. Weitere Informationen finden Sie unter [zirkuläre Objektverweise behandeln](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Für dieses Tutorial müssen Sie nicht die `Author.Book` Navigationseigenschaft, damit Sie ihn weglassen können.

> [!div class="step-by-step"]
> [Zurück](part-3.md)
> [Weiter](part-5.md)
