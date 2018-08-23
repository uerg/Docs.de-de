---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Dynamisches Auffüllen eines Steuerelements (c#) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement, auf t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f37a3a41c7b83738f97c0daacd781c52b55abc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835816"
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="20303-103">Dynamisches Auffüllen eines Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="20303-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="20303-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="20303-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="20303-105">[Code herunterladen](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="20303-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="20303-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="20303-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="20303-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="20303-107">Overview</span></span>

<span data-ttu-id="20303-108">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode), und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="20303-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="20303-109">In diesem Tutorial veranschaulicht, wie dies eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="20303-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="20303-110">Schritte</span><span class="sxs-lookup"><span data-stu-id="20303-110">Steps</span></span>

<span data-ttu-id="20303-111">Zunächst einmal benötigen Sie eines ASP.NET-Webdiensts die implementiert wird, die Methode aufgerufen werden, indem Sie `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="20303-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="20303-112">Die Webdienstklasse erfordert die `ScriptService` in definierte Attribut `Microsoft.Web.Script.Services`; andernfalls ASP.NET AJAX kann nicht erstellt werden die clientseitige JavaScript-Proxy für den Webdienst, die wiederum erforderlich ist `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="20303-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="20303-113">Die Webmethode muss erwarten, dass ein Argument vom Typzeichenfolge, mit dem Namen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Informationen zum Sitzungskontext mit jeder Aufruf des Webdiensts.</span><span class="sxs-lookup"><span data-stu-id="20303-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="20303-114">Der folgende Webdienst liefert das aktuelle Datum in einem Format dargestellt, durch die `contextKey` Argument:</span><span class="sxs-lookup"><span data-stu-id="20303-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="20303-115">Der Webdienst wird als gespeichert `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="20303-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="20303-116">Alternativ könnten Sie implementieren die `getDate()` Methode als Seitenmethode in der tatsächlichen ASP.NET-Seite mit dem `DynamicPopulate` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="20303-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="20303-117">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Datei.</span><span class="sxs-lookup"><span data-stu-id="20303-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="20303-118">Wie immer ist der erste Schritt zum Einschließen der `ScriptManager` in der aktuellen Seite, um die ASP.NET AJAX-Bibliothek zu laden und die Steuerelement-Toolkit-Arbeit zu machen:</span><span class="sxs-lookup"><span data-stu-id="20303-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="20303-119">Fügen Sie dann ein Label-Steuerelement (z. B. über das HTML-Steuerelement mit dem gleichen Namen, oder die &lt; `asp:Label`  / &gt; Webserver-Steuerelement) zeigt die das Ergebnis des Webdienstaufrufs später noch mal.</span><span class="sxs-lookup"><span data-stu-id="20303-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="20303-120">Eine HTML-Schaltfläche (wie ein HTML-Steuerelement, da es ein Postback an den Server nicht erforderlich ist) wird dann verwendet werden, um die dynamische Auffüllung auszulösen:</span><span class="sxs-lookup"><span data-stu-id="20303-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="20303-121">Schließlich müssen wir die `DynamicPopulateExtender` Steuerelement zum Verbinden aller Elemente.</span><span class="sxs-lookup"><span data-stu-id="20303-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="20303-122">Die folgenden Attribute festgelegt (abgesehen von der offensichtlichen Beziehungen, `ID` und `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="20303-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="20303-123">`TargetControlID` an welcher Stelle das Ergebnis über den Aufruf des Webdiensts</span><span class="sxs-lookup"><span data-stu-id="20303-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="20303-124">`ServicePath` Pfad, an den Webdienst (weglassen, wenn Sie eine Seitenmethode verwenden möchten)</span><span class="sxs-lookup"><span data-stu-id="20303-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="20303-125">`ServiceMethod` Name der Webmethode oder Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="20303-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="20303-126">`ContextKey` Kontextinformationen, die an den Webdienst gesendet werden</span><span class="sxs-lookup"><span data-stu-id="20303-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="20303-127">`PopulateTriggerControlID` Element, das den Aufruf des Webdiensts wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20303-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="20303-128">`ClearContentsDuringUpdate` angibt, ob das Zielelement beim den Aufruf des Webdiensts leere</span><span class="sxs-lookup"><span data-stu-id="20303-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="20303-129">Wie Sie sehen können, wird das Steuerelement erfordert einige Informationen, aber alles, was näher ist relativ einfach.</span><span class="sxs-lookup"><span data-stu-id="20303-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="20303-130">Dies ist das Markup für die `DynamicPopulateExtender` -Steuerelement in das aktuelle Szenario:</span><span class="sxs-lookup"><span data-stu-id="20303-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="20303-131">Führen Sie die ASP.NET-Seite in den Browser, und klicken Sie auf die Schaltfläche; Sie erhalten das aktuelle Datum im Format Monat-Tag-Jahr.</span><span class="sxs-lookup"><span data-stu-id="20303-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="20303-132">[![Ein Klick auf die Schaltfläche mit den Ruft das Datum vom Server ab.](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20303-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="20303-133">Ein Klick auf die Schaltfläche mit den Ruft das Datum ab, auf dem Server ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="20303-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="20303-134">Nächste</span><span class="sxs-lookup"><span data-stu-id="20303-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
