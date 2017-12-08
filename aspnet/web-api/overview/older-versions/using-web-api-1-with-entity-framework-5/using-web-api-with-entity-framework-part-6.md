---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt- und Reihenfolge Controller | Microsoft Docs'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="8b859-102">Teil 6: Erstellen von Produkt- und Reihenfolge Controller</span><span class="sxs-lookup"><span data-stu-id="8b859-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="8b859-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b859-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8b859-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="8b859-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="8b859-105">Hinzufügen einer Produkts-Controllers</span><span class="sxs-lookup"><span data-stu-id="8b859-105">Add a Products Controller</span></span>

<span data-ttu-id="8b859-106">Der Administrator-Controller ist für Benutzer mit Administratorrechten aus.</span><span class="sxs-lookup"><span data-stu-id="8b859-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="8b859-107">Kunden, andererseits, können Produkte anzeigen, jedoch kann nicht erstellen, aktualisieren oder löschen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="8b859-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="8b859-108">Wir können problemlos den Zugriff auf die Methoden Post, Put und Delete einschränken, ohne die Get-Methoden.</span><span class="sxs-lookup"><span data-stu-id="8b859-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="8b859-109">Aber sehen Sie sich die Daten, die für ein Produkt zurückgegeben werden:</span><span class="sxs-lookup"><span data-stu-id="8b859-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="8b859-110">Die `ActualCost` Eigenschaft darf nicht für Kunden sichtbar sein!</span><span class="sxs-lookup"><span data-stu-id="8b859-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="8b859-111">Die Lösung besteht darin, definieren Sie eine *Datenübertragungsobjekt* (DTO) enthält, die eine Teilmenge der Eigenschaften, die Kunden angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="8b859-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="8b859-112">Wir verwenden LINQ zum Projekt `Product` -Instanzen `ProductDTO` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="8b859-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="8b859-113">Fügen Sie eine Klasse mit dem Namen `ProductDTO` Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="8b859-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="8b859-114">Nun fügen Sie den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="8b859-114">Now add the controller.</span></span> <span data-ttu-id="8b859-115">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="8b859-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8b859-116">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="8b859-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="8b859-117">In der **Controller hinzufügen** Dialogfeld Namen des Controllers &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b859-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="8b859-118">Klicken Sie unter **Vorlage**Option **leere API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="8b859-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="8b859-119">Ersetzen Sie alles in der Quelldatei an, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8b859-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="8b859-120">Der Controller verwendet weiterhin den `OrdersContext` zum Abfragen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8b859-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="8b859-121">Aber anstatt `Product` Instanzen direkt, wir rufen `MapProducts` , diese auf projizieren `ProductDTO` Instanzen:</span><span class="sxs-lookup"><span data-stu-id="8b859-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="8b859-122">Die `MapProducts` Methode gibt ein **IQueryable**, sodass wir das Ergebnis mit anderen Abfrageparametern verfassen können.</span><span class="sxs-lookup"><span data-stu-id="8b859-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="8b859-123">Sehen Sie hierzu finden Sie unter der `GetProduct` -Methode, die Fügt eine **, in dem** -Klausel, um die Abfrage:</span><span class="sxs-lookup"><span data-stu-id="8b859-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="8b859-124">Fügen Sie einen Orders-Controller</span><span class="sxs-lookup"><span data-stu-id="8b859-124">Add an Orders Controller</span></span>

