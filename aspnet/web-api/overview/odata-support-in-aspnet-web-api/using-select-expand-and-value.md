---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Verwenden $select, $expand, und $value in ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Verwenden $select, $expand, und $value in ASP.NET Web API 2 OData
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2 bietet Unterstützung für die $expand, $select und $value Optionen auf OData. Mit diesen Optionen können einen Client, um die Darstellung zu steuern, die es wieder vom Server abruft.

- **$expand-** bewirkt, dass verknüpfte Entitäten Inlineschemainformationen enthält, in der Antwort werden.
- **$select** wählt eine Teilmenge der Eigenschaften in die Antwort eingeschlossen werden sollen.
- **$value** Ruft den Rohwert einer Eigenschaft ab.

## <a name="example-schema"></a>Beispielschema

Für diesen Artikel verwende ich einen OData-Dienst, der drei Entitäten definiert: Product, Lieferanten und Kategorie. Jedes Produkt verfügt über eine Kategorie und ein Lieferant.

![](using-select-expand-and-value/_static/image1.png)

Hier sind die C#-Klassen, die die Entitätsmodelle zu definieren:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Beachten Sie, dass die `Product` Klasse definiert die Navigationseigenschaften für das `Supplier` und `Category`. Die `Category` Klasse definiert eine Navigationseigenschaft für die Produkte in jeder Kategorie.

Verwenden Sie das Gerüst für Visual Studio 2013, um einen OData-Endpunkt für dieses Schema zu erstellen, wie in beschrieben [erstellen einen OData-Endpunkt in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Fügen Sie separate Controller für Produkt, Kategorie und Lieferanten.

## <a name="enabling-expand-and-select"></a>Aktivieren von $erweitern und $select

In Visual Studio 2013 das Web API OData Gerüst einen Controller, automatisch unterstützt, die $expand- und erstellt $select. Hier zu Referenzzwecken werden die Anforderungen zur Unterstützung von $erweitern und $select in einem Controller.

Bei Auflistungen, die Controller des `Get` Methodenrückgabewert muss ein **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Für einzelne Entitäten Zurückgeben einer **SingleResult&lt;T&gt;**, wobei T ist ein **IQueryable** , NULL oder eins Entitäten enthält.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Außerdem ergänzen die `Get` Methoden mit den **[Queryable]** Attribut, wie im vorherigen Codeausschnitte dargestellt. Rufen Sie alternativ **EnableQuerySupport** auf die **HttpConfiguration** Objekt beim Start. (Weitere Informationen finden Sie unter [Aktivieren der OData-Abfrageoptionen](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Erweitern Sie mithilfe von $

Wenn Sie eine OData-Entität oder eine Auflistung Abfragen, umfasst die Standardantwort nicht verknüpfte Entitäten. So sieht z. B. die standardmäßige Antwort für die Entitätenmenge Kategorien aus:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Wie Sie sehen können, enthält die Antwort alle Produkte aus, keine, obwohl die Category-Entität einen Navigationslink Produkte verfügt. Der Client kann jedoch $ verwenden erweitert, um die Liste der Produkte für die einzelnen Kategorien zu erhalten. Der $expand-Option wird in der Abfragezeichenfolge der Anforderung:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Der Server wird nun die Produkte für jede Kategorie, die Inline in die Kategorien enthalten. So sieht die antwortnutzlast aus:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Beachten Sie, dass jeder Eintrag im Array "Value" eine Produktliste enthält.

Die $expand-Option akzeptiert eine durch Trennzeichen getrennte Liste der Navigationseigenschaften zu erweitern. Die folgende Anforderung wird erweitert, die Kategorie und den Lieferanten für ein Produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Sie können mehr als eine Navigationseigenschaft übergeordnete Ebene erweitern. Das folgende Beispiel schließt alle Produkte für eine Kategorie und auch den Lieferanten für jedes Produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Web-API schränkt standardmäßig die maximale erweiterungstiefe auf 2. Die verhindert, dass den Client senden komplexere Anforderungen wie `$expand=Orders/OrderDetails/Product/Supplier/Region`, Abfragen und Erstellen von großen Antworten ineffizient sein. Um die Standardeinstellung zu überschreiben, legen die **MaxExpansionDepth** Eigenschaft auf die **[Queryable]** Attribut.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Weitere Informationen zu den $expand-Option, finden Sie unter [Expand System Query-Option (expand, $)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in der offiziellen OData-Dokumentation.

## <a name="using-select"></a>$Select verwenden

Die $select-Option gibt eine Teilmenge der Eigenschaften in den Antworttext eingeschlossen werden sollen. Um nur den Namen und den Preis jedes Produkts zu erhalten, verwenden Sie z. B. die folgende Abfrage:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Sie kombinieren können, $select und $expand-in derselben Abfrage. Stellen Sie sicher, dass die erweiterte Eigenschaft in der $select-Option. Die folgende Anforderung ruft z. B. ab, der Produktname und Lieferanten.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Sie können auch die Eigenschaften in einer erweiterten Eigenschaft auswählen. Die folgende Anforderung wird erweitert, Produkte und wählt Kategorienamen plus Produktname.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Weitere Informationen zur Option $select finden Sie unter [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in der offiziellen OData-Dokumentation.

## <a name="getting-individual-properties-of-an-entity-value"></a>Beim Abrufen der einzelnen Eigenschaften einer Entität ($value)

Es gibt zwei Möglichkeiten für einen OData-Client auf eine einzelne Eigenschaft einer Entität abzurufen. Der Client kann entweder rufe den Wert in OData-Format oder Abrufen den unformatierten Wert der Eigenschaft.

Die folgende Anforderung Ruft eine Eigenschaft in OData-Format ab.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Hier ist eine Beispielantwort im JSON-Format ein:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Fügen Sie zum Abrufen des unformatierten Wert der Eigenschaft $value an den URI ein:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

So sieht die Antwort aus. Beachten Sie, dass der Inhaltstyp "Text/Plain" nicht JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Um diese Abfragen in der OData-Controller zu unterstützen, fügen Sie eine Methode namens `GetProperty`, wobei `Property` ist der Name der Eigenschaft. Z. B. den Namen der Methode zum Abrufen der Eigenschaft Name `GetName`. Die Methode sollte den Wert dieser Eigenschaft zurückgeben:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
