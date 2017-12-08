---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Teil 2: Erstellen der Domänenmodelle | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5d4c7d7d02ced5a99db5b59f9e2e1adf6588208a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="b79bb-102">Teil 2: Erstellen der Domänenmodelle</span><span class="sxs-lookup"><span data-stu-id="b79bb-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="b79bb-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b79bb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b79bb-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="b79bb-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="b79bb-105">Hinzufügen von Modellen</span><span class="sxs-lookup"><span data-stu-id="b79bb-105">Add Models</span></span>

<span data-ttu-id="b79bb-106">Es gibt drei Möglichkeiten, Ansatz Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="b79bb-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="b79bb-107">Database First: beginnen Sie mit einer Datenbank und Entity Framework generiert den Code.</span><span class="sxs-lookup"><span data-stu-id="b79bb-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="b79bb-108">Model First: Sie beginnen mit einem visual Modell und Entity Framework generiert, die Datenbank und den Code.</span><span class="sxs-lookup"><span data-stu-id="b79bb-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="b79bb-109">Code First: beginnen Sie mit Code und Entity Framework generiert die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b79bb-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="b79bb-110">Verwenden wir den Code First-Ansatz, damit wir beginnen mit unserer Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren.</span><span class="sxs-lookup"><span data-stu-id="b79bb-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="b79bb-111">Mit dem Code First-Ansatz benötigen Domänenobjekte nicht keinen zusätzlichen Code für die Datenbankebene, z. B. Transaktionen oder Persistenz unterstützen.</span><span class="sxs-lookup"><span data-stu-id="b79bb-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="b79bb-112">(Insbesondere benötigen keinen zu Vererben der [EntityObject](https://msdn.microsoft.com/en-us/library/system.data.objects.dataclasses.entityobject.aspx) Klasse.) Datenanmerkungen können weiterhin steuern, wie Entity Framework für das Datenbankschema erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="b79bb-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/en-us/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="b79bb-113">Da POCOs keine zusätzlichen Eigenschaften enthalten, die beschreiben, [Datenbankstatus](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx), sie können problemlos in JSON oder XML serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b79bb-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="b79bb-114">Allerdings bedeutet dies nicht immer die Entity Framework-Modelle direkt an Clients sollten verfügbar machen, wie wir später in diesem Lernprogramm sehen werden.</span><span class="sxs-lookup"><span data-stu-id="b79bb-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="b79bb-115">Wir werden die folgenden POCOs erstellen:</span><span class="sxs-lookup"><span data-stu-id="b79bb-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="b79bb-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="b79bb-116">Product</span></span>
- <span data-ttu-id="b79bb-117">Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="b79bb-117">Order</span></span>
- <span data-ttu-id="b79bb-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="b79bb-118">OrderDetail</span></span>

<span data-ttu-id="b79bb-119">Um jede Klasse erstellen, und mit der rechten Maustaste im Projektmappen-Explorer die Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="b79bb-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="b79bb-120">Wählen Sie im Kontextmenü der **hinzufügen** und wählen Sie dann **Klasse.**</span><span class="sxs-lookup"><span data-stu-id="b79bb-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="b79bb-121">Hinzufügen einer `Product` Klasse mit der folgenden Implementierung:</span><span class="sxs-lookup"><span data-stu-id="b79bb-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="b79bb-122">Gemäß der Konvention Entity Framework verwendet die `Id` Eigenschaft als Primärschlüssel und ordnet sie einer Identity-Spalte in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="b79bb-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="b79bb-123">Beim Erstellen einer neuen `Product` Instanz wird nicht für einen Wert festlegen `Id`, da die Datenbank den Wert generiert.</span><span class="sxs-lookup"><span data-stu-id="b79bb-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="b79bb-124">Die **ScaffoldColumn** -Attribut weist ASP.NET MVC zum Überspringen der `Id` Eigenschaft während der Generierung einer Formular-Editors.</span><span class="sxs-lookup"><span data-stu-id="b79bb-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="b79bb-125">Die **erforderlich** Attribut wird verwendet, um das Modell zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b79bb-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="b79bb-126">Gibt an, dass die `Name` Eigenschaft muss eine nicht leere Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="b79bb-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="b79bb-127">Hinzufügen der `Order` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b79bb-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="b79bb-128">Hinzufügen der `OrderDetail` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b79bb-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="b79bb-129">Foreign Key-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="b79bb-129">Foreign Key Relations</span></span>

