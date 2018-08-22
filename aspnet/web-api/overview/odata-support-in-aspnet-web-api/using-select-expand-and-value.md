---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826568"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2 bietet Unterstützung für die $expand, $select und Optionen für $value in OData. Mit diesen Optionen können einen Client, um die Darstellung zu steuern, die sie wieder vom Server abruft.

- **der $expand-** bewirkt, dass verknüpfte Entitäten Inlineschemainformationen enthält, in der Antwort zu sein.
- **$select** wählt eine Teilmenge der Eigenschaften in der Antwort eingeschlossen werden sollen.
- **$value** Ruft den Rohwert einer Eigenschaft ab.

## <a name="example-schema"></a>Beispielschema

In diesem Artikel verwende ich einen OData-Dienst, der drei Entitäten definiert: Produkt, Lieferanten und Kategorie. Jedes Produkt verfügt über eine Kategorie und ein Lieferant.

![](using-select-expand-and-value/_static/image1.png)

Hier sind die C#-Klassen, die die Entitätsmodelle zu definieren:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Beachten Sie, dass die `Product` -Klasse definiert die Navigationseigenschaften für die `Supplier` und `Category`. Die `Category` Klasse definiert eine Navigationseigenschaft für die Produkte in jeder Kategorie.

Verwenden Sie das Gerüst für Visual Studio 2013, um einen OData-Endpunkt für dieses Schema zu erstellen, siehe [Erstellen eines OData-Endpunkts in ASP.NET Web-API](odata-v3/creating-an-odata-endpoint.md). Fügen Sie für Produkt, Kategorie und Lieferanten separate Controller hinzu.

## <a name="enabling-expand-and-select"></a>Erweitern Sie mit der Aktivierung von $ und $select

In Visual Studio 2013 erstellt die Web-API OData-Gerüst ein Controllers, automatisch unterstützt, die $expand- und $select. Zu Referenzzwecken hier sind die Anforderungen zur Unterstützung von $erweitern und $select in einem Controller.

Für Sammlungen, die Controller `Get` Methodenrückgabewert muss ein **"IQueryable"**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Für einzelne Entitäten Zurückgeben einer **"SingleResult"&lt;T&gt;**, wobei T ist ein **"IQueryable"** , das NULL Entitäten oder eine Entität enthält.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Außerdem ergänzen Ihre `Get` Methoden mit dem **[Queryable]** Attribut, wie in den vorherigen Codeausschnitten gezeigt. Rufen Sie alternativ **EnableQuerySupport** auf die **HttpConfiguration** Objekt beim Start. (Weitere Informationen finden Sie unter [Aktivieren der OData-Abfrageoptionen](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Erweitern Sie unter Verwendung von $

Beim Abfragen von einem OData-Entität oder einer Auflistung schließt die Standardantwort verknüpfte Entitäten nicht. So sieht z. B. die Standardantwort für die Entitätenmenge für die Kategorien aus:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Wie Sie sehen, enthält die Antwort alle Produkte, keine, obwohl die Category-Entität einen Navigationslink für die Produkte verfügt. Der Client kann jedoch $ verwenden erweitert werden, um die Liste der Produkte für die einzelnen Kategorien zu erhalten. Der $expand-Option wird in der Abfragezeichenfolge der Anforderung:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Der Server wird jetzt die Produkte für die einzelnen Kategorien, die Inline in die Kategorien enthalten. Hier ist die Nutzlast der Antwort aus:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Beachten Sie, dass jeder Eintrag im Array "Value" eine Liste der Produkte enthält.

Der $expand-Option akzeptiert eine durch Trennzeichen getrennte Liste der Navigationseigenschaften zu erweitern. Die folgende Anforderung wird erweitert, sowohl die Kategorie und den Lieferanten für ein Produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Sie können mehrere Ebenen der Navigationseigenschaft erweitern. Das folgende Beispiel schließt alle Produkte für eine Kategorie und der Lieferant für jedes Produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Standardmäßig beschränkt die Web-API die maximale erweiterungstiefe auf 2. Die wird verhindert, dass den Client sendet komplexe Anforderungen wie `$expand=Orders/OrderDetails/Product/Supplier/Region`, der möglicherweise ineffizient, Abfragen und große Antworten erstellen. Um die Standardeinstellung zu überschreiben, legen die **MaxExpansionDepth** Eigenschaft für die **[Queryable]** Attribut.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Weitere Informationen zur $expand-Option, finden Sie unter [Expand System Query-Option (expand, $)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in der offiziellen OData-Dokumentation.

## <a name="using-select"></a>$Select verwenden

Die $select-Option gibt eine Teilmenge der Eigenschaften in den Antworttext eingeschlossen werden sollen. Um nur die Namen und die Preise für jedes Produkt zu erhalten, verwenden Sie z. B. die folgende Abfrage:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Können kombiniert werden $select und $expand, in der gleichen Abfrage. Stellen Sie sicher, dass die erweiterte Eigenschaft in der Option "$select" enthalten. Die folgende Anforderung ruft z. B. ab, der Produktname und Lieferanten.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Sie können auch die Eigenschaften in einer erweiterten Eigenschaft auswählen. Die folgende Anforderung wird erweitert, Produkte und wählt Kategorienamen und Produktname.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Hier ist der Antworttext:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Weitere Informationen zur Option "$select" finden Sie unter [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in der offiziellen OData-Dokumentation.

## <a name="getting-individual-properties-of-an-entity-value"></a>Abrufen von einzelnen Eigenschaften einer Entität ($value)

Es gibt zwei Möglichkeiten für einen OData-Client, um eine einzelne Eigenschaft aus einer Entität zu erhalten. Der Client kann entweder rufe den Wert in OData-Format, oder rufen Sie den raw-Wert der Eigenschaft.

Die folgende Anforderung Ruft eine Eigenschaft in der OData-Format.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Hier ist Beispiel zeigt eine Antwort im JSON-Format ein:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Um den unformatierten Wert der Eigenschaft zu erhalten, fügen Sie $value an den URI ein:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Hier ist die Antwort. Beachten Sie, dass der Inhaltstyp "Text/Plain", nicht JSON ist.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Um diese Abfragen in Ihren OData-Controller zu unterstützen, fügen Sie eine Methode namens `GetProperty`, wobei `Property` ist der Name der Eigenschaft. Beispielsweise würde die Methode zum Abrufen der Eigenschaft Name den Namen `GetName`. Die Methode sollte den Wert dieser Eigenschaft zurückgeben:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
