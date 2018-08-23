---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Teil 2: Erstellen der Domänenmodelle | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836473"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="ed9dc-102">Teil 2: Erstellen der Domänenmodelle</span><span class="sxs-lookup"><span data-stu-id="ed9dc-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="ed9dc-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ed9dc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ed9dc-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="ed9dc-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="ed9dc-105">Hinzufügen von Modellen</span><span class="sxs-lookup"><span data-stu-id="ed9dc-105">Add Models</span></span>

<span data-ttu-id="ed9dc-106">Es gibt drei Möglichkeiten, um den Ansatz von Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="ed9dc-107">Database First: beginnen Sie mit einer Datenbank und Entity Framework generiert den Code.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="ed9dc-108">Model First: beginnen Sie mit einem visual Modell und Entity Framework generiert die Datenbank und den Code.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="ed9dc-109">Code First: beginnen Sie mit Code und Entity Framework generiert die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="ed9dc-110">Wir verwenden die Code First-Ansatz, damit unsere Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren wir zunächst.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="ed9dc-111">Mit dem Code-First-Ansatz erforderlich nicht Domänenobjekte zusätzlichen Code zur Unterstützung der Datenbankebene, z. B. Transaktionen oder Persistenz.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="ed9dc-112">(Insbesondere, sie müssen nicht das erben die ["EntityObject"](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) Klasse.) Sie können weiterhin datenanmerkungen verwenden, um zu steuern, wie Entity Framework für das Schema der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="ed9dc-113">Da POCOs keine zusätzlichen Eigenschaften enthalten, die beschreiben, [Datenbankstatus](https://msdn.microsoft.com/library/system.data.entitystate.aspx), sie können ganz einfach JSON oder XML serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="ed9dc-114">Allerdings bedeutet dies nicht immer die Entity Framework-Modelle direkt an die Clients sollten verfügbar machen, wie wir später in diesem Lernprogramm sehen werden.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="ed9dc-115">Wir erstellen die folgenden POCOs:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="ed9dc-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="ed9dc-116">Product</span></span>
- <span data-ttu-id="ed9dc-117">Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="ed9dc-117">Order</span></span>
- <span data-ttu-id="ed9dc-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ed9dc-118">OrderDetail</span></span>

<span data-ttu-id="ed9dc-119">Um jede Klasse zu erstellen, und mit der rechten Maustaste im Projektmappen-Explorer den Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="ed9dc-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="ed9dc-120">Wählen Sie im Kontextmenü des **hinzufügen** und wählen Sie dann **Klasse.**</span><span class="sxs-lookup"><span data-stu-id="ed9dc-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="ed9dc-121">Hinzufügen einer `Product` Klasse durch die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="ed9dc-122">Gemäß der Konvention verwendet Entity Framework die `Id` Eigenschaft als Primärschlüssel und ordnet es zu einer Identity-Spalte in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="ed9dc-123">Beim Erstellen einer neuen `Product` -Instanz wird nicht legen Sie einen Wert für `Id`, da die Datenbank den Wert generiert.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="ed9dc-124">Die **ScaffoldColumn** Attribut teilt ASP.NET MVC zum Überspringen der `Id` Eigenschaft, wenn ein Formular-Editors zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="ed9dc-125">Die **erforderlich** Attribut wird verwendet, um das Modell überprüft.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="ed9dc-126">Es gibt an, dass die `Name` -Eigenschaft muss eine nicht leere Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="ed9dc-127">Hinzufügen der `Order` Klasse:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="ed9dc-128">Hinzufügen der `OrderDetail` Klasse:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="ed9dc-129">Foreign Key-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="ed9dc-129">Foreign Key Relations</span></span>

<span data-ttu-id="ed9dc-130">Ein Auftrag enthält viele Auftragsdetails, und jede auftragsdetailbereich bezieht sich auf einem einzigen Produkt.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="ed9dc-131">Zur Darstellung dieser Beziehungen, die `OrderDetail` -Klasse definiert Eigenschaften, die mit dem Namen `OrderId` und `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="ed9dc-132">Entitätsframework folgert, dass diese Eigenschaften Fremdschlüssel darstellen, und der Datenbank foreign Key-Einschränkungen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="ed9dc-133">Die `Order` und `OrderDetail` Klassen umfassen auch "Navigation"-Eigenschaften, die Verweise auf die verknüpften Objekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="ed9dc-134">Wenn einen Auftrag, können Sie auf die Produkte in der Reihenfolge navigieren, anhand der Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="ed9dc-135">Kompilieren Sie das Projekt jetzt.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-135">Compile the project now.</span></span> <span data-ttu-id="ed9dc-136">Entitätsframework verwendet Reflektion, um die Eigenschaften der Modelle, zu ermitteln, deshalb sie eine kompilierte Assembly ein, um das Datenbankschema zu erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="ed9dc-137">Der Medientypformatierer konfigurieren</span><span class="sxs-lookup"><span data-stu-id="ed9dc-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="ed9dc-138">Ein [medientypformatierer](../../formats-and-model-binding/media-formatters.md) ein Objekt, das Ihre Daten serialisiert werden, wenn die Web-API-HTTP-Antworttext geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="ed9dc-139">Die integrierten Formatierungsprogrammen unterstützen JSON- und XML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="ed9dc-140">Standardmäßig serialisiert werden beide dieser Formatierer alle Objekte nach Wert.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="ed9dc-141">Serialisierung nach Wert führt zu einem Problem, wenn ein Objektdiagramm Zirkuläre Verweise enthält.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="ed9dc-142">Das ist genau der Fall mit der `Order` und `OrderDetail` Klassen, da Sie jeweils einen Verweis auf die andere enthält.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="ed9dc-143">Der Formatierer wird führen Sie die Verweise, Schreiben jedes Objekt als Wert und wechseln Sie im Kreis.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="ed9dc-144">Aus diesem Grund müssen wir das Standardverhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="ed9dc-145">Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="ed9dc-146">Fügen Sie der `WebApiConfig` -Klasse folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="ed9dc-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="ed9dc-147">Dieser Code legt das JSON-Formatierungsprogramm Objektverweise beizubehalten, und den XML-Formatierer vollständig aus der Pipeline entfernt.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="ed9dc-148">(Sie können konfigurieren, dass des XML-Formatierers zum Beibehalten von Objektverweisen, aber es ist etwas mehr Arbeit, und wir brauchen nur JSON für diese Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ed9dc-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="ed9dc-149">Weitere Informationen finden Sie unter [zirkuläre Objektverweise behandeln](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="ed9dc-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed9dc-150">[Zurück](using-web-api-with-entity-framework-part-1.md)
> [Weiter](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ed9dc-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
