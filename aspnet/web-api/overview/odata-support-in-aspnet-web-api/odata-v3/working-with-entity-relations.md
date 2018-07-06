---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Unterstützung für Entitätsbeziehungen in OData v3 mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten. Mithilfe von OData, können Clients über navigieren...'
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: b24e3ca4e3d39b424bec6bb408bb0f85825c6761
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810743"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Unterstützung für Entitätsbeziehungen in OData v3 mit Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten. Mithilfe von OData, können Clients auf entitätsbeziehungen navigieren. Wenn ein Produkt, finden Sie den Lieferanten. Sie können auch erstellt oder Beziehungen entfernt werden. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
> 
> In diesem Tutorial wird gezeigt, wie diese Vorgänge in ASP.NET Web-API unterstützt. Das Tutorial baut auf dem Tutorial [erstellen eine OData v3-Endpunkts mit Web-API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API 2
> - OData v3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Fügen Sie eine Entität "Supplier" hinzu.

Zuerst müssen wir unsere OData-feed ein neuer Entitätstyp hinzu. Wir fügen eine `Supplier` Klasse.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Diese Klasse verwendet eine Zeichenfolge für den Entitätsschlüssel. In der Praxis kann dies seltener auf als die Verwendung eines Schlüssels für die ganze Zahl sein. Aber es lohnt sich sehen, wie OData andere Schlüsseltypen als ganze Zahlen verarbeitet.

Als Nächstes erstellen wir eine Beziehung durch Hinzufügen einer `Supplier` Eigenschaft, um die `Product` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Fügen Sie einen neuen **"DbSet"** auf die `ProductServiceContext` Klasse, sodass Entity Framework enthält die `Supplier` Tabelle in der Datenbank.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Fügen Sie eine Entität "Suppliers" in WebApiConfig.cs auf das EDM-Modell:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Navigationseigenschaften

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Hier ist "Supplier" eine Navigationseigenschaft auf der `Product` Typ. In diesem Fall `Supplier` bezieht sich auf ein einzelnes Element, aber eine Navigation kann Eigenschaft auch eine Auflistung (1: n- oder m: n Beziehung) zurückgeben.

Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode der `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Die *Schlüssel* Parameter ist der Schlüssel des Produkts. Die Methode gibt zurück, die verknüpfte Entität & #8212 in diesem Fall eine `Supplier` Instanz. Der Methodenname und Parameternamen sind wichtig. Wenn die Navigationseigenschaft "X" benannt wird, müssen Sie in der Regel zum Hinzufügen einer Methode, die mit dem Namen "GetX". Die Methode muss einen Parameter namens annehmen "*Schlüssel*", die den Datentyp des Schlüssels des übergeordneten Elements entspricht.

Es ist auch wichtig, Sie enthalten die **[FromOdataUri]** -Attribut in der *Schlüssel* Parameter. Dieses Attribut teilt die Web-API-OData-Syntaxregeln beim Analysieren des Schlüssels aus der Anforderungs-URI zu verwenden.

## <a name="creating-and-deleting-links"></a>Erstellen und Löschen von Links

OData unterstützt erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten. In der OData-Terminologie ist die Beziehung "Link". Jeder Link verfügt über einen URI mit dem Formular *Entität*/$links /*Entität*. Beispielsweise sieht der Verbindung vom Produkt zum Lieferanten folgendermaßen aus:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Um einen neuen Link erstellen zu können, sendet der Client eine POST-Anforderung auf den Link-URI. Der Hauptteil der Anforderung ist der URI der Zielentität. Nehmen wir beispielsweise an, dass ein Anbieter mit dem Schlüssel "CTSO" vorhanden ist. Um eine Verknüpfung zwischen "Product(1)" und "Supplier('CTSO')" zu erstellen, sendet der Client eine Anforderung wie folgt:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Um einen Link zu löschen, sendet der Client eine DELETE-Anforderung auf den Link-URI.

**Erstellen von Verknüpfungen**

Um einen Client zum Erstellen von Produktlieferant Links zu aktivieren, fügen Sie den folgenden Code der `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Diese Methode akzeptiert drei Parameter:

- *Schlüssel*: der Schlüssel für die übergeordnete Entität (das Produkt)
- *NavigationProperty*: der Name der Navigationseigenschaft. In diesem Beispiel ist die einzige gültige Navigationseigenschaft "Supplier".
- *Link*: der OData-URI der verknüpften Entität. Dieser Wert stammt aus dem Anforderungstext. Beispielsweise den Link URI möglicherweise "`http://localhost/odata/Suppliers('CTSO')`, d. h. den Lieferanten mit ID ="CTSO".

Die Methode verwendet den Link, um den Lieferanten zu suchen. Wenn der entsprechende Lieferant gefunden wird, wird die-Methode legt die `Product.Supplier` Eigenschaft und speichert das Ergebnis in der Datenbank.

Der schwierigste Teil ist den Link-URI wird analysiert. Im Grunde müssen Sie das Ergebnis des Sendens einer GET-Anforderung an diesen URI zu simulieren. Die folgende Hilfsmethode veranschaulicht dies. Die Methode ruft die Web-API-routing-Prozess und zurückerhält ein **OData-Pfad** -Instanz, die analysierten OData-Pfad darstellt. Nach einem Link-URI muss eines der Segmente des Entitätsschlüssels. (Wenn nicht, den Client gesendet wurde einen ungültigen URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Löschen von Links**

Um einen Link zu löschen, fügen Sie den folgenden Code der `ProductsController` Klasse:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

In diesem Beispiel ist die Navigationseigenschaft eine einzelne `Supplier` Entität. Wenn die Navigationseigenschaft eine Auflistung ist, muss der URI, um die Verknüpfung löschen einen Schlüssel für die verknüpfte Entität enthalten. Zum Beispiel:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Diese Anforderung wird die Reihenfolge 1 von Kunde 1 entfernt. In diesem Fall verfügen die DeleteLink-Methode die folgende Signatur:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Die *RelatedKey* Parameter enthält den Schlüssel für die verknüpfte Entität. Im Ihre `DeleteLink` -Methode, suchen die primäre Entität von der *Schlüssel* Parameter finden Sie die verknüpfte Entität durch die *RelatedKey* -Parameter, und entfernen Sie dann die Zuordnung. Je nach Ihrem Datenmodell müssen möglicherweise beide Versionen implementieren `DeleteLink`. Web-API wird die richtige Version, die basierend auf der Anforderungs-URI aufgerufen.