<span data-ttu-id="8b859-125">Als Nächstes fügen Sie einen Controller, mit dem Benutzer erstellen und Anzeigen von Aufträgen an.</span><span class="sxs-lookup"><span data-stu-id="8b859-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="8b859-126">Wir beginnen mit einem anderen DTO.</span><span class="sxs-lookup"><span data-stu-id="8b859-126">We'll start with another DTO.</span></span> <span data-ttu-id="8b859-127">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner Models, und fügen Sie eine Klasse mit dem Namen `OrderDTO` verwenden Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="8b859-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="8b859-128">Nun fügen Sie den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="8b859-128">Now add the controller.</span></span> <span data-ttu-id="8b859-129">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="8b859-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8b859-130">Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="8b859-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="8b859-131">In der **Controller hinzufügen** im Dialogfeld die folgenden Optionen festlegen:</span><span class="sxs-lookup"><span data-stu-id="8b859-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="8b859-132">Klicken Sie unter **Controllernamen**, geben Sie "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="8b859-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="8b859-133">Klicken Sie unter **Vorlage**Option "API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="8b859-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="8b859-134">Klicken Sie unter **Modellschemas**Option &quot;Reihenfolge (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b859-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="8b859-135">Klicken Sie unter **Datenkontextklasse**Option &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b859-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="8b859-136">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="8b859-136">Click **Add**.</span></span> <span data-ttu-id="8b859-137">Dadurch wird eine Datei namens OrdersController.cs hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8b859-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="8b859-138">Als Nächstes müssen wir die Standardimplementierung des Controllers ändern.</span><span class="sxs-lookup"><span data-stu-id="8b859-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="8b859-139">Löschen Sie zuerst die `PutOrder` und `DeleteOrder` Methoden.</span><span class="sxs-lookup"><span data-stu-id="8b859-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="8b859-140">Für dieses Beispiel nicht Kunden ändern oder löschen vorhandene Aufträge.</span><span class="sxs-lookup"><span data-stu-id="8b859-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="8b859-141">In einer realen Anwendung benötigen Sie viele der Back-End-Logik, um diese Fälle zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="8b859-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="8b859-142">Geliefert (z. B. wurde die Bestellung bereits?)</span><span class="sxs-lookup"><span data-stu-id="8b859-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="8b859-143">Ändern der `GetOrders` Methode, um nur die Aufträge zurückzugeben, die dem Benutzer gehören:</span><span class="sxs-lookup"><span data-stu-id="8b859-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="8b859-144">Ändern der `GetOrder` Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="8b859-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="8b859-145">Hier werden die Änderungen, die wir an die Methode vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="8b859-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="8b859-146">Der Rückgabewert ist eine `OrderDTO` Instanz anstelle von einer `Order`.</span><span class="sxs-lookup"><span data-stu-id="8b859-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="8b859-147">Wenn wir die Datenbank für die Bestellung abzufragen, verwenden wir die [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) Methode, um den zugehörigen fetch `OrderDetail` und `Product` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="8b859-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="8b859-148">Wir haben das Ergebnis mithilfe einer Projektion vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="8b859-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="8b859-149">Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:</span><span class="sxs-lookup"><span data-stu-id="8b859-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="8b859-150">Dieses Format ist einfacher, Clients als die ursprüngliche Objektdiagramm nutzen die verschachtelte Entitäten (Reihenfolge, Details und Produkte) enthält.</span><span class="sxs-lookup"><span data-stu-id="8b859-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="8b859-151">Die letzte Methode, die beachtet werden `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="8b859-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="8b859-152">Jetzt, diese Methode nimmt ein `Order` Instanz.</span><span class="sxs-lookup"><span data-stu-id="8b859-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="8b859-153">Aber in Betracht ziehen, was geschieht, wenn ein Client, einen Anforderungstext wie folgt sendet:</span><span class="sxs-lookup"><span data-stu-id="8b859-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="8b859-154">Dies ist eine gut strukturierte Reihenfolge und Entity Framework wird es problemlos in die Datenbank einfügen.</span><span class="sxs-lookup"><span data-stu-id="8b859-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="8b859-155">Aber es enthält eine Produktentität, die nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8b859-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="8b859-156">Der Client ein neues Produkt in unserer Datenbank erstellte!</span><span class="sxs-lookup"><span data-stu-id="8b859-156">The client just created a new product in our database!</span></span> <span data-ttu-id="8b859-157">Dies wird eine plötzliche an die Reihenfolge Fullfilment Abteilung sein, wenn sie eine Bestellung für Koala wegen finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="8b859-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="8b859-158">Die Moral ist, achten Sie tatsächlich zu Daten, die Sie in einer POST- oder PUT-Anforderung akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="8b859-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="8b859-159">Um dieses Problem zu vermeiden, ändern Sie die `PostOrder` Methode, um eine `OrderDTO` Instanz.</span><span class="sxs-lookup"><span data-stu-id="8b859-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="8b859-160">Verwenden der `OrderDTO` zum Erstellen der `Order`.</span><span class="sxs-lookup"><span data-stu-id="8b859-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="8b859-161">Beachten Sie, die wir verwenden die `ProductID` und `Quantity` Eigenschaften, und es ignoriert alle Werte, die der Client für Produktnamen oder Preis gesendet.</span><span class="sxs-lookup"><span data-stu-id="8b859-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="8b859-162">Wenn die Produkt-ID ungültig ist, es wird die foreign Key-Einschränkung in der Datenbank verletzen, und die Einfügung schlägt fehlt, wie er es sollte.</span><span class="sxs-lookup"><span data-stu-id="8b859-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="8b859-163">Hier wird die vollständige `PostOrder` Methode:</span><span class="sxs-lookup"><span data-stu-id="8b859-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="8b859-164">Fügen Sie schließlich die **autorisieren** -Attribut auf den Controller:</span><span class="sxs-lookup"><span data-stu-id="8b859-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="8b859-165">Jetzt können nur registrierte Benutzer erstellen oder Aufträge anzeigen.</span><span class="sxs-lookup"><span data-stu-id="8b859-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8b859-166">[Zurück](using-web-api-with-entity-framework-part-5.md)
[Weiter](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="8b859-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
