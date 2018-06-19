---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen Sie die Sicht (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878801"
---
<a name="create-the-view-ui"></a><span data-ttu-id="0b5ab-102">Erstellen Sie die Sicht (UI)</span><span class="sxs-lookup"><span data-stu-id="0b5ab-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="0b5ab-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0b5ab-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0b5ab-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="0b5ab-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0b5ab-105">In diesem Abschnitt beginnen Sie den HTML-Code für die app definieren und die Datenbindung zwischen den HTML-Code und das Ansichtsmodell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="0b5ab-106">Öffnen Sie die Datei Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="0b5ab-107">Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="0b5ab-108">Die meisten der `div` Elemente müssen für die [Bootstrap](http://getbootstrap.com/) formatieren.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="0b5ab-109">Die wichtigen Elemente sind die Personen mit `data-bind` Attribute.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="0b5ab-110">Dieses Attribut verknüpft den HTML-Code für das Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="0b5ab-111">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0b5ab-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="0b5ab-112">In diesem Beispiel wird die &quot; `text` &quot; binden Ursachen der `<p>` Element den Wert der anzuzeigenden der `error` Eigenschaft aus dem Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="0b5ab-113">Bedenken Sie, dass `error` wurde als deklariert eine `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="0b5ab-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="0b5ab-114">Wenn ein neuer Wert zugewiesen wird, um `error`, Knockout aktualisiert den Text in die `<p>` Element.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="0b5ab-115">Die `foreach` Bindung weist Knockout so durchlaufen Sie den Inhalt der `books` Array.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="0b5ab-116">Für jedes Element im Array Knockout erstellt ein neues &lt;li&gt; Element.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="0b5ab-117">Bindungen, die innerhalb des Kontexts der `foreach` verweisen auf Eigenschaften auf das Arrayelement.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="0b5ab-118">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0b5ab-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="0b5ab-119">Hier die `text` Bindung liest die Author-Eigenschaft, jedes Buch.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="0b5ab-120">Wenn Sie die Anwendung jetzt ausführen, sollten sie wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="0b5ab-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="0b5ab-121">Die Liste der Bücher lädt asynchron ausgeführt wird, nachdem die Seite geladen.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="0b5ab-122">Zurzeit die &quot;Details&quot; Links sind nicht funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="0b5ab-123">Wir werden diese Funktionalität im nächsten Abschnitt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0b5ab-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b5ab-124">[Zurück](part-6.md)
> [Weiter](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="0b5ab-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
