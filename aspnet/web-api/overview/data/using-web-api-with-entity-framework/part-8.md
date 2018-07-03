---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Anzeigen von Elementdetails | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 268c44f842cc2beb32a0a3e4c74b83b7ca9fd787
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375178"
---
<a name="display-item-details"></a>Anzeigen von Elementdetails
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen von Details für jedes Buch hinzu. Fügen Sie in "app.js" in den folgenden Code an das Ansichtsmodell:

[!code-javascript[Main](part-8/samples/sample1.js)]

Fügen Sie in Views/Home/Index.cshtml ein Data-Bind-Element, das Details-Link:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Bindet den Klickhandler für die &lt;eine&gt; Element der `getBookDetail` Funktion im Ansichtsmodell.

Ersetzen Sie in der gleichen Datei die folgende Mark-nach-oben ein:

[!code-html[Main](part-8/samples/sample3.html)]

durch diesen:

[!code-html[Main](part-8/samples/sample4.html)]

Dieses Markup erstellt eine Tabelle, die auf die Eigenschaften der Datenbindung die `detail` Observable im Ansichtsmodell.

Die "&lt;! – ko--&gt; &quot; Syntax können Sie eine Bindung Knockout außerhalb eines DOM-Elements enthalten. In diesem Fall die `if` -Bindung bewirkt, dass in diesem Abschnitt des Markups angezeigt werden nur dann, wenn `details` ungleich Null ist.

[!code-html[Main](part-8/samples/sample5.html)]

Jetzt, wenn Sie die app auszuführen, und klicken Sie auf eines der &quot;Detail&quot; Links, die app zeigt die Book-Details.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Zurück](part-7.md)
> [Weiter](part-9.md)
