---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Unterstützen von Entitätsbeziehungen in OData v3 mit Web-API 2 | Microsoft Docs"
author: MikeWasson
description: "Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über navigieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Unterstützen von Entitätsbeziehungen in OData v3 mit Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über entitätsbeziehungen navigieren. Wenn Sie ein Produkt, können Sie den Lieferanten suchen. Sie können auch erstellen oder Beziehungen entfernt werden. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
> 
> In diesem Lernprogramm wird gezeigt, wie diese Vorgänge in ASP.NET Web-API unterstützt. Das Lernprogramm baut auf das Lernprogramm [erstellen einen OData-v3-Endpunkt mit Web-API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 2
> - OData-Version 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Hinzufügen einer Entität "Supplier"

Zunächst müssen wir unsere OData-feed ein neuen Entitätstyps hinzugefügt. Fügen wir eine `Supplier` Klasse.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Diese Klasse verwendet eine Zeichenfolge für den Entitätsschlüssel. In der Praxis, die möglicherweise weniger gebräuchlich als mit einem Integer-Schlüssel. Aber es ist Folgendes zu sehen, wie OData andere Schlüsseltypen als ganze Zahlen behandelt.

Als Nächstes erstellen wir eine Beziehung durch Hinzufügen einer `Supplier` Eigenschaft, um die `Product` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Fügen Sie einen neuen **DbSet** auf die `ProductServiceContext` Klasse, sodass Entity Framework umfasst die `Supplier` Tabelle in der Datenbank.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Fügen Sie in WebApiConfig.cs eine Entität "Suppliers" zum EDM-Modell hinzu:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Navigationseigenschaften

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

"Supplier" wird hier eine Navigationseigenschaft, auf die `Product` Typ. In diesem Fall `Supplier` bezieht sich auf ein einzelnes Element, aber eine Navigation kann Eigenschaft auch eine Auflistung (1: n- oder m: n-Beziehung) zurückgeben.

Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode, die die `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Die *Schlüssel* Parameter ist der Schlüssel des Produkts. Die Methode gibt die verknüpfte Entität &#8212;in diesem Fall eine `Supplier` Instanz. Der Methodenname und Parameternamen sind wichtig. Wenn die Navigationseigenschaft "X" benannt wird, müssen Sie im Allgemeinen eine Methode mit dem Namen "GetX" hinzufügen. Die Methode muss einen Parameter namens annehmen "*Schlüssel*", die den Datentyp der Schlüssel des übergeordneten Elements entspricht.

Es ist auch wichtig, enthalten die **[FromOdataUri]** Attribut in der *Schlüssel* Parameter. Dieses Attribut weist Web API OData-Syntaxregeln verwenden, wenn es sich um den Schlüssel vom Anforderungs-URI analysiert werden.

## <a name="creating-and-deleting-links"></a>Erstellen und Löschen von Verknüpfungen

OData unterstützt beim Erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten. In der Terminologie von OData-ist die Beziehung "Link". Jeder Link verfügt über einen URI mit dem Formular *Entität*/$links /*Entität*. Beispielsweise sieht die Verknüpfung Produkt mit Lieferanten wie folgt:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Um einen neuen Link erstellen, sendet der Client eine POST-Anforderung an die Link-URI an. Der Text der Anforderung ist der URI der Zielentität. Nehmen Sie z. B. an, dass ein Lieferant mit dem Schlüssel "CTSO" vorhanden ist. Um eine Verknüpfung zwischen "Product(1)" und "Supplier('CTSO')" zu erstellen, sendet der Client eine Anforderung wie folgt:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Um einen Link zu löschen, sendet der Client eine DELETE-Anforderung an die Link-URI an.

**Zum Erstellen von Verknüpfungen**

Um einen Client zum Erstellen von Produkt-Lieferanten Links zu aktivieren, fügen Sie den folgenden Code, der `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Diese Methode nimmt drei Parameter:

- *Schlüssel*: der Schlüssel für die übergeordnete Entität (Product)
- *NavigationProperty*: der Name der Navigationseigenschaft. In diesem Beispiel ist die einzige gültige Navigationseigenschaft "Supplier".
- *Link*: der OData-URI der verknüpften Entität. Dieser Wert stammt aus dem Anforderungstext. Beispielsweise den Link URI möglicherweise "`http://localhost/odata/Suppliers('CTSO')`, d. h. den Lieferanten mit der ID ="CTSO".

Die Methode verwendet der Link, um den Lieferanten zu suchen. Wenn übereinstimmende Lieferanten gefunden wird, legt die Methode die `Product.Supplier` Eigenschaft und speichert das Ergebnis in der Datenbank.

Das schwierigste Teil ist der Link-URI analysieren. Im Wesentlichen müssen Sie das Ergebnis eine GET-Anforderung an diesen URI senden zu simulieren. Die folgende Hilfsmethode veranschaulicht dies. Die Methode ruft die Web-API-routing-Prozess und ruft wieder ein **OData-Pfad** Instanz, die die analysierten OData-Pfad darstellt. Für eine Link-URI sollte der Segmente der Entitätsschlüssel. (Wenn nicht, den Client gesendet hat einen ungültigen URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Löschen von Links**

Um einen Link zu löschen, fügen Sie den folgenden Code aus, um die `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

In diesem Beispiel ist die Navigationseigenschaft eine einzelne `Supplier` Entität. Wenn die Navigationseigenschaft eine Auflistung ist, muss der URI, der die Verknüpfung zu löschen einen Schlüssel für die verknüpfte Entität enthalten. Zum Beispiel:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Diese Anforderung wird von Kunde 1 Reihenfolge 1 entfernt. In diesem Fall wird die DeleteLink-Methode, die folgende Signatur aufweisen:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Die *RelatedKey* Parameter enthält den Schlüssel für die verknüpfte Entität. In diesem Fall Ihre `DeleteLink` -Methode, die primäre Entität durch Suchen der *Schlüssel* Parameter, suchen Sie die verknüpfte Entität von der *RelatedKey* Parameter, und entfernen Sie dann die Zuordnung. Je nach Ihr Datenmodell müssen beide Versionen des implementieren `DeleteLink`. Web-API wird die richtige Version basierend auf den Anforderungs-URI aufgerufen.
