---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt- und Order-Controllern | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 08abc66624526f4b1931231d114158afe8b63247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382847"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="329c6-102">Teil 6: Erstellen von Produkt- und in der Reihenfolge Controller</span><span class="sxs-lookup"><span data-stu-id="329c6-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="329c6-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="329c6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="329c6-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="329c6-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="329c6-105">Hinzufügen eines Controllers Produkte</span><span class="sxs-lookup"><span data-stu-id="329c6-105">Add a Products Controller</span></span>

<span data-ttu-id="329c6-106">Der Administrator-Controller ist für Benutzer, die über Administratorrechte verfügen.</span><span class="sxs-lookup"><span data-stu-id="329c6-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="329c6-107">Kunden, auf der anderen Seite können Produkte anzeigen, jedoch kann nicht erstellen, aktualisieren oder löschen.</span><span class="sxs-lookup"><span data-stu-id="329c6-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="329c6-108">Wir können problemlos den Zugriff auf die POST-, PUT- und Delete-Methoden, einschränken, bei die Get-Methoden geöffnet bleibt.</span><span class="sxs-lookup"><span data-stu-id="329c6-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="329c6-109">Aber sehen Sie sich die Daten, die für ein Produkt zurückgegeben werden:</span><span class="sxs-lookup"><span data-stu-id="329c6-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="329c6-110">Die `ActualCost` Eigenschaft sollte nicht für Kunden sichtbar sein!</span><span class="sxs-lookup"><span data-stu-id="329c6-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="329c6-111">Die Lösung besteht darin, definieren Sie eine *Datenübertragungsobjekt* (DTO), umfasst eine Teilmenge der Eigenschaften, die für Kunden sichtbar sein sollen.</span><span class="sxs-lookup"><span data-stu-id="329c6-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="329c6-112">Wir verwenden LINQ zum Projekt `Product` -Instanzen `ProductDTO` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="329c6-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="329c6-113">Fügen Sie eine Klasse, die mit dem Namen `ProductDTO` zum Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="329c6-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="329c6-114">Nun fügen Sie den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="329c6-114">Now add the controller.</span></span> <span data-ttu-id="329c6-115">Klicken Sie im Projektmappen-Explorer den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="329c6-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="329c6-116">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="329c6-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="329c6-117">In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="329c6-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="329c6-118">Klicken Sie unter **Vorlage**Option **leeren API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="329c6-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="329c6-119">Ersetzen Sie alles, was in der Quelldatei an, mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="329c6-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="329c6-120">Der Controller verwendet weiterhin die `OrdersContext` zum Abfragen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="329c6-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="329c6-121">Aber anstatt `Product` Instanzen direkt, wir rufen `MapProducts` , diese auf projizieren `ProductDTO` Instanzen:</span><span class="sxs-lookup"><span data-stu-id="329c6-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="329c6-122">Die `MapProducts` Methode gibt ein **"IQueryable"**, sodass wir das Ergebnis mit anderen Abfrageparametern verfassen können.</span><span class="sxs-lookup"><span data-stu-id="329c6-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="329c6-123">Sehen Sie in der `GetProduct` -Methode, die Fügt eine **, in denen** -Klausel, um die Abfrage:</span><span class="sxs-lookup"><span data-stu-id="329c6-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="329c6-124">Fügen Sie eine Orders-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="329c6-124">Add an Orders Controller</span></span>

