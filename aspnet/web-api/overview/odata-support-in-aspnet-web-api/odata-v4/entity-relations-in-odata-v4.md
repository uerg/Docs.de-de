---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Entitätsbeziehungen in OData v4 mithilfe von ASP.NET Web API 2.2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten. Mithilfe von OData, können Clients über navigieren...'
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 98f65b068d8f22e3eeef48ca7fa441434939db8b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827945"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Entitätsbeziehungen in OData v4 mit ASP.NET Web API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten. Mithilfe von OData, können Clients auf entitätsbeziehungen navigieren. Wenn ein Produkt, finden Sie den Lieferanten. Sie können auch erstellt oder Beziehungen entfernt werden. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
> 
> In diesem Tutorial veranschaulicht, wie diese Vorgänge in OData v4 mithilfe von ASP.NET Web-API unterstützen. Das Tutorial baut auf dem Tutorial [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API 2.1
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Die OData-Version 3, finden Sie unter [Unterstützung von Entitätsbeziehungen in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Fügen Sie eine Entität "Supplier" hinzu.

> [!NOTE]
> Das Tutorial baut auf dem Tutorial [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).


Zunächst benötigen wir eine verknüpfte Entität. Fügen Sie eine Klasse, die mit dem Namen `Supplier` im Ordner "Models".

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Fügen Sie eine Navigationseigenschaft für die `Product` Klasse:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Fügen Sie einen neuen **"DbSet"** auf die `ProductsContext` Klasse, sodass Entity Framework die Lieferanten-Tabelle in der Datenbank berücksichtigt werden.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Fügen Sie in WebApiConfig.cs, eine &quot;Lieferanten&quot; Entitätenmenge mit dem Entity Data Model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Hinzufügen eines Controllers ' Suppliers '

Hinzufügen einer `SuppliersController` Klasse, um den Ordner "Controllers".

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Ich nicht das Hinzufügen von CRUD-Vorgänge für diesen Controller angezeigt. Die Schritte sind identisch mit dem Produkts-Controllers (finden Sie unter [erstellen ein OData v4-Endpunkts](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Abrufen von verknüpften Entitäten

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode der `ProductsController` Klasse:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Diese Methode wird eine standardmäßige Namenskonvention verwendet.

- Methodenname: GetX, wobei X die Navigationseigenschaft ist.
- Parametername: *Schlüssel*

Wenn Sie diese Benennungskonvention befolgen, ordnet Web-API-Controller-Methode automatisch die HTTP-Anforderung.

Für HTTP-beispielanforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

HTTP-Beispielantwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Abrufen einer entsprechenden Sammlung

Im vorherigen Beispiel wurde ein Produkt ein Lieferant. Eine Navigationseigenschaft kann auch eine Sammlung zurück. Der folgende Code Ruft die Produkte für einen Lieferanten:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

In diesem Fall die Methode gibt ein **"IQueryable"** statt einer **"SingleResult"&lt;T&gt;**

Für HTTP-beispielanforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

HTTP-Beispielantwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Erstellen eine Beziehung zwischen Entitäten

OData unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei vorhandenen Entitäten. In der Terminologie von OData v4-die Beziehung ist eine &quot;Verweis&quot;. (In OData v3 hieß die Beziehung eine *Link*. Die Protokollunterschiede sind in diesem Tutorial zulässig.)

Ein Verweis verfügt über einen eigenen URI, mit dem Formular `/Entity/NavigationProperty/$ref`. Hier ist z. B. der URI, der den Verweis zwischen eines Produkts und dem zugehörigen Lieferanten zu behandeln:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Um eine Beziehung hinzuzufügen, sendet der Client eine POST- oder PUT-Anforderung an diese Adresse an.

- Einfügen, wenn die Navigationseigenschaft eine einzelne Entität, z. B. `Product.Supplier`.
- Posten, wenn die Navigationseigenschaft eine Auflistung, z. B. `Supplier.Products`.

Der Hauptteil der Anforderung enthält den URI der anderen Entität in der Beziehung. Hier ist eine beispielanforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

In diesem Beispiel ist der Client sendet eine PUT-Anforderung an `/Products(6)/Supplier/$ref`, dies ist der URI "$ref" für die `Supplier` des Produkts, mit der ID = 6. Wenn die Anforderung erfolgreich ist, sendet der Server eine Antwort mit 204 (No Content):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Hier ist die Controllermethode eine Beziehung zum Hinzufügen einer `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Die *NavigationProperty* Parameter gibt an, welche Beziehung festzulegen. (Wenn mehr als einer Navigationseigenschaft auf die Entität vorhanden ist, können Sie weitere hinzufügen `case` Anweisungen.)

Die *Link* Parameter enthält den URI des Lieferanten. Web-API analysiert automatisch den Hauptteil der Anforderung, um den Wert für diesen Parameter abzurufen.

Um Lieferanten anzuzeigen, benötigen wir die ID (oder Schlüssel), diese ist Teil der *Link* Parameter. Verwenden Sie hierzu die folgende Hilfsmethode ein:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Im Grunde verwendet diese Methode die OData-Bibliothek, den URI-Pfad in Segmente aufgeteilt, suchen Sie den Abschnitt mit dem Schlüssel und den Schlüssel in den richtigen Typ zu konvertieren.

## <a name="deleting-a-relationship-between-entities"></a>Löschen einer Beziehung zwischen Entitäten

Um eine Beziehung zu löschen, sendet der Client eine HTTP DELETE-Anforderung an den $ref-URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Hier ist der Controllermethode, um die Beziehung zwischen einem Produkt und einem Lieferanten zu löschen:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

In diesem Fall `Product.Supplier` ist die &quot;1&quot; Ende einer 1: n-Beziehung, ganz einfach durch Festlegen die Beziehung entfernen zu können `Product.Supplier` zu `null`.

In der &quot;viele&quot; -Ende einer Beziehung, die dem Client muss angeben, die zu entfernende Entität bezieht. Hierzu sendet der Client den URI der verknüpften Entität in der Abfragezeichenfolge der Anforderung. Beispielsweise so entfernen Sie "Product 1" von "Lieferant 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Um dies in Web-API zu unterstützen, müssen wir einen zusätzlichen Parameter in umfassen die `DeleteRef` Methode. Hier ist die Controllermethode zum Entfernen eines Produkts aus der `Supplier.Products` Beziehung.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Die *Schlüssel* Parameter ist der Schlüssel für den Lieferanten, und die *RelatedKey* Parameter ist der Schlüssel für das Produkt, das Entfernen aus der `Products` Beziehung. Beachten Sie, dass Web-API ruft automatisch den Schlüssel aus der Abfragezeichenfolge ab.
