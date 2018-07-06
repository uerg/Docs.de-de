---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen Sie die Sicht (UI) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820985"
---
<a name="create-the-view-ui"></a><span data-ttu-id="e9373-102">Erstellen Sie die Sicht (UI)</span><span class="sxs-lookup"><span data-stu-id="e9373-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="e9373-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9373-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e9373-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="e9373-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e9373-105">In diesem Abschnitt Starten Sie den HTML-Code für die app zu definieren und die Datenbindung zwischen der HTML-Code und das Ansichtsmodell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e9373-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="e9373-106">Öffnen Sie die Datei Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="e9373-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="e9373-107">Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e9373-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="e9373-108">Die meisten der `div` Elemente sind für [Bootstrap](http://getbootstrap.com/) formatieren.</span><span class="sxs-lookup"><span data-stu-id="e9373-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="e9373-109">Die wichtigen Elemente sind mit `data-bind` Attribute.</span><span class="sxs-lookup"><span data-stu-id="e9373-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="e9373-110">Dieses Attribut verknüpft den HTML-Code an das Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="e9373-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="e9373-111">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e9373-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="e9373-112">In diesem Beispiel die &quot; `text` &quot; Bindung bewirkt, dass die `<p>` Element den Wert der anzuzeigenden der `error` Eigenschaft aus dem Anzeigemodell.</span><span class="sxs-lookup"><span data-stu-id="e9373-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="e9373-113">Zur Erinnerung: `error` deklariert wurde, als eine `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="e9373-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="e9373-114">Jedes Mal, wenn ein neuer Wert zugewiesen wird, um `error`, aktualisiert Knockout des Texts in der `<p>` Element.</span><span class="sxs-lookup"><span data-stu-id="e9373-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="e9373-115">Die `foreach` Bindung weist Knockout so durchlaufen Sie den Inhalt der `books` Array.</span><span class="sxs-lookup"><span data-stu-id="e9373-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="e9373-116">Knockout erstellt für jedes Element im Array, ein neues &lt;li&gt; Element.</span><span class="sxs-lookup"><span data-stu-id="e9373-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="e9373-117">Bindungen, die innerhalb des Kontexts der `foreach` verweisen auf Eigenschaften für das Arrayelement.</span><span class="sxs-lookup"><span data-stu-id="e9373-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="e9373-118">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e9373-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="e9373-119">Hier die `text` Bindung liest die Author-Eigenschaft für jedes Buch.</span><span class="sxs-lookup"><span data-stu-id="e9373-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="e9373-120">Wenn Sie die Anwendung jetzt ausführen, sollte es wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="e9373-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="e9373-121">Die Liste der Bücher lädt asynchron, nachdem die Seite geladen werden.</span><span class="sxs-lookup"><span data-stu-id="e9373-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="e9373-122">Momentan die &quot;Details&quot; Links sind nicht funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="e9373-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="e9373-123">Wir fügen diese Funktionalität im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="e9373-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9373-124">[Zurück](part-6.md)
> [Weiter](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="e9373-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
