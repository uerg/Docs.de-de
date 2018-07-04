---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Routingkonventionen in der ASP.NET Web-API 2-Odata | Microsoft-Dokumentation
author: MikeWasson
description: Dieser Artikel beschreibt die routingkonventionen, die Web-API für OData-Endpunkte verwendet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63a0e4f4f61580ea9da3bd491e7a45f20cd7aaae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370538"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Routingkonventionen in der ASP.NET Web-API 2-Odata
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieser Artikel beschreibt die routingkonventionen, die Web-API für OData-Endpunkte verwendet.


Wenn Web-API eine OData-Anforderung erhält, ordnet es die Anforderung zum Controllernamen und einen Aktionsnamen. Die Zuordnung basiert auf der HTTP-Methode und den URI. Z. B. `GET /odata/Products(1)` ordnet `ProductsController.GetProduct`.

In Teil 1 dieses Artikels beschreibe ich die integrierte OData-standardroutingkonventionen. Diese Konventionen dienen insbesondere für OData-Endpunkte, und sie das Standard-Web-API-Routingsystem ersetzen. (Der Ersatz erfolgt beim Aufrufen **MapODataRoute**.)

In Teil 2 zeige ich, wie benutzerdefinierte routingkonventionen hinzufügen. Die integrierten Konventionen decken derzeit nicht der gesamte Bereich der OData-URIs, aber Sie können erweitern, um zusätzliche Fälle zu behandeln.

