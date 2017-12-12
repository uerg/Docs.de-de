---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Kapselung in OData v4 mithilfe von Web-API 2.2 | Microsoft Docs
author: rick-anderson
description: "Bisher konnte eine Entität nur zugegriffen werden, wenn es in einer Entitätenmenge gekapselt wurden. OData v4 bietet zwei zusätzliche Optionen Singleton und Con jedoch..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Kapselung in OData v4 mithilfe von Web-API 2.2
====================
durch Jinfu Tan

> Bisher konnte eine Entität nur zugegriffen werden, wenn es in einer Entitätenmenge gekapselt wurden. OData v4 bietet zwei zusätzliche Optionen Singleton und Aufnahme, beide WebAPI 2.2 unterstützt jedoch.


In diesem Thema wird gezeigt, wie Kapselung in einem OData-Endpunkt in WebApi 2.2 definiert. Weitere Informationen zu Containment, finden Sie unter [Containment stammt mit OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Zum Erstellen einer OData V4-Endpunkts in der Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md).

Zunächst erstellen ein Domänenmodell Containment im OData-Dienst wir dieses Datenmodell verwenden:

![Datenmodell](odata-containment-in-web-api-22/_static/image1.png)

Ein Konto enthält viele PaymentInstruments (PI), aber nicht definieren wir eine Entitätenmenge, die für eine PI. Stattdessen können der PIs nur über ein Konto zugegriffen werden.

Sie können die Projektmappe, die in diesem Thema aus verwendet herunterladen [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Die `Contained` Attribut wird für Navigationseigenschaften Containment verwendet.
2. Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Die `ODataConventionModelBuilder` werden verarbeitet, die das EDM-Modell erstellen, wenn die `Contained` wird die entsprechende Navigationseigenschaft Attribut hinzugefügt. Wenn die Eigenschaft ein Sammlungstyp ist eine `GetCount(string NameContains)` Funktion wird auch erstellt werden.

    Die generierte Metadaten wird wie folgt aussehen:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Die `ContainsTarget` Attribut gibt an, dass die Navigationseigenschaft eine Kapselung ist.

## <a name="define-the-containing-entity-set-controller"></a>Definieren Sie die enthaltende Entität Satz controller

Enthaltenen Entitäten verfügen nicht ihre eigenen Controller; die Aktion, die in der enthaltenden Entität Satz Controller definiert ist. In diesem Beispiel ist ein AccountsController, aber keine PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Wenn der OData-Pfad 4 oder mehr Segmente ist, nur Attribut routing funktioniert, z. B. `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` im obigen Controller. Andernfalls sowohl Attribute als auch herkömmliche routing funktioniert: z. B. `GetPayInPIs(int key)` entspricht `GET ~/Accounts(1)/PayinPIs`.

*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*
