---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Füllen eine Liste mit CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870972"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="a9e9e-103">Füllen eine Liste mit CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="a9e9e-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="a9e9e-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a9e9e-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a9e9e-105">[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a9e9e-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="a9e9e-106">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="a9e9e-107">(Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Die erste Abfrage zur Lösung ist, tatsächlich eine Dropdownliste mit diesem Steuerelement geben.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="a9e9e-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a9e9e-108">Overview</span></span>

<span data-ttu-id="a9e9e-109">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="a9e9e-110">(Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Die erste Abfrage zur Lösung ist, tatsächlich eine Dropdownliste mit diesem Steuerelement geben.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="a9e9e-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="a9e9e-111">Steps</span></span>

<span data-ttu-id="a9e9e-112">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="a9e9e-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="a9e9e-113">Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="a9e9e-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="a9e9e-114">Für diese Liste wird ein CascadingDropDown Extender hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="a9e9e-115">Eine asynchrone Anforderung sendet an einen Webdienst dann eine Liste von Einträgen, die in der Liste angezeigt werden zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="a9e9e-116">Damit dies funktioniert müssen die folgenden CascadingDropDown Attribute festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="a9e9e-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="a9e9e-117">`ServicePath`: Die URL eines Webdiensts, die Übermittlung von Einträge in der Liste</span><span class="sxs-lookup"><span data-stu-id="a9e9e-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="a9e9e-118">`ServiceMethod`: Einträge in der Liste zu übermitteln Web-Methode</span><span class="sxs-lookup"><span data-stu-id="a9e9e-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="a9e9e-119">`TargetControlID`: Die ID der Dropdownliste</span><span class="sxs-lookup"><span data-stu-id="a9e9e-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="a9e9e-120">`Category`: Kategorie Informationen, die beim Aufruf der Web-Methode übermittelt wird</span><span class="sxs-lookup"><span data-stu-id="a9e9e-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="a9e9e-121">`PromptText`: Der Text angezeigt, wenn Sie Daten asynchron vom Server zu laden</span><span class="sxs-lookup"><span data-stu-id="a9e9e-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="a9e9e-122">Hier wird das Markup für die `CascadingDropDown` Element.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="a9e9e-123">Der einzige Unterschied zwischen c# und VB ist der Name des Webdiensts verknüpft:</span><span class="sxs-lookup"><span data-stu-id="a9e9e-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="a9e9e-124">Der JavaScript-Code, der von der `CascadingDropDown` Extender Ruft eine Webdienstmethode mit der folgenden Signatur:</span><span class="sxs-lookup"><span data-stu-id="a9e9e-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="a9e9e-125">Damit der wichtigste Aspekt ist, dass die Methode ein Array vom Typ zurückgeben muss `CascadingDropDownNameValue` (definiert durch das ASP.NET AJAX-Steuerelement-Toolkit).</span><span class="sxs-lookup"><span data-stu-id="a9e9e-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="a9e9e-126">In der `CascadingDropDownNameValue` Konstruktor zuerst den Listeneintrag Text, und klicken Sie dann ihren Wert angegeben werden müssen, ebenso wie `<option value="VALUE">NAME</option>` in HTML führen würde.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="a9e9e-127">Hier sind einige Beispieldaten:</span><span class="sxs-lookup"><span data-stu-id="a9e9e-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="a9e9e-128">Beim Laden der Seite im Browser wird die Liste mit den drei Anbietern gefüllt werden ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a9e9e-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="a9e9e-129">[![Die Liste wird automatisch ausgefüllt.](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a9e9e-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="a9e9e-130">Die Liste wird automatisch ausgefüllt, ([klicken Sie hier, um das Bild in voller Größe angezeigt](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a9e9e-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a9e9e-131">Nächste</span><span class="sxs-lookup"><span data-stu-id="a9e9e-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
