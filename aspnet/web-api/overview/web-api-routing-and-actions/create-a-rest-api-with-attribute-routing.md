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
ms.sourcegitcommit: 7f92990bad6a6cb901265d621dcbc136794f5f3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="49c80-102">Erstellen Sie eine REST-API mit Routing in ASP.NET Web API 2-Attribut</span><span class="sxs-lookup"><span data-stu-id="49c80-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="49c80-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49c80-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="49c80-104">Web-API 2 unterstützt einen neuen Typ wird aufgerufen, Routing, *routing-Attribut*.</span><span class="sxs-lookup"><span data-stu-id="49c80-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="49c80-105">Eine allgemeine Übersicht über routing-Attribut, finden Sie unter [Routing-Attribut in der Web-API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="49c80-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="49c80-106">In diesem Lernprogramm verwenden Sie routing-Attribut eine REST-API für eine Sammlung von Büchern zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="49c80-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="49c80-107">Die API unterstützt die folgenden Aktionen:</span><span class="sxs-lookup"><span data-stu-id="49c80-107">The API will support the following actions:</span></span>

| <span data-ttu-id="49c80-108">Aktion</span><span class="sxs-lookup"><span data-stu-id="49c80-108">Action</span></span> | <span data-ttu-id="49c80-109">Beispiel-URI</span><span class="sxs-lookup"><span data-stu-id="49c80-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="49c80-110">Ruft eine Liste aller Bücher.</span><span class="sxs-lookup"><span data-stu-id="49c80-110">Get a list of all books.</span></span> | <span data-ttu-id="49c80-111">/api/books</span><span class="sxs-lookup"><span data-stu-id="49c80-111">/api/books</span></span> |
| <span data-ttu-id="49c80-112">Ein Buch-ID abrufen</span><span class="sxs-lookup"><span data-stu-id="49c80-112">Get a book by ID.</span></span> | <span data-ttu-id="49c80-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="49c80-113">/api/books/1</span></span> |
| <span data-ttu-id="49c80-114">Abrufen von Details eines Buchs.</span><span class="sxs-lookup"><span data-stu-id="49c80-114">Get the details of a book.</span></span> | <span data-ttu-id="49c80-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="49c80-115">/api/books/1/details</span></span> |
| <span data-ttu-id="49c80-116">Abrufen einer Liste von Büchern von "Genre".</span><span class="sxs-lookup"><span data-stu-id="49c80-116">Get a list of books by genre.</span></span> | <span data-ttu-id="49c80-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="49c80-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="49c80-118">Abrufen einer Liste von Büchern nach Veröffentlichungsdatum.</span><span class="sxs-lookup"><span data-stu-id="49c80-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="49c80-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span><span class="sxs-lookup"><span data-stu-id="49c80-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="49c80-120">Ruft eine Liste von Büchern von einem bestimmten Autor.</span><span class="sxs-lookup"><span data-stu-id="49c80-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="49c80-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="49c80-121">/api/authors/1/books</span></span> |

