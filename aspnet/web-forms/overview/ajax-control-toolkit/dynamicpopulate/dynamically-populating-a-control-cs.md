---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Dynamisch Auffüllen eines Steuerelements (c#) | Microsoft Docs
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 113b8c043c14e4ebc476b021884dd1430757452a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878606"
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="086c5-103">Dynamisch Auffüllen eines Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="086c5-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="086c5-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="086c5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="086c5-105">[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="086c5-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="086c5-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="086c5-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="086c5-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="086c5-107">Overview</span></span>

<span data-ttu-id="086c5-108">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="086c5-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="086c5-109">Dieses Lernprogramm zeigt, wie dies einrichten.</span><span class="sxs-lookup"><span data-stu-id="086c5-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="086c5-110">Schritte</span><span class="sxs-lookup"><span data-stu-id="086c5-110">Steps</span></span>

<span data-ttu-id="086c5-111">Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="086c5-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="086c5-112">Webdienstklasse erfordert die `ScriptService` innerhalb definierte Attribut `Microsoft.Web.Script.Services`; andernfalls ASP.NET AJAX kann nicht erstellt werden den clientseitigen JavaScript-Proxy für den Webdienst, der wiederum durch erforderlich ist `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="086c5-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="086c5-113">Die Webmethode muss erwarten, dass ein Argument vom Typzeichenfolge sind, aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst.</span><span class="sxs-lookup"><span data-stu-id="086c5-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="086c5-114">Der folgende Webdienst gibt das aktuelle Datum in einem Format dargestellt, die von der `contextKey` Argument:</span><span class="sxs-lookup"><span data-stu-id="086c5-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="086c5-115">Der Webdienst wird dann als gespeichert `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="086c5-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="086c5-116">Alternativ könnten Sie implementieren die `getDate()` Methode als eine Seitenmethode in der tatsächlichen ASP.NET-Seite mit der `DynamicPopulate` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="086c5-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="086c5-117">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Datei.</span><span class="sxs-lookup"><span data-stu-id="086c5-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="086c5-118">Wie immer der erste Schritt besteht, enthalten die `ScriptManager` in der aktuellen Seite die ASP.NET AJAX-Bibliothek geladen und die Arbeit Control Toolkit vornehmen:</span><span class="sxs-lookup"><span data-stu-id="086c5-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="086c5-119">Anschließend fügen Sie ein Bezeichnungsfeld-Steuerelement (z. B. mithilfe des HTML-Steuerelements mit dem gleichen Namen oder die &lt; `asp:Label`  / &gt; Webserver-Steuerelement) wird die höher das Ergebnis den Aufruf des Webdiensts angezeigt.</span><span class="sxs-lookup"><span data-stu-id="086c5-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="086c5-120">Eine HTML-Schaltfläche (als ein HTML-Steuerelement, da es ein Postback an den Server nicht erforderlich ist) wird dann verwendet werden, die dynamische Auffüllung auslösen:</span><span class="sxs-lookup"><span data-stu-id="086c5-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="086c5-121">Schließlich müssen wir die `DynamicPopulateExtender` Kontrolle über das Netzwerk Dinge einrichten.</span><span class="sxs-lookup"><span data-stu-id="086c5-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="086c5-122">Die folgenden Attribute werden festgelegt werden kann (abgesehen von offensichtlichen diejenigen `ID` und `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="086c5-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="086c5-123">`TargetControlID` eines Speicherorts für das Ergebnis aus den Aufruf des Webdiensts</span><span class="sxs-lookup"><span data-stu-id="086c5-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="086c5-124">`ServicePath` Pfad zum Webdienst (auslassen, wenn Sie eine Seitenmethode verwenden möchten)</span><span class="sxs-lookup"><span data-stu-id="086c5-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="086c5-125">`ServiceMethod` Name der Webmethode oder Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="086c5-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="086c5-126">`ContextKey` Informationen zum Sitzungskontext an den Webdienst gesendet werden</span><span class="sxs-lookup"><span data-stu-id="086c5-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="086c5-127">`PopulateTriggerControlID` Element, das den Aufruf des Webdiensts ausgelöst</span><span class="sxs-lookup"><span data-stu-id="086c5-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="086c5-128">`ClearContentsDuringUpdate` angibt, ob das Zielelement während Webdienstaufruf leer</span><span class="sxs-lookup"><span data-stu-id="086c5-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="086c5-129">Wie Sie sehen können, das Steuerelement erfordert einige Informationen-Zustandsmodells alles vor Ort ist jedoch ziemlich selbsterklärend.</span><span class="sxs-lookup"><span data-stu-id="086c5-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="086c5-130">Hier wird das Markup für die `DynamicPopulateExtender` -Steuerelement in das aktuelle Szenario:</span><span class="sxs-lookup"><span data-stu-id="086c5-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="086c5-131">Führen Sie die ASP.NET-Seite in den Browser, und klicken Sie auf die Schaltfläche ""; Sie erhalten das aktuelle Datum im Format Monat-Tag-Jahr.</span><span class="sxs-lookup"><span data-stu-id="086c5-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="086c5-132">[![Mit einem Klick auf die Schaltfläche wird das Datum vom Server abgerufen.](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="086c5-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="086c5-133">Mit einem Klick auf die Schaltfläche mit den vom Server abgerufen, das Datum ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="086c5-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="086c5-134">Nächste</span><span class="sxs-lookup"><span data-stu-id="086c5-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
