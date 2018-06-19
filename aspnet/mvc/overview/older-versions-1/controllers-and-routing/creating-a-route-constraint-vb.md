---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Erstellen einer Routeneinschränkung (VB) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871219"
---
<a name="creating-a-route-constraint-vb"></a>Erstellen einer Routeneinschränkung (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen.


Verwenden Sie routeneinschränkungen, um die Browseranforderungen einzuschränken, die mit eine bestimmte Route übereinstimmen. Einen regulären Ausdruck können Sie eine routeneinschränkung angeben.

Angenommen Sie, dass Sie die Route in Codebeispiel 1 in der Datei "Global.asax" definiert haben.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Auflisten von 1 enthält eine Route mit dem Namen Product. Die Produkt-Route können Sie die ProductController in auflisten 2 enthaltene Browseranforderungen zuordnen.

**Auflisten von 2 – Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Beachten Sie, dass die Details()-Aktion, die von der Produkt-Controller verfügbar gemacht werden, einen einzelnen Parameter mit dem Namen "ProductID" akzeptiert. Dieser Parameter ist ein Integer-Parameter.

Im Codebeispiel 1 definierten Route entspricht eine der folgenden URLs:

- /Product/23
- /Product/7

Leider wird die Route auch die folgenden URLs übereinstimmen:

- /Product/blah
- /Product/apple

Da die Details() Aktion einen Ganzzahlparameter erwartet, verursacht der eine Anforderung, die etwas anderes als einen ganzzahligen Wert enthält einen Fehler aus. Beispielsweise bei der Eingabe der URL-/Product/apple in Ihrem Browser erhalten die Seite "Fehler" in Abbildung 1 Sie.


[![Das Dialogfeld "Neues Projekt"](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Abbildung 01**: Anzeigen einer Seite auflösen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-route-constraint-vb/_static/image2.png))


Was Sie tatsächlich möchten ist nur mit URLs übereinstimmen, die eine richtige ganze Zahl "ProductID" enthalten. Eine Einschränkung können beim Definieren einer Route die URLs einzuschränken, die die Route entsprechen. Die geänderte Produkt Route auflisten 3 enthält eine reguläre-Einschränkung, die nur ganze Zahlen übereinstimmt.

**3 – Global.asax.vb auflisten**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

Der reguläre Ausdruck \d+ entspricht mindestens einem ganzen Zahlen. Diese Einschränkung bewirkt, dass die Route Produkt entsprechend den folgenden URLs:

- /Product/3
- /Product/8999

Aber nicht die folgenden URLs:

- /Product/apple
- / Produkt

Diese Browseranforderungen von einer anderen Route verarbeitet werden oder, wenn keine übereinstimmenden Routen vorhanden sind eine *die Ressource konnte nicht gefunden werden* Fehler zurückgegeben.

> [!div class="step-by-step"]
> [Zurück](creating-custom-routes-vb.md)
> [Weiter](creating-a-custom-route-constraint-vb.md)
