---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Öffnen Sie die Typen in OData v4 mit ASP.NET Web-API | Microsoft Docs"
author: microsoft
description: "In OData v4 ist ein offener Typ eines Stuctured-Typs, das dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Typdefinition deklariert werden. Öffnen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: c2d7454534ff0e9e0a80365793800ab7c45d3b6e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Öffnen Sie die Typen in OData v4 mit ASP.NET Web-API
====================
durch [Microsoft](https://github.com/microsoft)

> In OData v4 ein *öffnen geben* ist ein Stuctured-Typ, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Typdefinition deklariert werden. Offene Typen können Sie Ihre Datenmodelle Flexibilität hinzufügen. In diesem Lernprogramm wird gezeigt, wie offene Typen in ASP.NET Web API OData verwendet werden.
> 
> In diesem Lernprogramm wird davon ausgegangen, dass Sie bereits wissen, wie einen OData-Endpunkt in ASP.NET Web-API zu erstellen. Wenn dies nicht der Fall, lesen Sie zuerst [erstellen Sie einen OData v4-Endpunkt](create-an-odata-v4-endpoint.md) erste.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web API OData 5.3
> - OData v4


Zunächst einige OData-Terminologie:

- Entitätstyp: ein strukturierter Typ mit einem Schlüssel.
- Komplexer Typ: ein strukturierter Typ ohne einen Schlüssel.
- Öffnen Sie: ein Typ mit dynamischen Eigenschaften. Entitätstypen und komplexe Typen können geöffnet sein.

Der Wert einer dynamischen Eigenschaft kann es sich um einen primitiven Typ, komplexer Typ oder Enumerationstyp sein; oder eine Auflistung aller dieser Typen. Weitere Informationen zu offenen Typen, finden Sie unter der [OData v4-Spezifikation](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installieren Sie die Web-OData-Bibliotheken

Verwenden Sie NuGet Package Manager zum Installieren der neuesten Web API OData-Bibliotheken. Aus dem Fenster für die Paket-Manager-Konsole:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definieren Sie die CLR-Typen

Starten Sie durch die Definition der EDM-Modellen als CLR-Typen.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Wenn die Entity Data Model (EDM) erstellt wird,

- `Category`ist ein Enumerationstyp.
- `Address`ist ein komplexer Typ an. (Er weist kein Schlüssel auf, damit es sich nicht um ein Entitätstyp ist.)
- `Customer`ist ein Entitätstyp. (Es hat einen Schlüssel an.)
- `Press`ist ein offener komplexen Typ.
- `Book`ist ein open Entitätstyp.

Um einen offenen Typ zu erstellen, benötigen der CLR-Typ eine Eigenschaft vom Typ `IDictionary<string, object>`, die die dynamischen Eigenschaften enthält.

## <a name="build-the-edm-model"></a>Das EDM-Modell erstellen

Bei Verwendung von **ODataConventionModelBuilder** EDM erstellen `Press` und `Book` werden automatisch hinzugefügt, als open-Typen basierend auf dem Vorhandensein von einer `IDictionary<string, object>` Eigenschaft.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Sie können auch Versionsattribut EDM explizit **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Fügen Sie einem OData-Controller hinzu

Als Nächstes fügen Sie einen OData-Controller. Für dieses Lernprogramm verwenden wir einen vereinfachten Controller nur unterstützt GET und POST anfordert, und verwendet eine in-Memory-Liste, um Entitäten zu speichern.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Beachten Sie, dass die erste `Book` Instanz hat keine dynamischen Eigenschaften. Die zweite `Book` Instanz hat die folgenden dynamischen Eigenschaften:

- "Veröffentlicht": primitiver Typ
- "Autoren": Auflistung primitiver Typen
- "OtherCategories": Sammlung von Enumerationstypen.

Darüber hinaus die `Press` -Eigenschaft dieses `Book` Instanz hat die folgenden dynamischen Eigenschaften:

- "Blog": primitiver Typ
- "Address": komplexer Typ

## <a name="query-the-metadata"></a>Abfragen der Metadaten

Um das Metadatendokument des OData zu erhalten, senden Sie eine GET-Anforderung `~/$metadata`. Der Antworttext sollte etwa wie folgt aussehen:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Aus dem Metadatendokument sehen Sie, die Folgendes:

- Für die `Book` und `Press` Typen wird der Wert, der die `OpenType` Attribut ist "true". Die `Customer` und `Address` Typen nicht über dieses Attribut verfügen.
- Die `Book` Entitätstyp verfügt über drei deklarierten Eigenschaften: ISBN, Titel, und drücken Sie. Die OData-Metadaten umfasst nicht die `Book.Properties` Eigenschaft von der CLR-Klasse.
- Auf ähnliche Weise die `Press` komplexer Typ hat nur zwei deklarierte Eigenschaften: Name und Kategorie. Die Metadaten umfasst keine nicht die `Press.DynamicProperties` Eigenschaft von der CLR-Klasse.

## <a name="query-an-entity"></a>Die Abfrage eine Entität

Um das Adressbuch mit ISBN gleich "978-0-7356-7942-9" erhalten, senden senden eine GET-Anforderung `~/Books('978-0-7356-7942-9')`. Der Antworttext sollte etwa wie folgt aussehen. (Eingerückt, um sie besser lesbar zu machen.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Beachten Sie, dass die dynamischen Eigenschaften Inlineschemainformationen enthält die deklarierten Eigenschaften sind.

## <a name="post-an-entity"></a>Bereitstellen einer Entität

Um ein Buch Entität hinzuzufügen, senden Sie eine POST-Anforderung an `~/Books`. Der Client festlegen in der Anforderungsnutzlast dynamische Eigenschaften.

Hier ist eine Beispiel für eine Anforderung aus. Beachten Sie die Eigenschaften "Price" und "Veröffentlicht".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Wenn Sie einen Haltepunkt in der Controllermethode festlegen, können Sie sehen, dass Web-API zu diesen Eigenschaften hinzugefügt der `Properties` Wörterbuch.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[OData-öffnen-Typ-Beispiel](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
