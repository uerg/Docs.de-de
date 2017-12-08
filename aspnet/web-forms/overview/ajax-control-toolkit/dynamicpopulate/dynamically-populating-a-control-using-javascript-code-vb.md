---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: "Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (VB) | Microsoft Docs"
author: wenz
description: "Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b4090b3a785059c8f09de266df79eba0914e9f13
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="80a3b-103">Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (VB)</span><span class="sxs-lookup"><span data-stu-id="80a3b-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="80a3b-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="80a3b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="80a3b-105">[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="80a3b-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="80a3b-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="80a3b-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="80a3b-107">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="80a3b-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="80a3b-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="80a3b-108">Overview</span></span>

<span data-ttu-id="80a3b-109">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="80a3b-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="80a3b-110">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="80a3b-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="80a3b-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="80a3b-111">Steps</span></span>

<span data-ttu-id="80a3b-112">Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="80a3b-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="80a3b-113">Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst.</span><span class="sxs-lookup"><span data-stu-id="80a3b-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="80a3b-114">Hier ist der Code (Datei `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in einem der drei Formate:</span><span class="sxs-lookup"><span data-stu-id="80a3b-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="80a3b-115">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website und beginnen Sie mit ASP.NET AJAX ScriptManager-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="80a3b-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="80a3b-116">Anschließend fügen Sie ein Bezeichnungsfeld-Steuerelement (z. B. mithilfe des HTML-Steuerelements mit dem gleichen Namen oder die `<asp:Label />` Webserver-Steuerelement) wird die höher das Ergebnis den Aufruf des Webdiensts angezeigt.</span><span class="sxs-lookup"><span data-stu-id="80a3b-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="80a3b-117">Als Nächstes enthalten eine `DynamicPopulateExtender` steuern und Bereitstellen von Webdienstinformationen, Zielsteuerelement, aber nicht den Namen des Steuerelements, der die Auffüllung später Dies wird mit benutzerdefinierten JavaScript löst!</span><span class="sxs-lookup"><span data-stu-id="80a3b-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="80a3b-118">Jetzt auf der JavaScript-Teil.</span><span class="sxs-lookup"><span data-stu-id="80a3b-118">Now to the JavaScript part.</span></span> <span data-ttu-id="80a3b-119">Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definiert gibt einen Verweis auf serverseitige Objekte des ASP.NET AJAX-Steuerelement-Toolkits, z. B. `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="80a3b-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="80a3b-120">In der aktuellen Datei `$find("dpe")` gibt einen Verweis auf die `DynamicPopulateExtender` Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="80a3b-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="80a3b-121">Macht eine Methode namens `populate()` die löst der dynamischen Auffüllung.</span><span class="sxs-lookup"><span data-stu-id="80a3b-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="80a3b-122">Die `populate()` Methode erfordert ein Argument: die Kontext-Taste das dient als Argument für die `getDate()` -Webmethode.</span><span class="sxs-lookup"><span data-stu-id="80a3b-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="80a3b-123">Also z. B. `$find("dpe").populate("format1")` füllen die Bezeichnung mit dem aktuellen Datum im Format Monat-Tag-Jahr.</span><span class="sxs-lookup"><span data-stu-id="80a3b-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="80a3b-124">Um das Beispiel etwas flexibler zu machen, kann der Benutzer jetzt zwischen mehreren Datumsformate auswählen.</span><span class="sxs-lookup"><span data-stu-id="80a3b-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="80a3b-125">Für jeden von ihnen wird ein Optionsfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="80a3b-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="80a3b-126">Einmal füllt der Benutzer auf ein Optionsfeld klickt, JavaScript-Code dynamisch die Bezeichnung mit dem ausgewählten Datum-Format.</span><span class="sxs-lookup"><span data-stu-id="80a3b-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="80a3b-127">Hier sind die Optionsfelder aus:</span><span class="sxs-lookup"><span data-stu-id="80a3b-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="80a3b-128">Beachten Sie, dass innerhalb des Kontexts eines Optionsfelds, der JavaScript-Ausdruck `this.value` verweist auf den Wert der aktuellen Schaltfläche, die ausgeführt wird, um genau die gleichen Informationen werden die `getDate()` Methode arbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="80a3b-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="80a3b-129">[![Mit einem Klick auf die Schaltfläche ruft das Datum vom Server in das angegebene Format ab.](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="80a3b-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="80a3b-130">Mit einem Klick auf die Schaltfläche ruft das Datum ab, aus dem Server in das angegebene Format ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="80a3b-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="80a3b-131">[Zurück](dynamically-populating-a-control-vb.md)
[Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="80a3b-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
