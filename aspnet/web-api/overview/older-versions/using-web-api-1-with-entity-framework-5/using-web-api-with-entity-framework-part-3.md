---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Controllers Admin | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="283a3-102">Teil 3: Erstellen eines Admin-Controllers</span><span class="sxs-lookup"><span data-stu-id="283a3-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="283a3-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="283a3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="283a3-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="283a3-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="283a3-105">Fügen Sie einen Controller-Administrator hinzu</span><span class="sxs-lookup"><span data-stu-id="283a3-105">Add an Admin Controller</span></span>

<span data-ttu-id="283a3-106">In diesem Abschnitt fügen wir einen Web-API-Controller, der CRUD unterstützt (erstellen, lesen, aktualisieren und löschen) Vorgänge für Produkte.</span><span class="sxs-lookup"><span data-stu-id="283a3-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="283a3-107">Der Controller wird Entity Framework verwendet, für die Kommunikation mit der Datenbankebene.</span><span class="sxs-lookup"><span data-stu-id="283a3-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="283a3-108">Nur Administratoren werden können diesen Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="283a3-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="283a3-109">Kunden werden die Produkte über einen anderen Controller zugreifen.</span><span class="sxs-lookup"><span data-stu-id="283a3-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="283a3-110">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="283a3-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="283a3-111">Wählen Sie **hinzufügen** und dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="283a3-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="283a3-112">In der **Controller hinzufügen** Dialogfeld Namen des Controllers `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="283a3-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="283a3-113">Klicken Sie unter **Vorlage**Option &quot;API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="283a3-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="283a3-114">Klicken Sie unter **Modellschemas**, wählen Sie "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="283a3-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="283a3-115">Klicken Sie unter **Datenkontext**Option "&lt;neuen Datenkontext&gt;".</span><span class="sxs-lookup"><span data-stu-id="283a3-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="283a3-116">Wenn die **Modellschemas** Dropdownliste zeigt keine Modellklassen stellen Sie sicher, dass Sie das Projekt kompiliert.</span><span class="sxs-lookup"><span data-stu-id="283a3-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="283a3-117">Entity Framework verwendet Reflektion, damit sie die kompilierte Assembly benötigt.</span><span class="sxs-lookup"><span data-stu-id="283a3-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="283a3-118">Auswählen von "&lt;neuen Datenkontext&gt;" wird geöffnet, die **neuen Datenkontext** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="283a3-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="283a3-119">Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="283a3-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="283a3-120">Klicken Sie auf **OK** beim Schließen der **neuen Datenkontext** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="283a3-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="283a3-121">In der **Controller hinzufügen** Dialogfeld klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="283a3-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="283a3-122">Hier ist, was dem Projekt hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="283a3-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="283a3-123">Eine Klasse namens `OrdersContext` abgeleitet, die **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="283a3-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="283a3-124">Diese Klasse stellt die Verbindung zwischen den POCO-Modellen und die Datenbank bereit.</span><span class="sxs-lookup"><span data-stu-id="283a3-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="283a3-125">Eine Web-API-Controller mit dem Namen `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="283a3-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="283a3-126">Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="283a3-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="283a3-127">Er verwendet die `OrdersContext` Klasse für die Kommunikation mit Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="283a3-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="283a3-128">Eine neue Datenbank-Verbindungszeichenfolge in der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="283a3-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="283a3-129">Öffnen Sie die OrdersContext.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="283a3-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="283a3-130">Beachten Sie, dass der Konstruktor gibt den Namen der Datenbank-Verbindungszeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="283a3-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="283a3-131">Dieser Name bezieht sich auf die Verbindungszeichenfolge, die Datei "Web.config" hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="283a3-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="283a3-132">Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="283a3-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="283a3-133">Ein **DbSet** stellt eine Reihe von Entitäten, die abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="283a3-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="283a3-134">Hier wird die vollständige Auflistung für die `OrdersContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="283a3-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="283a3-135">Die `AdminController` Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionalität zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="283a3-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="283a3-136">Jede Methode entspricht ein URI, der der Client aufrufen kann:</span><span class="sxs-lookup"><span data-stu-id="283a3-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="283a3-137">Controllermethode</span><span class="sxs-lookup"><span data-stu-id="283a3-137">Controller Method</span></span> | <span data-ttu-id="283a3-138">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="283a3-138">Description</span></span> | <span data-ttu-id="283a3-139">URI</span><span class="sxs-lookup"><span data-stu-id="283a3-139">URI</span></span> | <span data-ttu-id="283a3-140">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="283a3-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="283a3-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="283a3-141">GetProducts</span></span> | <span data-ttu-id="283a3-142">Ruft alle Produkte ab.</span><span class="sxs-lookup"><span data-stu-id="283a3-142">Gets all products.</span></span> | <span data-ttu-id="283a3-143">API-Produkte</span><span class="sxs-lookup"><span data-stu-id="283a3-143">api/products</span></span> | <span data-ttu-id="283a3-144">GET</span><span class="sxs-lookup"><span data-stu-id="283a3-144">GET</span></span> |
| <span data-ttu-id="283a3-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="283a3-145">GetProduct</span></span> | <span data-ttu-id="283a3-146">Sucht nach einem Produkt nach ID auf.</span><span class="sxs-lookup"><span data-stu-id="283a3-146">Finds a product by ID.</span></span> | <span data-ttu-id="283a3-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="283a3-147">api/products/*id*</span></span> | <span data-ttu-id="283a3-148">GET</span><span class="sxs-lookup"><span data-stu-id="283a3-148">GET</span></span> |
| <span data-ttu-id="283a3-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="283a3-149">PutProduct</span></span> | <span data-ttu-id="283a3-150">Aktualisiert ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="283a3-150">Updates a product.</span></span> | <span data-ttu-id="283a3-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="283a3-151">api/products/*id*</span></span> | <span data-ttu-id="283a3-152">PUT</span><span class="sxs-lookup"><span data-stu-id="283a3-152">PUT</span></span> |
| <span data-ttu-id="283a3-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="283a3-153">PostProduct</span></span> | <span data-ttu-id="283a3-154">Erstellt ein neues Produkt.</span><span class="sxs-lookup"><span data-stu-id="283a3-154">Creates a new product.</span></span> | <span data-ttu-id="283a3-155">API-Produkte</span><span class="sxs-lookup"><span data-stu-id="283a3-155">api/products</span></span> | <span data-ttu-id="283a3-156">BEREITSTELLEN</span><span class="sxs-lookup"><span data-stu-id="283a3-156">POST</span></span> |
| <span data-ttu-id="283a3-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="283a3-157">DeleteProduct</span></span> | <span data-ttu-id="283a3-158">Löscht ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="283a3-158">Deletes a product.</span></span> | <span data-ttu-id="283a3-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="283a3-159">api/products/*id*</span></span> | <span data-ttu-id="283a3-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="283a3-160">DELETE</span></span> |

<span data-ttu-id="283a3-161">Jede Methode ruft `OrdersContext` zum Abfragen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="283a3-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="283a3-162">Aufrufen von Methoden, die die Auflistung (PUT, POST und DELETE) ändern `db.SaveChanges` damit die Änderungen in der Datenbank beibehalten.</span><span class="sxs-lookup"><span data-stu-id="283a3-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="283a3-163">Controller sind pro HTTP-Anforderung erstellt, und dann verworfen, daher ist es erforderlich, um Änderungen beizubehalten, bevor eine Methode zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="283a3-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="283a3-164">Hinzufügen eines Datenbankinitialisierers</span><span class="sxs-lookup"><span data-stu-id="283a3-164">Add a Database Initializer</span></span>

<span data-ttu-id="283a3-165">Entity Framework ist ein nützliches Feature, mit dem Sie die füllen Sie die Datenbank beim Start und die Datenbank automatisch neu, wenn die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="283a3-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="283a3-166">Diese Funktion ist nützlich während der Entwicklung, da Sie immer einige Testdaten verfügen, auch wenn Sie die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="283a3-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="283a3-167">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner Models, und erstellen Sie eine neue Klasse mit dem Namen `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="283a3-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="283a3-168">Fügen Sie in der folgenden Implementierung:</span><span class="sxs-lookup"><span data-stu-id="283a3-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="283a3-169">Durch Erben von der **DropCreateDatabaseIfModelChanges** -Klasse, sagen wir Entity Framework, die Datenbank abzulegen, wenn wir die Modellklassen ändern.</span><span class="sxs-lookup"><span data-stu-id="283a3-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="283a3-170">Wenn Entity Framework erstellt (oder neu erstellt) die Datenbank, ruft er die **Ausgangswert** Methode, um die Tabellen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="283a3-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="283a3-171">Wir verwenden die **Ausgangswert** Methode, um einige Beispiel-Produkte sowie eine Beispiel-Bestellung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="283a3-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="283a3-172">Diese Funktion eignet sich hervorragend für das Testen, aber verwenden Sie nicht die **DropCreateDatabaseIfModelChanges** Klasse in der Produktion, da Ihre Daten beeinträchtigt werden könnte, wenn jemand eine Modellklasse ändert.</span><span class="sxs-lookup"><span data-stu-id="283a3-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="283a3-173">"Global.asax" öffnen, und fügen Sie folgenden Code, der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="283a3-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="283a3-174">Senden Sie eine Anforderung an den Controller</span><span class="sxs-lookup"><span data-stu-id="283a3-174">Send a Request to the Controller</span></span>

<span data-ttu-id="283a3-175">An diesem Punkt wir noch keinen Clientcode geschrieben, aber Sie können die Web-API mit einem Webbrowser oder ein HTTP-debugging-wie z. B. Tools Aufrufen [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="283a3-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="283a3-176">Drücken Sie F5, um mit dem Debuggen beginnen, in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="283a3-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="283a3-177">Ihr Browser wird geöffnet, um `http://localhost:*portnum*/`, wobei *Portnum* ist eine Portnummer.</span><span class="sxs-lookup"><span data-stu-id="283a3-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="283a3-178">Senden Sie eine HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="283a3-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="283a3-179">Die erste Anforderung möglicherweise nur langsam abgeschlossen ist, möglicherweise Entify Framework zum Erstellen und den Ausgangswert für der Datenbank muss.</span><span class="sxs-lookup"><span data-stu-id="283a3-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="283a3-180">Die Antwort sollte etwa ähnlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="283a3-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="283a3-181">[Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="283a3-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
