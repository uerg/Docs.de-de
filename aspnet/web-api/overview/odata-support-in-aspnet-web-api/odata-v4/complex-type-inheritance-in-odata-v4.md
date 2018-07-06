---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung von komplexen Typen in OData v4 mit ASP.NET Web-API | Microsoft-Dokumentation
author: microsoft
description: Gemäß der OData v4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist einen strukturierten Typ ohne einen Schlüssel.) Web-API...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824029"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Vererbung von komplexen Typen in OData v4 mit ASP.NET Web-API
====================
durch [Microsoft](https://github.com/microsoft)

> Gemäß den OData v4 [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/), ein komplexer Typ kann von einem anderen komplexen Typ erben. (Ein *komplexe* Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web-API OData 5.3 unterstützt die Vererbung von komplexen Typen.
> 
> In diesem Thema veranschaulicht die Erstellung ein Entity Data Model (EDM) mit von komplexen Vererbungstypen. Den vollständigen Quellcode, finden Sie unter [OData Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Hierarchie des integritätsmodells

Um die Vererbung von komplexen Typen zu veranschaulichen, verwenden wir die folgende Hierarchie von Klassen.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` wird ein abstrakter komplexer Typ. `Rectangle`, `Triangle`, und `Circle` komplexe Typen abgeleitet sind `Shape`, und `RoundRectangle` leitet sich von `Rectangle`. `Window` ein Entitätstyp und enthält eine `Shape` Instanz.

Hier sind die CLR-Klassen, die diese Typen definieren.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Das EDM-Modell erstellen

Um das EDM zu erstellen, können Sie **ODataConventionModelBuilder**, dem ableitet vererbungsbeziehungen von der CLR-Typen.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Sie können auch erstellen, das EDM explizit mit **ODataModelBuilder**. Dies nimmt mehr Code, aber es gibt Ihnen mehr Kontrolle über das EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Diese beiden Beispiele erstellen, das gleiche EDM-Schema.

## <a name="metadata-document"></a>Metadatendokument

Hier ist das OData-Dokument, mit der Vererbung von komplexen Typen.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Aus dem Dokument sehen Sie, die Folgendes:

- Die `Shape` komplexe Typ abstrakt ist.
- Die `Rectangle`, `Triangle`, und `Circle` komplexen Typs haben den Basistyp `Shape`.
- Die `RoundRectangle` Typ muss der Basistyp `Rectangle`.

## <a name="casting-complex-types"></a>Wandeln komplexe Typen

Für komplexe Typen umwandeln, wird jetzt unterstützt. Z. B. die folgende Abfrage Wandelt eine `Shape` zu einem `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Hier ist die Nutzlast der Antwort aus:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
