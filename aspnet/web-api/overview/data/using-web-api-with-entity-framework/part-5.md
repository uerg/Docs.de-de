---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Erstellen Sie die Datenübertragungsobjekte (DTOs) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: a1be1b72559aaffdf530402313f58d6c2e25b189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828162"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="2f8e2-102">Erstellen Sie die Datenübertragungsobjekte (DTOs)</span><span class="sxs-lookup"><span data-stu-id="2f8e2-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="2f8e2-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2f8e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2f8e2-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="2f8e2-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2f8e2-105">Die Datenbankentitäten an den Client, macht rechts nun unsere Web-API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="2f8e2-106">Der Client empfängt Daten, die direkt von Datenbanktabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="2f8e2-107">Allerdings ist, die nicht immer eine gute Idee.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-107">However, that's not always a good idea.</span></span> <span data-ttu-id="2f8e2-108">Manchmal möchten Sie die Form der Daten zu ändern, die an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="2f8e2-109">Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:</span><span class="sxs-lookup"><span data-stu-id="2f8e2-109">For example, you might want to:</span></span>

- <span data-ttu-id="2f8e2-110">Zirkuläre Verweise entfernen (Siehe vorheriger Abschnitt).</span><span class="sxs-lookup"><span data-stu-id="2f8e2-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="2f8e2-111">Blenden Sie bestimmte Eigenschaften, die Clients sollten nicht anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="2f8e2-112">Lassen Sie einige Eigenschaften Weg, um die Nutzlastgröße zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="2f8e2-113">Vereinfachen von Objektdiagrammen, die geschachtelte Objekte, um die bequemer für Clients zu enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="2f8e2-114">Vermeiden Sie "overposting" Sicherheitsrisiken.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="2f8e2-115">(Finden Sie unter [Modellvalidierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) eine Erläuterung der overposting.)</span><span class="sxs-lookup"><span data-stu-id="2f8e2-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="2f8e2-116">Entkoppeln von Ihrer Dienstebene aus der Datenbankebene.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="2f8e2-117">Um dies zu erreichen, können Sie definieren eine *Datenübertragungsobjekt* (DTO).</span><span class="sxs-lookup"><span data-stu-id="2f8e2-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="2f8e2-118">Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="2f8e2-119">Sehen wir uns an, wie dies mit der Entität Buch funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="2f8e2-120">Fügen Sie in den Ordner "Models" zwei DTO-Klassen hinzu:</span><span class="sxs-lookup"><span data-stu-id="2f8e2-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="2f8e2-121">Die `BookDetailDTO` Klasse enthält alle Eigenschaften aus dem Buch-Modell, außer dass `AuthorName` ist eine Zeichenfolge, die der Name des Autors enthalten wird.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="2f8e2-122">Die `BookDTO` -Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="2f8e2-123">Ersetzen Sie als Nächstes die beiden GET-Methoden in der `BooksController` Klasse, die mit Versionen, die DTOs zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="2f8e2-124">Wir verwenden die LINQ **wählen** -Anweisung von buchentitäten in DTOs zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="2f8e2-125">Hier ist die SQL-generiert, durch die neue `GetBooks` Methode.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="2f8e2-126">Sie sehen, dass EF LINQ übersetzt **wählen** in eine SQL-SELECT-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="2f8e2-127">Ändern Sie abschließend die `PostBook` Methode, um ein DTO zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="2f8e2-128">In diesem Tutorial konvertieren zu DTOs manuell im Code wir.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="2f8e2-129">Eine weitere Möglichkeit ist die Verwendung von einer Bibliothek wie [AutoMapper](http://automapper.org/) , die die Konvertierung automatisch verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2f8e2-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="2f8e2-130">[Zurück](part-4.md)
> [Weiter](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="2f8e2-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
