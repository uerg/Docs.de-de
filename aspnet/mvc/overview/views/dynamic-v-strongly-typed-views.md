---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamische V. Stark typisierte Ansichten | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504089"
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamische V. Stark typisierte Ansichten
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

Es gibt drei Möglichkeiten, um Informationen von einem Controller in ASP.NET MVC 3 übergeben:

1. Als stark typisierte Model-Objekts.
2. Als ein dynamischer Typ (mit @model dynamische)
3. Verwenden das ViewBag-Objekt

Ich habe eine einfache MVC 3 oben Blog-Anwendung zu vergleichen und vergleichen Sie dynamische und stark typisierte Ansichten geschrieben. Der Controller beginnt mit einer einfachen Liste von Blogs:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Klicken Sie mit der mit der rechten Maustaste in der IndexNotStonglyTyped()-Methode, und fügen Sie einer Razor-Ansicht.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Stellen Sie sicher, dass die **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert ist. Die Ansicht enthalten nicht viel:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Da wir einen dynamischen und nicht auf einer stark typisierten Ansicht verwenden, nicht in Intellisense uns helfen. Der vollständige Code wird unten gezeigt:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Nun fügen wir eine stark typisierte Ansicht. Fügen Sie den folgenden Code an den Controller aus:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Beachten Sie, dass sie genau die gleichen return View(topBlogs) ist. Rufen Sie als nicht stark typisierten Ansicht. Klicken Sie mit der rechten Maustaste innerhalb des *StonglyTypedIndex()* , und wählen Sie **Ansicht hinzufügen**. Wählen Sie dieses Mal die **Blog** Modellschemas, und wählen Sie **Liste** als Gerüst Vorlage.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

In der neuen Sicht Vorlage erhalten wir die Intellisense-Unterstützung.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C#-Projekt kann heruntergeladen werden [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