<span data-ttu-id="49c80-122">Alle Methoden sind ohne Schreibzugriff (HTTP GET-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="49c80-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="49c80-123">Für die Datenschicht verwenden wir Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49c80-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="49c80-124">Datensätze müssen die folgenden Felder:</span><span class="sxs-lookup"><span data-stu-id="49c80-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="49c80-125">Id</span><span class="sxs-lookup"><span data-stu-id="49c80-125">ID</span></span>
- <span data-ttu-id="49c80-126">Titel</span><span class="sxs-lookup"><span data-stu-id="49c80-126">Title</span></span>
- <span data-ttu-id="49c80-127">Genre</span><span class="sxs-lookup"><span data-stu-id="49c80-127">Genre</span></span>
- <span data-ttu-id="49c80-128">Veröffentlichungsdatum</span><span class="sxs-lookup"><span data-stu-id="49c80-128">Publication date</span></span>
- <span data-ttu-id="49c80-129">Preis</span><span class="sxs-lookup"><span data-stu-id="49c80-129">Price</span></span>
- <span data-ttu-id="49c80-130">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="49c80-130">Description</span></span>
- <span data-ttu-id="49c80-131">AutorID (Fremdschlüssel mit einer Autorentabelle)</span><span class="sxs-lookup"><span data-stu-id="49c80-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="49c80-132">Für die meisten Anforderungen gibt die API jedoch eine Teilmenge dieser Daten (Title, Author und "Genre") zurück.</span><span class="sxs-lookup"><span data-stu-id="49c80-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="49c80-133">Die vollständigen Datensatz, der Client Anforderungen abrufen `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="49c80-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49c80-134">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="49c80-134">Prerequisites</span></span>

<span data-ttu-id="49c80-135">[Visual Studio-2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="49c80-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="49c80-136">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="49c80-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="49c80-137">Starten von Visual Studio ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="49c80-137">Start by running Visual Studio.</span></span> <span data-ttu-id="49c80-138">Aus der **Datei** klicken Sie im Menü **neu** und wählen Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="49c80-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="49c80-139">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="49c80-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="49c80-140">Klicken Sie unter **Visual C#-**Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="49c80-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="49c80-141">Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="49c80-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="49c80-142">Nennen Sie das Projekt &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="49c80-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="49c80-143">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="49c80-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="49c80-144">Klicken Sie unter "Hinzufügen von Ordnern und Verweise für core" Wählen Sie die **Web-API** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="49c80-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="49c80-145">Klicken Sie auf **erstellen Projekt**.</span><span class="sxs-lookup"><span data-stu-id="49c80-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="49c80-146">Dies erstellt ein Skeleton-Projekt, das für die Web-API-Funktionalität konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="49c80-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="49c80-147">Domänenmodelle</span><span class="sxs-lookup"><span data-stu-id="49c80-147">Domain Models</span></span>

<span data-ttu-id="49c80-148">Als Nächstes fügen Sie Klassen für Domänenmodelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="49c80-148">Next, add classes for domain models.</span></span> <span data-ttu-id="49c80-149">Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="49c80-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="49c80-150">Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="49c80-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="49c80-151">Nennen Sie die Klasse `Author`.</span><span class="sxs-lookup"><span data-stu-id="49c80-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="49c80-152">Ersetzen Sie den Code in Author.cs durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="49c80-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="49c80-153">Fügen Sie jetzt eine andere Klasse, die mit dem Namen `Book`.</span><span class="sxs-lookup"><span data-stu-id="49c80-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="49c80-154">Fügen Sie eine Web-API-Controller hinzu</span><span class="sxs-lookup"><span data-stu-id="49c80-154">Add a Web API Controller</span></span>

<span data-ttu-id="49c80-155">In diesem Schritt fügen wir einen Web-API-Controller, der als die Datenschicht Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="49c80-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="49c80-156">Drücken Sie STRG+UMSCHALT+B, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="49c80-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="49c80-157">Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle ermitteln, daher ist eine kompilierte Assembly So erstellen das Schema der Datenbank erforderlich.</span><span class="sxs-lookup"><span data-stu-id="49c80-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="49c80-158">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="49c80-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="49c80-159">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="49c80-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="49c80-160">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Lese-/schreibaktionen unter Verwendung von Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="49c80-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="49c80-161">In der **Controller hinzufügen** im Dialogfeld für **Controllernamen**, geben Sie &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="49c80-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="49c80-162">Wählen Sie die &quot;asynchrone Controlleraktionen verwenden&quot; Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="49c80-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="49c80-163">Für **Modellschemas**Option &quot;Buch&quot;.</span><span class="sxs-lookup"><span data-stu-id="49c80-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="49c80-164">(Wenn Sie sehen die `Book` Klasse, die in der Dropdownliste aufgeführt sind, stellen Sie sicher, dass Sie das Projekt erstellt.) Klicken Sie dann auf die Schaltfläche "+".</span><span class="sxs-lookup"><span data-stu-id="49c80-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="49c80-165">Klicken Sie auf **hinzufügen** in der **neuen Datenkontext** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="49c80-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="49c80-166">Klicken Sie auf **hinzufügen** in der **Controller hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="49c80-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="49c80-167">Das Gerüst Fügt eine Klasse namens `BooksController` , die die API-Controller definiert.</span><span class="sxs-lookup"><span data-stu-id="49c80-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="49c80-168">Sie fügt außerdem eine Klasse namens `BooksAPIContext` im Ordner Models, der den Datenkontext für Entity Framework definiert.</span><span class="sxs-lookup"><span data-stu-id="49c80-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="49c80-169">Ausgangswert für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="49c80-169">Seed the Database</span></span>

<span data-ttu-id="49c80-170">Wählen Sie im Menü Extras **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="49c80-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="49c80-171">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="49c80-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="49c80-172">Dieser Befehl erstellt einen Ordner Migrationen und fügt eine neue Codedatei mit dem Namen "Configuration.cs".</span><span class="sxs-lookup"><span data-stu-id="49c80-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="49c80-173">Öffnen Sie diese Datei, und fügen Sie folgenden Code zu der `Configuration.Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="49c80-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="49c80-174">Geben Sie die folgenden Befehle aus, klicken Sie im Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="49c80-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="49c80-175">Diese Befehle eine lokale Datenbank erstellen und Aufrufen der Seed-Methode, um die Datenbank aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="49c80-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="49c80-176">Fügen Sie DTO-Klassen</span><span class="sxs-lookup"><span data-stu-id="49c80-176">Add DTO Classes</span></span>

<span data-ttu-id="49c80-177">Wenn Sie die Anwendung jetzt ausführen und senden eine GET-Anforderung an /api/books/1, sieht die Antwort ähnlich der folgenden.</span><span class="sxs-lookup"><span data-stu-id="49c80-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="49c80-178">(Ich hinzugefügt Einzug zur besseren Lesbarkeit.)</span><span class="sxs-lookup"><span data-stu-id="49c80-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="49c80-179">Stattdessen werden diese Anforderung, die eine Teilmenge der Felder zurückgeben soll.</span><span class="sxs-lookup"><span data-stu-id="49c80-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="49c80-180">Darüber hinaus möchte ich den Namen des Autors, anstatt die Autor-ID zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="49c80-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="49c80-181">Zu diesem Zweck ändern wir die Controllermethoden zurückzugebenden eine *Datenübertragungsobjekt* (DTO) anstelle des EF-Modells.</span><span class="sxs-lookup"><span data-stu-id="49c80-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="49c80-182">Ein DTO ist ein Objekt, das entworfen wurde, nur für Daten ausführen.</span><span class="sxs-lookup"><span data-stu-id="49c80-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="49c80-183">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen** | **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="49c80-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="49c80-184">Benennen Sie den Ordner &quot;DTOs&quot;.</span><span class="sxs-lookup"><span data-stu-id="49c80-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="49c80-185">Fügen Sie eine Klasse mit dem Namen `BookDto` in den Ordner DTOs mit folgender Definition:</span><span class="sxs-lookup"><span data-stu-id="49c80-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="49c80-186">Fügen Sie eine andere Klasse, die mit dem Namen `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="49c80-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="49c80-187">Aktualisieren Sie als Nächstes die `BooksController` Klasse zurückzugebenden `BookDto` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="49c80-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="49c80-188">Wir verwenden die [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) Methode Projekt `Book` -Instanzen `BookDto` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="49c80-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="49c80-189">Hier ist der aktualisierte Code für die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="49c80-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="49c80-190">Ich gelöscht, die `PutBook`, `PostBook`, und `DeleteBook` Methoden, da sie nicht für dieses Lernprogramm benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="49c80-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="49c80-191">Wenn Sie die Anwendung auszuführen und /api/books/1 anfordern, sollte der Antworttext jetzt wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="49c80-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="49c80-192">Hinzufügen von Routenattributen</span><span class="sxs-lookup"><span data-stu-id="49c80-192">Add Route Attributes</span></span>

<span data-ttu-id="49c80-193">Konvertieren Sie als Nächstes den Controller, um das routing-Attribut verwenden.</span><span class="sxs-lookup"><span data-stu-id="49c80-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="49c80-194">Fügen Sie zunächst eine **Routenpräfix** -Attribut auf den Controller.</span><span class="sxs-lookup"><span data-stu-id="49c80-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="49c80-195">Dieses Attribut definiert die anfänglichen URI-Segmente für alle Methoden auf diesem Controller.</span><span class="sxs-lookup"><span data-stu-id="49c80-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="49c80-196">Fügen Sie dann **[Route]** Attribute für die Controlleraktionen, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="49c80-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="49c80-197">Die routenvorlage für jede Controllermethode ist das Präfix sowie der angegebenen Zeichenfolge der **Route** Attribut.</span><span class="sxs-lookup"><span data-stu-id="49c80-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="49c80-198">Für die `GetBook` -Methode, die routenvorlage enthält, die parametrisierte Zeichenfolge &quot;{-Id: Int}&quot;, dem entspricht, sofern das URI-Segment einen ganzzahligen Wert enthält.</span><span class="sxs-lookup"><span data-stu-id="49c80-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="49c80-199">Methode</span><span class="sxs-lookup"><span data-stu-id="49c80-199">Method</span></span> | <span data-ttu-id="49c80-200">Routenvorlage</span><span class="sxs-lookup"><span data-stu-id="49c80-200">Route Template</span></span> | <span data-ttu-id="49c80-201">Beispiel-URI</span><span class="sxs-lookup"><span data-stu-id="49c80-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="49c80-202">"-api/Books"</span><span class="sxs-lookup"><span data-stu-id="49c80-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="49c80-203">"api/books/{id:int}"</span><span class="sxs-lookup"><span data-stu-id="49c80-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="49c80-204">Abrufen von Details für das Offlineadressbuch</span><span class="sxs-lookup"><span data-stu-id="49c80-204">Get Book Details</span></span>

<span data-ttu-id="49c80-205">Um Buchdetails zu erhalten, der Client sendet eine GET-Anforderung `/api/books/{id}/details`, wobei *{Id}* ist die ID des Buchs.</span><span class="sxs-lookup"><span data-stu-id="49c80-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="49c80-206">Fügen Sie der `BooksController`-Klasse die folgende Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="49c80-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="49c80-207">Wenn Sie anfordern `/api/books/1/details`, die Antwort sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="49c80-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="49c80-208">Abrufen von Büchern "Genre"</span><span class="sxs-lookup"><span data-stu-id="49c80-208">Get Books By Genre</span></span>

<span data-ttu-id="49c80-209">Um eine Liste von Büchern in einer bestimmten "Genre" abzurufen, der Client sendet eine GET-Anforderung `/api/books/genre`, wobei *"Genre"* ist der Name des der "Genre".</span><span class="sxs-lookup"><span data-stu-id="49c80-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="49c80-210">(Beispiel: `/api/books/fantasy`)</span><span class="sxs-lookup"><span data-stu-id="49c80-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="49c80-211">Fügen Sie die folgende Methode hinzu `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="49c80-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="49c80-212">Hier werden wir eine Route definieren, die einen {"Genre"}-Parameter in der URI-Vorlage enthält.</span><span class="sxs-lookup"><span data-stu-id="49c80-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="49c80-213">Beachten Sie, dass Web-API kann diese zwei URIs unterscheiden und zu den verschiedenen Methoden weiterleiten:</span><span class="sxs-lookup"><span data-stu-id="49c80-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="49c80-214">Grund hierfür ist die `GetBook` Methode enthält eine Einschränkung, dass das Segment "Id" als ganze Zahl sein muss:</span><span class="sxs-lookup"><span data-stu-id="49c80-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="49c80-215">Wenn Sie /api/books/fantasy anfordern, sieht die Antwort wie folgt:</span><span class="sxs-lookup"><span data-stu-id="49c80-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="49c80-216">Abrufen von Büchern vom Autor</span><span class="sxs-lookup"><span data-stu-id="49c80-216">Get Books By Author</span></span>

<span data-ttu-id="49c80-217">Um eine Liste von einem Büchern für einen bestimmten Autor zu erhalten, der Client sendet eine GET-Anforderung `/api/authors/id/books`, wobei *Id* ist die ID des Autors.</span><span class="sxs-lookup"><span data-stu-id="49c80-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="49c80-218">Fügen Sie die folgende Methode hinzu `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="49c80-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="49c80-219">In diesem Beispiel ist interessant, da &quot;Bücher&quot; ist eine untergeordnete Ressource behandelt &quot;Autoren&quot;.</span><span class="sxs-lookup"><span data-stu-id="49c80-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="49c80-220">Dieses Vorgehen ist üblich, das in Rest-APIs.</span><span class="sxs-lookup"><span data-stu-id="49c80-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="49c80-221">Die Tilde (~) in der routenvorlage überschreibt das Routenpräfix in der **Routenpräfix** Attribut.</span><span class="sxs-lookup"><span data-stu-id="49c80-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="49c80-222">Abrufen von Büchern nach Veröffentlichungsdatum</span><span class="sxs-lookup"><span data-stu-id="49c80-222">Get Books By Publication Date</span></span>

<span data-ttu-id="49c80-223">Um eine Liste von Büchern nach Veröffentlichungsdatum zu erhalten, der Client sendet eine GET-Anforderung `/api/books/date/yyyy-mm-dd`, wobei *jjjj-mm-tt* ist das Datum.</span><span class="sxs-lookup"><span data-stu-id="49c80-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="49c80-224">Hier ist eine Möglichkeit hierfür ein:</span><span class="sxs-lookup"><span data-stu-id="49c80-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="49c80-225">Die `{pubdate:datetime}` Parameter entsprechend eingeschränkt ist ein **"DateTime"** Wert.</span><span class="sxs-lookup"><span data-stu-id="49c80-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="49c80-226">Dies funktioniert, allerdings handelt es sich tatsächlich schwächer einschränkend sein als wir möchten.</span><span class="sxs-lookup"><span data-stu-id="49c80-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="49c80-227">Diese URIs werden z. B. auch die Route entspricht:</span><span class="sxs-lookup"><span data-stu-id="49c80-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="49c80-228">Es ist nichts falsch mit dem diese URIs.</span><span class="sxs-lookup"><span data-stu-id="49c80-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="49c80-229">Allerdings können Sie die Route auf ein bestimmtes Format beschränken, indem Sie eine Einschränkung für reguläre Ausdrücke an die routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="49c80-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="49c80-230">Jetzt nur Datumsangaben in der Form &quot;jjjj-mm-tt&quot; entspricht.</span><span class="sxs-lookup"><span data-stu-id="49c80-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="49c80-231">Beachten Sie, dass wir nicht den regulären Ausdruck verwenden, um sicherzustellen, dass wir ein echtes Datum haben.</span><span class="sxs-lookup"><span data-stu-id="49c80-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="49c80-232">Die behandelt wird, wenn die Web-API wird versucht, konvertieren das URI-Segment in einem **"DateTime"** Instanz.</span><span class="sxs-lookup"><span data-stu-id="49c80-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="49c80-233">Ein ungültiges Datum z. B. "2012-47-99' kann nicht konvertiert werden, und der Client erhält einen 404-Fehler.</span><span class="sxs-lookup"><span data-stu-id="49c80-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="49c80-234">Sie können auch eine Schrägstrich-Trennzeichen unterstützen (`/api/books/date/yyyy/mm/dd`) durch Hinzufügen eines anderen **[Route]** Attribut mit einem anderen regulären Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="49c80-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="49c80-235">Es ist hier ein feinen, aber wichtige Detail.</span><span class="sxs-lookup"><span data-stu-id="49c80-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="49c80-236">Die zweite routenvorlage hat ein Platzhalterzeichen (\*) am Anfang der {Pubdate}-Parameter:</span><span class="sxs-lookup"><span data-stu-id="49c80-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="49c80-237">Dies weist dem Routingmodul, dass diese {Pubdate} der Rest des URI übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="49c80-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="49c80-238">Standardmäßig entspricht ein Vorlagenparameter ein einzelnes URI-Segment.</span><span class="sxs-lookup"><span data-stu-id="49c80-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="49c80-239">In diesem Fall möchten wir {Pubdate}, die mehrere URI-Segmenten umfassen:</span><span class="sxs-lookup"><span data-stu-id="49c80-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="49c80-240">Controllercode</span><span class="sxs-lookup"><span data-stu-id="49c80-240">Controller Code</span></span>

<span data-ttu-id="49c80-241">Hier ist der vollständige Code für die BooksController-Klasse.</span><span class="sxs-lookup"><span data-stu-id="49c80-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="49c80-242">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="49c80-242">Summary</span></span>

<span data-ttu-id="49c80-243">Routing-Attribut bietet Ihnen mehr Kontrolle und Flexibilität beim Entwerfen der URIs für Ihre API.</span><span class="sxs-lookup"><span data-stu-id="49c80-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
