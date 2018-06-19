---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Erstellen Sie die Datenübertragungsobjekte (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878684"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="7295e-102">Erstellen Sie die Datenübertragungsobjekte (DTOs)</span><span class="sxs-lookup"><span data-stu-id="7295e-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="7295e-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7295e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7295e-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="7295e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7295e-105">Rechts nun unsere Web-API macht Datenbankentitäten an den Client.</span><span class="sxs-lookup"><span data-stu-id="7295e-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="7295e-106">Der Client empfängt Daten, die direkt an den Datenbanktabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7295e-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="7295e-107">Allerdings ist, die nicht immer empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="7295e-107">However, that's not always a good idea.</span></span> <span data-ttu-id="7295e-108">In einigen Fällen möchten Sie die Form der Daten zu ändern, die an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="7295e-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="7295e-109">Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:</span><span class="sxs-lookup"><span data-stu-id="7295e-109">For example, you might want to:</span></span>

- <span data-ttu-id="7295e-110">Zirkuläre Verweise entfernen (Siehe vorherige Abschnitt).</span><span class="sxs-lookup"><span data-stu-id="7295e-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="7295e-111">Blenden Sie bestimmte Eigenschaften aus, die Clients nicht anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7295e-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="7295e-112">Lassen Sie einige Eigenschaften, um die Nutzlastgröße zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="7295e-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="7295e-113">Vereinfachen von Objektdiagrammen, die geschachtelten Objekte, um sie für Clients erleichtern enthalten.</span><span class="sxs-lookup"><span data-stu-id="7295e-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="7295e-114">Vermeiden Sie "stark Posten von Beiträgen" Sicherheitsrisiken.</span><span class="sxs-lookup"><span data-stu-id="7295e-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="7295e-115">(Siehe [Modellvalidierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) Nähere Informationen zu senden.)</span><span class="sxs-lookup"><span data-stu-id="7295e-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="7295e-116">Entkoppeln Sie die Dienstebene aus der Datenbankebene.</span><span class="sxs-lookup"><span data-stu-id="7295e-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="7295e-117">Um dies zu erreichen, können Sie definieren eine *Datenübertragungsobjekt* (DTO).</span><span class="sxs-lookup"><span data-stu-id="7295e-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="7295e-118">Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="7295e-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="7295e-119">Sehen wir, die Funktionsweise der Book-Entität.</span><span class="sxs-lookup"><span data-stu-id="7295e-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="7295e-120">Fügen Sie im Ordner Models zwei DTO Klassen hinzu:</span><span class="sxs-lookup"><span data-stu-id="7295e-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="7295e-121">Die `BookDetailDTO` Klasse enthält alle Eigenschaften aus dem Buch-Modell, außer dass `AuthorName` ist eine Zeichenfolge, die den Namen des Autors enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="7295e-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="7295e-122">Die `BookDTO` Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="7295e-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="7295e-123">Als Nächstes ersetzen Sie die beiden GET-Methoden in der `BooksController` -Klasse, mit Versionen, die DTOs zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="7295e-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="7295e-124">Wir verwenden die LINQ **wählen** Anweisung Buch von Entitäten in DTOs zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="7295e-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="7295e-125">Hier wird die generiert wird, durch die neue SQL `GetBooks` Methode.</span><span class="sxs-lookup"><span data-stu-id="7295e-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="7295e-126">Beachten Sie, dass EF LINQ übersetzt **wählen** in eine SQL SELECT-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="7295e-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="7295e-127">Ändern Sie abschließend die `PostBook` Methode, um ein DTO zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="7295e-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="7295e-128">In diesem Lernprogramm möchten wir in DTOs manuell im Code konvertieren.</span><span class="sxs-lookup"><span data-stu-id="7295e-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="7295e-129">Eine andere Möglichkeit ist die Verwendung eine Bibliothek wie [AutoMapper](http://automapper.org/) , die die Konvertierung automatisch verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7295e-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="7295e-130">[Zurück](part-4.md)
> [Weiter](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="7295e-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
