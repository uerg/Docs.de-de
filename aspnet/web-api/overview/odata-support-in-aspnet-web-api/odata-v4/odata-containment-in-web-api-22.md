---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Kapselung in OData v4-Web-API 2.2 mit | Microsoft-Dokumentation
author: rick-anderson
description: In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden. OData v4 stellt zwei zusätzliche Optionen, die Singleton-als auch Nachteile jedoch...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365606"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Kapselung in OData v4 mithilfe von Web-API 2.2
====================
durch Jinfu Tan

> In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden. OData v4 stellt zwei zusätzliche Optionen, Singleton und Kapselung, beide Web-API 2.2 unterstützt jedoch.


In diesem Thema veranschaulicht, wie eine Kapselung in einem OData-Endpunkt in der Web-API 2.2 zu definieren. Weitere Informationen zu Kapselung, finden Sie unter [Kapselung kommt mit OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Zum Erstellen eines OData V4-Endpunkts in Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md).

Zunächst erstellen ein Domänenmodell für die Kapselung in OData-Diensts wir mit diesem Datenmodell:

![Datenmodell](odata-containment-in-web-api-22/_static/image1.png)

Ein Konto enthält viele PaymentInstruments (PI), aber nicht definieren wir eine Entitätenmenge für einen PI. Stattdessen können der PIs nur über ein Konto zugegriffen werden.

Sie können die Lösung, die in diesem Thema verwendet [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Die `Contained` Attribut wird für Navigationseigenschaften Containment verwendet.
2. Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Die `ODataConventionModelBuilder` übernimmt das EDM-Modell erstellen, wenn die `Contained` Attribut wird die entsprechende Navigationseigenschaft hinzugefügt. Wenn die Eigenschaft ein Sammlungstyp ist eine `GetCount(string NameContains)` Funktion wird auch erstellt werden.

    Die generierte Metadaten sieht folgendermaßen aus:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Die `ContainsTarget` Attribut gibt an, dass die Navigationseigenschaft eine Kapselung ist.

## <a name="define-the-containing-entity-set-controller"></a>Definieren des enthaltenden Entität Set-Controllers

Enthaltene Entitäten keine eigenen Controller; die Aktion, die im enthaltenden Entität Satz Controller definiert ist. In diesem Beispiel ist ein AccountsController, aber keine PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Ist der OData-Pfad mindestens 4 Segmente, nur Attribut routing funktioniert, z. B. `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` im oben genannten Controller. Andernfalls sowohl Attribut als auch beim herkömmlichen routing funktioniert: z. B. `GetPayInPIs(int key)` entspricht `GET ~/Accounts(1)/PayinPIs`.

*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*
