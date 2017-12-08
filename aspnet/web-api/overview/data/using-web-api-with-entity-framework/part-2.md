---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "Hinzufügen von Modellen und Controller | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: b75eae11fd99b60864256f79d4770a3007487964
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="add-models-and-controllers"></a><span data-ttu-id="0dec4-102">Hinzufügen von Modellen und Controllern</span><span class="sxs-lookup"><span data-stu-id="0dec4-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="0dec4-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0dec4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0dec4-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="0dec4-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0dec4-105">In diesem Abschnitt fügen Sie ein Modellklassen, die die Datenbankentitäten zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0dec4-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="0dec4-106">Dann können Sie Web-API-Controller hinzufügen, die CRUD-Vorgänge für die Entitäten ausführen.</span><span class="sxs-lookup"><span data-stu-id="0dec4-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="0dec4-107">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="0dec4-107">Add Model Classes</span></span>

<span data-ttu-id="0dec4-108">In diesem Lernprogramm erstellen wir die Datenbank mithilfe des "Code First" Ansatzes zu Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="0dec4-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="0dec4-109">Mit Code First Schreiben von C#-Klassen, die entsprechen an den Datenbanktabellen und EF erstellt die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0dec4-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="0dec4-110">(Weitere Informationen finden Sie unter [Ansätze zur Entity Framework-Entwicklung](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="0dec4-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="0dec4-111">Beginnen wir unsere Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren.</span><span class="sxs-lookup"><span data-stu-id="0dec4-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="0dec4-112">Wir werden die folgenden POCOs erstellen:</span><span class="sxs-lookup"><span data-stu-id="0dec4-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="0dec4-113">Autor</span><span class="sxs-lookup"><span data-stu-id="0dec4-113">Author</span></span>
- <span data-ttu-id="0dec4-114">Adressbuch</span><span class="sxs-lookup"><span data-stu-id="0dec4-114">Book</span></span>

<span data-ttu-id="0dec4-115">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="0dec4-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="0dec4-116">Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="0dec4-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="0dec4-117">Nennen Sie die Klasse `Author`.</span><span class="sxs-lookup"><span data-stu-id="0dec4-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="0dec4-118">Ersetzen Sie alle Standardcode in Author.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="0dec4-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="0dec4-119">Fügen Sie eine andere Klasse, die mit dem Namen `Book`, durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="0dec4-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="0dec4-120">Entity Framework werden diese Modelle verwenden, um Tabellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0dec4-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="0dec4-121">Für jedes Modell die `Id` Eigenschaft werden die Primärschlüsselspalte der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="0dec4-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="0dec4-122">In der Book-Klasse die `AuthorId` definiert einen Fremdschlüssel in der `Author` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="0dec4-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="0dec4-123">(Aus Gründen der Einfachheit, habe ich vorausgesetzt, dass jedes Buch der Autor ein einzelnes hat.) Die Book-Klasse enthält außerdem eine Navigationseigenschaft, die verwandte `Author`.</span><span class="sxs-lookup"><span data-stu-id="0dec4-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="0dec4-124">Können Sie den Zugriff auf die Navigationseigenschaft `Author` im Code.</span><span class="sxs-lookup"><span data-stu-id="0dec4-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="0dec4-125">Ich mehr über Navigationseigenschaften in Teil 4, sagen Sie [Behandlung von Entitätsbeziehungen](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="0dec4-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="0dec4-126">Hinzufügen von Web-API-Controller</span><span class="sxs-lookup"><span data-stu-id="0dec4-126">Add Web API Controllers</span></span>

<span data-ttu-id="0dec4-127">In diesem Abschnitt fügen wir Web-API-Controller, die CRUD-Vorgänge zu unterstützen (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="0dec4-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="0dec4-128">Controller werden Entity Framework verwendet, für die Kommunikation mit der Datenbankebene.</span><span class="sxs-lookup"><span data-stu-id="0dec4-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="0dec4-129">Erstens können Sie die Datei Controllers/ValuesController.cs löschen.</span><span class="sxs-lookup"><span data-stu-id="0dec4-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="0dec4-130">Diese Datei enthält einen Beispiel für Web-API-Controller, jedoch ist dies nicht erforderlich für dieses Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="0dec4-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="0dec4-131">Als Nächstes erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="0dec4-131">Next, build the project.</span></span> <span data-ttu-id="0dec4-132">Das Gerüst für die Web-API verwendet Reflektion, um die Modellklassen zu finden, damit sie die kompilierte Assembly benötigt.</span><span class="sxs-lookup"><span data-stu-id="0dec4-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="0dec4-133">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="0dec4-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0dec4-134">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="0dec4-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="0dec4-135">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="0dec4-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="0dec4-136">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0dec4-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="0dec4-137">In der **Controller hinzufügen** Dialogfeld, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="0dec4-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="0dec4-138">In der **Modellschemas** Dropdownliste, wählen die `Author` Klasse.</span><span class="sxs-lookup"><span data-stu-id="0dec4-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="0dec4-139">(Wenn Sie in der Dropdownliste aufgeführt nicht angezeigt wird, stellen Sie sicher, dass Sie das Projekt erstellt.)</span><span class="sxs-lookup"><span data-stu-id="0dec4-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="0dec4-140">Überprüfen Sie "Async-Controlleraktionen verwenden".</span><span class="sxs-lookup"><span data-stu-id="0dec4-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="0dec4-141">Lassen Sie den Namen des Controllers als &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0dec4-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="0dec4-142">Klicken Sie auf Pluszeichen (+) neben Schaltfläche **Datenkontextklasse**.</span><span class="sxs-lookup"><span data-stu-id="0dec4-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="0dec4-143">In der **neuen Datenkontext** Dialogfeld, lassen Sie den Standardnamen, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0dec4-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="0dec4-144">Klicken Sie auf **hinzufügen** zum Abschließen der **Controller hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="0dec4-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="0dec4-145">Das Dialogfeld fügt zwei Klassen zu Ihrem Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="0dec4-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="0dec4-146">`AuthorsController`definiert einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="0dec4-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="0dec4-147">Der Controller implementiert die REST-API, mit denen Clients CRUD-Vorgänge in der Liste der Autoren ausführen.</span><span class="sxs-lookup"><span data-stu-id="0dec4-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="0dec4-148">`BookServiceContext`verwaltet von Entitätsobjekten während der Laufzeit, Auffüllen mit Daten aus einer Datenbank, Nachverfolgen von Änderungen und Beibehalten von Daten in die Datenbank Objekte enthält.</span><span class="sxs-lookup"><span data-stu-id="0dec4-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="0dec4-149">Es erbt von `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0dec4-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="0dec4-150">Nun erstellen Sie das Projekt erneut.</span><span class="sxs-lookup"><span data-stu-id="0dec4-150">At this point, build the project again.</span></span> <span data-ttu-id="0dec4-151">Kehren Sie nun über die gleichen Schritte zum Hinzufügen von eines API-Controllers für `Book` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="0dec4-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="0dec4-152">Wählen Sie dieses Mal `Book` für die Modellklasse, und wählen Sie die vorhandene `BookServiceContext` Datenkontextklasse-Klasse.</span><span class="sxs-lookup"><span data-stu-id="0dec4-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="0dec4-153">(Erstellen Sie keinen neuen Datenkontext.) Klicken Sie auf **hinzufügen** Controller hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0dec4-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
<span data-ttu-id="0dec4-154">[Zurück](part-1.md)
[Weiter](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="0dec4-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
