---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Routingkonventionen in der ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: "Dieser Artikel beschreibt die routingkonventionen, die Web-API für OData-Endpunkte verwendet."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: cd24a85a05e427f83d28cae876431d04cc295f17
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Routingkonventionen in der ASP.NET Web API 2 Odata
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieser Artikel beschreibt die routingkonventionen, die Web-API für OData-Endpunkte verwendet.


Beim Web-API eine OData-Anforderung abgerufen werden, wird die Anforderung auf einen Controllernamen und einen Aktionsnamen zugeordnet. Die Zuordnung basiert auf der HTTP-Methode und der URI. Beispielsweise `GET /odata/Products(1)` ordnet `ProductsController.GetProduct`.

In Teil 1 dieses Artikels wird die integrierte OData-routingkonventionen beschrieben. Diese Konventionen dienen insbesondere für OData-Endpunkte, und Ersetzen Sie das routing Standardsystem für Web-API. (Der Ersatz erfolgt beim Aufruf von **MapODataRoute**.)

In Teil 2 zeigen ich, wie benutzerdefinierte routingkonventionen hinzufügen. Gegenwärtig die integrierten Konventionen umfassen den gesamten Bereich von OData URIs, aber Sie können erweitern, um zusätzliche Fälle zu behandeln.

- [Integrierte Routingkonventionen](#conventions)
- [Benutzerdefinierte Routingkonventionen](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Integrierte Routingkonventionen

Bevor ich die OData-routingkonventionen in Web-API zu beschreiben, ist es hilfreich zu verstehen, OData-URIs. Ein [OData-URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) besteht aus:

- Der dienststamm
- Pfad der Ressource
- Abfrageoptionen

![](odata-routing-conventions/_static/image1.png)

Für das routing ist der wichtigste Teil der Ressourcenpfad. Der Ressourcenpfad ist in Segmente unterteilt. Beispielsweise `/Products(1)/Supplier` verfügt über drei Segmente:

- `Products`bezieht sich auf eine Entitätenmenge, die mit dem Namen "Products".
- `1`Ein Entitätsschlüssel gezeigt, eine einzelne Entität auswählen, aus der Gruppe ist.
- `Supplier`ist eine Navigationseigenschaft, die eine verknüpfte Entität auswählt.

Dieser Pfad übernimmt daher den Hersteller der Product-1.

> [!NOTE]
> OData-Pfadsegmente müssen nicht immer URI-Segmenten entsprechen. "1" wird z. B. ein Pfadsegment betrachtet.


**Namen der Domänencontroller.** Den Namen des Controllers ist immer aus der Entitätssammlung am Stamm der Pfad der Ressource abgeleitet. Falls der Ressourcenpfad zeigt beispielsweise `/Products(1)/Supplier`, Web-API für einen Controller mit dem Namen sucht `ProductsController`.

**Aktionsnamen.** Aktionsnamen werden von der Pfadsegmente plus dem Entity Data Model (EDM) abgeleitet, wie in den folgenden Tabellen aufgeführt. In einigen Fällen müssen Sie zwei Optionen für den Aktionsnamen. Z. B. "Get" oder &quot;GetProducts&quot;.

**Abfragen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel-Aktion |
| --- | --- | --- | --- |
| /Entityset abrufen | / Products | GetEntitySet oder Get | GetProducts |
| /Entityset(key) abrufen | /Products(1) | GetEntityType oder Get | GetProduct |
| Abrufen von /entityset (Schlüssel) / umgewandelt | / /Models.Book Products (1) | GetEntityType oder Get | GetBook |

Weitere Informationen finden Sie unter [erstellen Sie einen schreibgeschützten OData-Endpunkt](odata-v3/creating-an-odata-endpoint.md).

**Erstellen, aktualisieren und Löschen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel-Aktion |
| --- | --- | --- | --- |
| POST /entityset | / Products | PostEntityType oder Post | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType oder PUT-Anforderung | PutProduct |
| /Entityset (Schlüssel) PUT / umgewandelt | / /Models.Book Products (1) | PutEntityType oder PUT-Anforderung | PutBook |
| Patch für /entityset(key) | /Products(1) | PatchEntityType oder Patch | PatchProduct |
| Patch für /entityset (Schlüssel) / umgewandelt | / /Models.Book Products (1) | PatchEntityType oder Patch | PatchBook |
| /Entityset(key) löschen | /Products(1) | DeleteEntityType oder Delete | DeleteProduct |
| Löschen von /entityset (Schlüssel) / umgewandelt | / /Models.Book Products (1) | DeleteEntityType oder Delete | DeleteBook |

**Eine Navigationseigenschaft Abfragen**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel-Aktion |
| --- | --- | --- | --- |
| GET-/entityset (Schlüssel) / Navigation | / Products (1) / Lieferanten | GetNavigationFromEntityType oder GetNavigation | GetSupplierFromProduct |
| Abrufen Sie/Umwandlung und zur Navigation /entityset (Schlüssel) | / /Models.Book/Author Products (1) | GetNavigationFromEntityType oder GetNavigation | GetAuthorFromBook |

Weitere Informationen finden Sie unter [arbeiten mit Entitätsbeziehungen](odata-v3/working-with-entity-relations.md).

**Erstellen und Löschen von Verknüpfungen**

| Anforderung | Beispiel-URI | Aktionsname |
| --- | --- | --- |
| POST /entityset (Schlüssel) / $links/Navigation | / Products (1) / $Links/Lieferanten | CreateLink |
| PUT /entityset (Schlüssel) / $links/Navigation | / Products (1) / $Links/Lieferanten | CreateLink |
| DELETE /entityset (Schlüssel) / $links/Navigation | / Products (1) / $Links/Lieferanten | DeleteLink |
| /Entityset(key)/$links/navigation(relatedKey) löschen | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Weitere Informationen finden Sie unter [arbeiten mit Entitätsbeziehungen](odata-v3/working-with-entity-relations.md).

**Eigenschaften**

*Erfordert die Web-API 2*

| Anforderung | Beispiel-URI | Aktionsname | Beispiel-Aktion |
| --- | --- | --- | --- |
| GET-/entityset (Schlüssel) / Eigenschaft | / Products (1) / Name | GetPropertyFromEntityType oder GetProperty | GetNameFromProduct |
| /Entityset (Schlüssel) / Cast/Eigenschaft abrufen | / /Models.Book/Author Products (1) | GetPropertyFromEntityType oder GetProperty | GetTitleFromBook |

**Aktionen**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel-Aktion |
| --- | --- | --- | --- |
| POST /entityset (Schlüssel) / Vorgang | / Products (1) / Rate | ActionNameOnEntityType oder ActionName | RateOnProduct |
| /Entityset (Schlüssel) / Cast/Aktion nach der | / /Models.Book/CheckOut Products (1) | ActionNameOnEntityType oder ActionName | CheckOutOnBook |

Weitere Informationen finden Sie unter [OData-Aktionen](odata-v3/odata-actions.md).

**Methodensignaturen**

Hier sind einige Regeln für die Methodensignaturen:

- Wenn der Pfad einen Schlüssel enthält, müssen die Aktion einen Parameter namens *Schlüssel*.
- Wenn der Pfad einen Schlüssel in einer Navigationseigenschaft enthält, müssen die Aktion einen Parameter namens *RelatedKey*.
- Ergänzen *Schlüssel* und *RelatedKey* Parameter mit dem **[FromODataUri]** Parameter.
- POST- und PUT-Anforderungen akzeptieren einen Parameter des Entitätstyps.
- PATCH-Anforderungen nehmen einen Versionsparameter vom Typ **Delta&lt;T&gt;**, wobei *T* ist der Entitätstyp.

Zu Referenzzwecken ist hier ein Beispiel für jede integrierte OData-routing-Konvention Methodensignaturen.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Benutzerdefinierte Routingkonventionen

Die integrierten Konventionen behandelt derzeit nicht alle möglichen OData-URIs. Sie können neue Konventionen hinzufügen, durch die Implementierung der **IODataRoutingConvention** Schnittstelle. Diese Schnittstelle verfügt über zwei Methoden:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** gibt den Namen des Controllers zurück.
- **SelectAction** gibt den Namen der Aktion zurück.

Für beide Methoden Wenn die Konvention nicht für diese Anforderung gelten sollte die Methode null zurück.

Die **OData-Pfad** Parameter darstellt, die analysierten OData Ressourcenpfad. Er enthält eine Liste von  **[OData-Pfadsegment dar](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatapathsegment.aspx)**  -Instanzen und eine für jedes Segment der Pfad der Ressource. **OData-Pfadsegment dar** ist eine abstrakte Klasse; jedes Segment-Typ wird durch eine Klasse, die abgeleitet dargestellt **OData-Pfadsegment dar**.

Die **ODataPath.TemplatePath** -Eigenschaft ist eine Zeichenfolge, die die Verkettung darstellt aller Pfadsegmente. Wenn der URI ist z. B. `/Products(1)/Supplier`, ist die pfadvorlage &quot;~/entityset/key/navigation&quot;. Beachten Sie, dass die Segmente direkt URI-Segmenten entsprechen nicht. Der Entitätsschlüssel (1) wird beispielsweise dargestellt, als eigene **OData-Pfadsegment dar**.

In der Regel wird eine Implementierung der **IODataRoutingConvention** bewirkt Folgendes:

1. Vergleichen Sie die pfadvorlage, um festzustellen, ob die aktuelle Anforderung diese Konvention gilt. Wenn sie nicht zutreffen, wird zurückgegeben Sie null.
2. Wenn die Konvention gilt, verwenden Sie die Eigenschaften der der **OData-Pfadsegment dar** Instanzen Controller-und Aktionsnamen abgeleitet werden.
3. Fügen Sie für Aktionen alle Werte zum Wörterbuch Route, das an der Action-Parameter (in der Regel Entitätsschlüssel) gebunden werden soll.

Sehen wir uns auf ein bestimmtes Beispiel. Die integrierten routingkonventionen unterstützt indizieren einer Auflistung Navigation nicht. Es gibt also keine Konvention für URIs wie folgt:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Hier ist eine benutzerdefinierte routing-Konvention auf diese Art der Abfrage.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notizen:

1. Ich abgeleitet **EntitySetRoutingConvention**, da die **SelectController** Methode in dieser Klasse eignet sich für dieses neue routing-Konvention. Das bedeutet, dass ich möchte nicht erneut implementieren **SelectController**.
2. Die Konvention gilt nur für die GET-Anforderungen, und nur, wenn die pfadvorlage ist &quot;~/entityset/key/navigation/key&quot;.
3. Der Aktionsname ist &quot;abrufen {EntityType}&quot;, wobei *{EntityType}* ist der Typ der Navigation-Auflistung. Beispielsweise &quot;GetSupplier&quot;. Sie können eine Benennungskonvention, die Ihnen gefällt &#8212; Achten Sie einfach Ihre Controlleraktionen übereinstimmen.
4. Die Aktion akzeptiert zwei Parameter, die mit dem Namen *Schlüssel* und *RelatedKey*. (Eine Liste der einige vordefinierte Parameternamen, finden Sie unter [ODataRouteConstants](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Der nächste Schritt besteht darin, die Liste der routingkonventionen neue Konvention hinzuzufügen. Dies geschieht bei der Konfiguration, wie im folgenden Code gezeigt:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Hier sind einige andere Beispiel routingkonventionen, die Sie untersuchen nützlich sein:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Web-API selbst ist natürlich Open-Source, damit Sie sehen die [Quellcode](http://aspnetwebstack.codeplex.com/) für die integrierte routingkonventionen. Diese sind definiert, der **System.Web.Http.OData.Routing.Conventions** Namespace.
