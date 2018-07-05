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
<a name="handling-entity-relations"></a><span data-ttu-id="72599-102">Verarbeiten von Entitätsbeziehungen</span><span class="sxs-lookup"><span data-stu-id="72599-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="72599-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72599-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="72599-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="72599-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="72599-105">Dieser Abschnitt beschreibt einige Details der wie EF auf verknüpfte Entitäten geladen, und das zirkuläre Navigationseigenschaften in Ihren Modellklassen zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="72599-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="72599-106">(Dieser Abschnitt enthält Hintergrundinformationen und ist nicht erforderlich, um das Lernprogramm abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="72599-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="72599-107">Wenn Sie möchten, fahren Sie mit [Teil 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="72599-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="72599-108">Mittels Eager Load und Lazy Load geladen</span><span class="sxs-lookup"><span data-stu-id="72599-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="72599-109">Wenn Sie EF mit einer relationalen Datenbank verwenden zu können, ist es wichtig zu verstehen, wie EF zugehörige Daten lädt.</span><span class="sxs-lookup"><span data-stu-id="72599-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="72599-110">Es ist auch hilfreich, um die SQL-Abfragen anzuzeigen, die EF generiert.</span><span class="sxs-lookup"><span data-stu-id="72599-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="72599-111">Um die SQL-Anweisung zu verfolgen, fügen Sie die folgende Codezeile, die `BookServiceContext` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="72599-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="72599-112">Wenn Sie eine GET-Anforderung an /api/books senden, wird JSON wie folgt zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="72599-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="72599-113">Sie sehen, dass die Author-Eigenschaft null ist, obwohl das Buch eine gültige AutorID enthält.</span><span class="sxs-lookup"><span data-stu-id="72599-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="72599-114">Das ist da EF die verknüpften Entitäten für den Autor nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="72599-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="72599-115">Das Ablaufverfolgungsprotokoll der SQL-Abfrage, bestätigt dies:</span><span class="sxs-lookup"><span data-stu-id="72599-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="72599-116">Die SELECT-Anweisung ruft aus der Tabelle "Books", und verweist nicht auf die Tabelle erstellen.</span><span class="sxs-lookup"><span data-stu-id="72599-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="72599-117">Zu Referenzzwecken ist hier die Methode in der `BooksController` Klasse, die die Liste der Bücher zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="72599-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="72599-118">Sehen wir uns an, wie wir den Autor im Rahmen der JSON-Daten zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="72599-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="72599-119">Es gibt drei Möglichkeiten zum Laden von verknüpfter Daten im Entity Framework: Eager Load und Lazy Load explizites Laden.</span><span class="sxs-lookup"><span data-stu-id="72599-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="72599-120">Es gibt vor-und Nachteile mit jede Technik, daher es wichtig ist zu verstehen, wie sie funktionieren.</span><span class="sxs-lookup"><span data-stu-id="72599-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="72599-121">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="72599-121">Eager Loading</span></span>

<span data-ttu-id="72599-122">Mit *Eager Load*, EF verknüpfte Entitäten als Teil der Abfrage für die ursprüngliche Datenbank geladen.</span><span class="sxs-lookup"><span data-stu-id="72599-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="72599-123">Verwenden Sie zum Ausführen von Eager Load der **System.Data.Entity.Include** -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="72599-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="72599-124">Dadurch wird EF die Autor-Daten in der Abfrage enthalten.</span><span class="sxs-lookup"><span data-stu-id="72599-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="72599-125">Wenn Sie diese Änderung vornehmen, führen Sie die app jetzt die JSON-Daten sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="72599-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="72599-126">Das Ablaufverfolgungsprotokoll zeigt, dass EF eine Verknüpfung für die Tabellen Buch und Autor ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="72599-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="72599-127">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="72599-127">Lazy Loading</span></span>

<span data-ttu-id="72599-128">Mit lazy Loading lädt EF automatisch eine verknüpfte Entität, wenn die Navigationseigenschaft für diese Entität dereferenziert wird.</span><span class="sxs-lookup"><span data-stu-id="72599-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="72599-129">Zum Aktivieren von lazy Loading machen Sie die Navigationseigenschaft virtuell.</span><span class="sxs-lookup"><span data-stu-id="72599-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="72599-130">Z. B. in der Book-Klasse:</span><span class="sxs-lookup"><span data-stu-id="72599-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="72599-131">Betrachten Sie nun den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="72599-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="72599-132">Wenn lazy Loading aktiviert ist, den Zugriff auf die `Author` Eigenschaft `books[0]` bewirkt, dass EF zum Abfragen der Datenbank für den Autor.</span><span class="sxs-lookup"><span data-stu-id="72599-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="72599-133">Lazy Loading erfordert mehrere Trips zur Datenbank, da EF sendet eine Abfrage, die jedes Mal eine verknüpfte Entität abgerufen.</span><span class="sxs-lookup"><span data-stu-id="72599-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="72599-134">Im Allgemeinen möchten Sie lazy Loading deaktiviert für Objekte, die Sie serialisieren.</span><span class="sxs-lookup"><span data-stu-id="72599-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="72599-135">Das Serialisierungsprogramm muss alle Eigenschaften im Modell zu lesen, in dem löst die verknüpften Entitäten werden geladen.</span><span class="sxs-lookup"><span data-stu-id="72599-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="72599-136">Hier sind z. B. die SQL-Abfragen, wenn EF die Liste der Bücher mit lazy Loading aktiviert serialisiert.</span><span class="sxs-lookup"><span data-stu-id="72599-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="72599-137">Sie können sehen, dass EF drei separate Abfragen für die drei Autoren machen.</span><span class="sxs-lookup"><span data-stu-id="72599-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="72599-138">Es gibt noch Mal, wenn Sie lazy Loading verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="72599-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="72599-139">Eager Loading kann dazu führen, dass EF eine sehr komplexe Verknüpfung generieren.</span><span class="sxs-lookup"><span data-stu-id="72599-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="72599-140">Oder möglicherweise verknüpfte Entitäten für eine kleine Teilmenge der Daten und Lazy Load wäre effizienter.</span><span class="sxs-lookup"><span data-stu-id="72599-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="72599-141">Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung ist datentransferobjekte (DTOs) anstelle von Entitätsobjekte zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="72599-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="72599-142">Dieser Ansatz zeige ich später in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="72599-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="72599-143">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="72599-143">Explicit Loading</span></span>

<span data-ttu-id="72599-144">Explizites Laden ist ähnlich wie beim verzögerten Laden, außer dass Sie die verwandten Daten explizit im Code können; Dies erfolgt nicht automatisch beim Zugriff auf eine Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="72599-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="72599-145">Explizites Laden gibt Ihnen mehr Kontrolle darüber, wann zum Laden von verwandter Daten, aber es ist zusätzlicher Code erforderlich.</span><span class="sxs-lookup"><span data-stu-id="72599-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="72599-146">Weitere Informationen zu expliziten Laden, finden Sie unter [Laden von verknüpften Entitäten](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="72599-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="72599-147">Navigationseigenschaften und Zirkelverweise</span><span class="sxs-lookup"><span data-stu-id="72599-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="72599-148">Wenn ich die Buch und Autor Modellen definiert, definierte ich eine Navigationseigenschaft, auf die `Book` -Klasse für den Buchautor Beziehung, aber ich eine Navigationseigenschaft nicht definiert, in die andere Richtung.</span><span class="sxs-lookup"><span data-stu-id="72599-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="72599-149">Was geschieht, wenn Sie die entsprechende Navigationseigenschaft zum Hinzufügen der `Author` Klasse?</span><span class="sxs-lookup"><span data-stu-id="72599-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="72599-150">Leider ergibt sich ein Problem auf, wenn Sie die Modelle serialisieren.</span><span class="sxs-lookup"><span data-stu-id="72599-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="72599-151">Wenn Sie die verwandten Daten laden, wird ein zirkulärer Objektdiagramm erstellt.</span><span class="sxs-lookup"><span data-stu-id="72599-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="72599-152">Wenn das JSON- oder XML-Formatierungsprogramm versucht, die das Diagramm zu serialisieren, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="72599-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="72599-153">Die zwei Formatierungsprogrammen löst verschiedene ausnahmemeldungen.</span><span class="sxs-lookup"><span data-stu-id="72599-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="72599-154">Hier ist ein Beispiel für das JSON-Formatierungsprogramm ein:</span><span class="sxs-lookup"><span data-stu-id="72599-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="72599-155">Hier ist das XML-Formatierungsprogramm ein:</span><span class="sxs-lookup"><span data-stu-id="72599-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="72599-156">Eine Lösung ist die Verwendung von DTOs, denen ich im nächsten Abschnitt beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="72599-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="72599-157">Alternativ können Sie die JSON- und XML-Formatierer zum Behandeln von Graph-Zyklen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="72599-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="72599-158">Weitere Informationen finden Sie unter [zirkuläre Objektverweise behandeln](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="72599-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="72599-159">Für dieses Tutorial müssen Sie nicht die `Author.Book` Navigationseigenschaft, damit Sie ihn weglassen können.</span><span class="sxs-lookup"><span data-stu-id="72599-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72599-160">[Zurück](part-3.md)
> [Weiter](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="72599-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