<span data-ttu-id="b79bb-130">Eine Bestellung enthält viele Bestelldetails, und jede Order Detail bezieht sich auf einem einzigen Produkt.</span><span class="sxs-lookup"><span data-stu-id="b79bb-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="b79bb-131">Zur Darstellung dieser Beziehungen die `OrderDetail` Klasse definiert die Eigenschaften, die mit dem Namen `OrderId` und `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="b79bb-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="b79bb-132">Entity Framework wird abgeleitet werden, dass diese Eigenschaften Fremdschlüssel darstellen, und die Datenbank foreign Key-Einschränkungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="b79bb-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="b79bb-133">Die `Order` und `OrderDetail` Klassen umfassen auch ""-Navigationseigenschaften, die Verweise auf die verknüpften Objekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="b79bb-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="b79bb-134">Erhält eine Bestellung, können Sie die Produkte in der Reihenfolge navigieren, anhand der Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b79bb-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="b79bb-135">Kompilieren Sie das Projekt jetzt an.</span><span class="sxs-lookup"><span data-stu-id="b79bb-135">Compile the project now.</span></span> <span data-ttu-id="b79bb-136">Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle ermitteln, daher ist eine kompilierte Assembly So erstellen das Schema der Datenbank erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b79bb-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="b79bb-137">Konfigurieren Sie die Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="b79bb-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="b79bb-138">Ein [medientypformatierer](../../formats-and-model-binding/media-formatters.md) ist ein Objekt, das Ihre Daten serialisiert, wenn die Web-API über HTTP-Antworttext schreibt.</span><span class="sxs-lookup"><span data-stu-id="b79bb-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="b79bb-139">Die integrierten Formatierer unterstützt JSON und XML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="b79bb-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="b79bb-140">Standardmäßig serialisiert beider dieser Formatierer alle Objekte nach Wert an.</span><span class="sxs-lookup"><span data-stu-id="b79bb-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="b79bb-141">Serialisierung nach Wert erstellt ein Problem auf, wenn ein Objektdiagramm Zirkuläre Verweise enthält.</span><span class="sxs-lookup"><span data-stu-id="b79bb-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="b79bb-142">Dies ist genau der Fall mit der `Order` und `OrderDetail` Klassen, da jeweils einen Verweis auf die andere enthält.</span><span class="sxs-lookup"><span data-stu-id="b79bb-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="b79bb-143">Der Formatierer wird führen Sie die Verweise, Schreiben jedes Objekt als Wert, und wechseln Sie im Kreis.</span><span class="sxs-lookup"><span data-stu-id="b79bb-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="b79bb-144">Aus diesem Grund müssen wir das Standardverhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="b79bb-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="b79bb-145">Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b79bb-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="b79bb-146">Fügen Sie der `WebApiConfig` -Klasse folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="b79bb-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="b79bb-147">Dieser Code legt die JSON-Formatierungsprogramm Objektverweise beizubehalten und den XML-Formatierer vollständig aus der Pipeline entfernt.</span><span class="sxs-lookup"><span data-stu-id="b79bb-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="b79bb-148">(Sie können konfigurieren, dass des XML-Formatierers zum Beibehalten von Objektverweisen, aber es etwas aufwendiger ist und wir brauchen nur JSON für diese Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b79bb-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="b79bb-149">Weitere Informationen finden Sie unter [Behandlung von Objekt-Zirkelverweise](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="b79bb-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b79bb-150">[Zurück](using-web-api-with-entity-framework-part-1.md)
[Weiter](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="b79bb-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
