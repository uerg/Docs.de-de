---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen Sie einen Singleton in OData v4 mithilfe von Web-API 2.2 | Microsoft Docs
author: rick-anderson
description: In diesem Thema wird gezeigt, wie einen Singleton in einem OData-Endpunkt im Web-API 2.2 definiert.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508209"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Erstellen Sie einen Singleton in OData v4 mithilfe von Web-API 2.2
====================
durch Zoe Luo

> Bisher konnte eine Entität nur zugegriffen werden, wenn es in einer Entitätenmenge gekapselt wurden. OData v4 bietet zwei zusätzliche Optionen Singleton und Aufnahme, beide WebAPI 2.2 unterstützt jedoch.


In diesem Artikel wird gezeigt, wie einen Singleton in einem OData-Endpunkt im Web-API 2.2 definiert. Informationen über welche Singleton ist, und erläutert, wie Sie aus ihrer Verwendung profitieren können, finden Sie unter [mit Singleton spezielle Entität definieren](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Zum Erstellen einer OData V4-Endpunkts in der Web-API finden Sie unter [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2.2](create-an-odata-v4-endpoint.md). 

Wir erstellen einen Singleton in Ihrer Web-API-Projekt mithilfe des folgenden-Datenmodells:

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Ein Singleton, mit dem Namen `Umbrella` basierend auf Typ definiert `Company`, und einer Entität mit der Bezeichnung `Employees` basierend auf Typ definierten `Employee`.

Die Lösung, die in diesem Lernprogramm verwendete heruntergeladen werden kann [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Das EDM-Modell auf Grundlage der CLR-Typen zu generieren.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Hier `builder.Singleton<Company>("Umbrella")` weist den Modellgenerator, erstellen Sie einen Singleton, mit dem Namen `Umbrella` im EDM-Modell.

    Die generierte Metadaten wird wie folgt aussehen:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Aus den Metadaten sehen, der die Navigationseigenschaft `Company` in der `Employees` Entitätenmenge an den Singleton gebunden ist `Umbrella`. Die Bindung erfolgt automatisch vom `ODataConventionModelBuilder`, da nur `Umbrella` hat die `Company` Typ. Wenn keine Mehrdeutigkeiten im Modell vorhanden ist, können Sie `HasSingletonBinding` , eine Navigationseigenschaft explizit in ein Singleton; binden `HasSingletonBinding` hat dieselbe Wirkung wie die Verwendung der `Singleton` Attribut in der Definition des CLR-Typ:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definieren Sie den Singleton-controller

Wie den Controller EntitySet erbt die Singleton-Controller von `ODataController`, des Controllernamens Singleton sollte auch `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vorab definiert werden. **-Attribut routing** WebApi 2.2 standardmäßig aktiviert. Um beispielsweise eine Aktion zum Verarbeiten von Abfragen definieren `Revenue` aus `Company` mithilfe von routing-Attribut, verwenden Sie die folgenden:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Wenn Sie nicht zum Definieren von Attributen für jede Aktion bereit sind, definieren Sie nur die folgenden Aktionen [OData-Konventionen für das Routing](../odata-routing-conventions.md). Da ein Schlüssel nicht erforderlich, zum Abfragen von Singleton ist, werden die Aktionen im Controller Singleton definiert geringfügig von Aktionen im Controller Entityset definiert.

Zu Referenzzwecken sind Methodensignaturen für jede Aktionsdefinition im Controller Singleton unten aufgeführt.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Im Grunde ist dies alles, was, die Sie auf der Dienstseite ausführen müssen. Die [Beispielprojekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält alle des Codes für die Projektmappe und der OData-Client, der zeigt, wie Sie den Singleton. Der Client wird erstellt, indem Sie die Schritte in [erstellen Sie eine Client-App für OData v4](create-an-odata-v4-client-app.md).

. 

*Unser Dank gilt Leo Hu für den ursprünglichen Inhalt dieses Artikels.*