<span data-ttu-id="329c6-125">Als Nächstes fügen Sie einen Controller, mit dem Benutzer erstellen und Anzeigen von Bestellungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="329c6-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="329c6-126">Wir beginnen mit einem anderen DTO.</span><span class="sxs-lookup"><span data-stu-id="329c6-126">We'll start with another DTO.</span></span> <span data-ttu-id="329c6-127">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine Klasse, die mit dem Namen `OrderDTO` verwenden Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="329c6-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="329c6-128">Nun fügen Sie den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="329c6-128">Now add the controller.</span></span> <span data-ttu-id="329c6-129">Klicken Sie im Projektmappen-Explorer den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="329c6-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="329c6-130">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="329c6-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="329c6-131">In der **Controller hinzufügen** im Dialogfeld die folgenden Optionen festlegen:</span><span class="sxs-lookup"><span data-stu-id="329c6-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="329c6-132">Klicken Sie unter **Controllername**, geben Sie "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="329c6-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="329c6-133">Klicken Sie unter **Vorlage**Option "API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entitätsframework".</span><span class="sxs-lookup"><span data-stu-id="329c6-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="329c6-134">Klicken Sie unter **Modellklasse**Option &quot;Reihenfolge (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="329c6-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="329c6-135">Klicken Sie unter **Datenkontextklasse**Option &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="329c6-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="329c6-136">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="329c6-136">Click **Add**.</span></span> <span data-ttu-id="329c6-137">Dadurch wird eine Datei namens OrdersController.cs hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="329c6-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="329c6-138">Als Nächstes müssen wir die Standardimplementierung des Controllers zu ändern.</span><span class="sxs-lookup"><span data-stu-id="329c6-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="329c6-139">Löschen Sie zuerst die `PutOrder` und `DeleteOrder` Methoden.</span><span class="sxs-lookup"><span data-stu-id="329c6-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="329c6-140">In diesem Beispiel nicht Kunden ändern oder löschen vorhandene Aufträge.</span><span class="sxs-lookup"><span data-stu-id="329c6-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="329c6-141">In einer realen Anwendung benötigen Sie viele Back-End-Logik zum Behandeln dieser Fälle.</span><span class="sxs-lookup"><span data-stu-id="329c6-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="329c6-142">Geliefert (z. B. wurde die Bestellung bereits?)</span><span class="sxs-lookup"><span data-stu-id="329c6-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="329c6-143">Ändern der `GetOrders` Methode, um nur die Aufträge zurückzugeben, die dem Benutzer gehören:</span><span class="sxs-lookup"><span data-stu-id="329c6-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="329c6-144">Ändern der `GetOrder` -Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="329c6-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="329c6-145">Hier sind die Änderungen, die wir der Methode vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="329c6-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="329c6-146">Der Rückgabewert ist ein `OrderDTO` Instanz statt einer `Order`.</span><span class="sxs-lookup"><span data-stu-id="329c6-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="329c6-147">Wenn wir die Datenbank für die Bestellung abgefragt wird, verwenden wir die [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) Methode, um die zugehörigen abrufen `OrderDetail` und `Product` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="329c6-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="329c6-148">Wir vereinfachen das Ergebnis mithilfe einer Projektion.</span><span class="sxs-lookup"><span data-stu-id="329c6-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="329c6-149">Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:</span><span class="sxs-lookup"><span data-stu-id="329c6-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="329c6-150">Dieses Format ist einfacher, Clients als im ursprünglichen Objektdiagramm nutzen, die geschachtelte Entitäten (Order, Details und Produkte) enthält.</span><span class="sxs-lookup"><span data-stu-id="329c6-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="329c6-151">Die letzte Methode, die beachtet werden `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="329c6-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="329c6-152">Derzeit, diese Methode akzeptiert eine `Order` Instanz.</span><span class="sxs-lookup"><span data-stu-id="329c6-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="329c6-153">Aber bedenken, was geschieht, wenn ein Client, Anforderungstext sollte wie folgt sendet:</span><span class="sxs-lookup"><span data-stu-id="329c6-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="329c6-154">Dies ist eine gut strukturierte Reihenfolge aus, und Entity Framework wird zum Glück fügen sie in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="329c6-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="329c6-155">Aber es enthält eine Product-Entität, die nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="329c6-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="329c6-156">Der Client erstellt einfach ein neues Produkt in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="329c6-156">The client just created a new product in our database!</span></span> <span data-ttu-id="329c6-157">Dies wird an die Reihenfolge Ausführung-Abteilung, überrascht sein, wenn sie einen Auftrag für Koala Bears finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="329c6-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="329c6-158">Die Moral, seien Sie wirklich die Daten, die Sie in einer POST- oder PUT-Anforderung annehmen.</span><span class="sxs-lookup"><span data-stu-id="329c6-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="329c6-159">Um dieses Problem zu vermeiden, ändern Sie die `PostOrder` Methode, um eine `OrderDTO` Instanz.</span><span class="sxs-lookup"><span data-stu-id="329c6-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="329c6-160">Verwenden der `OrderDTO` zum Erstellen der `Order`.</span><span class="sxs-lookup"><span data-stu-id="329c6-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="329c6-161">Beachten Sie, die wir verwenden die `ProductID` und `Quantity` Eigenschaften und ignorieren Sie alle Werte, die vom Client für Produktnamen oder Preis gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="329c6-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="329c6-162">Wenn die Produkt-ID ungültig ist, wird es verletzen, foreign Key-Einschränkung in der Datenbank und der Einfügevorgang fehl, wie er es sollte.</span><span class="sxs-lookup"><span data-stu-id="329c6-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="329c6-163">Hier ist die vollständige `PostOrder` Methode:</span><span class="sxs-lookup"><span data-stu-id="329c6-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="329c6-164">Fügen Sie abschließend die **autorisieren** Attribut mit dem Controller:</span><span class="sxs-lookup"><span data-stu-id="329c6-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="329c6-165">Jetzt können nur registrierte Benutzer erstellen oder Anzeigen von Bestellungen.</span><span class="sxs-lookup"><span data-stu-id="329c6-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="329c6-166">[Zurück](using-web-api-with-entity-framework-part-5.md)
> [Weiter](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="329c6-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
