---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Anzeigen von Elementdetails | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 83f291326307a4964afdd5e8b50f2c375348ed0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823754"
---
<a name="display-item-details"></a><span data-ttu-id="daefe-102">Anzeigen von Elementdetails</span><span class="sxs-lookup"><span data-stu-id="daefe-102">Display Item Details</span></span>
====================
<span data-ttu-id="daefe-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="daefe-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="daefe-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="daefe-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="daefe-105">In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen von Details für jedes Buch hinzu.</span><span class="sxs-lookup"><span data-stu-id="daefe-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="daefe-106">Fügen Sie in "app.js" in den folgenden Code an das Ansichtsmodell:</span><span class="sxs-lookup"><span data-stu-id="daefe-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="daefe-107">Fügen Sie in Views/Home/Index.cshtml ein Data-Bind-Element, das Details-Link:</span><span class="sxs-lookup"><span data-stu-id="daefe-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="daefe-108">Bindet den Klickhandler für die &lt;eine&gt; Element der `getBookDetail` Funktion im Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="daefe-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="daefe-109">Ersetzen Sie in der gleichen Datei die folgende Mark-nach-oben ein:</span><span class="sxs-lookup"><span data-stu-id="daefe-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="daefe-110">durch diesen:</span><span class="sxs-lookup"><span data-stu-id="daefe-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="daefe-111">Dieses Markup erstellt eine Tabelle, die auf die Eigenschaften der Datenbindung die `detail` Observable im Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="daefe-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="daefe-112">Die "&lt;! – ko--&gt; &quot; Syntax können Sie eine Bindung Knockout außerhalb eines DOM-Elements enthalten.</span><span class="sxs-lookup"><span data-stu-id="daefe-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="daefe-113">In diesem Fall die `if` -Bindung bewirkt, dass in diesem Abschnitt des Markups angezeigt werden nur dann, wenn `details` ungleich Null ist.</span><span class="sxs-lookup"><span data-stu-id="daefe-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="daefe-114">Jetzt, wenn Sie die app auszuführen, und klicken Sie auf eines der &quot;Detail&quot; Links, die app zeigt die Book-Details.</span><span class="sxs-lookup"><span data-stu-id="daefe-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="daefe-115">[Zurück](part-7.md)
> [Weiter](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="daefe-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
