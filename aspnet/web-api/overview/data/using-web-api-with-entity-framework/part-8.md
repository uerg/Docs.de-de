---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Elementdetails anzeigen | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>Elementdetails anzeigen
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen der Details für jedes Buch. Fügen Sie in "App.js" in den folgenden Code für das Modell anzeigen:

[!code-javascript[Main](part-8/samples/sample1.js)]

Fügen Sie in Views/Home/Index.cshtml ein Data-Bind-Element auf den Link Details ein:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Bindet das Click-Ereignishandler für die &lt;eine&gt; Element an der `getBookDetail` Funktion für das Modell anzeigen.

Ersetzen Sie die folgenden Markierung-nach-oben, in der gleichen Datei:

[!code-html[Main](part-8/samples/sample3.html)]

durch diesen:

[!code-html[Main](part-8/samples/sample4.html)]

Dieses Markup erstellt eine Tabelle, die an den Eigenschaften des datengebundenen ist die `detail` Observable im Modell anzeigen.

Die "&lt;!--ko--&gt; &quot; Syntax können Sie eine Bindung Knockout außerhalb ein DOM-Element enthalten. In diesem Fall die `if` Bindung führt dazu, dass in diesem Abschnitt des Markups angezeigt werden nur, wenn `details` ungleich Null ist.

[!code-html[Main](part-8/samples/sample5.html)]

Wenn Sie die app auszuführen, und klicken Sie auf eines der nun die &quot;Detail&quot; Links, die app zeigt die Details für das Offlineadressbuch.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
[Zurück](part-7.md)
[Weiter](part-9.md)
