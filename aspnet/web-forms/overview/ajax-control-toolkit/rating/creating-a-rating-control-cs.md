---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Erstellen eines Steuerelements Bewertung (c#) | Microsoft Docs
author: wenz
description: Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites. Dies erfordert in der Regel einige Codierungsaufwand, aber wir haben die...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="dd99d-104">Erstellen eines Steuerelements Bewertung (c#)</span><span class="sxs-lookup"><span data-stu-id="dd99d-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="dd99d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dd99d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dd99d-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dd99d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="dd99d-107">Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites.</span><span class="sxs-lookup"><span data-stu-id="dd99d-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="dd99d-108">Dies erfordert in der Regel einige Codierungsaufwand allerdings haben wir die Control-Toolkit zu Verfügung.</span><span class="sxs-lookup"><span data-stu-id="dd99d-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="dd99d-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="dd99d-109">Overview</span></span>

<span data-ttu-id="dd99d-110">Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites.</span><span class="sxs-lookup"><span data-stu-id="dd99d-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="dd99d-111">Dies erfordert in der Regel einige Codierungsaufwand allerdings haben wir die Control-Toolkit zu Verfügung.</span><span class="sxs-lookup"><span data-stu-id="dd99d-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="dd99d-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="dd99d-112">Steps</span></span>

<span data-ttu-id="dd99d-113">Erstens, Sie benötigen (mindestens) zwei Arten von Images: eine für eine ausgefüllte Bewertung Element und eine für ein Element leer Bewertung.</span><span class="sxs-lookup"><span data-stu-id="dd99d-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="dd99d-114">Eine Bewertung-Element ist in der Regel einen Stern oder einem Smiley.</span><span class="sxs-lookup"><span data-stu-id="dd99d-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="dd99d-115">In diesem Szenario finden Sie drei Dateien, smiley.png und empty.png und Smiley done.png im Rahmen des Downloads Source Code für dieses Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="dd99d-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="dd99d-116">Anschließend erstellen Sie eine neue ASP.NET-Datei, und starten Sie mit dem Hinzufügen einer `ScriptManager` Steuerelement darauf:</span><span class="sxs-lookup"><span data-stu-id="dd99d-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="dd99d-117">Fügen Sie dann die `Rating` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit.</span><span class="sxs-lookup"><span data-stu-id="dd99d-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="dd99d-118">Die folgenden Attribute müssen für dieses Beispiel festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="dd99d-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="dd99d-119">`CurrentRating` die erste Bewertung verwendet werden</span><span class="sxs-lookup"><span data-stu-id="dd99d-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="dd99d-120">`MaxRating` die maximale Bewertung für</span><span class="sxs-lookup"><span data-stu-id="dd99d-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="dd99d-121">`EmptyStarCssClass` die CSS-Klasse verwenden, wenn ein Element Bewertung (Stern) leer ist.</span><span class="sxs-lookup"><span data-stu-id="dd99d-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="dd99d-122">`FilledStarCssClass` die CSS-Klasse verwenden, wenn ein Element Bewertung (Stern) ausgefüllt wird</span><span class="sxs-lookup"><span data-stu-id="dd99d-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="dd99d-123">`StarCssClass` die CSS-Klasse, die für einen sichtbaren Stat verwendet</span><span class="sxs-lookup"><span data-stu-id="dd99d-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="dd99d-124">`WaitingStarCssClass` die CSS-Klasse verwenden, während Bewertungssterne zurück an den Server gesendet wird</span><span class="sxs-lookup"><span data-stu-id="dd99d-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="dd99d-125">Und hier ist das Markup erstellt ein Steuerelement für die Bewertung mit fünf Elementen (Smileys), von denen keine anfänglich ausgefüllt wird:</span><span class="sxs-lookup"><span data-stu-id="dd99d-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="dd99d-126">Die drei referenzierten CSS-Klassen müssen nun die entsprechenden Bilddateien anzeigen also erleichtert, Verwendung von CSS:</span><span class="sxs-lookup"><span data-stu-id="dd99d-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="dd99d-127">Stellen Sie sicher, dass Sie die Breite und Höhe der drei Images bereitstellen, andernfalls kann die Anzeige etwas daran aussehen.</span><span class="sxs-lookup"><span data-stu-id="dd99d-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="dd99d-128">Schließlich sollte das Ergebnis der Bewertung werden dem Benutzer angezeigten (oder mindestens in einer Datenbank gespeichert).</span><span class="sxs-lookup"><span data-stu-id="dd99d-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="dd99d-129">So fügen Sie eine Bezeichnung für die Ausgabe von SMS und eine Schaltfläche "Absenden" Zurücksenden der Bewertung Formular an den Server hinzu:</span><span class="sxs-lookup"><span data-stu-id="dd99d-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="dd99d-130">In den serverseitigen Code Zugriff auf das Steuerelement Bewertung über seine `ID` , und klicken Sie dann Zugriff auf seine `CurrentRating` Eigenschaft, die die Anzahl der Elemente ausgewählten Bewertung in unserem Beispiel einen Wert zwischen 0 und 5 ist.</span><span class="sxs-lookup"><span data-stu-id="dd99d-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="dd99d-131">Speichern Sie die Seite, und Laden Sie sie in Ihren Browser.</span><span class="sxs-lookup"><span data-stu-id="dd99d-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="dd99d-132">Wenn Sie auf die Elemente (Anfangs leer) Bewertung zeigen, ein JavaScript-Effekt tritt: die Bewertung ändert.</span><span class="sxs-lookup"><span data-stu-id="dd99d-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="dd99d-133">Wenn Sie für den Satz von Sternen klicken, wird die Bewertung des aktuellen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="dd99d-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="dd99d-134">Bei der Übermittlung des Formulars wird der serverseitige Code schließlich ausgewählte Bewertung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="dd99d-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="dd99d-135">[![Erstellen ein Bewertungssystem mit minimalem Codeeinsatz](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dd99d-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="dd99d-136">Erstellen ein Bewertungssystem mit minimalem Codeeinsatz ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dd99d-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dd99d-137">Nächste</span><span class="sxs-lookup"><span data-stu-id="dd99d-137">Next</span></span>](creating-a-rating-control-vb.md)
