---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamisches Auffüllen eines Steuerelements, die mit JavaScript-Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement, auf t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1c3b59896b8c509e9c62738ccd1b37c250a840
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832351"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="a2c83-103">Dynamisches Auffüllen eines Steuerelements, die mit JavaScript-Code (VB)</span><span class="sxs-lookup"><span data-stu-id="a2c83-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="a2c83-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a2c83-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a2c83-105">[Code herunterladen](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a2c83-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="a2c83-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="a2c83-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="a2c83-107">Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.</span><span class="sxs-lookup"><span data-stu-id="a2c83-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a2c83-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a2c83-108">Overview</span></span>

<span data-ttu-id="a2c83-109">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode), und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="a2c83-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="a2c83-110">Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.</span><span class="sxs-lookup"><span data-stu-id="a2c83-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a2c83-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="a2c83-111">Steps</span></span>

<span data-ttu-id="a2c83-112">Zunächst einmal benötigen Sie eines ASP.NET-Webdiensts die implementiert wird, die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="a2c83-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="a2c83-113">Der Webdienst implementiert, die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge, mit dem Namen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Informationen zum Sitzungskontext mit jeder Aufruf des Webdiensts.</span><span class="sxs-lookup"><span data-stu-id="a2c83-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="a2c83-114">Hier ist der Code (Datei `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in der folgenden drei Formaten:</span><span class="sxs-lookup"><span data-stu-id="a2c83-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a2c83-115">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website aus, und beginnen Sie mit ASP.NET AJAX ScriptManager-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="a2c83-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="a2c83-116">Fügen Sie dann ein Label-Steuerelement (z. B. über das HTML-Steuerelement mit dem gleichen Namen, oder die `<asp:Label />` Webserver-Steuerelement) zeigt die das Ergebnis des Webdienstaufrufs später noch mal.</span><span class="sxs-lookup"><span data-stu-id="a2c83-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="a2c83-117">Als Nächstes fügen Sie eine `DynamicPopulateExtender` steuern und Bereitstellen von Informationen zu den Webdiensten, Zielsteuerelement, aber nicht den Namen des Steuerelements die löst die Auffüllung dies später ausgeführt werden wird, die mit benutzerdefinierten JavaScript-Code!</span><span class="sxs-lookup"><span data-stu-id="a2c83-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a2c83-118">Nun, der JavaScript-Teil.</span><span class="sxs-lookup"><span data-stu-id="a2c83-118">Now to the JavaScript part.</span></span> <span data-ttu-id="a2c83-119">Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definierten gibt einen Verweis auf serverseitigen Objekten von der ASP.NET AJAX Control Toolkit, z. B. `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="a2c83-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="a2c83-120">In der aktuellen Datei `$find("dpe")` gibt einen Verweis auf die `DynamicPopulateExtender` Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="a2c83-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="a2c83-121">Es stellt eine Methode namens `populate()` die wird ausgelöst, der dynamische Auffüllung.</span><span class="sxs-lookup"><span data-stu-id="a2c83-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="a2c83-122">Die `populate()` Methode erfordert ein Argument: den Kontextschlüssel das dient als Argument für die `getDate()` Webmethode.</span><span class="sxs-lookup"><span data-stu-id="a2c83-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="a2c83-123">Daher beispielsweise `$find("dpe").populate("format1")` würde die Bezeichnung mit dem aktuellen Datum im Format Monat-Tag-Jahr aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="a2c83-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="a2c83-124">Um das Beispiel etwas flexibler zu machen, kann der Benutzer nun verschiedene Datumsformate auswählen.</span><span class="sxs-lookup"><span data-stu-id="a2c83-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="a2c83-125">Für jeden einzelnen wird ein Optionsfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a2c83-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="a2c83-126">Einmal füllt der Benutzer auf ein Optionsfeld klickt, JavaScript-Code dynamisch auf die Bezeichnung mit dem ausgewählten Datum-Format.</span><span class="sxs-lookup"><span data-stu-id="a2c83-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="a2c83-127">Hier sind die Optionsfelder aus:</span><span class="sxs-lookup"><span data-stu-id="a2c83-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="a2c83-128">Beachten Sie, dass innerhalb des Kontexts eines Optionsfelds, der JavaScript-Ausdruck `this.value` verweist auf den Wert der aktuellen Schaltfläche, die genau die gleichen Informationen werden die `getDate()` Methode mit arbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="a2c83-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="a2c83-129">[![Ein Klick auf die Schaltfläche mit den Ruft das Datum vom Server ab, in dem angegebenen Format ab.](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a2c83-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a2c83-130">Ein Klick auf die Schaltfläche mit den Ruft das Datum ab, von dem Server in das angegebene Format ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a2c83-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2c83-131">[Zurück](dynamically-populating-a-control-vb.md)
> [Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a2c83-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
