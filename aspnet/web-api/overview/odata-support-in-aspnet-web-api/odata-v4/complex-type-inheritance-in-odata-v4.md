---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung von komplexen Typ in OData v4 mit ASP.NET Web-API | Microsoft Docs
author: microsoft
description: Gemäß den OData v4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web-API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508419"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Vererbung von komplexen Typ in OData v4 mit ASP.NET Web-API
====================
durch [Microsoft](https://github.com/microsoft)

> Gemäß den OData v4 [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/), ein komplexer Typ kann von einer anderen komplexen Typ erben. (Ein *komplexe* Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web API OData 5.3 unterstützt Vererbung von komplexen Typ.
> 
> Dieses Thema veranschaulicht das Erstellen eines Entity Data Modells (EDM) mit von komplexen Vererbungstypen. Den vollständigen Quellcode, finden Sie unter [OData-Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Modellhierarchie

Um die Vererbung von komplexen Typ zu veranschaulichen, verwenden wir die folgenden Klassenhierarchie.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`ist ein abstrakte komplexer Typ. `Rectangle`, `Triangle`, und `Circle` komplexe Typen abgeleitet sind `Shape`, und `RoundRectangle` leitet sich von `Rectangle`. `Window`ein Entitätstyp ist, und enthält eine `Shape` Instanz.

Hier werden die CLR-Klassen, die diese Typen zu definieren.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Das EDM-Modell erstellen

Um das EDM zu erstellen, können Sie **ODataConventionModelBuilder**, dem leitet die vererbungsbeziehungen von CLR-Typen ab.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Sie können auch Versionsattribut EDM explizit **ODataModelBuilder**. Dies nimmt mehr Code, bietet Ihnen jedoch mehr Kontrolle über das EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Diese beiden Beispiele erstellen, das gleiche EDM-Schema.

## <a name="metadata-document"></a>Metadatendokument

Hier ist die Vererbung von komplexen Typ mit OData-Metadatendokument ein.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Aus dem Metadatendokument sehen Sie, die Folgendes:

- Die `Shape` komplexer Typ abstrakt ist.
- Die `Rectangle`, `Triangle`, und `Circle` komplexen Typs haben den Basistyp `Shape`.
- Die `RoundRectangle` Typ ist den Basistyp `Rectangle`.

## <a name="casting-complex-types"></a>Umwandeln von komplexen Typen

Für complex-Typen umwandeln wird jetzt unterstützt. Die folgende Abfrage beispielsweise Umwandlungen eine `Shape` zu einem `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

So sieht die antwortnutzlast aus:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
