---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Elementdetails anzeigen | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868086"
---
<a name="display-item-details"></a><span data-ttu-id="599fa-102">Elementdetails anzeigen</span><span class="sxs-lookup"><span data-stu-id="599fa-102">Display Item Details</span></span>
====================
<span data-ttu-id="599fa-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="599fa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="599fa-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="599fa-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="599fa-105">In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen der Details für jedes Buch.</span><span class="sxs-lookup"><span data-stu-id="599fa-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="599fa-106">Fügen Sie in "App.js" in den folgenden Code für das Modell anzeigen:</span><span class="sxs-lookup"><span data-stu-id="599fa-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="599fa-107">Fügen Sie in Views/Home/Index.cshtml ein Data-Bind-Element auf den Link Details ein:</span><span class="sxs-lookup"><span data-stu-id="599fa-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="599fa-108">Bindet das Click-Ereignishandler für die &lt;eine&gt; Element an der `getBookDetail` Funktion für das Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="599fa-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="599fa-109">Ersetzen Sie die folgenden Markierung-nach-oben, in der gleichen Datei:</span><span class="sxs-lookup"><span data-stu-id="599fa-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="599fa-110">durch diesen:</span><span class="sxs-lookup"><span data-stu-id="599fa-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="599fa-111">Dieses Markup erstellt eine Tabelle, die an den Eigenschaften des datengebundenen ist die `detail` Observable im Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="599fa-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="599fa-112">Die "&lt;!--ko--&gt; &quot; Syntax können Sie eine Bindung Knockout außerhalb ein DOM-Element enthalten.</span><span class="sxs-lookup"><span data-stu-id="599fa-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="599fa-113">In diesem Fall die `if` Bindung führt dazu, dass in diesem Abschnitt des Markups angezeigt werden nur, wenn `details` ungleich Null ist.</span><span class="sxs-lookup"><span data-stu-id="599fa-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="599fa-114">Wenn Sie die app auszuführen, und klicken Sie auf eines der nun die &quot;Detail&quot; Links, die app zeigt die Details für das Offlineadressbuch.</span><span class="sxs-lookup"><span data-stu-id="599fa-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="599fa-115">[Zurück](part-7.md)
> [Weiter](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="599fa-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
