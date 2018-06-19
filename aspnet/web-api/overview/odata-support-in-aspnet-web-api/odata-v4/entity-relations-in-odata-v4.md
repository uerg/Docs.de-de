---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Entitätsbeziehungen in OData v4 mithilfe von ASP.NET Web-API 2.2 | Microsoft Docs
author: MikeWasson
description: 'Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über navigieren...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508099"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Entitätsbeziehungen in OData v4 mithilfe von ASP.NET Web-API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über entitätsbeziehungen navigieren. Wenn Sie ein Produkt, können Sie den Lieferanten suchen. Sie können auch erstellen oder Beziehungen entfernt werden. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
> 
> Dieses Lernprogramm zeigt, wie zur Unterstützung dieser Vorgänge in OData v4 mithilfe der ASP.NET Web API. Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
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
> Die OData-Version 3, finden Sie unter [Entitätsbeziehungen in OData v3 unterstützen](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Hinzufügen einer Entität "Supplier"

> [!NOTE]
> Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).


Zunächst benötigen wir eine verknüpfte Entität. Fügen Sie eine Klasse mit dem Namen `Supplier` im Ordner "Modelle".

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Hinzufügen eine Navigationseigenschaft an die `Product` Klasse:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Fügen Sie einen neuen **DbSet** auf die `ProductsContext` Klasse, sodass Entity Framework die Lieferanten-Tabelle in der Datenbank berücksichtigt werden.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Fügen Sie in WebApiConfig.cs, eine &quot;Lieferanten&quot; Entitätenmenge mit dem Entity Data Model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Fügen Sie einen Controller Lieferanten

Hinzufügen einer `SuppliersController` Klasse, um den Ordner Controllers.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Gewusst wie: Hinzufügen von CRUD-Vorgänge für diesen Controller zeigen nicht an. Die Schritte entsprechen den Produkts-Controllers (siehe [erstellen Sie einen OData v4-Endpunkt](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Beim Abrufen der verknüpften Entitäten

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode, die die `ProductsController` Klasse:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Diese Methode verwendet eine standardmäßige Benennungskonvention

- Methodenname: GetX, wobei X die Navigationseigenschaft ist.
- Parametername: *Schlüssel*

Wenn Sie diese Benennungskonvention befolgen, werden die HTTP-Anforderung an die Controllermethode automatisch von Web-API zugeordnet.

Für HTTP-beispielanforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Für HTTP-Beispielantwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Abrufen einer verknüpften Sammlung

Im vorherigen Beispiel wurde ein Produkt eine Lieferanten. Eine Navigationseigenschaft kann auch eine Auflistung zurückgeben. Der folgende Code Ruft die Produkte für ein Lieferant ab:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

In diesem Fall die Methode gibt ein **IQueryable** anstelle von einer **SingleResult&lt;T&gt;**

Für HTTP-beispielanforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Für HTTP-Beispielantwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Erstellen eine Beziehung zwischen Entitäten

OData unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei vorhandene Entitäten. In OData v4-Terminologie, die Beziehung ist eine &quot;Verweis&quot;. (Im OData v3 hieß die Beziehung eine *Link*. Die Protokollunterschiede unerheblich nicht für dieses Lernprogramm.)

Ein Verweis verfügt über einen eigenen URI, mit dem Formular `/Entity/NavigationProperty/$ref`. Hier ist z. B. der URI, der den Verweis zwischen eines Produkts und seiner Lieferanten zu behandeln:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Um eine Beziehung hinzufügen möchten, sendet der Client eine POST oder PUT-Anforderung an diese Adresse.

- PUT ist die Navigationseigenschaft eine einzelne Entität, wie z. B. `Product.Supplier`.
- BEREITSTELLEN, wenn die Navigationseigenschaft eine Auflistung, z. B. `Supplier.Products`.

Der Anforderungstext enthält den URI der Entität in der Beziehung. Hier ist eine Beispiel für eine Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

In diesem Beispiel wird der Client sendet eine PUT-Anforderung zum `/Products(6)/Supplier/$ref`, dies ist der URI "$ref" für die `Supplier` des Produkts mit der ID = 6. Wenn die Anforderung erfolgreich ist, sendet der Server eine Antwort mit 204 (kein Inhalt):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Hier wird der Controllermethode, um eine Beziehung zum Hinzufügen einer `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Die *NavigationProperty* Parameter gibt an, welche Beziehung festzulegen. (Wenn mehr als einer Navigationseigenschaft auf die Entität vorhanden ist, können Sie weitere hinzufügen `case` Anweisungen.)

Die *Link* Parameter enthält den URI des Lieferanten. Web-API analysiert automatisch den Hauptteil der Anforderung, um den Wert für diesen Parameter abzurufen.

Um den Lieferanten zu suchen, die ID (oder Schlüssel) erforderlich ist Teil der *Link* Parameter. Verwenden Sie hierzu die folgende Hilfsmethode ein:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Im Grunde mithilfe dieser Methode die OData-Bibliothek auf den URI-Pfad in Segmente unterteilt und das Segment mit dem Schlüssel des Schlüssels in den richtigen Typ zu konvertieren.

## <a name="deleting-a-relationship-between-entities"></a>Löschen einer Beziehung zwischen Entitäten

Um eine Beziehung löschen, sendet der Client eine HTTP DELETE-Anforderung an den $ref-URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

So sieht die Controllermethode zum Löschen der Beziehung zwischen einem Produkt und einem anderen Lieferanten aus:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

In diesem Fall `Product.Supplier` ist die &quot;1&quot; Ende eine 1: n-Beziehung, daher können Sie die Beziehung nur durch Festlegen entfernen `Product.Supplier` auf `null`.

In der &quot;viele&quot; -Ende einer Beziehung, die der Client muss angeben, die miteinander in Beziehung Entität zu entfernen. Hierzu sendet der Client den URI der verknüpften Entität in der Abfragezeichenfolge der Anforderung. Beispielsweise so entfernen Sie "Product 1" von "Lieferant 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Um dies in Web-API unterstützen, müssen wir einen zusätzlichen Parameter in umfassen die `DeleteRef` Methode. Hier ist die Controllermethode zum Entfernen eines Produkts aus der `Supplier.Products` Beziehung.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Die *Schlüssel* Parameter ist der Schlüssel für den Lieferanten und die *RelatedKey* Parameter ist der Schlüssel für das Produkt, das Aufheben der `Products` Beziehung. Beachten Sie, dass Web-API ruft automatisch den Schlüssel aus der Abfragezeichenfolge ab.