- [Integrierte Routingkonventionen](#conventions)
- [Benutzerdefinierte Routingkonventionen](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Integrierte Routingkonventionen

Bevor ich die OData-routingkonventionen in Web-API zu beschreiben, ist es hilfreich zu verstehen, OData-URIs. Ein [OData-URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) besteht aus:

- Der dienststamm
- Pfad der Ressource
- Abfrageoptionen

![](odata-routing-conventions/_static/image1.png)

Für das routing ist der wichtigste Teil der Ressourcenpfad. Pfad der Ressource ist in Segmente unterteilt. Z. B. `/Products(1)/Supplier` verfügt über drei Segmente:

- `Products` bezieht sich auf eine Entitätenmenge, die mit dem Namen "Products".
- `1` ist ein Entitätsschlüssel, Auswählen einer einzelnen Entität aus dem Satz.
- `Supplier` ist eine Navigationseigenschaft, die eine verknüpfte Entität auswählt.

Dieser Pfad übernimmt also den Lieferanten des Produkts 1.

> [!NOTE]
> OData-Pfadsegmente entsprechen nicht immer URI-Segmente. "1" wird z. B. ein Pfadsegment betrachtet.


**Namen der Domänencontroller.** Der Namen des Controllers wird immer aus der Entitätssammlung am Stamm der Pfad der Ressource abgeleitet. Wenn der Pfad der Ressource ist z. B. `/Products(1)/Supplier`, Web-API sieht für einen Controller namens `ProductsController`.

**Aktionsnamen.** Aktionsnamen werden von der Pfadsegmente sowie das Entity Data Model (EDM) abgeleitet, wie in den folgenden Tabellen aufgeführt. In einigen Fällen müssen Sie zwei Möglichkeiten für den Aktionsnamen. Z. B. "Get" oder &quot;GetProducts&quot;.

**Abfragen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispielaktion |
| --- | --- | --- | --- |
| /Entityset abrufen | / Produkte | GetEntitySet "oder" Get | GetProducts |
| /Entityset(key) abrufen | /Products(1) | GetEntityType "oder" Get | GetProduct |
| Abrufen von /entityset (Schlüssel), / umwandeln | /Products(1)/Models.Book | GetEntityType "oder" Get | GetBook |

Weitere Informationen finden Sie unter [erstellen Sie einen schreibgeschützten OData-Endpunkt](odata-v3/creating-an-odata-endpoint.md).

**Erstellen, aktualisieren und Löschen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispielaktion |
| --- | --- | --- | --- |
| /Entityset Posten | / Produkte | PostEntityType "oder" Post | PostProduct |
| Fügen Sie /entityset(key) | /Products(1) | PutEntityType oder PUT-Anforderung | PutProduct |
| Fügen Sie /entityset (Schlüssel) / umwandeln | /Products(1)/Models.Book | PutEntityType oder PUT-Anforderung | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType oder Patch | PatchProduct |
| PATCH /entityset (Schlüssel) / umwandeln | /Products(1)/Models.Book | PatchEntityType oder Patch | PatchBook |
| /Entityset(key) löschen | /Products(1) | DeleteEntityType oder Delete | DeleteProduct |
| Löschen von /entityset (Schlüssel), / umwandeln | /Products(1)/Models.Book | DeleteEntityType oder Delete | DeleteBook |

**Abfragen von einer Navigationseigenschaft**

| Anforderung | Beispiel-URI | Aktionsname | Beispielaktion |
| --- | --- | --- | --- |
| GET-/entityset (Schlüssel) / Navigation | / Produkte (1) / Lieferanten | GetNavigationFromEntityType oder GetNavigation | GetSupplierFromProduct |
| Abrufen Sie/Umwandlung und zur Navigation /entityset (Schlüssel) | /Products(1)/Models.Book/Author | GetNavigationFromEntityType oder GetNavigation | GetAuthorFromBook |

Weitere Informationen finden Sie unter [arbeiten mit Entitätsbeziehungen](odata-v3/working-with-entity-relations.md).

**Erstellen und Löschen von Links**

| Anforderung | Beispiel-URI | Aktionsname |
| --- | --- | --- |
| POST /entityset (Schlüssel) / $links/Navigation | / Produkte (1) / $Links/Lieferanten | CreateLink |
| PUT /entityset (Schlüssel) / $links/Navigation | / Produkte (1) / $Links/Lieferanten | CreateLink |
| DELETE /entityset (Schlüssel) / $links/Navigation | / Produkte (1) / $Links/Lieferanten | DeleteLink |
| /Entityset(key)/$links/navigation(relatedKey) löschen | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Weitere Informationen finden Sie unter [arbeiten mit Entitätsbeziehungen](odata-v3/working-with-entity-relations.md).

**Eigenschaften**

*Erfordert die Web-API 2*

| Anforderung | Beispiel-URI | Aktionsname | Beispielaktion |
| --- | --- | --- | --- |
| GET-/entityset (Schlüssel) / Eigenschaft | / Produkte (1) / Name | GetPropertyFromEntityType oder GetProperty | GetNameFromProduct |
| /Entityset (Schlüssel) / Cast/Eigenschaft abrufen | /Products(1)/Models.Book/Author | GetPropertyFromEntityType oder GetProperty | GetTitleFromBook |

**Aktionen**

| Anforderung | Beispiel-URI | Aktionsname | Beispielaktion |
| --- | --- | --- | --- |
| POST /entityset (Schlüssel) / Aktion | / Produkte (1) / Rate | ActionNameOnEntityType oder ActionName | RateOnProduct |
| /Entityset (Schlüssel) / Cast/Aktion nach der | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType oder ActionName | CheckOutOnBook |

Weitere Informationen finden Sie unter [OData-Aktionen](odata-v3/odata-actions.md).

**Methodensignaturen**

Hier sind einige Regeln für die Methodensignaturen:

- Wenn der Pfad einen Schlüssel enthält, müssen die Aktion einen Parameter namens *Schlüssel*.
- Wenn der Pfad einen Schlüssel in einer Navigationseigenschaft enthält, müssen die Aktion einen Parameter namens *RelatedKey*.
- Ergänzen Sie *Schlüssel* und *RelatedKey* Parameter mit dem **[FromODataUri]** Parameter.
- Akzeptieren einen Parameter des Entitätstyps, POST- und PUT-Anforderungen.
- PATCH-Anforderungen akzeptieren einen Parameter vom Typ **Delta&lt;T&gt;**, wobei *T* ist der Entitätstyp.

Zu Referenzzwecken ist hier ein Beispiel, Methodensignaturen für jede integrierte OData-routing-Konvention veranschaulicht.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Benutzerdefinierte Routingkonventionen

Die integrierten Konventionen behandelt derzeit nicht alle möglichen OData-URIs. Sie können neue Konventionen hinzufügen, durch die Implementierung der **IODataRoutingConvention** Schnittstelle. Diese Schnittstelle verfügt über zwei Methoden:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** gibt den Namen des Controllers zurück.
- **SelectAction** gibt den Namen der Aktion zurück.

Bei beiden Methoden ist, wenn die Konvention nicht für die Anforderung gilt, sollte die Methode null zurückgeben.

Die **OData-Pfad** Parameter stellt die analysierten OData-Ressourcenpfad dar. Es enthält eine Liste der **[OData-Pfadsegment dar](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** -Instanzen und eine für jedes Segment der Pfad der Ressource. **OData-Pfadsegment dar** ist eine abstrakte Klasse; jedes Segment-Typ wird durch eine abgeleitete Klasse dargestellt **OData-Pfadsegment dar**.

Die **ODataPath.TemplatePath** Eigenschaft ist eine Zeichenfolge, die die Verkettung darstellt. alle der Pfadsegmente. Wenn der URI ist z. B. `/Products(1)/Supplier`, ist die pfadvorlage &quot;~/entityset/key/navigation&quot;. Beachten Sie, dass die Segmente direkt an den URI-Segmente entsprechen nicht. Beispielsweise wird der Entitätsschlüssel (1) dargestellt, ein eigenes **OData-Pfadsegment dar**.

In der Regel eine Implementierung von **IODataRoutingConvention** bewirkt Folgendes:

1. Vergleichen Sie die pfadvorlage, um festzustellen, ob diese Konvention für die aktuelle Anforderung gilt. Wenn sie nicht anwendbar ist, gibt null zurück.
2. Wenn die Konvention gilt, verwenden Sie die Eigenschaften der der **OData-Pfadsegment dar** -Instanzen, die Controller-und Aktionsnamen abgeleitet werden.
3. Fügen Sie für Aktionen beliebige Werte hinzu, um die Route-Wörterbuch, das an die Aktionsparameter (in der Regel Entitätsschlüssel) gebunden werden soll.

Betrachten wir ein konkretes Beispiel. Die integrierten routingkonventionen unterstützt Indizierung in einer Auflistung für die Navigation nicht. Es gibt also keine Konvention für URIs wie folgt:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Hier ist eine benutzerdefinierte routing-Konvention, um diese Art von Abfrage zu verarbeiten.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notizen:

1. Ich leite von **EntitySetRoutingConvention**, da die **SelectController** -Methode in der Klasse ist für diese neue routingkonvention geeignet. Das heißt, ich muss nicht erneut implementieren **SelectController**.
2. Die Konvention bezieht sich nur auf GET-Anforderungen, und nur, wenn die pfadvorlage &quot;~/entityset/key/navigation/key&quot;.
3. Der Aktionsname ist &quot;erhalten {EntityType}&quot;, wobei *{EntityType}* ist der Typ der Navigation-Auflistung. Z. B. &quot;GetSupplier&quot;. Können Sie jede Benennungskonvention, die Ihnen gefallen &#8212; Achten Sie darauf Ihre Controlleraktionen übereinstimmen.
4. Die Aktion akzeptiert zwei Parameter, die mit dem Namen *Schlüssel* und *RelatedKey*. (Eine Liste der einige vordefinierte Parameternamen, finden Sie unter [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Der nächste Schritt ist die neue Konvention der Liste der routingkonventionen hinzufügen. Dies geschieht bei der Konfiguration, wie im folgenden Code gezeigt:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Hier sind einige weitere Beispiel-routingkonventionen, die nützlich sein, um zu untersuchen:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Und natürlich ist die Web-API selbst Open-Source, damit Sie sehen die [Quellcode](http://aspnetwebstack.codeplex.com/) für die integrierte routingkonventionen. Diese werden definiert, der **System.Web.Http.OData.Routing.Conventions** Namespace.
