---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen eines Singletons in OData v4 mithilfe von Web-API 2.2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Thema veranschaulicht definieren Sie einen Singleton in einem OData-Endpunkt im Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390926"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Erstellen eines Singletons in OData v4 mithilfe von Web-API 2.2
====================
durch Zoe Luo

> In der Vergangenheit konnte eine Entität nur zugegriffen werden kann, wenn diese nicht in einer Entitätenmenge gekapselt wurden. OData v4 stellt zwei zusätzliche Optionen, Singleton und Kapselung, beide Web-API 2.2 unterstützt jedoch.


In diesem Artikel zeigt, wie einen Singleton in einem OData-Endpunkt im Web API 2.2 definiert wird. Informationen dazu, welche ein Singleton ist und wie Sie aus Ihrer Verwendung profitieren können, finden Sie unter [mit, dass ein Singleton spezielle Entität definieren](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Zum Erstellen eines OData V4-Endpunkts in Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md). 

Wir erstellen einen Singleton in Ihrem Web-API-Projekt mit dem folgenden Datenmodell:

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Einen Singleton namens `Umbrella` werden basierend auf Typ definiert `Company`, und einer Entität mit dem Namen `Employees` werden basierend auf Typ definiert `Employee`.

Die Lösung, die in diesem Tutorial verwendete kann heruntergeladen werden [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Hier `builder.Singleton<Company>("Umbrella")` weist den Modellgenerator, erstellen Sie einen Singleton namens `Umbrella` in das EDM-Modell.

    Die generierte Metadaten sieht folgendermaßen aus:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Aus den Metadaten sehen, dass die Navigationseigenschaft `Company` in die `Employees` Entitätenmenge gebunden ist, auf das Singleton `Umbrella`. Die Bindung erfolgt automatisch durch `ODataConventionModelBuilder`, da nur `Umbrella` hat die `Company` Typ. Wenn im Modell eine Mehrdeutigkeit vorliegt, können Sie `HasSingletonBinding` , eine Navigationseigenschaft explizit in ein Singleton; binden `HasSingletonBinding` hat dieselbe Wirkung wie die Verwendung der `Singleton` Attribut in der Definition der CLR-Typ:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definieren der Singleton-Controllers

Wie auch der EntitySet-Controller, erbt der Controller Singleton `ODataController`, und die Singleton-Controller-Name sollte `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vorab definiert werden. **Attributrouting** in Web-API 2.2 standardmäßig aktiviert ist. Beispielsweise, um eine Aktion zum Verarbeiten von Abfragen definieren `Revenue` aus `Company` das attributrouting, verwenden Sie Folgendes:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Wenn Sie nicht zum Definieren von Attributen für jede Aktion bereit sind, definieren Sie einfach die folgenden Aktionen [OData-Konventionen für Routing](../odata-routing-conventions.md). Da ein Schlüssel nicht erforderlich, für die Abfrage eines Singleton ist, sind die Aktionen, die in der Singleton-Controller definierten geringfügig von den Aktionen im Controller Entityset definiert.

Zu Referenzzwecken Methodensignaturen für jede Aktionsdefinition im Controller Singleton weiter unten.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Im Grunde ist dies alles, was, die Sie auf der Dienstseite tun müssen. Die [Beispielprojekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält alle von der Code für die Projektmappe und der OData-Client, der zeigt, wie Sie mit der Singleton. Der Client wird erstellt, indem Sie die Schritte in [erstellen Sie eine Client-App in OData v4](create-an-odata-v4-client-app.md).

sein. 

*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*
